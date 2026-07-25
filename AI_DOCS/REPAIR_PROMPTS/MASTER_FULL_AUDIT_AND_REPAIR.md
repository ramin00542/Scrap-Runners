
### ۱. فایل مادر و کنترل‌کننده اصلی (Master Controller)
این فایل مغز متفکر پروژه است. تمام ۶۰ مشکل، قوانین مطلق، و دستورالعمل‌های اجرایی در آن یکپارچه شده است.
**نام فایل:** `MASTER_FULL_AUDIT_AND_REPAIR.md`

```markdown
# Scrap Runners — Master Audit & Repair Controller
**Version:** 60-Issue Consolidated  
**Target Path:** `AI_DOCS/REPAIR_PROMPTS/MASTER_FULL_AUDIT_AND_REPAIR.md`

---

## 📊 Project Status & Locked Decisions
The project is a 2D Godot 4.7.1 top-down extraction shooter. The game has NOT been built yet. Only documentation exists.

### Locked Architectural Decisions
1. **Project Root:** Canonical Godot project root is `Game/`. Repository root is `Scrap Runners 2D_V2/`.
2. **ADR-016 (Visual Offset):** Approved fixed upward visual offset for standing entities is `Vector2(0, -4)`. Collision remains centered.
3. **Asset Count:** Exactly 18 PNG assets. The directional arrow is a `Polygon2D` fallback, NOT a PNG.
4. **`.freebuff/`:** Local AI-agent workspace/cache. Must be ignored by Git. Never move, delete, or inspect it.
5. **New Features:** The `scrap_runners_new_features_prompts.md` is a PROPOSAL. Prompt 05 (Hideout) is **BLOCKED** (contradicts ADR-012).

---

## 🤖 Master Prompt (For AI Agent)

### Purpose
This is the single authoritative master prompt for documentation reconciliation. It operates in THREE phases.

### Absolute Restrictions (All Phases)
- Do NOT create, modify, move, or delete any file under `Game/` unless explicitly authorized.
- Do NOT create gameplay code, scenes, GDScript, resources, autoloads, or assets.
- Do NOT modify `CURRENT_TASK.md` or `CHANGELOG.md` unless Rules 6.4 and 6.5 are fully satisfied.
- Do NOT run Godot or claim runtime/editor verification unless actually performed.
- Do NOT invent numeric values (offsets, intervals) without explicit user approval.
- The authoritative Task file under `AI_DOCS/PARTS/` ALWAYS wins over its matching one-shot Prompt under `AI_DOCS/TASK_PROMPTS/`.

---

### PHASE 1 — Full Audit and Repair Prompt Generation
**Trigger:** `EXECUTE: MASTER AUDIT — Generate all repair prompts`

**Actions:**
1. Read all governance docs, all 58 Task files, all 58 Task Prompts, and the New Features pack.
2. Run the Audit Checklist (A-J) for every Part.
3. Generate repair prompts in `AI_DOCS/REPAIR_PROMPTS/` using the `00_full_project_audit_and_prompt_generation.md` template.
4. STOP and wait for user commands.

### PHASE 2 — Controlled Execution
**Trigger:** `EXECUTE REPAIR PROMPT: AI_DOCS/REPAIR_PROMPTS/[filename].md`

**Rules:**
- Execute EXACTLY ONE named repair prompt per user command.
- NEVER auto-advance. NEVER execute multiple prompts in one response.
- If the prompt requires an unresolved user decision → **Clarification Mode**.
- After completion, STOP and wait for the next user command.

### PHASE 3 — Final Verification
**Trigger:** `EXECUTE: FINAL VERIFICATION — Confirm all repairs applied`

**Actions:**
- Re-read every modified file. Re-run the full audit checklist.
- Produce a Final Compliance Matrix.
- Confirm if the project is ready for `TASK-01-01` implementation.

---

## 📋 Known Issues Reference (Pre-Loaded — 60 Items)
*The Phase 1 audit must CONFIRM or REJECT each against current file contents.*

### 🔴 Critical (3)
1. **Godot Project Root:** Docs say `Game/`, reality was flat root. (Resolved via migration prompt).
2. **Root `project.godot`:** Had wrong stretch aspect (`expand`), missing resolution/filter, contained unrelated Jolt 3D/d3d12 settings.
3. **TASK-02-01:** `player_controller.gd` created but never explicitly attached to Player root node.

### 🟠 High (15)
4-8. TASK-01-01 to 01-04 Prompts: Contradictions with authoritative Task files (autoloads, scene creation, physics masks).
9-11. Tests: Manually call `_physics_process(0.016)` (Tasks 02-02, 03-03).
12-15. New Features 01, 02, 05: False "BUG FIX" claims, ADR-005 misreference, direct contradiction with ADR-012 (Hideout blocked).

### 🟡 Medium (23)
16-25. Documentation inconsistencies: `.gitkeep` vs `.gd.keep`, `git init` out of scope, `.gitignore` missing `Game/.godot/` and `.freebuff/`.
26-35. New Features: Magic numbers (0.2s, 3.0s), assumes `Game/` exists, LightOccluder2D compatibility, State enum changes affecting tests.

### 🟢 Low (14)
36-45. Minor undocumented behaviors, performance issues, private access, asset count mismatch (18 vs 19).

### 🔵 Needs Godot 4.7 Verification (5)
46-50. Typed dict stability, `ResourceLoader` cache modes, `extraction_progressed` performance, `Input` in `_process`, Polygon2D rotation in CanvasLayer.

---

## 🎯 Execution Commands

### Phase 1
```text
EXECUTE: MASTER AUDIT — Generate all repair prompts
```

### Phase 2 (Execute ONE by ONE)
```text
EXECUTE REPAIR PROMPT: AI_DOCS/REPAIR_PROMPTS/01_project_root_migration.md
EXECUTE REPAIR PROMPT: AI_DOCS/REPAIR_PROMPTS/02_part_01_foundation_repair.md
EXECUTE REPAIR PROMPT: AI_DOCS/REPAIR_PROMPTS/03_adr_016_reconciliation.md
EXECUTE REPAIR PROMPT: AI_DOCS/REPAIR_PROMPTS/04_part_02_player_movement_repair.md
EXECUTE REPAIR PROMPT: AI_DOCS/REPAIR_PROMPTS/05_part_03_health_damage_repair.md
EXECUTE REPAIR PROMPT: AI_DOCS/REPAIR_PROMPTS/06_part_06_combat_repair.md
EXECUTE REPAIR PROMPT: AI_DOCS/REPAIR_PROMPTS/07_part_07_enemy_repair.md
EXECUTE REPAIR PROMPT: AI_DOCS/REPAIR_PROMPTS/08_part_08_inventory_ui_repair.md
EXECUTE REPAIR PROMPT: AI_DOCS/REPAIR_PROMPTS/09_part_09_test_level_repair.md
EXECUTE REPAIR PROMPT: AI_DOCS/REPAIR_PROMPTS/10_part_10_hub_save_audit.md
EXECUTE REPAIR PROMPT: AI_DOCS/REPAIR_PROMPTS/11_part_11_content_ui_audit.md
EXECUTE REPAIR PROMPT: AI_DOCS/REPAIR_PROMPTS/12_part_12_qa_export_audit.md
```

### Phase 3
```text
EXECUTE: FINAL VERIFICATION — Confirm all repairs applied
```
```

---

### ۲. فایل پرامپت‌های ویژگی‌های جدید (New Features)
این فایل کاملاً تمیز شده و ساختار ۰۰ تا ۰۵ در آن به صورت استاندارد جدا شده است.
**نام فایل:** `scrap_runners_new_features_prompts.md`

```markdown
# Scrap Runners — New Feature Reconciliation Prompt Pack
**Status:** PROPOSAL ONLY. Do NOT execute without explicit user approval and dedicated task creation.
**Note:** Prompt 05 (Hideout) is **BLOCKED** due to direct conflict with ADR-012 and `02_GAME_GOAL.md`.

---

## 00 — Authorization Preflight
**File:** `00_new_features_authorization.prompt.md`
**Purpose:** Verify the 5 proposed additions are formally authorized. Planning-only.
**Action:** Output Clarification Mode report. State if any item conflicts with locked docs. Request user approval for dedicated tasks.

---

## 01 — NavigationAgent2D Pathfinding
**File:** `01_navigation_agent_pathfinding.prompt.md`
**Purpose:** Replace direct chase vector with `NavigationAgent2D`. 
**Correction:** This is a **Feature Addition**, NOT a bug fix. Requires a new ADR.
**Key Rules:** 
- Repathing interval (e.g., 0.2s) requires explicit user approval (no magic numbers).
- Requires `TileMapLayer` or equivalent navigation mesh (conflicts with `ColorRect` walls in TASK-09-01, must be resolved).

---

## 02 — Fog of War / Line of Sight
**File:** `02_fog_of_war_line_of_sight.prompt.md`
**Purpose:** Add visibility-limiting system using Light2D masking.
**Status:** NEW SCOPE. Not in MVP. Requires new ADR.
**Key Rules:** 
- Clarification Mode required.
- `LightOccluder2D` compatibility with current wall geometry must be verified.
- ADR-005 is about Minimap, NOT Fog of War. Do not conflate them.

---

## 03 — Alert/Searching AI State
**File:** `03_alert_searching_state.prompt.md`
**Purpose:** Add third enemy state (ALERT) between PATROL and CHASE.
**Status:** NEW SCOPE. Requires new ADR.
**Key Rules:** 
- Wait duration (e.g., 3.0s) requires explicit user approval.
- Changing `State` enum affects `TASK-07-06` tests. Test must be updated.
- Line-of-sight dependency (Prompt 02 vs standalone RayCast2D) must be clarified.

---

## 04 — Hitscan Weapon Option
**File:** `04_hitscan_weapon_option.prompt.md`
**Purpose:** Add `RayCast2D` hitscan firing mode alongside Projectile.
**Status:** NEW SCOPE. MVP is locked to 1 weapon (Projectile).
**Key Rules:** 
- Requires structural change to `WeaponData` schema (`fire_mode` enum).
- Requires new ADR.
- Choice between `RayCast2D` and `PhysicsDirectSpaceState2D.intersect_ray()` must be explicitly decided.

---

## 05 — Hideout Loadout/Upgrades
**File:** `05_hideout_loadout.prompt.md`
**Status:** 🚫 **BLOCKED**
**Reason:** Directly contradicts `02_GAME_GOAL.md` ("no loadout picker · no gear upgrades between raids") and ADR-012.
**Action:** Do NOT send to agent. Requires formal ADR reversal by the user before any implementation.
```

---
