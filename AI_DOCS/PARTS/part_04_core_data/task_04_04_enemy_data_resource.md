# Task 04-04 — Create the EnemyData resource class

## ID
TASK-04-04

## Prerequisites
- TASK-04-03 completed

## Allowed Files
- Game/scripts/data/enemy_data.gd

## Requirements
```gdscript
class_name EnemyData
extends Resource

@export var enemy_id: StringName
@export var display_name: String
@export var max_health: float = 30.0
@export var move_speed: float = 60.0
@export var detection_radius: float = 120.0
@export var attack_damage: float = 5.0
@export var attack_cooldown: float = 1.0
@export var loot_table: LootTable
```

## Acceptance Criteria
- [ ] File compiles with zero errors.
- [ ] Inspector shows exactly 8 fields in order, Loot Table accepts a
      LootTable resource.

## Test Procedure
1. Create EnemyData, confirm 8 fields in order, discard.

## Required Report Format
Implementation Mode. Note in Known Limitations: no `.tres` instances yet
— created in Part 06/07.
