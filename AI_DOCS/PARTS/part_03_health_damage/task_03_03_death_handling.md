# Task 03-03 — Minimal death handling for the Player, with committed test (CORRECTED)

## ID
TASK-03-03

## Objective
React to the Player's `died()` signal by stopping movement input, with a
committed, repeatable test, while PRESERVING animation logic from TASK-02-04.

## Prerequisites
- TASK-03-02 completed

## Allowed Files
- Game/scripts/entities/player_controller.gd
- Game/tests/player_death_test.gd
- Game/tests/player_death_test.tscn

## Requirements

### 1. `player_controller.gd` (diff — add to existing file, preserve animation)

After TASK-02-04, the file has `_process_movement()` and `_update_movement_animation()`
separated. TASK-03-03 must NOT replace `_physics_process` wholesale. Instead:

```gdscript
var _is_dead: bool = false

@onready var health: HealthComponent = $HealthComponent

func _ready() -> void:
    health.died.connect(_on_died)

func _on_died() -> void:
    _is_dead = true

func _physics_process(_delta: float) -> void:
    if _is_dead:
        _process_dead_movement()
        return
    _process_movement()
    _update_movement_animation()

func _process_dead_movement() -> void:
    velocity = Vector2.ZERO
    move_and_slide()
    # Keep idle animation playing after death for visual feedback
    if animation_player.current_animation != "idle":
        animation_player.play("idle")

func _process_movement() -> void:
    var input_vector: Vector2 = Vector2.ZERO
    input_vector.x = Input.get_axis("move_left", "move_right")
    input_vector.y = Input.get_axis("move_up", "move_down")
    input_vector = input_vector.normalized()

    velocity = input_vector * move_speed
    move_and_slide()

func _update_movement_animation() -> void:
    if velocity.length() > 0.1:
        if animation_player.current_animation != "walk":
            animation_player.play("walk")
    else:
        if animation_player.current_animation != "idle":
            animation_player.play("idle")
```

**Important:** This structure preserves movement + animation separation established in TASK-02-04.
Future tasks (combat, extraction indicator) must continue using `_process_xxx()` helper composition pattern, never replacing `_physics_process` wholesale.

### 2. Committed test
`player_death_test.tscn`: root `Node2D` with a `Player` instance, script
`player_death_test.gd`:

```gdscript
extends Node2D

@onready var player: PlayerController = $Player

func _ready() -> void:
    await get_tree().process_frame
    player.health.take_damage(1000.0)
    await get_tree().process_frame

    var pos_before: Vector2 = player.global_position
    Input.action_press("move_right")
    await get_tree().physics_frame
    await get_tree().physics_frame
    var pos_after: Vector2 = player.global_position
    var moved: bool = pos_before.distance_to(pos_after) > 0.1

    print("PASS: player does not move after death" if not moved else "FAIL: player moved after death")
    Input.action_release("move_right")
```

## Acceptance Criteria
- [ ] Running `player_death_test.tscn` prints
      "PASS: player does not move after death".
- [ ] Animation logic from TASK-02-04 still works before death.
- [ ] After death, velocity is zero and idle animation plays.

## Test Procedure
1. Run `player_death_test.tscn` (F6), confirm PASS.
2. Run `player.tscn` manually, move, then kill via health damage in Remote, confirm stops and idle animation.

## Required Report Format
Implementation Mode.
