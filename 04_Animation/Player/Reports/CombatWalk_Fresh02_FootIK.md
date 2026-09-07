# FootIK root cause and minimum repair — measured on the real Player

`__freshHumanForward_02` untouched (`a1630664`), Transport copy curves untouched. No v012. No
production install. Transport (Y baked, `GuardGait_Fresh02`, 0.970× at 1.30 m/s) used for every
session; lock target dead ahead (pure forward); UpperBodyAim / WeaponInertia bypassed for the
primary qualification. Analysis: `scratchpad/transport/iktrace.py`, per-frame FootIK internals
exported through a new `FootIK.FootTrace` (never read by gameplay).

## FOOTIK CALL CHAIN (as shipped before this milestone)

`FootIK` on the Player root (same GameObject as the Animator), script order 0, `OnAnimatorIK(layer)`
on every layer with IK Pass — solves on the frame's first callback, `Reapply()` on later layers.

1. Authority: LOD tier from camera distance (Player: `AlwaysHighQuality`), `Weight 1`, ×0.8 while
   attacking, 0 when dead / airborne / (rolling and `!GroundFeetWhileRolling`); smoothed 0.10 s.
2. **Goal source:** `Animator.GetIKPosition/GetIKRotation(LeftFoot/RightFoot)` read before
   anything is written. Goal weights read: whatever the component itself set last frame.
3. **Goal repair** (`RepairGoal`): estimate of the animated ankle =
   `bone.position − up × lerp(pelvisApplied, Offset, AppliedPosW)` (the bone holds LAST frame's
   final, already-corrected pose) + one frame of its own delta; broken-goal test = goal drop
   below the body ÷ ankle drop below the body `< 0.5` (enter) / `> 0.65` (exit), 0.12 s
   crossfade; a broken goal is replaced by the estimate **anchored in root space**, captured only
   while the foot's weight is `< 0.3` and carried by the root while weighted; rotation position-only
   until a bone-to-goal rotation offset has been learned from honest frames (never, on a goal-less
   clip).
4. **Probe** (`Solve`): sole = goal − goalUp × `_bottom` (avatar feet-bottom height, 70 mm here);
   heel = sole − goalFwd × 0.055, toe = sole + goalFwd × 0.16 (root axes while the goal is rebuilt
   and uncalibrated); `Physics.RaycastNonAlloc` from 0.5 m above each point, 1.2 m down,
   `GroundLayers` = Default, own-character colliders rejected via a static registry; clearance =
   min(heel, toe) sole height above hit; `TargetOffset = −clearance`, tilt from heel/toe contacts.
5. **Weight** (`Weigh`): released by lift (this sole above the lower sole by 40→160 mm), rise
   (> 0.15 m/s), goal speed (> 0.5 m/s), both-feet-airborne; engage 0.09 s, release 0.025 s.
6. **Pelvis:** `Animator.bodyPosition += up × min(weighted offsets)`, smoothed 0.12 s, drop only.
7. **Apply:** `SetIKPosition(goal + up × Offset)`, `SetIKRotation(tilt × goalRot)`, weights
   `w × 1.0` / `w × 0.8 × rotTrust`.

## GETIKPOSITION ON A GOAL-LESS CLIP — what Unity returns

Measured inside FootIK before it writes anything (Fresh02 Transport, `ik_fresh_legacy`):

- `GetIKPosition` = **(−61, 797, 0) mm root-local, every frame** — the root's XZ (a fixed lateral
  offset), 86 mm above `bodyPosition`, invariant to the pose. Not the ankle, not the toe, not the
  body frame; the avatar's parked default. Drop ratio 0.005 (an honest goal reads ~1).
- Once FootIK has set a goal, the getter hands **that** goal back next frame (position and weight
  persist): read after FootIK, `ik0` equalled the previous applied goal within 26 mm median
  while weighted and jumped back to the parked point (760 mm) whenever the weight fell to 0.
- Control, `Loco_Walk_Fwd` (imported, 14 goal curves): `GetIKPosition` sits **105–146 mm
  horizontally / 5–17 mm vertically from the retargeted ankle bone** — it is the source rig's foot
  placement in the body frame (`bodyRotation × goalT × humanScale + bodyPosition` fits to 60 mm
  median), not this rig's bone. So even an "honest" goal is a retarget target, and FootIK on the
  mocap set pulls the retargeted feet 80–140 mm toward it (goalUsed-vs-ankle 139 / 81 mm median
  while weighted). That is pre-existing behaviour, not touched here.
- Edit Mode cannot answer this: `AnimationMode.SamplePlayableGraph` moves the bones but
  `GetIKPosition` / `bodyPosition` return (0,0,0) outside Play Mode.

Fresh02, v011, v008 (all generated) carry **no** `LeftFootT/Q`, `RightFootT/Q` curves; every
imported FBX clip does.

## BAD FRAME TRACE (`ik_fresh_legacy`, frame 348, phase 0.247 — left stance, right swing; root-local mm)

| | value |
|---|---|
| animated (raw copy, same phase) | Hips y 774 · L knee (−133, 438, 112) · L ankle (−83, 73, 71) · L toes (−113, 49, 143) · foot rot (69.0, 75.1, 66.8) |
| Animator IK goal read by FootIK | **(−61, 797, 0)**, rot (87, 90, 180); drop ratio **0.005** → repair ON, blend 1.0 |
| FootIK's ankle estimate | (−83, **195**, 73): X/Z right to 2 mm, Y **+122 mm wrong** — it subtracts last frame's offset (−134 × w 0.79) and pelvis shift (−88) from a bone the IK had already pulled to the ground |
| rebuilt goal (root anchor) | (−80, 194, **162**): 90 mm AHEAD of the foot — captured earlier in the stride at weight < 0.3, then carried forward by the root at 1.3 m/s while the planted foot stayed behind |
| raycasts | heel hit y 0 (floor), toe hit y 0, normals (0,1,0), bias 143 mm; clearance from the estimate's sole **135 mm** |
| correction | TargetOffset −134.9, Offset −134.2 (smoothed), weight 0.789, lift 0, rise 0.015, speed 0.30 m/s |
| applied goal | (−80, 59, 162) posW 0.789 rotW 0; **pelvis −88 mm** |
| after FootIK | Hips y **686** (−88) · knee (−155, 417, **219**) · ankle (−80, 59, **162**) · toes (−105, 30, 233) |

A self-consistent wrong fixed point: the foot ends on the floor, the estimate insists the animation
holds it 135 mm up, the pelvis is dropped by the "missing" reach, and the root-anchored goal drags
the leg 90 mm forward. Over the window: pelvis −135 mm max (115 rms), knees 443 / 423 mm (74°),
ankles 507 / 494 mm, toes 519 / 501 mm, toes 86 / 106 mm under the floor, 1-frame foot steps up
to 394 / 513 mm (the previous milestone's "hoist" of 100–170 mm was the same loop's other fixed
point, on the unbaked clip under Warrior Base).

## CLIP COMPARISON (legacy FootIK, unarmoured, production rating for v008/v011)

| clip | goal curves | goal vs animated ankle | repair path | FootIK correction | resulting ankle / toe displacement |
|---|---|---|---|---|---|
| Fresh02 Transport | none | 600–670 mm (parked) | on 100 % | offsets −127…−190 mm, pelvis to −135 | ankles 507 / 494, toes 519 / 501 mm; toes −86 / −106 mm under floor |
| v011 (production, locked) | none | 698–751 mm | on 100 % | offsets −29…−146 mm | vs its own Animator output: pelvis 36, knees 231 / 202, ankles **415 / 300**, toes 422 / 298 mm; toes −31 mm (raw +59) |
| v008 (production travel, unlocked, 0.67 + 0.33 `Loco_Jog_Fwd`) | none | 586–822 mm | on 100 % | offsets −219…+25 mm | pelvis **169**, knees 199 / 187, ankles 219 / 217 mm; toes **−140 / −148 mm** (raw +34 / +26) |
| `Loco_Walk_Fwd` (imported control) | 14 | 113–162 mm (retarget) | off (ratio 0.86–1.10) | offsets −92…+40 mm | feet pulled 81–139 mm toward the source-rig goal (existing behaviour) |

**Systemic.** Every generated clip — including both production slots — runs the broken path today.

## ROOT CAUSE

`FootIK hoists / drags the stance leg on generated clips because it takes Unity's Humanoid IK goal as
its baseline, and on a clip without goal curves that goal is a parked point at the root (≈86 mm
above the body centre, pose-invariant); the fallback rebuilds the goal from the ankle bone read
INSIDE OnAnimatorIK — which still holds last frame's final, IK-corrected pose — by subtracting its own
previous correction (an estimator that feeds back on itself) and then anchors that estimate in ROOT
space while the foot is weighted, so a planted foot's goal travels with the body at 1.3 m/s; ground
clearance is measured from that wrong estimate, and the pelvis drop, foot offset and leg solve all
converge on a fixed point 90–500 mm from the authored foot.`

## REPAIR OPTIONS

| | mechanism | Fresh02 flat, unarmoured (vs raw at the same phase) |
|---|---|---|
| A — current (AnimatorGoals + root anchor) | above | pelvis 135, knees 443 / 423, ankles 507 / 494, toes −86 / −106 under floor |
| C — DisableWhenInvalid (goal judged broken → foot released) | `GoalPolicy.DisableWhenInvalid` on the legacy path | weights 0: identical to FootIK off (≤ 8 mm); no terrain adaptation at all |
| **B — AnimatedBones (selected)** | below | **pelvis 4.0 mm, ankles 15.3 / 5.4, toes 15.3 / 5.4, knees 63.6 / 25.1 mm** |

An "animated-foot baseline" cannot be built inside `OnAnimatorIK`: the bones there are stale and
already corrected, so any estimate that un-corrects them is a low-pass with pole = IK weight, i.e.
frozen at full weight (the foot could never be released). The clean animated foot exists exactly
once per frame: after the Animator writes it, before anything else runs.

## SELECTED MINIMUM REPAIR — `FootIK.Mode.AnimatedBones` (default)

`FootIK` now runs at **LateUpdate −50** (after the Animator, before AccelerationLean / UpperBodyAim /
WeaponInertia) and:

- reads the authored ankle / foot rotation / toe bone **from the transforms the Animator just wrote**
  (world-absolute, captured before the pelvis settles). The Animator re-writes the clean pose every
  frame, so nothing this component does can feed back into what it reads. No goal, no estimator,
  no anchor, no rotation-offset learning;
- probes the ground from those bones: heel = ankle − up × 70 mm − footFwd × 55 mm (foot forward =
  ankle→toe bone, so bone axes never matter), toe = toe bone − up × `ToeBottomHeight` (20 mm,
  measured-good on the warrior; a spawn-pose auto-calibration was tried and rejected — at `Start`
  the bones hold the bind pose and it produced 61 mm);
- keeps the existing clearance / bias / lift / rise / speed / pelvis logic unchanged, plus a soft
  dead-zone (`CorrectionDeadZone` 4 mm, full at 8 mm) so millimetre disagreements between an authored
  foot and the floor are not "corrected" — on a straight leg an 11 mm shortening swings the knee
  48 mm;
- applies the correction on the transforms: hips += up × pelvis; per foot, target = animated ankle +
  up × Offset (world-absolute, lerped by weight), two-bone analytic solve (`SolveTwoBone`: bone
  lengths preserved, knee kept in the authored bend plane, blended toward the character's forward
  as the leg straightens so a near-straight leg has no noisy bend direction), foot world rotation =
  authored rotation × ground tilt (slerp by 0.8 × weight) — the rollover is the animation's;
- `OnAnimatorIK` releases the Humanoid goals once and returns. `Mode.AnimatorGoals` keeps the
  legacy path (with the `GoalPolicy` options) for comparison.

The field is new, so the Player prefab (unchanged on disk) deserialises the C# default: **production
FootIK behaviour changed without a prefab edit** — deliberately, since the defect is in every
production clip (above). A heel-off toe-pitch branch exists (`HeelOffPitchThreshold` 20 mm,
`MaxToePitch` 12°) but is inert on flat ground for this clip.

## FLAT-GROUND RESULT (Fresh02 Transport, unarmoured, `ik_fresh_bones8`; window frames 60–380)

| | FootIK OFF vs raw | **AnimatedBones vs raw** | AnimatedBones vs Animator output (its own effect) |
|---|---|---|---|
| pelvis | 0.7 / 0.4 mm | **5.6 / 1.9 mm** | 4.0 / 1.7 mm (settle ≤ 4.0) |
| L / R knee | 1.9 / 1.8 mm | 63.7 / 26.9 mm (18.5 / 6.6 rms), 20.8° / 9.0° | 63.6 / 25.1 |
| L / R ankle | 2.7 / 4.4 mm | **15.5 / 8.9 mm** (5.9 / 2.8 rms) | 15.3 / 5.4 |
| L / R toes | 2.4 / 4.4 mm | 15.5 / 8.9 mm | 15.3 / 5.4 |
| chest / head / hand | 0.7 mm | 5.5 mm (the pelvis settle) | 4.0 |
| pelvis rhythm | 69.2 mm | **69.7 mm** | |
| toe bone above ground, L / R min | **−12.4** / +4.2 mm | **+20.0 / +27.9 mm** | |
| foot 1-frame step median / max | 23.5 / 49.6 mm | 23.5 / 49.6 mm | knee step-change max 43.9 = identical to OFF: no new jitter |

Toe local rotation delta 0.00°: the foot's world orientation is the authored one (heel contact →
sole → rollover → toe release untouched). The 64 mm left-knee figure is the geometric response of a
fully extended leg (hip–ankle 732 of 733 mm) to a 15 mm ankle lift at late stance, spread over the
same frames as the animation's own knee motion.

## TOE PENETRATION

The late-stance left-toe penetration (raw −12.4 mm) is gone: minimum toe-bone clearance +20.0 mm
(right +27.9), achieved with ≤ 15 mm of ankle lift and 4 mm of pelvis settle. Ankle / toe drift are
the animation's (steps identical to FootIK off). No authoring change.

## WARRIOR BASE (`ik_wb_bones`, 1.149 m/s, 0.857×)

Pelvis rhythm 70.4 mm, FootIK pelvis settle ≤ 4.2 mm, ankles 15.4 / 6.0, toes 15.4 / 6.0 mm, toes
above ground min +20.2 / +28.1. Transport unchanged (MotionSpeed 0.857 = 1.1489 / 1.34).

## TERRAIN TEST — 4 m up, 4 m down at 6° (`ik_ramp_off` vs `ik_ramp_bones`)

| segment | FootIK OFF: toes above ground L / R | **AnimatedBones** L / R | pelvis settle / knee (bones) | pelvis rhythm |
|---|---|---|---|---|
| uphill 6° | 35…110 / 47…121 mm | 29…105 / 36…113 | ≤ 17 mm / 48 mm | 78 mm (off 68) |
| downhill 6° | **−17**…170 / 8…180 | **+0.9**…164 / 13…172 | ≤ 16 mm / 89 mm | 64 mm (off 69) |
| flat after | 13…139 / 31…147 | 20…136 / 27…147 | ≤ 8 / 63 | 72 |

The planted foot's weight averages 0.52 on the up-ramp with 100 % heel contacts; the toe that
stabbed 17 mm into the down-slope now rests on it. Sensible, small adaptation.

## PRODUCTION CLIP COMPATIBILITY (read-only, real production condition: legacy rating, no override)

| clip | legacy (today) | **AnimatedBones** |
|---|---|---|
| v011 (`Light_Walk8` fwd, locked, 1.30 m/s) | pelvis 36, knees 231 / 202, ankles 415 / 300, toes to −31 mm | pelvis **2.2**, knees 14.5 / 31.7, ankles **8.4 / 3.3**, toes 8.4 / 3.3 mm; toes min +52 / +59 (raw +59 / +60) |
| v008 (`Travel_Walk8` fwd, unlocked, blended 0.67 with `Loco_Jog_Fwd`) | pelvis 169, knees 199 / 187, ankles 219 / 217, toes to −148 mm | pelvis **8.2**, knees 55 / 57, ankles 18.5 / 14.0 mm; toes min +31 / +24 (raw +34 / +26) |

Both improve from catastrophic to terrain-scale. Imported mocap (`Loco_Walk_Fwd`, honest goals,
`ik_mocap_bones`): the retargeted clip hovers (raw right-toe minimum +78 mm above the floor, left
+15); the bone solver lowers the right foot up to 122 mm with a pelvis settle ≤ 28 mm (14.6 rms),
ankles ≤ 45 / 39 mm, left toe minimum +15 mm (legacy pushed it to −15). Different from today's
behaviour (feet pulled toward the source-rig goal), not worse; the mocap strafes deserve a look in
the final runtime QA.

## VISUALS (real Player, real camera, Warrior Base set 0, unarmoured load, 1.30 m/s, 0.970×)

- `Artifacts/AnimationReview/Fresh02_FootIK_OFF_vs_REPAIRED_blind.mp4` — unlabelled side by side,
  same lane / camera / speed / playback / ground. (Identity: LEFT = FootIK OFF, RIGHT = repaired ON.)
- `Fresh02_FootIK_REPAIRED_runtime.mp4` — repaired FootIK alone.
- `Fresh02_FootIK_REPAIRED_ramp6deg.mp4` — the 6° up/down ramp.
- FootIK OFF alone: `Fresh02_Transport_Unarmoured_runtime.mp4` (previous milestone).

Human question: does repaired FootIK improve ground compatibility without changing the approved gait?

## TESTS

`S1_PlayerFootIK_UsesAnimatedBoneSolver_NotAnimatorGoals` — the C# default is the bone solver and
the shipped Player resolves to it (a goal-less clip can never make FootIK target the parked goal).
`S2_FootIK_TwoBoneSolve_ReachesTarget_KeepsLengths_NoOpAtRest` — the solver reaches a 30 mm / 20 mm
correction within 1 mm, keeps both bone lengths to 0.1 mm, keeps the knee on its authored side, and
leaves the pose untouched for a zero correction. Suite, run after every session with production
verified: **34 passed / 0 failed / 0 skipped** (32 existing unchanged + S1 + S2).

## TECHNICAL DEBT (unchanged)

Runtime sword re-seat 125 mm / 4.3° in the hand (grip/attachment; every clip). The heel-off pitch
branch is inert on flat ground. `GuardGait` still null on the production prefab (legacy rating +
warning). The harness's first `ToggleLock` NRE in `PlayerTargeting.SetLocked` (game-side).

## PRODUCTION STATE (serialized)

Controller md5 `186fb872` = backup, Player.prefab `a03bf275` = backup (never written; reimported
only), Fresh02 `a1630664`, `VerifyProduction` CLEAN, harness disarmed, queue cleared.

```
Light_Walk8  Forward = Sword1H_WalkForward_v011  @ ts 1.00
Travel_Walk8 Forward = Sword1H_WalkForward_v008  @ ts 0.50
Player.prefab CombatWalkSpeedScale = 0.65      research clip referenced: NONE      v012: none
```
