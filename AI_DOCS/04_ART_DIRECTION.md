# Art Direction

## Overall Visual Style
2D top-down pixel art. Base resolution and camera setup are defined in
05_TECH_SPEC.md and locked by ADR-004 (640x360 base resolution, nearest-
neighbor filtering, no smoothing).

Color palette: industrial grays and browns for environment, with muted
neon accents (cyan / amber) reserved for interactive elements (loot,
extraction point, UI highlights).

## Player Character — Scavenger Bot (ADR-003)
NOT a duck, NOT humanoid. A small scavenger robot.

Visual traits:
- Round metallic body with visible panel seams
- One or two LED "eyes" whose color communicates state:
  - green = normal / idle
  - red = damaged / low health
  - cyan = scanning / interacting
- A small antenna on top that bobs slightly during movement
- Overall proportions: short and round, silhouette must read clearly at
  32x32 pixels against the environment palette

Alternate identity (not default, requires a new ADR to activate):
Small cat-like alien creatures ("Cosmo-Cats"), same functional traits.

## Enemies
Visually related to the player's robot design but corrupted/hostile:
rust textures, exposed wiring, red-dominant LED indicators.

MVP requires exactly ONE enemy type (see part_07_enemy):
- "Patrol Drone": light, fast, low health, ranged attack

## Environment
Theme: abandoned warehouse / derelict factory interior.
Tileset assets are built in part_09_test_level_and_extraction — do not
build tileset assets before that Part is active. The correct TileMap
node type (`TileMap` vs `TileMapLayer`) depends on the exact engine
version locked in 05_TECH_SPEC.md/ADR-002 — confirm against that version
when part_09 begins.

## Placeholder Asset Rules

Until final art exists, use flat-colored geometric placeholders.
**For MVP and AI text-model reliability, Polygon2D/ColorRect is MANDATORY for all MVP placeholders, not optional:**
- `Polygon2D`/`ColorRect` shapes are preferred and REQUIRED for MVP placeholders,
  since they require no binary file generation and no asset license entry.
  Text-only AI cannot reliably generate valid PNG binaries.
- Player: green circle **Polygon2D** (12-sided, 32x32, #4CAF50)
- Enemy: red square **Polygon2D** (32x32, #E53935)
- Loot item: yellow diamond **Polygon2D** (16x16 diamond, #FFEB3B)
- Extraction point: cyan **translucent filled circle** Polygon2D (64x64,
  #00BCD4, modulate.a ≈ 0.3) — Polygon2D fills only, cannot stroke an
  outline
- Projectiles, indicators, and other small effects: **must** use `Polygon2D`
  shapes, no image files.

**Previous tasks requiring PNG + license log have been corrected to Polygon2D.**

Placeholder file naming (only if an image is EVER used, e.g. final art):
`<entity>_placeholder.png`

Every placeholder and final IMAGE FILE asset must be logged in
08_ASSET_LICENSES.md at the moment it is committed — never before, and
never with blank required fields. `Polygon2D`/`ColorRect`-based visuals
defined purely in a `.tscn` file do not require an asset license entry
(they are not separate binary assets).
