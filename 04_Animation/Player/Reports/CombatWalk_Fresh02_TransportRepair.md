# Fresh02 minimum transport repair — research copy, measured on the real Player

`__freshHumanForward_02` untouched (`a1630664`). No FootIK tuning. No v012. No production install.
Motion design untouched: the research copy has **102 / 102 curve bindings key-for-key identical**
(time, value, tangents) to Fresh02; the only serialized difference is `m_LoopBlendPositionY: 1`.

## What was built

| | |
|---|---|
| `Generated/__freshHumanForward_02_Transport.anim` | `Object.Instantiate` of Fresh02 + `loopBlendPositionY = true` (Root Transform Position Y → Bake Into Pose; Based Upon Original kept). Research only. |
| `HumanWalkPerformance.ApplyInPlaceClipSettings` | the generator now sets Y-bake explicitly (loopTime, loopBlend off, **loopBlendPositionY on**, keepOriginalPositionY, heightFromFeet off). Historical clips untouched. |
| `GuardGaitReference` (ScriptableObject, `Assets/Core/Scripts/Gameplay`) | per-heading native travel speeds of the installed guard family: `walk[]`, `jog[]` of `{angleDeg, metresPerSecond}`, sampled with the tree's own wrap-around neighbour interpolation. |
| `CharacterAnimator.GuardGait` (serialized field) + `ResolveGuardWalkSpeed` / `LegacyGuardWalkSpeed` | `GuardExpectedSpeed` rates the walk tier against the reference; unassigned it uses the old `WalkLightRef` table and **warns once** (never silent). `SampleAngular` made public. |
| `Generated/GuardGait_Fresh02.asset` (research) | walk: 0° = **1.34** (Fresh02), other six headings keep the mocap clips' measured 1.80–1.84; jog = the existing table. |
| `RuntimeQaDriver` | in-memory `AnimatorOverrideController` (v011 → research clip, controller file never written), `RTQA_guardGait` assignment, `RTQA_load1` (LoadSpeedMultiplier 1 re-asserted per frame), `RTQA_bypass` (UpperBodyAim + WeaponInertia off), `RTQA_side` (lock-target lateral offset), sword columns. |
| `GameplayCameraPreview` | **bug fixed**: it added `(RootT.y − RootT.y0) × humanScale` on top of what `SampleAnimation` already applies, doubling the vertical rhythm of every RAW review video rendered before 2026-09-02 (Fresh02 showed ~129 mm of bob instead of 69). Measured on the renderer's exact setup: unbaked clip → SampleAnimation moves the root 60.1 mm and the hips read 68.9 mm in holder space; baked clips (v011, c40) → root 0, hips 53 mm inside the pose. Nothing is added now; the render reports the measured hips range. |

The production Player prefab was **not** modified: `GuardGait` is null there, so production still rates
against the legacy table and now logs one warning per CharacterAnimator saying so.

## Y TRANSPORT REPAIR — verified first, FootIK off, aim/inertia bypassed (`t_y0`, legacy rating)

| | Fresh02 (before) | Transport copy (after) |
|---|---|---|
| `RootT.y` curve | 72.5 humanScale = 60.3 mm, identical | identical |
| clip setting | `m_LoopBlendPositionY: 0` | **`1`** |
| `OnAnimatorMove` `deltaPosition.y` per cycle | 59.9 mm (discarded) | **0.0 mm** |
| `Animator.bodyPosition.y` (world) range | 5.3 mm | **60.2 mm** |
| runtime pelvis (mid upper-leg, rel. Player) range | 11.6 mm | **69.1 mm** (authored 69.2) |
| Hips / chest / head range | 11.6 / 11.6 / 11.9 | **69.1 / 69.1 / 68.7** |
| feet L / R range (rel. Player) | 64.0 / 74.9 | 67.5 / 67.5 (raw baked 68.0 / 67.5) |
| HumanPoseHandler bodyPosition.y range | 0.0 | 72.5 (= raw 72.5) |

Same at every stage: Animator output, after `OnAnimatorMove`, IK pass, LateUpdate −100 / +5 / +100,
end of frame — 69.1 mm throughout. Fixed-phase delta vs the RAW copy (12 phases, Animator output):
pelvis 0.9 / 0.5 mm, chest 0.9, head 0.9, clavicle 0.9, knees ≤ 3.3 mm / 0.93°, ankles ≤ 3.1 mm,
toes ≤ 2.9 mm, sword hand 1.3 mm / 0.02°. Y restored cleanly → timing repair proceeded.

## GUARD SPEED RATING REPAIR

| | old | new |
|---|---|---|
| walk-tier rating at 0° | `WalkLightRef` lerp(1.79 @ −9.3°, 1.81 @ 41.7°) = **1.7959 m/s** (captured mocap guard walk) | `GuardGait_Fresh02.walk` = **1.340 m/s** (Fresh02 native) |
| other headings | mocap table | same mocap numbers, per heading — one direction's clip does not re-rate the rest |
| MotionSpeed at 1.1489 m/s | 0.6397 | **0.8574** |
| unassigned | silently the table | the table + one `LogWarning` naming the fix |

## UNARMOURED RUNTIME (`t_load1`: LoadSpeedMultiplier 1.0, target dead ahead, FootIK off, aim/inertia off)

| | measured | expected |
|---|---|---|
| requested / Motor.Velocity / displacement | 1.3000 / 1.3000 / **1.3000 m/s** | 1.30 |
| normalizedTime rate (fit rms 0.00000) | **1.21267 /s** | |
| world cycle | **0.8246 s** | ~0.825 |
| cadence | **145.5 spm** | ~145.5 |
| Fresh02 native playback | **0.9701×** (MotionSpeed 0.97015) | ~0.970 |
| pelvis rhythm | **69.2 mm** | 69.2 |

Window frames 60–380 (the lane ends at recording frame 449 at this speed; the video is trimmed to
440 frames so it stops before the wall).

## WARRIOR BASE RUNTIME (`t_wb`: real load 0.88375, target dead ahead, FootIK off, aim/inertia off)

| | measured | expected |
|---|---|---|
| requested / Motor.Velocity / displacement | 1.1489 / 1.1489 / **1.1489 m/s** | 2.0 × 0.65 × 0.88375 |
| normalizedTime rate (fit rms 0.00001) | **1.07173 /s** | |
| world cycle | **0.9331 s** | |
| cadence | **128.6 spm** | ~128 |
| Fresh02 native playback | **0.8574×** (MotionSpeed 0.85737 = 1.1489 / 1.34) | ~0.857 |
| pelvis rhythm | **68.7 mm** | |

Slower cadence than unarmoured by exactly the load ratio — intended load response, feet stride-matched
(planted-foot speed = 1.34 × 0.8574 = 1.149 m/s = body speed).

## LOCK-ON BLEND EFFECT (`t_lock`: real target 1.2 m left at 12 m = 5.7° off heading, Warrior Base)

| | pure forward | real lock-on |
|---|---|---|
| `Horizontal / Forward` | 0 / 1 | 0.0995 / 0.9950 |
| tree children | Fresh02 1.000 | Fresh02 0.9338 + `Sword1h_Strafe45RightLoop` 0.0662 (1.000 s) |
| gait rating at heading | 1.340 (0°) | **1.371** (5.7°, interpolating toward the strafe entry 1.81 @ 86.3°) → MotionSpeed 0.8371 (−2.3 %) |
| tree length | 0.800 s | 0.8132 s (**+1.7 %**, length averaging) |
| state length | 0.9331 s | 0.9715 s |
| normalizedTime rate | 1.0717 /s | 1.0293 /s |
| Fresh02 playback | 0.8574× | **0.8235×** (−4.0 % total) |
| cadence | 128.6 | 123.5 spm |
| pelvis rhythm | 68.7 mm | 63.2 mm (the strafe clip's vertical dilutes it) |
| legs vs RAW | ≤ 4 mm | knees 17–19 mm rms, ankles 26–31 mm rms (the 6.6 % strafe pose) |

Both halves are expected behaviour of the existing tree: a target off the heading plays a small strafe
component, whose clip is longer and rated faster. Not redesigned here; reported.

## OTHER RUNTIME WRITERS — read-only (`t_writers`: UpperBodyAim + WeaponInertia on, FootIK still off)

Timing and pelvis identical to `t_wb` (1.07173 /s, 68.7 mm). Final-pose deltas vs RAW at 12 phases:
head 1.3 mm, clavicle 1.1 mm, chest 0.6 mm / 0.07°, hand 0.7 mm / 0.53° (WeaponInertia lag).
With the target dead ahead the aim tuck contributes nothing measurable.

## POSE PRESERVATION — untouched Fresh02 vs Transport copy, 480 identical phases (Edit Mode)

| bone | max mm | rms mm | max rot |
|---|---|---|---|
| pelvis / Hips / chest / head / clavicle / knees / ankles / L toes / sword hand | **0.00** | 0.00 | 0.00° |
| R toes | 0.49 | 0.02 | 0.00° |
| sword position / blade direction | 0.00 mm | | 0.28° |

(comparing the copy's root-local pose against Fresh02's pose + its root Y: the same world motion, just
carried by the pose instead of the root.)

**Runtime sword note:** every session shows a constant 125 mm / 4.3° sword displacement vs the
Edit-Mode prefab, at every phase, already present at the Animator-output stage, identical with
WeaponInertia on or off and on every clip. Cause: the sword's local transform in the hand is re-seated
at runtime — hand-local (−0.1677, 0.0597, −0.0568) vs prefab (−0.0432, 0.0630, −0.0445), 124.5 mm
along hand-local x. A grip/attachment matter, not transport; not touched.

## VISUALS (real Player, real camera, Warrior Base set 0, FootIK off, aim/inertia off)

- `Artifacts/AnimationReview/Fresh02_RAW_reference_0970x.mp4` — Edit-Mode reference, Fresh02 at 0.970× /
  1.30 m/s, renderer fixed (measured hips range 69.1 mm in the render).
- `Fresh02_Transport_Unarmoured_runtime.mp4` — Play Mode, load 1.0, 1.30 m/s, 145.5 spm.
- `Fresh02_Transport_WarriorBase_runtime.mp4` — Play Mode, load 0.884, 1.149 m/s, 128.6 spm (slower
  cadence is the load response).

## TESTS

`R1_HumanWalkPerformanceClip_BakesRootYIntoPose` — a clip through the generator's settings pass must
serialize Y baked, based upon original, with its curves untouched.
`R2_GuardWalkRating_UsesSerializedGaitReference_NeverSilentlyTheMocapTable` — forward rated at the
installed clip's 1.34, other headings keep their own numbers, and the null-reference fallback must
warn (LogAssert) and equal the legacy value.
Suite result, run after all Play sessions with production verified: **32 passed / 0 failed / 0 skipped**
(30 existing, unchanged, + R1 + R2).

## PRODUCTION STATE (serialized)

Controller md5 `186fb872` = pre-milestone backup (never written this milestone — the preview was an
in-memory override), Player.prefab `a03bf275` = backup, Fresh02 `a1630664`; neither the Transport copy's
guid nor `GuardGait` appears in the controller or the prefab; `VerifyProduction` CLEAN;
`RTQA_armed = 0`, all forensic flags cleared.

```
Light_Walk8  Forward = Sword1H_WalkForward_v011  @ ts 1.00
Travel_Walk8 Forward = Sword1H_WalkForward_v008  @ ts 0.50
Player.prefab CombatWalkSpeedScale = 0.65      research clip referenced: NONE      v012: none
```

## What the next milestone inherits

- FootIK on goal-less clips (stance foot hoisted 100–170 mm) — untouched, still the largest deviation.
- Production still rates the guard walk against the mocap table (and warns) until a `GuardGaitReference`
  is assigned on the Player and the installed forward clip's speed is known.
- The RAW review videos before this date carried a doubled vertical rhythm; Fresh02's ~69 mm was measured
  numerically and is unaffected, but earlier visual approvals were made on exaggerated bob.
