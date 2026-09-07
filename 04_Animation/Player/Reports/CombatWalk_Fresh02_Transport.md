# Fresh02 runtime motion-transport forensics — RAW → RUNTIME, measured

`__freshHumanForward_02` unchanged (`a1630664`). No v012. No FootIK / UpperBodyAim / WeaponInertia
tuning. Production byte-restored after the sessions (verification at the end).

Three real Play Mode sessions on the real Player (debug set 0 "Warrior Base", baked duplicates
hidden), Fresh02 temporarily in `Light_Walk8` child 0 (timeScale 1, cycleOffset 0.92 inherited from
the v011 slot), lock-on active, 60 fps capture, 600 recorded frames, steady window frames 60–460:

| session | 2D tree | FootIK | purpose |
|---|---|---|---|
| `asis` | real (target 5.7° off heading → 6.6 % `Sword1h_Strafe45RightLoop`) | on | the observed gameplay condition |
| `pure` | pinned to forward child after `CharacterAnimator.Update` | on | Fresh02 alone, all runtime writers |
| `pure_noik` | pinned | component disabled in memory | Animator output with no pose modifier |

RAW reference: Fresh02 sampled in Edit Mode at 480 phases on a Player instance at the origin
(root-local bone positions + `RootT.y` curve + HumanPoseHandler body position).
Per-frame runtime record: Animator state time/length/speed, every parameter, clip weights, KCC
velocity and every speed multiplier, `deltaPosition` / `rootPosition` / `bodyPosition` in
`OnAnimatorMove`, `bodyPosition` and IK goals in the IK pass before (order −2000) and after (+2000)
FootIK, bones at LateUpdate −100 / +5 / +100 and at end of frame.

## ACTUAL RUNTIME PLAYBACK (measured `normalizedTime`, not parameter names)

| | asis | pure |
|---|---|---|
| normalizedTime rate (linear fit, rms 0.00000) | **0.78587 /s** | 0.79886 /s |
| state length (`AnimatorStateInfo.length`, speed-adjusted) | 1.27249 s | 1.25178 s |
| runtime cycle | **1.2725 s** | 1.2518 s |
| cadence | **94.3 spm** | 95.9 spm |
| Fresh02 playback vs native (0.800 s) | **0.629×** | 0.639× |
| authority | 0.825 s cycle, 145.5 spm, 0.970× | |

Fresh02 child phase at runtime = `frac(normalizedTime + 0.9208)` (the child's cycleOffset 0.92) —
established by a global fit of the horizontal leg pose, residual 0.9 mm rms / 2.1 mm max in
`pure_noik`. Transitions: 0 frames in the window. `Animator.speed` 1.000 throughout.

## ACTUAL WORLD SPEED

| | value |
|---|---|
| requested: `MaxStableMoveSpeed 2.0 × CombatStanceSpeedMultiplier 0.650 × LoadSpeedMultiplier 0.88375 × StatusSpeedMultiplier 1 × CombatMoveScale 1` | 1.1489 m/s |
| directional tax `lerp(StrafeSpeedMultiplier 0.8, 1, forwardness 0.99504)` | × 0.99901 → **1.1477** |
| `Motor.Velocity` mean (frames 60–460) | 1.1477 m/s |
| world displacement (linear fit of Player z; x 0.0000) | **1.1477 m/s** |
| `Speed` parameter (smoothed) | 1.1477 |

Steady state, not acceleration: velocity is flat from frame 60 until frame 538, when the character
reaches the end of the lane (z ≈ 3.5) and stops. Not a projection or measurement error: KCC velocity
and transform displacement agree to four decimals.

**Why 1.145 and not 1.30:** 1.30 = 2.0 × 0.65 is the *unarmoured* figure. `EquipmentManager.
RecalculateLoad` reports `TotalWeight 15.5 / fullLoadWeight 24 → ArmorLoad01 0.6458 →
LoadSpeedMultiplier = lerp(1, 0.82, 0.6458) = 0.88375`. With Warrior Base equipped the design speed is
1.1489; the remaining 0.1 % is the strafe tax from the lock target sitting 5.7° off the heading.

## TIME MULTIPLIER CHAIN — every contributor, measured

| contributor | value | writer / timing | movement | animation |
|---|---|---|---|---|
| clip native duration | 0.800 s (`m_StopTime`), 1.34 m/s native travel | asset | – | ✓ |
| `Light_Walk8` child 0 `timeScale` | 1.00 | controller (serialized) | – | ✓ |
| child `cycleOffset` | 0.92 | controller | – | phase only |
| blend-tree length averaging | asis: 0.9338×0.800 + 0.0662×1.000 = **0.8132 s** (+1.7 %); pure: 0.800 | Animator, per frame from `Horizontal/Forward` (lock target offset) | – | ✓ |
| `Gait_Light` (Speed 1.148 < threshold 1.85) | 100 % `Light_Walk8`, timeScale 1 | Animator | – | ✓ |
| `Stance_Blend` (Stance 1.000) | 100 % guard tree, timeScale 1 | `CharacterAnimator.Update` → `Stance` | – | ✓ |
| Locomotion state `m_Speed` | 1 | controller | – | ✓ |
| state speed parameter **`MotionSpeed`** | **0.6391** | `CharacterAnimator.Update` (Update phase, every frame): `clamp(Speed / expectedSpeed, 0.6, 1.5)`, `expectedSpeed = lerp(Travel, Guard, Stance)`, `GuardExpectedSpeed = SampleAngular(WalkLightRef)` = lerp(1.79 @ −9.3°, 1.81 @ 41.7°) at blendAngle 5.7° = **1.7959** → 1.1477 / 1.7959 = 0.6391 exactly | – | ✓ |
| `Animator.speed` | 1.000 | `HitStop.LateUpdate` writes 0/1 only during a hit freeze | – | ✓ |
| `CombatStanceSpeedMultiplier` | 0.650 | `CharacterAnimator.Update` = lerp(1, `CombatWalkSpeedScale` 0.65, Stance) | ✓ | indirectly (via Speed → MotionSpeed) |
| `LoadSpeedMultiplier` | 0.88375 | `EquipmentManager.RecalculateLoad` on equip | ✓ | indirectly |
| `StatusSpeedMultiplier`, `CombatMoveScale` | 1, 1 | – | ✓ | indirectly |
| directional tax | 0.99901 | `BaseCharacterController.UpdateVelocity` | ✓ | indirectly |
| transitions | none | | | |

**Effective product:** rate = `Animator.speed 1 × stateSpeed 1 × MotionSpeed 0.6391 / treeLength 0.8132`
= 0.7859 /s — measured 0.78587 /s. Cycle 1.2725 s, 94.3 spm, playback 0.629× native.

Two things set the timing, both upstream of the clip: (1) the guard set is rated at 1.796 m/s
(`WalkLightRef`, the captured mocap guard walk) while Fresh02 travels 1.34 m/s natively — the
stride matcher therefore slows Fresh02 to 0.746 of the correct rate; (2) the body moves at 1.148
m/s, not 1.30, because of armour load. At the correct rating the loaded body would play Fresh02 at
0.857× (0.934 s, 128 spm); at 1.30 m/s it would play at 0.970× — the authority.

## RAW ROOT / PELVIS AUTHORITY — where the 69 mm lives

| component (RAW, Edit-Mode sample, root-local) | range |
|---|---|
| `RootT.y` curve (humanScale units, 49 keys) | 0.8962 … 0.9687 = 72.5 → × humanScale 0.8312 = **60.3 mm metric** |
| pelvis (mid upper-leg) inside the pose, relative to root | **11.7 mm** |
| Hips bone relative to root (Hips → Root bone; = pelvis mid within 0.003 mm) | 11.7 mm |
| chest / head relative to root | 11.7 / 11.9 mm |
| authored pelvis in world = pose + root Y | **69.2 mm** |

Profile of the authored root Y: a +54 mm plateau over phases 0.17–0.42 and 0.67–0.92 (single
support), low at 0.0–0.08 and 0.5–0.58 (double support). So ~87 % of the authored vertical rhythm is
in `RootT.y` (the mass-weighted body frame), ~13 % inside the pose. Clip settings, serialized:
`m_LoopBlendPositionY: 0` (Root Transform Position Y **not** baked into pose), `m_KeepOriginalPositionY: 1`
(Based Upon Original), `m_LoopBlend: 0`, `m_HeightFromFeet: 0`. Writer: `HumanWalkPerformance.cs:360`
sets `loopTime`, `loopBlend=false`, `keepOriginalPositionY=true` and nothing else.
`HumanoidWalkGenerator.cs:3275` sets `loopBlendPositionY = true` — v008 and v011 are serialized
with `m_LoopBlendPositionY: 1` (their 64 / 60 mm `RootT.y` IS in the pose at runtime). The earlier
claim that "every clip shares the setting" was wrong and is corrected in the RuntimeQA report.

Player Animator: `m_ApplyRootMotion: 1`, update mode Normal, `IK Pass` on layer 0.
`CharacterAnimator.OnAnimatorMove` forwards only `deltaRotation` yaw (`AddTurnRootMotion`); no
position delta is ever applied — the KinematicCharacterMotor owns the transform. No script resets
the root; nothing writes the Animator transform.

## STAGE-BY-STAGE Y TRACE (pure = FootIK on; noik = FootIK component disabled)

| stage | root transform Y | `Animator.bodyPosition.y` (world) | `deltaPosition.y` | pelvis rel. root | feet rel. root (L / R) | chest / head rel. root |
|---|---|---|---|---|---|---|
| A RAW clip | authored root Y **60.3 mm** | body frame 60.3 mm | – | 11.7 mm | 64.0 / 75.4 mm | 11.7 / 11.9 |
| B Animator output (`OnAnimatorMove`, before IK) | – | **5.3 mm** (noik 0.0) | integrates to **59.9 mm per cycle** (asis 56.1, blend-diluted) — the authored root Y arrives here as root motion | | | |
| C after `OnAnimatorMove` (KCC) | **0.0 mm** (Player.y flat; Animator localY 0.0) | 5.3 mm | dropped | | | |
| D IK pass before FootIK / after FootIK | 0.0 | 5.3 → 5.3 (FootIK `_pelvisApplied` mean −1.1 mm, range 5.3) | | | | |
| E LateUpdate −100 (A) | 0.0 | 5.3 | | **13.2 mm** (noik 11.6) | 156.5 / 137.7 (noik **64.0 / 74.9**) | 13.2 / 12.8 (noik 11.6 / 11.9) |
| E LateUpdate +5 (after AccelerationLean / UpperBodyAim / Twist / Grip) | 0.0 | | | 13.2 | same | same |
| E LateUpdate +100 (after WeaponInertia) | 0.0 | | | 13.2 | same | same |
| F end of frame (rendered) | 0.0 | | | 13.2 | same | same |
| `Animator.rootPosition.y` | 28.5 mm range | | | | | |

`HumanPoseHandler.bodyPosition.y` at LateUpdate: RAW 0.8962…0.9687 (72.5 range); runtime noik
0.8223 constant (0.0 range); runtime FootIK-on 31.9 mm range (the IK-pulled legs move the mass frame).

The pelvis loses 69.2 − 11.7 ≈ 57 mm at exactly one hand-off: **Animator evaluation → `OnAnimatorMove`**.
Everything after that (FootIK, LateUpdate writers, rendering) leaves the pelvis Y within 1.6 mm of the
Animator output.

## FIXED-PHASE POSE DELTA — 12 phases, runtime vs RAW at the same Fresh02 phase (root-local, pose only)

`pure_noik` = Animator output alone (A) and final (C):

| bone | A max / rms mm | A rot max / rms ° | C max / rms mm | C rot ° |
|---|---|---|---|---|
| pelvis (mid upper-leg) | 0.5 / 0.4 | – | 0.5 / 0.4 | – |
| Hips | 0.6 / 0.4 | 0.06 / 0.03 | 0.6 / 0.4 | 0.06 |
| chest | 0.6 / 0.4 | 0.03 / 0.02 | 0.6 / 0.4 | 0.05 |
| head | 0.6 / 0.4 | 0.01 | 1.1 / 0.6 | 0.02 |
| right clavicle | 0.6 / 0.4 | 0.01 | 0.9 / 0.5 | 0.01 |
| knees L / R | 2.3 / 3.4 | 0.56 / 0.93 | same | same |
| ankles L / R | 2.2 / 3.2 | 0.19 / 0.18 | same | same |
| toes L / R | 2.4 / 3.1 | 0.01 | same | same |
| sword hand | 1.0 / 0.5 | 0.02 | 0.8 / 0.6 | 0.42 (WeaponInertia) |

**The Animator reproduces Fresh02's pose to ≤ 3.4 mm / ≤ 0.9° at every phase.** Only the root-Y
component is missing.

With the runtime modifiers on, separately identified (`pure` FootIK on; `asis` adds UpperBodyAim's
target offset):

| bone | FootIK on: A max / rms mm (rot) | asis (FootIK + aim): A max / rms mm (rot) |
|---|---|---|
| pelvis | 4.1 / 1.8 | 6.8 / 4.6 (Hips rot 2.9°) |
| chest / head / clavicle | 4.1 / 1.8 (≤ 0.05°) | 6.5 / 4.2 / 4.3 (1.3° / 2.4° / 0.88°) |
| knees L / R | **338 / 393 mm (47° / 67°)** | 341 / 384 (48° / 69°) |
| ankles L / R | **391 / 388 mm** | 386 / 372 |
| toes L / R | **403 / 383 mm** | 400 / 371 |
| sword hand | 4.1 / 1.9 | 21.5 / 17.6 (5.9°, UpperBodyAim tuck at 5.7° target offset) |

FootIK at fixed phases, root-local mm (raw → FootIK on), goal weight in brackets:

```
phase  LFoot raw→IK   LKnee raw→IK    RFoot raw→IK   RKnee raw→IK
0.167   30 → 82  (0.61)  396 → 445      75 → 67        397 → 391
0.250   24 → 126 (0.71)  389 → 486      83 → 81        380 → 378
0.333   20 → 165 (0.74)  376 → 523      72 → 72        413 → 413
0.417   22 → 194 (0.77)  369 → 546      58 → 56        394 → 395
0.750   80 → 77          375 → 371      25 → 113 (0.63) 376 → 466
0.917   51 → 52          415 → 415      26 → 184 (0.76) 370 → 529
```

That is the **stance** foot (the low, extended leg in the raw clip) being hoisted 100–170 mm with the
knee raised 150–160 mm while its goal weight is 0.6–0.77 — visible in the earlier runtime video
(frame 70: left leg lifted to knee height mid-stance). Cause, as observed: Fresh02 (like v008 and
v011) has **no IK-goal curves** (`LeftFootT/Q`, `RightFootT/Q` absent), so the IK goals the Animator
hands FootIK are not the feet — `GetIKPosition` before FootIK's write varies 705 mm in Y over the
window — and FootIK's goal repair reconstructs goals that sit far above the animated foot. This is a
FootIK-on-goal-less-clip interaction, not a Fresh02 authoring defect; it is reported, not tuned.
Toe clearance vs raycast ground: noik L −12.4 / R +4.2 mm minimum; FootIK on L −1.7 / R +30.1 mm
minimum but L +175 / R +170 mm maximum (raw maximum 118 / 130).

## FIRST DIVERGENCE

**Timing:** the Locomotion state's speed parameter, written each frame by `CharacterAnimator.Update`
as `MotionSpeed = Speed / GuardExpectedSpeed(WalkLightRef)`. Everything downstream (state, tree,
Animator.speed) is 1.0. First and only writer.

**Pelvis:** the Animator's root-motion extraction at evaluation, dictated by the clip's
`m_LoopBlendPositionY: 0`, followed by `CharacterAnimator.OnAnimatorMove` discarding
`deltaPosition`. Pose-internal pelvis motion (11.7 mm) survives every later stage within 1.6 mm.

## ROOT CAUSE

`Fresh02 runtime timing differs because CharacterAnimator.Update writes the Locomotion state speed
parameter MotionSpeed = Speed / SampleAngular(WalkLightRef) = 1.1477 / 1.7959 = 0.639 — the guard walk
is rated against the hard-coded 1.79–1.84 m/s mocap table while Fresh02 travels 1.34 m/s natively, and
the body travels at 1.148 m/s (armour load 0.884, not 1.30) — giving 0.629× playback (with the 6.6 %
strafe child stretching the tree length 1.7 %), a 1.27 s cycle at 94 spm instead of 0.825 s at 145.5.`

`Fresh02 pelvis rhythm collapses because HumanWalkPerformance writes the clip with Root Transform
Position (Y) not baked into pose (m_LoopBlendPositionY: 0, unlike v008/v011), so the Animator extracts
the 60.3 mm RootT.y rhythm as root-motion deltaPosition.y (measured 59.9 mm per cycle) which
CharacterAnimator.OnAnimatorMove discards (only yaw is forwarded; the KCC owns the transform),
leaving only the 11.7 mm pelvis motion inside the pose. FootIK and the LateUpdate writers do not
touch it (≤ 1.6 mm).`

## RECOMMENDED REPAIR (smallest; not applied — Fresh02 stays byte-identical)

1. **Pelvis (configuration-only, one clip setting):** `loopBlendPositionY = true` (Root Transform
   Position Y → Bake Into Pose, Based Upon Original), applied to the next clip version / a copy, and
   `s.loopBlendPositionY = true` at `HumanWalkPerformance.cs:360` so future clips match
   `HumanoidWalkGenerator`. No curve changes. Proven in the previous milestone on an in-memory copy
   (Hips 12 → 69 mm). Not yet measured: FootIK's behaviour once the body rises (its pelvis drop and
   goal repair will see different foot heights).
2. **Timing (one rating constant):** rate the guard walk against the installed clip's real travel
   speed instead of the mocap table — a serialized `GuardWalkRefSpeed` (1.34 for Fresh02) used by
   `GuardExpectedSpeed`, or `WalkLightRef` values regenerated from the installed clips. With 1.34 the
   loaded body plays Fresh02 at 0.857× (0.934 s, 128 spm); an unarmoured body at 0.970× (145.5 spm),
   which is the authority. Whether 1.30 m/s must hold *with* Warrior Base is a design call on
   `LoadSpeedMultiplier`, not an animation defect.
3. Separate, out of scope here: FootIK on goal-less clips (above) is the largest pose deviation in
   the chain (390 mm at the feet) and applies to the production clips too; it deserves its own
   milestone before Fresh02's planting is judged on the Player.

## EQUIPMENT

All sessions: renderers `Sword, Warrior_Head, EQ_Torso_Warrior_Torso, EQ_Legs_Warrior_Legs` — debug set
0 "Warrior Base", baked duplicates hidden. Not investigated further.

## TESTS

EditMode `HumanoidIdleRegressionTests`, run after the restore: **30 passed / 0 failed / 0 skipped**
(20.0 s). No test changed.

## PRODUCTION STATE (serialized, after the byte restore)

Controller md5 `186fb872` = pre-milestone backup; Player.prefab `a03bf275` = backup (never touched);
Fresh02 `a1630664` unchanged; reimported; `AnimationAssetSafety.VerifyProduction` → CLEAN.

```
Light_Walk8  Forward = Sword1H_WalkForward_v011  @ ts 1.00
Travel_Walk8 Forward = Sword1H_WalkForward_v008  @ ts 0.50
Player.prefab CombatWalkSpeedScale = 0.65
research clip referenced: NONE      v012: none      harness: RTQA_armed=0, forensic flags 0
```

## HARNESS NOTES

`RuntimeQaDriver` gained a forensic mode (`RTQA_forensic`, `RTQA_pureFwd`, `RTQA_footikOff`,
`RTQA_shots`): `ForensicMove` (OnAnimatorMove), `ForensicIkEarly/Late` (IK pass at order ∓2000),
`ForceForward` (Update order 5000), end-of-frame capture, `<tag>_transport.csv`. Analysis:
`scratchpad/transport/analyze.py <tag> [phaseOffset]`. Raw table: `raw_fresh02.csv`.
Edit-Mode `SampleAnimation` on the Player DOES move the root by `RootT` (x and y) — measure bones
root-locally and add the root's own Y for the authored world height.
