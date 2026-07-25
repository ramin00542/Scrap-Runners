Run a documentation migration check:

1. Search the ENTIRE `AI_DOCS/` tree (including `AI_DOCS/PARTS/**/*.md`) for stale Part/Task number references that do not match the current 12-Part structure (ADR-008).
2. List every file that needs correction.
3. Fix each stale reference to match ADR-008's numbering.
4. Re-search after edits to confirm zero stale references remain.

AC: Zero remaining stale Part/Task references across all AI_DOCS files.
