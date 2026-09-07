# Fresh02 runtime character QA — real Play Mode, real Player, real camera

`__freshHumanForward_02` unchanged (`a1630664`). No curves edited. No v012. Production restored after
the preview (verification at the end).

## RUNTIME ORDER — from the compiled types and MonoImporter overrides, not names

```
Animator evaluate  (applyRootMotion=true; KCC consumes root motion in OnAnimatorMove)
  IK pass          FootIK (Weight 1, solves once on the first callback, re-imposes on later layers)
                   OffHandPose (LEFT arm, Weight 0 - inert)
LateUpdate  -10    AccelerationLean            spine / chest lean
LateUpdate    0    UpperBodyAim                spine, chest, upper chest, head, right upper arm tuck, elbow guard
LateUpdate    0    TwistBoneDriver             twist bones
LateUpdate    0    HandGripPose                fingers
LateUpdate   60    WeaponInertia               weapon transform + arm drag   <- what the camera renders
LateUpdate  600    WeaponTrail (VFX)      1000 BodyPartHurtbox x14 (colliders follow bones)
```
HitStop, DeathHandler, FootstepPlayer, LockOnReticle also have LateUpdate at 0; none writes
locomotion pose. `AccelerationLean`'s -10 is a LateUpdate order: it runs AFTER the Animator and
the IK pass, not before.

The harness captured the pose at three points bracketing these: A (LateUpdate -100, after
Animator + IK pass), B (+5, after aim/twist/grip), C (+100, after WeaponInertia). "Raw Animator
before FootIK" came from a second session with `FootIK.Weight = 0` in memory only.

## HOW THE PLAYER ACTUALLY RUNS THIS CLIP

- Combat-stance ground speed is **1.18 m/s**, not 1.30: `MaxStableMoveSpeed 2 x CombatWalkSpeedScale
  0.65 x LoadSpeedMultiplier 0.88` (armour load). 1.30 is the unarmoured figure.
- The guard walk is rated against a **hard-coded `WalkLightRef` of 1.84 m/s**, so the Animator plays
  Light_Walk8 at `MotionSpeed = 1.18 / 1.84 = 0.656` (clamp floor 0.6). Fresh02's native speed is
  1.34 m/s, so at 0.656x its feet track at **0.88 m/s under a 1.18 m/s body: ~0.30 m/s of skate
  is baked in by the rating constant**, independent of the clip's own quality. Measured planted-foot
  drift at runtime: 263 mm/s (FootIK on), 323 mm/s (FootIK off).
- With lock-on active, `__freshHumanForward_02` carries 0.93 of the layer; a 0.07
  `Sword1h_Strafe45RightLoop` component comes from the lock target sitting 5.7 deg off the heading.

## RAW FOOT STATE — authoritative, one method (per-bone floors measured on the live capsule frame)

The earlier discrepancy (5.2 vs 32.3 mm right penetration) came from two different floor
references and two different window definitions in two scans. Authoritative, edit-mode, per-bone
floors from the approved planted pose with hysteresis-merged windows: **LEFT 58.6 mm, RIGHT 5.2 mm**
(the 32.3 figure used the raw product-rule windows, which include heel-off frames where the ankle
floor is the wrong reference; it is retired).

At runtime the floors differ (ankle 59/68 mm, toes 49/47 mm above ground on the capsule frame) and
so does the number - see FootIK below.

## FOOT IK EFFECT (per-session; frame-matching across sessions is unreliable because the two
## runs' playback rates differ by ~1.5 %)

| | FootIK OFF (raw Animator) | FootIK ON (shipped) |
|---|---|---|
| L penetration | 70.8 mm | **61.5 mm** |
| R penetration | 107.1 mm | **78.2 mm** |
| planted-foot drift | 323 mm/s | **263 mm/s** |
| foot local pitch range (rollover) | 10.7 / 11.1 deg | 10.7 / 11.1 deg - **unchanged** |
| pelvis Y excursion | 9 mm | 11 mm |

FootIK reduces penetration by 9-29 mm and drift by ~60 mm/s, and it does **not** flatten the
foot: heel-contact / sole / roll / toe-release survive with identical pitch range. It does NOT
remove the penetration - and the reason is the next section, not FootIK.

## PELVIS / WEIGHT — DEFECT FOUND, ROOT CAUSE PROVEN

Fresh02 authors a **69 mm** pelvis rhythm. In the raw Animator pose at runtime the Hips bone
moves **9 mm**. The weight cycle the human review selected never reaches the screen.

Cause: the clip ships with `loopBlendPositionY` (Root Transform Position Y -> **Bake Into Pose**)
OFF, so the authored vertical becomes root-motion Y, which `BaseCharacterController` /
`OnAnimatorMove` discards because the KCC owns the transform. Proven on an in-memory copy of
Fresh02: `loopBlendPositionY=false -> Hips range 12 mm; true -> 69 mm`. (`keepOriginalPositionY`
is "Based Upon: Original" and has no effect - I had the two confused earlier.)

This is also why runtime penetration (61-107 mm) exceeds the authored 58.6: the legs were solved
for a body that rises 69 mm over mid-stance; with the rise gone, the stance leg drives the foot
through the floor by roughly the missing height, and FootIK lifts it only part-way back.

~~Every clip this pipeline has generated shares the setting, so v011 and v008 have shipped without
their vertical rhythm too.~~ **ERRATUM (transport forensics, 2026-09-02):** wrong. Serialized
`Sword1H_WalkForward_v008.anim` and `_v011.anim` both carry `m_LoopBlendPositionY: 1`
(`HumanoidWalkGenerator.cs` bakes Y); only Fresh02 (`HumanWalkPerformance.cs`) ships it unbaked.
See `CombatWalk_Fresh02_Transport.md`. The fix is one clip setting - **not applied this milestone**.

## POSE DELTA — Animator+IK (A) -> final Player (C), full cycle, max

| bone | position | local rotation |
|---|---|---|
| pelvis / hips / both feet / toes | 0.0 mm | 0.0 deg |
| chest | 0.2 mm | 0.1 deg |
| head | 2.2 mm | 0.0 deg |
| right clavicle | **1.7 mm / 0.0 deg** |
| right upper arm | 2.0 mm / 0.0 deg |
| right forearm | 0.5 mm / 0.0 deg |
| right hand / sword | 1.2 mm / **1.1 deg** (WeaponInertia, B -> C) |

## SHOULDER / ARM — survives

Nothing at runtime rotates the clavicle (0.0 deg). `UpperBodyAim`'s arm tuck contributed 2.0 mm at
the upper arm with the target 5.7 deg off heading. Fresh02's shoulder authority (0.5 deg deviation,
Down-Up +0.081..+0.117) reaches the screen intact.

## SWORD — survives

Path 240 mm/cycle at runtime vs 261 authored (rate-scaled), plus 1.1 deg of WeaponInertia lag.
No pumping, no jitter (hand 1-frame step 2.7 mm), no anatomy change. Controlled inertia intact.

## HARNESS NOTES (so the next run is not a day)

`test_general` spawns the Player 0.6 m from a `Backboard` collider on +Z - three sessions measured
zero velocity for that reason. The lock cone is measured from the CAMERA forward and the orbit
only yaws on look input; LOS from the camera passed through the player's own capsule when the
target was dead ahead and behind the Backboard when parked on camera forward - the driver now
searches the cone for a parking spot with a clear LOS-mask Linecast, locks, then walks. The first
`ToggleLock` at frame 20 throws an NRE inside `PlayerTargeting.SetLocked` (a game-side
init-order issue; the retry locks cleanly). An unfocused editor stalls Play Mode. Console reads are
blind through the bridge in Play Mode; the driver writes its own per-second dump.

## PRODUCTION SAFETY
(filled in after the byte-restore - see end of file)

Serialized, after the byte-exact restore (controller md5 `186fb872`, prefab md5 `a03bf275`, both equal
to the pre-install backups; reimported; `VerifyProduction` CLEAN):

```
Light_Walk8  Forward = Sword1H_WalkForward_v011  @ ts 1.00
Travel_Walk8 Forward = Sword1H_WalkForward_v008  @ ts 0.50
Player.prefab CombatWalkSpeedScale = 0.65
research clip referenced: NONE        v012: none        Fresh02: a1630664 (untouched)
```

## SECOND SAMPLE (the +Z video run, runtime ON) — reproduces the -X data

travel 1.145 m/s, MotionSpeed 0.639, pelvis 10 mm, clavicle 1.7 mm / 0.0 deg, head 2.1 mm,
hand 0.7 mm / 0.4 deg. Same picture on a different lane.

## TECHNICAL QA (raw asset, unchanged)

Interpolation violations 0, seam C1, temporal audit unchanged, shoulder unchanged in the asset.
EditMode suite **30 passed / 0 failed / 0 skipped**; nothing weakened.

## VIDEOS

`Fresh02_RAW_at_runtime_rate.mp4` - Edit-Mode shoulder rig, Fresh02 at the runtime's own rate
(0.656x) and speed (1.178 m/s), with the authored vertical present.
`Fresh02_RUNTIME_main.mp4` - real Play Mode, real Player, real Cinemachine camera and HUD, all
runtime systems on, +Z lane.
`Fresh02_RAW_vs_RUNTIME_blind.mp4` - unlabelled side-by-side. The difference to look for is the
weight: LEFT carries the 69 mm rhythm, RIGHT does not. (Identity: LEFT = RAW, RIGHT = RUNTIME.)

## RESULT

`FRESH02 RUNTIME QA FOUND COMPATIBILITY DEFECT — Animator root-motion Y (clip loopBlendPositionY off) discarded by BaseCharacterController/OnAnimatorMove; CharacterAnimator.WalkLightRef 1.84 m/s rating constant`
