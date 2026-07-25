# Game Design Goal (Condensed GDD)

## Working Title
Scrap Runners (placeholder — see ADR-003 if changed)

## Genre
Top-down 2D extraction looter-shooter.
Inspiration: core loop structure of "Escape from Duckov" — NOT its
characters, art, or names. See 04_ART_DIRECTION.md for our own identity.

## Core Loop
1. Player leaves a safe Hub and enters a Raid Zone (level).
2. Player explores the zone, picks up loot (materials, weapons, items).
3. Player encounters AI-controlled enemies and must fight or avoid them.
4. Player must reach an Extraction Point before a timer runs out or
   before dying.
5. Loot carried out successfully is added to the Hub Stash.
6. Repeat.

## Player Character Identity
NOT a duck, NOT humanoid. Default identity: a small scavenger robot
("Scavenger Bot"). Full description in 04_ART_DIRECTION.md. Locked by
ADR-003; changing it requires a new ADR.

## MVP Feature List

- [ ] Top-down 8-directional player movement with idle/walk animation
- [ ] Health and damage component (separate from movement)
- [ ] Item, weapon, loot, and enemy data resources (see 06_DATA_SCHEMA.md)
- [ ] Slot-based inventory component (grid-displayed, NOT spatial/
      rotatable — see ADR-001)
- [ ] Scrap Pistol as the Player's PERMANENT MVP loadout — not stored in
      or lost from the raid Inventory, always fully available at the
      start of every raid (see ADR-012)
- [ ] One enemy type with simple AI (patrol → chase → attack)
- [ ] Inventory UI (grid display of the slot-based inventory)
- [ ] A test level containing loot pickups and an Extraction Point
- [ ] Loot drops from enemies on death
- [ ] Extraction Point with timer, live progress feedback, and
      success/cancel states
- [ ] Hub scene with a persistent Stash
- [ ] Minimal HUD: health bar, ammo counter, directional extraction
      indicator (NOT a full minimap — see ADR-005)
- [ ] Save/load of the Hub Stash ONLY (see ADR-012 — Player loadout is
      fixed and never persisted, since it never changes)
- [ ] A defined raid-failure flow: player death returns to Hub with no
      Stash changes (see ADR-012)

## Explicitly Out of Scope for MVP

- Multiplayer / networking
- Procedural level generation
- Complex crafting trees
- Branching dialogue / narrative systems
- Melee weapons (see ADR-006)
- Minimap (see ADR-005)
- Spatial/rotatable inventory (see ADR-001)
- Loadout selection UI / choosing which weapon to bring into a raid —
  the Player always brings the fixed Scrap Pistol (see ADR-012)
- Equipment upgrades / gear progression between raids
- Persisting or loading player loadout state (only Stash is persisted)

## MVP Success Criterion

A single playable raid can be completed end-to-end:
Enter Hub → Enter Raid → Find loot → Survive/fight one enemy type →
Reach Extraction Point → Return to Hub → Stash updated and saved.
A failed raid (player death) must also be handled gracefully: return to
Hub with the Stash unchanged.

## Build Order Rationale

Health and damage must exist before combat is meaningful. Core data
resources (Item, Weapon, Loot, Enemy — in that dependency order) must
exist before Inventory has anything real to store, and before Combat or
Enemy AI can reference them. Inventory Core must exist before Combat can
consume ammo. A test level must exist before Loot/Extraction logic can
be placed anywhere. This dependency chain is why the Part order in
AI_DOCS/PARTS/ differs from the feature list above — always follow the
Part order, not this list's order, when deciding what to build next.
