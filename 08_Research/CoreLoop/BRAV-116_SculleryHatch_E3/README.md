# BRAV-116 — Scullery Hatch E3 Tripo research

Approval package for the W12 Kitchen fast-extraction scullery hatch. These are
the downloaded Tripo research exports generated from the accepted BRAV-105
concepts on 2026-09-27.

## Accepted references

- `A1_SculleryHatch_Main.png` — archive branch `concepts/coreloop-accepted`
- `A1_SculleryHatch_Sash.png` — archive branch `concepts/coreloop-accepted`

## Generation settings and budget

- Tripo Smart Mesh P2.0, Triangle topology, Private, one generation per source
- Housing target: 4,500 triangles; result: 4,067 faces
- Sash target: 1,500 triangles; result: 1,430 faces
- Combined result: 5,497 faces (ticket budget: 6,000)
- Segmentation was run before texturing, using the requested middle tier:
  `Balanced (6-15 parts)` on both sources
- Texturing was generated after segmentation at 2K; FBX export uses current 2K
  texture resolution and Blender compatibility

## Packages

| Package | Tripo source job | Auto-segmentation | Notes |
|---|---|---:|---|
| `exports/Housing_Segmented_Textured.zip` | `e2355f2c-7196-4ebe-a721-663f59fcfd97` | Balanced; 100 parts shown in Tripo, 96 base-color images exported | Main wall/housing, counterweight hardware, crank, niche and dressing source. |
| `exports/Sash_Segmented_Textured.zip` | `24a5cebb-a3cf-4956-b51e-3a907b565556` | Balanced; 49 exported parts/base-color images | Standalone moving sash source. |

Each ZIP preserves the downloaded FBX and its `.fbm` base-color JPEGs. SHA-256
checksums and source byte sizes are in `MANIFEST.tsv`.

## Approval scope

Please review silhouette, proportions, material read, and whether these sources
are acceptable to take into Blender cleanup. The production piece must fit an
approximately 1.6 x 1.2 m opening with a 0.9 m sill and a roughly 3.2 x 3.0 m
wall-panel guide.

Approval does **not** mean the files are Unity-ready. Tripo substantially
over-segmented both exports despite the Balanced selection. Cleanup must merge
the pieces into the ticket's logical objects (`Housing`, `Sash`,
`Counterweight_L`, `Counterweight_R` and optional small props), establish the
required pivots and sockets, set real-world scale, reduce the material layout to
the ticket limit, and repack textures for URP.

