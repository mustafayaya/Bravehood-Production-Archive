# BRAV-125 — T4 Sterilisation Lamps approval source

Visual-source approval package generated from the accepted Core Loop concept. This is not a Unity-ready production asset.

## Source and generation

- Geometry source: `00_Project/ArtDirection/Concepts/CoreLoop/B4_SterilisationLamps_Main.png`
- Tripo job: `eaf9bc54-c2bc-4660-81f5-bc82355d0260`
- Smart Mesh P2.0, Triangle, one generation, 7,000-face target
- Result: 6,548 faces; 9,781 vertices after segmentation/texturing
- Balanced segmentation: 158 generated parts
- Texture: standard 2K, Remove Lighting OFF
- Export: FBX, Blender compatibility, Pack UV OFF

## Review scope

Approve silhouette, hanging-fixture proportions, lamp readability, and material direction before Blender cleanup. Tripo over-segmented the assembly; part consolidation, semantic naming, chain/pivot cleanup, scale/orientation cleanup, material consolidation, collision, emissive setup, and Unity prefab work remain downstream.

The ZIP under `exports/` is the untouched Tripo download.
