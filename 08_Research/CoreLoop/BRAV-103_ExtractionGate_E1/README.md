# BRAV-103 — Extraction Gate E1 Tripo research

Approval package for the W6 Waste Disposal extraction gate. These are the
downloaded Tripo Smart Mesh P2.0 research exports generated from the accepted
BRAV-102 concept set on 2026-09-26.

## Settings and budget

- Smart Mesh P2.0, Triangle topology, Private
- Reference-derived texture pass on every source
- Correct-source geometry total: 7,136 faces (ticket budget: 8,000)
- Housing: 4,557 faces
- Leaf_L: 933 faces
- Leaf_R: 896 faces
- LockBox + bolt bar: 750 faces

## Packages

| Package | Tripo source job | Auto-segmentation | Notes |
|---|---|---:|---|
| `exports/Housing_FullGate_Segmented_Textured.zip` | `1a80d34d-fa6d-4712-bc0a-cce3a01e0349` | Balanced, 159 parts | Main+Back source. Contains the full gate silhouette; housing-only separation is still required. |
| `exports/Leaf_L_Segmented_Textured.zip` | `ff5dc02d-a1eb-4510-820e-cf9a9b725dc4` | Simple, 33 parts | Standalone left leaf source. |
| `exports/Leaf_R_Segmented_Textured.zip` | `3d33b19b-1f22-45dc-a99c-3d73448b9dd6` | Simple, 24 parts | Standalone right leaf source. |
| `exports/LockBox_BoltBar_Segmented_Textured.zip` | `ebaf1500-6805-4978-aaf7-e1627f048b2b` | Simple, 44 parts | Standalone lock box and bolt-bar source. |

Each ZIP preserves the downloaded FBX and its `.fbm` base-color JPEGs. SHA-256
checksums and source byte sizes are in `MANIFEST.tsv`.

## Approval scope

Please review silhouette, proportions, material read, and whether these sources
are acceptable to take into Blender cleanup. The gate must ultimately fit the
3.6 × 5.0 m opening with an approximately 5.2 × 6.2 × 1.2 m housing.

Approval does **not** mean the files are Unity-ready. Tripo substantially
over-segmented all four exports. The cleanup pass must consolidate them into the
ticket's logical objects (`Housing`, `Hinge_L/Leaf_L`, `Hinge_R/Leaf_R`, and
`LockBox` + bolt bar), establish pivots and scale, remove duplicated full-gate
parts from the housing source, and prepare the final material layout.

The two earlier batch-image trials are intentionally excluded from this package.

