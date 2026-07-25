# Task 10-03 — SaveManager autoload, with injectable path and committed tests

## ID
TASK-10-03

## Objective
Implement save/load with an injectable path parameter, so tests never
touch the real save file.

## Prerequisites
- TASK-10-02 completed

## Allowed Files
- Game/scripts/autoload/save_manager.gd
- Game/project.godot
- Game/tests/save_manager_test.gd
- Game/tests/save_manager_test.tscn

## Requirements

### 1. `save_manager.gd` (no `class_name`, per ADR-009)
```gdscript
extends Node

const SAVE_PATH: String = "user://stash_save.tres"

func save_stash(stash: StashData, path: String = SAVE_PATH) -> Error:
    var result: Error = ResourceSaver.save(stash, path)
    if result != OK:
        push_error("Failed to save stash: error code %d" % result)
    return result

func load_stash(path: String = SAVE_PATH) -> StashData:
    if not FileAccess.file_exists(path):
        return StashData.new()
    var loaded: Resource = ResourceLoader.load(path, "StashData", ResourceLoader.CACHE_MODE_IGNORE)
    if loaded == null or not (loaded is StashData):
        push_warning("Save file corrupted or invalid, starting with empty stash.")
        return StashData.new()
    return loaded as StashData
```

### 2. Committed test (uses a SEPARATE path, never touches the real save)
```gdscript
extends Node

const TEST_PATH: String = "user://stash_save_test.tres"

func _ready() -> void:
    var stash := StashData.new()
    stash.add_amount(&"basic_ammo", 25)
    SaveManager.save_stash(stash, TEST_PATH)
    var loaded: StashData = SaveManager.load_stash(TEST_PATH)
    print("PASS: round-trip" if loaded.entries.size() == 1 and loaded.entries[0].amount == 25 else "FAIL")

    var file := FileAccess.open(TEST_PATH, FileAccess.WRITE)
    file.store_string("not a valid resource")
    file.close()
    var corrupted: StashData = SaveManager.load_stash(TEST_PATH)
    print("PASS: corrupted file returns empty stash" if corrupted.entries.is_empty() else "FAIL")

    DirAccess.remove_absolute(ProjectSettings.globalize_path(TEST_PATH))
    var missing: StashData = SaveManager.load_stash(TEST_PATH)
    print("PASS: missing file returns empty stash" if missing.entries.is_empty() else "FAIL")
```

## Acceptance Criteria
- [ ] All 3 PASS lines print.
- [ ] The real `user://stash_save.tres` is never touched by this test.

## Test Procedure
1. Run `save_manager_test.tscn` (F6), confirm 3 PASS lines.

## Required Report Format
Implementation Mode.
