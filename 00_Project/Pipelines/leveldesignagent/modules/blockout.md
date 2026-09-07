# Module 1 — Blockout

How Bravehood dungeon floors are blocked out, learned from DungeonA Floor 1. Use this module when creating a NEW floor/dungeon or (only on explicit request) editing frozen geometry.

## Topology grammar

- **Spine + grid + loops.** A central spine of hero rooms (W1 Cathedral → W15 Triage → W16 Hub → W20 Reliquary → W28 Chapel) flanked by a 5×5 grid of side rooms. Every room has ≥2 exits; the whole floor is fully looped (no dead-end rooms except gated POIs).
- **Hub room** (W16): the nexus, 6 corridor approaches, ring of interior pillars (3×4 grid) for cover.
- **Spawn separation:** the two spawns sit at opposite ends of the spine (~172 m apart in DungeonA) with **no direct line-of-sight** between them; both enter from elevated positions.
- **Gated POIs:** miniboss (W30) and optional loot room (W31) sit off the grid behind LockedDoors; extraction W6 is behind a LockedDoor too; OneWayGates create asymmetric routing pressure (W20, W22, W29).

## Room definition format (the room table)

Each room = one row: `W# | Name | center (X,Z) | shape | elevation | role/notes`. Shapes used: `rect WxD`, `octagon rN`, `circle rN`, `cross/cathedral`, `bulb rN`. DungeonA's full table lives in `AI/DUNGEON.md` + `Assets/Levels/Dungeon1/DungeonA_Floor01_Blockout_Plan.md`. Scene mirror: empty markers under `Rooms/` named `W#_Name` at the room center — create these first; everything else derives from them.

Sizing heuristics (post-scale, world meters): smallest utility rooms ~14×12 (W18/W24/W27), standard side rooms ~20×20, hero octagons r15–r16, biggest hall ~40×44 (W25). Central naves/corridors between spine rooms are **8 m wide**; side corridors ~4–5 m.

## Kit + dimensions (base bounds, pivot at floor, BEFORE the ×0.45454 scene scale — DungeonA was built then uniformly scaled about origin)

- `Wall_01` (Assets/Levels/Dungeon1/NewAssets/Prefabs): 4.552 L × 4.800 H × 0.905 T; visible body 4.176 — **grow X-scale until bodies overlap** to avoid seam gaps. Rotate walls 180° so the clean face points into the room.
- `Floor_01`: 2.212 × 2.212 (but DungeonA floors are `Plane.prefab` instances with size-based `Stone_01` materials tiled to ~2.885 m stones — do not revert to flat 3×3 tiling).
- `Corner_Pillar_01`: closes wall-meeting corner gaps (avoid `Prefabs/Column_01` — renders magenta and lies horizontal).
- Post-scale wall height ≈ 5.28 m (~5.8× character), segment ≈ 2.4 m.

### Wall system v2 — WorldBasedTileWall (user-taught, W16 final 2026-07-14 — **use everywhere**)

The kit-Wall_01 approaches (segment grids, stacked tiers) are superseded for interior room shells. The new standard:

- **Panel = plain Unity Cube named `WorldBasedTileWall`** + world-space-tiling material `Stone_01TileTest 1` (see LEARNED R014/R015). No prefab, no UV work — the shader tiles in world space, so one GPU-instanced material fits every panel size.
- **Standard panel scale (13.32, 11.92, 1.00)** — 13.3 m wide × 11.9 m tall × 1 m thick. Narrower fills 11.47/7.41/5.92/3.46; door header scale.y 4.18; thin partition scale.z 0.37.
- **Rotate the cube to the face angle** — octagon diagonals are single rotated panels (W16: 45/88/135/178/224/270/321).
- **Border cornice mandatory on tall panels:** `border.prefab` (s=3.25, ~3.2 m segments) tiled along the top at ~8.0–8.2 m above floor; also crowns freestanding two-story shelf walls (~9.1 m). A bare tile-wall top reads unfinished.
- Kit `Wall_01` remains for the frozen corridor/room shells not yet remodeled; diagonal-rotation + X-scale-consolidation rules from session 1 still apply to it.
- **Retrofit technique (applied W15):** for a room that still has a kit-wall shell, overlay one tile panel per wall segment, **pivot centered on the kit wall's bounds center** (panel face ends up ~0.05 m inside the old face — no z-fighting, minimal room loss, door gaps stay open because panels only span the solid segments). Extend panel length over adjacent non-door seam gaps; never across a doorway.
- **Seam closure (user-taught, session 5; scoped down sessions 7–8 — R027/R032):** cover panel/wall junction gaps with **full-height Corner_Pillar_01** — non-uniform scale **(216.4, 216.4, 632.6)**, vertically centered (one pillar = full ~12 m wall height). **Pragmatic-only placement:** pillar a junction only where the seam ACTUALLY reads (inspect corners after paneling — short panels are the usual culprits); 45° diagonal junctions and corridor mouths usually do, plain 90° corners usually don't. Doors do NOT automatically get flank pairs — the DoorStructure arch + header often suffice (W11's S door has none). Orient the pillar's decorated face toward the room/opening (W11: rot (270, 0, 0) at the west flank, not the blanket 180 yaw). Parent under root `Corner_Pillars`. (The 2-tier stacks stay only inside DoorStructure groups.)
- **Corridor gate sequences (user-taught, session 15 — R046, W16):** a major door's treatment continues INTO its corridor: door header + DoorStructure arch at the door line, then a second Stone_Archway_01 ~4–5 m into the corridor, a corridor-side tile panel, two full-height Corner_Pillar_01 framing the corridor ~8 m out, and a fitted Stone_Stairs_02 where the corridor steps. Jamb stacks go 3-TIER (Δy ≈ 4.08–4.15) on full-height diagonals; corners may use PAIRED-YAW pillar clusters (e.g. 90° + 65.8° together) for mass. Walls consolidate into fewer 13.32-wide panels wherever possible.
- **Corridor-owned interior faces (user-taught, session 10 — R037):** after converting a room, sweep ALL its interior faces for visible legacy kit walls — including faces owned by `Corridors`/`GapFill` groups — and overlay a tile panel on the room side (W1's NE alcove north face got an 8.17-wide panel over the corridor's Wall_01). The frozen rule protects corridor geometry, not its looks. Delete any seam pillar the new panel makes redundant.
- **Door tuning in the art pass (user-taught, session 12 — R043):** (1) DEAD DOORS — blockout gaps that fail the corridor cross-check get sealed: extend full-height panels over the gap, delete the header, keep the DoorStructure arch as a blind decorative arch on the solid wall (W25 W door). Agent may do this when the cross-check confirms no corridor. (2) NARROWING — a wide door can be reduced by a full-height infill panel inside one end of the arch span, with the flank seam pillar moved to the new passage edge (W25 S door: 7.9 m arch → 6.3 m passage); user-initiated only.
- **Oversized openings (user-taught, session 8 — R031):** when a blockout opening is wider than its corridor, keep the full-width grand arch as a decor frame and fill the dead span with a **partial-height panel under the arch** (W11: scale (4.21, 7.52, 1), bottom sunk below floor, top at the arch crown ~7.2 m) — the live passage stays aligned to the corridor, the rest reads as a bricked-up archway. Never invent full-height walls over the opening, never leave the dead span open.
- A room's cover pillars may live under `_Blockout/Walls/<room>/Pillars` — interior columns are `Column_01` at s=2.40 (pedestal base; correct rendering at this scale).

### Ceiling architecture (user-taught, W16 final — the pattern for big rooms)

Layered vault, three heights: **side-aisle gallery ceiling** = kit Ceiling_01 panels (s=239.86) at ~8.5 m above floor → **flat aisle ceiling** = `WorldBasedTileCeiling` cubes (scale 11.27 × 30.18 × 1.83, rX=90, "Ceiling" world-tile material) underside ~9.5 m → **central barrel vault** = `ArcCeiling.prefab` (s=4.07) one arc per ~9.7 m of nave at ~11.9 m. Arc over the void, flat over the aisles — the height step is the architecture. All parented under `Props_Dressing/<room>`.

### Gates — the DoorStructure standard

Every doorway/pathway gets a **`DoorStructure` group** (one parent per door, under `Props_Dressing/<room>`):

- `Stone_Archway_01` (`Prefabs/Stone_Archway_01.prefab`, **s=250**, rX=270, yaw across the passage) spanning the opening at the wall line, base sunk ~0.05 into the floor. At s=250 it's ~4.8 m wide — scale proportionally for narrower doors (s≈208 → 4 m, s≈172 → 3.3 m).
- Two **jamb stacks** flanking the arch ends: each = 2 stacked `Corner_Pillar_01` (s=228.22, rX=270, tiers at floor+1.82 / floor+5.97).

Reference instances: `Props_Dressing/W16_GrandInfirmary_Hub/DoorStructure` (original), `W15_TriageHall/DoorStructure_N/S/W1/W2` (applied). Legacy ungrouped jambs live in the root `Corner_Pillars` group (~284 pieces).

## Corridors & doors

Straight, **orthogonal only** (H/V). A corridor connects a room pair along their overlap centerline; the door is carved into the wall at that centerline (leave a gap ≥2 wall segments ≈ 4 m for naves, ≥1 segment for side doors). Corridors carry their own floor strips and flanking walls under `_Blockout/Corridors`. After walls, run a **GapFill pass**: any perimeter gap that isn't a door gets a fill wall under `_Blockout/GapFill`.

## Elevation & stairs

- Elevations step in **±2 m** increments per room transition (range −6..+6 in room-table terms).
- `Stone_Stairs_01` (5-step) for short runs; `Stone_Stairs_02` (13-step) for long corridor drops.
- **Stone_Stairs_02 fitting math** at euler `(270,90,0)` (run along world Z), prefab native 0.019×0.0184×0.0148 m: `scale.x = run/0.019`, `scale.y = width/0.0184`, `scale.z = rise/0.0148`. Then translate by *measured renderer bounds* so `min.y` sits on the lower floor and the low-Z end overlaps the lower floor by ~1 m. Overlap ~1 m at both ends, no gaps.
- Auto-placed stairs live under `_Blockout/Stone_Stairs_Auto`, manual ones under their own roots.
- **Validation (mandatory):** scan all floor tiles, cluster by top-Y, find adjacent tiles whose top-Y differs by >0.5 m without a stairs bounds bridging them → report unbridged transitions. DungeonA passes with 0 (54 tiles vs 44 stairs).

## Cover

Interior cover under `_Blockout/Cover`: pillar rings in hero rooms (named `Pillar_<Room>`), scattered `Cover_Crate` boxes in long corridors and open halls. Purpose: break spawn-to-spawn and door-to-door sightlines at player eye height (~0.85 m) — verify with the toolkit's POV renderer.

## Naming & hierarchy conventions

- Roots: `_Blockout/{Floors,Walls,Corridors,Cover,GapFill,Stone_Stairs_Auto}`, `Rooms/`, `Props_Dressing/`, `Gameplay/`.
- Wall groups: `_Blockout/Walls/W#_Name` (one group per room, matching the `Rooms/` marker name exactly).
- Sub-rooms/annexes get a letter suffix: `W2b_Vestibule`, `W9b_SideWard`.
- Scene scale: build at kit-native size, then uniformly scale the whole floor **×0.45454 about origin** once, at the end (DECISIONS.md records this for DungeonA). Never rescale twice.

## Workflow for a new floor

1. Write the plan doc (`Assets/Levels/DungeonX/..._Blockout_Plan.md`) with the room table + corridor list; get user sign-off.
2. Create `Rooms/` markers → floors → walls per room → corridors + doors → GapFill + corner pillars → stairs (auto pass + fitting math) → cover.
3. Run the elevation-transition scan (0 unbridged), then the boundary extractor on every room (doorway count must match the plan's corridor list).
4. Uniform-scale about origin, save, update `AI/DUNGEON.md` room table, mark geometry FROZEN.
