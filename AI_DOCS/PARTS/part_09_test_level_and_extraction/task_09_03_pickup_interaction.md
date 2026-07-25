# Task 09-03 — Verify pickup interaction with committed test

## ID
TASK-09-03

## Objective
Verify pickup interaction including partial pickup when inventory nearly full,
using a committed test scene (not just manual verification).

## Prerequisites
- TASK-09-02 completed
- part_05_inventory_core completed

## Allowed Files
- Game/tests/loot_pickup_test.tscn
- Game/tests/loot_pickup_test.gd
- AI_DOCS/CHANGELOG.md (only if verified per 6.4)

## Requirements
1. Create `loot_pickup_test.tscn`: root `Node2D` with:
   - `Player` instance
   - `LootPickup` instance (e.g. basic_ammo, amount 20) positioned within interact range
   - Script attached to root for automated test
2. `loot_pickup_test.gd`:
```gdscript
extends Node2D

@onready var player: PlayerController = $Player
@onready var pickup: LootPickup = $LootPickup

var _test_count: int = 0
var _fail_count: int = 0

func _check(label: String, condition: bool) -> void:
    _test_count += 1
    if condition:
        print("PASS: %s" % label)
    else:
        _fail_count += 1
        print("FAIL: %s" % label)

func _ready() -> void:
    # Setup: fill inventory to 15/16 slots with dummy items to leave 1 slot with partial space
    # Use small stack_size item to force partial pickup scenario
    var dummy := ItemData.new()
    dummy.item_id = &"dummy_filler"
    dummy.display_name = "Filler"
    dummy.stack_size = 1
    
    for i in range(14):
        var item := ItemData.new()
        item.item_id = StringName("filler_%d" % i)
        item.display_name = "Filler %d" % i
        item.stack_size = 1
        player.inventory.add_item(item, 1)
    
    # Add almost full stack of same item as pickup to test partial
    var ammo := ItemData.new()
    ammo.item_id = &"basic_ammo"
    ammo.display_name = "Scrap Ammo"
    ammo.stack_size = 60
    player.inventory.add_item(ammo, 55)  # slot has 55/60, 5 space left
    
    pickup.item = ammo
    pickup.amount = 20  # 20 pickup, only 5 fits, 15 should remain
    
    # Simulate player in range
    pickup._player_in_range = true
    pickup._player_inventory = player.inventory
    
    # Test the add_item / partial-fill logic directly:
    var leftover: int = player.inventory.add_item(pickup.item, pickup.amount)
    _check("partial pickup leftover correct", leftover == 15)
    _check("inventory has 60 after partial fill", player.inventory.has_item(&"basic_ammo", 60))
    
    # Simulate pickup's partial logic
    pickup.amount = leftover
    _check("pickup amount reduced to leftover", pickup.amount == 15)
    _check("pickup persists with reduced amount when inventory full", pickup.amount > 0)
    
    print("---")
    print("%d/%d PASS" % [_test_count - _fail_count, _test_count])
    print("ALL PASS" if _fail_count == 0 else "FAILURES DETECTED")

func _process(_delta: float) -> void:
    # Allow manual testing too: walk into range, press interact
    pass
```

3. Acceptance for this task includes both manual verification (walk + E) AND committed test passing.

## Acceptance Criteria
- [ ] Item added correctly, respecting `add_item()`'s partial-fill (60 max stack, 55 existing + 20 pickup = 60 stored, 15 leftover)
- [ ] Pickup persists with reduced amount (15) if inventory couldn't take all
- [ ] `loot_pickup_test.tscn` prints ALL PASS
- [ ] Manual: walk into range, press interact, confirm pickup disappears or reduces

## Test Procedure
1. Run `loot_pickup_test.tscn` (F6), confirm ALL PASS
2. Run `test_level.tscn`, fill inventory near capacity, pick up large stack, confirm partial pickup behavior manually

## Required Report Format
Implementation Mode.
