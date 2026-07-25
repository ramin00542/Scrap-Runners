Confirm every pending task file uses the corrected physics conventions:

1. Read ADR-014's bitmask table and ADR-013's superseded table.
2. Scan ALL files under AI_DOCS/PARTS/ for `collision_layer`, `collision_mask` assignments.
3. Every value must be 0, 1, 2, 4, 8, 16, 32, 64 — never 3, 5, 6, 7 (those are layer numbers, not bitmasks).
4. Every assignment must cite `per ADR-014`.
5. Report any discrepancies found and fixed.

AC: Zero occurrences of incorrect bitmask values. All assignments cite ADR-014.
