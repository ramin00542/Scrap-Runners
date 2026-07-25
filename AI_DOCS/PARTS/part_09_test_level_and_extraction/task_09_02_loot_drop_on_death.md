# Task 09-02 — Loot drop on enemy death, with correct initialization order (CORRECTED)

## ID
TASK-09-02

## Objective
Spawn loot on enemy death, setting `LootPickup`'s properties BEFORE
`add_child()` (not after), and never overwriting a placeholder with a null texture.
Use Polygon2D placeholder (no PNG) for reliability.

## Prerequisites
- TASK-09-01 completed

## Allowed Files
- Game/scripts/systems/test_level.gd
- Game/scenes/items/loot_pickup.tscn
- Game/scripts/systems/loot_pickup.gd

## Requirements
1. `loot_pickup.gd` (type-safe, no Sprite2D texture overwrite issue):
```gdscript
class_name LootPickup
extends Area2D

var item: ItemData
var amount: int = 1

@onready var polygon: Polygon2D = $Polygon2D

var _player_in_range: bool = false
var _player_inventory: Inventory = null

func _ready() -> void:
    collision_layer = 32  # pickups, per ADR-014
    collision_mask = 2   # player, per ADR-014
    body_entered.connect(_on_body_entered)
    body_exited.connect(_on_body_exited)
    # If item has icon, we could use it, but keep Polygon2D visible as fallback
    # No null texture overwrite — Polygon2D always visible
    # Optionally modulate color based on item type, but keep simple for MVP

func _process(_delta: float) -> void:
    if _player_in_range and Input.is_action_just_pressed("interact"):
        if _player_inventory != null:
            var leftover: int = _player_inventory.add_item(item, amount)
            if leftover == 0:
                queue_free()
            else:
                amount = leftover

func _on_body_entered(body: Node2D) -> void:
    if not body.is_in_group("player"):
        return
    if not body is PlayerController:
        return
    var player: PlayerController = body as PlayerController
    _player_in_range = true
    _player_inventory = player.inventory

func _on_body_exited(body: Node2D) -> void:
    if not body.is_in_group("player"):
        return
    if not body is PlayerController:
        return
    _player_in_range = false
    _player_inventory = null
```
2. `loot_pickup.tscn`: root `Area2D` named `LootPickup`, script above,
   child `CollisionShape2D` (circle radius 8), child `Polygon2D` named `Polygon2D`:
   diamond shape (4 vertices, yellow #FFEB3B, 16x16), no external texture —
   per 04_ART_DIRECTION.md, Polygon2D does NOT require asset license entry.
3. In `test_level.gd` — set properties BEFORE `add_child()`:
```gdscript
extends Node2D

const LOOT_PICKUP_SCENE: PackedScene = preload("res://scenes/items/loot_pickup.tscn")

func _ready() -> void:
    for enemy in $Enemies.get_children():
        if enemy.has_signal("enemy_died"):
            enemy.enemy_died.connect(_on_enemy_died.bind(enemy))

func _on_enemy_died(_enemy_id: StringName, enemy_node: EnemyController) -> void:
    if enemy_node.enemy_data == null or enemy_node.enemy_data.loot_table == null:
        return
    var spawn_position: Vector2 = enemy_node.global_position
    for entry in enemy_node.enemy_data.loot_table.entries:
        if randf() <= entry.drop_chance:
            var amount: int = randi_range(entry.min_amount, entry.max_amount)
            var pickup: LootPickup = LOOT_PICKUP_SCENE.instantiate()
            pickup.item = entry.item
            pickup.amount = amount
            add_child(pickup)
            pickup.global_position = spawn_position
```

## Acceptance Criteria
- [ ] `pickup.item` is non-null inside `_ready()`.
- [ ] Killing the enemy spawns loot respecting drop_chance/min/max amount.
- [ ] Layer/mask exactly match ADR-014's "Pickup" row (32 / 2).
- [ ] Visual is Polygon2D diamond, no PNG required.

## Test Procedure
1. Run `test_level.tscn`, kill the enemy repeatedly, confirm loot spawns
   statistically per `drop_chance = 0.8`.

## Required Report Format
Implementation Mode.

## Note
Previous version required PNG + sprite texture logic that could null-overwrite.
Corrected to Polygon2D + type-safe inventory access per issue #5.
