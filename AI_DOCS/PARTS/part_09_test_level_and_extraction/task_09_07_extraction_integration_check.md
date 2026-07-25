# Task 09-07 — Verify Part 09 end-to-end, with deterministic loot for testing

## ID
TASK-09-07

## Objective
Verify the full Part 09 loop. Use a DEDICATED deterministic test resource
(drop_chance = 1.0) for this verification, so the check is not flaky —
without ever modifying the real, content-tuned `patrol_drone.tres`
(which correctly keeps `drop_chance = 0.8` for actual gameplay).

## Allowed Files
- Game/resources/loot_tables/patrol_drone_loot_test.tres
- Game/resources/enemies/patrol_drone_test.tres
- Game/tests/extraction_integration_test.tscn
- AI_DOCS/CHANGELOG.md

## Requirements
1. `patrol_drone_loot_test.tres`: copy of `patrol_drone_loot.tres` with
   `drop_chance = 1.0`.
2. `patrol_drone_test.tres`: copy of `patrol_drone.tres` referencing
   `patrol_drone_loot_test.tres` instead.
3. `extraction_integration_test.tscn`: a copy of `test_level.tscn`'s
   structure, but its Enemy instance uses `patrol_drone_test.tres` —
   used ONLY for this verification, never for real gameplay.

## Acceptance Criteria
- [ ] Killing the enemy in this test scene ALWAYS drops loot
      (deterministic, drop_chance = 1.0).
- [ ] Full loop verified: kill enemy → guaranteed loot → pick up →
      navigate via indicator → extract with live progress bar → complete.

## Test Procedure
1. Run `extraction_integration_test.tscn`, complete the full loop,
   confirm zero errors and guaranteed loot pickup.

## Required Report Format
Implementation Mode, changes limited mostly to new test files plus
CHANGELOG.md, only after real verification.
