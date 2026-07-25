# Task 01-00 — Lock the exact Godot engine version

## ID
TASK-01-00

## Objective
Record the exact installed Godot version. Documentation-only.

## Prerequisites
- Godot Editor installed
- Empty repository initialized with Git

## Allowed Files
- AI_DOCS/05_TECH_SPEC.md
- AI_DOCS/09_DECISIONS.md

## Forbidden in This Task
- No Game/ files
- No code

## Requirements
1. Run `godot --version` or check Help > About Godot; copy the exact
   version string verbatim.
2. Paste it into 05_TECH_SPEC.md's "Engine Version (LOCKED)" section.
3. Paste the same string into ADR-002 in 09_DECISIONS.md; set Status to
   "Accepted".

## Acceptance Criteria
- [ ] Strings in both files are character-for-character identical.
- [ ] String matches the actual installed editor version.
- [ ] ADR-002 Status is "Accepted".
- [ ] No file outside Allowed Files touched.

## Test Procedure
1. Open both files, visually confirm the strings match exactly.

## Required Report Format
Implementation Mode.
