# PROJECT

## Overview
**Bravehood** — a PvPvE extraction dungeon game. Under NDA, private @ Blackburne Games FZ LLC.

## Genre
PvPvE extraction (players raid a dungeon, fight AI + each other, extract with loot).

## Engine
Unity **6000.3.10f1**, Universal Render Pipeline (URP).

## Platforms
TBD — not yet documented.

## Camera
TBD — not yet documented.

## Core Pillars
- Extraction PvPvE: contested loot, risk/reward, multiple exits.
- Looped dungeon layout with no direct line-of-sight between spawns.

## Technical Standards
- **Room culling:** every dungeon scene gets a `DungeonCullingSystem` (`Bravehood.Optimization`, Assets/Core/Scripts/Optimization/) + generated `RoomCullingCell` graph (menu Bravehood → Optimization → Build Culling Cells). Far cells hide Renderers/Lights only; colliders/NavMesh/gameplay stay live. Scan roots + room-key regex are serialized per scene — reusable across dungeons.
- Editor automation via the **UnityMCP bridge** — `mcp__UnityMCP__execute_code` runs C# through CodeDom (**C# 6 only**: no `using` directives, no local functions, fully-qualify `UnityEngine.*`).
- Scene edits are scripted procedurally, then saved with `EditorSceneManager.SaveScene` / `MarkSceneDirty`.
- Git: work on feature branches (current: `leveldesign/dungeonA`); `main` is the integration branch.
- **Level design work goes through the LevelDesignAgent skill** (`.claude/skills/leveldesignagent/SKILL.md`) — blockout / gameplay / art-polisher modules + analysis toolkit + LEARNED.md placement rules.

## Repository as Memory
The `AI/` folder is the project's permanent memory. Read it at the start of every session; update it whenever something important changes. See `AI_WORKFLOW.md` (in the project owner's docs) for the full procedure.
