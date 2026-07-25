# Task 11-01 — Health bar HUD

## ID
TASK-11-01

## Allowed Files
- Game/scenes/entities/player/player.tscn
- Game/scripts/entities/player_controller.gd

## Requirements
1. Add `ProgressBar` named `HealthBar` to Player's `CanvasLayer`,
   top-left, `min_value = 0`.
2. In `player_controller.gd`:
```gdscript
@onready var health_bar: ProgressBar = $CanvasLayer/HealthBar

func _ready() -> void:
    # NOTE: this `_ready()` is CUMULATIVE — it must contain EVERY line
    # added by TASK-03-03, TASK-06-05 and TASK-08-02 below, plus the
    # new health_bar lines. Do NOT drop any earlier line, or
    # death-handling / ammo / inventory UI will silently regress.
    health.died.connect(_on_died)
    if equipped_weapon != null:
        _magazine_ammo = equipped_weapon.magazine_size
    inventory_ui.setup(inventory)
    health.health_changed.connect(_on_health_changed)
    health_bar.max_value = health.max_health
    health_bar.value = health.current_health

func _on_health_changed(current: float, maximum: float) -> void:
    health_bar.max_value = maximum
    health_bar.value = current
```

## Acceptance Criteria
- [ ] Health bar reflects current/max health at all times, live.

## Test Procedure
1. Run, deal damage via combat_test.tscn's dummy target setup adapted to
   damage the player (or a committed test scene), confirm bar updates.

## Required Report Format
Implementation Mode.
