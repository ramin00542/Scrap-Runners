# Task 09-01 — Test level shell (CORRECTED - no PNG)

## ID
TASK-09-01

## Objective
Build the test level with EXPLICIT world-layer collision on walls,
never relying on Godot's implicit default (per ADR-014 and 01_RULES.md
section 9's "no magic numbers/implicit values" rule).

## Prerequisites
- part_07_enemy completed

## Allowed Files
- Game/scenes/levels/test_level.tscn
- Game/scripts/systems/test_level.gd

## Requirements
1. `test_level.tscn`: root `Node2D` named `TestLevel`, containing:
   - A `ColorRect` or `Polygon2D` floor background 640x360 (dark gray #2B2B2B) — no external image needed, avoids PNG generation issue.
   - Wall boundaries as `StaticBody2D` nodes with `ColorRect` visuals, each EXPLICITLY setting
     `collision_layer = 1` (world, per ADR-014) and `collision_mask = 0`
     — do not rely on the default value. Visuals via Polygon2D/ColorRect do NOT require asset license entry.
   - A `Node2D` named `Entities` with **Y-sort enabled** (`y_sort_enabled = true`) — required by ADR-016 for correct draw order of entities with visual offsets. Player, Enemy, and LootPickup instances must be children of this node.
   - Inside `Entities`: a `Marker2D` named `PlayerSpawn`, a `Player` instance positioned at `PlayerSpawn`, and one `enemy_patrol_drone.tscn` instance with patrol points wired.
   - **Exclusions (per ADR-016):** ExtractionPoint, Projectiles, and CanvasLayer UI elements do NOT use the `Vector2(0, -4)` visual offset — they are not "standing entities" and must remain at their natural positions.
2. `test_level.gd`: minimal for now, e.g. `extends Node2D` with empty `_ready()`.

## Acceptance Criteria
- [ ] Level runs with zero errors, player spawns correctly, walls
      EXPLICITLY set to collision_layer = 1 correctly block movement
      (verified by walking into a wall and confirming no clipping).
- [ ] The enemy patrols/chases/attacks correctly.
- [ ] No external PNG placeholder required — floors/walls are ColorRect/Polygon2D.

## Test Procedure
1. Run `test_level.tscn` (F6), confirm wall collision and enemy behavior.

## Required Report Format
Implementation Mode.

## Note
Previous version required `floor_placeholder.png` + asset license. Corrected to use
Polygon2D/ColorRect for reliability per 04_ART_DIRECTION.md placeholder rules.
