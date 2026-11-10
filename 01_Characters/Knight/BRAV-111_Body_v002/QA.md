# Final QA — 2026-09-26
PASS: Blender4.3.2 FBX reimport. Exactly Knight_Head/Knight_Torso/Knight_Legs; identity transforms, shared floor origins, report-matching bounds/counts, valid finite UVs and coordinates. Six linked2048-square PNGs (basecolor/normal).

8825 geometric vertices /17470 triangles. Zero triangles below area1e-12 after cleanup. Estimated rendered vertices35375 (Head28562, Torso4590, Legs2223); not an actual Unity measurement and exceeds the earlier18–25k render-vertex target.

All169 tested armpit triangles survive processing with matching winding within1e-5 positional tolerance. Raw bilateral underarm check:271 edges, zero boundaries/nonmanifold edges/winding conflicts after diagnostic positional welding. Underside views show closed surfaces.

Not tested: posed deformation, master-body fit, actual Unity import/runtime rendering. No rig or live game assets changed. The old body's reported visible defect was not reproduced by static geometry tests.
