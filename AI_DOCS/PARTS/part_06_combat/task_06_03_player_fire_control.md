# Task 06-03 — Player fire control with permanent weapon assignment

## ID
TASK-06-03

## Objective
Let the player fire the permanently-equipped Scrap Pistol toward the
mouse cursor, respecting fire rate. This establishes the `_process()`
composition pattern (`_process_xxx()` methods called from one central
`_process()`) that all later tasks touching `player_controller.gd` must
follow — never replace `_process()` wholesale.

## Prerequisites
- TASK-06-02 completed

## Allowed Files
- Game/scripts/entities/player_controller.gd

## Requirements

**Note on Testing Discipline (01_RULES.md section 10):** This task introduces non-trivial fire control logic.
Per rule 10, a committed test would normally be required in the SAME task. However, hit detection and projectile
lifetime require a dummy target and full projectile scene which are created in TASK-06-04. Therefore this task is
explicitly exempted from same-task committed test under rule 10's "unless the task file explicitly says otherwise"
clause. Verification is via manual Test Procedure here, and via committed test `combat_test.tscn` in TASK-06-04 and
`combat_test.gd` ammo test in TASK-06-05. This exemption is documented here to satisfy rule 10.

```gdscript
@export var equipped_weapon: WeaponData = preload("res://resources/weapons/scrap_pistol.tres")

var _fire_cooldown: float = 0.0

func _process(delta: float) -> void:
    _process_combat(delta)

func _process_combat(delta: float) -> void:
    if _fire_cooldown > 0.0:
        _fire_cooldown -= delta
    if Input.is_action_pressed("attack") and _fire_cooldown <= 0.0 and equipped_weapon != null:
        _fire()

func _fire() -> void:
    if equipped_weapon.projectile_scene == null:
        return
    _fire_cooldown = equipped_weapon.fire_rate

    var projectile: Projectile = equipped_weapon.projectile_scene.instantiate()
    get_tree().current_scene.add_child(projectile)
    projectile.global_position = global_position
    projectile.damage = equipped_weapon.damage
    projectile.direction = (get_global_mouse_position() - global_position).normalized()
```

**Convention established here (binding for all future tasks touching
this file):** any new per-frame logic is added as a NEW `_process_xxx(delta)`
method with ONE new line added inside `_process()` calling it — the
existing `_process()` body is never replaced wholesale.

## Acceptance Criteria
- [ ] Every new Player instance fires `scrap_pistol.tres` projectiles by
      default without manual per-scene Inspector setup.
- [ ] Fire rate is respected (0.35s minimum between shots).

## Test Procedure
1. Run `player.tscn` (F6), hold left mouse button, confirm projectiles
   fire toward the cursor at the correct rate.

## Required Report Format
Implementation Mode.
