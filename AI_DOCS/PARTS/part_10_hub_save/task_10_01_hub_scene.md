# Task 10-01 — Hub scene shell

## ID
TASK-10-01

## Allowed Files
- Game/scenes/hub/hub.tscn
- Game/scripts/systems/hub.gd

## Requirements
1. `hub.tscn`: root `Node2D` named `Hub`, floor via `ColorRect` 640x360 dark gray (same as test_level, Polygon2D/ColorRect, no PNG),
   a `Marker2D` `PlayerSpawn`, a `Player` instance at spawn (fresh, empty
   Inventory — per ADR-012, intentional).
2. `hub.gd`: minimal, `extends Node2D`, empty `_ready()`.

## Acceptance Criteria
- [ ] Hub runs standalone with zero errors, player can move.

## Test Procedure
1. Run `hub.tscn` (F6), confirm movement.

## Required Report Format
Implementation Mode.
