---
name: leveldesignagent
description: LevelDesignAgent — design, analyze, dress, and polish Bravehood dungeons (blockouts, gameplay elements, prop placement/art pass, room lighting & fire/crystal VFX) through Unity MCP. Use whenever the user asks about level design, dungeon layout, blockout, room dressing, prop placement, spawns/extractions/objectives, enemy or loot positions, art polish, torches/lamps/candles/chandeliers/crystals/flames/lighting palette, or wants to teach placements by hand-placing models. Routes to 4 modules — blockout, gameplay, art-polisher, lighting-vfx — plus a shared analysis toolkit (room boundaries, player-camera POV, snapshot/diff teaching loop).
---

# LevelDesignAgent — Bravehood dungeon level design

Bravehood is a **PvPvE extraction dungeon crawler**: enter dungeon → fight AI + rival players → loot → extract; dying loses the run. Third-person over-the-shoulder camera (Cinemachine follow, 2.0–3.5 m behind). Art style: **stylized low-poly**, gothic sanatorium, teal/blue-cyan glow + warm candle gold.

**The visual target** (final-quality reference art): dense candle-lit stone halls; a glowing azure hero crystal on a raised rune-circle dais as the focal point of each key room; secondary crystal pedestals; teal rune circles inlaid in the floor; gold trim on stairs/columns/platforms; crates, barrels, urns as mid-ground clutter; strong warm-vs-cool light contrast (candles vs crystals). Every room should have a clear focal hierarchy readable from its doorways at player eye height.

## The measuring stick

Judge every size against the **live `PlayKit/Player` (~2.0 m tall, scale 1.0)** in the scene — not world units. W16-final proportions: wall panels 11.9 m ≈ 6× player, shelf tiers 3.63 m ≈ 1.8×, border cornice at ~8–9 m, gate column tiers 4.15 m ≈ 2×. (The older 0.91 m `NewRoom_01 (7)` reference described the pre-teaching character; historical docs may still cite it.)

## Hard rules (non-negotiable)

1. **Geometry is FROZEN** — except rooms the user is actively remodeling (currently **W16**, rebuilt by the user 2026-07-13 with the new wall techniques in `modules/blockout.md`). Never move/resize/re-material/regenerate `_Blockout/Walls`, `_Blockout/Floors`, `_Blockout/Corridors`, `_Blockout/GapFill` wholesale or in untouched rooms unless the user explicitly asks. `Props_Dressing/*` and `Gameplay/*` stay mutable.
2. **Unity MCP `execute_code` is C# 6 (codedom)**: no `using` directives, no local functions, no string interpolation edge features beyond C#6, fully-qualify (`UnityEngine.GameObject`, `UnityEditor.SceneManagement.EditorSceneManager`). Always `MarkSceneDirty` + `SaveScene` after mutations, and `read_console` after any script work.
3. **Gameplay markers are not props.** Yellow spheres = Objectives, green = Extractions, cyan cylinders = Ladders, cyan cubes = Balconies, purple = OneWayGates/LockedDoors. Never delete/move them during an art pass.
4. **Docs of record**: after any level work, update `AI/DUNGEON.md` (what changed) and `AI/TASKS.md` (status). Read them before starting.
5. **Auto-orient + ground every placed prop** (flattest face down, Y-yaw only, base on floor Y, character-relative scale) — see [toolkit.md](toolkit.md).

## Connecting to the editor

Prefer the harness's unityMCP tools. If they report **"No Unity Editor instances found"**, the editor's own HTTP MCP hub is at `http://127.0.0.1:8080/mcp` (verify: `lsof -iTCP:8080 -sTCP:LISTEN` shows Python `mcp-for-unity`; pidfile `Library/MCPForUnity/RunState/mcp_http_8080.pid`). Speak JSON-RPC over curl: `initialize` (capture `mcp-session-id` response header) → `notifications/initialized` → `tools/call` with the normal tool names (`execute_code`, `manage_scene`, `manage_camera`, `read_console`, …). Responses arrive as SSE `data:` lines.

## Scene contract — DungeonA Floor 1

`Assets/Levels/Dungeon1/DungeonA_Floor01_Blockout.unity` — 36 rooms, spine + 5×5 grid, fully looped.

```
_Blockout/            FROZEN geometry
  Floors              33 groups (87 Plane tiles, size-based Stone_01 materials, ~2.885 m stones)
  Walls/W#_Name       per-room wall groups (~533 walls total, rotated 180°, clean face inward)
  Corridors           42 straight orthogonal corridors (own floors + walls)
  Cover               pillars (Cathedral×4, Hub×12, Triage×2) + 9 Cover_Crates  ← mutable-ish, ask first
  GapFill             50 seam-closing walls
Rooms/                36 empty center markers W1..W32 (+W2b/4b/9b/21b) — the room-position source of truth
Props_Dressing/W#_*   per-room dressing (mutable; ~739 props currently, many stand-ins)
Gameplay/             Spawns, Extractions, Miniboss, Objectives, VerticalAccess_Ladders,
                      VerticalAccess_Balconies, LockedDoors, OneWayGates, Stairs (marker objects)
```

Room elevations run −11.7 (W31) to +6.3 (W3); floor-top Y per room = the `Rooms/W#` marker Y + 0 (markers sit at floor level ~ elevation − 1.7 offset convention; always measure the actual floor with the boundary extractor rather than trusting a constant).

## Modules

| Request smells like | Load |
|---|---|
| New floor/dungeon layout, rooms, corridors, walls, stairs, elevations, cover | [modules/blockout.md](modules/blockout.md) |
| Spawns, extractions, objectives, miniboss, ladders/balconies, locked doors, one-way gates, enemy/gem/chest placement | [modules/gameplay.md](modules/gameplay.md) |
| Dressing a room, prop placement, art pass, replacing stand-ins, "make it look like the reference" | [modules/art-polisher.md](modules/art-polisher.md) |
| Torches, floor lamps, candles/candelabras, chandeliers, crystal VFX, flame VFX, room lighting palette (green/purple/warm), lamp audits after user edits | [modules/lighting-vfx.md](modules/lighting-vfx.md) |
| Any spatial analysis (room boundaries, free floor, player POV shots, snapshot/diff) | [toolkit.md](toolkit.md) — shared by all modules |

Always also read [LEARNED.md](LEARNED.md) — user-taught placement rules override module defaults. Session 1 (W16, 2026-07-13) established canonical prop scales, the rX=270 upright rule, two-story shelf architecture, and the candle point-light + reflection-probe rig. Session 2 (2026-07-14) completed the room and set the **environment-shell standard**: `WorldBasedTileWall` cube panels + `MotoX/Environment/WorldSpaceTiling` materials (GPU-instanced, reusable everywhere), border-prefab cornices on all tall wall/aisle tops, layered ceiling (gallery 8.5 m → flat aisle 9.5 m → ArcCeiling barrel vault 11.9 m), Stone_Archway + double-stacked Corner_Pillar gate jambs, interior Column_01 pillars with sand-mound bases. **`Props_Dressing/W16_GrandInfirmary_Hub` is the finished gold-standard room** — copy its patterns, not the older rooms'.

## Teaching loop (user hand-places → agent learns)

1. User announces "I'll teach room W#" → **snapshot** the room (toolkit → Scene snapshot) to `snapshots/W#_<label>_before.json`.
2. User places/adjusts models in the editor by hand.
3. User says done → snapshot again (`..._after.json`) → **diff** (toolkit → Snapshot diff): added/moved/rotated/scaled/deleted, each with center-relative, nearest-wall-relative, and nearest-door-axis-relative coordinates.
4. Derive candidate rules from the diff: wall offsets, sibling spacing, facing (toward hero/door/center), scale vs character, symmetry groups. Present them to the user in plain language.
5. On confirmation, append to [LEARNED.md](LEARNED.md) (format documented there). When a rule proves stable across ≥2 rooms, fold it into `modules/art-polisher.md` and note the promotion in LEARNED.md.

## Definition of done for any room work

- Boundary respected (nothing intersecting frozen walls, no doorway blocked — doorways from the boundary extractor).
- Props auto-oriented, grounded, character-scaled.
- Player-camera POV screenshots taken from each doorway + room center (toolkit) and composition checked: hero visible from every entrance, silhouettes readable, cover sightlines intact.
- Gameplay markers untouched; scene saved; console clean; `AI/DUNGEON.md` + `AI/TASKS.md` updated.
