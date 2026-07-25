# Changelog

Each completed and VERIFIED task gets one entry here, in chronological
order. An entry is only added per 01_RULES.md section 6.4 — never based
on static review alone.

## Format

```
## [TASK-ID] Title — YYYY-MM-DD
Status: Done | Reverted
Verified by: user-reported | AI tool execution
Files Created: ...
Files Modified: ...
Summary: one paragraph
Acceptance Criteria: all met | list exceptions
Notes: anything relevant for future tasks
```

## Log

## [DECISION] ADR-016 Camera Angle Option B — 2026-07-24
Status: Done
Verified by: user-reported (decision accepted in session)
Files Created: none
Files Modified: AI_DOCS/09_DECISIONS.md
Summary: Accepted ADR-016 — slight isometric feel within fully 2D
architecture via Option B (fixed vertical visual offset + Y-sorting;
square floor tiles). Option A (diamond TileMap) rejected for MVP.
ADR-004 resolution/filter/sprite sizes remain locked. No PARTS or
CURRENT_TASK edits; apply when task_02_01 and task_09_01 are reached.
Acceptance Criteria: n/a (decision record, not a PARTS task)
Notes: Visual-inspiration log for rejected 3D pixel-art style lives in
AI_DOCS/VISUAL_INSPIRATION.md (reference only, no implementation).
