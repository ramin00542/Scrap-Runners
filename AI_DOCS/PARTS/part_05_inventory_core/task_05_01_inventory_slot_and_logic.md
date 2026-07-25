# Task 05-01 — Create InventorySlot and the Inventory component, with committed tests

## ID
TASK-05-01

## Prerequisites
- part_04_core_data completed and TASK-04-05 verified

## Allowed Files
- Game/scripts/data/inventory_slot.gd
- Game/scripts/systems/inventory.gd
- Game/tests/inventory_test.gd
- Game/tests/inventory_test.tscn

## Requirements

### 1. `inventory_slot.gd`
```gdscript
class_name InventorySlot
extends RefCounted

var item: ItemData
var amount: int = 0

func is_empty() -> bool:
    return item == null or amount <= 0
```

### 2. `inventory.gd`
```gdscript
class_name Inventory
extends Node

## Emitted exactly once per call that actually changes inventory state.
signal inventory_changed()

@export_range(1, 128, 1) var slot_count: int = 16

var _slots: Array[InventorySlot] = []

func _ready() -> void:
    _slots.resize(slot_count)
    for i in range(slot_count):
        _slots[i] = InventorySlot.new()

func add_item(item: ItemData, amount: int = 1) -> int:
    if item == null or amount <= 0:
        return max(amount, 0)
    if item.stack_size <= 0:
        push_error("ItemData.stack_size must be > 0 for item_id: %s" % item.item_id)
        return amount

    var remaining: int = amount
    var changed: bool = false

    for slot in _slots:
        if remaining <= 0:
            break
        if (
            not slot.is_empty()
            and slot.item.item_id == item.item_id
            and slot.amount < item.stack_size
        ):
            var space: int = item.stack_size - slot.amount
            var to_add: int = min(space, remaining)
            slot.amount += to_add
            remaining -= to_add
            changed = true

    for slot in _slots:
        if remaining <= 0:
            break
        if slot.is_empty():
            var to_add: int = min(item.stack_size, remaining)
            slot.item = item
            slot.amount = to_add
            remaining -= to_add
            changed = true

    if changed:
        inventory_changed.emit()

    return remaining

func remove_item(item_id: StringName, amount: int = 1) -> int:
    if amount <= 0:
        return 0

    var remaining: int = amount
    var removed: int = 0

    for slot in _slots:
        if remaining <= 0:
            break
        if not slot.is_empty() and slot.item.item_id == item_id:
            var to_remove: int = min(slot.amount, remaining)
            slot.amount -= to_remove
            removed += to_remove
            remaining -= to_remove
            if slot.amount <= 0:
                slot.item = null
                slot.amount = 0

    if removed > 0:
        inventory_changed.emit()

    return removed

func has_item(item_id: StringName, amount: int = 1) -> bool:
    if amount <= 0:
        return true

    var total: int = 0
    for slot in _slots:
        if not slot.is_empty() and slot.item.item_id == item_id:
            total += slot.amount
            if total >= amount:
                return true
    return total >= amount

func get_slot_count() -> int:
    return _slots.size()

func get_slot(index: int) -> InventorySlot:
    return _slots[index]

func get_slots() -> Array[InventorySlot]:
    return _slots.duplicate()
```

### 3. Committed test
`inventory_test.tscn`: root `Node` with a child `Inventory` node
(`slot_count = 4`) named `TestInventory`, script `inventory_test.gd`:

```gdscript
extends Node

@onready var inventory: Inventory = $TestInventory

var _test_count: int = 0
var _fail_count: int = 0
var _signal_count_before: int = 0

func _ready() -> void:
    _run_all_tests()
    print("---")
    print("%d/%d tests passed" % [_test_count - _fail_count, _test_count])
    print("ALL PASS" if _fail_count == 0 else "FAILURES DETECTED")

func _check(label: String, condition: bool) -> void:
    _test_count += 1
    if condition:
        print("PASS: %s" % label)
    else:
        _fail_count += 1
        print("FAIL: %s" % label)

func _make_item(id: StringName, stack: int) -> ItemData:
    var item := ItemData.new()
    item.item_id = id
    item.display_name = String(id)
    item.stack_size = stack
    return item

func _on_inventory_changed_for_noop_test() -> void:
    _signal_count_before += 1

func _run_all_tests() -> void:
    var ammo: ItemData = _make_item(&"basic_ammo", 5)

    var leftover: int = inventory.add_item(ammo, 3)
    _check("add_item: partial fill returns 0 leftover", leftover == 0)
    _check("add_item: amount stored correctly", inventory.has_item(&"basic_ammo", 3))

    leftover = inventory.add_item(ammo, 2)
    _check("add_item: fills existing stack to capacity", leftover == 0)
    _check("add_item: has 5 total after filling stack", inventory.has_item(&"basic_ammo", 5))

    leftover = inventory.add_item(ammo, 1)
    _check("add_item: overflow goes to new slot", leftover == 0)
    _check("add_item: has 6 total across two slots", inventory.has_item(&"basic_ammo", 6))

    inventory.inventory_changed.connect(_on_inventory_changed_for_noop_test)

    leftover = inventory.add_item(null, 5)
    _check("add_item: null item is no-op, returns amount", leftover == 5)

    leftover = inventory.add_item(ammo, 0)
    _check("add_item: amount 0 is no-op, returns 0", leftover == 0)

    leftover = inventory.add_item(ammo, -3)
    _check("add_item: negative amount is no-op, returns 0", leftover == 0)

    _check("no-op calls did not emit inventory_changed", _signal_count_before == 0)

    leftover = inventory.add_item(ammo, 20)
    _check("add_item: overflow beyond capacity returns correct leftover", leftover == 6)

    var ammo_duplicate: ItemData = _make_item(&"basic_ammo", 5)
    _check(
        "has_item: recognizes different resource object with same item_id",
        inventory.has_item(ammo_duplicate.item_id, 1)
    )

    var removed: int = inventory.remove_item(&"basic_ammo", 1000)
    _check("remove_item: removes all available, no more", removed == 20)
    _check("remove_item: inventory now empty for this item", not inventory.has_item(&"basic_ammo", 1))

    removed = inventory.remove_item(&"basic_ammo", 5)
    _check("remove_item: removing from empty returns 0", removed == 0)

    removed = inventory.remove_item(&"basic_ammo", 0)
    _check("remove_item: amount 0 is no-op", removed == 0)

    var found_stale_reference: bool = false
    for slot in inventory.get_slots():
        if slot.item != null and slot.item.item_id == &"basic_ammo":
            found_stale_reference = true
    _check("slots are cleared (item == null) after full removal", not found_stale_reference)
```

## Acceptance Criteria
- [ ] `inventory_test.tscn` prints "ALL PASS".
- [ ] Stacking/removal/lookup compare by `item_id`.
- [ ] `inventory_changed` fires zero times for no-ops.
- [ ] `Inventory` is a `Node` with editable `slot_count`.

## Test Procedure
1. Run `inventory_test.tscn` (F6), confirm "ALL PASS".

## Required Report Format
Implementation Mode.
