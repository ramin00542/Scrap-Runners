# Task 06-02 — Create the generic Projectile scene and script

## ID
TASK-06-02

## Objective
Create a reusable projectile, using `Polygon2D` for its visual (no image
asset needed), with collision layers exactly per ADR-014.

## Prerequisites
- TASK-06-01 completed
- part_03_health_damage completed

## Allowed Files
- Game/scripts/systems/projectile.gd
- Game/scenes/items/projectile_base.tscn
- Game/scenes/items/player_projectile.tscn
- Game/resources/weapons/scrap_pistol.tres

## Requirements

### 1. `projectile.gd`
```gdscript
class_name Projectile
extends Area2D

@export var speed: float = 400.0
@export var damage: float = 10.0
@export var lifetime: float = 2.0

var direction: Vector2 = Vector2.RIGHT

func _ready() -> void:
    body_entered.connect(_on_target_entered)
    area_entered.connect(_on_target_entered)
    get_tree().create_timer(lifetime).timeout.connect(queue_free)

func _physics_process(delta: float) -> void:
    global_position += direction * speed * delta

func _on_target_entered(node: Node) -> void:
    var health: HealthComponent = node.get_node_or_null("HealthComponent") as HealthComponent
    if health != null:
        health.take_damage(damage)
    # Always free on any collision (wall, enemy, etc.), not only when health found.
    # Previously this only freed on health hit, causing projectiles to pass through walls.
    queue_free()
```

### 2. `projectile_base.tscn`
- Root `Area2D` named `Projectile`, script `projectile.gd`
- Child `CollisionShape2D`, `CircleShape2D` radius 4
- Child `Polygon2D`: 8x8 amber square (no image file — no asset license
  entry needed)
- `monitoring = true`, `monitorable = true`

### 3. `player_projectile.tscn` (inherited scene)
Per ADR-014's entity table:
```gdscript
# collision_layer = 8  (player_projectiles, per ADR-014)
# collision_mask = 1 | 4  (world=1, enemies=4, per ADR-014)
```
Set `collision_layer = 8`, `collision_mask = 5` (bitmask: `1 | 4` = world
layer bit + enemies layer bit → in Godot's Inspector, check "world" and
"enemies" checkboxes under Collision Mask).

### 4. Update `scrap_pistol.tres`
`projectile_scene = preload("res://scenes/items/player_projectile.tscn")`

## Acceptance Criteria
- [ ] Both scenes open with zero errors.
- [ ] `player_projectile.tscn` has collision_layer/mask exactly matching
      ADR-014's "Player projectile" row (layer 4; mask = world + enemies).
- [ ] `scrap_pistol.tres`'s `projectile_scene` field is correctly assigned.

## Test Procedure
1. Open both scenes, confirm layer/mask checkboxes in Inspector match
   ADR-014 exactly.
2. Open `scrap_pistol.tres`, confirm Projectile Scene field is set.

## Required Report Format
Implementation Mode.
