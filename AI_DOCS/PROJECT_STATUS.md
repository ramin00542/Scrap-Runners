# Project Status

Single source of truth for where the project stands right now. Updated
at the end of every session.

---

## Current Part
**Part 01 — Foundation** (part_01_foundation)

## Current Task
**TASK-01-00** — Lock the exact Godot engine version

Status: Documentation complete, awaiting user confirmation to mark done.

## Completed Tasks
_(none yet — first task pending confirmation)_

## Blocked Tasks
_(none — all prerequisites are met for Part 01)_

## Next Task (after current)
**TASK-01-01** — Create Project Skeleton (`project.godot`, folder layout,
8 input actions, 7 physics layers, autoload stubs, project settings)

---

## Part Completion Overview

| Part | Tasks | Status |
|---|---|---|
| 01 — Foundation | 5 tasks | 🔜 Ready (not started) |
| 02 — Player Movement | 5 tasks | ⏳ Pending Part 01 |
| 03 — Health & Damage | 4 tasks | ⏳ Pending Part 02 |
| 04 — Core Data | 5 tasks | ⏳ Pending Part 03 |
| 05 — Inventory Core | 4 tasks | ⏳ Pending Part 04 |
| 06 — Combat | 6 tasks | ⏳ Pending Part 05 |
| 07 — Enemy | 6 tasks | ⏳ Pending Part 06 |
| 08 — Inventory UI | 3 tasks | ⏳ Pending Part 07 |
| 09 — Test Level & Extraction | 7 tasks | ⏳ Pending Part 08 |
| 10 — Hub & Save | 7 tasks | ⏳ Pending Part 09 |
| 11 — Content UI | 3 tasks | ⏳ Pending Part 10 |
| 12 — QA & Export | 3 tasks | ⏳ Pending Part 11 |

**Total: 58 tasks across 12 parts.**

---

## Known Risks / Notes
- Godot `v4.7.1.stable.official [a13da4feb]` — user confirmed correct
- Physics layers per ADR-014 (bitmask values: 1,2,4,8,16,32,64)
- All sprites use Polygon2D fallback; PNGs placed by user override
- See AI_DOCS/ASSET_PROMPTS.md for all 19 sprite generation prompts
