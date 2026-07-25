# AGENTS.md — Read This First

This file is the entry point for any AI assistant working on this
repository. It applies regardless of which tool is being used (Cursor,
Claude, Copilot, ChatGPT with pasted context, or any agent with file
access).

## Before doing anything

Read, in this exact order:
1. AI_DOCS/01_RULES.md
2. AI_DOCS/02_GAME_GOAL.md
3. AI_DOCS/04_ART_DIRECTION.md
4. AI_DOCS/05_TECH_SPEC.md
5. AI_DOCS/06_DATA_SCHEMA.md
6. AI_DOCS/07_TEST_PLAN.md
7. AI_DOCS/09_DECISIONS.md
8. AI_DOCS/CURRENT_TASK.md

## Hard constraints

- Implement ONLY what is described in AI_DOCS/CURRENT_TASK.md.
- Do not modify any file that is not listed under that task's
  "Allowed Files," EXCEPT AI_DOCS/CHANGELOG.md, which may only be
  appended to after Acceptance Criteria are actually confirmed
  (see 01_RULES.md section 6.4) — never before.
- Do not introduce features, systems, or files outside the current
  task's scope, even if they seem related or obviously useful.
- If the task is ambiguous, contradicts another document, or is missing
  required information, respond in Clarification Mode. Never guess.
- If the request is a review, critique, or planning discussion rather
  than "implement CURRENT_TASK.md," respond in Review/Planning Mode and
  do not write or modify any file.
- If a Part/Task renumbering, or a locked cross-cutting convention
  (e.g. physics layers, naming schemes), has changed and has not yet
  been reconciled across all documentation and pending task files
  (01_RULES.md section 11), stop and request a consistency-check task
  before proceeding with any feature work.

## If no CURRENT_TASK.md is set

Ask the user which task from AI_DOCS/PARTS/ should become active, then
request they populate CURRENT_TASK.md before any code is written.
