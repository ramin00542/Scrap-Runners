# FILE: 04_part_02_player_movement_repair.md

## Purpose
Fix critical design flaws in Part 02 documentation and test design.

## Required Changes

### TASK-02-01
Add explicit requirement: "Attach `player_controller.gd` to the root `Player` CharacterBody2D node." The Task file must make this attachment explicit to avoid ambiguity.

### TASK-02-02
1. Remove `player._physics_process(0.016)` from test design — tests must never manually call `_physics_process()`.
2. Replace with `await get_tree().physics_frame` to let the engine step naturally.
3. Add position-change assertion to verify movement occurred.
4. Ensure Input cleanup (`Input.action_release()`) after test.

### TASK-02-03
Clarify that Camera limits are deferred to Part 09 (test level has actual boundaries). Camera follow works without limits in Part 02.

### TASK-02-04
Define `idle` as the AnimationPlayer autoplay/default animation. The Task file should specify which animation plays on `_ready()`.

## Forbidden Actions
- Do not modify any Game/ files.
- Do not change the authoritative Task files' core logic — only add missing requirements and fix test patterns.

## Verification
After repair:
1. TASK-02-01 explicitly states script attachment to root node.
2. TASK-02-02 test does NOT contain `_physics_process(0.016)`.
3. TASK-02-03 mentions Part 09 for camera limits.
4. TASK-02-04 specifies `idle` as default animation.
