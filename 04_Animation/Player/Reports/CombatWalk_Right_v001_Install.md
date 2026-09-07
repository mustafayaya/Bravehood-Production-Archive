# `Sword1H_WalkRight_v001` — production install of the approved Right01 (2026-09-05)

Installs the approved full-cycle sword-side lateral strafe at +90° in `Light_Walk8` with the Left-matched 1.10×
presentation. Production integration only; no artistic change.

### RIGHT v001 ASSET
`Assets/Bravehood/Animation/Generated/Sword1H_WalkRight_v001.anim` (guid 01484aeb…), a `CopyAsset` promotion of
`__freshHumanRight_01`. Contract: loopTime on, loopBlend off, orientation / Y / XZ baked, Based Upon Original,
heightFromFeet off, 0.80 s, canonical root, controller-driven translation. Research source untouched.

### SOURCE EQUIVALENCE
102 / 102 bindings, 0 missing; max |Δ value| 0, |Δ time| 0, |Δ tangent| 0; sampled feet within 0.0003 mm over 120
samples. Key-identical.

### CONTROLLER SLOT
Serialized ring before the edit: slot 1 was `Sword1h_Strafe45RightLoop @ +86.3° (0.9979, 0.0645) co 0.80`, the only
child within 20° of +90. Replaced by `Sword1H_WalkRight_v001 @ (1, 0) ts 1 co 0.87`. Active family now: Forward 0°,
Right +90°, BackRight +135°, Back 180°, BackLeft −135°, Left −90°, ForwardLeft −45°; the parked origin child (7) kept.
Between 0° and +90° no active child remains — the +45 sector interpolates Forward v012 ↔ Right v001 until ForwardRight
exists (nothing moved). Verified from the file on disk (mocap guid gone from the ring, no research guid).

### CYCLE OFFSET
Measured on the finished production clip (12 mm contact threshold, 400 samples): Right lands the left foot at 0.900
and the right at 0.400. BackRight (co 0.87) lands at tree time 0.025 / 0.525; **co 0.87** puts Right at 0.030 / 0.530
(Δ 0.005). Left v002 (co 0.34) lands at 0.060 / 0.560 (Δ 0.030). Smallest offset satisfying both neighbours.

### GUARDGAIT
The legacy 86.3 : 1.81 entry replaced by **90 : 0.496 m/s** (native; = Left v002's rating; raw-clip stance-window
velocity 0.464 / 0.464 m/s along travel over the detector window, planted at runtime to 0 mm). Table now
−135 0.592 / −90 0.496 / −45 1.24 / 0 1.34 / 90 0.496 / 135 0.463 / 180 0.463 — seven true family headings. The 1.10×
presentation is not in the table.

### GAMEPLAY SPEED SCALE
`gameplayScale` at +90° = **0.477** (= −90's); production math 2.0 × 0.65 × 0.88 × 0.477 = 0.546 m/s → 1.100×.
Left and Right share one presentation by data; +112.5 samples 0.4795 / 0.441, +45 (now Forward↔Right) 0.918 / 0.739.

### RUNTIME PLAYBACK / CADENCE (real Player, production controller and GuardGait, no override, no speed pref)
| condition | load | world speed | MotionSpeed | cadence | 0.25 / 0.5 / 1.0 s | stride error | planted drift |
|---|---|---|---|---|---|---|---|
| gameplay loadout | 0.880 | **0.546 m/s** | **1.100** | **165 spm** | 0.14 / 0.27 / 0.55 m | −0.1 mm/s | 0 mm |
| unarmoured | 1.000 | 0.620 m/s | 1.250 | 188 spm | 0.16 / 0.31 / 0.62 m | +0.0 mm/s | 0 mm |
| Left v002 pair (same session batch) | 0.880 | 0.546 m/s | 1.100 | 165 spm | | | |

Travel relative to the root +90.0°, root yaw 354.29° constant. Player authority verified before the runs (prefab 2 / 4,
scene Player 2 with no override); every session diag logged MaxStableMoveSpeed = 2.

### BODY FACING
Moving: pelvis **−14.7°** (−21.0…−8.9), shoulders **−11.6°** (−13.3…−10.0) relative to the root, head on the target,
root constant. The approved modest right blade; not squared to match Left (+19.5 / +1).

### RIGHT FOOT
On the true +90° slot the planted feet drift 0 mm through mid-stance (the research 9–10 mm was the +86.3° study-slot
slide). Right boot world heading mean −0.1° authored (forward through stance), swing reorientation ±10°. Support yaw:
the research forensic's mid-stance metric read 2.6–3.4°; this milestone's whole-stance metric (ankle→toe heading range
across the entire planted window, edges included) reads 12.6° right / 17.1° left — the same clip, a wider window; the
right boot remains the most stable planted foot in the family by either measure. No inward twist, no toe-out.

### FOOT CONTACT (production runs)
Knee minimum 36.1° / 42.2° (never straight), toe bone minimum 60 / 52 mm (flat 61 / 53 — no burial), minimum boot
separation 273 mm in Right, 177 mm through the BackRight ↔ Right blend, both feet in their own lanes, alternating
steps with continuous pelvis transfer. Authored audit unchanged from research (knee 36 / 42°, flat 62 / 62 %).

### RIGHT-ANKLE TEMPORAL NOTE
On the true slot the right boot's world pitch changes at most 44°/s (0.7° per 60 Hz frame; p95 41°/s) — the audit's
9.3° / 108°/s release is spread by the runtime writers and is not a visible snap. Accepted; curves untouched.

### BACKRIGHT → RIGHT
| input | world | MotionSpeed | BackRight_v001 | Right_v001 | parked mocap |
|---|---|---|---|---|---|
| +135° | 0.463 | 1.001 | 1.000 | – | – |
| +125° | 0.482 | 1.024 | 0.772 | 0.220 | 0.007 |
| +115° | 0.500 | 1.047 | 0.540 | 0.431 | 0.029 |
| +95° | 0.536 | 1.090 | 0.111 | 0.887 | 0.002 |
| +90° | 0.546 | 1.100 | – | 1.000 | – |

Monotonic; stride matched at every input (playback = world / interpolated native); the parked child peaks at 3 %.
`..._D_BackRight_Right_BackRight.mp4`: boundary foot speeds ≤ 1.17 m/s (steady 3.69), hip rate ≤ 0.82, sword ≤ 0.61,
minimum boot separation 183 mm — no pop. `..._F_Back_BackRight_Right.mp4`: Back 0.463 → BackRight 0.463 → Right 0.546,
boundaries ≤ 1.29 m/s, separation ≥ 186 mm; pelvis −15 → −21.5 → −14.7, root constant.

### IDLE → RIGHT
`..._C_Idle_Right_Idle_start.mp4`. Idle pelvis −34.5° / shoulders −39.4° → moving −14.7° / −11.6°: change **+19.8° /
+27.8°** (Back 19.5 / 27.7, BackRight 13 / 25, Left 54 / 41). Peak yaw rates through the crossfade: pelvis 172°/s
(2.9° per frame), shoulders 200°/s; Right → Idle 117 / 136°/s; steady 52 / 14°/s. Foot reposition at the entry: maximum
foot speed 2.37 m/s (40 mm/frame) inside the 0.25 s crossfade — lower than Back (3.95) and BackRight (4.0), in Left's
class (2.2); no single-frame discontinuity, no pop.

### VIDEOS (`Artifacts/AnimationReview/`)
A `Right_v001_PRODUCTION_A_lockon_right.mp4` · B `..._B_unarmoured.mp4` · C `..._C_Idle_Right_Idle_start.mp4` ·
D `..._D_BackRight_Right_BackRight.mp4` · E `..._E_Left_v002_vs_Right_v001.mp4` (both production, both 0.546 / 1.10×) ·
F `..._F_Back_BackRight_Right.mp4` · `Left_v002_PRODUCTION_pair_for_Right.mp4`.

### TESTS
`AC1_ProductionRight_v001_Contract` (asset, (1,0) slot, baked contract, key-identical to Right01, GuardGait 90 = 0.496
and scale 0.477 = Left's, no 86.3 entry, no competing child within 30° of +90, the other six production slots unchanged,
cycleOffset shared with BackRight). Suite 52 / 52.

### PRODUCTION STATE
Light_Walk8: Forward v012 (0°), **Right v001 (+90°, co 0.87)**, BackRight v001 (+135°), Back v001 (180°), BackLeft v001
(−135°), Left v002 (−90°), ForwardLeft v001 (−45°); parked origin child kept; only ForwardRight (+45°) missing.
Travel_Walk8 preserved exactly. GuardGait as above. FootIK, grip, Player 2 / 4 untouched. No research clip referenced.
VerifyProduction TRUE. Editor out of Play before every write; controller, GuardGait and prefab read back from disk after.
Git index untouched; nothing added or committed.
