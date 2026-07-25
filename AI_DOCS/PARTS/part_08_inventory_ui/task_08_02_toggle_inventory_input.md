# Task 08-02 — Toggle inventory visibility

## ID
TASK-08-02

## Allowed Files
- Game/scripts/entities/player_controller.gd
- Game/scenes/entities/player/player.tscn

## Requirements
1. Add a `CanvasLayer` child to Player with `InventoryUI` instanced
   inside, `visible = false` by default.
2. In `player_controller.gd`:
```gdscript
@onready var inventory_ui: InventoryUI = $CanvasLayer/InventoryUI

func _ready() -> void:
    # NOTE: this `_ready()` is CUMULATIVE — it must contain EVERY line
    # added by TASK-03-03 and TASK-06-05 below (health.died.connect,
    # magazine init), plus the new inventory_ui.setup call. Do NOT drop
    # any earlier line, or death-handling / ammo will silently regress.
    health.died.connect(_on_died)
    if equipped_weapon != null:
        _magazine_ammo = equipped_weapon.magazine_size
    inventory_ui.setup(inventory)

func _unhandled_input(event: InputEvent) -> void:
    if event.is_action_pressed("toggle_inventory"):
        inventory_ui.visible = not inventory_ui.visible
```

## Acceptance Criteria
- [ ] Pressing Tab shows/hides the inventory UI.
- [ ] Movement still works while UI is open (no pause in MVP).

## Test Procedure
1. Run, press Tab repeatedly, confirm toggle.

## Required Report Format
Implementation Mode.
