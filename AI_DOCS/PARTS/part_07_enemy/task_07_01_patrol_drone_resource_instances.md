# Task 07-01 — Create Patrol Drone data instances

## ID
TASK-07-01

## Allowed Files
- Game/resources/loot_tables/patrol_drone_loot.tres
- Game/resources/enemies/patrol_drone.tres

## Requirements
1. `patrol_drone_loot.tres` (LootTable): one LootEntry referencing
   `basic_ammo.tres`, `min_amount = 5`, `max_amount = 15`,
   `drop_chance = 0.8`.
2. `patrol_drone.tres` (EnemyData): `enemy_id = &"patrol_drone"`,
   `display_name = "Patrol Drone"`, `max_health = 20.0`,
   `move_speed = 70.0`, `detection_radius = 140.0`, `attack_damage = 5.0`,
   `attack_cooldown = 1.2`, `loot_table = patrol_drone_loot.tres`.

## Acceptance Criteria
- [ ] Both resources load without error, match values exactly.

## Test Procedure
1. Open both `.tres` files, confirm values.

## Required Report Format
Implementation Mode.
