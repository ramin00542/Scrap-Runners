# FILE: 05_part_03_health_damage_repair.md

## Purpose
Fix the test design in TASK-03-03 that calls `_physics_process()` directly.

## Issues Found

### ISSUE M-1 (Medium / Audit Check F — Tests)
**Task:** TASK-03-03
**Problem:** The committed test `player_death_test.gd` calls `player._physics_process(0.016)` directly (line 86 of the task file). This violates 01_RULES.md section 10's testing discipline and the known issues list (#9-11). Tests should never manually invoke `_physics_process()`.
**Fix Required:** Replace the direct `_physics_process(0.016)` call with `await get_tree().physics_frame` to let the engine step naturally. Add a position-change assertion to verify the player did NOT move after death.

## Required Changes

### TASK-03-03 (`AI_DOCS/PARTS/part_03_health_damage/task_03_03_death_handling.md`)

Replace the committed test section (lines 72-91) with:

```gdscript
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

Key changes:
1. Remove `player._physics_process(0.016)` — use `await get_tree().physics_frame` instead.
2. Add position-before/position-after comparison for a proper assertion.
3. Wait TWO physics frames to ensure the engine has stepped.

### TASK-03-03 Prompt (`AI_DOCS/TASK_PROMPTS/task_03_03_death_handling.prompt.md`)

Update to:
```
Add death handling to player_controller: bool _is_dead, connect health.died to _on_died which sets _is_dead=true. In _physics_process: if dead call _process_dead_movement() (velocity=0, play idle anim) then return. Otherwise call _process_movement() + _update_movement_animation(). Create player_death_test.tscn using await get_tree().physics_frame (NOT direct _physics_process call). IMPORTANT: preserve TASK-02-04's helper method composition. AC: Test prints PASS.
```

## Forbidden Actions
- Do not modify any Game/ files.
- Do not change the player_controller.gd logic (only the test).

## Verification
After repair, the task file's test code must NOT contain `_physics_process(0.016)` or any direct call to `_physics_process`.
