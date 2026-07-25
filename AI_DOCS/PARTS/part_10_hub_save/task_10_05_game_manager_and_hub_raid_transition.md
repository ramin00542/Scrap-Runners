# Task 10-05 — GameManager autoload, real Hub↔Raid transition, and Raid Failure flow

## ID
TASK-10-05

## Objective
Create GameManager and use it immediately for: (a) Hub → Raid via a
reliable door, (b) Raid → Hub on successful extraction with Stash
transfer, (c) Raid → Hub on player death with NO Stash transfer, and
(d) set the Hub as the project's actual main scene.

## Prerequisites
- part_09_test_level_and_extraction completed
- TASK-10-00 through TASK-10-04 completed

## Allowed Files
- Game/scripts/autoload/game_manager.gd
- Game/project.godot
- Game/scenes/hub/hub.tscn
- Game/scripts/systems/raid_door.gd
- Game/scenes/levels/test_level.tscn
- Game/scripts/systems/test_level.gd

## Requirements

### 1. `game_manager.gd` (no `class_name`, per ADR-009)
```gdscript
extends Node

func go_to_scene(scene_path: String) -> void:
    get_tree().change_scene_to_file(scene_path)
```

### 2. `raid_door.gd` — reliable interaction via `_process`, never a
single-frame check inside `body_entered`
```gdscript
extends Area2D

var _player_in_range: bool = false

func _ready() -> void:
    collision_layer = 0
    collision_mask = 2  # player, per ADR-014
    body_entered.connect(_on_body_entered)
    body_exited.connect(_on_body_exited)

func _process(_delta: float) -> void:
    if _player_in_range and Input.is_action_just_pressed("interact"):
        GameManager.go_to_scene("res://scenes/levels/test_level.tscn")

func _on_body_entered(body: Node2D) -> void:
    if body.is_in_group("player"):
        _player_in_range = true

func _on_body_exited(body: Node2D) -> void:
    if body.is_in_group("player"):
        _player_in_range = false
```
3. Add this door (as an `Area2D` with a visible `Polygon2D` marker) to
   `hub.tscn`, positioned away from PlayerSpawn.

### 3. Extraction success transfers loot; test_level.gd updated:
```gdscript
# NOTE: this `_ready()` is CUMULATIVE — it must contain EVERY line
# added by TASK-09-02, TASK-09-05 and TASK-09-06, plus the new
# `_on_player_died` connection below. Do NOT drop any earlier line
# (notably the extraction_target assignment from TASK-09-06), or the
# directional indicator / progress bar will silently regress.
func _ready() -> void:
    for enemy in $Enemies.get_children():
        if enemy.has_signal("enemy_died"):
            enemy.enemy_died.connect(_on_enemy_died.bind(enemy))
    extraction_point.extraction_started.connect(_on_extraction_started)
    extraction_point.extraction_progressed.connect(_on_extraction_progressed)
    extraction_point.extraction_cancelled.connect(_on_extraction_cancelled)
    extraction_point.extraction_completed.connect(_on_extraction_completed)
    $Player.extraction_target = extraction_point   # from TASK-09-06 — keep
    $Player.health.died.connect(_on_player_died)

func _on_extraction_completed() -> void:
    $Player.show_extraction_bar(false)
    var stash: StashData = SaveManager.load_stash()
    for slot in $Player.inventory.get_slots():
        if not slot.is_empty():
            stash.add_amount(slot.item.item_id, slot.amount)
    SaveManager.save_stash(stash)
    GameManager.go_to_scene("res://scenes/hub/hub.tscn")

func _on_player_died() -> void:
    # Per ADR-012: raid Inventory is discarded on failure — Stash
    # remains unchanged. Simply return to Hub after a short delay.
    await get_tree().create_timer(1.5).timeout
    GameManager.go_to_scene("res://scenes/hub/hub.tscn")
```

### 4. Set Hub as the real main scene
In `project.godot`, change `run/main_scene` from `main.tscn` to
`res://scenes/hub/hub.tscn`. `main.tscn` remains in the repo as a minimal
smoke-test scene but is no longer the entry point.

## Acceptance Criteria
- [ ] Walking to the door and pressing interact reliably transitions to
      the raid, regardless of whether interact is pressed before or
      after entering the zone (as long as still inside it).
- [ ] Completing extraction transfers raid Inventory contents to Stash,
      then returns to Hub; StashInventory reflects the new items.
- [ ] Dying in the raid returns to Hub after a delay, with Stash
      UNCHANGED (no items added).
- [ ] The Scrap Pistol and its magazine are never transferred to Stash
      (they simply reset on the next raid, per ADR-012).
- [ ] Pressing F5 from a cold start opens directly into the Hub.
- [ ] Repeating the full loop twice accumulates loot correctly in Stash.

## Test Procedure
1. Run the project (F5), confirm it opens into Hub.
2. Walk to the door, interact, confirm transition to raid.
3. Pick up loot, kill enemy, extract — confirm return to Hub with new
   Stash contents.
4. Repeat: enter raid again, let the enemy kill the player — confirm
   return to Hub with Stash UNCHANGED from before this raid.
5. Repeat the full success loop once more, confirm loot accumulates.

## Required Report Format
Implementation Mode.
