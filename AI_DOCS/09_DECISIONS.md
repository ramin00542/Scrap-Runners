# Architecture Decision Records (ADR)

Decisions here are LOCKED and append-only. An ADR's Decision/Reason text
is never silently repurposed for an unrelated topic — if a topic needs
revision, either amend it explicitly with the change clearly marked, or
set its Status to "Superseded by ADR-0XX" and write a new ADR.

## ADR-001 — Inventory Model
Status: Accepted
Decision: Slot-based inventory for MVP (fixed slot count, one item
identity per slot, stacking up to `stack_size`, matched by `item_id`).
Reason: Spatial/rotatable inventory is out of MVP scope.

## ADR-002 — Engine Version Lock
Status: Accepted
Decision format (fill exactly):
- Engine version string: `v4.7.1.stable.official [a13da4feb]`
Reason: Prevents API drift across AI sessions.

## ADR-003 — Player Character Identity
Status: Accepted
Decision: Small scavenger robot ("Scavenger Bot"), not a duck, not
humanoid.
Reason: Establish visual identity distinct from Escape from Duckov.

## ADR-004 — Base Resolution and Camera
Status: Accepted
Decision: 640x360 base resolution, canvas_items stretch mode, keep
aspect, nearest filtering, 32x32 character sprites.
Reason: 320x180 with 32x32 sprites leaves insufficient vertical
visibility for ranged top-down combat.

## ADR-005 — Minimap Deferred
Status: Accepted
Decision: No minimap in MVP. A directional indicator (arrow toward
Extraction Point, built with Polygon2D, no image asset needed) is used
instead.
Reason: Minimap requires fog-of-war, markers, and coordinate conversion
— unnecessary before the core loop works.

## ADR-006 — Melee Weapon Deferred
Status: Accepted
Decision: MVP ships with exactly one ranged weapon.
Reason: Two weapon types double complexity before the core loop is proven.

## ADR-007 — No Speculative Autoloads
Status: Accepted
Decision: Autoloads are created only in the task that gives them their
first real responsibility and, wherever feasible, first real usage.
Reason: Prevents unclear data ownership.

## ADR-008 — 12-Part Structure Lock
Status: Accepted
Decision: The project is structured into 12 Parts in this exact order:
part_01_foundation, part_02_player_movement, part_03_health_damage,
part_04_core_data, part_05_inventory_core, part_06_combat, part_07_enemy,
part_08_inventory_ui, part_09_test_level_and_extraction,
part_10_hub_save, part_11_content_ui, part_12_qa_export.
Reason: Resolves dependency ordering (WeaponData needs ItemData first,
EnemyData needs LootTable first, Combat needs Inventory first,
Extraction needs a Test Level first).

## ADR-009 — Inventory Is a Node, Autoload Scripts Have No class_name
Status: Accepted
Decision: `Inventory` is a `Node` subclass, attached as a child node.
Autoload singleton scripts omit `class_name` entirely.
Reason: A `RefCounted`-based Inventory has no Inspector-tunable
`@export` values in practice and is invisible in the Remote Scene Tree.
Declaring `class_name X` on a script whose global autoload name is also
`X` risks a name collision.

## ADR-010 — Reload Input Action
Status: Accepted
Decision: Add a `reload` input action bound to R (physical key), created
directly in TASK-01-01 as part of the Foundation Input Map (not deferred
to Part 06).
Reason: Combat (Part 06) requires a reload action; creating it upfront
in Foundation, alongside the other 7 actions, avoids a Part 02+
dependency on a decision made mid-project.

## ADR-011 — ItemDatabase Autoload
Status: Accepted
Decision: Introduce an `ItemDatabase` autoload mapping
`item_id -> ItemData`. ItemDatabase is the canonical runtime lookup for
reconstructing ItemData references from persisted item_id values.
Save/Hub-loading systems (SaveManager, Hub) use it to rebuild Stash
contents from disk. Gameplay systems (Player, Inventory, Combat)
continue to carry direct ItemData references at runtime and do not query
ItemDatabase during normal play.
Reason: StashData persists only `item_id` + `amount`; a lookup table is
required to turn saved IDs back into usable `ItemData` at load time.

## ADR-012 — Raid Inventory Lifecycle and Fixed MVP Loadout
Status: Accepted
Decision:
1. The player's raid Inventory always starts EMPTY at the beginning of a
   raid.
2. The player's WEAPON (Scrap Pistol) is NOT part of the raid Inventory —
   it is a permanent, fixed property of the Player character in MVP,
   always available, always starts with a full magazine (10/10) on raid
   entry, with 0 reserve ammo unless the player picks some up during the
   raid.
3. Only items present in the raid Inventory at `extraction_completed`
   transfer into the Stash. The Scrap Pistol and its magazine ammo are
   NOT transferred.
4. On player death (raid failure), the raid Inventory is discarded, the
   Stash remains unchanged, and the player returns to Hub.
5. There is no loadout-selection UI, no gear progression, and no
   persistence of anything except the Hub Stash in MVP.
Reason: Avoids the complexity of persisting a live Player node across a
scene transition; keeps MVP scope aligned with 02_GAME_GOAL.md.

## ADR-013 — Physics Layer Conventions (SUPERSEDED)
Status: Superseded by ADR-014
Decision: Lock 7 physics collision layers, created directly in
TASK-01-01 (not deferred to a later Part):

| Layer # | Name |
|---|---|
| 1 | world |
| 2 | player |
| 3 | enemies |
| 4 | player_projectiles |
| 5 | enemy_projectiles |
| 6 | pickups |
| 7 | extraction_zone |

Standard layer/mask assignment per entity type (ORIGINAL — INCORRECT, used layer numbers as bitmask values):

| Entity | collision_layer | collision_mask |
|---|---|---|
| World geometry (walls) | 1 (world) | 0 |
| Player | 2 (player) | 1 (world) |
| Enemy | 3 (enemies) | 1 (world) |
| Player projectile | 4 (player_projectiles) | 1 (world) + 3 (enemies) |
| Enemy projectile | 5 (enemy_projectiles) | 1 (world) + 2 (player) |
| Pickup | 6 (pickups) | 2 (player) |
| Extraction zone | 7 (extraction_zone) | 2 (player) |

Reason: An earlier draft of this decision was mistakenly written into
ADR-010's text (a different, unrelated topic), and additionally omitted
a "world" layer, leaving Player's collision_mask at 0 (collides with
nothing) — making "walls block movement" unverifiable. This ADR is a
distinct, separately-numbered decision, correcting both errors. ADR-010
has been restored to its own original, unrelated topic (the reload
action). See TASK-01-04 for the consistency check confirming every task
file uses this table correctly from the start.

**Supersession Note (added by ADR-014):** This ADR incorrectly used
layer numbers (1..7) as `collision_layer`/`collision_mask` values. In
Godot, those properties are bitmasks: value = 1 << (layer_number-1).
Thus `collision_layer = 3` does NOT mean "layer 3" but means "layers
1 and 2" simultaneously (binary 0b11). This caused wrong masks for all
entities except world/player. See ADR-014 for the corrected bitmask table.
Content preserved for audit trail.

## ADR-014 — Physics Layer Bitmask Correction
Status: Accepted
Decision: Clarify and CORRECT the physics layer convention to use proper
Godot bitmask values.

Godot 2D physics layers work as follows:
- Inspector shows "Layer #" 1..32 as checkboxes.
- The runtime property `collision_layer` / `collision_mask` is a bitmask
  integer where bit (layer_number-1) represents that layer.
- Correct conversion: `bitmask_value = 1 << (layer_number - 1)`.
- Example: `collision_layer = 3` is NOT layer 3; it is binary `0b11`,
  meaning layers 1 AND 2 simultaneously. Correct value for layer 3 is 4.

### Locked Layer Names (unchanged, per TASK-01-01)

| Layer # (Inspector checkbox) | Name | Correct Bitmask Value |
|---:|---|---:|
| 1 | world | 1 (1 << 0) |
| 2 | player | 2 (1 << 1) |
| 3 | enemies | 4 (1 << 2) |
| 4 | player_projectiles | 8 (1 << 3) |
| 5 | enemy_projectiles | 16 (1 << 4) |
| 6 | pickups | 32 (1 << 5) |
| 7 | extraction_zone | 64 (1 << 6) |

Named directly in `project.godot`'s `layer_names/2d_physics/*` settings.

### Corrected Standard layer/mask assignment per entity type (LOCKED)

| Entity | collision_layer (value) — layer name | collision_mask (value) — includes |
|---|---|---|
| World geometry (walls) | 1 — world | 0 — collides with nothing, but is collided into |
| Player | 2 — player | 1 — world |
| Enemy | 4 — enemies | 1 — world |
| Player projectile | 8 — player_projectiles | 5 — world(1) + enemies(4) = 1+4=5 |
| Enemy projectile | 16 — enemy_projectiles | 3 — world(1) + player(2) = 1+2=3 |
| Pickup | 32 — pickups | 2 — player |
| Extraction zone | 64 — extraction_zone | 2 — player |

### Implementation Rule

- When setting in GDScript, use the BITMASK values above: e.g.
  `collision_layer = 4  # enemies, bitmask 4 for layer 3 — per ADR-014`
  or `collision_layer = 8  # player_projectiles — per ADR-014`
  or use bit-shift for clarity: `1 << 2` for layer 3.
- In Editor, check the correct Layer # checkbox (1..7). Godot will store
  the correct bitmask automatically — but any code that sets the value
  explicitly MUST use the bitmask values, not the layer numbers.
- Every `CollisionObject2D`/`Area2D` in the project sets its layer/mask
  exactly per this corrected table, with a code comment citing
  "per ADR-014" — never a bare unexplained integer (01_RULES.md section 9).
- All future task files and any existing task files found during
  TASK-01-04 consistency check must be updated to this table.

Reason: ADR-013 used layer numbers as values, which in Godot means
multiple layers simultaneously (e.g. `collision_layer = 3` enables layers
1 and 2, `collision_layer = 6` enables layers 2 and 3, etc.), breaking
collision filtering. Correcting before any scene with physics is created
avoids runtime bugs that are hard to debug later. No code has been
executed yet (still in Part 01), so this is a pure documentation fix.

Migration: TASK-01-04 will verify and correct every pending task file
under AI_DOCS/PARTS/ that sets collision_layer/mask. ADR-013 is kept for
audit but marked Superseded by ADR-014.

## ADR-015 — Post-MVP Roadmap Adoption

Status: Accepted
Decision: Adopt `AI_DOCS/10_POST_MVP_ROADMAP.md` as the canonical
collection of post-MVP feature ideas, organized into 5 phases.
Reason: Without a roadmap, post-MVP features would be added ad-hoc,
risking scope creep and architectural inconsistency. This document
serves as the single source of truth for what could come next.

## ADR-016 — Camera Angle: Slight Isometric Tilt (2D)

Status: Accepted
Supersedes: only the camera-angle / presentation clause implied by
ADR-004 and related top-down wording. ADR-004 remains in force for
base resolution (640×360), stretch mode, nearest filtering, sprite
sizes (32×32 characters), and Camera2D zoom (1.0). Those values are
NOT changed by this ADR.

Decision:
1. Architecture stays fully 2D. No pivot to 3D. Nodes remain
   Node2D / CharacterBody2D / Camera2D / Polygon2D (and later PNG
   sprites). Camera2D is not rotated in 3D space — a 2D camera has
   no pitch/yaw angle to tilt.

2. "Slight isometric feel" is achieved by scene/sprite presentation
   only, not by changing the physics plane or the engine projection.

3. Chosen technique: **Option B**
   - Keep floor tiles flat/square (no diamond TileMap shape change).
   - Apply a small fixed vertical visual offset to standing entities
     (player, enemies, loot markers, etc.) so they read as "standing
     up" slightly toward the camera, rather than pure flat top-down.
   - Enable **Y-sorting** (draw order by Y position) on the relevant
     parent node(s) so overlapping entities and props paint in a
     depth-correct order.

4. Rejected for MVP (higher risk): **Option A**
   - TileMap with diamond-shaped (isometric) tiles instead of square
     tiles.
   - Rejected because it multiplies art, collision, navigation, and
     level-authoring complexity (half-offset grids, diamond collision
     shapes, harder AI path reasoning) for a visual gain that Option B
     can approximate more cheaply.

5. New technical requirement: **Y-sorting**
   - Flat pure top-down did not require Y-sort for correct draw order.
   - With any height-like presentation (offsets, multi-tile props,
     overlapping characters), Y-sorting is required so entities lower
     on the screen draw in front of entities higher on the screen.

Reason: A slight isometric presentation improves readability and depth
without abandoning the locked 2D architecture or the ADR-004
resolution/filter/sprite pipeline. Option B is the lower-risk path:
placeholders are Polygon2D, physics is orthographic CharacterBody2D
movement, and MVP tilesets are not yet built (part_09). Diamond tiles
(Option A) would force an early, hard-to-reverse tileset and collision
convention for a modest visual return.

Non-goals (explicitly out of scope for this ADR):
- Real 3D models, custom pixelization shaders, or Camera3D
- Changing base resolution, filter mode, or sprite pixel sizes
- Changing movement physics to true isometric axes (e.g. 2:1
  screen-space movement) unless a later ADR says so
- Immediate task edits; apply as a reference when TASK-02-01 and
  TASK-09-01 become active

<!-- Add new ADRs below this line, do not renumber existing ones -->
