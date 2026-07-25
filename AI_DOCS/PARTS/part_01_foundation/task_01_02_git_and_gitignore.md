# Task 01-02 — Repository hygiene

## ID
TASK-01-02

## Objective
Ensure the repository ignores Godot-generated and OS/editor-generated
files at the repo root level.

## Prerequisites
- TASK-01-01 completed

## Allowed Files
- .gitignore (repository root)
- README.md (repository root, create if missing)

## Forbidden in This Task
- No changes to Game/.gitignore
- No engine logic

## Requirements
1. Create/confirm root `.gitignore` ignores OS files, editor folders, and
   does NOT ignore anything under `AI_DOCS/`.
2. Create/confirm root `README.md` points to `AI_DOCS/00_INDEX.md` and
   `AGENTS.md`.

## Acceptance Criteria
- [ ] `git status` after a clean clone shows no unwanted OS/editor noise.
- [ ] README.md exists and links correctly.

## Test Procedure
1. Run `git status` in a clean checkout.
2. Open README.md, confirm links.

## Required Report Format
Implementation Mode.
