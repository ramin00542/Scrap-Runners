# Task 07-03 — Patrol state with fixed-position patrol points

## ID
TASK-07-03

## Objective
Implement patrol movement, caching Marker2D global positions ONCE in
`_ready()` to prevent patrol points from moving along with the Enemy
(since Marker2D children of a moving CharacterBody2D move with it).

## Prerequisites
- TASK-07-02 completed

## Allowed Files
- Game/scripts/entities/enemy_controller.gd
- Game/scenes/entities/enemies/enemy_patrol_drone.tscn

## Requirements

**Note on Testing Discipline (01_RULES.md section 10):** Patrol logic is non-trivial but its full verification
(patrol → chase → attack cycle) requires Player reference and chase/attack states that are added in TASK-07-04/07-05.
The committed AI test scene is created in TASK-07-06 (`enemy_ai_test.tscn`) which tests all states together.
Therefore TASK-07-03 is explicitly exempt from same-task committed test per rule 10's "unless explicitly says otherwise".
Manual Test Procedure here confirms fixed patrol points; full automated state log is verified in TASK-07-06.

```gdscript
enum State { PATROL, CHASE, ATTACK }

@export var patrol_point_a: NodePath
@export var patrol_point_b: NodePath

var _state: State = State.PATROL
var _patrol_position_a: Vector2
var _patrol_position_b: Vector2
var _patrol_target: Vector2
var _going_to_b: bool = true

func _ready() -> void:
    if enemy_data != null:
        health.initialize(enemy_data.max_health)
    collision_layer = 4
    collision_mask = 1
    # Cache world positions ONCE — Marker2D children move with this
    # CharacterBody2D otherwise, breaking patrol entirely.
    _patrol_position_a = get_node(patrol_point_a).global_position
    _patrol_position_b = get_node(patrol_point_b).global_position
    _patrol_target = _patrol_position_b

func _physics_process(_delta: float) -> void:
    match _state:
        State.PATROL:
            _process_patrol()
    move_and_slide()

func _process_patrol() -> void:
    var direction: Vector2 = (_patrol_target - global_position)
    if direction.length() < 4.0:
        _going_to_b = not _going_to_b
        _patrol_target = _patrol_position_b if _going_to_b else _patrol_position_a
        direction = _patrol_target - global_position
    # Always set velocity after possibly switching target, avoid leaving old velocity
    if direction.length() > 0.1:
        velocity = direction.normalized() * enemy_data.move_speed
    else:
        velocity = Vector2.ZERO
```
Add two `Marker2D` children (`PatrolPointA`, `PatrolPointB`) to the
scene, wire the exported NodePaths.

## Acceptance Criteria
- [ ] Enemy walks back and forth between FIXED world positions,
      confirmed by observing it actually returns to each marker's
      original position across multiple cycles, not a moving target.

## Test Procedure
1. Run the scene, observe patrol for at least two full cycles.

## Required Report Format
Implementation Mode.
