# FILE: 01_project_root_migration.md

## Purpose
Migrate the flat repository root to the canonical nested `Game/` project root.

## Required Changes
1. Create `Game/` directory.
2. Move `project.godot` and `icon.svg` to `Game/`.
3. DO NOT move `.godot/` or `icon.svg.import` (they are generated).
4. Update root `.gitignore` to ignore `Game/.godot/`, `.freebuff/`, and `*.import`.
5. DO NOT alter `project.godot` contents during migration.

## Forbidden Actions
- Do not modify `AI_DOCS/`.
- Do not implement 640x360 settings or Input Map (belongs to TASK-01-01).
- Do not move `.freebuff/`.

## Verification
After migration:
1. `Game/project.godot` exists.
2. Root `project.godot` does NOT exist (only `Game/project.godot`).
3. `.gitignore` contains `Game/.godot/` and `.freebuff/`.
4. `project.godot` contents are byte-identical to pre-migration.
