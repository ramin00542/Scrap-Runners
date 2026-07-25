# FILE: 06_part_06_combat_repair.md

## Purpose
Fix incorrect cross-task reference direction in TASK-06-05's cumulative `_ready()` comment.

## Issues Found

### ISSUE B-1 (Medium / Audit Check B — Dependencies/References)
**Task:** TASK-06-05
**Problem:** The `_ready()` comment says "it must contain EVERY line added by TASK-03-03 **below**" — but TASK-03-03 is an EARLIER task (Part 03, not Part 06). This is a directional typo: it should say "above".
**Fix Required:** Change "below" to "above" in the cumulative `_ready()` comment.

## Required Changes

### TASK-06-05 (`AI_DOCS/PARTS/part_06_combat/task_06_05_ammo_and_reload.md`)

Change line 34 from:
```
    # added by TASK-03-03 below (health.died.connect), plus the new
```
to:
```
    # added by TASK-03-03 above (health.died.connect), plus the new
```

### TASK-08-02 (`AI_DOCS/PARTS/part_08_inventory_ui/task_08_02_toggle_inventory_input.md`)

Change line 19 from:
```
    # added by TASK-03-03 and TASK-06-05 below (health.died.connect,
```
to:
```
    # added by TASK-03-03 and TASK-06-05 above (health.died.connect,
```

## Forbidden Actions
- Do not modify any Game/ files.
- Do not change code logic — only the comment text.

## Verification
After repair, search all task files for "TASK-03-03 below" and "TASK-06-05 below" — zero matches expected.
