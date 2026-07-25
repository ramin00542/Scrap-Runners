# FILE: 07_part_07_enemy_repair.md

## Purpose
Apply ADR-016 (Visual Offset) to all enemy scene tasks in Part 07.

## Issues Found

### ISSUE H-1 through H-4 (Medium / Audit Check H — Art/ADR-016)
**Tasks:** TASK-07-02, TASK-07-03, TASK-07-04, TASK-07-05
**Problem:** None of these tasks document the ADR-016 visual offset (`PlaceholderVisual.position = Vector2(0, -4)`) or the requirement to keep `CollisionShape2D` centered. ADR-016 applies to ALL standing entities including enemies.
**Fix Required:** Add ADR-016 visual offset requirements to each task.

## Required Changes

### TASK-07-02 (`AI_DOCS/PARTS/part_07_enemy/task_07_02_enemy_scene_and_health.md`)

After the scene description (line 36-39), add:

```
   - Per ADR-016, the `PlaceholderVisual` Polygon2D must have
     `position = Vector2(0, -4)` (slight upward offset for isometric
     feel). The `CollisionShape2D` must remain centered at
     `Vector2(0, 0)` — only the visual moves, not the collision.
```

### TASK-07-03 (`AI_DOCS/PARTS/part_07_enemy/task_07_03_patrol_state.md`)

Add a note after the Requirements section:

```
**ADR-016 Note:** The enemy's `PlaceholderVisual` already has the
visual offset from TASK-07-02. Do NOT modify `PlaceholderVisual.position`
in this task — patrol logic operates on `global_position` of the
CharacterBody2D, which is unaffected by the visual offset.
```

### TASK-07-04 (`AI_DOCS/PARTS/part_07_enemy/task_07_04_chase_state.md`)

Add a note after the Requirements section:

```
**ADR-016 Note:** Chase logic uses `global_position` of the
CharacterBody2D. The `PlaceholderVisual` offset does not affect
chase distance calculations.
```

### TASK-07-05 (`AI_DOCS/PARTS/part_07_enemy/task_07_05_attack_state.md`)

Add a note after the Requirements section:

```
**ADR-016 Note:** Attack range and projectile spawn position use
`global_position` of the CharacterBody2D, not the offset visual.
The enemy projectile fires from `global_position`, which is the
collision center.
```

## Forbidden Actions
- Do not modify any Game/ files.
- Do not change code logic — only add documentation notes.

## Verification
After repair, all four enemy tasks must mention ADR-016 or the visual offset.
