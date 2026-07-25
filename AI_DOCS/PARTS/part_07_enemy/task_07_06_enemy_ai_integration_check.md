# Task 07-06 — Committed Enemy AI test scene and verification

## ID
TASK-07-06

## Allowed Files
- Game/tests/enemy_ai_test.tscn
- Game/tests/enemy_ai_test.gd
- AI_DOCS/CHANGELOG.md

## Requirements
`enemy_ai_test.tscn`: root `Node2D` with a `Player` instance and one
`enemy_patrol_drone.tscn` instance with patrol points wired. Script:

```gdscript
extends Node2D

@onready var enemy: EnemyController = $EnemyPatrolDrone
@onready var player: PlayerController = $Player

func _ready() -> void:
    enemy.enemy_died.connect(_on_enemy_died)

func _on_enemy_died(enemy_id: StringName) -> void:
    print("PASS: enemy_died fired with id: %s" % enemy_id)

func _process(_delta: float) -> void:
    print("Enemy state: %s | Player pos: %s" % [
        EnemyController.State.keys()[enemy._state], player.global_position
    ])
```

## Acceptance Criteria
- [ ] Manually walking the player demonstrates patrol → chase → attack →
      chase → patrol transitions (visible in printed state log).
- [ ] Killing the enemy prints the PASS line with correct id.

## Test Procedure
1. Run `enemy_ai_test.tscn` (F6), move player around, observe states.
2. Fire at the enemy until it dies, confirm PASS line.

## Required Report Format
Implementation Mode, then Changelog Exception once verified.
