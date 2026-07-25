# ASSET PROMPTS & MASTER IDENTITY SHEETS
## Scrap Runners — Pixel Art Production Guide

This document defines every visual asset for the game with production-grade
prompts, identity lock protocols, and technical specifications. Each asset is
a MASTER IDENTITY reference that child assets (walk frames, damaged states)
must faithfully derive from.

---

████████████████████████████████████████████████████████
TECHNICAL CONSTRAINTS — ALL ASSETS
████████████████████████████████████████████████████████

| Specification | Value |
|---|---|
| Style | 8-bit / 16-bit pixel art — hard pixel edges, flat colors |
| Palette | Limited (< 16 colors per sprite), industrial grays + muted neon accents |
| Rendering | Nearest-Neighbor filtering (NO bilinear, NO smoothing) |
| Format | PNG with transparent alpha channel |
| Canvas size (characters) | 32×32 pixels |
| Canvas size (items / UI) | 16×16 or 32×8 pixels |
| Canvas size (tiles) | 32×32 pixels (tileable / seamless) |
| Canvas size (extraction) | 64×64 pixels |
| Color space | sRGB, no HDR, no post-processing |
| Background | NEUTRAL — transparent (alpha 0) |
| Godot import | Texture import preset: 2D / Pixel Art, Filter=Nearest |

⚠️ VIOLATIONS THAT WILL BE REJECTED:
- Anti-aliasing or soft edges → REJECTED
- Gradients or smooth shading → REJECTED
- Non-transparent background → REJECTED
- Wrong canvas size → REJECTED
- AI hallucination (extra limbs, merged objects) → REGENERATE

---

████████████████████████████████████████████████████████
1. PLAYER CHARACTER — SCAVENGER BOT (MASTER SHEET)
████████████████████████████████████████████████████████

This is the MASTER IDENTITY REFERENCE for the Scavenger Bot.
ALL player sprites (walk1, walk2, damaged) MUST preserve 100% of the
identity established here: body shape, eye design, antenna placement,
metallic panel layout, color palette, and proportions.

---

### 1a. player_idle.png — MASTER IDENTITY FRAME

▸ PURPOSE
Default standing / idle pose. This is the PRIMARY reference from which
all other player sprites derive.

▸ ⚠️ IDENTITY LOCK PROTOCOL (MANDATORY)

Preserve 100% of the following across ALL child assets:
- Body silhouette: short, round, top-down view, metallic gray (#607D8B-#9E9E9E)
- Eye: single large circular LED, green (#4CAF50), centered upper-body
- Antenna: small, top-center, slightly bent forward
- Panel seams: visible horizontal/vertical lines on body
- Proportions: height ≈ width (roughly square), rounded corners
- Palette: gray body + green LED + dark gray shadow details

NO body reshaping.
NO proportion changes.
NO color palette shift.
NO artistic reinterpretation.
NO style deviation between frames.

▸ PROMPT

```
32x32 pixel art, top-down 2D view, small round scavenger robot,
idle pose facing downward, metallic gray body with visible panel
seams, one large circular LED eye glowing green (#4CAF50),
small antenna on top, short rounded proportions,
no anti-aliasing, hard pixel edges, transparent background
```

▸ TECHNICAL SPECIFICATIONS
Canvas: 32×32 pixels
Palette: ≤ 12 colors (grays + green + dark gray shadows)
Lighting: Flat top-down, no directional shadows, no gradients
Style: 8-bit pixel art, hard outlines, flat color fill
Background: Transparent (alpha channel)

▸ FILE PATH
`res://assets/sprites/player/player_idle.png`

▸ CHILD ASSETS (must match this identity EXACTLY)
→ player_walk1.png
→ player_walk2.png
→ player_damaged.png

---

### 1b. player_walk1.png — WALK FRAME 1

▸ PURPOSE
First frame of 2-frame walk animation. MUST be pixel-by-pixel consistent
with player_idle.png except for leg/panel position.

▸ IDENTITY LOCK
SAME as player_idle.png with ONE change:
- Left foot-panel extended forward by 2-3 pixels
- Antenna tilts slightly forward
- Everything else IDENTICAL to idle

▸ PROMPT

```
32x32 pixel art, top-down 2D view, small round scavenger robot,
walking pose, metallic gray body with panel seams, one green LED eye,
antenna tilting slightly forward, one foot-panel extended forward,
short rounded proportions, no anti-aliasing, hard pixel edges,
transparent background
```

▸ FILE PATH
`res://assets/sprites/player/player_walk1.png`

---

### 1c. player_walk2.png — WALK FRAME 2

▸ PURPOSE
Second frame of 2-frame walk animation. MIRROR of walk1.

▸ IDENTITY LOCK
SAME as player_idle.png with ONE change:
- Right foot-panel extended forward by 2-3 pixels (opposite of walk1)
- Antenna tilts slightly backward
- Everything else IDENTICAL to idle

▸ PROMPT

```
32x32 pixel art, top-down 2D view, small round scavenger robot,
walking pose, metallic gray body with panel seams, one green LED eye,
antenna tilting slightly backward, opposite foot-panel extended
forward compared to player_walk1, short rounded proportions,
no anti-aliasing, hard pixel edges, transparent background
```

▸ FILE PATH
`res://assets/sprites/player/player_walk2.png`

---

### 1d. player_damaged.png — DAMAGED STATE

▸ PURPOSE
Replaces idle sprite when health < 25%. Communicates danger to player.

▸ IDENTITY LOCK
SAME body as player_idle.png with TWO changes:
- LED eye color changes to RED (#E53935)
- Body shows scratches / small orange sparks
- Antenna bent slightly

All proportions, panel layout, and silhouette PRESERVED.

▸ PROMPT

```
32x32 pixel art, top-down 2D view, small round scavenger robot,
damaged state, metallic gray body with scratches and small sparks,
one large circular LED eye glowing RED (#E53935),
antenna bent slightly, short rounded proportions,
no anti-aliasing, hard pixel edges, transparent background
```

▸ FILE PATH
`res://assets/sprites/player/player_damaged.png`

---

████████████████████████████████████████████████████████
2. ENEMY — PATROL DRONE (MASTER SHEET)
████████████████████████████████████████████████████████

MASTER IDENTITY for Patrol Drone. Hostile variant of robot design.
Rusty, aggressive, flying. Child assets (walk1, walk2) must preserve.

---

### 2a. patrol_drone_idle.png — MASTER IDENTITY FRAME

▸ PURPOSE
Default hostile idle sprite. PRIMARY reference for all drone sprites.

▸ ⚠️ IDENTITY LOCK PROTOCOL

Preserve across all drone sprites:
- Body: rusty metallic brown (#6D4C41), angular/aggressive silhouette
- Eye: single red LED (#E53935)
- Thrusters: small amber flame (#FF9800) on sides
- Wiring: exposed orange/brown cables
- Size: slightly larger than player (fills 32×32 canvas)

NO softening of aggressive shape.
NO color shift from rusty palette.
NO proportion change.

▸ PROMPT

```
32x32 pixel art, top-down 2D view, small flying drone robot,
hostile appearance, rusty metallic body (#6D4C41) with exposed
orange wiring, one red LED eye (#E53935), small thrusters on
sides, aggressive angular shape, slightly larger than player,
no anti-aliasing, hard pixel edges, transparent background
```

▸ TECHNICAL SPECIFICATIONS
Canvas: 32×32 pixels
Palette: ≤ 12 colors (rust browns + red + amber + dark grays)
Style: 8-bit pixel art, hard outlines, flat color fill
Background: Transparent

▸ FILE PATH
`res://assets/sprites/enemies/patrol_drone_idle.png`

---

### 2b. patrol_drone_walk1.png — CHASE FRAME 1

▸ PURPOSE
First movement frame. Thruster flame on one side brighter.

▸ PROMPT

```
32x32 pixel art, top-down 2D view, small flying drone robot,
moving forward aggressively, rusty metallic body with exposed
wiring, red LED eye glowing bright, side thrusters firing small
amber flame (#FF9800), angular shape,
no anti-aliasing, hard pixel edges, transparent background
```

▸ FILE PATH
`res://assets/sprites/enemies/patrol_drone_walk1.png`

---

### 2c. patrol_drone_walk2.png — CHASE FRAME 2

▸ PURPOSE
Second movement frame. Opposite thruster flame brighter.

▸ PROMPT

```
32x32 pixel art, top-down 2D view, small flying drone robot,
moving forward aggressively, rusty metallic body with exposed
wiring, red LED eye glowing bright, side thrusters firing small
amber flame (#FF9800) — opposite side brighter than
patrol_drone_walk1, angular shape,
no anti-aliasing, hard pixel edges, transparent background
```

▸ FILE PATH
`res://assets/sprites/enemies/patrol_drone_walk2.png`

---

████████████████████████████████████████████████████████
3. ITEMS & PICKUPS
████████████████████████████████████████████████████████

Small 16×16 icons. Must read clearly at that resolution.
No transparency violations. Silhouette must be distinct per item.

---

### 3a. scrap_ammo.png — AMMUNITION PICKUP

▸ PURPOSE
World pickup + inventory icon for basic ammunition (Scrap Ammo).

▸ PROMPT

```
16x16 pixel art, top-down 2D view, small pile of bullet casings,
rusty orange-brown color (#795548), metallic shine,
simple distinct silhouette on transparent background,
no anti-aliasing, hard pixel edges
```

▸ FILE PATH
`res://assets/sprites/items/scrap_ammo.png`

---

### 3b. scrap_pistol.png — WEAPON ICON

▸ PURPOSE
Inventory icon for the Scrap Pistol weapon.

▸ PROMPT

```
16x16 pixel art, top-down 2D view, small crude pistol,
made of scrap metal and duct tape, gray-brown metallic colors
(#607D8B), visible rough texture, simple distinct shape
on transparent background, no anti-aliasing, hard pixel edges
```

▸ FILE PATH
`res://assets/sprites/items/scrap_pistol.png`

---

### 3c. loot_crate.png — LOOT CONTAINER

▸ PURPOSE
World pickup visual for lootable containers / loot drops.

▸ PROMPT

```
16x16 pixel art, top-down 2D view, small loot container
like a metal crate or box, yellow-amber highlights (#FFC107),
metallic gray body, simple distinct shape on transparent
background, no anti-aliasing, hard pixel edges
```

▸ FILE PATH
`res://assets/sprites/items/loot_crate.png`

---

████████████████████████████████████████████████████████
4. ENVIRONMENT — TILES
████████████████████████████████████████████████████████

32×32 tileable textures. ALL tiles MUST be seamless on all four edges.
Test by tiling 3×3 in any image editor before finalizing.

⚠️ REJECTION CONDITIONS:
- Visible seams between tiles → REJECTED
- Non-tileable edges → REJECTED
- Wrong canvas size → REJECTED

---

### 4a. floor_warehouse.png — TEST LEVEL FLOOR

▸ PURPOSE
Floor tile for the raid zone / test level.

▸ PROMPT

```
32x32 pixel art, top-down 2D tileable floor texture,
worn concrete or metal floor, dark gray (#424242) with subtle
scratches and dirt spots, tileable / seamless on all four sides,
no anti-aliasing, hard pixel edges
```

▸ FILE PATH
`res://assets/sprites/tiles/floor_warehouse.png`

---

### 4b. wall_warehouse.png — TEST LEVEL WALL

▸ PURPOSE
Wall / obstacle tile for the test level.

▸ PROMPT

```
32x32 pixel art, top-down 2D tileable wall texture,
corrugated metal wall, dark industrial gray-brown (#616161)
with visible vertical ridges, tileable horizontally,
no anti-aliasing, hard pixel edges
```

▸ FILE PATH
`res://assets/sprites/tiles/wall_warehouse.png`

---

### 4c. floor_hub.png — HUB FLOOR

▸ PURPOSE
Floor tile for the Hub scene (safe zone). Must feel cleaner than warehouse.

▸ PROMPT

```
32x32 pixel art, top-down 2D tileable floor texture,
clean metal floor with safety stripes, lighter gray (#757575)
with thin yellow (#FFEB3B) stripe lines, tileable / seamless,
no anti-aliasing, hard pixel edges
```

▸ FILE PATH
`res://assets/sprites/tiles/floor_hub.png`

---

### 4d. wall_hub.png — HUB WALL

▸ PURPOSE
Wall tile for the Hub scene (safe zone). Clean, maintained appearance.

▸ PROMPT

```
32x32 pixel art, top-down 2D tileable wall,
clean white-gray metal panels (#BDBDBD) with visible bolt
details at corners, tileable horizontally,
no anti-aliasing, hard pixel edges
```

▸ FILE PATH
`res://assets/sprites/tiles/wall_hub.png`

---

████████████████████████████████████████████████████████
5. EXTRACTION POINT — ZONE MARKER
████████████████████████████████████████████████████████

Larger canvas (64×64) to be visible from across the level.
Semi-transparent so floor texture shows underneath.

---

### 5a. extraction_marker.png

▸ PURPOSE
Ground marker indicating the extraction zone. Player must stand here to
extract. Visual feedback: cyan glow, sci-fi teleport pad.

▸ PROMPT

```
64x64 pixel art, top-down 2D extraction zone marker,
translucent cyan (#00BCD4) circle or arrow pattern on the ground,
glowing effect at the edges, sci-fi teleport pad look,
semi-transparent style (visible floor underneath),
no anti-aliasing, hard pixel edges, transparent background
```

▸ FILE PATH
`res://assets/sprites/misc/extraction_marker.png`

---

████████████████████████████████████████████████████████
6. UI ELEMENTS
████████████████████████████████████████████████████████

Clean, readable UI at 640×360 base resolution. Dark theme.
No decorative flourishes — function over form.

---

### 6a. inventory_slot.png

▸ PURPOSE
Background for each slot in the 4×4 inventory grid (16 slots).

▸ PROMPT

```
32x32 pixel art, UI element, dark gray (#424242) square with
thin lighter gray border (#757575), subtle inset shadow effect,
simple clean UI style, no anti-aliasing, hard pixel edges,
transparent background
```

▸ FILE PATH
`res://assets/sprites/ui/inventory_slot.png`

---

### 6b. health_bar_fill.png

▸ PURPOSE
Filled portion of the HP bar. Width 32px, height 8px.

▸ PROMPT

```
32x8 pixel art, UI element, horizontal health bar fill,
gradient from green (#4CAF50) on left to red (#E53935) on right,
simple clean UI style, no anti-aliasing, hard pixel edges,
transparent background
```

▸ FILE PATH
`res://assets/sprites/ui/health_bar_fill.png`

---

### 6c. health_bar_bg.png

▸ PURPOSE
Background (empty portion) of the HP bar.

▸ PROMPT

```
32x8 pixel art, UI element, horizontal health bar background,
dark gray (#212121) with thin border (#424242),
simple UI style, no anti-aliasing, hard pixel edges,
transparent background
```

▸ FILE PATH
`res://assets/sprites/ui/health_bar_bg.png`

---

### 6d. directional_arrow (FALLBACK — NO PNG NEEDED)

The extraction direction indicator uses Polygon2D per ADR-005.
No PNG file required. Do not generate.

---

████████████████████████████████████████████████████████
MASTER CHECKLIST — 18 PNG FILES TOTAL
████████████████████████████████████████████████████████

▸ PLAYER (4)
- [ ] `res://assets/sprites/player/player_idle.png` (32×32) — MASTER
- [ ] `res://assets/sprites/player/player_walk1.png` (32×32)
- [ ] `res://assets/sprites/player/player_walk2.png` (32×32)
- [ ] `res://assets/sprites/player/player_damaged.png` (32×32)

▸ ENEMY (3)
- [ ] `res://assets/sprites/enemies/patrol_drone_idle.png` (32×32) — MASTER
- [ ] `res://assets/sprites/enemies/patrol_drone_walk1.png` (32×32)
- [ ] `res://assets/sprites/enemies/patrol_drone_walk2.png` (32×32)

▸ ITEMS (3)
- [ ] `res://assets/sprites/items/scrap_ammo.png` (16×16)
- [ ] `res://assets/sprites/items/scrap_pistol.png` (16×16)
- [ ] `res://assets/sprites/items/loot_crate.png` (16×16)

▸ TILES (4)
- [ ] `res://assets/sprites/tiles/floor_warehouse.png` (32×32, tileable)
- [ ] `res://assets/sprites/tiles/wall_warehouse.png` (32×32, tileable)
- [ ] `res://assets/sprites/tiles/floor_hub.png` (32×32, tileable)
- [ ] `res://assets/sprites/tiles/wall_hub.png` (32×32, tileable)

▸ MISC (1)
- [ ] `res://assets/sprites/misc/extraction_marker.png` (64×64)

▸ UI (3)
- [ ] `res://assets/sprites/ui/inventory_slot.png` (32×32)
- [ ] `res://assets/sprites/ui/health_bar_fill.png` (32×8)
- [ ] `res://assets/sprites/ui/health_bar_bg.png` (32×8)

---

> **⚠️ FALLBACK POLICY**
> `Polygon2D` colored-shape placeholders remain the DEFAULT for all
> sprites until a PNG asset is placed in the path above. The game
> loads PNG if it exists; otherwise falls back to Polygon2D.
> This prevents crashes from missing assets.
> Polygon2D placeholders are NOT logged in 08_ASSET_LICENSES.md
> (they are code-generated shapes, not binary assets).

---

████████████████████████████████████████████████████████
WORKFLOW — HOW TO GENERATE EACH ASSET
████████████████████████████████████████████████████████

1. **Copy the PROMPT** from the asset section below into your AI tool.
2. **Generate** at a higher resolution (256×256 recommended).
3. **Downscale** to target size using Nearest-Neighbor interpolation.
4. **Remove background** (export with transparency).
5. **Save as PNG** to the listed FILE PATH in the project.
6. **For walk1/walk2:** Generate walk1 with AI, then open in Piskel,
   duplicate frame, and manually edit the leg position for walk2.
   Do NOT rely on AI to produce two consistent walk frames — it won't.

> **⚠️ IDENTITY LOCK LIMITATION**
> Without uploaded reference images, AI image generators may produce
> inconsistent results between frames (e.g. walk1 vs walk2 may look
> different). To work around this:
> - **Leonardo.ai:** Use "Image Guidance" — upload the first frame as
>   reference when generating the second.
> - **Midjourney:** Use `--seed [same number]` across both frames.
> - **Best practice:** Generate the MASTER frame with AI, then edit
>   child frames manually in Piskel (2-3 minutes per pair).
