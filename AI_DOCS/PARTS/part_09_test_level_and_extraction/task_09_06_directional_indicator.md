# Task 09-06 — Directional extraction indicator (ADR-005), no image asset

## ID
TASK-09-06

## Objective
Add a `Polygon2D`-based arrow (no image asset needed) pointing toward
the Extraction Point, using the `_process_xxx()` composition convention
established in task_06_03 (adds a call, never replaces `_process()`).

## Prerequisites
- TASK-09-05 completed

## Allowed Files
- Game/scenes/entities/player/player.tscn
- Game/scripts/entities/player_controller.gd
- Game/scripts/systems/test_level.gd

## Requirements
1. Add a `Polygon2D` triangle named `ExtractionIndicator` to Player's
   `CanvasLayer`.
2. In `player_controller.gd` (diff — add a call, don't replace `_process`):
```gdscript
@onready var extraction_indicator: Polygon2D = $CanvasLayer/ExtractionIndicator

var extraction_target: Node2D = null

func _process(delta: float) -> void:
    _process_combat_and_reload(delta)
    _update_extraction_indicator()

func _update_extraction_indicator() -> void:
    if extraction_target != null:
        var direction: Vector2 = (extraction_target.global_position - global_position)
        extraction_indicator.rotation = direction.angle()
        extraction_indicator.visible = true
    else:
        extraction_indicator.visible = false
```
3. In `test_level.gd`'s `_ready()`: `$Player.extraction_target = extraction_point`.

## Acceptance Criteria
- [ ] Arrow always points toward the Extraction Point as player moves.
- [ ] NOT a minimap — no map rendering, no fog of war, no other markers
      (per ADR-005).
- [ ] `_process()` still calls `_process_combat_and_reload` correctly —
      combat/reload unaffected by this addition.

## Test Procedure
1. Run `test_level.tscn`, walk around, confirm arrow tracking.
2. Confirm firing/reload still work correctly (regression check).

## Required Report Format
Implementation Mode.
