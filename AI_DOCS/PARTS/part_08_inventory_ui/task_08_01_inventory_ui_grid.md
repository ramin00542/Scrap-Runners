# Task 08-01 — Grid-based inventory UI

## ID
TASK-08-01

## Objective
Display Inventory as a grid, using an explicit `setup()` method for
wiring (more reliable than `@export` for nested-scene assembly).

## Prerequisites
- part_05_inventory_core completed

## Allowed Files
- Game/scenes/ui/inventory_ui.tscn
- Game/scripts/systems/inventory_ui.gd

## Requirements
1. `inventory_ui.tscn`: root `Control` named `InventoryUI`, containing a
   `GridContainer` (columns = 4) with 16 child `Panel` nodes, each with a
   `TextureRect` and `Label`.
2. `inventory_ui.gd`:
```gdscript
class_name InventoryUI
extends Control

var inventory: Inventory

@onready var grid: GridContainer = $GridContainer

func setup(source_inventory: Inventory) -> void:
    inventory = source_inventory
    inventory.inventory_changed.connect(_refresh)
    _refresh()

func _refresh() -> void:
    var slots: Array[InventorySlot] = inventory.get_slots()
    for i in range(grid.get_child_count()):
        var panel: Panel = grid.get_child(i)
        var icon: TextureRect = panel.get_node("TextureRect")
        var label: Label = panel.get_node("Label")
        if i < slots.size() and not slots[i].is_empty():
            icon.texture = slots[i].item.icon
            icon.visible = true
            label.text = str(slots[i].amount)
            label.visible = slots[i].amount > 1
        else:
            icon.visible = false
            label.visible = false
```

## Acceptance Criteria
- [ ] UI reflects Inventory contents live on `inventory_changed`.
- [ ] Empty slots show no icon/label; single stacks hide quantity.

## Test Procedure
1. Instance `inventory_ui.tscn`, call `setup()` with a populated
   Inventory, confirm live updates.

## Required Report Format
Implementation Mode.
