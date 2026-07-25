# Task 03-01 — Create HealthComponent with core logic and committed tests

## ID
TASK-03-01

## Objective
Create a reusable `HealthComponent` Node for any entity, with an explicit
`initialize()` method to avoid a child-before-parent `_ready()` ordering
bug when a parent (e.g. Enemy) needs to set `max_health` from external
data.

## Prerequisites
- part_02_player_movement fully completed

## Allowed Files
- Game/scripts/components/health_component.gd
- Game/tests/health_component_test.gd
- Game/tests/health_component_test.tscn

## Requirements

### 1. `health_component.gd`
```gdscript
class_name HealthComponent
extends Node

signal health_changed(current_health: float, maximum_health: float)
signal died()

@export var max_health: float = 30.0

var current_health: float

func _ready() -> void:
    current_health = max_health

## Call this explicitly from a parent's _ready() to set max_health
## dynamically (e.g. from an EnemyData resource) BEFORE relying on
## current_health. Godot runs a child's _ready() BEFORE its parent's, so
## simply setting @export max_health in the parent's _ready() would be
## too late without this explicit call.
func initialize(maximum_health: float) -> void:
    max_health = max(maximum_health, 1.0)
    current_health = max_health

func take_damage(amount: float) -> void:
    if amount <= 0.0 or current_health <= 0.0:
        return
    var previous: float = current_health
    current_health = clamp(current_health - amount, 0.0, max_health)
    if current_health != previous:
        health_changed.emit(current_health, max_health)
    if current_health <= 0.0 and previous > 0.0:
        died.emit()

func heal(amount: float) -> void:
    if amount <= 0.0 or current_health <= 0.0:
        return
    var previous: float = current_health
    current_health = clamp(current_health + amount, 0.0, max_health)
    if current_health != previous:
        health_changed.emit(current_health, max_health)

func is_dead() -> bool:
    return current_health <= 0.0
```

### 2. Committed test
`health_component_test.tscn`: root `Node` with a child `HealthComponent`
(`max_health = 30`) named `TestHealth`, script `health_component_test.gd`:

```gdscript
extends Node

@onready var health: HealthComponent = $TestHealth

var _test_count: int = 0
var _fail_count: int = 0
var _changed_count: int = 0
var _died_count: int = 0

func _ready() -> void:
    _run_all_tests()
    print("---")
    print("%d/%d tests passed" % [_test_count - _fail_count, _test_count])
    print("ALL PASS" if _fail_count == 0 else "FAILURES DETECTED")

func _check(label: String, condition: bool) -> void:
    _test_count += 1
    if condition:
        print("PASS: %s" % label)
    else:
        _fail_count += 1
        print("FAIL: %s" % label)

func _on_health_changed(_current_health: float, _maximum_health: float) -> void:
    _changed_count += 1

func _on_died() -> void:
    _died_count += 1

func _run_all_tests() -> void:
    _check("starts at max_health", health.current_health == 30.0)

    health.health_changed.connect(_on_health_changed)
    health.died.connect(_on_died)

    health.take_damage(10.0)
    _check("take_damage reduces health", health.current_health == 20.0)
    _check("health_changed fired once", _changed_count == 1)

    health.take_damage(0.0)
    _check("take_damage(0) is a no-op", health.current_health == 20.0)
    _check("no signal for no-op damage", _changed_count == 1)

    health.take_damage(-5.0)
    _check("negative damage is a no-op", health.current_health == 20.0)

    health.heal(5.0)
    _check("heal increases health", health.current_health == 25.0)
    _check("health_changed fired again for heal", _changed_count == 2)

    health.heal(100.0)
    _check("heal clamps to max_health", health.current_health == 30.0)

    health.take_damage(1000.0)
    _check("damage clamps to 0, not negative", health.current_health == 0.0)
    _check("died fired exactly once", _died_count == 1)
    _check("is_dead reports true", health.is_dead())

    health.take_damage(10.0)
    _check("damage after death is ignored", health.current_health == 0.0)
    _check("died did not fire a second time", _died_count == 1)

    health.heal(10.0)
    _check("heal after death is ignored (MVP rule)", health.current_health == 0.0)

    # initialize() ordering fix
    health.initialize(50.0)
    _check("initialize sets max_health", health.max_health == 50.0)
    _check("initialize sets current_health to new max", health.current_health == 50.0)
```

## Acceptance Criteria
- [ ] `health_component_test.tscn` (F6) prints "ALL PASS".
- [ ] `died()` fires exactly once, never repeats.
- [ ] Health never exceeds `max_health` or drops below 0.
- [ ] `initialize()` correctly resets both fields.

## Test Procedure
1. Run `health_component_test.tscn` (F6), confirm "ALL PASS".

## Required Report Format
Implementation Mode.
