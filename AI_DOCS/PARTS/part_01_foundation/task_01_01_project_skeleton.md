# Task 01-01 — Create the minimal, runnable Godot project skeleton

## ID
TASK-01-01

## Objective
Produce a Godot project that opens and runs without errors, with correct
project settings, a complete Input Map (8 actions per ADR-010), named
physics layers (7 layers per ADR-014 (ADR-013 superseded by ADR-014)), and the full folder structure.

## Prerequisites
- TASK-01-00 completed (engine version locked)

## Allowed Files
- Game/project.godot
- Game/scenes/main/main.tscn
- Game/.gitignore
- Game/scenes/entities/player/.gitkeep
- Game/scenes/entities/enemies/.gitkeep
- Game/scenes/levels/.gitkeep
- Game/scenes/items/.gitkeep
- Game/scenes/hub/.gitkeep
- Game/scenes/ui/.gitkeep
- Game/scripts/autoload/.gitkeep
- Game/scripts/components/.gitkeep
- Game/scripts/entities/.gitkeep
- Game/scripts/systems/.gitkeep
- Game/scripts/data/.gitkeep
- Game/resources/items/.gitkeep
- Game/resources/weapons/.gitkeep
- Game/resources/enemies/.gitkeep
- Game/resources/loot_tables/.gitkeep
- Game/assets/sprites/.gitkeep
- Game/assets/audio/.gitkeep
- Game/assets/fonts/.gitkeep
- Game/assets/tilesets/.gitkeep
- Game/tests/.gitkeep

## Forbidden in This Task
- No player scene or script
- No inventory, combat, enemy, or UI logic
- No autoloads (ADR-007)
- No external plugins or addons
- No final art

## Requirements
1. Create and validate `Game/project.godot` using the locked engine
   version.
2. Set display settings per 05_TECH_SPEC.md (640x360, canvas_items,
   keep aspect, nearest filtering).
3. Create the Input Map with EXACTLY these 8 bindings (per ADR-010):

   | Action | Binding |
   |---|---|
   | move_up | W (physical), Up Arrow (physical) |
   | move_down | S (physical), Down Arrow (physical) |
   | move_left | A (physical), Left Arrow (physical) |
   | move_right | D (physical), Right Arrow (physical) |
   | attack | Left Mouse Button |
   | interact | E (physical) |
   | toggle_inventory | Tab (physical) |
   | reload | R (physical) |

4. In Project Settings > Layer Names > 2D Physics, name layers 1–7
   exactly (per ADR-014 (ADR-013 superseded by ADR-014)):
   `world`, `player`, `enemies`, `player_projectiles`,
   `enemy_projectiles`, `pickups`, `extraction_zone`.
5. Create `main.tscn`: root `Node2D` named `Main`, `ColorRect` sized
   640x360, solid dark color.
6. Set `main.tscn` as the main scene (temporarily — TASK-10-05 will
   later change this to `hub.tscn`).
7. Create the full folder layout with `.gitkeep` files as listed above.

## Acceptance Criteria
- [ ] Project opens/runs with zero errors.
- [ ] Solid background renders full-viewport.
- [ ] Input Map has exactly 8 custom actions with exact bindings above,
      excluding built-in `ui_*` actions.
- [ ] Layer Names > 2D Physics shows all 7 names correctly, in order.
- [ ] Full folder layout and all `.gitkeep` files exist, confirmed via
      `git status`.

## Test Procedure
1. Run the project, confirm background renders.
2. Project Settings > Input Map: confirm 8 actions and bindings.
3. Project Settings > Layer Names > 2D Physics: confirm 7 names.
4. Run `git status`, confirm all `.gitkeep` files appear.

## Required Report Format
Implementation Mode, then Changelog Exception once verified.
