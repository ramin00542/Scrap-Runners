# Task 04-01 — Create the ItemData resource class

## ID
TASK-04-01

## Prerequisites
- part_01_foundation fully completed (through TASK-01-04)
- part_03_health_damage fully completed and verified (per ADR-008 locked order — Part 04 must not start before Part 03)

## Allowed Files
- Game/scripts/data/item_data.gd

## Requirements
```gdscript
class_name ItemData
extends Resource

@export var item_id: StringName
@export var display_name: String
@export var icon: Texture2D
@export var stack_size: int = 1
@export var item_type: ItemType = ItemType.MATERIAL

enum ItemType { MATERIAL, WEAPON, CONSUMABLE, KEY_ITEM }
```

## Acceptance Criteria
- [ ] File compiles with zero parser errors.
- [ ] "ItemData" appears as a creatable resource type.
- [ ] Fields match 06_DATA_SCHEMA.md exactly.

## Test Procedure
1. Create New Resource "ItemData" in the editor, confirm fields, discard.

## Required Report Format
Implementation Mode.
