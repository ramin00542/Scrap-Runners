Confirm every pending task file uses the corrected physics conventions:

1. Read ADR-014's bitmask table and ADR-013's superseded table.
2. Scan ALL files under AI_DOCS/PARTS/ for `collision_layer` and `collision_mask` assignments.
3. `collision_layer` values must be single bitmasks: 0, 1, 2, 4, 8, 16, 32, 64 — never 3, 5, 6, 7 (those are layer numbers, not bitmasks).
4. `collision_mask` values MAY combine multiple layers (e.g. 3 = world 1 + player 2, 5 = world 1 + enemies 4) — these are valid.
5. Every assignment must cite `per ADR-014`.
6. Report any discrepancies found and fixed.

AC: Zero occurrences of incorrect bitmask values on collision_layer. collision_mask values correctly reflect combined layers per ADR-014's entity table. All assignments cite ADR-014.
