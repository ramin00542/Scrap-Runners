# Task 02-02 — Implement 8-directional movement, with committed test

## ID
TASK-02-02

## Objective
Add movement logic to the Player and verify it with a committed,
numeric test scene (not a visual-only check).

## Prerequisites
- TASK-02-01 completed

## Allowed Files
- Game/scripts/entities/player_controller.gd
- Game/tests/player_movement_test.gd
- Game/tests/player_movement_test.tscn

## Requirements

### 1. `player_controller.gd`
```gdscript
class_name PlayerController
extends CharacterBody2D

@export var move_speed: float = 120.0

func _physics_process(_delta: float) -> void:
    var input_vector: Vector2 = Vector2.ZERO
    input_vector.x = Input.get_axis("move_left", "move_right")
    input_vector.y = Input.get_axis("move_up", "move_down")
    input_vector = input_vector.normalized()

    velocity = input_vector * move_speed
    move_and_slide()
```

### 2. Committed numeric test
`player_movement_test.tscn`: root `Node2D` with several `Marker2D`
children every 100px (visual reference for camera work in task_02_03), a
`Player` instance, a `Label` for output, script
`player_movement_test.gd`:

```gdscript
extends Node2D

@onready var player: PlayerController = $Player
@onready var output_label: Label = $Label

var _test_count: int = 0
var _fail_count: int = 0
var _tests_run: bool = false

func _check(label: String, condition: bool) -> void:
    _test_count += 1
    if condition:
        print("PASS: %s" % label)
    else:
        _fail_count += 1
        print("FAIL: %s" % label)

func _physics_process(_delta: float) -> void:
    if _tests_run:
        return
    _tests_run = true

    Input.action_press("move_right")
    player._physics_process(0.016)
    _check(
        "horizontal speed equals move_speed",
        is_equal_approx(player.velocity.length(), player.move_speed)
    )
    Input.action_release("move_right")

    Input.action_press("move_right")
    Input.action_press("move_up")
    player._physics_process(0.016)
    _check(
        "diagonal speed does not exceed move_speed",
        player.velocity.length() <= player.move_speed + 0.01
    )
    Input.action_release("move_right")
    Input.action_release("move_up")

    player._physics_process(0.016)
    _check("zero input yields zero velocity", player.velocity == Vector2.ZERO)

    output_label.text = "%d/%d PASS" % [_test_count - _fail_count, _test_count]
    print("---")
    print(output_label.text)
```

## Acceptance Criteria
- [ ] `player_movement_test.tscn` prints numeric PASS for all 3 checks.
- [ ] Diagonal speed never exceeds axis-aligned speed.

## Test Procedure
1. Run `player_movement_test.tscn` (F6), confirm all PASS in Output.

## Required Report Format
Implementation Mode.
