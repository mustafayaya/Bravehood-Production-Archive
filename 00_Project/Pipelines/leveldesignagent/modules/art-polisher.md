# Module 3 — Art Polisher

Prop placement and artistic level dressing toward the reference target: candle-lit stylized low-poly stone halls, glowing azure hero crystals on rune-circle daises, teal-vs-gold light contrast, readable focal hierarchy from every doorway.

**Check [../LEARNED.md](../LEARNED.md) first — user-taught rules override everything below.**

## Canonical prop usage (taught by user, W16 session — authoritative)

These prefabs are near-zero size natively; **correct usage is a large uniform scale + rX=270 upright + yaw-only facing** (rZ=0). Measured canon:

| Prefab | Scale | Notes |
|---|---|---|
| Book_Shelves_01 | **190** | → 2.59 w × 3.63 h × 0.74 d |
| Court_Table_01 | **153.52** (re-tuned session 15; 216.74 and 460 both superseded) | hero/dispensary table |
| Alchemy_Set_01 (on-table) | **118.75** (session 15; 167.7 superseded) | with Mortar ≈ 11.8, tabletop candle ≈ 113 |
| Table_01 | **120** (corrected session 7; 91.22 read small) | side/prep table — cluster in L-shapes, don't isolate |
| Fireplace_01 | **200** (corrected session 7; 137 too small) | against a SOLID wall — never in front of a door |
| Armory_Rack_01 | **150** (corrected session 13; 87.6 too small) | casual non-axial yaws |
| Toolbench_02 | **150** (z ≈ 132) (corrected session 13) | workbench |
| Knight_Armor / Knight_Helmet | ~127 standing guard; **s 38 / 24 = discarded loot pieces** (tumbled, 3-axis) | small scale = loot, never statues (statues are banned stand-ins, R039) |
| Boiler_01 | **150** as a freestanding central cauldron (session 7) | |
| Serving_Car_01 | ~62–70 | 1 of 3 may be deliberately toppled (3-axis tumble, on floor) |
| Barrels_01 | 109.16 | |
| Candlelight_01 | 150 tabletop / 160 feature / **243–275 floor candelabra** | carries the room's Point Light |
| Alchemy_Set_01 | 167.7 | floor-standing feature |
| Mortar_with_Pestle_01 | 15.6–16.7 | tabletop |
| Potion_Bottle_01/02 | 14.7–25.9 | tabletop/shelf |
| Scrolls_01 | **25** (35 too big — corrected session 3) | rX≈288.6 tilt + varied rZ; seat flush on surface |
| Production_Table_01/02 | **112** (corrected session 3) | workbench |
| Distillation_System_01 | **125** (corrected session 3; 83.8 too small) | corner alchemy feature |
| Ritual_Altar_01 | ~1875 | hero-adjacent totem |
| Book_01/02, Book_Collections_01, Skull_01, Bottle_02 | prefab-native (real-world sized) | micro-narrative clusters, ~1 per zone |
| Sand_01 (`Sand_01/Sand_01 (2).prefab`) | 211–422 | prefer ONE large wall drift (s≈273, z-squash ~0.63) per room over scattered small patches (session 13); pile against walls/column bases |
| border (`Models/Borders/border.prefab`) | 3.25 | ~3.2 m cornice segment — tile along wall/aisle tops |
| ArcCeiling (`Prefabs/ArcCeiling.prefab`) | 4.07 | barrel-vault arc, one per ~9.7 m of nave |
| Corner_Pillar_01 (gate jamb) | 228.22 | stack two (tier Δy 4.15) = CornelColumnDouble |
| Column_01 (interior pillar) | 2.40 | pedestal-based column, renders correctly at this scale |

**Scale reference:** the live `PlayKit/Player` is ~2.0 m tall — judge sizes against it (wall panels ≈ 6× player, shelf tier ≈ 1.8×). When placing a prefab not in this table, match a comparable item's real-world size against the player, then confirm with a POV shot.

**Colliders (session 9, R034):** interactable-scale props carry a non-convex MeshCollider on the prefab's mesh child (user standard — Table/Chair/Stool/Chest/Barrels/Pottery have them). Rely on prefab-level colliders; flag prefabs that lack one instead of adding per-instance colliders.

**Material value range (session 9, R035):** props that read too bright against the dark tile walls get their material's _BaseColor darkened (Corner_Pillar_01 went 0.94 → 0.48 grey) — fix the material, don't relight the room.

## Surface materials — world-space tiling (taught, session 2)

All big surfaces use `MotoX/Environment/WorldSpaceTiling` (`Assets/Shaders/WorldSpaceEnvironment.shader`) — tiles in world space, so one material fits any mesh size with no UVs, **GPU instancing ON**. The trio in `NewAssets/Materials/Stone_01/`:

| Material | Use | _TilesPerMeter | Key params |
|---|---|---|---|
| `Stone_01TileTest 1` | walls | 0.11 (big blocks) | _BumpScale 1.57, _Smoothness 0.34 |
| `Stone_01TileTest` | floors | 0.40 (2.5 m stones) | _SandSmoothness 0.75 |
| `Ceiling` | ceilings | 0.21 | _Smoothness 0.43 |

Shared accents: `_SandColor (0.72, 0.64, 0.49)`, `_PuddleColor (0.76, 0.81, 0.84)`. Apply these instead of per-size tiled Stone_01 instances for anything new.

## Composition pattern (every dressed room)

Work in four layers, center-out:

1. **Hero piece (rewritten session 12, R041)** — ONE floor-standing piece grounded directly at the room's focal point. NO dais/platform underneath, no flanking crystals, no satellite pedestals, no garnish (user deleted all of these in W25 and grounded the Ritual_Table alone at center). The old "Treasury dais crowned by Azure_Crystal" recipe is retired as a default — it appears only where the user builds it himself. Spawn/traversal halls and objective rooms get NO hero at all (R038/R040).
2. **Secondary ring** — 2–6 satellites orbiting the hero at ⅓–½ room radius, symmetric about the room's door axes: crystal pedestals (`Treasury_Platform_02` + `Crystal_01`), candelabra pedestals, benches facing the hero, guardian statues (`Knight_Armor_01` stand-in).
3. **Wall band** — function furniture along walls, 0.3–0.6 m off the wall face, never inside doorway gaps: shelves, racks, beds, desks, counters, fireplaces (back wall).
4. **Scatter** — lived-in clutter in corners and dead zones: barrels, sacks, pottery, bowls, books; `Candlelight_01` clusters ×2–8 per room (more in hero rooms) distributed so warm light rims the cool crystal glow.

**Layer 2.5 — furniture-as-architecture** (taught, W16; refined session 14, R045): in grand two-story rooms, shelving/furniture builds the room's inner structure. Two-tier shelf walls: stack Book_Shelves_01 (s=190) with **pivot Δy = 3.60** (tier1 pivot = floorY+1.88); runs hug every wall face *including octagon diagonals* (shelf yaw = wall yaw). Freestanding **double-sided aisles** = back-to-back pairs (yaw 270 + 90) offset 0.64 m, rows pitched 2.45–2.47 m. Decorate stack tops: candles (s=160, lit) and potions on the tier-2 top surface light and dress the upper story. One deliberately fallen shelf (toppled rotation, dropped to floor) per zone reads lived-in — never blocking a lane's full width.

**Standard library rooms (not the W16 showpiece) use DISCRETE SHELF BLOCKS instead (session 14, R045):** 3-wide × 2-tier modules (tier pivots floor+1.82 / +5.36, Δy 3.54; in-row pitch ~2.09 m, slightly overlapped), placed to FLANK the doors — ~3 blocks (18 shelves) for a 25×23 room, and that's the whole theme. No desk rings, no benches, no hero, no pedestals; 3 candles at the blocks.

**Circulation beats density**: shelf/furniture fields must preserve the room's crossing lanes (W16 keeps an 8.4 m N-S central lane with the hero table in it, and a 5.7 m E-W lane aligned to the side doors). Verify lanes against the boundary extractor's doorways.

Density calibration: small rooms 10–16 props, standard 20–27, hero rooms 33–41, **architectural showpiece rooms (taught W16) ~125** — most of it repeated shelf modules, not unique clutter. Leave the door axes and the center walking cross clear — playable space first.

**Density follows narrative (taught session 7, R029 — overrides the calibration above when a story beat calls for it):** adjacent rooms want CONTRAST, not uniform targets. The user made W12 Kitchen dense (31 props, food clone-clusters with varied yaws) then stripped W13 Pantry next door to 7 props (3 carts incl. 1 toppled, 1 pottery, a single candle) — a looted-empty beat. Not every room needs a hero (W13 has none), a single candle is a valid mood, and sand patches are optional. Food/goods scatter = clones of one prefab with per-instance yaw variation, piled on one surface.

**When in doubt, UNDER-dress (session 12, R042):** every agent-dressed room the user has finalized was THINNED, never thickened (sole exception: W12 kitchen got denser). User finals run 4–13 objects for formal/ritual/objective/traversal/utility rooms — W25 chapel ended as shell + 1 floor hero + 4 candelabra. Even seating arrays (8 pews) were deleted. Reserve density for lived-in narrative rooms only.

**No placeholder stand-ins (taught session 11, R039):** never keep a prop that approximates a missing asset in a finalized room — the user deleted W2's Boiler "bell", W13's fountain, W11's wash stations. The gameplay marker carries the objective; the missing asset stays on ASSETS.md's shopping list. An empty floor beats a wrong prop.

**Objective-room pattern (taught session 11, R040 — W2 Bell-Signal):** rooms whose purpose is a gameplay objective keep a CLEAR floor: architecture + candles only (4 corner candelabra s=275 inside the columns + one s=160 near the objective wall). No workbenches, racks, chests, or sand — the fight happens around the markers.

**Spawn/traversal-hall pattern (taught session 10, R038 — W1 Intake Cathedral):** entry and spawn-adjacent halls keep an OPEN CENTER — no hero dais, no monument, no center furniture. The room is colonnade + gameplay cover pillars + light perimeter clutter (a few barrels/chest/pottery/cart at the walls). The 4 big candles (s=275) hug the interior columns to light the colonnade lanes; a s=160 pair flanks the main door; corners stay dark. Heroes belong to destination/POI rooms, never the first fight space.

**Utility-room pattern (taught sessions 7+8, R033 — proven across W13 Pantry + W11 Laundry):** utility/service rooms are SPARSE, floor-level, station-free. No workstations, no shelves, no bench rows, no hero dais, no sand. Dressing = a handful of loose thematic goods on the floor (Pile_of_Fabric s=57 laundry piles, sacks with varied yaws, pottery, carts — one may be toppled) + exactly ONE lit candle (s=275). Do not build furniture stations to illustrate the room's function; the theme is told with loose goods.

## Lighting recipe (taught, W16 — apply to every enclosed/roofed room)

- **Candles are the light sources.** Each lighting Candlelight_01 carries a **Point Light: amber RGB(1.0, 0.665, 0.0), live intensity 3.0, range 69.3, shadows NONE** (duplicate from an existing lit candle rather than rebuilding). ⚠ **R048 (2026-07-28): NO shadow casting on candle lights and reflection probes are BAKED, never realtime** — the floor-wide realtime accumulation (22 realtime probes + 140 soft-shadowed lights) caused unrecoverable D3D12 device-removal crashes. Treat the "shadow maps don't fit the atlas" console warning as a stop sign.
- Distribution: 4 large candelabra (s=275) marking the room's inner rectangle corners, large ones at lane ends, s=160 candles on shelf-stack tops for the upper story, s=150–160 on tabletops. W16 uses 14 lit candles for a 28×28 hub.
- **One Reflection Probe per room**: realtime, box projection ON, resolution 128, **positioned at the room's XZ center** at mid-height (~3.3 m above floor), box sized to the full interior (W16: 31.9×20.1×30.4) with the box `center` offset tuned so the volume hugs the room bounds.
- **One LightProbeGroup per room** (W16: 516 probes) — the probe grammar:
  - **2×2×2 m cell clusters threaded along the open walk lanes and voids** — a probe must never sit inside a shelf, wall, or prop (validate with the toolkit's probe check).
  - **Six height bands** so baked interpolation captures light *and* dark: ~0.2 m above floor (candle pools), 1–2 m (flame/eye), ~4 m (mid dark band), 5.5–7.7 m (upper gallery + shelf-top candles), 8.7–9.7 m (ceiling shadow), sparse vault sentinels (~11 m).
  - Densest where lighting changes fastest (candle pools, lane crossings); dark corners still get probes — capturing darkness is the point, or dynamic objects (players, mobs) will glow wrong when they walk out of a pool.
- Once ceilings close a room off, the directional light dies there — the candle rig + probes carry it. Judge only via POV screenshots, never top-down.

## The feeling (confirmed by W16 final POV review — session 2)

Dark enclosed stone interior; candle light falls in **warm pools** with real shadow between them — don't flood-light. Two-story furniture walls vanish upward into shadow; the barrel vault catches only faint light. Gold reads as **glints**, not surfaces: pedestal bases, cornice line, candle flames. Door openings read as bright portals that pull the eye. Navigation is candle-guided: pools should chain along the walk lanes toward the room's focal table/hero. Sand/rubble mounds anchor architecture (against column bases, aisle ends) — age, not litter. Aim for this in every room: dense but navigable, dark but readable.

## Finishing a tall room (session-2 checklist additions)

- [ ] Border cornice tiled along every tall wall top (~8 m) and shelf-aisle top (~9 m) — no bare panel tops, and the line runs **continuously across door headers** (don't stop at openings).
- [ ] **`bordersroof` rows** along the inner edges of the flat side-ceilings where they meet the vault (pivot 0.4 m below the flat underside, pitch ~3.2 m, yaw 90) — the ceiling-void edge must be trimmed.
- [ ] Layered ceiling: gallery 8.5 m → flat aisle 9.5 m → central vault 11.9 m (ArcCeiling arcs over the nave).
- [ ] **DoorStructure group at every door/pathway**: Stone_Archway_01 (s=250, scaled to opening width) + double-stacked Corner_Pillar jambs, grouped under one `DoorStructure*` parent per door.
- [ ] **Seam-cover pillars**: full-height Corner_Pillar_01 (s 216.4/216.4/632.6) at every 45° wall junction, wall end beside doors, and 1–2 into each corridor mouth — no visible panel seams or room-to-hall cuts.
- [ ] World-tile materials on all big surfaces (walls/floor/ceiling), GPU instancing on.
- [ ] Interior columns with a sand mound against 1–2 bases.

## Placement standard (applies to every prop)

- **Upright = rX 270, rZ 0, yaw-only** (these are Z-up assets; confirmed across all 125 taught placements). The mixed rotations in pre-teaching rooms (`[0,0,90]`, `[90,0,0]`) are wrong — do not copy them.
- **Auto-orient:** detect flattest mesh face, rest it on the floor, then Y-yaw only (project standard — models have inconsistent up-axes). Snippet in [../toolkit.md](../toolkit.md).
- **Grounded:** renderer-bounds min.y == floor top Y (from the boundary extractor).
- **Character-relative scale:** judge against the 0.91 m character. Benches seat-height ~0.45 m, tables ~0.75 m, shelves 1.8–2.4 m, hero crystals 2–4 m.
- **Facing:** functional props face their user-space (benches→hero, desks→room, beds head-to-wall); symmetry pairs mirror across a door axis.
- ⚠ **Flat props** (curtains, flags, tapestries, banners) fail auto-orient (lie flat) — skip them until wall-mount handling exists.

## Asset palette by theme (`Assets/Levels/Dungeon1/NewAssets/...`)

| Theme | Pack / key prefabs |
|---|---|
| Crystal/shrine | `Azure_Crystal_01` (hero), `Crystal_01` (pedestal), `Mineral Crystal Room/` (Ore_Veins, Broken_Rocks, Digging, Wheelbarrow) |
| Treasure/loot | `Treasure Room/` — Treasury_Platform_01/02, Crown_01/02, Jewelrys, Pile_of_Gold |
| Alchemy/apothecary | `Alchemy Laboratory/` — Alchemist_Shelf, Alchemy_Set, Boiler, Distillation_System, Potion_Bottle_01/02, Production_Table_01/02 |
| Ritual/chapel | `Ritual Room/` — Ritual_Altar, Ritual_Table, Sacrifice_Table, Blood_Symbol, Magic_Book, Incense_Holder |
| Library/records | `Library/` — Book_Shelves, Ladder_Shelving, Lectern, Scribes_Desk(_Double), Map_Desk, Wall_Map, Topglobe, Scrolls, Parchments |
| Infirmary/living | `Infirmary_Models/` — Bed_01/02, Chest_01, Nightstand, Chair, Stool, Table, Stretcher, bottles/bowls, Pile_of_Fabric, Sack |
| Dining/kitchen | `Dining_Hall/` — Dining_Table, Chair_02, Chandelier, Fireplace, Serving_Car, Plate_Set, Wine, Rotten_Food |
| Court/meeting | `Courtroom/` + `Meeting Room/` — Justice_Bench (the universal bench/pew), Court_Table, Round_Table, Leader_Seat, Chessboard |
| Martial | Models/: Armory_Rack, Sword_Rack, Knight_Armor/Helmet, shields, Toolbench_01–03, Barrels, Pottery_Trio, Candlelight, Skull, Fallen_Warrior, Wrapped_Body |
| Servants | `Servants Quarters/` — Bunk_Bed, Cabinet, Suitcases, Outfit, Shoes |
| Floor dressing | `Sand_01/Sand_01 (2)` — sand/debris mounds overlaying floor tiles (new, taught W16) |

**Blacklist / gotchas:** `Crystal_02` scales into a blob (never a hero); `Prefabs/Column_01` renders magenta and lies horizontal under auto-orient (use `Corner_Pillar_01`); `Tample Models/` are raw un-prefabbed .glb (import properly before use).

## Stand-in → real asset map (art pass targets, see `AI/ASSETS.md` shopping list)

| Stand-in in scene | Represents | Where |
|---|---|---|
| Treasury_Platform + Azure_Crystal | water basin/fountain/pool | W4, W10, W13, W17, W21, W26, W30, W31 |
| Boiler_01 | bell | W2, W29 |
| Knight_Armor_01 | stone statues | W21–W32 niches |
| Bouquet_01 | trees/hedges/flower beds | W26, W28, W30 |
| Blood_Symbol_01 | floor rune circles | W19, W22, W25 |
| Topglobe_01 | telescope | W9 |
| big Azure_Crystal | sacred tree (W28), tiered chained relic (W20), monolith (W22) | |

When real assets arrive: match position/yaw/footprint of the stand-in, then re-verify with POV shots.

## Per-room dressed themes

`AI/DUNGEON.md` §"Required Props per Room" is the authoritative per-room recipe list (W1–W32, incl. the theme↔group-name divergences like `W25_SurgeryTheatre` dressed as a chapel). Read it before touching a room; update it after.

**W16 is the gold standard** (user-taught 2026-07-13): redesigned from "grand pharmacy" into a **two-story Grand Library hub** — full shelf architecture, lit-candle lighting rig, reflection probe, sand floor patches, tabletop vignettes, one fallen shelf. Reference its live state (`Props_Dressing/W16_GrandInfirmary_Hub`) and `../LEARNED.md` R001–R013 before dressing any comparable room. Ceiling pattern is unfinished — don't replicate it yet.

## Reference-image aesthetic checklist (score every room before "done")

- [ ] One clear focal glow (hero crystal) — brightest, tallest, centered in doorway sightlines.
- [ ] Rune circle / dais grounding the hero (not floating on bare floor).
- [ ] Warm-vs-cool contrast: candle clusters (warm) rimming crystal glow (cool teal).
- [ ] Gold/brass accents on platforms, trim, candelabra — sparingly, at focal + secondary ring only.
- [ ] Mid-ground clutter (crates/barrels/urns) breaking up empty floor, hugging walls/pillars.
- [ ] Silhouettes readable at player eye height — no prop soup; each layer (hero/ring/wall/scatter) distinct.
- [ ] Doorways clear; cover sightlines from the blockout preserved (don't fill cover lanes with clutter).
- [ ] Symmetry where the architecture is symmetric; deliberate asymmetry (collapsed shelf, spilled sacks) for lived-in rooms.

## Player-camera verification workflow (mandatory before declaring a room done)

The game camera is third-person, ~2.0–3.5 m behind a 0.91 m character — rooms are experienced at eye level, not top-down.

1. Run the boundary extractor → get doorways + floor Y.
2. For each doorway: probe camera at the doorway midpoint, **eye height = floorY + 0.85 m**, looking down the door axis into the room. Screenshot via `manage_camera`.
3. One more from room center looking at the hero, and (hero rooms) one from any balcony marker.
4. Evaluate against the checklist above; fix, re-shoot, repeat.
5. Full recipe + code in [../toolkit.md](../toolkit.md) §POV renderer.
