Create the project skeleton for Scrap Runners:

1. Create `Game/` with folder structure per 05_TECH_SPEC.md (scenes/, scripts/, resources/, assets/, tests/).
2. Create minimal `project.godot` with:
   - Window: 640x360, canvas_items stretch, keep aspect, nearest filter
   - 8 input actions: move_up/down/left/right (WASD + arrows), attack (LMB), interact (E), toggle_inventory (Tab), reload (R)
   - 7 physics layer names: world, player, enemies, player_projectiles, enemy_projectiles, pickups, extraction_zone
3. Create stub files for each folder (empty `.gitkeep` files).
4. Create `Game/scenes/main/main.tscn` — root Node2D named `Main`, with a ColorRect sized 640x360, solid dark color.
5. Set `run/main_scene` to `res://scenes/main/main.tscn`.
6. Do NOT create autoloads, player scripts, inventory, combat, enemy, or UI logic.

AC: Project opens in editor with zero errors. Input map has exactly 8 custom actions. Physics layers named correctly in Project Settings. `main.tscn` renders a solid dark background.
