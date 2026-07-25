# Task 06-04 — Verify hit detection against a committed dummy target

## ID
TASK-06-04

## Prerequisites
- TASK-06-03 completed

## Allowed Files
- Game/tests/dummy_target.tscn
- Game/tests/dummy_target.gd
- Game/tests/combat_test.tscn

## Requirements
1. `dummy_target.gd`:
```gdscript
extends StaticBody2D

@onready var health: HealthComponent = $HealthComponent

func _ready() -> void:
    health.died.connect(func(): queue_free())
```
2. `dummy_target.tscn`: root `StaticBody2D` named `DummyTarget`,
   `collision_layer = 4` (enemies, per ADR-014, for test purposes),
   `collision_mask = 1` (world), child `CollisionShape2D` (16x16 box),
   child `HealthComponent` (`max_health = 20`), script `dummy_target.gd`.
3. `combat_test.tscn`: contains one `Player` instance and one
   `DummyTarget` instance a short distance away.

## Acceptance Criteria
- [ ] Firing at the dummy reduces its health; it disappears after
      3 hits (20 health / 8 damage).
- [ ] Layer values exactly match ADR-014 for the "enemies" role.

## Test Procedure
1. Run `combat_test.tscn` (F6), fire 3 times, confirm dummy disappears.

## Required Report Format
Implementation Mode.
