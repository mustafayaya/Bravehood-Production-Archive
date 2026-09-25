# BRAV-111 — Accepted Knight body image research

2026-09-25. Mustafa explicitly accepted all final B1–B5 outputs and requested this research archive. Image approval is complete. Figma placement, 3D conversion and master-rig work remain pending. Jira: https://blbrn-team.atlassian.net/browse/BRAV-111

## Archive status

Filed under `08_Research/Characters/Knight/BRAV-111_Body/`. These are accepted image references and research, not a finished 3D body or rig. Clean image bytes and QA records are copied unchanged from the generation workspace; original source files are preserved. Rejected iterations are excluded from this final-output archive.

## Files

- `Knight_Body_B1_FrontA.png`: primary measurable reference, accepted candidate09.
- `Knight_Body_B2_BackA.png`: rear, corrected heel orientation.
- `Knight_Body_B3_LeftProfileA.png`: left profile, corrected upper-arm length/elbow position.
- `Knight_Body_B4_ThreeQuarter.png`: form reference; mild perspective, not a metric view.
- `Knight_Body_B5_Hands.png`: both hands dorsal and palmar, five digits each.
- `qa/B1_Measurement_Overlay.svg`: six-head ruler and measured landmarks; import into Figma. Not uploaded to Figma. Keep labels out of image-to-3D inputs.
- `qa/B1_measurements.json`, `qa/prompts.json`, `qa/SHA256.json`: observations, complete prompt set, provenance and file hashes.

Built-in image_gen used; seed not exposed. B1 used the accepted BRAV-106 text-free face/style reference at art commit ef5911c; its body proportions were not adopted. B1 needed nine attempts. B2–B5 were generated only after B1 passed the independent visible-landmark check. PNG B1–B4 1024×1536; B5 1254×1254. RGB, all five decode successfully. Visually white backgrounds, no cast shadows/text; corner channels 252–255, not uniformly exact #FFFFFF.

## B1 measurements

Crown y30, sole y1441, total 1411px; chin under beard y258 → **6.19 heads**. Fractions measured upward from soles: `(1441-y)/1411`. Landmarks are visually annotated, not automatically detected bones. All directly visible joint-height rows pass the Jira ±0.020 tolerance. User explicitly chose literal Jira heights over arm-elevation reinterpretation.

| Landmark | Target | Measured | Delta | Evidence |
|---|---:|---:|---:|---|
| Crown | 1.000 | 1.000 | 0.000 | visible |
| Eyes | 0.917 | 0.917 | 0.000 | visible |
| Chin incl. beard | 0.833 | 0.838 | 0.005 | visible |
| Shoulder line | 0.780 | 0.787 | 0.007 | visible contour |
| Armpit | 0.700 | 0.705 | 0.005 | visible |
| Elbow | 0.590 | 0.603 | 0.013 | visible sleeve hinge |
| Waist seam | 0.560 | 0.574 | 0.014 | visible |
| Hip | 0.500 | 0.499 | -0.001 | inferred under tunic; plausible y710–765 |
| Wrist | 0.475 | 0.473 | -0.002 | visible cuff/hand boundary |
| Fingertips | 0.350 | 0.355 | 0.005 | visible |
| Knee | 0.250 | 0.259 | 0.009 | visible trouser hinge |
| Ankle | 0.042 | 0.056 | 0.014 | inferred inside boot; plausible y1355–1370 |

Hip and ankle are covered by the mandated tunic/boots. Their plausible image positions are compatible with the table but are **inferred**, not directly verified. The 3D artist must place and verify these joints against the table, not the tunic hem or boot cuff. Shoulder contour width449/1411=.318 vs target.310 is a silhouette proxy, not certified skeletal centers. Heel spacing estimate377/1411=.267 vs nominal.280; use the nominal joint/stance targets in 3D. Nose width41px / cheekbone face width119px=.345 (≥1/3).

## 3D handoff — pending, assigned to Ömer by Jira

Use clean B1 as primary; B2/B3/B4 as additional form reference where supported, B5 for hands. B1/Jira measurements override any multiview generation drift. Deliver `Knight_Body_v1.fbx` plus `.fbm`: mesh `Knight_Body`, binary FBX, Y-up, floor-centered pivot, unrigged, no auto-rig/blendshapes, 4–6k triangles, PNG basecolor and normal ≤2048. Archive `TripoModels/knight_body/` with this prompt/reference provenance.

New master proportions normalize height to 1.0 and retain 1.59m in-game scale. Preserve current master/avatar/world scale until an explicit rig migration. Build new joint positions to the Jira table, add facial controls and three-segment fingers per rig brief, then run fit/pose QA. No rig, prefab, scene, armour, weapon-fit or animation-retarget changes were made here. These remain gated on the approved 3D body/master. Jira ticket is not closed; no FBX produced.
