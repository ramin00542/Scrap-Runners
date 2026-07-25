# Test Plan

Every task's Acceptance Criteria must be verifiable using the procedures
below. Per 01_RULES.md section 10, tests for non-trivial logic are
committed under `Game/tests/` in the same task that introduces the logic.

## Manual Test Procedure (baseline, applies to every task)

1. Open `Game/project.godot` in the locked Godot version.
2. Run the project (F5) or the specific scene under test (F6).
3. Check the Debugger > Errors tab.
4. Perform the steps listed in the task's "Test Procedure" section.
5. Confirm each Acceptance Criterion individually.

## Per-System Smoke Tests

### Movement (Part 02)
- All 8 directions produce correct movement, numerically verified
  (velocity magnitude equals move_speed axis-aligned, never exceeds it
  diagonally, zero on zero input).
- Camera follows with visible smoothing lag, not instant snapping.
- Character does not clip through world-layer geometry.

### Health/Damage (Part 03)
- `health_changed` fires exactly once per damage/heal call that changes
  the value.
- Health never exceeds max or drops below 0.
- `died()` fires exactly once.
- `initialize(value)` correctly sets both max_health and current_health,
  regardless of prior `_ready()` state (avoids child-before-parent
  ordering bugs).
- Player stops responding to movement input after death (committed test,
  not Remote Inspector).

### Core Data (Part 04)
- All five resource classes are creatable in the editor with zero errors.
- EnemyData's `loot_table` field accepts a LootTable resource.

### Inventory (Part 05)
- Stacking/removal/lookup compare by `item_id`, never object reference.
- `inventory_changed` fires exactly once per state-changing call, zero
  times for no-ops (null item, amount <= 0, remove of absent item).
- `get_slots()` returns a duplicated array.

### Combat (Part 06)
- Firing consumes magazine ammo; magazine refills from Inventory reserve
  only during reload.
- Firing with 0 magazine ammo does nothing, no error.
- Fire rate and reload time are respected.
- Projectile layer/mask exactly match ADR-014's corrected bitmask table (ADR-013 superseded — values 8/5 for player projectile, 16/3 for enemy projectile).
- Hit detection verified against a committed dummy target scene.

### Enemy AI (Part 07)
- Patrol points remain fixed in world space (cached once in `_ready()`,
  never re-read from a moving Marker2D parented to the enemy).
- patrol → chase → attack → chase → patrol transitions all verified in a
  committed test scene with printed state log.
- `enemy_died` fires exactly once with correct `enemy_id`.
- Enemy's `HealthComponent` is initialized via `initialize()`, never left
  to the child-before-parent `_ready()` default.

### Loot / Extraction (Part 09)
- LootPickup's `item`/`amount` are set BEFORE `add_child()`, never after.
- Enemy death rolls its `loot_table` correctly.
- Extraction start/progress/cancel/complete signals all fire correctly
  and exactly once each per raid attempt.
- Any new per-frame logic added to `player_controller.gd`'s `_process()`
  is added as a new `_process_xxx()` method called from the existing
  `_process()`, never as a full replacement (see 01_RULES.md section 9
  convention established in Part 06).

### Save/Load (Part 10)
- Save/load round-trips correctly using a TEST-ONLY save path, never the
  real `user://stash_save.tres` file.
- A corrupted or missing save file returns an empty StashData, no crash.
- Raid failure (player death) returns to Hub with Stash unchanged.
- Raid success transfers only the raid Inventory's contents to Stash —
  the fixed Scrap Pistol and its magazine are never transferred (ADR-012).
- Integration/full-loop tests use a DETERMINISTIC loot table
  (`drop_chance = 1.0`), never the real content-tuned table, to avoid
  flaky test results.

## Regression Rule

Before marking any task complete, re-run the smoke tests of ALL
previously completed tasks that touch the same Autoload, scene, or
component.

## Verification Authority

Per 01_RULES.md section 6.4, no test result may be reported as passing
in a Changelog entry unless it was actually executed.
