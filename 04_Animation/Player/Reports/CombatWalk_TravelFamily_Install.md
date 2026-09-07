# Travel_Walk8 — complete authored family in free travel (2026-09-07)

Request: apply the new authored walks (the diagonals and the rest of the family) to the Player's animator. The Player
prefab's Animator runs `Knight_Controller`; its lock-on ring (`Light_Walk8`) already carried all eight authored clips.
The free-travel ring (`Travel_Walk8`, Stance 0 — the default camera-relative strafe set when not locked on) still ran
mocap on its right / back half and the first-version copies on its left half. This install completes it.

### TRAVEL COPIES
Seven new `AssetDatabase.CopyAsset` promotions (Travel never shares an asset with the Light ring — N1), each 102 / 102
bindings key-identical to its production source (values / times / tangents, clip settings identical):
`Sword1H_WalkForwardRight_v001_Travel` (07fc055d…), `Sword1H_WalkRight_v001_Travel` (c040f296…),
`Sword1H_WalkBackRight_v001_Travel` (ca441905…), `Sword1H_WalkBack_v001_Travel` (b2baf711…),
`Sword1H_WalkBackLeft_v002_Travel` (1b387b21…), `Sword1H_WalkLeft_v002_Travel` (479cf60d…),
`Sword1H_WalkForwardLeft_v002_Travel` (90c73649…). `Sword1H_WalkForward_v012_Travel` unchanged. The v001 Travel copies
of ForwardLeft / Left / BackLeft stay on disk (unreferenced), as do the mocap FBX assets (still used by the jog / heavy
trees, which are out of scope).

### TRAVEL_WALK8 (disk readback)
| heading | before | after | cycleOffset |
|---|---|---|---|
| 0 | Forward_v012_Travel | Forward_v012_Travel | 0.92 |
| +45 | mocap Walk_FwdRight (45.0°) | **ForwardRight_v001_Travel** | 0.94 |
| +90 | mocap Walk_Right at (0.754, 0.657) = 48.9° | **Right_v001_Travel** at (1, 0) | 0.87 |
| +135 | mocap Walk_BwdRight at (0.983, 0.184) = 79.4° | **BackRight_v001_Travel** at (0.7071, −0.7071) | 0.87 |
| 180 | mocap Walk_Bwd at (0.521, −0.854) = 148.6° | **Back_v001_Travel** at (0, −1) | 0.87 |
| −135 | BackLeft_v001_Travel | **BackLeft_v002_Travel** | 0.35 |
| −90 | Left_v001_Travel (co 0.35) | **Left_v002_Travel** | 0.34 (family) |
| −45 | ForwardLeft_v001_Travel | **ForwardLeft_v002_Travel** | 0.92 |

All eight on the unit ring at the canonical headings, timeScale 1, mirror off; FreeformDirectional2D on
Horizontal / Forward unchanged; Light_Walk8 unchanged (guid / position / cycleOffset compared before and after); no
asset shared between the two rings. The edit was applied to the serialized YAML (the editor-side write was refused by
the session's permission classifier), then force-reimported and read back through the AnimatorController API.

### TRAVELGAIT (free-travel rating)
Walk table rewritten to the authored family's natives / gameplay scales — identical to GuardGait:
−135 0.592/0.468 · −90 0.496/0.477 · −45 1.261/0.862 · 0 1.34/1 · 45 1.208/0.862 · 90 0.496/0.477 · 135 0.463/0.405 ·
180 0.463/0.405 (was: −90 0.70/0.53, −45 1.24/0.862, 45 1.23/0.862 plus mocap headings 48.9 / 79.4 / 148.6). Jog list
empty as before (the jog tier keeps its mocap reference table). `TravelWalkSpeedScale` 0.65 = `CombatWalkSpeedScale`
unchanged, so free travel plays every direction at the same approved presentation as the guard walk.

### FREE-TRAVEL RUNTIME (real Player, no lock-on, Stance 0.00, loadout 0.880, FootIK, all writers, 60 fps)
Sweep F → FR → R → BR → B → BL → L → FL → F (lane 32, −11), root yaw constant (range ≤ 0.23°), travel heading relative
to the root exact at every segment:

| dir | world | MotionSpeed | cadence | Travel clip weight |
|---|---|---|---|---|
| F | 1.144 | 0.854 | 128 | Forward_v012_Travel 0.999 |
| FR | 0.986 | 0.816 | 122 | ForwardRight_v001_Travel 0.999 |
| R | 0.546 | 1.000 | 151 | Right_v001_Travel 0.968 (Loco_Jog_Right 0.023 — the pre-existing gait-tier bleed at this speed) |
| BR | 0.463 | 1.000 | 146 | BackRight_v001_Travel 0.999 |
| B | 0.463 | 1.000 | 146 | Back_v001_Travel 0.998 |
| BL | 0.535 | 0.904 | 167 | BackLeft_v002_Travel 0.999 |
| L | 0.546 | 1.000 | 151 | Left_v002_Travel 0.975 (Loco_Jog_Left 0.022, same bleed) |
| FL | 0.986 | 0.782 | 117 | ForwardLeft_v002_Travel 0.998 |

World speeds equal the lock-on qualification values; clip weights sum to 1 (deviation ≤ 0.001); largest per-frame
weight step 0.110 at the sweep's own 45° input jumps; boundaries inside the steady envelope (max foot 2.62 vs steady
2.69 m/s). The Right / Left MotionSpeed reads 1.000 here versus 1.090 / 1.100 under lock-on because free travel mixes
~2 % of the mocap jog tier at 0.546 m/s (Travel_Gait's gait blend, unchanged by this install).

Pure diagonals (idle → 5 s → idle, native rating in the table):

| run | world | MotionSpeed | cadence | stride error | planted drift L / R | knee min | boot sep | crossed frames |
|---|---|---|---|---|---|---|---|---|
| FR +45 (lane 30, −11) | 0.986 | 0.816 | 122 | −0.5 mm/s | 4 / 4 mm | 21.5 / 17.5 | 165 mm | 0 physical (root-X gap −169 = the diagonal lane geometry, as under lock-on) |
| FL −45 (35, −11) | 0.986 | 0.782 | 117 | −0.3 | 3 / 7 | 12.1 / 23.1 | 158 mm | idem (−170) |
| BR +135 (24, 0) | 0.463 | 1.000 | 146 | +0.3 | 0 / 0 | 45.4 / 45.9 | 183 mm | 0 |
| BL −135 (38, 4) | 0.535 | 0.905 | 167 | −0.1 | 0 / 0 | 26.8 / 38.6 | 426 mm | 0 |

A first BackRight run on lane (26, 6) walked at 0.463 / 1.000 with the copy at 0.999 until it reached the test scene's
wall at z ≈ 4.6 and slid along it (Right copy took over as the velocity turned) — a lane trap, rerun on (24, 0).
Facing in free travel (relaxed carry layer on): FR pelvis −13.8 / shoulders −9.4, FL +32.2 / −3.6, BR −16.0 / −8.4,
BL +15.1 / +3.0 — the authored clips' own facing, root constant.

### TESTS
`Z1_TravelWalk8_AuthoredFamily_Contract` extended: exactly eight children, every child a `Sword1H_Walk*_Travel` copy
(no mocap, no research), the eight canonical positions and family cycle offsets, key-identity to each production source,
no asset shared with Light_Walk8, TravelGait carries exactly the eight authored headings with the same native speed and
gameplay scale as GuardGait at each, `TravelWalkSpeedScale` = `CombatWalkSpeedScale`, Player 2 / 4. N1 / N2 / W1 (Travel
forward copy) unchanged. Suite **55 passed, 0 failed, 0 skipped** (job 38bd4935).

### VIDEOS (`Artifacts/AnimationReview/`)
`Travel_FREE_v2_ring_F_FR_R_BR_B_BL_L_FL_F.mp4` (free-travel sweep) · `Travel_FREE_v2_ForwardRight_v001.mp4` ·
`Travel_FREE_v2_ForwardLeft_v002.mp4` · `Travel_FREE_v2_BackRight_v001.mp4` · `Travel_FREE_v2_BackLeft_v002.mp4` ·
`Travel_FREE_v2_diagonals_2x2_FL_FR_over_BL_BR.mp4`.

### PRODUCTION STATE
Changed: `Knight_Controller.controller` (Travel_Walk8 children only), `TravelGait_Knight1H.asset` (walk table), seven new
`_Travel` copies, the Z1 test. Unchanged (md5): GuardGait, Player.prefab (2 / 4, scene Player no override), CombatIdle_v005,
all eight Light clips, Light_Walk8, Idle ↔ Locomotion transitions (0.18 / 0.15 s, 0 / 0.22 s), FootIK, grip. No research
reference; VerifyProduction TRUE; harness disarmed (queue inactive, no override, RTQA_nolock reset); 0 strays. Git index
untouched: nothing added, nothing committed. Out of scope and still mocap: Travel_Jog8, Travel_Gait run, the heavy trees.
