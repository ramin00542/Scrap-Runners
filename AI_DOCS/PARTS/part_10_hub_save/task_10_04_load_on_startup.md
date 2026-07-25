# Task 10-04 — Load Stash into Hub on startup

## ID
TASK-10-04

## Prerequisites
- TASK-10-00, TASK-10-03 completed

## Allowed Files
- Game/scenes/hub/hub.tscn
- Game/scripts/systems/hub.gd

## Requirements
1. Add `Inventory` child to Hub, named `StashInventory` (`slot_count = 128`
   — generous capacity for a persistent stash, avoiding the overflow risk
   of an under-sized container).
2. `hub.gd`:
```gdscript
extends Node2D

@onready var stash_inventory: Inventory = $StashInventory

func _ready() -> void:
    var stash: StashData = SaveManager.load_stash()
    for entry in stash.entries:
        var item: ItemData = ItemDatabase.get_item(entry.item_id)
        if item != null:
            var leftover: int = stash_inventory.add_item(item, entry.amount)
            if leftover > 0:
                push_warning("Stash inventory capacity exceeded for item_id: %s (%d units not shown)" % [entry.item_id, leftover])
        else:
            push_warning("Unknown item_id in save file: %s" % entry.item_id)
```

## Acceptance Criteria
- [ ] Items saved previously repopulate `StashInventory` correctly.
- [ ] Unrecognized item_id logs a warning, no crash.
- [ ] Capacity overflow logs a warning instead of silently losing data
      visibility.

## Test Procedure
1. Save a StashData with `basic_ammo` via `stash_data_test.tscn`-style
   setup or the real gameplay loop, reload Hub, confirm StashInventory.

## Required Report Format
Implementation Mode.
