# Sword1H_WalkBackLeft_v001 — production install and qualification (2026-09-04)

Human review: `__freshHumanBackLeft_01` ARTISTICALLY APPROVED at the MEDIUM presentation (0.904× / 0.535 m/s / 167 spm,
tested loadout). No BackLeft02, no motion changes. The ~20° left support-yaw drift is a family-wide WATCH ITEM.

### BACKLEFT v001 ASSET
`Generated/Sword1H_WalkBackLeft_v001.anim` (guid e56dca383ec5b4bbdae40d2f17171580), `AssetDatabase.CopyAsset` of
`__freshHumanBackLeft_01` (unchanged, guid 6a017b42…). Research → production: 102/102 bindings, 0 missing, max |Δvalue|
0, max |Δtime| 0, max |Δtangent| 0, clip settings identical, sampled pose max |Δ| 0.0003 mm over 120 samples (float
noise). Contract: loopTime ON, loopBlend OFF, orientation / Y / XZ baked, Based Upon Original, heightFromFeet OFF,
length 0.65 s, canonical root, controller-driven translation.

### CONTROLLER INSTALL
Serialized `Light_Walk8` inspected by ring POSITION: the active child nearest BackLeft was child 4 at (−0.6211, −0.7837)
= −141.6° (`Sword1h_WalkBwdLoop`, co 0.72). That slot now holds `Sword1H_WalkBackLeft_v001` at the true
(−0.7071068, −0.7071068), timeScale 1, cycleOffset 0.35, mirror off. Forward 0°, ForwardLeft −45°, Left −90° and the
right-side ring untouched (verified from disk). Child 7 stays parked at the origin. The −141.6° mocap child no longer
exists in the tree (Y1 asserts no active child within 30° of −135° other than BackLeft).

The controller was first force-reimported from disk (= git HEAD) before the edit: the editor held an in-memory Travel
tree with three left-side children swapped (FwdLeft → ForwardLeft_v001, Left → Left_v001, BwdLeft → the RESEARCH
`__freshHumanBackLeft_01`) that had been saved to disk twice from outside this pipeline. That state was not carried
into the install: Travel_Walk8 on disk = HEAD (v012_Travel forward, mocap ring). See PRODUCTION STATE.

### CYCLE OFFSET
Measured support phases (root-local foot heights, 200 samples):

| clip | left lands | left lifts | right lands | right lifts |
|---|---|---|---|---|
| Left_v001 | 0.390 | 0.015 | 0.845 | 0.505 |
| BackLeft_v001 | 0.390 | 0.015 | 0.890 | 0.515 |

Both step-close clips share the lead/trail schedule to within 0.045 of a cycle, so the smallest adjustment is none:
BackLeft takes **cycleOffset 0.35 = Left's**, and the Left ↔ BackLeft crossfade always mixes the same support phase
(left foot loaded together, right foot swinging together). The old mocap 0.72 was not inherited.

### GUARDGAIT
`walk`: **−135 : 0.592** (replaces −141.6 : 1.84), −90 : 0.70, −45 : 1.24, 0 : 1.34, 86.3 : 1.81, 125.3 : 1.84, 170.2 : 1.80.
0.592 is the measured native planted-foot speed of the production clip (0.592 / 0.592 m/s L / R); 0.904× is not encoded.

### DIRECTIONAL GAMEPLAY SPEED (new, data-driven, reusable)
`GuardGaitReference.Entry.gameplayScale` — the intended gameplay movement scale per heading in the guard stance
(0 = not set → legacy). `GuardGaitReference.SampleGameplayScale(angle)` interpolates it with the same wrap-around
sampler as the speeds. `CharacterAnimator` samples it at the controller's move-INPUT heading
(`BaseCharacterController.MoveInputHeadingDeg`, new) and writes `DirectionalSpeedScale` / `DirectionalSpeedBlend`
(= stance) on the controller; `BaseCharacterController` lerps its legacy strafe/backpedal interpolation toward that
scale by the blend. No clip-name or angle conditionals; free travel is untouched (blend 0).

Production table: 0 : **1.0**, −45 : **0.862**, −90 : **0.53**, −135 : **0.468**, 86.3 : 0.56, 125.3 : 0.57, 170.2 : 0.599.
The three approved directions and the right/back mocap entries equal the legacy interpolation at their headings
(0.862 = lerp(0.53, 1, cos 45°)), so nothing else moved; only −135° changes (legacy 0.585 → 0.468 = 0.535 / 1.144).
Between entries the scale now interpolates linearly by angle (e.g. −112.5° → 0.499).

### RUNTIME PLAYBACK / CADENCE (real prefab, production controller / GuardGait / speed data, FootIK AnimatedBones,
lock-on, real camera, no override, no status multiplier)

| direction | load | world speed | MotionSpeed (min) | cadence | cycle | 0.25 / 0.5 / 1.0 s | stride error |
|---|---|---|---|---|---|---|---|
| **BackLeft −135° loadout** | 0.880 | **0.535 m/s** | **0.904 (0.904)** | **167 spm** | 0.719 s | 0.13 / 0.27 / 0.54 m | **+0.0 mm/s** |
| BackLeft −135° unarmoured | 1.000 | 0.608 m/s | 1.028 (1.028) | 190 spm | 0.632 s | 0.15 / 0.30 / 0.61 m | −0.0 mm/s |
| Forward 0° loadout | 0.880 | 1.144 m/s | 0.854 (0.853) | 128 spm | 0.937 s | 0.29 / 0.57 / 1.14 m | +0.1 mm/s |
| ForwardLeft −45° loadout | 0.880 | 0.986 m/s | 0.795 (0.795) | 119 spm | 1.006 s | 0.25 / 0.49 / 0.99 m | −0.0 mm/s |
| Left −90° loadout | 0.880 | 0.606 m/s | 0.866 (0.866) | 173 spm | 0.693 s | 0.15 / 0.30 / 0.61 m | +0.2 mm/s |

Forward / ForwardLeft / Left are unchanged from their qualification runs (1.144 / 0.854, 0.987 / 0.796, 0.606 / 0.866).
Load reduces movement and playback together (0.880 on both). No direction touches the 0.6 clamp.

### STRIDE MATCH
world − native × MotionSpeed = +0.0 (BackLeft loadout), −0.0 (unarmoured), +0.1 / −0.0 / +0.2 mm/s (F / FL / L).
Across the −90…−135 sweep MotionSpeed follows the interpolated rating (0.866 → 0.904) with clip weights summing to 1.

### LEFT → BACKLEFT BLEND (sweep, loadout)
| input | Left_v001 | BackLeft_v001 | parked Strafe45Left | MotionSpeed | world |
|---|---|---|---|---|---|
| −90° | 1.000 | — | — | 0.866 | 0.606 |
| −100° | 0.773 | 0.219 | 0.007 | 0.874 | 0.591 |
| −110° | 0.541 | 0.430 | 0.029 | 0.881 | 0.575 |
| −120° | 0.329 | 0.654 | 0.017 | 0.890 | 0.559 |
| −130° | 0.112 | 0.886 | 0.002 | 0.899 | 0.543 |
| −135° | 0.002 | 0.999 | — | 0.904 | 0.535 |
Monotonic Left → BackLeft; the only other contributor is the parked origin child at ≤ 0.029 (pre-existing
freeform-directional origin leakage). No mocap clip appears anywhere in the −45…−135 sector.

### FOOT / CROSSOVER CHECK
Root-frame right-minus-left foot X gap (positive = right foot stays right of the left):
pure BackLeft min **+412 mm** (0 / 599 frames crossed); Left → BackLeft → Left min **+322 mm** (0 / 899);
ForwardLeft → Left → BackLeft → Left → ForwardLeft: Left segments ≥ +111 mm, BackLeft segment ≥ +376 mm, 0 crossed
frames in either — the only negative gaps are inside the ForwardLeft segments (−135 mm), which is that clip's normal
alternating diagonal stride, not a retreat cross-behind. The right recovery foot never passes behind or across the
planted left foot.

Transition scan (per-frame bone speeds, boundaries ±0.4 s vs steady envelope): Left→BackLeft, BackLeft→Left,
ForwardLeft↔Left, BackLeft→stop all within the steady envelope (knee rate, pelvis Y, chest, sword included). Idle→BackLeft
entry: first-step foot speeds 2.8 / 3.5 m/s vs steady 1.8 / 1.9 (47–59 mm per frame — a fast first step out of the
idle crossfade, no teleport; Left's own idle entry peaks 2.6). No phase mismatch: the shared cycleOffset keeps the
lead foot loaded through the crossfade.

### PLAYER FACING
Root yaw 354.29° constant (range 0.000°) through every BackLeft run = the lock target; movement heading −140.7° world =
true −135° relative to root; input −135.0°; planted-foot tracks −135.0° in the clip. Authored pelvis 36° toward the
retreat, shoulders 11°, head on the threat (−0.3°) — preserved by the key-identical clip.

### FOOT CONTACT (production runs, runtime forensic)
| run | knee min L / R | toe-bone min L / R (flat 61 / 53) | stance ankle drift | support yaw drift L / R |
|---|---|---|---|---|
| loadout | 24.0 / 25.1° | 55 / 56 mm | 0–1 mm mean (max 10 / 2) | 17.1° / 5.0° |
| unarmoured | 24.5 / 25.0° | 54 / 56 mm | 0–1 mm mean (max 4 / 1) | 17.5° / 5.4° |
No knee lock, no toe burial (the right toe bone sits 3 mm ABOVE its flat reference), the ball-first landing and
world-pitch solve are in the curves (key-identical), soles compatible with FootIK AnimatedBones (feet planted to ≤ 1 mm
mean drift). Left support yaw drift ~17° recorded as the family-wide WATCH ITEM (Left01 14°, BackLeft 17°); no
correction pass added. FootIK untouched.

### VIDEOS (`Artifacts/AnimationReview/`)
- A `BackLeft_v001_PRODUCTION_A_pure135_selected.mp4` — pure −135°, loadout, approved presentation (idle → BackLeft → idle, = D)
- B `BackLeft_v001_PRODUCTION_B_unarmoured.mp4`
- C `BackLeft_v001_PRODUCTION_C_Left_BackLeft_Left.mp4`
- E `BackLeft_v001_PRODUCTION_E_Forward_ForwardLeft_Left_BackLeft.mp4` — family progression at each direction's presentation

### TESTS
`Y1_ProductionBackLeft_v001_Contract` (asset path, true (−0.7071, −0.7071), ts 1, baked settings, 0.65 s, key-identity
incl. tangents/times, shared cycleOffset with Left, no active child within 30° of −135°, Forward / ForwardLeft / Left
positions, GuardGait −135 ≈ 0.59 native, −90 / −45 / 0 unchanged, no −141.6 entry) and
`Y2_GuardGait_PerHeadingGameplayScale_Contract` (every entry scaled, 1.0 / 0.862 / 0.53 / 0.468 at the four production
headings, 0.535 and 0.606 m/s presentations at the loadout, Player baseline 2 / 4, empty table → legacy −1).
Result: **47 passed, 0 failed, 0 skipped** (job d1648633). Player prefab unchanged by the run.

### PRODUCTION STATE
Player.prefab MaxStableMoveSpeed 2 / MaxSprintSpeed 4 verified on disk before and after every write (unchanged through
the whole milestone). StrafeSpeedMultiplier 0.53 / Backpedal 0.6 untouched (now bypassed in the guard stance by the
table; still the free-travel and fallback path). CombatWalkSpeedScale 0.65. Forward v012, ForwardLeft v001, Left v001,
Travel v012_Travel, FootIK, grip untouched (md5 verified). Harness disarmed (no override, no gait pref, no speed pref).
Nothing committed. Stamina / health bar materials modified outside this pipeline and left alone.

External controller edits observed (not from this pipeline): between turns the Travel_Walk8 tree was rewritten on disk
with FwdLeft → ForwardLeft_v001, Left → Left_v001, BwdLeft → `__freshHumanBackLeft_01` (a research clip, which would
fail VerifyProduction). Restored to HEAD before the install and force-reimported. If those Travel swaps are wanted,
they need their own install (Travel is free-travel, faces movement, and the combat clips are lock-on strafes).
