# Task 05-03 — Extend committed inventory tests with edge cases

## ID
TASK-05-03

## Prerequisites
- TASK-05-01, TASK-05-02 completed

## Allowed Files
- Game/tests/inventory_test.gd
- Game/tests/inventory_test.tscn

## Requirements
Add test cases for: exact-capacity boundary, two ItemData instances with
the same item_id stacking together via `add_item`, `remove_item()` on a
never-added item_id, filling completely then checking an unrelated
item_id returns false, `get_slots()` returning a duplicated array.

## Acceptance Criteria
- [ ] All new cases print PASS.
- [ ] `inventory.gd`/`inventory_slot.gd` byte-for-byte unchanged.

## Test Procedure
1. Run inventory_test.tscn (F6), confirm "ALL PASS" with higher count.

## Required Report Format
Implementation Mode. If a real bug is found, switch to Clarification Mode.
