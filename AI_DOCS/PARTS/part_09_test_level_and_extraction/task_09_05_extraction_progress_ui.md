# Task 09-05 — Extraction progress feedback

## ID
TASK-09-05

## Objective
Show a LIVE progress bar during extraction, connected to
`extraction_progressed`.

## Prerequisites
- TASK-09-04 completed

## Allowed Files
- Game/scenes/entities/player/player.tscn
- Game/scripts/entities/player_controller.gd
- Game/scenes/levels/test_level.tscn
- Game/scripts/systems/test_level.gd

## Requirements
1. Add a `ProgressBar` named `ExtractionBar` to Player's `CanvasLayer`,
   `visible = false`, `min_value = 0`, `max_value = 1`.
2. In `test_level.gd`:
```gdscript
@onready var extraction_point: ExtractionPoint = $ExtractionPoint

func _ready() -> void:
    for enemy in $Enemies.get_children():
        if enemy.has_signal("enemy_died"):
            enemy.enemy_died.connect(_on_enemy_died.bind(enemy))
    extraction_point.extraction_started.connect(_on_extraction_started)
    extraction_point.extraction_progressed.connect(_on_extraction_progressed)
    extraction_point.extraction_cancelled.connect(_on_extraction_cancelled)
    extraction_point.extraction_completed.connect(_on_extraction_completed)

func _on_extraction_started(_duration: float) -> void:
    $Player.show_extraction_bar(true)

func _on_extraction_progressed(progress: float) -> void:
    $Player.update_extraction_progress(progress)

func _on_extraction_cancelled() -> void:
    $Player.show_extraction_bar(false)

func _on_extraction_completed() -> void:
    $Player.show_extraction_bar(false)
    print("Extraction complete!")  # replaced by real transition in part_10
```
3. In `player_controller.gd`:
```gdscript
@onready var extraction_bar: ProgressBar = $CanvasLayer/ExtractionBar

func show_extraction_bar(show: bool) -> void:
    extraction_bar.visible = show

func update_extraction_progress(progress: float) -> void:
    extraction_bar.value = progress
```

## Acceptance Criteria
- [ ] Bar becomes visible and fills from 0 to 1 LIVE during extraction.
- [ ] Bar hides on cancel or completion.

## Test Procedure
1. Run `test_level.tscn`, enter zone, watch bar fill in real time; leave
   early, confirm hide; stay full duration, confirm hide + print.

## Required Report Format
Implementation Mode.
