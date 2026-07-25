# Task 02-03 — Add a following camera

## ID
TASK-02-03

## Objective
Add a Camera2D that follows the player smoothly, verified visually using
the existing test scene's grid markers.

## Prerequisites
- TASK-02-02 completed

## Allowed Files
- Game/scenes/entities/player/player.tscn
- Game/tests/player_movement_test.tscn

## Requirements
1. Add a child `Camera2D` to Player, named `Camera2D`.
2. `enabled = true`, `zoom = Vector2(1, 1)` per ADR-004.
3. `position_smoothing_enabled = true`, `position_smoothing_speed = 5.0`.
4. Camera limits (`limit_left`, `limit_top`, `limit_right`, `limit_bottom`) are deferred to Part 09 (TASK-09-01) where the test level has actual world boundaries. Camera follow works without limits in Part 02.

## Acceptance Criteria
- [ ] Running `player_movement_test.tscn` and moving past the grid
      markers shows the camera trailing with visible smoothing lag, not
      snapping instantly.
- [ ] Zoom matches 640x360 base resolution 1:1.

## Test Procedure
1. Run `player_movement_test.tscn` (F6), move past markers, confirm
   smoothing behavior.

## Required Report Format
Implementation Mode.
