# Task 02-01 — Create the Player scene shell (CORRECTED - no PNG)

## ID
TASK-02-01

## Objective
Create the Player scene with body, collision (per ADR-014), and a
placeholder visual using Polygon2D (no binary PNG required, to avoid AI text-model image generation issues) — no movement logic yet.

## Prerequisites
- part_01_foundation fully completed (through TASK-01-04)

## Allowed Files
- Game/scenes/entities/player/player.tscn
- Game/scripts/entities/player_controller.gd

## Requirements
1. Create `player.tscn`:
   - Root: `CharacterBody2D`, named `Player`
   - `collision_layer = 2` (player), `collision_mask = 1` (world) —
     per ADR-014's entity table
   - Child `CollisionShape2D`, `CircleShape2D` radius 16
   - Child `Polygon2D` named `PlaceholderVisual`: 12-sided approximation of a 32x32 green circle (Color = #4CAF50), no external texture file needed — per 04_ART_DIRECTION.md placeholder rules, Polygon2D visuals do NOT require an asset license entry.
   - Add root to group `"player"`
2. Create `player_controller.gd`:
```gdscript
class_name PlayerController
extends CharacterBody2D
```
   (empty — logic added in task_02_02)

## Acceptance Criteria
- [ ] Scene opens with zero errors.
- [ ] Player is in the `"player"` group.
- [ ] collision_layer = 2, collision_mask = 1, per ADR-014.
- [ ] Placeholder visual is Polygon2D, not an external PNG — no asset license needed.

## Test Procedure
1. Open `player.tscn`, confirm node structure and collision values.
2. Check Node > Groups for `"player"`.
3. Confirm child is Polygon2D with green color, not Sprite2D with missing texture.

## Required Report Format
Implementation Mode.

## Note on previous version
Previous version required `player_placeholder.png` + asset license log.
That approach is unreliable for text-only AI models. This corrected version uses
Polygon2D as recommended in 04_ART_DIRECTION.md and avoids binary file generation.
