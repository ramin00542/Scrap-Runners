---
name: godot-debug
description: Diagnose and fix common Godot 4.x GDScript errors using a systematic workflow. Covers autoload conflicts, _ready() ordering, nil access, and Godot-specific pitfalls.
---

# Godot Error Diagnosis Skill

Systematic workflow for diagnosing and fixing Godot 4.x GDScript errors. Use whenever a Godot error message appears that requires code-level investigation.

## When to Use

- Godot parser errors (e.g., "X hides autoload singleton")
- Runtime nil/null reference errors on node access
- `_ready()` ordering issues (child runs before parent)
- Signal connection errors
- Scene tree / node path issues
- Any error requiring code tracing across multiple scripts

## Diagnosis Workflow

### Step 1 — Capture the Error
```
1. Copy the EXACT error message from Godot's Output/Debugger panel.
2. Note the script file and line number.
3. Check if the error is parser-time (red) or runtime (yellow/orange).
```

### Step 2 — Read the Failing Script
```
1. Read the file referenced in the error.
2. Identify the line causing the error.
3. Trace what variable/node/reference is null or wrong.
```

### Step 3 — Trace Root Cause
```
1. Where was the variable assigned?
2. Is it obtained via $NodePath, get_node(), or @onready?
3. Does the node exist in the scene tree at that point?
4. Is there an autoload or class_name conflict?
```

### Step 4 — Apply Fix
```
1. Apply the minimal fix (see Known Patterns below).
2. Do NOT refactor unrelated code.
3. Do NOT add new features as part of the fix.
```

### Step 5 — Verify
```
1. Confirm the fix addresses the specific error.
2. Check no new errors appear in the Output panel.
3. If Godot CLI is available: run headless scene check.
```

## Known Godot 4.x Patterns

### Pattern A: Autoload + class_name Conflict
**Error**: `Parser Error: "X" hides an autoload singleton of the same name`
**Cause**: A script declares `class_name X` AND is registered as an autoload named `X` in `project.godot`. Godot sees a naming collision.
**Fix**:
1. Remove `class_name X` from the script.
2. Remove `static var instance` singleton pattern (autoload IS the singleton).
3. Reference the autoload directly by name (e.g., `AudioManager.play_sfx()`).
**Rule**: Autoload scripts must NOT declare `class_name`. See ADR-009.

### Pattern B: _ready() Execution Order
**Error**: `Invalid call. Nonexistent function 'X' in base 'Nil'` or `Cannot access property 'X' on a null instance`
**Cause**: Child's `_ready()` runs BEFORE parent's `_ready()`. If a parent adds nodes dynamically in `_ready()`, children will see null.
**Fix**:
1. Null-check node references obtained in `_ready()` if the node is added by a parent.
2. Use `await ready` or `await get_tree().process_frame` for deferred access.
3. Prefer `@onready var node = $Path` over runtime `$Path` in `_ready()`.
**Reference**: This was the root cause of the `global_position` Nil error in Ember Wisp (ses_098b39808ffe).

### Pattern C: Nil Reference on Scene Tree Nodes
**Error**: `Invalid get index 'X' (on base: 'Nil')` or `Cannot call method 'X' on a null instance`
**Cause**: Accessing a node that doesn't exist in the scene tree at the time of access.
**Checklist**:
1. Is the node path correct? (Check spelling, case, nesting.)
2. Is the node in the scene tree when accessed? (e.g., not queue_free'd)
3. Is the node added by code? (If so, check timing — see Pattern B.)
4. Is the node conditional? (Only exists if some flag is set.)

### Pattern D: Input Action Mismatch
**Error**: Input not responding or wrong action triggered.
**Cause**: Input action name in code doesn't match `project.godot` Input Map.
**Fix**:
1. Check exact action name in `project.godot` → `[input]` section.
2. Compare with `Input.is_action_just_pressed("action_name")` in code.
3. Names are case-sensitive and must match character-for-character.

### Pattern E: Signal Connection Errors
**Error**: `Cannot emit signal 'X' - signal not connected` or double-connection.
**Checklist**:
1. Is the signal emitted before the receiver is ready?
2. Is the signal connected in `_ready()` or via editor?
3. Is there a duplicate connection? (Check `get_signal_connection_list()`)

### Pattern F: Collision Layer/Mask Conflicts
**Error**: Entities colliding when they shouldn't, or not colliding when they should.
**Cause**: Incorrect `collision_layer` or `collision_mask` bitmask values.
**Fix**:
1. Check `project.godot` → `[layer_names]` for assigned names.
2. `collision_layer` = what this entity IS (physics layer bitmask).
3. `collision_mask` = what this entity SCANS FOR (can be multiple bits).
4. See ADR-014 for this project's locked layer numbering.

## Post-Fix Reporting

After fixing a Godot error, report:
1. **Error**: exact error message
2. **Root cause**: what was actually wrong
3. **Fix**: what changed (minimal diff)
4. **Verification**: how the fix was confirmed
5. **Prevention**: any related task files that should get a note

## Notes

- Do NOT modify files outside the failing script unless the root cause requires it.
- If the error involves an autoload, check `project.godot` `[autoload]` section.
- If the error is scene-related, check both the `.tscn` file AND the attached script.
- For Godot 4.7+ specific: check if the API was added/changed in this version.
