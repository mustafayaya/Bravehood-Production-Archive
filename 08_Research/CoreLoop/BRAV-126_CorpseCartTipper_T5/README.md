# BRAV-126 — Corpse-Cart Tipper T5 Tripo research

Approval package for the W29 Mortuary rail-head corpse-cart tipper. These are
the downloaded Tripo research exports generated from the accepted BRAV-105
concept set on 2026-09-27.

## Accepted references

- `B5_CorpseCartTipper_Armed.png` — geometry source for the complete armed mechanism
- `B5_CorpseCartTipper_Thrown.png` — motion-state and hinge-angle reference
- `A2_CorpseCart_Lever.png` — geometry source for the shared E4/T5 lever

All three references live on archive branch `concepts/coreloop-accepted` under
`00_Project/ArtDirection/Concepts/CoreLoop/`.

The armed and thrown sheets were not combined as multi-view inputs because they
show different mechanical states. Mixing those states would make the hinge pose
ambiguous. The thrown sheet remains the Blender cleanup/animation reference.

## Generation settings and budget

- Tripo Smart Mesh P2.0, Triangle topology, Private, one generation per source
- Armed assembly target: 3,000 triangles; result: 2,563 faces
- Shared lever target: 1,000 triangles; result: 989 faces
- Combined result: 3,552 faces (ticket budget: 4,000)
- Segmentation was run before texturing using the requested middle tier:
  `Balanced (6-15 parts)` on both sources
- Texturing was generated after segmentation at 2K; FBX export uses current 2K
  texture resolution and Blender compatibility

## Packages

| Package | Tripo source job | Auto-segmentation | Notes |
|---|---|---:|---|
| `exports/Assembly_Segmented_Textured.zip` | `dcebd5e2-5c97-4fef-97fe-f42f0ed5204b` | Balanced; 115 exported parts/base-color images | Armed rail bed, hinged plate, mount, ratchet and lever assembly source. |
| `exports/Lever_Segmented_Textured.zip` | `37b097a9-67fb-44bd-a895-e48c29269d65` | Balanced; 25 exported parts/base-color images | Standalone shared lever source for BRAV-126 and E4/BRAV-117. |

Each ZIP preserves the downloaded FBX and its `.fbm` base-color JPEGs. SHA-256
checksums and source byte sizes are in `MANIFEST.tsv`.

## Approval scope

Please review silhouette, proportions, material read, and whether these sources
are acceptable to take into Blender cleanup. Production fit is a roughly
1.6 x 1.0 m rail-plate section with the lever axle around 1.2 m high.

Approval does **not** mean the files are Unity-ready. Tripo substantially
over-segmented both exports despite the Balanced selection. Cleanup must merge
the pieces into `LeverMount`, `Lever`, `RailPlate`, and `RailBed`; establish the
lever and hinged-edge pivots plus interaction/audio sockets; set real-world
scale; reduce the material layout to the two-material ticket limit; repack
textures for URP; and ensure the standalone lever is the mesh shared with E4.

