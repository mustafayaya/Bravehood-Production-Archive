# `Sword1H_WalkForwardRight_v001` — production install of the approved ForwardRight01 (2026-09-06/07)

Installs the approved +45° advancing combat walk in the formerly parked child of `Light_Walk8`, completing the
eight-direction Light / lock-on family. Production integration only; no artistic change.

### FORWARDRIGHT v001 ASSET
`Assets/Bravehood/Animation/Generated/Sword1H_WalkForwardRight_v001.anim` (guid 46854fcc…), a `CopyAsset`
promotion of `__freshHumanForwardRight_01`. Contract: loopTime on, loopBlend off, orientation / Y / XZ baked,
Based Upon Original, heightFromFeet off, 0.80 s, canonical root, controller-driven translation. Research source
untouched.

### SOURCE EQUIVALENCE
102 / 102 bindings, 0 missing; max |Δ value| 0, |Δ time| 0, |Δ tangent| 0; sampled feet / head / root within
0.0004 mm over 120 samples. Key-identical.

### FINAL +45 SLOT
Serialized ring before the edit (read from disk): 0 Forward_v012 (0, 1) co 0.92 · 1 Right_v001 (1, 0) co 0.87 ·
2 BackRight_v001 · 3 Back_v001 · 4 BackLeft_v001 · 5 Left_v002 · 6 ForwardLeft_v001 · **7 `Sword1h_Strafe45LeftLoop`
parked at (0, 0) co 0.98** — the only origin child, no other child within 30° of +45. Child 7 replaced in place by
`Sword1H_WalkForwardRight_v001 @ (0.7071068, 0.7071068) ts 1 co 0.94`; no child added, none of the seven moved.
On disk after the write: eight authored children, one per 45° heading, no origin placeholder, no legacy mocap
motion, no research guid. The `Sword1h_Strafe45LeftLoop` source asset is no longer referenced by the Light ring and
was not deleted (`AnimationAssetSafety`).

### CYCLE OFFSET
Measured on the finished production asset with the install convention (foot bone lowest, 12 mm threshold, 400
samples): ForwardRight lands L 0.023 / R 0.505; Forward v012 L 0.008 / R 0.543 (tree 0.088 / 0.623 at co 0.92,
matching the runtime landings 0.08–0.12 / 0.63); Right v001 L 0.900 / R 0.400 (tree 0.030 / 0.530 at 0.87).
Offsets 0.92…0.97 all give the same summed deviation from the two neighbours (0.151 tree phase); **co 0.94** has the
smallest worst-case deviation (0.058: L 0.083 / R 0.565 → Δ Forward −0.005 / −0.058, Δ Right +0.053 / +0.035) and is
below the research prediction of 0.95. Written as 0.94.

### GUARDGAIT
**+45 : 1.208 m/s** native added (runtime 1.210 at MotionSpeed 1.000 on the research slot; the 0.816× presentation is
not in the table). Table now −135 0.592 / −90 0.496 / −45 1.24 / 0 1.34 / **45 1.208** / 90 0.496 / 135 0.463 /
180 0.463 — all eight canonical headings authored. Interpolation 22.5° → 1.274, 67.5° → 0.852; wrap 179 / −179 →
0.463 / 0.466, −157.5 → 0.528 (unchanged).

### GAMEPLAY SPEED SCALE
`gameplayScale` at +45° = **0.862** (= −45's). Production math 2.0 × 0.88 × 0.65 × 0.862 = 0.986 m/s → 0.986 / 1.208
= 0.816×. ForwardLeft and ForwardRight share one presentation by data; their mechanics stay asymmetric (below).
No Animator speed hard-coded; MotionSpeed remains the only time scaler.

### RUNTIME PLAYBACK / CADENCE (real Player, production controller and GuardGait, no override, no controller pref)
| condition | load | world speed | MotionSpeed | cadence | 0.25 / 0.5 / 1.0 s | stride error | planted drift L / R |
|---|---|---|---|---|---|---|---|
| gameplay loadout | 0.880 | **0.986 m/s** | **0.816** | **122 spm** | 0.25 / 0.49 / 0.99 m | −0.1 mm/s | 4 / 3 mm |
| unarmoured | 1.000 | 1.121 m/s | 0.928 | 139 spm | 0.28 / 0.56 / 1.12 m | −0.1 mm/s | 5 / 3 mm |
| ForwardLeft v001 pair (same batch) | 0.880 | 0.986 m/s | 0.795 | 119 spm | | −0.8 mm/s | 3 / 3 mm |

Travel relative to the root +45.0°, root yaw 354.29° constant, the production clip at weight 1.000. Player
authority verified before every session (prefab 2 / 4, scene Player 2 with no override, Militia loadout 0.880, one
Player in the loaded scene, no research controller); every diag logged MaxStableMoveSpeed = 2, note empty.

### BODY FACING
Moving: pelvis **−19.3°** (−29.9…−8.9), shoulders **−13.8°** (−16.7…−10.7) relative to the root, head on the target,
root constant — ForwardRight01 preserved. In the family contexts: Forward −3.5 / −7.3 → ForwardRight −20.0 / −13.6 →
Right −14.8 / −11.7. Not squared, not rotated toward travel.

### RIGHT FOOT / RIGHT LEG
Right boot support heading +10…+13° authored, support-yaw drift 0.5° authored (`rightReachExtraMm 8`,
`reachMarginMm 20`, `stanceYawStabilize 1`, `yawReleaseSpan 0.16` all inside the promoted curves). Runtime on the
true slot: planted drift 3 mm mean / 6 max, support-yaw drift 0.3° at native and 4.7–4.9° at the 0.816× presentation
(the slower playback lets the planted-window detector include more of the forefoot pivot; the mid-stance drift
is unchanged), knee minimum 21.5° / 17.8°, toe bone minimum 34 mm. No inward twist, no toe-out, no planted pivot.

### FOOT CONTACT
`LocomotionFootContactAudit` on the production asset (identical to research): knee 21.5° / 29.6°, reach 0.982 /
0.967, heel / toe within ±4 mm of flat, support 63 / 59 %, flat 37 / 44 %, buried 0 %, support-yaw drift 3.7° / 0.5°.
Runtime toe bone minimum 34–39 mm (loadout / unarmoured), minimum boot separation 165 mm; FootIK settles the
planted foot by terrain-scale amounts only.

### 0 → 45 → 90 BLEND (production inputs, lock-on, loadout)
| input | world | MotionSpeed | cadence | Forward | ForwardRight | Right | planted frames L / R | pelvis rhythm |
|---|---|---|---|---|---|---|---|---|
| 0° | 1.144 | 0.854 | 128 | 1.000 | – | – | 36 / 31 % | 70 mm |
| 10° | 1.109 | 0.846 | 127 | 0.779 | 0.221 | – | 31 / 24 % | 60 mm |
| 20° | 1.074 | 0.838 | 126 | 0.557 | 0.443 | – | 31 / 15 % | 54 mm |
| 30° | 1.039 | 0.830 | 124 | 0.333 | 0.667 | – | 39 / 23 % | 56 mm |
| 40° | 1.004 | 0.821 | 123 | 0.111 | 0.889 | – | 38 / 43 % | 54 mm |
| 45° | 0.986 | 0.816 | 122 | – | 1.000 | – | 38 / 45 % | 56 mm |
| 55° | 0.888 | 0.846 | 127 | – | 0.779 | 0.221 | 12 / 30 % | 46 mm |
| 65° | 0.791 | 0.886 | 133 | – | 0.557 | 0.443 | 5 / 18 % | 40 mm |
| 75° | 0.693 | 0.944 | 142 | – | 0.334 | 0.666 | 16 / 19 % | 35 mm |
| 85° | 0.595 | 1.033 | 155 | – | 0.112 | 0.888 | 27 / 58 % | 35 mm |
| 90° | 0.546 | 1.100 | 165 | – | – | 1.000 | 68 / 49 % | 37 mm |

Weight handover is monotonic in both sectors with no third child anywhere; the former parked child is the real
+45 motion. The 45–90 sector keeps fewer planted frames mid-sector (5–19 %) than 0–45 (15–43 %): its natives differ
by 2.2× (1.208 vs 0.496) so the blended stride cannot match the interpolated world speed — the bounded tree-space
limitation research identified. Pure canonical headings plant correctly; recorded for the FAMILY-WIDE cleanup, not
a blocker (the +45 → +90 context video shows no pop: boundaries ≤ 2.14 m/s foot, separation ≥ 185 mm).

### IDLE → FORWARDRIGHT
`..._C_Idle_ForwardRight_Idle.mp4` (the pure run). Idle pelvis −34.5° / shoulders −39.4° → moving −19.3° / −13.8°:
change **+15.2° / +25.6°** (research 15 / 26). Peak yaw rates through the crossfade: pelvis 180°/s (3.0° per frame),
shoulders 194°/s; ForwardRight → Idle 128 / 113°/s; steady 65 / 22°/s. Foot reposition at the entry 3.51 m/s inside
the 0.25 s crossfade (research 3.71, Back 3.95, Right 2.37); no single-frame discontinuity. CombatIdle untouched.

### FORWARDLEFT vs FORWARDRIGHT
`..._G_ForwardLeft_vs_ForwardRight.mp4` (both production, both 0.986 m/s, same Player, camera, loadout, lock-on,
writers, 60 fps). ForwardLeft opens the pelvis +26.6° toward its travel (shoulders +4.4), ForwardRight blades
−19.3° toward the guard (shoulders −13.8); cadence 119 vs 122 spm; playback 0.795× vs 0.816×. Same family, no mirror.
Watch item preserved for the post-8-direction review: ForwardLeft v001 in the same harness reads support knees
4.2° / 1.9°, planted yaw 6–20°, toe bone 11–18 mm; ForwardRight 21.5° / 17.8°, 0.3–4.9°, 34 mm. Not touched.

### FINAL 8-DIRECTION SWEEP
`..._H_Family8_sweep.mp4`: Forward → ForwardRight → Right → BackRight → Back → BackLeft → Left → ForwardLeft → Forward,
2 s each at the approved gameplay presentations, one production child at weight ≥ 0.992 in every segment:

| heading | clip | world | MotionSpeed | cadence | pelvis / shoulders | planted L / R |
|---|---|---|---|---|---|---|
| 0 | Forward_v012 | 1.144 | 0.854 | 128 | −1.4 / −6.3 | 35 / 27 % |
| +45 | **ForwardRight_v001** | **0.986** | **0.816** | **122** | **−21.0 / −13.6** | 35 / 49 % |
| +90 | Right_v001 | 0.546 | 1.090 | 164 | −15.5 / −11.5 | 54 / 58 % |
| +135 | BackRight_v001 | 0.463 | 1.000 | 146 | −21.7 / −13.9 | 55 / 58 % |
| 180 | Back_v001 | 0.463 | 1.000 | 146 | −14.3 / −11.8 | 53 / 61 % |
| −135 | BackLeft_v001 | 0.535 | 0.904 | 167 | +29.9 / +5.3 | 58 / 66 % |
| −90 | Left_v002 | 0.546 | 1.100 | 165 | +20.2 / +1.2 | 66 / 50 % |
| −45 | ForwardLeft_v001 | 0.986 | 0.795 | 119 | +24.5 / +4.0 | 38 / 35 % |
| 0 | Forward_v012 | 1.144 | 0.854 | 128 | −1.4 / −6.3 | 35 / 27 % |

Boundary foot speeds ≤ 2.56 m/s (steady 2.98), hips ≤ 1.26, sword ≤ 1.27 m/s, minimum boot separation 162 mm,
root constant through all eight.

### CONTEXTS (D / E / F)
Forward → FR → Forward: boundaries ≤ 2.56 m/s foot, ≤ 1.31 hips, ≤ 1.29 sword, separation ≥ 167 mm; pelvis −3.5 →
−20.0 → −3.5. Right → FR → Right: ≤ 2.14 / 0.91 / 0.91, separation ≥ 185 mm; −14.8 → −20.4 → −14.8. Forward → FR →
Right: ≤ 2.57 / 1.31 / 1.25, separation ≥ 168 mm; −3.2 → −19.9 → −15.1, shoulders −6.9 → −13.6 → −11.7. Stride
matched at every segment (MotionSpeed = world / native), cadence 128 → 122 → 164.

### GENERATOR INERTNESS (pre-install check)
Unstaged generator diff touches only `Options` (the new dials), `AltSwayRoot` / `AltSinkMm` (earlier lateral
work) and `StabilizeStanceYaw`, which runs only when `stanceYawStabilize > 0` — no production recipe other than
ForwardRight sets it. Rebuilt from their recorded recipes: BackLeft, Back, BackRight, Right and ForwardRight
key-identical to production (max |Δ| 0, 0 NaN); Left v002 max |Δ| 0.0004 and ForwardLeft 1.05 against reconstructed
recipes (the Left03 / FL02 build code was not recovered verbatim); Forward v012 predates this generator. Suite
regression tests (V1–V3, W2, O2, X1…AC1) pass.

### VIDEOS (`Artifacts/AnimationReview/`)
A `ForwardRight_v001_PRODUCTION_A_lockon_forwardright.mp4` · B `..._B_unarmoured.mp4` · C `..._C_Idle_ForwardRight_Idle.mp4` ·
D `..._D_Forward_ForwardRight_Forward.mp4` · E `..._E_Right_ForwardRight_Right.mp4` · F `..._F_Forward_ForwardRight_Right.mp4` ·
G `..._G_ForwardLeft_vs_ForwardRight.mp4` (+ `ForwardLeft_v001_PRODUCTION_pair_for_ForwardRight_v001.mp4`) ·
H `..._H_Family8_sweep.mp4`.

### TESTS
`AD1_ProductionForwardRight_v001_Contract` (asset, (0.7071, 0.7071) slot, co 0.94, baked contract, key-identical to
ForwardRight01, GuardGait +45 authored at 1.208 with scale 0.862 = −45's, no research / legacy / origin child, the
other seven slots unchanged) and `AD2_LightWalk8_AllEightCanonicalHeadings` (exactly eight active children on the
unit ring, exactly one at each canonical heading, GuardGait authored at all eight). Suite **54 / 54**.

### FINAL LIGHT_WALK8 STATE
0° Forward v012 (co 0.92) · +45° **ForwardRight v001 (co 0.94)** · +90° Right v001 (0.87) · +135° BackRight v001
(0.87) · 180° Back v001 (0.87) · −135° BackLeft v001 (0.35) · −90° Left v002 (0.34) · −45° ForwardLeft v001 (0.92).
No legacy active motion, no origin placeholder. GuardGait authored at all eight headings. Travel_Walk8 preserved
exactly (read from disk).

### PRODUCTION SAFETY
Forward / ForwardLeft / Left / BackLeft / Back / BackRight / Right, Travel family, FootIK, grip and Player 2 / 4 untouched
(prefab read back: MaxStable 2, Sprint 4, TravelWalkSpeedScale 0.65, GuardGait_Knight1H). No research clip referenced.
VerifyProduction TRUE before and after. Editor out of Play before every write; controller, GuardGait and prefab read
from disk after. Git index untouched; nothing added or committed.
