# `Sword1H_WalkLeft_v002` — production install of the approved Left03 (2026-09-04)

Replaces the step-close `Sword1H_WalkLeft_v001` in the true −90° slot with the approved full-cycle, forward-facing
Left03. Production integration only; no artistic change.

### LEFT v002 ASSET
`Assets/Bravehood/Animation/Generated/Sword1H_WalkLeft_v002.anim` (guid f084324a…), a `CopyAsset` promotion of
`__freshHumanLeft_03`. Contract: loopTime on, loopBlend off, orientation / Y / XZ baked, Based Upon Original,
heightFromFeet off, length 0.80 s, canonical root. Lower body, body orientation, sword and shoulder are Left03's by
construction (below).

### SOURCE EQUIVALENCE
102 / 102 bindings, 0 missing; max |Δ value| 0, |Δ time| 0, |Δ tangent| 0; sampled foot positions differ by 0.0004 mm
over 120 samples (float noise). Key-identical.

### CONTROLLER SLOT
`Light_Walk8` child 5: `Sword1H_WalkLeft_v001 @ (−1, 0) ts 1 co 0.35` → `Sword1H_WalkLeft_v002 @ (−1, 0) ts 1 co 0.34`.
Slot position untouched; Forward (0), ForwardLeft (6), BackLeft (4), the right-side mocap children and the parked child 7
untouched (serialized listing verified from disk). Left_v001 no longer referenced by Light_Walk8 (its Travel copy remains
in Travel_Walk8, see BACKLEFT / TRAVEL note).

### CYCLE OFFSET
Measured foot contacts (height-threshold detector, 400 samples per clip):

| clip | length | cycleOffset | left foot lands (clip) | right foot lands (clip) | left lands (tree time) | right lands (tree time) |
|---|---|---|---|---|---|---|
| ForwardLeft_v001 | 0.80 s | 0.92 | 0.99 | ~0.49 (heel-strike, forward convention) | 0.07 | ~0.57 |
| **Left_v002** | 0.80 s | **0.34** | 0.408 | 0.908 | **0.068** | **0.568** |
| BackLeft_v001 | 0.65 s | 0.35 | 0.390 | 0.890 | 0.040 | 0.540 |
| Left_v001 (old) | 0.60 s | 0.35 | 0.390 | 0.843 | 0.040 | 0.493 |

0.34 is the smallest offset that puts Left_v002's landings on ForwardLeft's tree-time landings (Δ ≤ 0.01) and within
0.03 of BackLeft's; the old 0.35 was not inherited but happens to sit one hundredth away.

### GUARDGAIT
`GuardGait_Knight1H` −90°: 0.70 → **0.496 m/s** (Left_v002 native planted-foot speed, 230 mm per stance at 0.80 s × 0.58).
The 1.10× presentation is NOT in the table. Other entries unchanged (−135 0.592, −45 1.24, 0 1.34, mocap right/back).

### GAMEPLAY SPEED SCALE
`gameplayScale` at −90°: 0.53 → **0.477**. Production math: 2.0 × 0.65 × 0.88 × 0.477 = 0.546 m/s; 0.546 / 0.496 = 1.100×.
No playback multiplier is hard-coded anywhere; MotionSpeed follows from the two data entries.

### RUNTIME SPEED / PLAYBACK / CADENCE (real Player, production controller and GuardGait, no override, no speed pref)
| condition | load | world speed | MotionSpeed (min..max) | cadence | 0.25 / 0.5 / 1.0 s | stride error | planted drift |
|---|---|---|---|---|---|---|---|
| gameplay loadout | 0.880 | **0.546 m/s** | **1.100** (1.100..1.100) | **165 spm** | 0.14 / 0.27 / 0.55 m | +0.1 mm/s | 0–2 mm |
| unarmoured | 1.000 | 0.620 m/s | 1.250 | 187 spm | 0.16 / 0.31 / 0.62 m | +0.3 mm/s | 0–3 mm |

Player authority verified before the runs: prefab 2 / 4, scene Player 2 with no instance override; every session diag
logged MaxStableMoveSpeed = 2.

### BODY FACING (real Player, pose stage C, relative to the root)
Player root yaw 354.29°, constant (range 0.00) through every session — the root faces the lock target and nothing
rotates it toward travel. Moving: pelvis **+19.5°** (+12.5…+26.1) toward travel, shoulders **+1.1°** (−0.6…+2.8),
head bone Euler unchanged from idle (UpperBodyAim holds the head on the target). Idle by the same measure: pelvis
−34.5°, shoulders −39.4°.

### IDLE → LEFT
`Left_v002_PRODUCTION_C_Idle_Left_Idle_start.mp4`. The pelvis rotates from the idle's −34.5° to +19.5° through the
crossfade: maximum pelvis yaw rate **387°/s** (6.5° per 60 Hz frame) at 0.35 s into the blend, shoulders **286°/s**;
Left → Idle 274°/s and 196°/s. Steady Left: pelvis 60°/s, shoulders 15°/s. No single-frame discontinuity — the
rotation is spread over the crossfade — so this is a smooth (if brisk) orientation transition, not a pop. Recorded as
`IDLE / LOCOMOTION BODY-FACING TRANSITION WATCH ITEM`: most of the swing is the idle's own right-side blade, to be
reviewed across the whole family; CombatIdle and Left_v002 are not to be changed for it.

### FORWARDLEFT → LEFT
| input | world speed | MotionSpeed | ForwardLeft_v001 | Left_v002 | parked mocap |
|---|---|---|---|---|---|
| −45° | 0.986 | 0.795 | 1.000 | – | – |
| −50° | 0.937 | 0.810 | 0.888 | 0.110 | 0.002 |
| −60° | 0.840 | 0.846 | 0.657 | 0.326 | 0.016 |
| −70° | 0.742 | 0.897 | 0.433 | 0.538 | 0.029 |
| −80° | 0.644 | 0.973 | 0.222 | 0.770 | 0.008 |
| −90° | 0.546 | 1.099 | 0.003 | 0.999 | – |

Monotonic, no mocap take-over (the parked child peaks at 3 %). Visual: `..._D_ForwardLeft_Left_ForwardLeft.mp4`.

### FOOT CONTACT (production runs, forensic stage C)
Knee minimum L 25.5° / R 25.1° (never straight), toe bone minimum 49 / 55 mm above the floor (flat reference 61 / 53 →
no burial), mid-stance ankle drift 0 mm mean / 2 mm max on both feet, right foot swings 1.9–2.2 m/s in its own lane
(a real step, not a close), pelvis rhythm 37 mm continuous. Root-frame right-minus-left foot gap never below **98 mm**
in any Left segment (no crossover, no interception). Right support yaw drift 15–16° = family watch item, unchanged.

### BACKLEFT TRANSITION CHECK
`Left_v002_PRODUCTION_Left_BackLeft_Left.mp4`. Left → BackLeft and BackLeft → Left boundaries: foot speeds 1.5–2.2 m/s
(steady 2.1–2.2), knee rates 271–434°/s (steady 346), pelvis height rate ≤ 1.13 m/s (steady 1.28), sword ≤ 1.21 m/s
(steady 1.35), minimum foot gap 99 mm. No phase mismatch is visible: both clips land the left foot at tree time
0.04–0.07 and the right at 0.54–0.57. BackLeft's slot (co 0.35) left untouched; the contract test allows the 0.01
difference explicitly.

Note on Travel: `Travel_Walk8` still carries `Sword1H_WalkLeft_v001_Travel` at −90° with the travel table rating 0.70 /
0.53 — preserved as instructed; promoting Left_v002 into free travel is a separate step.

### VIDEOS (`Artifacts/AnimationReview/`)
A `Left_v002_PRODUCTION_A_lockon_left.mp4` · B `Left_v002_PRODUCTION_B_unarmoured.mp4` ·
C `Left_v002_PRODUCTION_C_Idle_Left_Idle_start.mp4` · D `Left_v002_PRODUCTION_D_ForwardLeft_Left_ForwardLeft.mp4` ·
E `Left_v002_PRODUCTION_E_Forward_ForwardLeft_Left.mp4` · F `Left_v001_vs_v002_PRODUCTION.mp4` (v001 at its old
0.606 m/s presentation left, v002 at 0.546 m/s right) · `Left_v002_PRODUCTION_Left_BackLeft_Left.mp4`.

### TESTS
X1 updated to `X1_ProductionLeft_v002_Contract` (v002 asset in the true −90° slot, no v001 in the ring, 0.80 s cycle,
baked contract, key-identical to `__freshHumanLeft_03`, GuardGait −90 = 0.496, no −97.1 entry); Y1 now references
Left_v002 and allows a one-frame cycle-offset difference to BackLeft; Y2 now asserts scale −90 = 0.477 and the 0.546 m/s
loadout presentation. Suite 48 / 48.

### PRODUCTION STATE
Light_Walk8: Forward v012 (0°), ForwardLeft v001 (−45°), **Left v002 (−90°, co 0.34)**, BackLeft v001 (−135°). Travel
family, FootIK, grip, Player 2 / 4 untouched. GuardGait −90 = 0.496 / 0.477. No research clip referenced (Left02, Left03,
BackLeft01 research assets remain under Generated, unreferenced). VerifyProduction TRUE. Nothing committed.
