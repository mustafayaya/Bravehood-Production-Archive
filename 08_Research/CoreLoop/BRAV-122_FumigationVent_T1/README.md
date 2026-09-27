# BRAV-122 — T1 Fumigation Vent approval source

Visual-source approval package generated from the accepted Core Loop concept. This is not a Unity-ready production asset.

## Source and generation

- Geometry source: `00_Project/ArtDirection/Concepts/CoreLoop/B1_FumigationVent_Main.png`
- Motion/state reference only: `B1_FumigationVent_OpenState.png`
- Tripo job: `fe7bc289-da8d-48de-a11f-150d94846480`
- Smart Mesh P2.0, Triangle, one generation, 4,000-face target
- Result: 3,665 faces; 3,161 vertices after segmentation/texturing
- Balanced segmentation: 78 generated parts
- Texture: standard 2K, Remove Lighting OFF
- Export: FBX, Blender compatibility, Pack UV OFF

## Review scope

Approve silhouette, proportions, material direction, and the generated mechanical source before Blender cleanup. Tripo over-segmented the assembly; part consolidation, semantic naming, pivot placement, scale/orientation cleanup, material consolidation, collision, and Unity prefab work remain downstream.

The ZIP under `exports/` is the untouched Tripo download.
