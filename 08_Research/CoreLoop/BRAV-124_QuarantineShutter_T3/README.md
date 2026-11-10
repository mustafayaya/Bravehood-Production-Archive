# BRAV-124 — T3 Quarantine Shutter approval source

Visual-source approval package generated from the accepted Core Loop concept. This is not a Unity-ready production asset.

## Source and generation

- Geometry source: `00_Project/ArtDirection/Concepts/CoreLoop/B3_QuarantineShutter_Raised.png`
- Motion/plate reference only: `B3_QuarantineShutter_Dropped.png`
- Tripo job: `694af79f-83d3-482d-a9fe-3eb47ed4a0a5`
- Smart Mesh P2.0, Triangle, one generation, 6,000-face target
- Result: 4,888 faces; 3,458 vertices after segmentation/texturing
- Balanced segmentation: 256 generated parts
- Texture: standard 2K, Remove Lighting OFF
- Export: FBX, Blender compatibility, Pack UV OFF

## Review scope

Approve the raised-state doorway silhouette, proportions, counterweight read, and material direction before Blender cleanup. The dropped concept was not used as geometry because its doorway proportions are not authoritative. Tripo severely over-segmented the assembly; consolidation, semantic naming, pivots, scale/orientation cleanup, material consolidation, collision, and Unity prefab work remain downstream.

The ZIP under `exports/` is the untouched Tripo download.
