# Task 10-00 — ItemDatabase autoload

## ID
TASK-10-00

## Objective
Create a lookup table mapping `item_id -> ItemData`, per ADR-011: used
by SaveManager/Hub for reconstructing item references from save data;
gameplay systems continue to carry direct references.

## Prerequisites
- part_09_test_level_and_extraction completed
- basic_ammo.tres, scrap_pistol.tres exist

## Allowed Files
- Game/scripts/autoload/item_database.gd
- Game/scenes/autoload/item_database.tscn
- Game/project.godot

## Requirements

**Note on Testing Discipline:** ItemDatabase is a simple lookup table; its verification is via SaveManager round-trip tests in TASK-10-03 and hub loading in TASK-10-04.
Per 01_RULES.md section 10, this task explicitly notes that no separate committed test scene is created here because functionality is exercised in those downstream integration tests, plus manual Remote check in Test Procedure.

1. `item_database.gd` (no `class_name`, per ADR-009):
```gdscript
extends Node

@export var all_items: Array[ItemData] = []

var _by_id: Dictionary[StringName, ItemData] = {}

func _ready() -> void:
    for item: ItemData in all_items:
        if item == null:
            continue
        if item.item_id == StringName():
            push_warning("ItemDatabase: ItemData with empty item_id ignored: %s" % item.resource_path)
            continue
        if _by_id.has(item.item_id):
            push_warning("ItemDatabase: duplicate item_id %s, keeping first, ignoring %s" % [item.item_id, item.resource_path])
            continue
        _by_id[item.item_id] = item

func get_item(item_id: StringName) -> ItemData:
    return _by_id.get(item_id, null)
```
2. `item_database.tscn`: root `Node` named `ItemDatabase`, script above,
   `all_items` populated with `basic_ammo.tres`, `scrap_pistol.tres`.
3. Register the SCENE (not just script) as autoload `ItemDatabase`.

## Acceptance Criteria
- [ ] `ItemDatabase.get_item(&"basic_ammo")` returns the correct
      resource at runtime.
- [ ] `ItemDatabase.get_item(&"nonexistent")` returns null, no error.

## Test Procedure
1. Run any scene, Remote tab, find ItemDatabase autoload node, confirm
   `all_items` populated. Call `get_item(&"basic_ammo")`, confirm result.

## Required Report Format
Implementation Mode.

---
Convention: any future task creating a new ItemData/WeaponData `.tres`
MUST also add it to `item_database.tscn`'s `all_items` array in the SAME
task, with that scene file in its Allowed Files.
