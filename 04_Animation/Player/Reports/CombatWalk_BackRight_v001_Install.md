# `Sword1H_WalkBackRight_v001` — production install of the approved BackRight01 (2026-09-04)

Installs the approved continuous sword-side diagonal retreat at +135° in `Light_Walk8` with its 1.00× presentation.
Production integration only; no artistic change.

### BACKRIGHT v001 ASSET
`Assets/Bravehood/Animation/Generated/Sword1H_WalkBackRight_v001.anim` (guid 16028bb4…), a `CopyAsset` promotion of
`__freshHumanBackRight_01`. Contract: loopTime on, loopBlend off, orientation / Y / XZ baked, Based Upon Original,
heightFromFeet off, 0.82 s, canonical root, controller-driven translation. Research source untouched.

### SOURCE EQUIVALENCE
102 / 102 bindings, 0 missing; max |Δ value| 0, |Δ time| 0, |Δ tangent| 0; sampled feet within 0.0002 mm over 120
samples. Key-identical.

### CONTROLLER SLOT
Serialized ring before the edit: slot 2 was `Sword1h_StrafeRightLoop @ +125.3° (0.8161, −0.5779) co 0.78`, the only
child within 20° of +135. Replaced by `Sword1H_WalkBackRight_v001 @ (0.7071068, −0.7071068) ts 1 co 0.87`. Forward (0),
Left_v002 (5), ForwardLeft (6), BackLeft (4), Back (3), the 86.3° mocap child (1) and the parked child (7) untouched;
verified from the file on disk (mocap guid gone from the ring, no research guid).

### CYCLE OFFSET
Measured on the finished production clip (12 mm contact threshold, 400 samples): BackRight lands the right foot at
0.395 and the left at 0.895 of its cycle — the same phases as Back v001, whose co 0.87 puts them at tree time
0.525 / 0.025 (= BackLeft's). **cycleOffset 0.87** makes Back ↔ BackRight land on the same frames (Δ 0.000) and
carries the same landing frames toward the future Right slot.

### GUARDGAIT
`GuardGait_Knight1H`: the legacy 125.3 : 1.84 entry replaced by **135 : 0.463 m/s** (native; raw-clip stance-window
velocity 0.4627 / 0.4625, runtime planted feet within 2 mm per stance). Table now −135 0.592 / −90 0.496 / −45 1.24 /
0 1.34 / 86.3 1.81 (legacy) / **135 0.463** / 180 0.463 — six true family headings. The 1.00× presentation is not in the table.

### GAMEPLAY SPEED SCALE
`gameplayScale` at +135° = **0.405** (= 0.463 / (2.0 × 0.65 × 0.88)); production math → 0.463 m/s at 1.001×. Rear
hemisphere: Back 0.463 / 1.00×, BackRight 0.463 / 1.00× (intentional), BackLeft 0.535 / 0.904× (untouched).

### RUNTIME PLAYBACK / CADENCE (real Player, production controller and GuardGait, no override, no speed pref)
| condition | load | world speed | MotionSpeed | cadence | 0.25 / 0.5 / 1.0 s | stride error | planted drift |
|---|---|---|---|---|---|---|---|
| gameplay loadout | 0.880 | **0.463 m/s** | **1.001** | **146 spm** | 0.12 / 0.23 / 0.46 m | +0.0 mm/s | 0–2 mm |
| unarmoured | 1.000 | 0.526 m/s | 1.137 | 166 spm | 0.13 / 0.26 / 0.53 m | −0.0 mm/s | 0–2 mm |

Travel relative to the root +135.0°, root yaw 354.29° constant. Player authority verified before the runs (prefab
2 / 4, scene Player 2 with no override); every session diag logged MaxStableMoveSpeed = 2.

### BODY FACING
Moving: pelvis **−21.5°** (−26.1…−16.8) and shoulders **−14.1°** (−15.2…−12.9) relative to the root, head on the target,
root constant. The approved slight right blade; nothing turns toward +135°.

### SWORD-SIDE GUARD
Right Shoulder Down-Up 0.093…0.105 (identical to the research authority and to Back / Left v002; no compression), sword
pivot never closer than 459 mm to the upper chest, sword-hand path 235 mm / cycle. Curves untouched.

### BACK → BACKRIGHT
| input | world | MotionSpeed | Back_v001 | BackRight_v001 | parked mocap |
|---|---|---|---|---|---|
| 180° | 0.463 | 1.001 | 1.000 | – | – |
| +170° | 0.463 | 1.001 | 0.772 | 0.220 | 0.007 |
| +160° | 0.463 | 1.001 | 0.540 | 0.431 | 0.029 |
| +150° | 0.463 | 1.001 | 0.328 | 0.655 | 0.017 |
| +140° | 0.463 | 1.001 | 0.111 | 0.887 | 0.002 |
| +135° | 0.463 | 1.001 | – | 1.000 | – |

Monotonic; world speed and playback flat across the sector (both entries 0.463 / 0.405); the parked child peaks at 3 %.
`..._D_Back_BackRight_Back.mp4`: boundary foot speeds ≤ 1.0 m/s (steady 2.7–3.8), knee rates 179–227°/s (steady 235),
pelvis-height rate ≤ 0.60, sword ≤ 0.58, minimum gap 174 mm — the calmest handover in the family.
`..._E_BackLeft_Back_BackRight.mp4`: BackLeft 0.535 / 0.904× → Back → BackRight, boundaries clean (foot ≤ 2.09 vs
steady 3.49, gap ≥ 179 mm), pelvis +31° → −15° → −21.5°, root constant.

Active neighbour toward +90°: `Sword1h_Strafe45RightLoop @ +86.3°` (legacy, co 0.80), so the +90 → +135 sector is
BackRight ↔ that mocap child until Right is authored; not moved.

### ±180 WRAP
GuardGait: +180 = −180 = 0.4630, **+179 = 0.4630** (now interpolating toward BackRight 0.463), −179 = 0.4659 (toward
BackLeft), +157.5 = 0.4630; scale +179 0.4050, −179 0.4064. Runtime +179 ↔ −179 twice: world 0.463 / 0.465 m/s,
MotionSpeed 1.001 / 0.998, Back weight 0.978 / 0.978 (the other 2 % is BackRight at +179 and BackLeft at −179);
per-frame velocity step at the crossings 2.5 mm/s, MotionSpeed step 0.013 (was 0.034 with the mocap neighbour).

### FOOT CONTACT (production runs, forensic stage C)
Knee minimum 25.7° / 25.2° (never straight), toe bone minimum 45 / 30 mm above the floor (no burial), mid-stance ankle
drift 0–1 mm mean / 2 mm max, own lanes with the root-frame gap never below 162 mm in any BackRight segment, pelvis
rhythm 37 mm continuous. Authored audit unchanged from research: knee 43.8° / 47.0°, flat 60 / 63 %, right planted yaw
drift 16.9° (runtime 15.6°) — the family watch item, not corrected.

### IDLE → BACKRIGHT
`..._C_Idle_BackRight_Idle_start.mp4`. Idle pelvis −34.5° / shoulders −39.4° → moving −21.5° / −14.1°: a change of
**+13.0° / +25.3°** (Back: 19.5 / 27.7; Left v002: 54 / 41) — the smallest pelvis change in the family, as the blade
directions agree. Peak yaw rates through the crossfade: pelvis 108°/s (1.8° per frame), shoulders 177°/s; BackRight →
Idle 57°/s / 113°/s; steady 36°/s / 9°/s. Foot reposition at the entry: the right foot moves from the idle's rear-right
stance to the retreat's lead lane at up to 4.0 m/s (67 mm/frame) for ~4 frames inside the crossfade (Back 3.95, Left
2.2) — a stance reposition, no single-frame discontinuity; filed with the idle / locomotion watch item.

### VIDEOS (`Artifacts/AnimationReview/`)
A `BackRight_v001_PRODUCTION_A_lockon_backright.mp4` · B `..._B_unarmoured.mp4` · C `..._C_Idle_BackRight_Idle_start.mp4` ·
D `..._D_Back_BackRight_Back.mp4` · E `..._E_BackLeft_Back_BackRight.mp4` · F `..._F_wrap_plus179_minus179.mp4`.

### TESTS
`AB1_ProductionBackRight_v001_Contract` (asset, (0.7071068, −0.7071068) slot, baked contract, key-identical to
BackRight01, GuardGait 135 = 0.463 and scale 0.405, no 125.3 entry, no competing child within 30° of +135, the other
five production slots unchanged, cycleOffset shared with Back). `AA2` wrap coverage updated: +179 now interpolates
toward BackRight within 0.01 and the +157.5 midpoint lies between BackRight and Back. Suite 51 / 51.

### PRODUCTION STATE
Light_Walk8: Forward v012 (0°), ForwardLeft v001 (−45°), Left v002 (−90°), BackLeft v001 (−135°), Back v001 (180°),
**BackRight v001 (+135°, co 0.87)**; remaining legacy: 86.3° mocap strafe + parked child. Travel_Walk8 preserved
exactly. GuardGait as above. FootIK, grip, Player 2 / 4 untouched. No research clip referenced. VerifyProduction TRUE.
Editor out of Play before every write; controller, GuardGait and prefab read back from disk after. Nothing committed.
