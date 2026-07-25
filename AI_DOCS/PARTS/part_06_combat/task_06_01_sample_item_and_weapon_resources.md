# Task 06-01 — Create the first real .tres data instances

## ID
TASK-06-01

## Prerequisites
- part_04_core_data completed and verified

## Allowed Files
- Game/resources/items/basic_ammo.tres
- Game/resources/weapons/scrap_pistol.tres

## Requirements
1. `basic_ammo.tres` (ItemData): `item_id = &"basic_ammo"`,
   `display_name = "Scrap Ammo"`, `stack_size = 60`, `item_type = MATERIAL`.
2. `scrap_pistol.tres` (WeaponData): `item_id = &"scrap_pistol"`,
   `display_name = "Scrap Pistol"`, `stack_size = 1`, `item_type = WEAPON`,
   `damage = 8.0`, `fire_rate = 0.35`, `ammo_item_id = &"basic_ammo"`,
   `magazine_size = 10`, `reload_time = 1.2`. `projectile_scene` left
   unassigned (set in task_06_02).

## Acceptance Criteria
- [ ] Both `.tres` files load without error, values match exactly.
- [ ] `scrap_pistol.tres`'s `ammo_item_id` matches `basic_ammo.tres`'s
      `item_id` exactly.

## Test Procedure
1. Open both `.tres` files, confirm field values.

## Required Report Format
Implementation Mode.
