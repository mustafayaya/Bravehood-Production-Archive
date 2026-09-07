# `Sword1H_WalkBack_v001` — production install of the approved Back01 (2026-09-04)

Installs the approved continuous true-Back retreat at 180° in `Light_Walk8` with its 1.00× presentation. Production
integration only; no artistic change.

### BACK v001 ASSET
`Assets/Bravehood/Animation/Generated/Sword1H_WalkBack_v001.anim` (guid a73fe51c…), a `CopyAsset` promotion of
`__freshHumanBack_01`. Contract: loopTime on, loopBlend off, orientation / Y / XZ baked, Based Upon Original,
heightFromFeet off, 0.82 s, canonical root, controller-driven translation. Research source untouched.

### SOURCE EQUIVALENCE
102 / 102 bindings, 0 missing; max |Δ value| 0, |Δ time| 0, |Δ tangent| 0; sampled feet within 0.0003 mm over 120
samples (float noise). Key-identical.

### CONTROLLER SLOT
Serialized ring before the edit: child 3 was `Sword1h_Strafe135RighttLoop @ 170.2° (0.1702, −0.9854) co 0.65`, the only
child within 20° of ±180. Replaced by `Sword1H_WalkBack_v001 @ (0, −1) ts 1 co 0.87`. Forward (0), ForwardLeft (6),
Left_v002 (5), BackLeft (4), the two right-side mocap children and the parked child 7 untouched (serialized listing
verified from disk; the mocap guid no longer appears in Light_Walk8).

### CYCLE OFFSET
Measured on the finished production clip with one contact threshold (12 mm, 400 samples) for both clips:

| clip | length | co | left lands (clip) | right lands (clip) | left (tree time) | right (tree time) |
|---|---|---|---|---|---|---|
| **Back_v001** | 0.82 s | **0.87** | 0.895 | 0.395 | **0.025** | **0.525** |
| BackLeft_v001 | 0.65 s | 0.35 | 0.375 | 0.875 | 0.025 | 0.525 |

0.87 makes both landings coincide with BackLeft's in tree time (Δ 0.000); the research estimate of 0.88 came from an
8 mm threshold and was not written.

### GUARDGAIT / ±180 WRAP
`GuardGait_Knight1H`: the legacy 170.2 : 1.80 entry replaced by **180 : 0.463 m/s** (native; the raw-clip stance-window
velocity is +0.4627 / +0.4625 m/s and the runtime planted feet move < 1 cm/s along travel at this rating). Table now
−135 0.592 / −90 0.496 / −45 1.24 / 0 1.34 / 86.3 1.81 / 125.3 1.84 / 180 0.463. Wrap: +180 = −180 = 0.4630;
+179 → 0.4882 and −179 → 0.4659 (one degree toward the right-rear mocap entry rated 1.84 and toward BackLeft 0.592
respectively — the asymmetry is the neighbour slope, the seam itself is continuous); +170 0.715, −170 0.492. The
1.00× presentation is not in the table.

### GAMEPLAY SPEED SCALE
`gameplayScale` at 180° = **0.405** (= 0.463 / (2.0 × 0.65 × 0.88)); +179 0.408, −179 0.406. Production math
2.0 × 0.65 × 0.88 × 0.405 = 0.463 m/s → 1.001×. No Animator speed hard-coded.

### RUNTIME PLAYBACK / CADENCE (real Player, production controller and GuardGait, no override, no speed pref)
| condition | load | world speed | MotionSpeed (min..max) | cadence | 0.25 / 0.5 / 1.0 s | stride error | planted drift |
|---|---|---|---|---|---|---|---|
| gameplay loadout | 0.880 | **0.463 m/s** | **1.001** (1.000..1.001) | **146 spm** | 0.12 / 0.23 / 0.46 m | +0.0 mm/s | 0–2 mm |
| unarmoured | 1.000 | 0.526 m/s | 1.137 | 166 spm | 0.13 / 0.26 / 0.53 m | +0.0 mm/s | 0–2 mm |

Travel relative to the root −180.0°, root yaw 354.29° constant. Player authority verified before the runs (prefab 2 / 4,
scene Player 2 with no override) and every session diag logged MaxStableMoveSpeed = 2.

### BODY FACING (pose stage C, relative to the root)
Moving: pelvis **−15.0°** (−19.6…−10.3), shoulders **−11.7°** (−12.8…−10.5), head on the target (UpperBodyAim), root
constant. Nothing rotates toward 180° travel.

### IDLE → BACK
`Back_v001_PRODUCTION_C_Idle_Back_Idle_start.mp4`. Idle pelvis −34.5° / shoulders −39.4° → moving −15.0° / −11.7°:
a change of **+19.5° / +27.7°** (Left_v002: 54° / 41°) — the smallest facing change in the family. Maximum yaw rate
through the crossfade: pelvis 442°/s (7.4° per 60 Hz frame) at 0.25 s into the blend, shoulders 294°/s; Back → Idle
83°/s and 125°/s; steady Back 36°/s and 9°/s. Frame strip across the entry (1.95–2.45 s) shows a continuous
reposition, no single-frame discontinuity. Noted: at the entry the right foot moves at up to 3.95 m/s (66 mm/frame)
for ~4 frames as it goes from the idle's rear-right stance to the retreat's forward-right lane (Left_v002's entry:
2.2 m/s). A stance reposition inside the 0.25 s crossfade, not a pop — flagged with the idle / locomotion body-facing
watch item for the family review.

### BACKLEFT → BACK
| input | world | MotionSpeed | BackLeft_v001 | Back_v001 | parked mocap |
|---|---|---|---|---|---|
| −135° | 0.535 | 0.904 | 1.000 | – | – |
| −145° | 0.519 | 0.922 | 0.773 | 0.219 | 0.007 |
| −155° | 0.503 | 0.941 | 0.541 | 0.430 | 0.029 |
| −165° | 0.487 | 0.963 | 0.329 | 0.654 | 0.017 |
| ±180° | 0.463 | 1.000 | 0.003 | 0.998 | – |

Monotonic; the parked child peaks at 3 %. Wrap test (`..._F_wrap_plus179_minus179.mp4`, +179 ↔ −179 twice): world
0.467 / 0.465 m/s, MotionSpeed 0.957 / 0.998, Back weight 0.982 / 0.978 (the other 2 % is the right-rear mocap child
at +179 and BackLeft at −179); at the seam crossings the per-frame velocity step is 2.5 mm/s and the MotionSpeed step
0.034 — no discontinuity in weights, speed, rating or playback.
BackLeft → Back → BackLeft (`..._D_...mp4`): boundary foot speeds 1.2–1.6 m/s (steady 2.7–3.5), knee rates
222–462°/s (steady 380), pelvis-height rate ≤ 1.25 (steady 1.28), sword ≤ 1.34 (steady 1.36), minimum foot gap
181 mm. Pelvis +31° → −15° through square, root constant.

### FOOT CONTACT (production runs, forensic stage C)
Knee minimum 25.7° / 25.2° (never straight), toe bone minimum 50 / 37 mm above the floor (no burial), mid-stance ankle
drift 0 mm mean / 2 mm max on both feet, feet in their own lanes with the root-frame gap never below 178 mm in any Back
segment, pelvis rhythm 38 mm continuous. Authored audit unchanged from research: knee 43.2° / 45.6°, flat 61 / 63 %,
planted yaw drift 7.8 / 9.9° (watch item, not corrected).

### VIDEOS (`Artifacts/AnimationReview/`)
A `Back_v001_PRODUCTION_A_lockon_back.mp4` · B `Back_v001_PRODUCTION_B_unarmoured.mp4` ·
C `Back_v001_PRODUCTION_C_Idle_Back_Idle_start.mp4` · D `Back_v001_PRODUCTION_D_BackLeft_Back_BackLeft.mp4` ·
E `Back_v001_PRODUCTION_E_Forward_ForwardLeft_Left_BackLeft_Back.mp4` (one run, 2 s per direction at the approved
presentations) · F `Back_v001_PRODUCTION_F_wrap_plus179_minus179.mp4`.

### TESTS
`AA1_ProductionBack_v001_Contract` (asset, (0,−1) slot, baked contract, key-identical to Back01, GuardGait 180 = 0.463,
scale 0.405, no 170.2 entry, no competing child within 40° of ±180, the other four production slots unchanged) and
`AA2_GuardGait_Wraps_Through_Back_At_PlusMinus180` (±180 equal, ±179 through Back within the neighbour slope, no seam
discontinuity, monotonic approach from both sides). Suite 50 / 50.

### PRODUCTION STATE
Light_Walk8: Forward v012 (0°), ForwardLeft v001 (−45°), Left v002 (−90°), BackLeft v001 (−135°), **Back v001 (180°,
co 0.87)**; right side still mocap 86.3° / 125.3° + parked child. Travel_Walk8 preserved exactly. GuardGait as above.
FootIK, grip, Player 2 / 4 untouched. No research clip referenced. VerifyProduction TRUE. Editor out of Play before
every write; controller, GuardGait and prefab read back from disk after. Nothing committed.
