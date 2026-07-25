# Task 06-05 — Ammo consumption and reload, with committed test

## ID
TASK-06-05

## Objective
Gate firing behind magazine ammo (refilled from Inventory reserve only
during reload), and add a committed test scene providing ammo without
Remote Inspector calls.

## Prerequisites
- TASK-06-04 completed
- part_05_inventory_core completed

## Allowed Files
- Game/scripts/entities/player_controller.gd
- Game/tests/combat_test.gd
- Game/tests/combat_test.tscn

## Requirements

### 1. `player_controller.gd` (extends the `_process_combat` pattern —
RENAMES it to `_process_combat_and_reload`, documented explicitly here
as the one expected rename in this codebase)
```gdscript
var _magazine_ammo: int = 0
var _is_reloading: bool = false
var _reload_timer: float = 0.0

func _ready() -> void:
    # NOTE: this `_ready()` is CUMULATIVE — it must contain EVERY line
    # added by TASK-03-03 above (health.died.connect), plus the new
    # magazine initialization. Do NOT drop any earlier line, or the
    # death-handling integration will silently regress.
    health.died.connect(_on_died)
    if equipped_weapon != null:
        _magazine_ammo = equipped_weapon.magazine_size

func _process(delta: float) -> void:
    _process_combat_and_reload(delta)

func _process_combat_and_reload(delta: float) -> void:
    if _fire_cooldown > 0.0:
        _fire_cooldown -= delta

    if _is_reloading:
        _reload_timer -= delta
        if _reload_timer <= 0.0:
            _finish_reload()
        return

    if Input.is_action_just_pressed("reload"):
        _start_reload()
        return

    if (
        Input.is_action_pressed("attack")
        and _fire_cooldown <= 0.0
        and equipped_weapon != null
        and _magazine_ammo > 0
    ):
        _fire()
        _magazine_ammo -= 1

func _start_reload() -> void:
    if equipped_weapon == null or _magazine_ammo >= equipped_weapon.magazine_size:
        return
    if not inventory.has_item(equipped_weapon.ammo_item_id, 1):
        return
    _is_reloading = true
    _reload_timer = equipped_weapon.reload_time

func _finish_reload() -> void:
    _is_reloading = false
    var needed: int = equipped_weapon.magazine_size - _magazine_ammo
    var available: int = 0
    for slot in inventory.get_slots():
        if not slot.is_empty() and slot.item.item_id == equipped_weapon.ammo_item_id:
            available += slot.amount
    var to_reload: int = min(needed, available)
    inventory.remove_item(equipped_weapon.ammo_item_id, to_reload)
    _magazine_ammo += to_reload
```

### 2. Committed test
`combat_test.tscn` script:
```gdscript
extends Node2D

const BASIC_AMMO: ItemData = preload("res://resources/items/basic_ammo.tres")

@onready var player: PlayerController = $Player

func _ready() -> void:
    player.inventory.add_item(BASIC_AMMO, 30)
    print("Combat test ready. Player has 30 reserve ammo + full magazine.")
```

## Acceptance Criteria
- [ ] Firing with 0 magazine ammo does nothing, no error.
- [ ] Reload with available reserve refills magazine after `reload_time`,
      correctly deducts from Inventory.
- [ ] Reload with full magazine or empty reserve is a no-op.

## Test Procedure
1. Run `combat_test.tscn` (F6), fire 10 times (magazine empties).
2. Press R, wait 1.2s, confirm magazine refills to 10, reserve drops to 20.

## Required Report Format
Implementation Mode.
