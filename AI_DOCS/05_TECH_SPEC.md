# Technical Specification

## Engine Version (LOCKED — set by TASK-01-00, do not edit outside that task)

- Engine version string: `v4.7.1.stable.official [a13da4feb]`
  (must be the exact string reported by `godot --version` or
  Help > About Godot, e.g. `4.3.0.stable.official.77dcf97d8`, copied
  verbatim — character for character identical to ADR-002)
- All code must be compatible with this exact version.
- Do not use APIs introduced in a later version.
- `project.godot` itself only stores a compatibility level (e.g.
  `config/features=PackedStringArray("4.3")`), not this full string —
  the full string lives here and in ADR-002 as the documented source of
  truth. "Targeting" a version means: developed and validated by
  actually running the project in that exact editor build.
- Engine upgrades require a dedicated task and a new ADR.

## Path Convention (LOCKED)

- Repository root: `GameProject/`
- Godot project root: `GameProject/Game/` (contains `project.godot`)
- `res://` always resolves to `GameProject/Game/`
- All in-engine references use `res://...`
- All chat/report references use the repo-relative path
- Never mix the two conventions in the same file or response.

## Naming Convention (LOCKED)

- File names: `snake_case` — `main.tscn`, `player_controller.gd`
- Node names / class names: `PascalCase` — root node of `main.tscn` is `Main`
- Signals: `snake_case`, past tense — `health_changed`, `item_added`
- Identifiers (item/enemy/weapon ids): `StringName`, `snake_case`
- Autoload scripts do NOT declare `class_name` (see ADR-009)

## Display and Camera (LOCKED — ADR-004)

- Base resolution: 640 x 360
- Stretch Mode: `canvas_items`
- Stretch Aspect: `keep`
- Texture Filter: Nearest (no smoothing)
- Character sprite size: 32x32
- Camera2D zoom: 1.0 at base resolution

## Physics

- Player and enemies use `CharacterBody2D`
- Projectiles use `Area2D` (not physics-simulated bodies) unless a task
  explicitly specifies otherwise

## Input Map (LOCKED — created in TASK-01-01, per ADR-010)

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

Exactly these 8 project-defined custom actions exist, in addition to
Godot's built-in `ui_*` actions.

## Physics Layers (LOCKED — created in TASK-01-01, per ADR-013, CORRECTED by ADR-014)

| Layer # (Inspector) | Name | Bitmask Value (what Godot stores) |
|---|---:|---:|
| 1 | world | 1 (1 << 0) |
| 2 | player | 2 (1 << 1) |
| 3 | enemies | 4 (1 << 2) |
| 4 | player_projectiles | 8 (1 << 3) |
| 5 | enemy_projectiles | 16 (1 << 4) |
| 6 | pickups | 32 (1 << 5) |
| 7 | extraction_zone | 64 (1 << 6) |

Named directly in `project.godot`'s `layer_names/2d_physics/*` settings.
In the editor, check the Layer # checkbox; Godot stores the bitmask.
In code, ALWAYS use the bitmask values below, not the Layer # numbers.

### Standard layer/mask assignment per entity type (LOCKED — ADR-014, supersedes ADR-013)

| Entity | collision_layer (bitmask) | collision_mask (bitmask) |
|---|---|---|
| World geometry (walls) | 1 (world) | 0 |
| Player | 2 (player) | 1 (world) |
| Enemy | 4 (enemies) | 1 (world) |
| Player projectile | 8 (player_projectiles) | 5 = 1 (world) + 4 (enemies) |
| Enemy projectile | 16 (enemy_projectiles) | 3 = 1 (world) + 2 (player) |
| Pickup | 32 (pickups) | 2 (player) |
| Extraction zone | 64 (extraction_zone) | 2 (player) |

Bitmask formula: value = 1 << (Layer# - 1). So Layer# 3 = 4, not 3.

Every `CollisionObject2D`/`Area2D` in the project sets its layer/mask
exactly per this corrected table, with a code comment citing
"per ADR-014" — never a bare, unexplained integer (01_RULES.md section 9).
ADR-013 is kept for audit but Status is Superseded by ADR-014.

## Autoload Policy

No autoload is created speculatively. Each singleton is introduced only
in the task that gives it its first real responsibility AND, wherever
feasible, its first real usage in the same task.

- `GameManager` — scene transitions (part_10_hub_save)
- `SaveManager` — persisting Stash to disk (part_10_hub_save)
- `ItemDatabase` — item_id lookup for save/load reconstruction (part_10_hub_save)

`Inventory` is NOT an autoload — it is a `Node` component owned by the
entity that has one. `Stash` is NOT an autoload — it is data owned by
`SaveManager` / the Hub scene.

## Signal Naming Reference

```
signal health_changed(current_health: float, maximum_health: float)
signal died()
signal item_added(item_id: StringName, amount: int)
signal item_removed(item_id: StringName, amount: int)
signal inventory_changed()
signal enemy_died(enemy_id: StringName)
signal extraction_started(duration: float)
signal extraction_progressed(progress: float)
signal extraction_cancelled()
signal extraction_completed()
```

## Type Safety (mandatory, see 01_RULES.md section 9)

```gdscript
func add_item(item: ItemData, amount: int = 1) -> int:
    var remaining: int = amount
    return remaining
```

## Project Folder Layout (inside Game/)

```
Game/
├── project.godot
├── scenes/
│   ├── main/
│   ├── entities/
│   │   ├── player/
│   │   └── enemies/
│   ├── levels/
│   ├── items/
│   ├── hub/
│   └── ui/
├── scripts/
│   ├── autoload/
│   ├── components/
│   ├── entities/
│   ├── systems/
│   └── data/
├── resources/
│   ├── items/
│   ├── weapons/
│   ├── enemies/
│   └── loot_tables/
├── assets/
│   ├── sprites/
│   ├── audio/
│   ├── fonts/
│   └── tilesets/
└── tests/
```

## Tileset Note

Tileset assets and node type (`TileMap` vs `TileMapLayer`) are decided in
part_09_test_level_and_extraction, based on the locked engine version.
