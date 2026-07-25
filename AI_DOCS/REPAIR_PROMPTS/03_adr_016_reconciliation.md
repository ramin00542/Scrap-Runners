# FILE: 03_adr_016_reconciliation.md

## Purpose
Apply ADR-016 (Slight Isometric Tilt) rules to pending Task documentation.

## Required Changes

### TASK-02-01 & TASK-07-02
Document `PlaceholderVisual.position = Vector2(0, -4)`. Ensure `CollisionShape2D` remains centered at `Vector2(0, 0)`.

### TASK-02-04
Explicitly state animation tracks target ONLY `PlaceholderVisual.scale`, NOT `position`. The visual offset must not be animated.

### TASK-09-01
Document an explicit Y-sort enabled parent node (e.g., `Entities`) for Player, Enemy, and LootPickup instances. ADR-016 requires Y-sorting for correct draw order with visual offsets.

### Exclusions
Explicitly state ExtractionPoint, Projectiles, and CanvasLayer UI elements do NOT use the visual offset. They are not "standing entities."

## Forbidden Actions
- Do not modify any Game/ files.
- Do not change code logic — only add documentation requirements.

## Verification
After repair:
1. TASK-02-01 and TASK-07-02 mention `Vector2(0, -4)`.
2. TASK-02-04 states animation targets scale only.
3. TASK-09-01 documents Y-sorting requirement.
4. All exclusion items are explicitly listed.
