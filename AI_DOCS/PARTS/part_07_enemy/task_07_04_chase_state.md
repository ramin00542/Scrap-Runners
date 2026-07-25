# Task 07-04 — Chase state

## ID
TASK-07-04

## Prerequisites
- TASK-07-03 completed
- Player is in the `"player"` group (task_02_01)

## Allowed Files
- Game/scripts/entities/enemy_controller.gd

## Requirements

**Note on Testing Discipline:** Chase logic depends on Player group and patrol state.
Same-task committed test is deferred to TASK-07-06 integration test which logs states.
Per 01_RULES.md section 10, this task explicitly notes exemption and relies on manual Test Procedure + final AI test.

```gdscript
var _player_ref: Node2D = null

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

func _physics_process(_delta: float) -> void:
    match _state:
        State.PATROL:
            _process_patrol()
            _check_for_chase()
        State.CHASE:
            _process_chase()
    move_and_slide()

func _check_for_chase() -> void:
    if _player_ref == null:
        return
    if global_position.distance_to(_player_ref.global_position) <= enemy_data.detection_radius:
        _state = State.CHASE

func _process_chase() -> void:
    if _player_ref == null:
        _state = State.PATROL
        return
    var distance: float = global_position.distance_to(_player_ref.global_position)
    if distance > enemy_data.detection_radius * 1.5:
        _state = State.PATROL
        return
    velocity = (_player_ref.global_position - global_position).normalized() * enemy_data.move_speed
```

## Acceptance Criteria
- [ ] Switches to chase within detection_radius, returns to patrol at
      1.5x radius (avoiding boundary flickering).

## Test Procedure
1. Run a scene with Player and Enemy, walk in/out of range, observe.

## Required Report Format
Implementation Mode.
