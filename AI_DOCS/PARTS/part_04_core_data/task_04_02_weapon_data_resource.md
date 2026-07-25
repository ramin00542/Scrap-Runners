# Task 04-02 — Create the WeaponData resource class

## ID
TASK-04-02

## Prerequisites
- TASK-04-01 completed

## Allowed Files
- Game/scripts/data/weapon_data.gd

## Requirements
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

## Acceptance Criteria
- [ ] File compiles with zero parser errors.
- [ ] "WeaponData" shows inherited + new fields.

## Test Procedure
1. Create New Resource "WeaponData", confirm all fields, discard.

## Required Report Format
Implementation Mode.
