# Task 02-04 — Placeholder idle/walk animation (CORRECTED)

## ID
TASK-02-04

## Objective
Give visual feedback for movement state using scale/squash animation,
since no real spritesheet exists yet. Preserve composition pattern for
future tasks (movement logic separated into helper methods).

## Prerequisites
- TASK-02-02 completed

## Allowed Files
- Game/scenes/entities/player/player.tscn
- Game/scripts/entities/player_controller.gd

## Requirements
1. Add `AnimationPlayer` child to `player.tscn`.
2. Set `idle` as the AnimationPlayer's default/autoload animation (autoplay = "idle") so it plays automatically on `_ready()`.
3. Create `idle` (pulses scale 1.0↔1.05 over 1.0s, loops) and `walk`
   (squash/stretch 0.9,1.1 ↔ 1.1,0.9 over 0.3s, loops) animations
   targeting the `PlaceholderVisual` node's `scale` property ONLY.
   Animation tracks must NOT target `position` — the ADR-016 visual
   offset (`Vector2(0, -4)`) must remain constant and must not be
   animated. `PlaceholderVisual` is the `Polygon2D` created in
   TASK-02-01; there is NO `Sprite2D` in this scene, so a track aimed
   at `Sprite2D` would silently fail.
3. In `player_controller.gd` (refactor to helper methods to avoid future overwrite):

```gdscript
@onready var animation_player: AnimationPlayer = $AnimationPlayer

func _physics_process(_delta: float) -> void:
    _process_movement()
    _update_movement_animation()

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

**Note:** `_process_movement()` and `_update_movement_animation()` are now separate,
so future tasks (e.g. death handling) can inject logic without duplicating or losing animation.

## Acceptance Criteria
- [ ] PlaceholderVisual (Polygon2D) pulses gently when idle,
      squashes/stretches faster when moving, switches correctly and
      immediately.
- [ ] Movement logic is in `_process_movement()`, animation logic in `_update_movement_animation()`.

## Test Procedure
1. Run `player.tscn` (F6), observe idle pulse, then move, observe walk
   animation, release, confirm return to idle.

## Required Report Format
Implementation Mode. Note in Known Limitations: placeholder animation,
to be replaced once real sprite frames exist.
