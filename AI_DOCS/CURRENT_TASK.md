# Current Task

## ID
TASK-01-00

## Title
Lock the exact Godot engine version

## Objective
Record the exact installed Godot version so all subsequent tasks target
the same API surface. This is a documentation-only task — no engine or
game files are touched.

## Prerequisites
- Godot Editor installed on the development machine
- Empty repository initialized with Git

## Allowed Files
- AI_DOCS/05_TECH_SPEC.md
- AI_DOCS/09_DECISIONS.md

## Forbidden in This Task
- No Game/ files of any kind
- No code

## Requirements
1. Run `godot --version` in a terminal, or open Help > About Godot in
   the editor, and copy the exact version string verbatim.
2. Paste this EXACT string into `05_TECH_SPEC.md` under
   "Engine Version (LOCKED)", replacing the placeholder.
3. Paste the SAME EXACT string into ADR-002 in `09_DECISIONS.md`, and
   set its Status to "Accepted".

## Acceptance Criteria
- [ ] The version string in 05_TECH_SPEC.md and ADR-002 are character-
      for-character identical.
- [ ] The string matches the actual installed editor's reported version.
- [ ] ADR-002 Status is "Accepted".
- [ ] No file outside Allowed Files was touched.

## Test Procedure
1. Open 05_TECH_SPEC.md and confirm the version line is filled in.
2. Open 09_DECISIONS.md and confirm ADR-002 matches exactly.

## Required Report Format
Implementation Mode. Files Created/Modified section only touches the two
Allowed Files.

---
<!--
When this task is complete and verified, move its final report into
CHANGELOG.md, then replace this file's content with TASK-01-01 from
AI_DOCS/PARTS/part_01_foundation/task_01_01_project_skeleton.md
-->
