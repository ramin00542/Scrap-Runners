# FILE: 09_part_09_test_level_repair.md

## Purpose
Add ADR-016 Y-sorting requirement to TASK-09-01 and fix the `_process(0.016)` test pattern in TASK-09-03.

## Issues Found

### ISSUE H-5 (Medium / Audit Check H — Art/ADR-016)
**Task:** TASK-09-01
**Problem:** ADR-016 requires Y-sorting on parent nodes for Player, Enemy, and Loot so overlapping entities paint in depth-correct order. The test level has a `Node2D` named `Enemies` but does not enable Y-sorting, nor does it document the requirement for a Y-sorting parent.
**Fix Required:** Document Y-sorting requirement and specify which parent nodes should have `y_sort_enabled = true`.

### ISSUE F-2 (Medium / Audit Check F — Tests)
**Task:** TASK-09-03
**Problem:** The committed test `loot_pickup_test.gd` calls `pickup._process(0.016)` directly (line 72). This violates the same testing discipline rule as the known issues (#9-11).
**Fix Required:** Remove the direct `_process()` call. The test already verifies the logic via direct `add_item()` calls, so the `_process()` call is redundant and should be removed.

## Required Changes

### TASK-09-01 (`AI_DOCS/PARTS/part_09_test_level_and_extraction/task_09_01_test_level_shell.md`)

Add after the scene description (after line 27, before Acceptance Criteria):

```
   - Per ADR-016, the `TestLevel` root node OR a dedicated `Entities`
     parent node must have `y_sort_enabled = true` so that Player,
     Enemy, and LootPickup nodes draw in depth-correct order (lower
     on screen draws in front of higher). This is required because
     ADR-016 introduces visual offsets that create overlap situations.
     The ExtractionPoint and Projectiles do NOT participate in Y-sorting
     (they are UI/effect elements, not standing entities).
```

### TASK-09-01 Acceptance Criteria

Add a new criterion:
```
- [ ] Y-sorting is enabled on the parent node containing Player, Enemy,
      and LootPickup instances (per ADR-016).
```

### TASK-09-03 (`AI_DOCS/PARTS/part_09_test_level_and_extraction/task_09_03_pickup_interaction.md`)

Remove lines 71-73 (the `_process(0.016)` call and its check):

```gdscript
    # DELETE these lines:
    pickup._process(0.016)
    _check("process without interact press does not consume pickup", pickup.amount == 20)
```

The test already verifies the pickup logic via direct `add_item()` calls on lines 75-81, which is the correct approach. The `_process()` call is redundant and violates testing discipline.

### TASK-09-03 Prompt (`AI_DOCS/TASK_PROMPTS/task_09_03_pickup_interaction.prompt.md`)

Update to remove the _process() reference:
```
Create loot_pickup_test.tscn with Player + LootPickup. Test fills inventory to near capacity (14 dummy items + 55/60 ammo), then verifies partial pickup via direct add_item() calls (NOT by calling _process directly). AC: ALL PASS, leftover=15, pickup persists with reduced amount.
```

## Forbidden Actions
- Do not modify any Game/ files.
- Do not change the test logic — only remove the direct `_process()` call and add Y-sorting documentation.

## Verification
After repair:
1. TASK-09-01 must mention `y_sort_enabled = true` and ADR-016.
2. TASK-09-03 must NOT contain `_process(0.016)` or any direct call to `_process()`.
