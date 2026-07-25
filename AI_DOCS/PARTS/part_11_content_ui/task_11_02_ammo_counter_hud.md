# Task 11-02 — Ammo counter HUD

## ID
TASK-11-02

## Objective
Add an ammo counter, following the `_process_xxx()` composition
convention — adds a call, never replaces `_process()`.

## Allowed Files
- Game/scenes/entities/player/player.tscn
- Game/scripts/entities/player_controller.gd

## Requirements
1. Add `Label` named `AmmoLabel` to Player's `CanvasLayer`, bottom-right.
2. In `player_controller.gd` (diff):
```gdscript
@onready var ammo_label: Label = $CanvasLayer/AmmoLabel

func _process(delta: float) -> void:
    _process_combat_and_reload(delta)
    _update_extraction_indicator()
    _update_ammo_label()

func _update_ammo_label() -> void:
    if equipped_weapon == null:
        ammo_label.text = ""
        return
    var reserve: int = 0
    for slot in inventory.get_slots():
        if not slot.is_empty() and slot.item.item_id == equipped_weapon.ammo_item_id:
            reserve += slot.amount
    ammo_label.text = "%d / %d" % [_magazine_ammo, reserve]
```

## Acceptance Criteria
- [ ] Label shows `magazine / reserve`, updates correctly on fire/reload.
- [ ] `_process()` still correctly calls combat, indicator, and ammo
      label logic together (regression check).

## Test Procedure
1. Run, fire and reload, confirm label updates at each step.
2. Confirm combat and extraction indicator still work (regression).

## Required Report Format
Implementation Mode.
