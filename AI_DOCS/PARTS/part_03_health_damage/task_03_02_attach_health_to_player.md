# Task 03-02 — Attach HealthComponent to the Player

## ID
TASK-03-02

## Prerequisites
- TASK-03-01 completed and passing

## Allowed Files
- Game/scenes/entities/player/player.tscn
- Game/scripts/entities/player_controller.gd

## Requirements
1. Add child `HealthComponent` to Player, named `HealthComponent`,
   `max_health = 30`.
2. In `player_controller.gd`:
```gdscript
@onready var health: HealthComponent = $HealthComponent
```

## Acceptance Criteria
- [ ] Remote Scene Tree shows `HealthComponent` under Player while running.
- [ ] No movement/animation behavior changed.

## Test Procedure
1. Run, inspect Player in Remote tab, confirm HealthComponent child.

## Required Report Format
Implementation Mode.
