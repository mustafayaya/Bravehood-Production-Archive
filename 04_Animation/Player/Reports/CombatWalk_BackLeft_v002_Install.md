# Sword1H_WalkBackLeft_v002 — body-facing production revision, install and qualification (2026-09-07)

Human review: BackLeft facing candidate B (`__freshHumanBackLeft_Facing01`) APPROVED. Body-facing revision only: the
BackLeft step architecture, speed and lower-body mechanics stay as approved for v001. CombatIdle_v005, the other seven
Light clips, Travel, GuardGait, gameplay scales, FootIK, grip and Player untouched (md5 / disk readback below).

### BACKLEFT v002 ASSET
`Generated/Sword1H_WalkBackLeft_v002.anim` (guid 59e0c184527b64bd7b2f5500a289a7c9), `AssetDatabase.CopyAsset` of
`__freshHumanBackLeft_Facing01` (guid ee9798a6…, unchanged). Length 0.65 s at 60 fps; loopTime ON, loopBlend OFF,
orientation / Y / XZ baked, Based Upon Original, heightFromFeet OFF, no mirror: the canonical baked in-place contract.
`Sword1H_WalkBackLeft_v001` (guid e56dca38…) remains on disk, md5 unchanged.

### SOURCE EQUIVALENCE
102 / 102 bindings, 0 missing, 0 key-count mismatches, max |Δvalue| 0, max |Δtime| 0, max |Δtangent| 0; clip settings
identical (loop / bake / start / stop / cycleOffset); interpolation legality PASS (no muscle leaves [−1, 1] between keys).
Sampled-pose equivalence on the Player prefab: 120 samples × 22 Humanoid bones, max position 0.0005 mm, max rotation
0.0000°, root at the canonical origin.

### V001 → V002 CURVE DIFFERENCES
17 / 102 curves differ, 85 bit-identical. The 17: Spine / Chest / UpperChest Twist Left-Right (0.117 / 0.117 / 0.201 max
|Δ|), Neck Turn Left-Right (0.195), RootQ.x / y / z / w (the body-frame rotation riding the torso yaw; y 0.113, w 0.011,
x / z < 1e-4), and the leg re-solve to the same foot marks: Left / Right Upper Leg Front-Back (0.050 / 0.103), Upper Leg
In-Out (0.032 / 0.056), Lower Leg Stretch (0.010 / 0.022), Left / Right Foot Up-Down (0.074 / 0.060), RootT.y (< 1e-4).
Every arm, shoulder, hand and finger curve is bit-identical; Head curves are untouched (only the Neck turn carries the
head-on-threat correction).

### CONTROLLER SLOT
`Light_Walk8` read from disk before the edit: child 4 = BackLeft_v001 at (−0.7071068, −0.7071068), ts 1, co 0.35. Only
that child's motion was replaced (Undo-recorded, saved, force-reimported): child 4 = `Sword1H_WalkBackLeft_v002` at
(−0.7071068, −0.7071068), timeScale 1, cycleOffset 0.35, mirror off. Ring after the write (disk): Forward v012 (0,1)
co 0.92 · Right v001 (1,0) 0.87 · BackRight v001 0.87 · Back v001 0.87 · **BackLeft v002 0.35** · Left v002 0.34 ·
ForwardLeft v002 0.92 · ForwardRight v001 0.94; blend type FreeformDirectional2D, parameters Horizontal / Forward
unchanged; Travel_Walk8 eight children unchanged (guid / position / timeScale / cycleOffset compared before and after).
The controller's working-tree diff vs HEAD gains exactly one line: the child-4 motion guid e56dca38… → 59e0c184….

### CYCLE OFFSET
Measured on the production asset (root-local foot / toe heights, 200 samples, 12 mm lift band): v001 L lifts 0.020 /
lands 0.330, R lifts 0.520 / lands 0.825; **v002 identical to the sample**. Left_v002 for reference: L 0.025 / 0.400,
R 0.525 / 0.900. Phase, footfall timing and the shared step-close schedule are unchanged, so **cycleOffset stays 0.35**
(Left ↔ BackLeft still crossfade on the same support phase). Runtime landings sit at the same tree times as v001.

### GUARDGAIT / GAMEPLAY SPEED
Untouched: −135 : 0.592 native / 0.468 gameplayScale (full table −135 0.592/0.468, −90 0.496/0.477, −45 1.261/0.862,
0 1.34/1, 45 1.208/0.862, 90 0.496/0.477, 135 0.463/0.405, 180 0.463/0.405). Authored stance-track speed −0.417 /
−0.418 m/s (v001 −0.418 / −0.418, same measure). Production runtime (real Player, production controller, lock-on,
Militia loadout 0.880, FootIK AnimatedBones, all writers, 60 fps, MaxStableMoveSpeed 2):

| | world speed | rel-root heading | MotionSpeed | cadence | cycle | stride error |
|---|---|---|---|---|---|---|
| BackLeft v002 −135° | **0.535 m/s** | −135.0° | **0.904** (0.904..0.904) | **167 spm** | 0.719 s | −0.0 mm/s vs native 0.592 |

Identical to the v001 qualification (0.535 / 0.904 / 167). No speed change; no STOP condition.

### BODY FACING
Runtime pelvis **+9.4°** (+3.2..+15.7), shoulders **−2.6°** (−4.4..−0.7) relative to the root (v001 +30.6 / +5.3); head on
the lock target (−10.2° vs the harness reference, the same value as every family clip); root yaw 354.29°, range 0.000°
in every run. Candidate B preserved exactly (research measured +9.4 / −2.6). Right boot heading −30..−23° (mean −26).
In the sweep: BackLeft +9.2 / −2.4 between Back −15.2 / −11.9 and Left +20.0 / +1.0.

### LOWER-BODY PRESERVATION
Runtime: planted mid-stance drift 0 mm mean / 0 max on both feet (4 stances each), support-yaw drift 20.7 / 7.1°
(v001 17.1 / 5.0 at install; the family watch item, unchanged in class), knee minimum 26.8 / 38.7°, toe bone minimum
59 / 51 mm (flat 61 / 53), minimum boot separation 426 mm, root-frame right-minus-left foot gap ≥ +424 mm, 0 crossed
frames (pure), ≥ +208 mm / 0 crossed (Back ↔ BackLeft), ≥ +116 mm / 0 crossed (BackLeft ↔ Left, the Left segments'
own gap). Authored audit: knee 26.7 / 40.2 (v001 24.0 / 38.2), toe −2 / 0 mm (same), buried 0 %, support-yaw drift
21.9 / 5.0 (20.3 / 3.6), stance-track speed unchanged. No leg correction pass was run.

### SWORD / SHOULDER
Arm and hand curves bit-identical to v001; the sword follows the smaller torso yaw only. Runtime sword speed through
the Idle → BackLeft entry **0.72 m/s** (v001 0.99), exit 0.77 (v001 ~1.2 before the offset install); Back → BackLeft
boundary 0.58 (v001 1.35), BackLeft → Back 1.31 (1.34); BackLeft → Left 1.05 / 0.83. Hand-in-chest displacement 0.0 mm
and sword pivot 459 mm from the chest, from the key-identical research measurement.

### IDLE → BACKLEFT (production transition, offset 0.18 / 0.15 s)
CombatIdle_v005 → BackLeft_v002 → CombatIdle_v005 on the production controller:

| | Δpelvis | Δshoulders | peak pelvis | peak shoulders | feet L / R | max step | knee | sword |
|---|---|---|---|---|---|---|---|---|
| v001 (study baseline) | +65.6° | +44.8° | 462°/s (7.7°/fr) | 298°/s | 2.71 / 2.83 | 55 mm | 474°/s | 0.99 |
| **v002** | **+44.4°** (−28.8 → +15.7) | **+37.0°** | **337°/s (5.6°/fr)** | **255°/s** | 2.40 / 3.21 | 55 mm | 458°/s | 0.72 |

Exit (BackLeft → Idle, 0.22 s): pelvis 211°/s (v001 306), shoulders 176°/s, feet 1.53 / 2.01, max step 33 mm, root
Δ 0.00. Stance map through the entry: idle L(−382,+150) R(+249,−326) → steady L(−356,−122) R(+340,+106), width 631 →
696, the lead-foot swap of the step-close architecture unchanged. The remaining +44° is the Idle's own blade; CombatIdle
untouched.

### BACK → BACKLEFT
Back_v001 → BackLeft_v002 → Back_v001 (lane 38, 6): pelvis −8.9 → +15.3 (**Δ +24**) at **136°/s** (2.3°/frame),
shoulders 50°/s (v001: Δ 44 at 261°/s, shoulders 98); return Δ −25 at 58°/s (v001 138). Boundary feet 1.54 / 1.49 m/s,
hips 0.63 / 1.28, sword 0.58 / 1.31, minimum boot separation 208 / 254 mm, planted drift ≤ 1 mm mean (max 3 / 4), knee
min 26.7 / 38.6, toe 45 / 41 mm, 0 crossed frames. Segment speeds Back 0.463 (MS 1.000, 146 spm) / BackLeft 0.535
(0.904, 167) / Back 0.463 — unchanged from their qualifications. The family's largest moving seam roughly halves.

### BACKLEFT → LEFT
BackLeft_v002 → Left_v002 → BackLeft_v002 (lane 40, 4): pelvis +14.8 → +25.2 (**Δ +10**) at **55°/s** (0.9°/frame),
shoulders 24°/s (v001: Δ −11 at 104°/s); return Δ −10 at 80°/s (v001 117). Boundary feet 1.54 / 1.13, hips 1.06 / 0.86,
sword 1.05 / 0.83, separation 238 / 214 mm, 0 crossed frames. Left 0.546 (MS 1.100, 165 spm) unchanged. Same
magnitude as before, opposite sign, gentler: no new facing discontinuity toward Left.

### FAMILY SWEEP (F → FR → R → BR → B → BL_v002 → L → FL → F, one session, lane 32, −11)
| dir | world | MotionSpeed | cadence | pelvis / shoulders | clip weights |
|---|---|---|---|---|---|
| F | 1.144 | 0.854 | 128 | −0.6 / −6.0 | Forward_v012 1.000 |
| FR | 0.986 | 0.816 | 122 | −19.3 / −13.3 | ForwardRight_v001 0.999 |
| R | 0.546 | 1.090 | 164 | −14.5 / −11.4 | Right_v001 0.992 (FR 0.013) |
| BR | 0.463 | 1.000 | 146 | −20.7 / −13.9 | BackRight_v001 0.999 |
| B | 0.463 | 1.000 | 146 | −15.2 / −11.9 | Back_v001 1.000 |
| **BL** | **0.535** | **0.904** | **167** | **+9.2 / −2.4** | **BackLeft_v002 1.000** |
| L | 0.546 | 1.100 | 165 | +20.0 / +1.0 | Left_v002 1.000 |
| FL | 0.986 | 0.782 | 117 | +24.8 / +4.5 | ForwardLeft_v002 1.000 |
| F | 1.144 | 0.854 | 128 | −0.6 / −6.0 | Forward_v012 1.000 |

Root yaw 354.29° constant (range 0.000°); clip weights sum to 1 at every frame (deviation 0.000); no BlendTree topology
change (eight children, same positions); the largest per-frame weight step is 0.110 at the FL → F input step (the
sweep's own 45° input jump, not a BackLeft seam); max per-frame KCC speed step 0.188 m/s and MotionSpeed step 0.096 at
the input steps as before. Boundaries around BackLeft: B → BL feet 1.64 / hips 0.63 / sword 0.69 / separation 208 mm;
BL → L 1.99 / 1.29 / 1.31 / 217 mm — inside the sweep's steady envelope (steady max foot 2.68, Forward boundaries 2.55).
No neighbour modified; world speeds equal their qualification values.

### TESTS
`Y1_ProductionBackLeft_v001_Contract` → `Y1_ProductionBackLeft_v002_Contract`: true −135 slot references
`Sword1H_WalkBackLeft_v002`, v001 no longer referenced by Light_Walk8, key-identity (values / times / tangents) to
`__freshHumanBackLeft_Facing01`, baked in-place contract, 0.65 s, cycleOffset 0.35 (measured, shared with Left within
0.02), no active child within 30° of −135°, Forward / ForwardLeft / Left slots, GuardGait −135 ≈ 0.592 native and
0.468 gameplayScale, −90 / −45 / 0 unchanged, no −141.6 entry. The AA1 / AB1 / AC1 / AD1 ring dictionaries and the AE1
cycleOffset table now name BackLeft_v002 (co 0.35); AD2 all-eight and AE1 (offset 0.18 / 0.15, exit 0 / 0.22, eight
children) pass unchanged; Z1 Travel contract unchanged (Travel keeps its own v001_Travel copy). No pelvis-angle
thresholds encoded. Result: **55 passed, 0 failed, 0 skipped** (job e3d20eec). A first run before the script refresh
(auto-refresh off) executed the stale v001 contract and failed 6 — recompiled and rerun.

### VIDEOS (`Artifacts/AnimationReview/`)
- A `BackLeft_v002_PRODUCTION_A_pure135.mp4` — pure −135° at the production presentation (4 s of the C run)
- B `BackLeft_v002_PRODUCTION_B_v001_left_vs_v002_right.mp4` — v001 production run (study video A) beside v002, same lane / script
- C `BackLeft_v002_PRODUCTION_C_Idle_BackLeft_Idle.mp4` — Idle → BackLeft_v002 → Idle, production transition
- D `BackLeft_v002_PRODUCTION_D_Back_BackLeft_Back.mp4`
- E `BackLeft_v002_PRODUCTION_E_BackLeft_Left_BackLeft.mp4`
- F `BackLeft_v002_PRODUCTION_F_ring_F_FR_R_BR_B_BL_L_FL_F.mp4` — final 8-direction sweep
All from the real Player, production controller and assets, gameplay camera, lock-on, loadout 0.880.

### PRODUCTION STATE
Editor confirmed out of Play Mode before the asset copy and the controller write; controller, GuardGait and Player
prefab force-reimported and read back from disk afterwards. md5 of 14 production files before / after: only
`Knight_Controller.controller` changed; GuardGait, Player.prefab (2 / 4, scene Player no override), CombatIdle_v005,
Forward v012, ForwardRight / Right / BackRight / Back v001, BackLeft v001, Left v002, ForwardLeft v002 identical; FootIK
and grip untouched. Travel_Walk8 unchanged. No research clip referenced (VerifyProduction TRUE). Harness disarmed
(no override, no research controller, queue inactive), 0 stray Players / holders. Git index untouched: nothing added,
nothing committed (v002 asset untracked, controller and tests modified in the working tree).
The runtime harness's lock step logs a NullReferenceException (`PlayerTargeting.SetLocked`: the runtime-created
target's `Health.OnDeath` UnityEvent is null when `AddListener` runs) after the lock has already been set — present in
every earlier qualification run as well; the lock holds (locked=True, lockAttempts=0) and the runs are unaffected.
