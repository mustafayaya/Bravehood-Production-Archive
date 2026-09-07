# `__freshHumanCombatIdle_06` — CombatIdle v006 research challenger (2026-09-07)

Research only. Not installed. `Sword1H_CombatIdle_v005`, Light_Walk8, all eight locomotion clips, GuardGait, gameplay
scales, Travel, TravelGait, FootIK, grip and Player untouched (read back from disk; VerifyProduction TRUE). The
challenger reached the Player only through an in-memory name override on the production controller.

### IDLE06 DESIGN
A pose-level surgery on the approved v005: the same profile (`BravehoodCombatIdleProfile`, unchanged) regenerates the
idle from a modified base pose, `Sword1H_CombatIdle_Pose_v3A_research` = `ApprovedPose_v2` with the stance narrowed and
the blade eased, every upper-body muscle preserved. Precondition proven first: regenerating v005 from
`ApprovedPose_v2` + the profile reproduces it with 77 / 102 curves identical, legs and root within 0.0003 and only the
right-arm curves within 0.008 muscle (the weapon-hand solve's noise floor). The base pose was solved as ONE pose with the
foot-lock solver: body yaw to the pelvis target, both feet and both toes to root-frame marks (position, flat pitch,
heading), the body raised only as far as the straighter knee needed, the three torso twists to the shoulder target, neck
and head turn to keep the head on the threat, iterated to convergence (foot residual 0.3 mm).

Micro-study (one bounded ambiguity: stance geometry / blade combination), three candidates built the same way:

| | width / lead (mm) | pelvis / shoulders | knees | pelvis rise | validator |
|---|---|---|---|---|---|
| **A = Idle06** | **366 / 270** | **−18.0 / −14.8** | 35 / 29° | +36 mm | 94.8, no failing line |
| B (gentler) | 402 / 324 | −20.9 / −17.9 | 33 / 26° | +35 mm | 94.0 |
| C (stronger) | 336 / 240 | −15.3 / −12.0 | 37 / 30° | +36 mm | 90.9 |
| v005 | 631 / 475 | −28.8 / −33.6 | 24 / 23° | – | 93.1 |

A is the challenger (the hypothesis point, mid of the study); B and C are the reviewer's alternatives
(`__freshHumanCombatIdle_06B/C`, `Idle06_microstudy_v005_A_B_C.mp4`). No Idle07.

### V005 → V006 CURVE / POSE DIFFERENCES
60 / 102 curves bit-identical (every left-arm, hand, spine/chest pitch-roll, and all holding curves). Changed curves are
constant offsets with the v005 motion shape preserved: the 12 leg channels (offset up to 0.30 muscle, shape Δ ≤ 0.009),
Spine / Chest / UpperChest Twist −0.080 each (shape Δ 0), Neck Turn + Head Turn +0.275 each (shape Δ 0), RootT / RootQ
constant (the yaw and the 36 mm rise), and the six right-arm curves within 0.005 (solver noise, same class as the v005
reproduction). Length 3.2 s, 9 keys per animated curve, loop settings identical.

### STANCE GEOMETRY
Authored (canonical frame): L (−186, +27) / R (+179, −243) → width 366, left foot 270 ahead (v005 631 / 475); feet
centre 42 mm left and 27 mm behind the pelvis (a light rear bias, v005 98 / 87). Runtime with FootIK: width 366, lead
271. Left foot stays the forward foot; no parallel stance.

### BODY FACING
Authored hip line −18.0° (v005 −28.8), shoulders −14.8° (−33.6); runtime −18.0 / −15.6 (v005 −28.8 / −33.8). Head on
the target: runtime head-vs-lock-target +0.9° (v005 read −10.2°, i.e. the old idle's head sat 10° off the threat).
Still a right-handed blade, 3° more than Right v001's −15 / −12 and inside the sword-side walks' band.

### KNEES / WEIGHT
Knees 35° / 29° authored (34 / 27 at runtime), v005 24 / 23. The narrower base cannot keep both the old height and the
old flexion: holding the straighter (right, shorter) leg near 26° raised the pelvis 36 mm (708 → 741, 2.3 % of body
height) with the front knee settling at 35°. Pelvis rhythm 6 mm both. Feet centre 27 mm behind the pelvis: rear bias
kept, not exaggerated; no squat, no locked rear knee.

### FOOT ORIENTATION / CONTACT
Boot headings −0.8° (front, on the threat) / +28.1° (rear toe-out; v005 −0.6 / +32). Toes flat on the calibrated
reference (45 / 36 mm toe bones, ankles 57 / 56), no burial, no intersection (366 mm apart), reach 0.9x with both knees
soft. Base-pose health PASS on all three candidates.

### UPPER BODY / SWORD PRESERVATION
Hand-in-chest offset drift 0.0 mm, sword-in-chest 0.0 mm, sword pivot 463 mm from the upper chest (v005 463); the
sword's root-frame position moves only with the chest yaw (218, 573, −113 vs 194, 546, −90). Arm and off-hand curves
unchanged; right shoulder curve within 0.0035 of v005. No clavicle change.

### IDLE LOOP PRESERVATION
Same profile, duration, key layout and phases: breathing / weight-shift / awareness deltas identical in shape on every
animated curve (max shape Δ 0.009 on the leg channels, 0 on torso / neck). Sword path per loop 9 mm vs 10 (authored).

### V005 vs V006 ARTISTIC COMPARISON
`Idle06_A_v005_vs_06A.mp4` (gameplay camera, lock-on, loadout, all writers). Same guard, same head, same sword and
off-hand, same breathing; the base is narrower and the torso 11° less bladed. It reads as the same Bravehood combat
idle with a more mobile base — the review question stands for the human.

### ALL-8 ENTRY BEFORE / AFTER (Idle → direction, production 0.15 s crossfade; peak pelvis rate, feet, class)
| dir | v005 Δpelvis / rate | v005 feet L/R, step | class | **06A Δpelvis / rate** | **06A feet, step** | class |
|---|---|---|---|---|---|---|---|
| F | +33° / 300°/s | 3.6 / 3.6, 69 mm | B/C | **+22° / 229** | 3.5 / 3.6, 74 mm | B/C |
| FR | +15 / 181 | 4.1 / 3.1, 58 | B | **+4.5 / 110** | 3.4 / 3.1, 45 | B |
| R | +19 / 174 | 2.8 / 1.8, 40 | B | **+8 / 101** | **1.7 / 1.6, 27** | **A** |
| BR | +13 / 106 | 3.0 / 3.1, 61 | A | **+2 / 36** | 1.4 / 2.5, 48 | **A** |
| B | +19 / 431 | 2.9 / 3.2, 63 | B | **+8 / 362** | 1.6 / 2.5, 52 | B |
| BL | +65 / 499 | 3.0 / 3.2, 58 | C | +55 / 423 | 2.3 / 3.4, 64 | C |
| L | +53 / 387 | 1.9 / 2.2, 41 | C | +42 / 325 | 1.5 / 1.6, 31 | C |
| FL | +61 / 491 | 2.4 / 3.2, 46 | C | +50 / 418 | 2.7 / 3.1, 36 | C |

Exits (direction → Idle) improve everywhere: F 181 → 134°/s, R 127 → 79, BR 83 → 34, B 113 → 63, BL 280 → 232, FL
331 → 280, feet 0.5–2.6 m/s. Head stays within 1° of the target through every entry (v005 −10°). Shoulder rates halve on
the sword side (F 240 → 108, R 200 → 66, BR 172 → 46).

### FORWARD ENTRY RESULT
Body: solved (+22° at 229°/s, shoulders 108°/s, reads fine). Feet: **not solved** — 3.5 / 3.6 m/s, 74 mm/frame on the
right shin, knee 1238°/s, identical to v005. The stance map shows why: the width now narrows gently (366 → 315 → 249 →
198 → 178 through the blend) but the left foot still travels +173 mm forward and the right foot −62 mm in nine frames,
because Forward v012 enters at a phase where the feet are mid-stride (left foot +200 ahead, right leg swinging). The
Forward foot cost is the walk's entry phase, not the idle stance width; it will not respond to any idle stance. The
lever for it is the Idle → Locomotion transition offset (entering the tree at double support), a controller number —
not Forward's curves, which stay untouched.

### LEFT-GROUP RESIDUAL
BL +55° / 423°/s, FL +50 / 418, L +42 / 325 — reduced by the idle's 11° but still class C; the residual is the
opposite-sign opening of those three clips (+37 / +32 / +25) and, for BL, the lead-foot swap. Dominant residual BODY
for FL and L; BOTH for BL. Left-group cleanup remains required and is unchanged in scope.

### CROSSFADE 0.15 / 0.18 / 0.22 STUDY (Idle06 entries; research controller copies, exits unchanged)
| dir | metric | 0.15 s | 0.18 s | 0.22 s |
|---|---|---|---|---|
| F | pelvis / shoulders peak | 229 / 108°/s | 193 / 95 | 157 / 78 |
| F | max foot step · blended travel | 74 mm · 0.12 m | 69 · 0.16 | 73 · 0.21 |
| BL | pelvis / shoulders peak | 423 / 180 | 352 / 150 | 287 / 122 |
| BL | max foot step · foot L/R · travel | 64 mm · 2.3/3.4 · 0.06 | 51 · 2.0/2.6 · 0.08 | 40 · 2.0/2.0 · 0.11 |
| L | pelvis peak · step | 325 · 31 | 282 · 24 | 252 · 32 |
| FL | pelvis peak · step | 418 · 36 | 350 · 36 | 286 · 36 |

Visual: with Idle06 the sword-side entries are already A / B at 0.15 s; the left group stays class C at every duration
(the destination is the read). Responsiveness: 0.18 adds two frames and 40 mm of blended travel on Forward; 0.22 adds
five frames and 90 mm and starts to soften the first step.

### RECOMMENDED TRANSITION DURATION
**0.18 s** for Idle → Locomotion as the complement: −15…−20 % on every peak, −20 % on BL's foot step, two frames of
cost, no visible mush. 0.22 is not selected — its extra gain is on the left group only, which timing cannot fix. Not
written.

### REMAINING CLASS-C TRANSITIONS
BackLeft (BOTH: +55° opening and lead-foot swap), ForwardLeft (BODY), Left (BODY). Forward's foot reorganisation stays
B/C (phase, see above). Everything else A / B.

### NEXT ARCHITECTURAL STEP
1. Human review of Idle06 by itself (question: same Bravehood idle?). 2. If approved: productionize v006 with the
0.18 s entry. 3. Idle → Locomotion transition offset study for the Forward foot cost (enter the tree at double
support; a controller number, Forward untouched). 4. Left-group opening reduction (BL first, then L, then FL) — the
only remaining class-C cause.

### TECHNICAL QA
Base-pose health PASS (all candidates); pipeline validation 94.8 / 100, weapon-hand stability 76.7 (v005 93.1 / 77.3),
no failing line; loop by construction (same profile), feet locked (foot residual 0.3 mm, toes flat), root canonical
(0.00° / 0.00 mm), deterministic (the pipeline reproduces v005 within the documented solver noise; A rebuilt to
`__freshHumanCombatIdle_06` key-identical, 102 / 102, Δ 0). Runtime with FootIK: stance 366 / 271, knees 34 / 27,
toes 45 / 36, no sink, no pop; every diag MaxStableMoveSpeed 2, loadout 0.880, one Player in the loaded scene.

### VIDEOS (`Artifacts/AnimationReview/`)
A `Idle06_A_v005_vs_06A.mp4` · B `Idle06_B_pure_loop_06A.mp4` · C `Idle06_C_grid_v005_to_each8.mp4` ·
D `Idle06_D_grid_06A_to_each8_F_FR_R_BR_over_B_BL_L_FL.mp4` · E `Idle06_E_Forward_entry_v005_vs_06A.mp4` ·
F `Idle06_F_BackLeft_entry_v005_vs_06A.mp4` · G `Idle06_G_crossfade_0p15_0p18_0p22_F_BL_L_FL.mp4` ·
H `Idle06_H_best2_worst2_BR_R_BL_FL.mp4` · micro-study `Idle06_microstudy_v005_A_B_C.mp4`, `Idle06_cand_B_loop.mp4`,
`Idle06_cand_C_loop.mp4`, `Idle06_ref_v005_loop.mp4`.

### PRODUCTION SAFETY
Research assets only: `__freshHumanCombatIdle_06` (= A), `_06A/B/C`, pose snapshots
`Sword1H_CombatIdle_Pose_v3A/B/C_research`, research controllers `Knight_Controller_XfadeStudy_E18/E22` (entry duration
only). Production Idle state still `Sword1H_CombatIdle_v005`; transitions 0.15 / 0.22 s unchanged; v005, controller,
GuardGait, prefab hashes unchanged; no research reference; VerifyProduction TRUE. Editor out of Play before every asset
write. Git index untouched; nothing added or committed.
