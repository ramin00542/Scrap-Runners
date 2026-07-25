# Task 01-03 — Reconcile all Part/Task references across documentation

## ID
TASK-01-03

## Objective
Ensure every AI_DOCS file describes the CURRENT 12-Part structure (ADR-008),
with zero stale references to any earlier numbering scheme.

## Prerequisites
- TASK-01-00, TASK-01-01, TASK-01-02 completed
- ADR-008 exists

## Allowed Files
Documentation-only. First search (read-only) the ENTIRE `AI_DOCS/` tree
including `AI_DOCS/PARTS/**/*.md` for stale Part/Task references, then
list every file needing a change. Known files to check (not exhaustive):
- AI_DOCS/00_INDEX.md
- AI_DOCS/02_GAME_GOAL.md
- AI_DOCS/04_ART_DIRECTION.md
- AI_DOCS/05_TECH_SPEC.md
- AI_DOCS/06_DATA_SCHEMA.md
- AI_DOCS/07_TEST_PLAN.md
- AI_DOCS/08_ASSET_LICENSES.md
- AI_DOCS/09_DECISIONS.md
- AI_DOCS/03_SESSION_BOOTSTRAP.md
- AGENTS.md
- AI_DOCS/CURRENT_TASK.md (only if stale)
- Any file under AI_DOCS/PARTS/**/*.md that contains stale references
  (this task explicitly allows fixing those files as well, to avoid the
  catch-22 where a stale file is found but outside Allowed Files)

## Requirements
1. Search for stale Part number references.
2. Correct each to match the ADR-008 structure.

## Acceptance Criteria
- [ ] Zero remaining stale Part/Task references on a second full-tree
      search after edits.

## Test Procedure
1. Full-text search AI_DOCS/**/*.md for stale numbering.
2. Re-search after edits to confirm zero stale hits.

## Required Report Format
Implementation Mode.
