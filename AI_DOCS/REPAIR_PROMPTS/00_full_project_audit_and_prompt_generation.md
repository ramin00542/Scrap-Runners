--

## 📄 فایل: `00_full_project_audit_and_prompt_generation.md`

```markdown
# Full Scrap Runners Documentation and Task Audit
# Generate One Repair Prompt Per Part — Do Not Apply Fixes Yet

## Purpose
Perform a complete, read-only audit of the Scrap Runners repository.
The project is documentation-first and has NOT yet implemented the actual game.

Your job in this task is NOT to implement gameplay and NOT to fix files now.
Your job is to inspect every planned Part, every Task, every Task Prompt, and
all governing documentation. Then create a separate Markdown repair prompt for
each Part or cross-cutting issue found.

The output must allow the user to later instruct an AI agent:
```text
EXECUTE REPAIR PROMPT: AI_DOCS/REPAIR_PROMPTS/[filename].md
```

Each generated repair prompt must be self-contained, precise, English-only,
and safe to execute one at a time.

---

## Absolute Restrictions

This is an audit and prompt-generation task only.

Do NOT:
- Modify any existing file under `Game/`.
- Create gameplay code.
- Create Godot scenes.
- Create GDScript files.
- Create resources.
- Create autoloads.
- Modify `AI_DOCS/CURRENT_TASK.md`.
- Modify `AI_DOCS/CHANGELOG.md`.
- Modify `AI_DOCS/01_RULES.md`.
- Modify `AI_DOCS/05_TECH_SPEC.md`.
- Modify `AI_DOCS/09_DECISIONS.md`.
- Move, rename, or delete files.
- Change `project.godot`.
- Run Godot.
- Claim runtime or editor verification.
- Apply documentation repairs directly.

The only files you may create are new Markdown prompt files inside:
```text
AI_DOCS/REPAIR_PROMPTS/
```
If this directory does not exist, create it.

---

## Mandatory Reading Order

Read the current version of ALL of the following before writing any repair prompt:

### Repository governance
1. `AGENTS.md`
2. `README.md`
3. `AI_DOCS/00_INDEX.md`
4. `AI_DOCS/01_RULES.md`
5. `AI_DOCS/02_GAME_GOAL.md`
6. `AI_DOCS/03_SESSION_BOOTSTRAP.md`
7. `AI_DOCS/04_ART_DIRECTION.md`
8. `AI_DOCS/05_TECH_SPEC.md`
9. `AI_DOCS/06_DATA_SCHEMA.md`
10. `AI_DOCS/07_TEST_PLAN.md`
11. `AI_DOCS/08_ASSET_LICENSES.md`
12. `AI_DOCS/09_DECISIONS.md`
13. `AI_DOCS/10_POST_MVP_ROADMAP.md`
14. `AI_DOCS/CURRENT_TASK.md`
15. `AI_DOCS/PROJECT_STATUS.md`
16. `AI_DOCS/SESSION_LOG.md`
17. `AI_DOCS/CHANGELOG.md`
18. `AI_DOCS/ASSET_PROMPTS.md`

### All planned implementation tasks
Read EVERY Markdown file under:
```text
AI_DOCS/PARTS/
```
including Parts 01 through 12.

### All one-shot task prompts
Read EVERY Markdown file under:
```text
AI_DOCS/TASK_PROMPTS/
```

### Relevant root/project files, read-only
Read these if they exist:
- `.gitignore`
- `.gitattributes`
- `.editorconfig`
- `project.godot`
- `icon.svg`
- `icon.svg.import`
- `Game/project.godot`
- `Game/.gitignore`

Also inspect the directory tree at repository root, including whether these
directories exist:
- `Game/`
- `.godot/`
- `.freebuff/`
- `AI_DOCS/`

Do not modify any of them.

---

## Known Already-Identified Issues

The audit must CONFIRM or REJECT each of these findings against current files.
Do not assume they are still present without checking.

### Foundation and project-root issues
1. The documentation locks `Game/` as the Godot project root, but the real
   repository may currently be flat with root-level `project.godot`.
2. The project-root decision must not be silently made by an AI.
3. The root `.freebuff/` directory is a local Freebuff AI-agent directory and
   should be ignored by Git when TASK-01-02 is officially performed.
4. `TASK-01-01` may conflict with its one-shot prompt regarding:
   - autoload stubs;
   - whether scenes may be created;
   - `.gitkeep` versus `.gd.keep`;
   - required main scene.
5. `TASK-01-02` may conflict with its prompt regarding:
   - `git init`;
   - first commit;
   - allowed files.
6. `TASK-01-03` may conflict with its prompt because one may describe Part/Task
   numbering migration while the other describes physics layers.
7. `TASK-01-04` prompt may incorrectly forbid `collision_mask = 3` and
   `collision_mask = 5`, even though ADR-014 requires them for standard
   projectile masks.
8. `PROJECT_STATUS.md` or `SESSION_LOG.md` may disagree about whether
   TASK-01-00 was formally verified.
9. Documentation may say there are 19 asset prompts, while the PNG checklist
   in `ASSET_PROMPTS.md` contains 18 PNG assets. The Polygon2D directional
   arrow is not a PNG asset.
10. The existing root `project.godot` may use `stretch/aspect="expand"` while
    the locked technical specification requires `keep`.
11. Existing root `project.godot` may contain unrelated settings such as Jolt
    3D physics or platform-specific rendering-driver settings. These must not
    be blindly adopted as locked Scrap Runners requirements.

### ADR-016 presentation issues
12. ADR-016 requires a slight isometric/depth presentation while remaining
    fully 2D.
13. ADR-016 does not itself specify the exact numeric vertical visual offset.
    An agent must not invent and lock a value without explicit user approval.
14. Player and Enemy placeholder visuals may need a visual-only offset while
    CollisionShape2D stays centered.
15. Player animation must not overwrite a visual offset by animating
    `PlaceholderVisual.position`.
16. Part 09 may need an explicit Y-sort world entity hierarchy.
17. Loot spawn parent may need to be compatible with Y-sorting.
18. Extraction zones, projectiles, and CanvasLayer UI should not accidentally
    inherit standing-entity visual-offset logic.

### Part 02 issues
19. `TASK-02-01` may create `player_controller.gd` without explicitly requiring
    it to be attached to the Player root node.
20. `TASK-02-02` may manually invoke `_physics_process()`, which is an engine
    lifecycle callback and should not be called manually in a runtime test.
21. The movement test may need real physics-frame waits, safe Input cleanup,
    and a position-change assertion.
22. `TASK-02-04` may not define how idle animation starts on first scene load.
23. `TASK-02-03` may need to state that camera limits are deferred until the
    actual Part 09 level exists.

---

## Audit Requirements for Every Part

For every Part from Part 01 through Part 12, inspect every Task file and its
matching Task Prompt file.

Check ALL of the following categories:

### A. Authority and task-scope consistency
- [ ] Does the Task Prompt match the authoritative Task file?
- [ ] Does the Prompt contradict Allowed Files or Forbidden Actions?
- [ ] Does the Task introduce files not listed in Allowed Files?
- [ ] Does the Task require modifying a file that is not allowed?
- [ ] Does the Task tell an agent to create a Git commit, run `git init`, or
  perform other repository actions outside scope?
- [ ] Does the Task require a CURRENT_TASK or CHANGELOG modification without
  satisfying Rules 6.4 and 6.5?

### B. Dependency and execution-order consistency
- [ ] Are prerequisites correct?
- [ ] Does a task depend on a file/class/resource that does not exist yet?
- [ ] Does a task use a class before the class is introduced?
- [ ] Does it assume an autoload before its authorized creation task?
- [ ] Does a task conflict with ADR-008's locked 12-Part order?
- [ ] Does a task incorrectly defer a required test without an explicit Rule 10
  exemption?

### C. Godot and GDScript correctness
Check code examples for likely Godot 4.7/GDScript problems, including:
- [ ] Invalid syntax.
- [ ] Invalid type annotations.
- [ ] Invalid typed arrays/dictionaries.
- [ ] Invalid signal connections.
- [ ] Invalid scene-node paths.
- [ ] Calling engine lifecycle methods manually inappropriately.
- [ ] Child-before-parent `_ready()` ordering mistakes.
- [ ] Unawaited/incorrect `await` usage.
- [ ] Invalid use of `ResourceLoader`, `ResourceSaver`, `FileAccess`, `DirAccess`,
  `PackedScene`, `NodePath`, `Node.get_node_or_null`, or casts.
- [ ] Incorrect access to typed members through generic Node references.
- [ ] Null-safety issues.
- [ ] Incorrect `queue_free()` timing.
- [ ] Signal re-emission or duplicate connection risks.
- [ ] Incorrect use of inherited scenes.
- [ ] Incorrect scene parenting assumptions after scene transitions.

Do not claim a runtime error unless it is provably invalid by static review.
For uncertain engine-version behavior, mark it as "Needs Godot 4.7 verification."

### D. Physics and collision correctness
Check every occurrence of:
- `collision_layer`
- `collision_mask`

against ADR-014.

The locked values are:

| Entity | collision_layer | collision_mask |
|--------|-----------------|----------------|
| World geometry | 1 | 0 |
| Player | 2 | 1 |
| Enemy | 4 | 1 |
| Player projectile | 8 | 5 |
| Enemy projectile | 16 | 3 |
| Pickup | 32 | 2 |
| Extraction zone | 64 | 2 |

Check that:
- [ ] A layer number is not incorrectly used as a bitmask.
- [ ] Combined masks 3 and 5 are accepted where appropriate.
- [ ] Code-set values cite ADR-014.
- [ ] Scene-set values are documented in the Task and verified in its procedure.
- [ ] Collision interactions actually match the intended gameplay behavior.

### E. Data/resource schema consistency
Check against `AI_DOCS/06_DATA_SCHEMA.md`:
- [ ] ItemData field names/order/types.
- [ ] WeaponData inheritance and fields.
- [ ] LootEntry/LootTable fields.
- [ ] EnemyData fields and order.
- [ ] InventorySlot behavior.
- [ ] Inventory behavior.
- [ ] StashData/StashEntry behavior.
- [ ] StringName identity comparison requirements.
- [ ] Resource references and load paths.

### F. Tests and verification quality
For every non-trivial task:
- [ ] Does it have a committed test in the same Task?
- [ ] If not, does it include a valid explicit Rule 10 exemption?
- [ ] Does the test actually validate the claimed behavior?
- [ ] Does it depend on timing, random chance, user input, editor state, or
  scene-order behavior in a flaky way?
- [ ] Does it clean up input state and generated test data?
- [ ] Does it avoid modifying real save files?
- [ ] Does it avoid manual lifecycle callback invocation?
- [ ] Does it distinguish static implementation from actual runtime verification?
- [ ] Does it avoid adding CHANGELOG entries before real verification?

### G. Cross-task composition/regression risks
Check especially:
- [ ] Cumulative `_ready()` requirements.
- [ ] Cumulative `_process()` composition requirements.
- [ ] Cumulative `_physics_process()` composition requirements.
- [ ] Whether a later task accidentally replaces logic added by an earlier task.
- [ ] Whether Player fields used by later tasks have been introduced first.
- [ ] Whether test scenes remain compatible after later scene changes.
- [ ] Whether scene transition logic preserves the intended raid/stash lifecycle.
- [ ] Whether permanent Scrap Pistol behavior remains consistent with ADR-012.

### H. Art, assets, and visual rules
Check against `AI_DOCS/04_ART_DIRECTION.md` and ADR-016:
- [ ] Polygon2D/ColorRect placeholders remain used for MVP.
- [ ] No task incorrectly requires unreliable binary PNG generation.
- [ ] No asset-license entry is required for Polygon2D/ColorRect visuals.
- [ ] Real image assets would require an asset-license entry.
- [ ] ADR-016 stays 2D and visual-only.
- [ ] UI remains CanvasLayer/screen-space.
- [ ] No task accidentally introduces a minimap, melee, spatial inventory,
  loadout selection, or another out-of-scope feature.

---

## Required Output Files

Create the following directory:
```text
AI_DOCS/REPAIR_PROMPTS/
```

Then create these Markdown files:

### Required index
```text
AI_DOCS/REPAIR_PROMPTS/00_REPAIR_PROMPTS_INDEX.md
```
The index must list:
- Every generated prompt file.
- The Part or cross-cutting topic it addresses.
- Whether it is:
  - Audit only;
  - Documentation-only repair;
  - Requires user clarification;
  - Requires a dedicated migration task;
  - Requires actual Godot verification later.
- Dependencies and recommended execution order.

### Required cross-cutting prompts
Create separate files as needed for:
- `01_project_root_decision_and_migration_plan.md`
- `02_foundation_documentation_reconciliation.md`
- `03_adr_016_documentation_reconciliation.md`
- `04_global_task_prompt_consistency_reconciliation.md`
- `05_global_test_plan_and_verification_reconciliation.md`

Only create a cross-cutting prompt when an actual cross-cutting problem exists.

### Required per-Part prompts
Create one file for each Part that contains at least one confirmed issue:
- `part_01_foundation_repair.md`
- `part_02_player_movement_repair.md`
- `part_03_health_damage_repair.md`
- `part_04_core_data_repair.md`
- `part_05_inventory_core_repair.md`
- `part_06_combat_repair.md`
- `part_07_enemy_repair.md`
- `part_08_inventory_ui_repair.md`
- `part_09_test_level_extraction_repair.md`
- `part_10_hub_save_repair.md`
- `part_11_content_ui_repair.md`
- `part_12_qa_export_repair.md`

If a Part has no confirmed issue, still create its file but state:
```text
No confirmed repair is authorized from static review.
This prompt is audit-only and must not modify any file.
```

---

## Format Required for Every Generated Repair Prompt

Every generated Markdown repair prompt must use this exact structure:

```markdown
# Repair Prompt — [Topic or Part]

## Purpose
[Explain exactly what is being repaired.]

## Audit Evidence
[List exact source files and exact contradictions or risks found.
Do not invent issues. Use quotes or concise references.]

## Mandatory Reading
[List every file that must be read before editing.]

## Required User Decision, If Any
[If no user decision is needed, write: None.]

## Allowed Files
[List every file allowed to be modified by this repair prompt.]

## Forbidden Actions
[List what must not be changed.]

## Required Changes
[Numbered, precise changes.]

## Compatibility Requirements
[ADRs, Rules, Data Schema, test requirements, path conventions.]

## Verification Boundaries
[State what can be statically checked and what must later be tested in Godot.]

## Acceptance Criteria
[Checklist.]

## Required Final Report
[Implementation Mode report requirements.]
```

Every generated repair prompt must additionally contain:
- A statement that the authoritative Task file under `AI_DOCS/PARTS/` wins if
  it conflicts with the matching one-shot Task Prompt.
- A statement that existing files must be read before modification.
- A statement that no runtime verification may be claimed without actual
  execution.
- A statement that CURRENT_TASK and CHANGELOG may only be changed according
  to Rules 6.4 and 6.5.
- A restriction preventing unrelated Game implementation.

---

## Project-Root Rule for Generated Prompts

The repository may currently be flat while documentation expects `Game/`.
Generated prompts must never silently select a migration path.

If the user has not explicitly approved either:
- Option A: Make documentation match the flat project root
or:
- Option B: Migrate the physical Godot project into Game/

then the generated Project Root prompt must require Clarification Mode.

Do not state that root `project.godot` is legacy, temporary, or removable
unless the user has explicitly said so.

---

## `.freebuff` Rule

Treat:
```text
.freebuff/
```
as a local Freebuff AI-agent workspace/cache directory.

It is not a Godot source directory and is not part of the game.

When TASK-01-02 or a dedicated repository-hygiene repair task is executed,
the root `.gitignore` should include:
```gitignore
# Freebuff AI agent local workspace/cache
.freebuff/
```

Do not move, delete, inspect private content, or otherwise alter `.freebuff/`.

---

## Final Audit Report

After creating all repair prompts, provide a final report in Review/Planning
Mode containing:

1. Total number of files inspected.
2. Total number of repair prompts generated.
3. A Part-by-Part issue summary table.
4. A severity table:
   - Critical;
   - High;
   - Medium;
   - Low;
   - Needs Godot 4.7 verification.
5. A list of all user decisions required before repair execution.
6. The recommended execution order of generated prompts.
7. Explicit confirmation that:
   - no existing documentation was modified;
   - no Game file was modified;
   - no CURRENT_TASK file was modified;
   - no CHANGELOG file was modified;
   - no runtime verification was claimed.

Do not apply any generated repair prompt in this audit task.

---

## Completion Statement

After completing this audit, end with:
```text
Audit complete. [N] repair prompts generated in AI_DOCS/REPAIR_PROMPTS/.
No existing file was modified. Awaiting user execution commands.
```
```

---

## 🎯 دستور اجرایی برای Agent

این دستور را به Agent بدهید:

```text
EXECUTE REPAIR PROMPT: AI_DOCS/REPAIR_PROMPTS/00_full_project_audit_and_prompt_generation.md
```

---

## ✅ نتیجه مورد انتظار

بعد از اجرا، Agent باید:
1. ✅ تمام فایل‌های `AI_DOCS/` را بخواند
2. ✅ تمام 58 Task و 58 Task Prompt را بررسی کند
3. ✅ 17 فایل repair prompt در `AI_DOCS/REPAIR_PROMPTS/` تولید کند
4. ✅ هیچ فایلی را تغییر ندهد
5. ✅ گزارش نهایی بدهد

سپس شما می‌توانید یکی یکی بگویید:
```text
EXECUTE REPAIR PROMPT: AI_DOCS/REPAIR_PROMPTS/part_01_foundation_repair.md
EXECUTE REPAIR PROMPT: AI_DOCS/REPAIR_PROMPTS/part_02_player_movement_repair.md
...
```

و Agent فقط همان یکی را اجرا می‌کند و متوقف می‌شود.