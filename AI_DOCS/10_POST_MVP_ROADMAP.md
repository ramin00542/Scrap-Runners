# Post-MVP Roadmap — Scrap Runners

> **⚠️ THIS IS A ROADMAP, NOT A TASK LIST.**
> This document exists solely to collect and organize ideas for features
> that can be added AFTER the MVP (Part 01–12) is complete and verified.
> Nothing in this file may be implemented before Part 12 is finished and
> its Acceptance Criteria are confirmed. Until then, the only actionable
> scope is the MVP Feature List in 02_GAME_GOAL.md.
>
> When a feature here is chosen for implementation, it must:
> 1. Be proposed as a new ADR in 09_DECISIONS.md (if it changes a locked
>    decision).
> 2. Get its own dedicated Part (or set of Tasks) under AI_DOCS/PARTS/.
> 3. Be fully specified in a new CURRENT_TASK.md before any code is written.
>
> See ADR-015 in 09_DECISIONS.md for the adoption of this roadmap.

---

## Overview

**Vision:** Transform Scrap Runners from a functional MVP into a rich,
replayable extraction looter-shooter on par with Escape from Duckov and
similar titles — while preserving its unique pixel-art identity and the
Scavenger Bot character.

Each phase builds on the previous one, but phases are independent enough
that they could be implemented in a different order if priorities change.

---

## PHASE 1 — Content Expansion 🎮
*Estimated effort: 3–5 Parts (~15–25 Tasks)*

The MVP has one weapon, one enemy, one map. Phase 1 doubles everything.

### 1.1 — More Weapons

| Weapon | Role | Inspiration |
|---|---|---|
| **Scrap Rifle** | Mid-range automatic | Assault rifle class |
| **Scrap Shotgun** | Close-range burst | Shotgun class |
| **Scrap SMG** | Fast-firing close-range | SMG class |
| **Scrap Sniper** | Long-range single-shot | Marksman class |

Requires:
- New WeaponData `.tres` files
- New projectile scenes (different speeds, damages, pierce)
- New sprite assets (32×32 per weapon)
- Ammo type integration — see 2.7

### 1.2 — More Enemy Types

| Enemy | Behavior | Difficulty |
|---|---|---|
| **Scavenger Drone** (melee) | Rushes player, explodes on contact | Easy |
| **Turret** (stationary) | Fixed position, high accuracy, slow rotation | Medium |
| **Boss — Scrap King** | Large, armored, multi-phase attack pattern | Very Hard |

> **Note:** Rival Scavenger Bot is deferred to Phase 3 (3.8) because
> it relies on the Loadout Selection system from Phase 2 to determine
> its equipment.

### 1.3 — More Maps

| Map Name | Theme | Size | Special Feature |
|---|---|---|---|
| **Warehouse District** | Industrial warehouses (MVP map) | Small | Tight corridors |
| **Solar Fields** | Open desert with solar panels | Medium | Long sightlines, little cover |
| **Underground Bunker** | Dark, cramped tunnels | Small | Limited visibility, sound matters |
| **Scrap Processing Plant** | Large facility with conveyor belts | Large | Multiple floors, hazards |

**Architectural requirements for multi-map support:**
- Extract map-specific data (spawn points, extraction zones, loot tables)
  into a `MapData` resource class.
- Create a scene-loading system that accepts a `MapData` reference and
  instantiates the correct `.tscn` + populates entities at spawn points.
- Each map needs its own `TileMapLayer` setup, collision layer
  assignments, and navigation region baking.
- Maps MUST share the same base resolution (640×360) and camera zoom (1.0)
  for consistency — see ADR-004.

### 1.4 — Healing & Consumables

| Item | Effect | Rarity |
|---|---|---|
| **Scrap Bandage** | Restore 20 HP | Common |
| **Repair Kit** | Restore 50 HP over 5 seconds | Uncommon |
| **Emergency Battery** | Full heal, single use | Rare |
| **Stim Injector** | +50% move speed for 10 seconds | Uncommon |

---

## PHASE 2 — Progression Systems 📈
*Estimated effort: 5–8 Parts (~25–40 Tasks)*

Phase 2 turns the linear loop into a persistent progression arc.

### 2.1 — Loadout Selection UI (supersedes ADR-012)

Currently: player always has Scrap Pistol, raid Inventory starts empty.

Future: Before each raid, player selects from their Stash:
- **Primary weapon** (from unlocked pool)
- **Secondary weapon** (sidearm / melee)
- **Armor vest** (reduces damage taken)
- **Backpack** (determines raid Inventory slot count)
- **Ammo pouches** (starting reserve ammo — see 2.7)
- **Consumable slots** (healing items, grenades)

Requires ADR-012 revision.

### 2.2 — Armor & Equipment System

| Equipment Type | Stats | Visual |
|---|---|---|
| **Light Frame** | +10% damage reduction | Thin metal panels |
| **Standard Frame** | +25% damage reduction | Thick chest plate |
| **Heavy Frame** | +40% damage reduction, -15% move speed | Full body armor |

Each armor piece has durability — takes damage, needs repair.

### 2.3 — Backpack / Inventory Size

| Backpack Type | Slots | Rarity |
|---|---|---|
| **Pouch** | 4 slots | Default |
| **Small Pack** | 8 slots | Common |
| **Standard Pack** | 12 slots | Uncommon |
| **Large Pack** | 16 slots, -10% move speed | Rare |

### 2.4 — Merchant / Trading NPC

Located in the Hub. Allows player to:
- **Sell** unwanted loot from Stash for Scrap Credits
- **Buy** specific items at marked-up prices
- **Barter** — trade N of item A for 1 of item B
- **Repair** — pay credits to restore armor/weapon durability

### 2.5 — Scrap Pistol Upgrades

Since Scrap Pistol is the signature weapon — allow upgrades:
- **Better barrel** (more damage)
- **Extended magazine** (more rounds)
- **Laser sight** (better accuracy)
- **Scrap Pistol Mark II / III / IV** tiers

### 2.6 — Weapon Attachments

| Attachment | Effect |
|---|---|
| **Scope** | Zoom in, better accuracy at range |
| **Suppressor** | Reduce sound range (stealth) |
| **Grip** | Faster reload |
| **Extended Mag** | More rounds per magazine |
| **Flashlight** | Illuminate dark areas |

### 2.7 — Ammo Types (moved from Phase 1)

Tightly coupled to loadout selection, backpack, and weapon attachments —
better designed here than in Phase 1.

| Ammo Type | Traits | Used By |
|---|---|---|
| **Light Rounds** | Fast fire rate, low damage | SMG, Pistol |
| **Heavy Rounds** | Slow, high damage, wall penetration | Rifle, Sniper |
| **Shells** | Spread, close-range | Shotgun |
| **Energy Cells** | Medium damage, no bullet drop, rare | Special weapons |

Slot-based ammo management — which ammo you bring in your pouches
affects gameplay.

---

## PHASE 3 — Advanced Gameplay 🔥
*Estimated effort: 6–9 Parts (~30–45 Tasks)*

Phase 3 adds depth and tactical choice to every raid.

### 3.1 — Melee Weapons (supersedes ADR-006)

| Weapon | Damage | Speed | Special |
|---|---|---|---|
| **Scrap Wrench** | 15 | Fast | None |
| **Pipe** | 25 | Medium | Stuns enemies |
| **Crowbar** | 20 | Medium | Can open locked doors |
| **Chainsaw** | 40/s | Slow | Loud, attracts enemies |

### 3.2 — Grenades & Throwables

| Item | Effect |
|---|---|
| **Scrap Grenade** | Area damage, 3-second fuse |
| **Smoke Canister** | Obscures vision in area |
| **Flashbang** | Stuns enemies for 2 seconds |
| **Proximity Mine** | Placeable trap, detonates on enemy step |

### 3.3 — Key System / Locked Doors

- **Scrap Keycards** (Single-use, open specific doors)
- **Locked rooms** contain rare loot
- **Crowbar** (melee weapon) can force some locks with noise
- **Key spawns** — randomized locations per raid

### 3.4 — Safe Rooms

Designated safe rooms on each map:
- No enemies enter
- Extraction timer pauses inside
- Can heal / reload safely
- But: enemies may camp outside

### 3.5 — Stealth Mechanics

- **Crouch** — slower movement, nearly silent footsteps
- **Sound radius** — each weapon/action emits sound, attracts enemies
- **Silhouette detection** — enemies spot you only if in line-of-sight
- **Darkness** — hide in shadows (requires flashlight or night vision)

### 3.6 — Day/Night Cycle

| Time | Effect |
|---|---|
| **Day** | Full visibility, enemies more aggressive |
| **Dusk** | Reduced range, enemies less alert |
| **Night** | Very limited visibility, stealth bonus, enemies patrol tighter |

Maps could have a random time-of-day on each raid.

### 3.7 — Interactive Environment

- **Breakable barrels** — explode, damage nearby
- **Disabled drones** — can be looted for parts
- **Control panels** — disable turrets briefly
- **Conveyor belts** — move player/enemies
- **Elevators** — vertical movement between floors

### 3.8 — Rival Scavenger Bot (moved from Phase 1.2)

| Weapon | Damage | Speed | Special |
|---|---|---|---|
| **Rival Scavenger Bot** | Uses player's current loadout | Same as player | Uses cover, heals when low |

Requires Phase 2 (Loadout Selection, 2.1) to be complete so the Rival
Bot can meaningfully mirror the player's equipment. Without loadout
variety, the Rival Bot would be identical every raid.

### 3.9 — Minimap (supersedes ADR-005)

> **ADR-005 currently:** Minimap Deferred — MVP uses a directional arrow
> indicator instead.
>
> **Post-MVP:** A minimal top-down minimap showing:
> - Explored area (fog-of-war for unexplored)
> - Extraction Point marker
> - Player position & facing direction
> - Enemy blips only when within detection range
>
> Must NOT show:
> - Loot locations
> - Full map geometry (keep fog-of-war)
> - Enemy positions outside detection

Design constraints: minimap must be a separate `Control` node in the HUD,
rendered from a second `Camera2D` or a decal overlay. Performance must
be validated on the lowest supported hardware before shipping.

---

## PHASE 4 — High-End Systems 🏗️
*Estimated effort: 6–10 Parts (~30–50 Tasks)*

Phase 4 adds systems that transform the game into a "forever game."

### 4.1 — Quest / Mission System

| Quest Type | Example | Reward |
|---|---|---|
| **Collection** | "Collect 5 circuit boards" | Credits, unique item |
| **Elimination** | "Kill 10 rival scavengers" | Weapon unlock |
| **Extraction** | "Extract from Bunker 3 times" | Backpack upgrade |
| **Boss Hunt** | "Defeat Scrap King" | Rare blueprint |
| **Timed** | "Extract within 2 minutes" | Bonus credits |

Daily / weekly quest rotation for replayability.

### 4.2 — Skills & Character Progression

| Skill | Effect | Max Level |
|---|---|---|
| **Scavenging** | More loot from containers | 5 |
| **Combat** | More weapon damage | 5 |
| **Engineering** | Better repair efficiency | 5 |
| **Stealth** | Quieter movement | 5 |
| **Endurance** | Faster stamina regen | 5 |

XP earned per raid, skill points allocated in Hub.

### 4.3 — Crafting System

- **Scrap Bench** in Hub
- **Recipes** learned or purchased
- **Materials** (circuit boards, steel plates, wiring, batteries)
- Craft: ammo → medkits → weapon attachments → items

Example recipes:
- `1 Steel Plate + 1 Wiring` → 1 Scrap Pistol Attachment
- `3 Scrap Metal` → 1 Magazine of Light Rounds
- `1 Battery + 2 Wiring` → 1 Repair Kit

### 4.4 — Boss Fights

Each map could have a boss with:
- Unique visual design (larger, more menacing)
- Multi-phase attack patterns
- Exclusive loot table (can't get elsewhere)
- Optional — can be avoided, but high risk/reward

### 4.5 — Procedural Elements (supersedes 02_GAME_GOAL.md "out of scope")

- **Loot placement** — randomized containers per raid
- **Enemy patrol routes** — slight randomization
- **Extraction point** — 2-3 possible locations, 1 active per raid
- **Obstacle placement** — some doors randomly blocked

Not full procedural generation — more like "hand-crafted maps with randomized elements."

### 4.6 — Advanced AI

- **Squad behavior** — enemies call for backup
- **Flanking** — enemies try to surround player
- **Suppression** — enemies lay down covering fire
- **Retreat** — wounded enemies flee
- **Ambush** — some enemies lie in wait

---

## PHASE 5 — Stretch Goals 🌟
*Estimated effort: 8–15 Parts (~40–75 Tasks)*

Phase 5 is aspirational — these features make the game truly special
but require significant engineering investment.

### 5.1 — Sound System & Audio Design

- **Spatial audio** — directional footsteps, gunshots
- **Ambient soundscapes** per map
- **Weapon SFX** — distinct per weapon type
- **Enemy sounds** — patrol beeps, alert chirps, attack buzzes
- **Music** — Hub ambient vs. Raid tension music
- **Voice cues** — "Extraction available," "Enemy detected"

### 5.2 — Weather System

| Weather | Effect |
|---|---|
| **Clear** | Normal |
| **Rain** | Reduced visibility, quieter footsteps |
| **Fog** | Very limited range, extraction harder to find |
| **Sandstorm** | (Scrap Fields map only) Near-zero visibility |

### 5.3 — Achievements / Badges

- Steam-style achievements for milestones
- Badges visible in Hub
- Cosmetic unlocks (antenna colors, LED colors, decals)

Examples:
- "First Raid" — Complete one raid
- "Scrap Hoarder" — Accumulate 1000 scrap
- "Untouchable" — Extract with full health 10 times
- "Boss Slayer" — Defeat Scrap King

### 5.4 — New Game Modes

| Mode | Description |
|---|---|
| **Free Roam** | No timer, no enemies, just explore and loot |
| **Timed Challenge** | Extract before countdown with bonus objectives |
| **Endless Wave** | Arena mode, survive increasingly hard waves |
| **Scavenger Hunt** | Find specific items on map, fastest time wins |

### 5.5 — Multiplayer / Co-op (major ADR required)

- **Co-op Raids** — 2-4 players enter same raid
- **Shared loot** or individual loot (TBD)
- **Revive system** — downed teammates can be revived
- **Competitive** — players can extract together or betray each other?
- Requires: networking, synchronization, dedicated server or P2P

### 5.6 — Modding Support

- Expose game data as editable `.tres` files
- Allow custom maps (TileMap export/import)
- Workshop integration for sharing mods

---

## Summary Table

| Phase | Focus | Parts (est.) | Tasks (est.) | Inspired By |
|---|---|---|---|---|
| **1 — Content** | More weapons, enemies, maps, items | 3–5 | 15–25 | Duckov base content |
| **2 — Progression** | Loadout, armor, backpack, merchant, ammo, upgrades | 5–8 | 25–40 | Duckov progression |
| **3 — Advanced** | Melee, grenades, keys, stealth, minimap, rival bot | 6–9 | 30–45 | Duckov tactical depth |
| **4 — Systems** | Quests, skills, crafting, bosses, AI | 6–10 | 30–50 | Duckov endgame |
| **5 — Stretch** | Audio, weather, achievements, co-op, mods | 8–15 | 40–75 | Aspirational |

**Total (estimated):** 28–47 Parts, 140–235 Tasks beyond MVP.

---

## How to Begin

When MVP (Part 12) is complete:

1. The chosen feature from this roadmap gets a proposal in Review/Planning Mode.
2. A new ADR is written (if needed) to revise any locked decisions.
3. Task files are created under a new `AI_DOCS/PARTS/part_13_.../` directory.
4. The feature is implemented one Task at a time, following the same
   documentation-driven process as the MVP.

---

<!--
This file is append-only. Add new features at the end of the appropriate
Phase section. Do not remove or demote existing entries without a new ADR.
-->
