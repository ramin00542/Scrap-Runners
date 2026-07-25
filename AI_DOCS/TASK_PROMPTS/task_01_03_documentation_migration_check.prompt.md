Run a documentation consistency check:

1. Verify every task file under AI_DOCS/PARTS/ uses the CORRECT physics layer bitmask values from ADR-014 (1,2,4,8,16,32,64), not the ADR-013 layer-number values (1..7).
2. Search for patterns like `collision_layer = 3`, `collision_layer = 5`, etc. — any found outside ADR-013's historical text is a bug.
3. Fix any incorrect values found, update the referencing ADR comment to ADR-014.

AC: `grep -rn "collision_layer = [3,5,6,7]" AI_DOCS/PARTS/` returns zero results outside ADR-013's preserved text.
