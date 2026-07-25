# Task 10-02 — StashData resource structure, with committed test

## ID
TASK-10-02

## Allowed Files
- Game/scripts/data/stash_entry.gd
- Game/scripts/data/stash_data.gd
- Game/tests/stash_data_test.gd
- Game/tests/stash_data_test.tscn

## Requirements
```gdscript
class_name StashEntry
extends Resource

@export var item_id: StringName
@export var amount: int
```

```gdscript
class_name StashData
extends Resource

@export var entries: Array[StashEntry] = []

func add_amount(item_id: StringName, amount: int) -> void:
    if amount <= 0:
        return
    for entry in entries:
        if entry.item_id == item_id:
            entry.amount += amount
            return
    var new_entry := StashEntry.new()
    new_entry.item_id = item_id
    new_entry.amount = amount
    entries.append(new_entry)
```

Committed test `stash_data_test.gd` (on a plain `Node`):
```gdscript
extends Node

func _ready() -> void:
    var stash := StashData.new()
    stash.add_amount(&"basic_ammo", 10)
    stash.add_amount(&"basic_ammo", 5)
    print("PASS: merges into existing entry" if stash.entries.size() == 1 and stash.entries[0].amount == 15 else "FAIL")

    stash.add_amount(&"scrap_pistol", 1)
    print("PASS: creates new entry for new item" if stash.entries.size() == 2 else "FAIL")

    stash.add_amount(&"basic_ammo", 0)
    print("PASS: amount 0 is a no-op" if stash.entries[0].amount == 15 else "FAIL")
```

## Acceptance Criteria
- [ ] Both resources creatable and inspectable.
- [ ] All 3 PASS lines print when running `stash_data_test.tscn`.

## Test Procedure
1. Run `stash_data_test.tscn` (F6), confirm 3 PASS lines.

## Required Report Format
Implementation Mode.
