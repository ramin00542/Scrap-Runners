# Task 09-04 — Extraction Point zone, with progress signal

## ID
TASK-09-04

## Allowed Files
- Game/scripts/systems/extraction_point.gd
- Game/scenes/levels/extraction_point.tscn
- Game/scenes/levels/test_level.tscn

## Requirements

**Note on Testing Discipline:** ExtractionPoint logic is non-trivial but its signals are verified via live progress bar and integration tests in TASK-09-05, TASK-09-07.
Per 01_RULES.md section 10, this task allows verification via temporary prints inside Allowed File `extraction_point.gd` (not external debug script) plus manual procedure.
The committed extraction integration test is in TASK-09-07 (`extraction_integration_test.tscn`). This note satisfies rule 10's explicit exemption clause.

1. `extraction_point.gd`:
```gdscript
class_name ExtractionPoint
extends Area2D

signal extraction_started(duration: float)
signal extraction_progressed(progress: float)
signal extraction_cancelled()
signal extraction_completed()

@export var extraction_duration: float = 8.0

var _timer: float = 0.0
var _in_progress: bool = false

func _ready() -> void:
    collision_layer = 64  # extraction_zone, per ADR-014
    collision_mask = 2   # player, per ADR-014
    body_entered.connect(_on_body_entered)
    body_exited.connect(_on_body_exited)

func _process(delta: float) -> void:
    if not _in_progress:
        return
    _timer -= delta
    var progress: float = 1.0 - (_timer / extraction_duration)
    extraction_progressed.emit(clamp(progress, 0.0, 1.0))
    if _timer <= 0.0:
        _complete_extraction()

func _on_body_entered(body: Node2D) -> void:
    if not body.is_in_group("player"):
        return
    _start_extraction()

func _on_body_exited(body: Node2D) -> void:
    if not body.is_in_group("player"):
        return
    if _in_progress:
        _cancel_extraction()

func _start_extraction() -> void:
    _in_progress = true
    _timer = extraction_duration
    extraction_started.emit(extraction_duration)

func _cancel_extraction() -> void:
    _in_progress = false
    _timer = 0.0
    extraction_cancelled.emit()

func _complete_extraction() -> void:
    _in_progress = false
    extraction_completed.emit()
```
2. `extraction_point.tscn`: root `Area2D`, `CollisionShape2D` (circle
   radius 32), `Polygon2D` (filled cyan circle, 16-sided approximation
   of 64x64, color #00BCD4, with `modulate.a = 0.3` for a
   semi-transparent zone look). NOTE: `Polygon2D` fills only and cannot
   stroke an outline; the translucent fill is the intended placeholder.
3. Add one instance to `test_level.tscn`, away from PlayerSpawn.

## Acceptance Criteria
- [ ] Entering starts countdown, emits `extraction_started`.
- [ ] Leaving early cancels, emits `extraction_cancelled`.
- [ ] Full duration emits `extraction_completed` exactly once.
- [ ] Layer/mask exactly match ADR-014's "Extraction zone" row.

## Test Procedure
1. Run `test_level.tscn`, walk into zone, wait/leave, confirm each signal
   via temporary print statements inside `extraction_point.gd` itself
   (an Allowed File, not a throwaway external script) — remove any debug
   prints not part of Requirements before finishing.

## Required Report Format
Implementation Mode.
