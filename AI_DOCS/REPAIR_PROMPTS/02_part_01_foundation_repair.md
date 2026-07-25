# FILE: 02_part_01_foundation_repair.md

## Purpose
Reconcile Part 01 Task Prompts with their authoritative Task files.

## Required Changes

### TASK-01-01 Prompt
1. Remove autoload stubs — the authoritative Task file does not create autoloads in TASK-01-01.
2. Require `main.tscn` creation — the Task file requires it.
3. Change `.gd.keep` to `.gitkeep` — the Task file uses `.gitkeep`.

### TASK-01-02 Prompt
1. Remove `git init` and `first commit` instructions — the Task file limits scope to `.gitignore` and `README.md` only.

### TASK-01-03 Prompt
1. Rewrite prompt to focus on documentation migration (Part/Task numbering consistency), NOT physics layers — the Task file is about documentation migration, not physics.

### TASK-01-04 Prompt
1. Correct prompt to allow `collision_mask = 3` and `5` for projectiles (masks combine multiple layers), while keeping `collision_layer` restricted to single bitmask values — the Task file correctly uses ADR-014 bitmask values.

## Forbidden Actions
- Do not modify any Game/ files.
- Do not change the authoritative Task files — only the Task Prompts.

## Verification
After repair:
1. Each Task Prompt must be a subset of (never a superset of) its authoritative Task file.
2. No Prompt should add requirements not present in the Task file.
