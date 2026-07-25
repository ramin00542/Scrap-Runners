# Data Schema — Resource Definitions

All game data is defined as a custom `Resource` subclass. Built in
**part_04_core_data**, in this exact dependency order:

1. ItemData
2. WeaponData (extends ItemData)
3. LootEntry / LootTable
4. EnemyData (references LootTable)

## Identity and Equality Convention (applies to ALL resources)

`item_id` (and `enemy_id`) is the canonical identity of a data resource,
NOT the object reference. All comparisons — stacking, removal,
`has_item()`, save serialization — MUST compare by `item_id`
(`StringName`), never by `==` object reference or `is` identity.

Only ONE `.tres` resource should exist per unique `item_id`. Two
different `.tres` files sharing an `item_id` is a content bug, not a
supported feature, but code must not crash — it treats them as the same
item.

## 1. ItemData (`res://scripts/data/item_data.gd`)

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

## 2. WeaponData (`res://scripts/data/weapon_data.gd`)

```gdscript
class_name WeaponData
extends ItemData

@export var damage: float = 10.0
@export var fire_rate: float = 0.3
@export var projectile_scene: PackedScene
@export var ammo_item_id: StringName
@export var magazine_size: int = 12
@export var reload_time: float = 1.5
```

`ammo_item_id` is a `StringName`, never a direct `ItemData` reference.
Melee fields are out of scope — see ADR-006.

## 3. LootEntry (`res://scripts/data/loot_entry.gd`)

```gdscript
class_name LootEntry
extends Resource

@export var item: ItemData
@export var min_amount: int = 1
@export var max_amount: int = 1
@export_range(0.0, 1.0) var drop_chance: float = 1.0
```

## 4. LootTable (`res://scripts/data/loot_table.gd`)

```gdscript
class_name LootTable
extends Resource

@export var entries: Array[LootEntry] = []
```

## 5. EnemyData (`res://scripts/data/enemy_data.gd`)

```gdscript
class_name EnemyData
extends Resource

@export var enemy_id: StringName
@export var display_name: String
@export var max_health: float = 30.0
@export var move_speed: float = 60.0
@export var detection_radius: float = 120.0
@export var attack_damage: float = 5.0
@export var attack_cooldown: float = 1.0
@export var loot_table: LootTable
```

Exactly 8 exported fields, in this order.

## InventorySlot (runtime, not a Resource — part_05_inventory_core)

```gdscript
class_name InventorySlot
extends RefCounted

var item: ItemData
var amount: int = 0

func is_empty() -> bool:
    return item == null or amount <= 0
```

## Inventory (Node component — part_05_inventory_core)

```gdscript
class_name Inventory
extends Node

signal inventory_changed()

@export_range(1, 128, 1) var slot_count: int = 16
```

## Inventory Model (LOCKED — ADR-001)

Slot-based grid inventory, NOT spatial/rotatable. Fixed slot count,
stacking up to `stack_size`, matched by `item_id`.

## StashEntry / StashData (part_10_hub_save)

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
