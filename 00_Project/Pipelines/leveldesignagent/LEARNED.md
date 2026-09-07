# LEARNED — user-taught placement rules

Rules taught by the user via the teaching loop (SKILL.md §Teaching loop). **These override module defaults.** Append only; never rewrite history — supersede with a newer rule and mark the old one `SUPERSEDED`.

## Rule format

```
RULE <id> | scope: global | theme:<name> | prop:<prefab> | room:W#
<statement — plain language, with numbers (wall offset, spacing, facing, scale)>
Evidence: <date>, room(s), snapshot diff files
Status: active | promoted-to:<module file> | superseded-by:<id>
```

Example (illustrative only):
```
RULE 001 | scope: theme:chapel | prop:Justice_Bench_01
Pews face the altar dais in 2 columns × N rows; row spacing 1.4 m, column aisle 2.2 m,
back row ≥ 1.0 m from rear wall, all yaws exactly aligned to the altar axis.
Evidence: 2026-07-XX, W25, snapshots/W25_pews_before/after.json
Status: active
```

## Promotion

When a rule holds across ≥2 rooms, fold it into `modules/art-polisher.md` (or gameplay/blockout module if applicable), set `Status: promoted-to:...`, and keep the entry here as the evidence trail.

---

## Rules

### Session 1 — 2026-07-13, W16 Grand Infirmary Hub redesigned as a two-story Grand Library
Evidence for all rules below: `snapshots/W16_GrandInfirmary_Hub_teach_before*.json` vs `..._teach_after.json` / `..._teach_after_walls.json`. 33 props → 125; walls rebuilt; real lights added. User: "this is the proper way of usage for our project."

```
RULE 001 | scope: global (props)
Upright = rX 270, rZ 0, yaw-only facing. All 125 placed props follow this (Z-up assets).
The pre-teaching mixed conventions ([0,0,90], [90,0,0]) are WRONG — never copy old rooms' rotations.
Exception: deliberately toppled props (see R010) and surface-scatter tilts (R008).
Status: active

RULE 002 | scope: global (props)
Canonical uniform scales — these prefabs are near-zero native size; correct usage is a large
uniform scale. Measured canon: Book_Shelves_01=190, Court_Table_01=216.74, Table_01=91.22,
Barrels_01=109.16, Candlelight_01=150 (tabletop) / 160 (feature) / 243–275 (floor candelabra),
Mortar_with_Pestle=15.6–16.7, Potion_Bottle=14.7–25.9, Scrolls_01=35.31, Alchemy_Set_01=167.7
(floor-standing), Sand_01=211–422, Ceiling_02=4.07. Court_Table at 460 (old hub) was oversized
— the corrected 216.74 puts its tabletop at ~1 m over floor (character-relative).
Status: active

RULE 003 | scope: pattern: shelf-architecture (two-story rooms)
Furniture can BE architecture. Two-tier shelf walls: stack Book_Shelves_01 (s=190 →
2.59 w × 3.63 h × 0.74 d) with pivot Δy = 3.60 (tier1 pivot −3.79 = floorY+1.88, tier2 −0.19).
Wall-lining runs hug every wall face including octagon diagonals, shelf yaw = wall yaw
(45/90/135/178.5/225/270/319). Freestanding double-sided aisles = back-to-back pairs
(yaw 270 + yaw 90) offset 0.64 m on the facing axis, rows pitched 2.45–2.47 m.
Status: active

RULE 004 | scope: pattern: circulation (hub rooms)
Shelf/furniture fields must preserve the room's crossing lanes: N-S central lane ~8.4 m
(aisle faces at x −4.2 / −12.6, hero table centered in it) and an E-W lane ~5.7 m
(row gap z −2.82 → −8.56) aligned with the side doors. Gameplay flow beats density.
Status: active

RULE 005 | scope: global (lighting)
Candles are the light sources: Candlelight_01 carries a Point Light — amber RGB(1.0, 0.665, 0.0),
intensity 6.69, range 69.3, soft shadows. Distribution in W16 (14 lit candles): 4 large (s=275)
at the aisle-corner rectangle (x 3/−19 × z 7/−13.5), large ones at lane ends, s=160 on top of
the tier-2 shelves (y≈1.58 = stack top) to light the upper story, s=150–160 on tabletops.
Status: active

RULE 006 | scope: global (lighting)
One Reflection Probe per enclosed room: realtime, box projection ON, box sized to the room
(W16: 31.9 × 20.1 × 30.4), positioned mid-air near room center. Lives under Props_Dressing/<room>.
Status: active

RULE 007 | scope: pattern: tabletop-vignette
Repeatable dressed-surface set: table (Table_01 s=91 or Court_Table s=217) + Mortar (s≈16) +
Potion_Bottle (s≈15–26) + Scrolls (s=35) + Candlelight (s=150–160), all pivots at surface height
(props sit ~0.6 above Table_01 pivot, ~0.4–1.0 above Court_Table pivot).
Status: active

RULE 008 | scope: prop: Scrolls_01 (and similar flat scatter)
Scrolls lie naturally: rX ≈ 288.6 (tilted off the 270 upright) with rZ varied per instance
(120/133/180) so copies don't read as clones. Scatter on floors along aisles as well as desks.
Status: active

RULE 009 | scope: pattern: floor-dressing
Sand_01 patches ("Sand_01/Sand_01 (2).prefab", new asset) break up clean floor: s=211–422,
pivot raised 0.5–1.1 above floor so the mound overlays the tiles, scattered in aisles/corners
(8 patches in W16). Vary the yaw when duplicating (W16's all share rY=18.65 — flagged).
Status: active

RULE 010 | scope: pattern: lived-in-accents
Deliberate imperfection: one fallen shelf in a walk lane (Book_Shelves rot [343.7, 172.6, 180],
dropped to floor y=−5.12) reads as age/struggle. Use sparingly — one per room zone, never
blocking a circulation lane's full width.
Status: active

RULE 011 | scope: blockout (walls)
Octagon/diagonal walls are real diagonals: Wall_01 rotated to the face angle (rY 45/315) and
X-stretched (scale.x ≈ 2.3), not stair-stepped segments. Long straight runs are consolidated
into fewer X-scaled pieces (scale.x 0.38–2.36) instead of many unit segments.
Status: active

RULE 012 | scope: blockout (walls, two-story sections)
Stacked wall tiers for tall/two-story sections: 3 vertical rows of Wall_01 (scale.y 0.88 ≈ ⅓
height) at pivot y −5.88 / −1.94 / +2.05, and NEGATIVE scale.z (−0.364) flips the clean face
inward. Room's cover pillars may be grouped under _Blockout/Walls/<room>/Pillars.
Status: superseded-by:014 (user replaced the stacked-Wall_01 experiment with WorldBasedTileWall panels)

RULE 013 | scope: pattern: ceiling (in progress — do not generalize yet)
Ceiling_02 slabs (s=4.07 → ~11.7 × 10.5 m) at pivot y 6.25 (underside ~4.2), rows pitched
~9.7 m on Z, parented under Props_Dressing/<room>. User: roof/ceiling NOT finished — await
the completed pattern before applying elsewhere.
Status: superseded-by:017 (ceiling pattern completed in session 2)
```

### First application — 2026-07-14, W15_TriageHall rebuilt by the agent using R001–R023
All rules held. Field notes: (1) Sand_01 at W16 scales (211–422) overwhelmed W15's open floor — scaled ×0.35–0.5 and pushed to wall corners/column bases before it read right → sand scale must follow room openness, not the canon table blindly. (2) Prefabs like Bed_01/Stretcher_01/Boiler_01 instantiate at real-world size (scale ≈1) — the big-scale canon applies only to the tiny-native prefabs; always measure instantiated bounds and normalize to target size instead of assuming. (3) Cloning W16 scene objects (borders, arcs, tile ceilings, lit candles, sand, columns) is the fastest correct path — materials and Point Lights carry over. (4) Production tables needed ×1.5 over their legacy scale to read right next to the 2 m player.

### Session 5 — 2026-07-14, seam-cover pillars (user closed W15 gaps)

```
RULE 025 | scope: pattern: seam-cover-pillars
Visual gaps at 45°-wall junctions and at wall-end→corridor transitions are closed with
FULL-HEIGHT Corner_Pillar_01s, not stacked tiers: non-uniform scale (216.4, 216.4, 632.6)
(rX=270, so Z is vertical → one pillar spans the full ~12 m wall), rot (270, 180, 0), pivot
centered vertically (y ≈ floor + 6.0). Place at:
1. Every diagonal-to-straight wall junction (the 45° seams where tile panels meet).
2. Wall-segment ends flanking door openings.
3. CONTINUING into the adjoining corridor mouths (1–2 per hall side) so the room-to-hall
   connection reads as one built structure, not a cut.
Parent: root "Corner_Pillars" group. Evidence: 15 pillars ringing W15 placed by user
(e.g. (5.86, 25.1)/(5.86, 36.6) E-wall diagonal seams; (−12.2/−3.8/−16/0, 47.84) N hall).
Prefer this over two-tier jamb stacks for seam coverage; keep the stacked tiers only inside
DoorStructure groups (they are door decor, height 8.1 m, not full-wall seam covers).
Status: active
```

### Session 3 — 2026-07-14, user corrections on the agent-built W15
Evidence: diff `W15_TriageHall_teach_before.json` vs live scene after user pass.

```
RULE 024 | scope: corrections (scales + seating, from user's W15 pass)
- Scrolls_01 canon corrected: s=25 (35.31 read too big), seated flush on the floor
  (my +0.13 float was wrong — scatter props sit ON the surface, bounds-min at surface Y).
- Production_Table canon corrected: s=112 (my 78 was still small next to the 2 m player).
- Ritual_Altar_01 canon: s≈1875 as a hero-adjacent totem (1500 read small).
- Potion bottles on tables: seat by measured table-top bounds, not assumed height
  (user dropped mine 0.41 — always re-measure the actual surface top).
- Fallen/toppled props must CONTACT the floor (user sank my fallen shelf 0.75 and added
  +10° tilt) — a floating "fallen" prop breaks the illusion; verify bounds touch.
- Hero dais doesn't always need a floating Azure_Crystal — in W15 the user removed it;
  the Treasury_Platform's own chest + flanking spires carry the focal point. Vary heroes
  per room; the crystal is one option, not a mandate.
- Micro-narrative props finish a room: Book_01/Book_02/Book_Collections_01/Bottle_02/
  Skull_01 ×3 scattered on desks/shelves/floor — small story beats, ~1 cluster per zone.
- Distillation_System_01 canon: s=125 (83.8 read small as a corner feature).
- Pedestal-crystal pairs (Treasury_Platform_02 + Crystal_01) are NOT default filler — user
  removed both from W15. Reserve them for crystal/shrine-themed rooms; in functional rooms
  (triage, kitchen, barracks) they clutter without adding identity.
Status: active
```

### Session 2 — 2026-07-14, W16 finished: world-tiled surfaces, border cornices, vaulted ceiling, gate columns
Evidence: `snapshots/W16_GrandInfirmary_Hub_teach2_after.json` (192 objects), live scene, POV shots. User: walls reusable everywhere, tile shader GPU-instanced, borders "so it looks authentic", arc ceiling "because the room is big", flat side ceilings, CornelColumnDouble gate columns, Pillar_Hub interior columns.

```
RULE 014 | scope: blockout (walls) — THE wall system going forward
WorldBasedTileWall = a plain Unity Cube (no prefab) + a WorldSpaceTiling material.
Standard panel: scale (13.32, 11.92, 1.00) = 13.3 m wide × 11.9 m tall × 1 m thick, pivot at
wall center (y = floorY + ~3.8 for a −5.67 floor with panel spanning −7.8..4.1).
Narrower fills: 11.47 / 7.41 / 5.92 / 3.46 wide; low header over doors: scale.y 4.18;
thin partition: scale.z 0.37. Rotate the cube to the wall face angle (45/88/135/178/224/270/
321 in W16's octagon) — diagonals are single rotated panels. Because tiling is world-space,
ONE material fits every panel size with zero UV work — reuse it everywhere, GPU instancing ON
(enabled 2026-07-14 on all three mats). Name panels "WorldBasedTileWall".
Status: active

RULE 015 | scope: global (materials)
Surface material trio (all `MotoX/Environment/WorldSpaceTiling`, shader at
Assets/Shaders/WorldSpaceEnvironment.shader, all in NewAssets/Materials/Stone_01/):
- Walls: "Stone_01TileTest 1" — _TilesPerMeter 0.11 (big blocks), _BumpScale 1.57, _Smoothness 0.34
- Floor: "Stone_01TileTest"  — _TilesPerMeter 0.40 (2.5 m stones), _SandSmoothness 0.75
- Ceiling: "Ceiling"         — _TilesPerMeter 0.21, _Smoothness 0.43
Shared accents: _SandColor (0.72, 0.64, 0.49), _PuddleColor (0.76, 0.81, 0.84). GPU instancing ON.
Status: active

RULE 016 | scope: pattern: border-cornice (extended 2026-07-14 session 3)
"border" prefab (NewAssets/Models/Borders/border.prefab, Tripo mesh) at s=3.25 → ~3.2 m
segments, tiled end-to-end. TWO distinct lines, both mandatory:
1. WALL CORNICE ("border"): along every wall top at pivot y ≈ 8.0–8.2 above floor, yaw =
   wall yaw — INCLUDING over door headers (run the line continuously across the opening,
   pitch ~2.9–3.2 m; a cornice that stops at a door reads unfinished).
2. ROOF BORDER ("bordersroof"): rows along the INNER EDGES of the flat side-ceilings where
   they meet the central vault — pivot 0.4 m below the flat-ceiling underside (W16: y 3.40,
   edges x −13.0/−2.94; W15: y 5.41, edges x −12.37/−3.64), pitch ~3.19 m, yaw 90 on both
   rows. This trims the ceiling-to-vault junction. Name these "bordersroof" (user convention).
Never leave a tall tile-wall top or a ceiling-void edge bare.
Status: active

RULE 017 | scope: pattern: ceiling-architecture (completed, supersedes R013)
Big rooms get a layered vault: CENTRAL NAVE = ArcCeiling prefabs
(NewAssets/Prefabs/ArcCeiling.prefab, s=4.07, one per ~9.7 m of nave, pivot y ≈ 11.9 above
floor) forming a barrel vault down the spine. SIDE AISLES = "WorldBasedTileCeiling" flat cubes
(scale 11.27 × 30.18 × 1.83, rX=90, one per aisle strip, underside ~9.5 above floor) with the
"Ceiling" world-tile material, plus kit Ceiling_01 panels (s=239.86, y ≈ 8.5 above floor) as
the lower aisle ceiling over the shelf galleries. Height layering (aisle 8.5 → flat 9.5 →
vault 11.9+) is deliberate architecture — arc over the void, flat over the aisles.
Status: active

RULE 018 | scope: pattern: gate-ensemble → "DoorStructure" (upgraded 2026-07-14 session 4)
Every door/pathway gets a **DoorStructure group** (user-taught, W16): parent GameObject named
"DoorStructure*" containing:
- **Stone_Archway_01** (s=250, rX=270, yaw across the passage direction) spanning the opening
  at the wall line — base embedded ~0.05 into the floor; at s=250 the arch is ~4.8 m wide ×
  4.36 m tall. Scale proportionally to fit narrower doors (W15: s=208 for a 4 m door, s=172
  for 3.3 m).
- **Two jamb stacks** flanking the arch ends, each = 2 stacked Corner_Pillar_01
  (s=228.22, rX=270, tier pivots at floor+1.82 and floor+5.97, Δy 4.15).
Group the whole ensemble under one parent per door (keeps doors editable as units).
Legacy: root "Corner_Pillars" group holds older ungrouped jambs floor-wide.
Status: active (supersedes the ungrouped-jambs form)

RULE 019 | scope: pattern: interior-columns
Freestanding interior columns: Column_01 at s=2.40 (prefab scale — renders correctly here;
pedestal base with gold trim) under Walls/<room>/Pillars, ~5 for the hub interior. Pile a
Sand_01 mound against 1–2 column bases (rubble anchors the architecture; see POV shot —
sand + column reads as ancient debris).
Status: active

RULE 020 | scope: global (measuring stick — UPDATE)
The live PlayKit Player measures ~2.0 m tall (scale 1.0). Room proportions vs player:
wall panels 11.9 m ≈ 6× player; shelf tier 3.63 m ≈ 1.8×; border cornice at 4–4.5× eye level;
gate column tiers 4.15 m ≈ 2×. Use the in-scene PlayKit/Player for scale checks, not the old
0.91 m reference (which described the earlier NewRoom_01 character).
Status: active

RULE 022 | scope: global (lighting — reflection probe placement, refines R006)
The room's Reflection Probe sits at the room's XZ CENTER, floated at mid-height (~3.3 m above
floor in W16); its box (boxProjection ON, realtime, res 128) covers the full interior —
size (31.9, 20.1, 30.4) for the 28×28 hub — with box `center` offset so the volume hugs the
actual room bounds even if the transform isn't perfectly centered. One per room, under
Props_Dressing/<room>.
Status: active

RULE 023 | scope: global (lighting — light probes)
One LightProbeGroup per room under Props_Dressing/<room> (W16: 516 probes). Placement grammar:
- Probes arranged as 2×2×2 m CELL CLUSTERS threaded along the OPEN walk lanes and voids —
  never inside shelves/walls/props (W16 cells sit in the aisle gaps at x ≈ −11.8/−9.8,
  −8.7/−6.7, −4.5/−2.5 and in the row gaps between shelf rows).
- Vertical coverage spans SIX height bands so interpolation captures both lit pools and dark
  zones (user rule: probes must sample light AND dark spots): ~0.2 m above floor (floor candle
  pools), ~1–2 m (flame/eye height), ~4 m (mid dark band), ~5.5–7.7 m (upper gallery /
  shelf-top candles), ~8.7–9.7 m (ceiling shadow), sparse sentinels in the vault (~11 m).
- Densest where light changes fastest (near candle pools and lane crossings); dark corners
  still get probes — a dark sample is data, not waste.
Status: active

### Session 6 — 2026-07-14, Floor_W28_Chapel room-template prefab (user-built)
Evidence: `Assets/Levels/Dungeon1/NewAssets/Floor_W28_Chapel.prefab` (131 objects), user replaced W28's kit floor with it. Names normalized to convention by agent 2026-07-14.

```
RULE 026 | scope: pattern: room-template-prefab — THE room packaging going forward
A whole room ships as ONE prefab (user model: Floor_W28_Chapel.prefab — the W15 shell copied,
re-dressed as a treasure chapel). Contents = complete architecture + dressing + lighting:
- FLOOR: a Unity Plane primitive (scale 2.66 → ~26.6 m square) with the world-tile floor
  material "Stone_01TileTest" — replaces kit Floor_01 grids. Named Floor_<room>.
- WALLS: 9 WorldBasedTileWall cube panels (R014 sizes: 13.32/11.64/6.41/4.43/3.38 wide ×
  11.92 tall; diagonals = single panels at rY 54/217/321; door header scale.y 4.27).
- ROOF: 3 ArcCeiling_02 barrel-vault arcs down the spine (pitch ~9.7 m, pivot ~12.1 above
  floor) + 2 flat WorldBasedTileCeiling slabs rX=90 (11.27 × 30.18 × 1.83, "Ceiling" mat)
  over the side aisles + 2 VERTICAL Ceiling-mat panels closing the vault ends (gable caps,
  11.27×5.75 and 9.65×2.96) — new element vs W16.
- BORDERS: wall cornice "border" s=3.25 at ~9.5–9.9 above floor following every wall yaw;
  roof-edge "bordersroof" rows at inner flat-ceiling edges, segments z-STRETCHED
  (scale 3.37, 3.25, 9.86 — deeper profile than the s=3.25 cornice), pitch ~3.26 m.
- COLUMNS: 4 interior Column_01 (wrapper s=2.40, child 200) in a 10×8.7 m grid.
- DOORS: DoorStructure_N/S/W wrappers, Stone_Archway_01 child s=250 inside wrapper scale
  2.50–2.63 → grand ~12 m × 11.4 m gates (much larger than W15's s=208 arches).
- LIGHTING: 7 Candlelight_Lit_N (i=3.0 amber point lights) + ReflectionProbe_<room>.
Naming canon inside room prefabs: WorldBasedTileWall / WorldBasedTileCeiling / ArcCeiling_02
/ border / bordersroof / DoorStructure_<dir> / Pillar_<room> / Floor_<room> /
ReflectionProbe_<room> / Candlelight_Lit_N / Sand_N.
User: future wall/floor updates across rooms will reuse these pieces — this prefab is the kit reference.
Status: active
```

### Session 7 — 2026-07-15, user finished W12 Kitchen + W13 Pantry (corrections on the agent's R026 conversions)
Evidence: `snapshots/W12_Kitchen_rebuild_before.json` / `W13_Pantry_rebuild_before.json` (agent state) vs `..._teach_after.json` (user-final). W12: 22 → 31 props, pillars 8 → 4. W13: 25 → 7 props (!), pillars 10 → 2.

```
RULE 027 | scope: blockout (seam pillars — REFINES R025)
Full-height Corner_Pillar seam covers belong ONLY at door flanks, and only at the doors that
deserve the framing (W12 kept both doors' flanks = 4; W13 kept only its S door pair = 2).
Plain 90° room corners get NO pillar — tile panels butt cleanly there; the user deleted every
corner pillar the agent placed in W12/W13. R025's junction/corner list applies to 45°-diagonal
seams and corridor mouths (W15-style), not to simple rectangular rooms.
Status: active

RULE 028 | scope: theme: kitchen (canon scales + composition, W12-final)
- Fireplace_01 s=200 against a solid wall (137 read small); Boiler_01 s=150 pulled off the
  hearth into the room center as a freestanding cauldron feature.
- Table_01 canon s=120 (supersedes the 91.22 value inside R002) — prep tables CLUSTER in an
  L-shape (3 tables at yaws 180/120.8/90) rather than standing isolated.
- Food scatter = clone-clusters with per-instance yaw variation: Loaf_Bread ×4 piled on one
  table (yaws 332/83/83/199), Rotten_Food_01 ×3 + Rotten_Food_02 (s=50) spread across the
  cluster, Plate_Set s=24. Pottery_Trio s≈58.
- One Serving_Car of the three is deliberately TOPPLED — full 3-axis tumble, e.g.
  rot (8.9, 230.5, 190.6), resting on the floor (extends R010's fallen-prop rule to carts).
- Candles sit at lane edges near the action, not on a symmetric corner grid; 3 suffice.
Status: active

RULE 029 | scope: composition (density follows narrative — REFINES art-polisher density targets)
Adjacent rooms want CONTRASTING density, not uniform targets: the user made the kitchen dense
(31 props) and then STRIPPED the pantry next door to 7 (3 carts incl. 1 toppled, 1 pottery,
1 single candle, probes) — a looted-empty beat right beside the busy room. He deleted the
agent's entire shelf ring, the fountain hero, barrels/chests/sacks, 3 of 4 candles and all sand.
Corollaries: not every room needs a hero (extends R024) — W13 now has none; a single candle
in a dark room is a valid mood; sand patches are optional, not per-room mandatory.
Status: active

RULE 030 | scope: pattern: bordersroof (depth tuning)
bordersroof rows are tuned PER SIDE: the profile's z-scale stretches (9.86 → 11.71) and the row
shifts toward the wall (W12 north row z 23.7 → 24.38) so the trim visually meets the flat
side-ceiling edge. Don't assume the two rows of a room are symmetric — check each against its
flat-ceiling edge.
Status: active
```

### Session 8 — 2026-07-15, user finished W11 Laundry (corrections on the agent's R026 conversion)
Evidence: `snapshots/W11_Laundry_rebuild_before.json` (agent) vs `W11_Laundry_teach_after.json` (user-final). Props 24 → 10, pillars 7 → 2, one new wall panel.

```
RULE 031 | scope: blockout (oversized openings — partial in-fill under a grand arch)
When a blockout opening is wider than the corridor it serves, do NOT invent full-height walls
and do NOT leave the whole span open. Keep the full-width grand arch as a decorative frame and
fill the dead span with a PARTIAL-HEIGHT panel tucked under/behind it:
W11's 9.2 m west arch kept, plus a WorldBasedTileWall scale (4.21, 7.52, 1.0) at pivot
y = floor − 2.27 + 3.4 (bottom sunk below floor, top ≈ 7.2 m ≈ the arch crown) covering the
southern 4.2 m; the live passage is the remaining ~6 m aligned to the corridor. The door
header above spans the full opening as before. Reads as a partially bricked-up archway.
Status: active

RULE 032 | scope: blockout (seam pillars — pragmatic-only, REFINES R027)
Pillars are placed only where a seam ACTUALLY shows, not systematically: W11 kept 2 of the
agent's 7 — one at the SE corner where a short (2.65) panel makes a visible junction, one at
the west door's live flank with its yaw TURNED TO FACE the opening (rot 270,0,0 — not the
blanket 270,180,0). The S door kept NO flank pillars: DoorStructure arch + header already
frame it. Procedure: after building panels, inspect each corner/junction (POV or bounds
check) and pillar only the seams that read; orient the pillar's face toward the room/opening.
Status: active

RULE 033 | scope: theme: utility/service rooms (CONFIRMS + extends R029 — holds across W13 + W11)
Utility rooms (pantry, laundry — likely baths/waste too) are SPARSE, floor-level, and
station-free: no furniture workstations, no shelves, no benches, no hero, no sand.
Dressing = a few thematic floor props + ONE lit candle:
- W11 Laundry: Pile_of_Fabric_01 s=57 ×3 scattered ON THE FLOOR (not staged on furniture),
  Sack_01 ×3 (varied yaws), Pottery_Trio s=44, 1 candle s=275.
- W13 Pantry: 3 serving carts (1 toppled), pottery, 1 candle.
The agent's instinct to build wash-stations/shelf-rings/bench-rows in utility rooms is wrong —
the user deleted every station in both rooms. Theme is told with loose goods on the floor.
Status: active (R029 pattern now proven across 2 rooms — folded into art-polisher)
```

### Session 9 — 2026-07-15, project-standard updates found in the working tree (not a room pass)
Evidence: git diff on prop prefabs + Corner_Pillar_01.mat; new `Models/Crystal_03/` import; scene state.

```
RULE 034 | scope: global (physics/collision)
Interactable-scale props carry a MeshCollider ON THE PREFAB (non-convex, added to the mesh
child) — the user added them to Table_01, Chair_01, Stool_01, Chest_01, Barrels_01,
Pottery_Trio_01. When dressing, rely on prefab-level colliders rather than adding per-instance
ones; when a cover/bump-scale prop prefab lacks a collider, flag it (or add it at the prefab,
not the instance). Purpose: props are physical cover in the extraction PvPvE loop.
Status: active

RULE 035 | scope: global (materials — value range)
Bright near-white prop materials get knocked down to the dungeon's value range: user darkened
Corner_Pillar_01 _BaseColor 0.94 → 0.48 grey so the full-height seam pillars sit with the dark
tile walls instead of glowing against them. Check any prop that reads too bright in a POV shot
— fix the MATERIAL's base color, don't relight the room around it.
Status: active

RULE 036 | scope: asset: Crystal_03 (new hero crystal) + W28 installation
Crystal_03 (Models/Crystal_03/, Meshy azure spire gen 0714110214) is the imported replacement
for the template's missing hero: user placed it s=250 at W28's center (scene root level,
(−8.1, 0.3, −57.4)). The Floor_W28_Chapel room-template INSTANCE now sits at W28's real socket
(content center ≈ (−8.5, −58.5)) — the treasure-chapel room is installed in the dungeon.
Gotchas: Crystal_03.mat has _EMISSION OFF though an emission texture ships with it (same
family as Azure_Crystal_01); the template prefab still contains the dead
"Crystal_03 (Missing Prefab guid f4287b9f...)" node — clean/replace it inside the prefab when
the user approves.
Status: active
```

### Session 10 — 2026-07-15, user finished W1 Intake Cathedral (corrections on the agent's conversion)
Evidence: agent build (this transcript / `W1_IntakeCathedral_rebuild_before.json`) vs `W1_IntakeCathedral_teach_after.json`. Props 25 → 13, pillars 16 → 15 (+ per-pillar yaw tuning), +1 wall panel.

```
RULE 037 | scope: blockout (corridor-owned faces inside converted rooms)
When part of a room's interior is bounded by a CORRIDOR-owned kit wall (Corridors/GapFill
groups), overlay a tile panel on the room side — the user added WorldBasedTileWall
(8.17 × 11.92) at W1's NE alcove north face over the corridor's kit Wall_01. After converting
a room, sweep ALL interior faces for visible legacy kit surfaces regardless of which group
owns them; the frozen-geometry rule protects the corridor's geometry, not its looks.
(A pillar whose seam the new panel absorbs gets deleted — pillar count follows the panels.)
Status: active

RULE 038 | scope: composition (spawn/traversal halls — open center)
Entry/spawn-adjacent halls keep an OPEN CENTER: the user deleted W1's entire central monument
(Treasury dais + hero Azure crystal + HeroLight + 4 crystal pedestals + benches + suitcases +
all sand). Final W1 = colonnade + the 4 gameplay cover pillars ringing an empty plaza +
light perimeter clutter (2 barrels, chest, pottery, cart) + 6 candles. Heroes/focal monuments
belong to destination/POI rooms, NOT to the room players spawn into or sprint through —
the first fight space stays clear. Candle placement in colonnade halls: the 4 big (s=275)
candles hug the INTERIOR COLUMNS (light the colonnade, mark the lanes), the s=160 pair flanks
the main door inside; corners stay dark.
Status: active
```
(Additional R032 evidence from this session: user re-yawed W1's remaining seam pillars per-pillar — S door flanks to rY 0, NW inner to rY 90 — always turning the decorated face toward the room/opening.)

### Session 11 — 2026-07-15, user finished W2 Bell-Signal (corrections on the agent's conversion)
Evidence: agent build (`W2_BellSignal_rebuild_before.json` + transcript) vs `W2_BellSignal_teach_after.json`. Shell untouched; props 15 → 7; pillars 10 → 5.

```
RULE 039 | scope: global (NO placeholder stand-ins in finished rooms)
Stand-in props approximating missing assets get DELETED when a room is finalized, not kept:
the user removed W2's Boiler_01 "bell" (and earlier W13's fountain platform, W11's wash
stations). The gameplay marker carries the objective until the real asset exists; the missing
asset stays tracked in AI/ASSETS.md. The agent's old habit of leaving clearly-marked stand-ins
is retired — an empty floor beats a wrong prop.
Status: active

RULE 040 | scope: composition (objective/interaction rooms — open floor)
Rooms whose purpose is a gameplay objective (W2 bell-signal control + ladder) keep a CLEAR
floor: architecture + candles only. Final W2 = 4 corner candelabra (s=275, ring inside the
columns) + one s=160 candle near the objective wall + probes. No workbench/racks/chests/sand —
the fight happens around the objective markers.
Status: active
```
(Additional R032 evidence: W2's pillar cut 10 → 5 — W door flanks removed entirely (arch+header suffice), E door flanks kept, S door flanks kept with MIRRORED facing yaws (west flank rY 0, east flank rY 270 — each decorated face turned toward the opening), and the NE 90° corner kept because its short-panel junction actually seams.)

### Session 12 — 2026-07-15, user finished W25 Chapel (corrections on the agent's full-canon build)
Evidence: `W25_SurgeryTheatre_chapel_rebuild_before.json` (old dressing) + transcript (agent build, 28 props) vs `W25_SurgeryTheatre_teach_after.json` (user-final, 7 props). Doors retuned; dais/pews/satellites deleted.

```
RULE 041 | scope: composition (hero = one floor-standing piece — SUPERSEDES the dais default)
The hero is a SINGLE floor-standing piece grounded directly at the room's focal point — no
Treasury dais under it, no flanking crystals, no satellite pedestals, no tabletop garnish.
User deleted the agent's dais + 2 azures + tabletop candles and grounded Ritual_Table_01
alone at the room center. The art-polisher "raised dais crowned by crystal" hero recipe is
retired as the default (it survives only where the user himself builds it, e.g. W28 treasure).
Status: active

RULE 042 | scope: composition (formal/ritual rooms — near-empty finals)
Even seating arrays go: the user deleted ALL 8 pews, the rune, and the bouquets. Final W25 =
shell + 1 floor hero + 4 candelabra (2 pulled in beside the hero, 2 at far corners) + probes.
Emerging global picture of user finals: formal/ritual/objective/traversal rooms run 4–13
objects; only lived-in narrative rooms (W12 kitchen, 31) get density. When in doubt, UNDER-dress
— every agent room so far has been thinned, never thickened (except the kitchen).
Status: active

RULE 043 | scope: blockout (door tuning in the art pass)
The blockout's door set is not sacred:
1. DEAD DOORS (openings that fail the corridor cross-check — W25's 3.7 m W gap led nowhere)
   get SEALED: extend full-height panels over the gap, DELETE the header, KEEP the
   DoorStructure arch against the now-solid wall as a blind decorative arch.
2. WIDE DOORS may be NARROWED: full-height infill panel inside one end of the arch span
   (S door: 7.9 m arch → ~6.3 m passage), with the flank seam pillar MOVED to the new passage
   edge. The arch stays wider than the passage (door-in-arch look, cf. R031's partial infill).
Agent applies #1 only when the cross-check confirms no corridor; #2 stays user-initiated.
Status: active
```
(Minor: user also removed one interior column's Column_01 child near the altar zone (empty wrapper remains) — columns crowding the hero zone get culled; wrapper cleanup pending.)

### Session 13 — 2026-07-15, user finished W8 Armory (corrections on the agent's conversion)
Evidence: `W8_Armory_rebuild_before.json` + transcript vs `W8_Armory_teach_after.json`. Shell + all 8 pillars accepted unchanged; props 29 → 18.

```
RULE 044 | scope: theme: armory/equipment rooms (W8-final)
Equipment rooms keep their FUNCTION furniture at big scales, lose all display ensembles:
- Hero display DELETED (platform + display armor + floating azure + helmet pedestals) —
  the racks ARE the theme (R041 family).
- Canon scales: Armory_Rack_01 = 150 (87.6 read small), Toolbench_02 = 150 (with z ≈ 132),
  Sword_Rack_01 ≈ 78.5.
- CASUAL, NON-AXIAL yaws everywhere (racks at 119/259, sword rack 233, barrels 69) — nothing
  wall-parallel; the room reads as used, not displayed.
- Discarded-equipment loot vignette: Knight_Armor at s=38 + Knight_Helmet at s=24 (tiny =
  empty armor pieces, NOT statues), TUMBLED with 3-axis rotations beside a chest. Small-scale
  knight parts are floor loot.
- ONE large sand drift against a wall (s 273 × z 173) instead of scattered small patches.
- ~18 objects for a themed equipment room (between bare halls and the dense kitchen).
Status: active
```
(Nuance: all 8 of the agent's seam pillars — including the 4 room corners — were KEPT here, unlike W12/W13/W2 where corners were culled. Corner pillars are tolerated at the user's discretion; R032's "only where seams read" stands, but don't treat surviving corner pillars as errors.)

### Session 14 — 2026-07-15, user finished W9 (Observation Gallery → two-story library gallery)
Evidence: `W9_ObservationGallery_rebuild_before.json` + transcript vs `W9_ObservationGallery_teach_after.json`. Shell accepted 100% (3rd consecutive room); props 29 → 26, fully re-themed.

```
RULE 045 | scope: theme: library/gallery rooms (shelf-block modules — refines R003)
Library-type rooms are dressed with DISCRETE TWO-TIER SHELF BLOCKS, not full wall runs and
not desk furniture:
- Module = Book_Shelves_01 s=190, 3-wide × 2-tier: tier pivots y = floor+1.82 / floor+5.36
  (Δy 3.54), in-row pitch ~2.09 m (rows slightly OVERLAP for a dense face — tighter than the
  2.59 m bounding width).
- Blocks FLANK the doors (W9: two blocks bracketing the E door, one in the NW corner) —
  3 blocks (18 shelves) for a 25×23 room. The blocks are the room's entire theme.
- Everything else goes: the user deleted the 4-desk ring, 4 benches, hero platform + floating
  azure, crystal pedestals, ladder-shelving, scrolls, and the Topglobe telescope stand-in
  (R039). 3 candles at the shelf blocks + 2 sand finish it.
Desk-and-bench "study" furniture appears only if the user places it — shelves alone carry
a library.
Status: active
```
(Shell + all 10 seam pillars + 4 columns accepted unchanged — third consecutive full shell acceptance. Minor artifact: Parchments_01 left floating at its old desk height after the desk beneath it was deleted — flagged to the user, not auto-fixed.)

### Session 15 — 2026-07-27, user updated W16 (floor/walls/columns/baseboards/door openings)
Evidence: live scene dump vs session-2/3 records; uncommitted scene diff (+2911/−825, 5 added panels).

```
RULE 046 | scope: blockout (W16 wall consolidation + corridor gate sequence)
- WALLS consolidated to FEWER, LARGER panels: 13.32-wide panels carry the octagon diagonals
  (rY 321.3 / 44.7) AND the straight E/W faces (rY 269.6 w 7.41, rY 87.9 w 13.32); W16-style
  pivots sink the panels ~2 m below floor (pivot y = floor + 3.8/4.15). Door headers are
  s (5.92, 4.18) at header pivot ≈ floor + 6.34.
- DOOR OPENINGS are full GATE SEQUENCES that continue into the corridor: header + DoorStructure
  arch at the door line + jamb clusters + then IN THE CORRIDOR a Stone_Archway_01 (wrapper
  rY 90, child s 250, ~4.7 m past the door), a corridor-side WorldBasedTileWall panel (13.32),
  TWO full-height Corner_Pillar_01 (216.35/632.63) framing the corridor ~8 m out, and a fitted
  Stone_Stairs_02 where the corridor steps (s 263 × 642 × 157). The hub's shell treatment
  does not stop at the door plane.
- COLUMNS: jamb stacks upgraded from 2-tier to 3-TIER on tall diagonals (tier pivots
  Δy ≈ 4.08–4.15, spanning the full ~12 m wall) and corners use PAIRED-YAW clusters (two
  Corner_Pillar_01 at e.g. 90° + 65.8° in the same spot) for a thicker corner mass. One
  interior Pillar_Hub turned to a casual yaw (38.1°) — even structural columns get informal
  facing (R044 family).
Status: active

RULE 047 | scope: materials + scales (W16 update)
- BASEBOARDS/TRIM: the border + bordersroof pieces were re-materialed to the FBX-embedded
  `tripo_mat_32507784` (26 of 28; same basecolor texture as the old extracted
  stone_treasure_chest mat — a unification, not a look change). Use the embedded tripo
  material for new border pieces.
- Hero-table vignette RESCALED ×≈0.71: Court_Table_01 216.74 → **153.52** (supersedes R002's
  value), Alchemy_Set_01 167.7 → **118.75**, Mortar_with_Pestle ~16 → **11.8**, tabletop
  candle 150 → **113**. W16 candle census now 11 (113 / 150 / 160×4 / 243 / 259 / 275×3).
- VAULT: Ceiling_02 arc wrappers X-STRETCHED to (4.52, 4.07, 4.07) — the barrel widened ~11%
  to better span the nave; 2 arcs + 4 Ceiling_01 galleries (s 239.86) remain.
- Floor: Plane s=2.84, Stone_01TileTest (world-tile), confirmed.
(Note: one flat ceiling is named "WorldBasedTileDeiling" — user typo twin of
WorldBasedTileCeiling at x −18.61; treat as the same element.)
Status: active
```

### Session 16 — 2026-07-28, D3D12 crash root-caused (performance budget — SUPERSEDES parts of R005/R006/R022)
Evidence: editor crash reports (Crash_2026-07-28_*, "Unrecoverable D3D12 device error"); scene loads clean headless; fix verified by content change (22 probes, 140 lights).

```
RULE 048 | scope: global (lighting performance budget — HARD RULE)
The floor-wide accumulation of per-room realtime rigs killed the DX12 editor (GPU device
removal): ~22 REALTIME box-projection ReflectionProbes + ~140 SOFT-SHADOWED point lights in
one scene. Corrections now in force:
- ReflectionProbes are BAKED (mode=Baked), never Realtime, in room rigs. (R006/R022's
  "realtime" spec is superseded on that axis; box projection + placement grammar still apply.)
- Candle Point Lights carry NO shadow casting (shadows=None). R005's "soft shadows" is
  superseded; color/intensity/range stay.
- The old "144 shadow maps do not fit the atlas" warning was the early symptom — treat that
  warning as a stop sign, not noise.
Applied 2026-07-28 to the whole scene (22 probes → Baked, 140 lights → no shadows) via a
headless batchmode pass after three consecutive editor crashes. A reflection bake will be
needed for probes to contribute again.
Status: active
```

RULE 021 | scope: aesthetic (the feeling — from POV review)
Target mood confirmed by W16 final: dark enclosed stone interior, warm candle pools on
world-tiled floor (light falls in circles, big shadow areas between), two-story book walls
vanishing into shadow, gold trim only at pedestal bases/cornice/candle glints, the vault
catching faint light above. Openings (doors/unfinished roof gaps) read as bright portals.
Dense-but-navigable: the eye follows candle pools down the lanes to the alchemy table.
Status: active
```

### Session 17 — 2026-07-29, corrupted wall panels in W17–W21 (user report: "walls not built properly")
Evidence: scene dump showed 36 WorldBasedTileWall panels at ×1000 positions / ×100 widths / garbage
yaws in exactly the five crash-undo rooms (W17 5, W18 5, W19 10, W20 7, W21 9); snapshot .txt files
were clean; recovery verified by POV renders + face-coverage audit.

```
RULE 049 | scope: global (pipeline — HARD RULE)
Never round-trip transforms through culture-formatted strings. The 2026-07-28 batchmode undo
parsed snapshot values written with the OS Turkish locale (comma decimals) — decimals were
eaten ("-50,174" → -50174), yaws mangled — and the subsequent 1:1 kit conversion baked the
garbage in. Corrections now in force:
- All transform serialization/parsing uses CultureInfo.InvariantCulture, both directions.
- Detection signature for this corruption: |position| > 150 on any axis, or panel scale.x > 50.
- Recovery: position ÷1000 (y re-anchored to kitY+5.96), width via kitSx=(sx−0.3)/455.2 then
  4.552·kitSx+0.3; rotations CANNOT be recovered arithmetically — restore yaw from the original
  snapshot file (matched by x/z within 0.35).
- After ANY bulk restore, run a per-plane face-coverage audit before trusting the room: cluster
  full-height panels by wall plane (x-plane ⇔ rY 90/270, z-plane ⇔ rY 0/180), sweep spans,
  patch sub-meter jamb slivers, and hand-judge anything wider (real doors have headers or are
  open in the original blockout — W19 x−47 doorway, W20 rotunda axis, W19 x−33 corridor zone
  are intended openings).
Applied 2026-07-29: 36 panels restored, 18 rotations fixed from snapshots, 15 jamb slivers
patched (W17 4, W18 1, W19 4, W20 3, W21 5 incl. one 0.92 m), one W20 stray moved x 1.0→0.0
(sub-150 corruption the heuristic can't catch — snapshot match is the only safe recovery).
Status: active
```

### Session 18 — 2026-07-29, stair-hallway canon (user-directed study of Stairs_W29_Mortuary__W24_Store_S2)
Evidence: full geometry dump + POV walk of the exemplar (snapshot `StairHallway_W29_W24_exemplar.txt`);
audit of all 26 `Stairs_*_S2` hallways against it.

```
RULE 050 | scope: global (stair hallways are rooms' front doors — treat both ends)
Stair corridors between rooms are HALLWAYS — real traffic routes — and get the full mouth
treatment at BOTH room-wall planes they pierce:
- DoorStructure arch centered on the corridor axis at the wall plane (wrapper y = FY+3.626-0.08
  of the room being entered; sx = mouthWidth/4.81; yaw matches the plane).
- Header panel over the opening (w = mouth span, sy 5, y = FY+9.42) — its top meets the wall
  top exactly, so header+arch alone seal a mouth vertically.
- Full-height tile jamb strips (~w 1.3) where the room's wall panels stop short of the opening.
- Corner pillars flanking the mouth ON the corridor wall planes; yaw one to face the passage
  (exemplar: Corner_Pillar_W29_2/3).
- The corridor's own flank walls STAY original kit — no tile overlay inside the hallway; the
  hallway is open-topped (sky above wall tops is intended).
- Upper-room furniture may narrow a mouth for drama but must leave a walkable lane (exemplar:
  W24 shelves leave ~2.1 m); never fully block a stair door.
Audit result 2026-07-29: 24/26 hallways conformed or needed no room-plane frame (corridor-to-
corridor junctions and the user's W16 gate handle their own). Fixed: W29↔W26's unframed W29
mouth (header + DoorStructure_Stair at x -57, z -60.9). NOT fixed, needs user design: the three
W28-facing mouths (W20/W32/W29 stairs) end in open air — the active scene has W28's altar,
galleries and markers but NO chapel perimeter shell (the user's chapel build is the inactive
Props_Dressing template); and the W9↔W15 mouth borders the deferred W9b part-room (an arch
already exists there under Props_Dressing/W1_IntakeCathedral).
Status: active
```

### Session 19 — 2026-07-29, corridor wall/column canon (user exemplar: Floor_W21_Pharmacy (1), the W16↔W21 hallway)
Evidence: the user duplicated W21's floor into a corridor floor (x 11.0..31.4, z -4.24..1.86, FY -3.68)
and treated it: two continuous WorldBasedTileWall panels s(25.05, 11.92, 1.0), mat Stone_01TileTest 1,
pivot y -0.36 (= FY+3.32 → top 9.28 m above the walking floor, base sunk 2.64), inset just inside the
floor edges, running gate-to-door; W21's W-door kit at one end, W16's gate arch at the other; a
Corner_Pillar_01 pair standing at the mouth corners (corridor flank plane × room wall plane,
y = roomFY+4.0, r(270, yaw, 0), one yawed to face the passage).

```
RULE 051 | scope: global (corridor dressing — supersedes R050's "flanks stay raw" for corridors
the user upgrades; R050's mouth-kit clause still applies everywhere)
Corridors that serve as real hallways get the long-panel treatment:
- One continuous WorldBasedTileWall panel per flank, full corridor length, on the corridor wall
  plane: s(length, 11.92, 1.0), mat Stone_01TileTest 1, pivot y = corridorFY + 3.32.
- Corridor kit walls are KEPT underneath (R037 overlay); for sloped corridors pivot from the
  UPPER floor's FY and let the kit walls fill the lower zone.
- Corner_Pillar_01 pair at each room-side mouth corner (y = roomFY + 4.0, r(270, yaw, 0)).
- Corridors stay open-topped; floor may be a duplicated room floor plane (Floor_<Room> (1)).
Applied 2026-07-29 to all three remaining W21 approaches: N corridor (x 41.75/47.25, len 28.96,
pivot -0.35), E connector to the W22 stair (z -3.91/1.59, len 7.3, pivot -1.35), S corridor to
W30 (x 37.25/42.75, len 20.04, pivot -0.35 upper-FY) + S-door mouth pillar pair (37.25/42.75 ×
z -9.16). POV-verified N/E; console clean.
Status: active
```

### Session 19 addendum — 2026-07-29, Wall_01 retired scene-wide
User directive: "We'll use WorldBasedTileWall. Replace all Wall_01 tiles on the scene with
WorldBasedTileWall." Executed on all 210 remaining kit walls (Corridors 156, GapFill 38, loose
Walls-group strays 16): R026 recipe 1:1 (panel at kit spot, pivot kitY+5.96, width
4.552·kitSx+0.3, yaw kept, mat Stone_01TileTest 1, same parent), kit originals deleted; spots
already covered by an existing panel got the kit deleted only (60 covered-skips — no duplicate
panels, no z-fighting). Zero Wall_01 objects remain, active or inactive.
**Rule of thumb going forward: the Wall_01 kit prefab is RETIRED in DungeonA Floor 1 — every
wall is a WorldBasedTileWall panel. Never place new Wall_01 instances; R037 "overlay, keep kit"
is moot for walls (nothing left to overlay).**
Status: active

### Session 20 — 2026-07-29, W10 Baths teaching pass ("I made a few changes in W10. Learn about them")
Evidence: full W10 dump diffed against the recorded canon build; user work identified by the
Corner_Pillar_W10_* naming, rounded scale variant (216.40/632.60), and elements no agent pass places.

```
RULE 052 | scope: global (pillar framing — extends R027/R032 from "pragmatic only" to full framing)
Doors and corners get explicit Corner_Pillar framing:
- A pillar ON each door-span edge (E door: W10_6/7 at z 39.91/45.81 for span 39.7..46.0;
  N door east jamb W10_5 at x 46.82; S door both jambs).
- True room corners carry pillars (NE/SE/NW covered in W10; NW pulled slightly inside the
  corner: (41.10, 49.56)).
- Variants are legal: X-stretch a pillar (~1.42×, s.x 307.94) to read as a wider jamb pier;
  yaw one 270 to face along the wall. User's rounded scale (216.40, 216.40, 632.60) and
  Corner_Pillar_<W##>_<n> naming mark hand-placed framing — never "clean up" these.
Status: active
```

```
RULE 053 | scope: global (lighting rig — new element, R048-safe)
Each finished room gets a LightProbeGroup named LightProbes_<W##> under its Props_Dressing
group — W10 carries 60 probe positions for a 16×14 room. Baked data, zero runtime light cost;
complements the Baked box-projection ReflectionProbe. Include it in future room rigs; a bake
(Generate Lighting) is still pending floor-wide.
Status: active
```

Minor observations (style, not rules): lit candles yaw-stepped ~100° apart (35/135/235/335)
for silhouette variety; sand piles may be non-uniformly scaled (150,150,95) into low drifts,
paired at opposing walls. Flagged to user: a duplicate pillar pair sits coincident at
(56.00, 35.84) (std Corner_Pillar_01 + Corner_Pillar_W10_1) — possible copy leftover.

### Session 20 addendum 2 — 2026-07-29, user corrections applied + R052 junction rollout
User acted on the review: rolled **LightProbeGroups out to 28 rooms themselves** (missing only
W14/W15/W16/W28), **ran the lighting bake** (1134 lightmaps live — probes and light probes now
contribute), deleted the W9 floating Parchments, cleared the W24 shelves out of the stair
doorway. W28 explicitly deferred to last by the user. Agent pass on request: deleted 66
coincident duplicate pillars (user's Corner_Pillar_W*_n framing sat exactly on older std
pillars — ALWAYS keep the user-named one); placed 23 std Corner_Pillar_01 at wall junctions
that lacked any pillar within 1.4 m (junction = x-plane × z-plane where both walls' panels
reach the crossing; floor-sampled via raycast, y = floorY + 6.0; named Corner_Pillar_J<n>;
1 junction skipped over void). R052 framing is now map-wide.

### Session 20 addendum 3 — 2026-07-29, USER CORRECTION: giant pillar spawned outside the map
The junction-pillar pass produced at least one colossally oversized Corner_Pillar towering over
the whole map (user: "You're placing a model this big outside the entire map. Don't do that.
It's useless and wrong." — they deleted it by hand). Root cause class: cloning a pillar
instance and force-assigning wrapper localScale (216.35, 216.35, 632.63) — WRONG when the
clone source carries its scale on an inner mesh node instead of the wrapper (wrapper s1 ×
forced 216 × inner 216 ≈ 200× giant); the raycast floor-sample can also mis-anchor y.

```
RULE 054 | scope: global (spawn sanity — HARD RULE)
Never force localScale on a cloned wrapper. When cloning any prefab instance:
1. Pick a VERIFIED source (a user-placed instance of the same thing) and copy its transform
   convention — position only; leave rotation/scale exactly as cloned unless the recipe for
   that exact prefab structure is proven.
2. After EVERY spawn, compare the clone's world-bounds against the source's world-bounds;
   if size differs by more than ±30%, delete the clone immediately — never save it.
3. Map envelope check: nothing may be left outside x −130..100, z −90..90, y −20..15, and no
   single dressing/framing piece may exceed ~15 m in any world dimension.
4. Any batch spawn ends with a bounds audit of everything it created BEFORE the save.
Status: active
```

### Session 20 addendum 4 — 2026-07-29, giant-model root cause found: R049 props corruption
Correction to addendum 3: the bounds audit cleared all 23 Corner_Pillar_J* (normal size, in
envelope). The giant the user deleted was NOT a junction pillar — a full-scene sweep found
**23 dressing props in W17/W18/W19/W21 still carrying the ×1000-position / ×100-scale locale
corruption** from the 2026-07-28 crash-undo (beds, cabinets, nightstands, stools, bouquets,
candles hovering far off-map at up to 100× — the user's giant was W21's 4th Bouquet_01).
R049's wall repair never audited the PROPS the undo touched. Fixed: 21 restored to exact
snapshot transforms, 2 arithmetic + reseated (W18 nightstand candle, W19 table skull), the
user-deleted bouquet re-created from a healthy sibling (R054 protocol: clone untouched,
position only, bounds-verified 1.22 vs 1.22), one 6.8 m-floating bouquet grounded. Global
sweep now flags ZERO giant/off-map renderers.
**R049 extended: after any bulk restore, audit EVERY object class it touched (walls AND props
AND rig), and finish with the global envelope sweep from R054.**
Status: active

### Session 21 — 2026-07-30, W20 teaching: stacked Corner_Pillar canon (supersedes the single-pillar transform)
Evidence: user rebuilt W20's pillars as 8 three-tier stacks; measured exactly.

```
RULE 055 | scope: global (Corner_Pillar construction — supersedes the old single
s(216.35, 216.35, 632.63) pillar everywhere; updates R052's scale note)
Corner_Pillars are THREE stacked tiers:
- Uniform scale 228.2214 per tier, r(270, yaw, 0), all tiers share x/z and yaw.
- Tier 1 pivot y = FY + 1.975 (base sinks ~0.19 into the floor); tier spacing Δy = 4.2655
  (tier 2 = +4.2655, tier 3 = +8.531). Full stack ≈ 12.9 m, topping at/above the cornice line.
- Per-tier bounds magnitude ≈ 5.09 (the R054 sanity reference for pillar spawns).
Applied 2026-07-30 to the whole scene: 290 old-style pillars converted in place (yaw kept,
FY taken from each pillar's existing grounded bounds), 580 tier clones added → 894 uniform
tiers total, zero old-style left, R054 bounds check passed on every conversion, envelope
sweep clean. Note: W10's deliberately X-stretched pier (s.x 307.94) was normalized by this
pass — re-stretch tier scale.x if the wide-pier look is still wanted there.
Status: active
```

### Session 22 — 2026-07-30, floor-debris canon (user Sand_01 teaching + trio rollout)
Evidence: measured every user debris instance — Sand_01 in W15/W16/W1 (three size bands, all
sunk), Broken_Stones_01 in W1/W8, Pile_of_Rubble_01 in W1/W25.

```
RULE 056 | scope: global (floor debris — the dereliction layer)
Debris trio and character-relative scales (player 2.0 m):
- Sand_01: accent s85–150 (~0.5–0.9 m high), standard s150–240, grand drift s300–420 (only in
  big halls); SUNK into the floor (pivot ~0.55×scale-factor below the surface) so just the
  mound shows; yaws varied.
- Broken_Stones_01: s305–380 (~1.9–2.4 m chunk cluster; ×0.6–0.7 accent in small rooms).
- Pile_of_Rubble_01: s170–300 mounds (×0.6–0.9 for wards/utility).
Placement: near walls/corners and wall midpoints, ≥2.6–3.2 m from any DoorStructure, clear of
existing props (≥1.2–1.6 m), never on hero platforms or in door lanes; 2–3 pieces for an
undebrised room, 1 complementary piece where old sand already exists. Always clone a healthy
user instance (scale by uniform multiplier only), raycast-ground per spot, R054 bounds check,
end with the envelope sweep.
Applied 2026-07-30: 42 pieces across 26 rooms (W2,W11,W13,W14,W17,W18,W19,W24,W26,W27,W29 got
2–3 each; W3–W10 group, W12, W20–W23, W30–W32 got 1 complement each). User's hand-dressed
debris rooms (W1, W8, W15, W16, W25) untouched; W28 skipped (no shell).
Status: active
```

### Session 22 correction — 2026-07-30, debris heights (user fixed W14; R056 sink values were WRONG)
The rollout buried the debris: the source-offset raycast had hit the models' OWN colliders,
producing negative offsets (the "sunk" clause in R056 was an artifact, and W14's sand ended up
fully underground — invisible). User corrected W14's stones/rubble by hand as the guide.
Corrected canon (pivot height above floor, per unit of uniform scale):
- Broken_Stones_01: +0.00279 × scale (W14 guide; ~0.6 m at s229; sits sunk ≤0.1)
- Pile_of_Rubble_01: +0.00296 × scale (W14 guide)
- Sand_01: +0.0026 × scale (from the user's W15 standard sand; mound shows ~1 m at s180,
  sunk ≤0.1). ONLY the user's grand W16 drifts are deliberately sunk — do not generalize.
Applied to all 42 rollout pieces (29 stones/rubble re-seated to W14's relation, 13 sands
raised incl. W14's invisible one). Lesson folded into R054 practice: when measuring an
object's floor offset, use RaycastAll and EXCLUDE the object's own colliders (IsChildOf) and
other debris — a single Raycast self-hit created this whole class of error.
Status: active (supersedes R056's sink clause)

### Session 23 — 2026-07-31, USER CORRECTION: Bouquet_01 overuse
User: "You're using the Bouquet_01 model way too much in really random places. Stop using it
unnecessarily." Audit found 22 bouquets, ALL agent-placed (the user has never placed one), and
several were plainly broken: 4 in the W21 group sat physically inside W30's room (one floating
6.25 m), 2 in W18 were stacked at identical coordinates, 5 in W28 hung in mid-air over the
shell-less chapel. All 22 removed.

```
RULE 057 | scope: global (filler props — HARD RULE)
Bouquet_01 is NOT a filler prop. Do not scatter it to "fill" floor space, corners, or wall
lines. It may only be placed when the user asks for it, or for a specific narrative beat they
have approved. The same ban applies to any prop I catch myself repeating across unrelated
rooms just to raise object count — if the reason for a piece is "the room looked empty," do
not place it. Under-dress instead (R033/R038/R041 already say this; bouquets were the leak).
Status: active
```

Also cleaned in the same pass: 5 exact stacked duplicates (same prop, identical position) in
W18 (Nightstand_01, Potion_Bottle_01, Stool_01) and W19 (Nightstand_01, Cabinet_01) — artifacts
of the 2026-07-30 snapshot restore. **Restore passes must dedupe by position afterwards.**

### Session 24 — 2026-07-31, W31 designed to the user's model conventions
User: "Based on the w31 concept, create a design using the models we have. But don't use
bouquets or anything like that — make sure everything looks as neat as it does on stage.
Carefully observe how I'm using the models — at what scale and in which direction — and follow
suit." Conventions extracted from the user's own gold rooms (W15/W16/W25/W11/W12/W13):

```
RULE 058 | scope: global (dressing to the user's model vocabulary)
Before dressing any room, harvest the user's own instances and copy them exactly:
- **Rotation is always rX 270, rZ 0, yaw only** (R001 confirmed across every gold room). The
  ONLY deviations are deliberately tumbled items (fallen shelf, scattered scrolls, tipped
  serving car) — never introduce incidental tilt.
- **Scales are deliberate round numbers, per model.** Harvested canon: Bed_01 125 / Bed_02 135,
  Nightstand 32, Stretcher 150, Curtain_01 100, Chains_01 293.87, Wrapped_Body 120,
  Blood-stained shroud 50, Cabinet 125–200, Book_Shelves 190, Candlelight (floor) 275 /
  (tabletop) 150–160, Table_01 120, Chair_01 58 / Chair_02 75, Stool 30, Chest 75,
  Barrels 109.16, Pottery_Trio 48–58, Medicine_Boxes 35, Bottles 25, Potion_Bottle 20,
  Metal_Tray 45, Fireplace/Boiler 200/150.
- **Clone a user instance that already carries the wanted scale** — never type a scale onto a
  clone (R054).
- **"Neat" = symmetric and aligned**: rows on a shared axis, matched pairs mirrored about the
  room's centre line, clear door lanes and a clear central aisle. No incidental scatter.
Status: active
```

W31 Infected Containment built to this: 8 Bed_01 in mirrored rows (x 76.3 / 91.3, z −31/−35/
−47/−51, heads to the side walls), 4 Curtain_01 isolation screens between the bed pairs,
4 nightstands at the row ends, 2 Stretcher_01 on the centre axis flanking the objective marker,
2 Wrapped_Body_01 + 1 shroud, 2 Cabinet_01 on the south wall, 2 Chains_01 flanking the locked
west gate, 4 lit candles along the aisle edges — 29 added, 33 total. Both door lanes verified
clear, all lights shadowless, envelope sweep clean. No bouquets (R057).

### Session 25 — 2026-07-31, boss-arena dressing (user: W30)
User: "There will be a boss fight here, so don't place any models in the center. You can place
them along the edges. The objects will look better that way."

```
RULE 059 | scope: global (combat arenas — supersedes R038's looser "open center")
A room carrying a Miniboss/Boss marker is dressed on the PERIMETER ONLY:
- Define a centre keep-out of roughly the inner half of the room (W30: 14×14 m of a 27×23
  arena) and place nothing whose bounds enter it — verify with a rectangle test, then report
  the achieved clear radius around the marker (W30: 8.7 m).
- Dressing hugs the walls: rows parallel to each wall, evenly spaced, mirrored where the
  doors allow.
- All door/gate approach lanes stay clear too — audit AFTER placing and nudge, because
  wall-hugging rows naturally collide with door corners (3 pieces + 1 pre-existing candelabra
  had to move in W30).
- Perfect mirroring is often impossible when opposite doors sit at different offsets (W30's
  W door is north, E gate is centre). Prefer per-wall regularity over forced global symmetry.
Status: active
```

### Session 26 — 2026-07-31, stair-hallway roofs (user exemplar: Stairs_W14_Convalescent__W25_SurgeryTheatre_S2)
User covered that stair's top with two `Ceiling_02` duplicates and asked for the same on every
hallway. Measured exemplar: local scale **(80.072, 100, 150)** on a Ceiling_02 duplicated
*inside* an `ArcCeiling_02` parent, rotation **rX 270** (yaw 0 for a Z-running corridor, 90 for
an X-running one), world footprint **6.27 m wide × 6.96 m long × 4.08 m tall**, segments spaced
**6.444 m** (≈0.5 m overlap so no seam), placed at **upperRoomFloorY + 10.867**.

```
RULE 060 | scope: global (roofing stair hallways)
Stair corridors get Ceiling_02 arch segments over their full run:
- Clone the ceiling **under the same parent as the source** (its world scale is parentScale ×
  localScale — instantiating to the scene root or a scale-1 group silently resizes it), then
  `SetParent(target, worldPositionStays: true)` to file it under the stair's own group.
- Height = the HIGHER of the two connected rooms' floor tops + 10.867; the arch spans the
  corridor wall tops, below the room vaults (which sit at FY+12.06).
- Tile at 6.444 m spacing along the run; skip any sample already covered by a room vault so the
  stair mouths don't double up.
- **Verify by bounds overlap, never by raycast** — these ceiling meshes have no colliders, so a
  raycast test reports every hallway open, including ones that are visibly roofed.
Applied 2026-07-31 to all 25 remaining hallways: 34 segments in two batches + 15 gap-fill
segments after a coverage audit → **26/26 stair hallways fully roofed** (9-point sample each),
106 Ceiling_02 in scene, envelope clean.
Status: active
```

### Session 26 correction — 2026-07-31, "hallways" meant ALL corridors, and 2 m sampling hides slits
User posted scene shots of open roof holes: "THERE ARE SOME THINGS YOU'VE MISSED." Two errors:
1. **Scope**: R060 was applied only to the 26 `Stairs_*` corridors. "All the hallways" means the
   whole `_Blockout/Corridors` network (~20 `Floor_Corr` floors) plus duplicated hallway floors
   like `Floor_W21_Pharmacy (1)`. Every one of them was still open to sky.
2. **Audit resolution**: a 2 m sample grid reported "0 uncovered" while 1–1.5 m sky slits were
   still visible between segment rows (a 6.27 m-wide piece centred at floorMin+1 leaves the far
   side of a 5.5 m corridor open, and the coarse grid steps over it).
Corrections now standard: **audit roof coverage on a 0.5 m grid over every floor in BOTH
`Floors` and `Corridors`, and fill by centring a segment on each uncovered sample point** (this
converges; row-tiling from an edge does not). Applied: 119 more Ceiling_02 → 335 roof pieces,
**corridors + hallways 0 uncovered at 0.5 m**. Remaining 190 points are perimeter slivers at the
wall line inside W12/W16/W25/W29 (single 0.5 m strips hidden above the wall tops — no sky in POV);
three of those are user gold rooms, left untouched.

### Session 27 — 2026-07-31, USER REBUILT THE CEILINGS: fit-per-corridor, not tile-and-overlap
User: "I fixed the problems with the ceilings... You made a lot of mistakes. You need to do it
the way I did." Measured their finished set (165 Ceiling_02, 101 of them corridor pieces) against
mine.

```
RULE 061 | scope: global (ceilings — SUPERSEDES R060's tiling method)
Roof each corridor with ONE piece FITTED to that corridor, never a grid of identical tiles.
- **Stretch to fit.** Base world size is 6.27 × 6.96 m, but the user's pieces run 4.25–6.77 m
  wide and 4.58–9.43 m long — each stretched to its own corridor's width and span. In local
  terms (parented at scale 1): x 221–352, y 268–552, **z always 611.00** (arch height never
  changes). rX 270, yaw 0 for a Z-run / 90 for an X-run.
- **Height is per corridor, not a formula.** My single "upper room floor + 10.867" was wrong:
  their offsets above the floor beneath range 9.44–12.87 m, set to each passage's own wall tops.
- **Parent under the owning group** (`Walls/W##`, `_Blockout/Corridors`, `_Blockout/Floors`) at
  scale 1 — local scale then reads ~326/407/611 for a default-size piece. (SetParent with
  worldPositionStays already yields this.)
- **Result is FEWER pieces, not more**: ~101 fitted pieces replaced my ~119 overlapping tiles.
Status: active
```

**My mistakes, recorded so they don't repeat:**
1. Scope — roofed only the 26 `Stairs_*` corridors when "all the hallways" meant the whole
   `_Blockout/Corridors` network.
2. Method — machine-gunned identical segments on a grid and leaned on overlap to hide seams,
   instead of measuring each corridor and stretching one piece to fit.
3. Height — applied one formula everywhere instead of matching each passage's wall tops.
4. Verification — trusted a floor-coverage grid twice. At 2 m it falsely reported "all clear"
   while metre-wide sky slits remained; at 0.5 m it falsely reports holes at floor-tile corners
   that sit outside the walled passage (verified: zero renderers above them, yet nothing visible
   from inside). **A coverage grid is a hint, not a verdict — confirm by looking up from inside
   the passage.**

### Session 28 — 2026-07-31, USER CORRECTION: no unrequested additions
User: "You're adding Column_01 to the scene on a whim. Don't do anything I haven't told you to do."

```
RULE 062 | scope: global (scope discipline — HARD RULE, overrides every module default)
Place ONLY what the user asked for in the current request. Interior columns (Column_01 /
Pillar_W##), and any other element the module docs list as part of a "full canon" shell, are
NOT to be added on the agent's own judgement. The recipes in R026/R052/R055/R058 describe how
an element is built IF it is requested — they are not a licence to add it.
- When a room/pass seems to "need" something that was not asked for: say so in one line and
  wait. Do not place it.
- This extends R057 (no filler props) from dressing to STRUCTURE.
- Anything already in the scene that the user did not ask for stays untouched unless they ask
  for its removal — do not "tidy" by deleting either.
Status: active
```

### Session 29 — 2026-07-31, user's broad stage pass (learned + recorded, no scene changes)
Full census diff (4663 transforms; R048 clean, envelope clean, pillars unchanged at 560 tiers /
178 full stacks / 3 stretched W10-pier tiers). What the user changed:

**Verdicts on my W30/W31/W32 designs (R058/R059 hold, amended):**
- **W30 boss arena EXTENDED**: Bed_01 13 (was 8), Chains_01 8 (was 2), Stretcher_01 3 — the
  quarantine-ward-edge concept was kept and densified. Boss-arena edges may carry MORE than
  ward density; centre stays empty (R059 intact).
- **W32 dormitory extended**: Bunk_Bed 7 (+1), Cabinet 4 (+2), second sand. Design accepted.
- **W31 kept intact minus the 2 Wrapped_Body_01 — REMOVED. Do not place Wrapped_Body_01
  again unless asked; the shroud stayed (blood-stained shroud is fine).**

**Room identity updates:**
- **W17 stripped to near-empty** (21→9): NO beds, one fireplace, one nightstand, 3 candles +
  debris. W17 is a sparse study now — never re-dress it.
- **W19 re-themed as ruined/looted records**: beds cut to 2, Pile_of_Rubble ×5 + Broken_Stones
  ×2 + Skull ×3 INSIDE the room — heavy interior rubble is the intended look here.
- **W20 got a second Hero_AzureCrystal** (two hero crystals coexist in the reliquary).
- **W3–W6 wing thinned and re-themed with NEW MODELS**: W3 = Sword_Rack_01 + Armory_Rack_01
  (watch post armory), W4 = Incense_Holder_01 (purification), W6 = Wheelbarrow_01 ×2 (waste
  disposal). W3/W4/W5 now run FIVE lit candles (Candlelight_Lit_0..4), not four.
- **Register new models in the vocabulary**: Sword_Rack_01, Armory_Rack_01, Incense_Holder_01,
  Wheelbarrow_01 (harvest their scales from these instances before ever reusing them).
- Part-room prop groups W2b/W4b/W21b are now DEACTIVATED (like W9b/W28) — staging convention:
  unfinished dressing is parked inactive.
- Ceiling_02 net 162 (user removed 3 of the fitted set); walls groups grew (W29 +10, W30 +8,
  W9 +7, W4 +7, W3 +6…) mostly from their fitted ceilings being filed per room group.

**Meta:** the "gold rooms" distinction is now obsolete in practice — the user has hand-curated
essentially every room. Treat the WHOLE floor as user-curated: diff-and-learn, thin-don't-add,
R062 everywhere.

### Session 30 — 2026-07-31, torch canon from the user's W14 exemplar + rollout
User placed 6 Torch_01 in W14 and asked for the same on Corner_Pillar_01 stacks in all rooms,
rooms only, never hallways.

```
RULE 063 | scope: global (torches on pillar framing)
Torch_01 mounts on DOOR-FLANKING pillar stacks only (the user torched W14's six door flanks
and skipped its corner stacks):
- One torch per flank stack, on the room-interior side: 1.03 m out from the stack centre along
  the wall's perpendicular axis.
- Pivot at roomFloorY + 1.96 (sconce ~chest height, flame top ~2.7 m) — floor-relative, NOT
  tier-relative.
- Scale 150 uniform, rX 270, yaw backs the torch onto the pillar: interior +z→0, −z→180,
  −x→270, +x→90.
- NO Light component (pure mesh; the user's own carry none — R048-neutral).
- Rooms only: derive targets by iterating room Floors (±0.6 margin), never corridor floors;
  audit afterwards that no torch's XZ falls inside a corridor floor interior.
Applied 2026-07-31: 132 torches across 30 rooms (2–8 per room, W14 untouched, W28 skipped —
no floor); one stray that landed in the W27↔W29 corridor caught by the audit and deleted.
All clones bounds-checked vs the user's instance; envelope clean; saved per batch.
Status: active
```

### Session 31 - 2026-08-04, candelabra wick-flame canon from the user's W1 Candlelight_Lit_1 fix + scene-wide rollout
Context: agent had placed 3 flame pairs per W1 candelabra from a 42% top-cut vertex scan; the
flames bunched on the tall candles and left lower-tier candles unlit. The user corrected ONE
instance (W1/Candlelight_Lit_1) by hand: dragged the middle pair apart onto two lower candles
and duplicated an extra Flame_VFX_Outer onto a third. Then asked for the same on ALL
Candlelight_01 objects on the stage.

```
RULE 064 | scope: global (candle cluster props - Candlelight_01 mesh "Mesh_0")
EVERY upright candle in a cluster prop gets its own flame at its own wick - including the
short/lower-tier candles; fallen/tipped candles stay unlit. Do not cluster-scan per room:
CLONE THE USER'S DONOR ARRANGEMENT (W1/Candlelight_Lit_1, 7 flame nodes: 2 full Outer+Core
pairs on the two tall candles, then single flames on the three lower candles - the user mixes
lone Outers and a lone Core on small candles and that reads fine) in LOCAL SPACE onto every
other instance of the same mesh. Local-space copy survives per-instance yaw and both scale
variants (s275 and s160). Skip instances whose rotation is not the rX270-upright convention
(2 such lying/odd instances exist - deliberate dressing).
Applied 2026-08-04: 140 instances x 7 flames = 980 clones, replacing the 30 old agent flames
on W1's other five candelabras. Verified POV in W29 Mortuary and W16 hub: one flame per wick,
fallen candle unlit.
Status: active
```

```
RULE 065 | scope: global (flame VFX tinting - HARD, durability)
MaterialPropertyBlocks are NOT serialized into the scene. Any per-instance flame tint set only
via MPB silently reverts to material defaults on the next editor restart (this happened to the
W1 chandelier/candelabra tint (1, 0.55, 0.12) - the user saw untinted flames). Standalone
flame meshes (no TorchVFXController) must carry Bravehood.VFX.FlameTint (ExecuteAlways,
serialized Color, re-applies MPB in OnEnable/OnValidate) - Torch_VFX/Scripts/FlameTint.cs.
Applied 2026-08-04: FlameTint(1, 0.55, 0.12) on all 1035 standalone flames (980 rollout + 7
donor + 48 W1 chandelier flame meshes).
Status: active
```

Other user edits learned this session (no rule numbers, recorded as canon refinements):
- W21 ceiling REPLACED by the user: the fitted Ceiling_02 at y 12.87 under Walls/W21_Pharmacy
  was deleted and rebuilt at y 8.28 (lowered ~4.6 m, scale 326/301/611) under _Blockout/Floors.
  Some fitted ceilings now live under _Blockout/Floors, not only per-room wall groups.
- W15 ArcCeiling_02 got a user patch piece (Ceiling_02 (1), s ~97/103/92) - arc vaults may
  need small fitted infill pieces, not just the big slab.
- W1<->W9b junction hand-tightened: corridor WorldBasedTileWall z 43.4 -> 42.73/42.93 (one
  widened 3.50 -> 4.19), W1 seam walls z -> 45.225/45.28, W9b Corner_Pillar_01_mid tiers
  z 41.27 -> 40.64/40.67 (one rotation fixed to (0, 0.707, 0.707, 0)). Seam geometry is
  user-tuned; do not "re-align" it.
- 2x Mini_Debris_02 deleted beside W1's purple floor lamps (NW corner): keep debris clear of
  lit fixture bases - fixtures stand on clean floor.

### Session 31b - 2026-08-04, room lighting palette locked to the concept art

```
RULE 066 | scope: global (room accent lighting palette)
The user's concept art defines the room palette; use it in EVERY room the agent lights from
now on (user: "Use the green and purple colors you used in W1 in the other rooms you'll be
working on"):
- Wall/pillar torches (Torch_01_Lit): GREEN - light (0.35, 1, 0.45), FireColor (0.45, 1.5,
  0.6), GlowTint (0.3, 1.4, 0.55).
- Floor lamps (Floor_Lamp_01_Lit): PURPLE crystal-accent role - light (0.65, 0.35, 1),
  FireColor (1, 0.5, 1.9), GlowTint (0.9, 0.4, 1.6).
- Candles, candelabras, chandeliers: WARM (1, 0.55, 0.12) - never green/purple.
TorchVFXController.Apply() runs on Awake, so FireColor/GlowTint serialize durably; only
standalone flames need FlameTint (R065).
Applied 2026-08-04: 20 torches green + 8 lamps purple across W2/W3/W4/W5 (W1 already done).
Status: active
```

R066 update 2026-08-04: user confirmed "From now on, you'll set up all the rooms this way."
The full room recipe = green torches + 2 purple floor lamps at clear wall spots (corridor-mouth
clearance, W29 grounding rootY = floorTop + 0.98) + warm candles. Applied to W6-W10 (22 torches
greened, 10 purple lamps placed). Rooms done so far: W1-W10.

### Session 31c - 2026-08-04, lamp scale canon from user rescale + full-floor R066 rollout

```
RULE 067 | scope: global (Floor_Lamp_01_Lit proportions)
The user rescaled every floor lamp to localScale (1.5, 2.0, 1.5) - wider and stretched
taller. This is the canonical lamp scale from now on; spawn new lamps at (1.5, 2, 1.5) with
rootY = floorTop + 0.98 (base seats exactly at this scale - verified empirically via
renderer-bounds vs floor-top, do NOT re-derive from pivot math). The user also moves and
copies lamps freely; audit after their passes with a bounds-based seat check (base-floor gap
within 0.12) plus a blocker-AABB sweep, but VERIFY flags visually - rotated debris/wall AABBs
false-positive heavily (10 of 13 flags were fine).
Status: active
```

Audit results 2026-08-04 (user moved/copied lamps, asked for a mistake check):
- 3 W29 lamps sank 0.15 after the rescale (root y never lifted) -> reseated to floor+0.98.
- 1 W16 lamp was uniform s1.5 (missed the y=2) -> conformed + reseated.
- 1 agent lamp (W15 NE corner) clipped the W1/W15 DIAGONAL WorldBasedTileWall - prop-only
  clearance misses rotated blockout walls; occupancy checks must include wall renderer AABBs.
- 1 agent lamp (W18) wedged between two bed frames -> grid-search relocation.
- 2 agent lamps landed on "Floor_W21_Pharmacy (1)" which is the R051 CORRIDOR exemplar piece,
  not a room -> deleted. Lamps follow the torch rule: rooms only, never corridor floors.
R066 rollout completed floor-wide: W11-W32 (39 placed, 37 kept), torches greened scene-wide
(157 total incl. corridor-adjacent flanks), 66 lamps / 359 shadowless point lights.

### Session 32 - 2026-08-05, hallway torch canon from the user's Floor_Corr (4) exemplar + rollout

```
RULE 068 | scope: global (hallway torches - amends R063's "rooms only, never hallways")
The user hand-placed 6 Torch_01_Lit in the W-E hallway on Floor_Corr (4) and asked for the
same in all hallways. Hallways now get WALL-MOUNTED torch pairs (R063's hallway ban is
lifted for this pattern; its pillar-stack recipe stays rooms-only):
- Prefab Torch_01_Lit at scale 1, upright rX 0 (NOT the rX270 mesh convention), scene root,
  "Torch_01_Lit (n)" naming.
- Mounted in FACING PAIRS on both flank walls: 0.23 m off the wall inner face (raycast the
  wall from the corridor centerline - floor edges lie on wide dupe floors like
  Floor_W21_Pharmacy (1)), root Y = local floorTop + 3.74.
- Yaw faces INTO the corridor: wall at max-z -> 180, min-z -> 0, max-x -> 270, min-x -> 90.
- Stations: ~2.5 m margin from each corridor end, pitch ~8.8 m, evenly spread; short
  corridors (<7 m) get one centered pair.
- Skip a station side when no wall is within 8 m (open junction plazas keep no torches) and
  when an existing torch is within 3 m (room-flank torches near mouths).
- Green light is an INSTANCE override (prefab asset ships warm 1/0.73/0.37; the R066 green
  0.35/1/0.45 int 2 range 6.5 shadowless lives on the instances) - always clone the user's
  exemplar instance, never InstantiatePrefab.
Applied 2026-08-05: 62 torches across 16 hallways (dry-run plan -> clone -> per-clone
Torch_Model bounds check vs exemplar +-30% -> envelope sweep). 2 planned spots on Floor_01's
south wall (z -91.4) rejected by the R054 envelope (z limit -90) and left unplaced - flagged
to the user. 6 junction floors skipped (no mounting wall in reach). POV-verified vs the
exemplar hallway.
Status: active
```

### Session 33 - 2026-08-05, stair-torch canon from the user's W21<->W22 stair exemplar + rollout

```
RULE 069 | scope: global (torches over staircases - extends R068 to stair passages)
The user placed a Torch_01_Lit pair over Stairs_W21_Pharmacy__W22_SpecimenLab_S2 (torches 70/71)
and asked for the same wherever stairs exist. Every staircase gets ONE facing pair at MID-RUN:
- Same mount grammar as R068 hallway torches: clone the user exemplar (green override), 0.23 m
  off the flank-wall inner face (raycast from the passage center at surface+3.5), yaw facing
  into the passage, scene root, "Torch_01_Lit (n)".
- Height follows the STAIR SURFACE: root Y = walking-surface Y at the stair's bounds-center
  (downward raycast, exclude torch colliders) + 3.834 (vs 3.74 for flat hallway floors).
- Run axis detected by sampling surface heights at the 4 bounds-edge midpoints - the axis with
  the larger height delta is the run; flank walls are on the cross axis.
- Standard skips apply: no wall within 8 m -> no torch that side; existing torch within 3 m ->
  skip (2 stairs already served by hallway torches were left alone).
- Stair inventory = active roots named Stairs_* / Stone_Stairs_01* / Stone_Stairs_02* with
  renderers, deduped for nesting (39 roots live under _Blockout/Walls/<room> groups).
Applied 2026-08-05: 72 torches over 36 staircases (exemplar + 2 near-torch stairs untouched),
all wall-raycast mounted, per-clone bounds check, envelope clean, POV-verified at the W21->W16
gate stair. Saved.
Status: active
```

### Session 32b - 2026-08-06, full-color fixture kits (user correction)

```
RULE 070 | scope: global (green/purple fixture coloring - supersedes the R066 tint recipe)
User: "The flame on the model emitting green VFX should also be green. The particles should be
green as well. If it is green, make it completely green; if it is purple, completely purple."
Tinting the warm flame gradient via FireColor/GlowTint reads muddy - colors now live in FULL
MATERIAL VARIANTS: M_FlameOuter/M_FlameCore (whole 3-stop gradient recolored), M_Ember (on
T_SoftRadial - the orange T_Ember texture muddies any tint), M_Glow, M_Magic, each x
_Green/_Purple in Torch_VFX/Materials/. Instance recipe: swap the kit onto Flame_Outer/
Flame_Core/Glow renderers + Ember/Magic particle system materials AND startColor, then set
controller FireColor = WHITE and flicker GlowTint = WHITE (serialized fields; they would
double-tint the colored materials). Classify fixtures by their Light color (green: g dominant;
purple: b dominant) - W1's two WARM aisle lamps stay warm, candles/chandeliers untouched.
NOTE: 134 user-copied torches carry no TorchVFXController/TorchFlicker on the root (stripped
during copying; lights fine, no flicker) - handle them by direct material swap. Flagged to the
user, not repaired.
Applied 2026-08-06: 164 green + 65 purple controller fixtures + 134 controllerless copies;
verified full-green torch and full-purple lamp POVs.
Status: active
```

### Session 32c - 2026-08-06, arch windows from the user W2 exemplar

```
RULE 071 | scope: rooms (Arch_Window_01 wall windows)
User exemplar in W2 (two on the north wall): scale 2.8 uniform, yaw facing into the room
(0 on a +z wall), window centre 4.04 m above the room floor top, 0.36 m inside the wall
line, ~6.7 m apart, offset roughly symmetric about the wall centre. Replicate by CLONING a
user instance, mapping the recipe onto the target wall (y = floorTop + 4.04,
inset = wallLine - 0.36), corridor-mouth check so no window lands over a doorway,
bounds-verify vs donor. Note: pre-existing s4.2 instances elsewhere (x 35-43, z 6-45) are
older user placements with their own scale - do not conform them.
Applied 2026-08-06: W4 north wall x 37.34 / 44.08 (y 0.37, z 75.24).
Status: active
```

R071 update 2026-08-06 (W5 + user correction): before placing windows, SURVEY the wall
segments (thin tall renderers near the wall line) - W5 north side is a BAY: one straight
panel x 56.4-63.6 flanked by rotY 45/315 diagonals. The two-window recipe only fits long
straight walls; my second window landed buried inside the 45-degree diagonal. The user
corrected W5 to ONE window centred on the straight panel (x 60.06 = panel centre).
Rule: windows go on straight panels only, centred when the panel is short (~7 m); never
span onto diagonal bay walls. W4 kept the two-window layout (long straight north wall).

R058 scale-table update 2026-08-11: Bed_01 canonical scale is now 160 uniform (was 125) -
user directive, applied to all 42 scene instances. Bed_01 pivot is AT THE FEET, so rescaling
does not lift the base; still always run a fresh-frame seat check after bulk scaling
(renderer.bounds read in the SAME execution as a transform change returns stale values -
verify in a second pass).
