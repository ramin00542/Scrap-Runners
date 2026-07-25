# Task 07-02 — Enemy scene shell with health, using initialize() (CORRECTED - no PNG)

## ID
TASK-07-02

## Objective
Use `HealthComponent.initialize()` (from task_03_01) to correctly set
Enemy health, avoiding the child-before-parent `_ready()` ordering bug.

## Prerequisites
- TASK-07-01 completed
- part_03_health_damage completed (with `initialize()` method)

## Allowed Files
- Game/scenes/entities/enemies/enemy_patrol_drone.tscn
- Game/scripts/entities/enemy_controller.gd

## Requirements
1. `enemy_controller.gd`:
```gdscript
class_name EnemyController
extends CharacterBody2D

@export var enemy_data: EnemyData

@onready var health: HealthComponent = $HealthComponent

func _ready() -> void:
    if enemy_data != null:
        health.initialize(enemy_data.max_health)
    collision_layer = 4  # enemies, per ADR-014
    collision_mask = 1   # world, per ADR-014
```
2. Scene: root `CharacterBody2D` named `EnemyPatrolDrone`, script above
   with `enemy_data = patrol_drone.tres`; child `CollisionShape2D`
   (circle radius 16); child `Polygon2D` named `PlaceholderVisual`:
   32x32 red square (Color = #E53935, 4 vertices), no external image —
   per 04_ART_DIRECTION.md, Polygon2D does NOT require asset license entry;
   child `HealthComponent`.

## Acceptance Criteria
- [ ] Scene runs standalone with zero errors.
- [ ] `health.max_health == 20.0` AND `health.current_health == 20.0`
      (both, confirming `initialize()` correctly avoided the ordering bug).
- [ ] collision_layer = 4, collision_mask = 1, per ADR-014.
- [ ] Placeholder is Polygon2D, not PNG.

## Test Procedure
1. Run `enemy_patrol_drone.tscn`, check Remote tab, confirm both health
   fields equal 20.0.

## Required Report Format
Implementation Mode.

## Note
Previous version required PNG + license log. Corrected to Polygon2D for reliability.
