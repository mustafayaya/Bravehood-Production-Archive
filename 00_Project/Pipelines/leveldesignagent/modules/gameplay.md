# Module 2 — Gameplay

Placement of gameplay elements: spawns, extractions, objectives, miniboss, vertical access, gates, and (future) enemy/gem/chest spawners. All live under the scene's `Gameplay/` root as marker objects.

## Marker vocabulary & visual code

| Category | Parent group | Marker | Naming |
|---|---|---|---|
| Player spawns | `Gameplay/Spawns` | green, scale 4 | `Spawn_A_<Room>`, `Spawn_B_<Room>` |
| Extractions | `Gameplay/Extractions` | green sphere, scale 4 | `Extract_<Room>_W#`, `Descent_to_Floor2` |
| Objectives | `Gameplay/Objectives` | yellow sphere, scale 3.2 (3.6 optional) | `Obj_<What>_W#`, `Optional_<What>_W#` |
| Miniboss POI | `Gameplay/Miniboss` | scale 5 | `Miniboss_<Name>_W#` |
| Ladders | `Gameplay/VerticalAccess_Ladders` | cyan cylinder, scale 2.2 | `Ladder_W#` |
| Balconies | `Gameplay/VerticalAccess_Balconies` | cyan cube, scale 2.6 | `Balcony_<Name>_W#` |
| Locked doors | `Gameplay/LockedDoors` | scale 2.2 | `LockedDoor_<Room>_W#` |
| One-way gates | `Gameplay/OneWayGates` | purple, scale 2.2 | `OneWayGate_<Room>_W#` |
| Stair markers | `Gameplay/Stairs` | scale 2.0 | `StairsUp/Down_<From>_to_<To>` |

Markers float above floor (spawns/objectives at ~+4..+6 over floor Y so they're visible in scene view). They are design intent, not runtime systems.

## Placement principles (derived from DungeonA Floor 1)

- **Spawns:** two, at opposite ends of the spine (DungeonA: (-8, 84.8) vs (-8, -87.2) ≈ 172 m apart), entering from elevated positions, no direct LOS. A spawn room is dressed lightly (vestibule feel), never contains loot objectives.
- **Extractions:** ≥2 exits per floor, asymmetric: one lateral (W6 Waste Disposal, far corner, behind a **LockedDoor**, adjacent to the best loot wing) and one "descend deeper" (Chapel W28 → Floor 2, next to Spawn B — so extracting there is contested by fresh spawners). Extraction ≠ spawn room, but adjacency to a spawn is a deliberate risk lever.
- **Objectives:** 6–8 per floor, on hero/themed rooms spread across all four quadrants (W2, W5, W9, W18, W19, W21, W28 + optional W31) so no single route covers them all. Each sits at the room's focal point (usually above the hero dais).
- **Miniboss:** one, mid-south (W30), behind a LockedDoor, guarding the richest optional wing (W31 optional objective next door, also gated).
- **LockedDoors** gate: miniboss, optional loot room, and the lateral extraction. **OneWayGates** (3) create no-return shortcuts from mid-map into the south (W20 Reliquary, W22 SpecimenLab, W29 Mortuary) — pressure toward endgame areas.
- **Vertical access:** Ladders (6) connect floor↔balcony inside rooms; Balconies (5) overlook hero rooms (Chapel, Observation, Surgery, Library, Bell Tower) giving ranged/ambush positions above the ground loop.
- **Stair markers** document every elevation route by name (`StairsDown_Hub_to_WardMatron`, …) — keep them in sync with actual `_Blockout` stairs.

## Room-boundary-aware placement

Before placing/moving any gameplay element, run the **room boundary extractor** ([../toolkit.md](../toolkit.md)) for the target room. Rules:

- Element must sit inside the interior footprint, ≥1 m from walls.
- Never inside a doorway gap or blocking the door axis (doorways come from the extractor).
- Objectives/extractions go at the focal point (room center or hero dais); spawn-facing elements are checked with the POV renderer — an objective should be visible from at least one doorway.
- Cross-check elevation: marker's implied floor = extractor `floorY`, not a guessed constant.

## Future systems — reserved conventions (not yet implemented)

No spawner/loot C# systems exist yet (`AI/TASKS.md`). When they arrive, follow:

- Parent groups: `Gameplay/Spawners/Enemy_W#_<Archetype>_<n>`, `Gameplay/Loot/Chest_W#_<n>`, `Gameplay/Loot/Gem_W#_<n>`.
- **Enemies:** `SimpleMobAI` needs a baked NavMesh (not yet baked — bake before first enemy pass). Detection range 15, attack range 2.5 → space patrol anchors ≥15 m from spawn rooms so fresh players aren't instantly aggroed; place mobs at chokepoints and objective rooms, density scaling toward the south/gated wing.
- **Chests/gems (loot):** richest loot behind gates (W30/W31 wing), medium at objectives, light scatter on loop routes; loot value should correlate with distance-from-spawn and gate count. Chest stand-in asset: `Infirmary_Models/Chest_01`; gem stand-ins: `Treasure Room/` pack + `Azure_Crystal_01`.
- Every spawner/loot marker must pass the same boundary + doorway rules above.

## Current DungeonA gameplay layout (snapshot 2026-07-13, world coords)

Spawns: A (-8, 4.3, 84.8), B (-8, 0.3, -87.2). Extractions: W6 (90, -1.7, 60.8), Descent (0, -5.7, -87.2). Miniboss W30 (40, -3.7, -41.2). Objectives: W28, W9, W5, W2, W21, W18, W19, optional W31. Ladders: W2/W8/W20/W21/W22/W32. Balconies: W3/W9/W28/W25/W19. LockedDoors: W30/W31/W6/W28. OneWayGates: W29/W20/W22. Re-derive live positions with `find_gameobjects` or the toolkit rather than trusting this table after edits.
