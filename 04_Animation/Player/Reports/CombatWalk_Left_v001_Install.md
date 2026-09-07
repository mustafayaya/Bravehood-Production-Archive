# Sword1H_WalkLeft_v001 — production install and qualification (2026-09-04)

Human review: `__freshHumanLeft_01` ARTISTICALLY APPROVED at the stride-matched presentation 0.874× / 0.612 m/s / 175 spm
(tested loadout). No Left02, no foot-placement pass, no head correction. This report productionizes it.

### LEFT v001 ASSET
- `Generated/Sword1H_WalkLeft_v001.anim` (guid 0b13a9196149044268ceaebf099840dd), created by `AssetDatabase.CopyAsset` from
  `__freshHumanLeft_01` — 102 curves, key-identical (test X1 asserts max |Δ| = 0 over every key). Research clip unchanged
  (md5 88cba72b).
- Clip contract: loopTime ON, loopBlend OFF, orientation / Y / XZ baked, Based Upon Original on all three, heightFromFeet OFF,
  length 0.600 s, canonical root, controller-driven movement (XZ baked = in place).
- Measured native planted-foot speed of the production clip (root-local, 240 samples, planted = lowest 12 mm, trimmed
  mean): **0.694 m/s** → GuardGait authority 0.70.

### CONTROLLER INSTALL
Serialized `Light_Walk8` ring, inspected before the change: child 5 was `Sword1h_Strafe135LeftLoop` at (−0.9923, −0.1236)
= −97.1°, cycleOffset 0.52 — the rotated mocap child. It is the slot replaced (chosen by ring position, not by name):

| child | motion | position | ts | co |
|---|---|---|---|---|
| 0 | Sword1H_WalkForward_v012 | (0, 1) 0° | 1 | 0.92 |
| 5 | **Sword1H_WalkLeft_v001** | **(−1, 0) −90°** | 1 | 0.35 |
| 6 | Sword1H_WalkForwardLeft_v001 | (−0.7071, 0.7071) −45° | 1 | 0.92 |
| 7 | Sword1h_Strafe45LeftLoop | (0, 0) parked | 1 | 0.98 |
| 1–4 | right/back mocap ring | unchanged | | |

cycleOffset 0.35 aligns Left's left-foot landing (clip phase 0.40) with ForwardLeft's (phase ≈0.97 at co 0.92) so the
−45…−90 blend region crossfades on matched footfalls. No −97.1° active child remains; X1 asserts no active child within
30° of −90° other than Left.

### GUARDGAIT
`GuardGait_Knight1H.walk`: −141.6:1.84, **−90:0.70**, −45:1.24, 0:1.34, 86.3:1.81, 125.3:1.84, 170.2:1.80.
The −97.1:1.82 mocap entry was replaced. 0.70 is the native rating (measured 0.694); the 0.874× presentation is NOT encoded here.

### SELECTED GAMEPLAY SPEED
Implemented through the existing directional movement multiplier on the Player: `StrafeSpeedMultiplier` 0.8 → **0.53**
(YAML patch, no prefab bake). Lateral target = MaxStable × CombatWalkSpeedScale 0.65 × load × 0.53. No Animator speed
hack; MotionSpeed = world / GuardGait(−90) stride-matches by construction.

**Baseline caveat (see PRODUCTION STATE):** the selected numbers below assume `MaxStableMoveSpeed = 2.0`, which every
approved measurement was taken at. The committed prefab now carries 1.2 (changed in the in-game session between the
verified run and the commit). Sessions marked ✱ reproduce the 2.0 baseline non-destructively via
`StatusSpeedMultiplier × 1.667` so the install could be qualified without touching that field.

### PLAYBACK / CADENCE + LOAD SCALING (pure −90°, production stack, lock-on) ✱
| condition | load | world speed | MotionSpeed | cadence | cycle | stride error |
|---|---|---|---|---|---|---|
| Warrior Base loadout (16 kg) | 0.880 | **0.606 m/s** | **0.866×** | **173 spm** | 0.693 s | +0.2 mm/s |
| unarmoured | 1.000 | 0.689 m/s | 0.984× | 197 spm | 0.610 s | +0.1 mm/s |
Load reduces movement and playback together (0.880 on both), as before.

### STRIDE MATCH
world − 0.70 × MotionSpeed = +0.2 mm/s (loadout), +0.1 mm/s (unarmoured); across the −45…−90 sweep MotionSpeed follows
the interpolated GuardGait rating (0.796 → 0.866) with clip weights summing to 1.

### FORWARDLEFT → LEFT BLEND (sweep, loadout) ✱
| input | FL_v001 | Left_v001 | parked Strafe45Left | MotionSpeed | world |
|---|---|---|---|---|---|
| −45° | 1.000 | — | — | 0.796 | 0.987 |
| −50° | 0.888 | 0.110 | 0.002 | 0.807 | 0.952 |
| −60° | 0.657 | 0.326 | 0.016 | 0.825 | 0.875 |
| −70° | 0.433 | 0.538 | 0.029 | 0.841 | 0.791 |
| −80° | 0.222 | 0.770 | 0.008 | 0.853 | 0.700 |
| −90° | 0.003 | 0.999 | — | 0.866 | 0.607 |
Monotonic FL → Left hand-over; the only other contributor is the parked origin child at ≤ 0.029 (freeform-directional
origin leakage, pre-existing architecture). No rotated mocap child appears.

### FOOT PLACEMENT PRESERVATION
Key-identical promotion (X1). No procedural pass ran. Runtime foot-contact forensic on the production run matches the
approved study run frame for frame (knee min L 3.0 / R 1.9°, toe min L 50 / R 32 mm, pelvis range 61 mm, 790 mm max
separation, same stance schedule).

### PLAYER FACING
Root yaw constant 354.29° (lock target) for the whole −90° travel — no rotation toward travel. Runtime hip-axis yaw
−32° (−38…−26 over the cycle), shoulder-axis −6° relative to root; the authored 38° / 12° / 3° relationship is preserved
by the unchanged clip (runtime numbers are bone-axis measures through FootIK/UpperBodyAim, not the authoring frame).
No head correction added.

### TRANSITIONS (production stack, per-frame bone-speed scan, boundaries ±0.4 s vs steady-state envelope) ✱
- ForwardLeft → Left → ForwardLeft: Left-foot peak 2.96 m/s at the FL→Left crossfade (steady 2.33; ×1.27, one swing
  re-phasing, no teleport — a teleport would read > 10 m/s); Left→FL and FL→stop within envelope. Knee rates, pelvis Y,
  chest and sword all within envelope.
- Forward → ForwardLeft → Left → ForwardLeft → Forward: every boundary within the steady envelope.
- Idle → Left → Idle: within envelope; entry 9 frames of transition.
No foot teleport, crossing, knee snap, pelvis pop, torso pop or sword pop found. Motions not redesigned.

### VIDEOS (`Artifacts/AnimationReview/`) ✱
- A `Left_v001_PRODUCTION_A_pure90_selected.mp4` — pure −90°, loadout, selected presentation (also Idle → Left → Idle = D)
- B `Left_v001_PRODUCTION_B_unarmoured.mp4`
- C `Left_v001_PRODUCTION_C_ForwardLeft_Left_ForwardLeft.mp4`
- E `Left_v001_PRODUCTION_E_Forward_ForwardLeft_Left_ForwardLeft_Forward.mp4` — family context
- `Left_v001_PRODUCTION_lockon_left.mp4` — the first production run at the real 2.0 prefab baseline (identical numbers)

### TESTS
45 / 45 (job ea40c515). New `X1_ProductionLeft_v001_Contract`: production asset path, true (−1, 0), timeScale 1, fully
baked settings, 0.6 s, no research clip, key-identity with `__freshHumanLeft_01`, no active child within 30° of −90°,
Forward/ForwardLeft positions intact, GuardGait −90 = 0.70 (native, not 0.874), −45 / 0 unchanged, no −97.1 entry,
StrafeSpeedMultiplier 0.53. No aesthetic thresholds.

### PRODUCTION STATE
- `AnimationAssetSafety.VerifyProduction` = TRUE: no research clip referenced. Forward v012, ForwardLeft v001,
  Travel v012_Travel, FootIK, grip untouched.
- **Blocker:** `Player.prefab` MaxStableMoveSpeed 2 → 1.2 and MaxSprintSpeed 4 → 3 entered the prefab from the in-game
  session after the verified run and were swept into commit eb4f9c73. At 1.2 the guard walk targets become
  0.686 (0°), 0.592 (−45°), 0.364 m/s (−90°) — all below the MotionSpeed floor 0.6 × native, so every heading clamps at
  0.600× and skates (Forward 15 %, Left 13 %); the selected Left presentation is not what plays. The field is a gameplay
  decision and was not reverted by this pipeline. Two exits: restore 2.0 / 4 (all measurements above then hold at face
  value), or keep 1.2 and re-select the whole family's presentations at the new baseline (lower MotionSpeed floor plus a
  new strafe scale) — a human choice.

## PLAYER SPEED BASELINE RESTORED (2026-09-04, human decision)

`Player.prefab`: MaxStableMoveSpeed 1.2 → 2, MaxSprintSpeed 3 → 4 (YAML patch; the working-tree diff is exactly those two
lines). Nothing else touched: CombatWalkSpeedScale 0.65, StrafeSpeedMultiplier 0.53, Backpedal 0.6, Status 1.

Re-qualification on the REAL prefab — no status multiplier, no override, production controller / GuardGait / FootIK
AnimatedBones, Warrior Base loadout (load 0.880), lock-on, real camera:

| heading | clip (weight) | world speed | MotionSpeed (min..max) | cadence | cycle | stride error | frames at 0.600 floor (steady / whole move segment) |
|---|---|---|---|---|---|---|---|
| Forward 0° | v012 (1.000) | 1.144 m/s | 0.854 (0.853..0.854) | 128 spm | 0.937 s | +0.1 mm/s | 0 / 0 |
| ForwardLeft −45° | FL_v001 (1.000) | 0.987 m/s | 0.796 (0.795..0.796) | 119 spm | 1.006 s | +0.0 mm/s | 0 / 0 |
| Left −90° | Left_v001 (1.000) | 0.606 m/s | 0.866 (0.866..0.866) | 173 spm | 0.693 s | +0.2 mm/s | 0 / 0 |

None of the three directions touches the 0.600 clamp at any moving frame (ramp-in included); playback equals
world / native everywhere, so no clamp-induced skate exists. Left lands on the approved presentation (0.606 m/s,
0.866×, 173 spm; the earlier 0.612 / 0.874 figures were the study's status-multiplier approximation of the same target).

Foot forensic on the real-prefab runs: Left stance ankle drift 0–1 mm mean (max 3 mm) — planted; Forward / ForwardLeft
unchanged from their qualification runs. Tests 45 / 45 (job dd4b509a). VerifyProduction TRUE. Light and Travel forward
slots reference separate assets (v012 vs v012_Travel); Travel slot configuration untouched.

Videos: `Restore_PRODUCTION_Forward_v012_lockon.mp4`, `Restore_PRODUCTION_ForwardLeft_v001_lockon.mp4`,
`Restore_PRODUCTION_Left_v001_lockon.mp4`.
