# Task 05-02 — Attach the Inventory component to the Player scene

## ID
TASK-05-02

## Prerequisites
- TASK-05-01 completed and passing
- part_02_player_movement completed

## Allowed Files
- Game/scenes/entities/player/player.tscn
- Game/scripts/entities/player_controller.gd

## Requirements
1. Add a child `Inventory` node, named `Inventory`, to Player, `slot_count = 16`.
2. In `player_controller.gd`:
```gdscript
@onready var inventory: Inventory = $Inventory
```

## Acceptance Criteria
- [ ] Remote Scene Tree shows an `Inventory` child under Player.
- [ ] Slot Count = 16.
- [ ] No movement behavior changed.

## Test Procedure
1. Run, inspect in Remote tab, confirm Inventory child and Slot Count.

## Required Report Format
Implementation Mode.
