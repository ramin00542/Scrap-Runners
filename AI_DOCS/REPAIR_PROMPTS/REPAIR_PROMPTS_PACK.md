
### ۳. بسته پرامپت‌های اصلاحی (Repair Prompts Pack)
این فایل شامل پرامپت‌های آماده برای رفع مشکلات بحرانی و متوسط شناسایی شده در Part 01، Part 02، ADR-016 و Project Root است. (AI Agent در فاز ۱، بقیه Partها را تولید می‌کند).
**نام فایل:** `REPAIR_PROMPTS_PACK.md`

```markdown
# Scrap Runners — Pre-Computed Repair Prompts Pack
These prompts address the critical and high-priority issues identified in the 60-issue audit. 

---

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

---

# FILE: 02_part_01_foundation_repair.md
## Purpose
Reconcile Part 01 Task Prompts with their authoritative Task files.
## Required Changes
1. **TASK-01-01:** Remove autoload stubs. Require `main.tscn` creation. Change `.gd.keep` to `.gitkeep`.
2. **TASK-01-02:** Remove `git init` and `first commit` instructions. Limit scope to `.gitignore` and `README.md`.
3. **TASK-01-03:** Rewrite prompt to focus on documentation migration (Part/Task numbering), NOT physics layers.
4. **TASK-01-04:** Correct prompt to allow `collision_mask = 3` and `5` for projectiles, while keeping `collision_layer` restricted to single bitmask values.

---

# FILE: 03_adr_016_reconciliation.md
## Purpose
Apply ADR-016 (Slight Isometric Tilt) rules to pending Task documentation.
## Required Changes
1. **TASK-02-01 & 07-02:** Document `PlaceholderVisual.position = Vector2(0, -4)`. Ensure `CollisionShape2D` remains centered.
2. **TASK-02-04:** Explicitly state animation tracks target ONLY `PlaceholderVisual.scale`, NOT `position`.
3. **TASK-09-01:** Document an explicit Y-sort enabled parent node (e.g., `Entities`) for Player, Enemy, and Loot.
4. **Exclusions:** Explicitly state ExtractionPoint, Projectiles, and CanvasLayer UI do NOT use the visual offset.

---

# FILE: 04_part_02_player_movement_repair.md
## Purpose
Fix critical design flaws in Part 02 documentation and test design.
## Required Changes
1. **TASK-02-01:** Add explicit requirement: "Attach `player_controller.gd` to the root `Player` CharacterBody2D node."
2. **TASK-02-02:** Remove `player._physics_process(0.016)` from test design. Replace with `await get_tree().physics_frame`. Add position-change assertion. Ensure Input cleanup.
3. **TASK-02-03:** Clarify that Camera limits are deferred to Part 09.
4. **TASK-02-04:** Define `idle` as the AnimationPlayer autoplay/default animation.

---

# FILE: 00_full_project_audit_and_prompt_generation.md
## Purpose
Generate repair prompts for Parts 03 through 12.
## Instructions
1. Read all Task files and Prompts for Parts 03-12.
2. Check against the 10-point Audit Checklist (Authority, Dependencies, Godot 4.7 correctness, Physics, Data Schema, Tests, Cross-Task, Art, Repo Hygiene, New Features).
3. Generate one `.md` repair prompt per Part in `AI_DOCS/REPAIR_PROMPTS/`.
4. If a Part has no issues, generate an audit-only file stating "No confirmed repair required".
```

---