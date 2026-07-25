# Task 04-03 — Create LootEntry and LootTable resource classes

## ID
TASK-04-03

## Prerequisites
- TASK-04-01 completed

## Allowed Files
- Game/scripts/data/loot_entry.gd
- Game/scripts/data/loot_table.gd

## Requirements
```gdscript
class_name LootEntry
extends Resource

@export var item: ItemData
@export var min_amount: int = 1
@export var max_amount: int = 1
@export_range(0.0, 1.0) var drop_chance: float = 1.0
```

```gdscript
class_name LootTable
extends Resource

@export var entries: Array[LootEntry] = []
```

## Acceptance Criteria
- [ ] Both compile with zero errors.
- [ ] LootTable's Entries array only accepts LootEntry.

## Test Procedure
1. Create LootTable, add a LootEntry to its array, confirm sub-fields.

## Required Report Format
Implementation Mode.
