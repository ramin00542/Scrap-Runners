# Task 01-04 — Confirm physics layer consistency across all pending task files (CORRECTED by ADR-014)

## ID
TASK-01-04

## Objective
Confirm that every not-yet-executed task file under `AI_DOCS/PARTS/`
(Parts 02, 06, 07, 09, 10 in particular, wherever `collision_layer` or
`collision_mask` values appear) uses the LOCKED 7-layer bitmask table from
ADR-014 (supersedes ADR-013), not any earlier draft numbering. This is a
documentation-only consistency task, required by 01_RULES.md section 11
whenever a cross-cutting locked convention changes before it has
propagated to all dependent, not-yet-executed task files.

## Prerequisites
- TASK-01-03 completed
- ADR-014 exists and is the authoritative physics layer bitmask table
  (ADR-013 is Superseded by ADR-014)

## Allowed Files
This task is documentation-only. Search (read-only) every file under
`AI_DOCS/PARTS/**/*.md` for the literal strings `collision_layer` and
`collision_mask`, and for any prose describing a specific layer number
or bitmask value.
List every file that needs correction as "Files Modified" in the report.
As a starting reference point (not a ceiling), these files are known to
contain physics layer values that must match ADR-014's corrected bitmask
table exactly:
- part_02_player_movement/task_02_01_player_scene_and_sprite.md
- part_06_combat/task_06_02_projectile_scene.md
- part_06_combat/task_06_04_hit_detection_dummy_target.md
- part_07_enemy/task_07_02_enemy_scene_and_health.md
- part_07_enemy/task_07_05_attack_state.md
- part_09_test_level_and_extraction/task_09_01_test_level_shell.md
- part_09_test_level_and_extraction/task_09_02_loot_drop_on_death.md
- part_09_test_level_and_extraction/task_09_04_extraction_point_zone.md
- part_10_hub_save/task_10_05_game_manager_and_hub_raid_transition.md (raid door mask)

## Requirements
1. For each file found, confirm its layer/mask values exactly match the
   ADR-014 corrected table:
   - world: layer #1 = bitmask 1, mask 0
   - player: layer #2 = bitmask 2, mask 1 (world)
   - enemies: layer #3 = bitmask 4, mask 1 (world)
   - player_projectiles: layer #4 = bitmask 8, mask 5 (world 1 + enemies 4)
   - enemy_projectiles: layer #5 = bitmask 16, mask 3 (world 1 + player 2)
   - pickups: layer #6 = bitmask 32, mask 2 (player)
   - extraction_zone: layer #7 = bitmask 64, mask 2 (player)
   With the corresponding mask combinations from ADR-014's entity table.
2. Correct any mismatch found (old values 3,4,5,6,7 used as layer numbers
   instead of bitmask values 4,8,16,32,64).
3. Confirm `05_TECH_SPEC.md`'s Physics Layers section matches ADR-014
   exactly (it should already, from TASK-01-01 corrected by ADR-014, but
   confirm here as this task is the formal checkpoint before Part 04 begins).
4. Confirm no remaining references to ADR-013 as authoritative — all
   references should now point to ADR-014 (ADR-013 is marked Superseded).

## Acceptance Criteria
- [ ] Every task file under AI_DOCS/PARTS/ that sets a collision
      layer/mask uses BITMASK values exactly matching ADR-014 (1,2,4,8,16,32,64
      and masks 0,1,3,5,2).
- [ ] Zero remaining references to the earlier, erroneous layer-number-as-value
      scheme (3,4,5,6,7 used as collision_layer) anywhere in AI_DOCS/PARTS/.
- [ ] Zero remaining references to ADR-013 as authoritative — all point to ADR-014.

## Test Procedure
1. Search AI_DOCS/PARTS/**/*.md for `collision_layer` and `collision_mask`.
2. For each hit, manually cross-check against ADR-014's corrected bitmask table.
3. Correct and re-search to confirm zero mismatches remain.
4. Search for "ADR-013" under AI_DOCS/PARTS/ and AI_DOCS/*.md — confirm none
   remain as authoritative (only historical notes saying superseded).

## Required Report Format
Implementation Mode.

---
This task must complete before TASK-04-01 begins (Part 04, Core Data).
Note: since no code has been written yet at this point in the project
(TASK-01-00/01/02/03 only touch documentation and project settings), this
is a pure documentation consistency pass — there is no runtime codebase
to migrate yet. Its purpose is to guarantee every task file that WILL be
executed later already has the correct bitmask numbers baked in, so this
mistake can never surface during actual implementation. ADR-013 is preserved
for audit but superseded by ADR-014.
