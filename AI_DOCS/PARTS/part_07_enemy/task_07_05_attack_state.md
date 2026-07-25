# Task 07-05 — Attack state, with enemy projectile per ADR-014

## ID
TASK-07-05

## Prerequisites
- TASK-07-04 completed
- part_06_combat completed

## Allowed Files
- Game/scripts/entities/enemy_controller.gd
- Game/scenes/items/enemy_projectile.tscn
- Game/scenes/entities/enemies/enemy_patrol_drone.tscn

## Requirements

**Note on Testing Discipline:** Attack state + enemy_died signal verification is fully exercised in TASK-07-06 committed test `enemy_ai_test.tscn` which demonstrates patrol→chase→attack→death cycle.
Per 01_RULES.md section 10, this task's same-task committed test is that integration scene created next; manual procedure here is sufficient.

1. Create `enemy_projectile.tscn` (inherits `projectile_base.tscn`):
```gdscript
# collision_layer = 16  (enemy_projectiles, per ADR-014)
# collision_mask = 1 | 2  (world=1, player=2, per ADR-014)
```
2. Add to `enemy_controller.gd`:
```gdscript
@export var attack_range: float = 150.0
@export var projectile_scene: PackedScene

var _attack_cooldown: float = 0.0

signal enemy_died(enemy_id: StringName)

func _ready() -> void:
    if enemy_data != null:
        health.initialize(enemy_data.max_health)
    collision_layer = 4
    collision_mask = 1
    _patrol_position_a = get_node(patrol_point_a).global_position
    _patrol_position_b = get_node(patrol_point_b).global_position
    _patrol_target = _patrol_position_b
    var players: Array[Node] = get_tree().get_nodes_in_group("player")
    if players.size() > 0:
        _player_ref = players[0]
    health.died.connect(_on_died)

func _physics_process(delta: float) -> void:
    if _attack_cooldown > 0.0:
        _attack_cooldown -= delta
    match _state:
        State.PATROL:
            _process_patrol()
            _check_for_chase()
        State.CHASE:
            _process_chase()
            _check_for_attack()
        State.ATTACK:
            _process_attack()
    move_and_slide()

func _check_for_attack() -> void:
    if _player_ref == null:
        return
    if global_position.distance_to(_player_ref.global_position) <= attack_range:
        _state = State.ATTACK

func _process_attack() -> void:
    velocity = Vector2.ZERO
    if _player_ref == null:
        _state = State.PATROL
        return
    var distance: float = global_position.distance_to(_player_ref.global_position)
    if distance > attack_range * 1.3:
        _state = State.CHASE
        return
    if _attack_cooldown <= 0.0 and projectile_scene != null:
        _fire_at_player()
        _attack_cooldown = enemy_data.attack_cooldown

func _fire_at_player() -> void:
    var projectile: Projectile = projectile_scene.instantiate()
    get_tree().current_scene.add_child(projectile)
    projectile.global_position = global_position
    projectile.damage = enemy_data.attack_damage
    projectile.direction = (_player_ref.global_position - global_position).normalized()

func _on_died() -> void:
    enemy_died.emit(enemy_data.enemy_id)
    queue_free()
```
3. Assign `projectile_scene = enemy_projectile.tscn` in the scene's
   Inspector.

**ADR-016 Note:** Attack range and projectile spawn position use
`global_position` of the CharacterBody2D, not the offset visual.
The enemy projectile fires from `global_position`, which is the
collision center.

## Acceptance Criteria
- [ ] Fires at player within attack_range, respects attack_cooldown.
- [ ] Transitions back to chase if player retreats.
- [ ] `enemy_died` fires exactly once with correct `enemy_id` before
      the node is freed.
- [ ] `enemy_projectile.tscn` layer/mask exactly match ADR-014's
      "Enemy projectile" row.

## Test Procedure
1. Run a scene with Player and Enemy, approach until attack starts,
   confirm firing rate.
2. Kill the enemy, confirm `enemy_died` signal via task_07_06's test scene.

## Required Report Format
Implementation Mode.
