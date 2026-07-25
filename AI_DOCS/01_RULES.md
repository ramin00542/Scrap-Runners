# Project Rules — Non-Negotiable

This file has the highest priority in the project. If any other
document, prompt, or request contradicts this file, this file wins, and
the AI must point out the conflict instead of proceeding.

## 1. Engine and Language

- Engine version is locked in 05_TECH_SPEC.md and ADR-002. Do not use
  APIs from a newer or older Godot version than the one specified there.
- Language: GDScript only. C# is not used unless a future ADR changes
  this.
- Engine upgrades are only performed as their own dedicated task, after
  a backup/commit exists, and only with explicit approval and a new ADR.

## 2. Scope Control

- Implement only what CURRENT_TASK.md describes.
- Do not implement features from 02_GAME_GOAL.md that are not part of
  the active task, even if they seem like natural next steps.
- New ideas are recorded as a proposal in a Review/Planning Mode
  response, never implemented without explicit approval.

## 3. One Task Per Request

- Each AI response addresses exactly one Task file from AI_DOCS/PARTS/.
- A "Task" is smaller than a "Part." A Part contains multiple Tasks.
- Code belonging to other tasks must not be modified unless that file
  appears in the current task's "Allowed Files" list.

## 4. File and Path Conventions

- Repository root: `GameProject/`
- Godot project root: `GameProject/Game/`
- `res://` inside any Godot file always maps to `GameProject/Game/`
- All in-engine references (scripts, scene references, preload/load
  calls) use `res://...` — never OS-style paths, never `Game/...`.
- In written reports (chat responses, changelog), paths are given
  relative to the repository root, e.g.
  `Game/scripts/entities/player_controller.gd`.
- Never mix the two conventions within the same file or response.

## 5. Naming Conventions

- File names (scripts, scenes, resources): `snake_case`
  e.g. `player_controller.gd`, `main.tscn`, `item_data.gd`
- Node names inside scenes and class names: `PascalCase`
  e.g. the root node of `main.tscn` is named `Main`
- Signal names: `snake_case`, past tense, describing what happened
  e.g. `health_changed`, `item_added`, `extraction_completed`
- Every reusable script must declare `class_name` in `PascalCase`,
  EXCEPT autoload scripts, which must NOT declare `class_name` (to avoid
  a name collision between the class and the global autoload singleton
  of the same name — see ADR-009).
- Item/enemy/weapon identifiers used in data and code are `StringName`,
  written in `snake_case`, e.g. `&"scrap_pistol"`.

## 6. Response Modes

Every AI response must declare which mode it is using, at the very top.

### 6.1 Clarification Mode
Used when the task is ambiguous, contradicts another document, or
requires information not present in any AI_DOCS file. Output only:
1. The contradiction or ambiguity found
2. The specific question(s) that must be answered
3. A suggested resolution, without implementing it

### 6.2 Review/Planning Mode
Used when the user asks for an architectural review, a critique of
existing code/docs, or help designing/splitting a future task — any
request that is NOT "implement CURRENT_TASK.md." Output:
1. Findings / analysis
2. Concrete recommendations
3. If applicable, a proposed new or revised Task file (as text, for the
   user to review), clearly marked as a PROPOSAL — not written to disk
   and not treated as approved until the user confirms it.
No source file under `Game/` is created or modified in this mode.

### 6.3 Implementation Mode
Used only when the task is fully specified. Output must contain, in
order:
1. Summary — one paragraph describing what will be built
2. Files Inspected — existing files read before writing new code
3. Files Created — full path and full content
4. Files Modified — full path and either full content (if new this
   session) or a clearly marked diff of only the changed lines
5. Test Procedure — exact steps to verify in the Godot editor
6. Acceptance Criteria Check — one line per criterion, marked met or not met
7. Known Limitations — anything intentionally left out of scope

### 6.4 Changelog Exception
`AI_DOCS/CHANGELOG.md` is exempt from the "Allowed Files" restriction of
any task. However, an entry may only be appended once Acceptance
Criteria that require editor/runtime execution have been CONFIRMED, not
merely implemented. Confirmation means one of:

(a) The user has run the Test Procedure and reported the result back to
    the AI in the conversation, or
(b) The AI itself has tool access to the Godot editor/CLI and has
    actually executed the Test Procedure, with the output shown in the
    response.

The AI must NOT append a changelog entry — and must NOT claim "zero
runtime errors" or similar — based on static code review alone. If
neither (a) nor (b) has happened, the response ends with Acceptance
Criteria marked "Implemented, not yet verified," and no changelog entry
is written.

### 6.5 CURRENT_TASK Transition Exception
`AI_DOCS/CURRENT_TASK.md` is ALSO exempt from the "Allowed Files"
restriction, but ONLY under these strict conditions, in this exact order:

1. The current task's Acceptance Criteria have been verified per 6.4
   (user-reported or AI tool execution).
2. Its changelog entry has been appended to `AI_DOCS/CHANGELOG.md`.
3. The user explicitly instructs to move to the next task, OR the task
   file itself says "replace this file's content with TASK-XX-YY"
   in its footer comment.

When those conditions are met, the AI MAY (and should) replace the entire
content of `AI_DOCS/CURRENT_TASK.md` with the content of the next task
file from `AI_DOCS/PARTS/...`. This operation must be reported in the
response as "CURRENT_TASK transitioned from TASK-XX to TASK-YY". The AI
must NOT modify CURRENT_TASK.md at any other time, and must NOT skip
tasks.

This exception exists because otherwise no task would ever have permission
to activate the next task, blocking the entire workflow.

## 7. Handling Existing Files

- For a brand-new file: provide full content.
- For an existing file: the AI must have been given its current content
  in the conversation (via paste or file-read tool). If not, the AI must
  ask the user to provide it — never guess or reconstruct it from memory
  of earlier turns.
- Full-file rewrites of existing files are only allowed when the task
  explicitly requires restructuring that file. Otherwise, apply the
  minimal targeted change and show it as a diff.

## 8. Prohibited Actions

- No deleting or silently rewriting code from previous tasks.
- No copyrighted assets or code from other games (including Escape from
  Duckov). Placeholder assets only, using simple geometry or explicitly
  freely-licensed resources, logged in 08_ASSET_LICENSES.md before being
  committed.
- No changes to overall architecture without a new entry in
  09_DECISIONS.md.
- No third-party plugins/addons without explicit approval, recorded as
  an ADR.
- No creation of Autoload singletons "for future use." An autoload is
  only created in the task that gives it its first real responsibility
  AND its first real usage in the same task, wherever feasible.
- No silently overwriting the CONTENT of an existing ADR to record a
  new, unrelated decision. If a locked convention needs to change,
  either amend that ADR's Status to "Superseded by ADR-0XX" and write a
  new ADR, or (if genuinely still the same topic) explicitly revise it
  in place with the change clearly noted — never silently repurpose an
  ADR number for a different topic.

## 9. Code Quality

- Type annotations are mandatory: typed variables, typed function
  parameters, typed return values, typed signal parameters. `Variant` is
  avoided unless there is no alternative.
- Use signals for cross-node communication instead of polling, except
  where polling is clearly justified (e.g. physics checks).
- Separate logic from presentation where reasonable.
- No magic numbers — expose tunable values as `@export var`. This
  applies to collision layer/mask bit values too:
  - If the value is set in GDScript (e.g. `collision_layer = 4`), it must
    be accompanied by a comment citing ADR-014 (ADR-013 superseded) and
    the layer meaning, e.g. `# enemies, per ADR-014`, never a bare
    unexplained integer. Value must be the bitmask (1,2,4,8,16,32,64)
    not the layer number (1..7).
  - If the value is set in a `.tscn` file via Inspector checkboxes, a
    code comment inside `.tscn` is not possible. In that case, the
    requirement is satisfied by the Task file explicitly documenting the
    expected layer/mask table and node names, and by the Test Procedure
    verifying checkboxes match ADR-014. Prefer setting layer/mask in code
    where feasible for auditability, but Inspector setting is allowed when
    documented in the task.
  - Where possible, use named constants or bit-shift expressions for clarity:
    `1 << 2` for layer 3 (enemies = 4), etc., but still cite ADR-014.
- All item, weapon, enemy, and loot definitions are `Resource` subclasses
  (see 06_DATA_SCHEMA.md), never hardcoded dictionaries.
- All identity comparisons (stacking, lookups, removal) between data
  resources compare by their `StringName` id field, never by object
  reference or `==`/`is` identity — see 06_DATA_SCHEMA.md's Identity and
  Equality Convention.
- Comment non-obvious logic. Comments may be in English or Persian;
  identifiers (variables, functions, classes, files) are always English.

## 10. Testing Discipline

- Whenever a task introduces non-trivial logic (anything beyond a
  trivial getter/setter), a committed, repeatable test scene or script
  under `Game/tests/` is created in the SAME task, not deferred to a
  later "smoke test" task, unless the task file explicitly says
  otherwise.
- Temporary, uncommitted debug scripts attached to non-test scenes are
  NOT an acceptable substitute for a committed test, and are not
  permitted as a Test Procedure step even if reverted afterward, because
  doing so requires touching a file outside the task's Allowed Files.
- If a test reveals a bug in code from a PREVIOUS task, the current task
  must switch to Clarification Mode and report the bug — it must not
  silently patch the earlier task's file unless that file is explicitly
  in the current task's Allowed Files.

## 11. Documentation and Cross-Cutting Consistency

Whenever the Part/Task numbering scheme changes, OR a locked
cross-cutting convention that multiple future Parts already depend on
changes (physics layer numbers, input action list, naming schemes,
resource schema field order, etc.), a dedicated documentation-only
consistency-check task must be run — and completed — before any further
feature task proceeds. That task must search the ENTIRE `AI_DOCS/`
tree, including all not-yet-executed task files under `PARTS/`, for
stale references and correct them, then record the change as a new ADR
(never by silently overwriting an existing ADR's topic — see section 8).

## 12. Ambiguity Policy

If a user request conflicts with 01_RULES.md, 02_GAME_GOAL.md, or
09_DECISIONS.md, the AI must use Clarification Mode. It must never
silently choose an interpretation and proceed.
