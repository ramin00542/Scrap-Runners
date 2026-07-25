Create the project skeleton for Scrap Runners:

1. Create `Game/` with folder structure per 05_TECH_SPEC.md (scenes/, scripts/, resources/, assets/, tests/).
2. Create minimal `project.godot` with:
   - Window: 640x360, canvas_items stretch, keep aspect, nearest filter
   - 8 input actions: move_up/down/left/right (WASD + arrows), attack (LMB), interact (E), toggle_inventory (Tab), reload (R)
   - 7 physics layer names: world, player, enemies, player_projectiles, enemy_projectiles, pickups, extraction_zone
   - Autoload stubs: GameManager, SaveManager, ItemDatabase (empty scripts)
3. Create stub files for each folder (empty .gd.keep files).
4. Do NOT create any scene, resource, or gameplay code.
5. Set `run/main_scene` to `res://scenes/main/main.tscn` (create a minimal empty scene).

AC: Project opens in editor with zero errors. Input map has exactly 8 custom actions. Physics layers named correctly in Project Settings.
