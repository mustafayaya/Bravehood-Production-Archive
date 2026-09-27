# BRAV-123 — T2 Bed Clamps approval source

Visual-source approval package generated from the accepted Core Loop concept. This is not a Unity-ready production asset.

## Source and generation

- Geometry source: `00_Project/ArtDirection/Concepts/CoreLoop/B2_BedClamps_Armed.png`
- Motion/state reference only: `B2_BedClamps_Closed.png`
- Tripo job: `b775dba9-a588-40ce-a940-d34e5604e098`
- Smart Mesh P2.0, Triangle, one selected result, 5,000-face target
- Result: 4,783 faces; 6,828 vertices after segmentation/texturing
- Balanced segmentation: 136 generated parts
- Texture: standard 2K, Remove Lighting OFF
- Export: FBX, Blender compatibility, Pack UV OFF

## Review scope

Approve silhouette, proportions, restraint readability, and material direction before Blender cleanup. Tripo over-segmented the assembly; part consolidation, semantic naming, pivot placement, scale/orientation cleanup, material consolidation, collision, and Unity prefab work remain downstream.

The ZIP under `exports/` is the untouched Tripo download.
