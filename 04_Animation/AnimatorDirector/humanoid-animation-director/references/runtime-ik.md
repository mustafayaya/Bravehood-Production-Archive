# Runtime IK on the Player — what actually runs, read from the code

Audited read-only from `Assets/Core/Prefabs/Gameplay/Player.prefab` and the scripts it carries.
Nothing here was modified. Re-verify before relying on it: these are components on a live prefab.

## Correction to the assumed architecture

The working assumption has been "Body IK, Foot IK, Right-Hand IK". Two of those three are not what
they sound like:

- There is **no right-hand IK component.** Nothing IK-solves the sword hand at runtime.
- **`OffHandPose` is the LEFT arm**, and it ships with `Weight = 0` — inert unless something raises
  it. (Disable it by weight, never by `.enabled`.)
- What behaves like "Body IK" is **`UpperBodyAim`**, a LateUpdate spine/head rotator with two
  right-arm addenda.

## Execution order

```
DefaultExecutionOrder(-10)   AccelerationLean        LateUpdate   spine/chest lean
                             Animator evaluates the clip
        Animator IK pass     FootIK                  OnAnimatorIK feet + Animator.bodyPosition
        Animator IK pass     OffHandPose             OnAnimatorIK left arm   (Weight 0)
DefaultExecutionOrder(0)     UpperBodyAim            LateUpdate   spine, chest, upper chest, head,
                                                                  right upper arm, right elbow
DefaultExecutionOrder(0)     TwistBoneDriver         LateUpdate   twist bones
DefaultExecutionOrder(0)     HandGripPose            LateUpdate   fingers
DefaultExecutionOrder(60)    WeaponInertia           LateUpdate   weapon transform + arm drag
```

The three order-0 LateUpdate writers have undefined relative order, which is currently harmless
because their bone sets are disjoint (aim chain / twist bones / fingers). It stops being harmless
the moment any of them is extended.

## Per-system detail

### FootIK — `Assets/Core/Scripts/Gameplay/FootIK.cs`

| | |
|---|---|
| stage | `OnAnimatorIK`, solves on the frame's FIRST callback, re-imposes the cached result on later ones |
| serialized | `Weight 1`, `PositionWeight 1`, `RotationWeight 0.8`, `WeightWhileAttacking 0.8` |
| chain | left/right foot goals |
| **moves the body** | **yes** — writes `Animator.bodyPosition` directly, pelvis drop up to `MaxPelvisDrop 0.45 m`, smoothed (`PelvisSmooth 0.12`) |
| clavicle/shoulder | no |
| when off | dead, rolling (unless `GroundFeetWhileRolling`), airborne, no tier |

The re-impose-on-later-callbacks design exists because any layer after the solved one would stomp
the corrected legs — `HeavyLower` under armour load was doing exactly that.

**Consequence for authoring: the pelvis height an authored clip specifies is not final.** Foot IK
will move it by up to 450 mm on uneven ground. Authoring decisions that depend on exact pelvis
height are only valid on flat ground.

### UpperBodyAim — `Assets/Core/Scripts/Gameplay/UpperBodyAim.cs`

| | |
|---|---|
| stage | `LateUpdate`, order 0 — **after** the Animator and after the IK pass |
| chain | `Spine`, `Chest`, `UpperChest`, `Head` (weighted), then `ApplySwordArmTuck`, then `ApplyElbowGuard` |
| serialized | `MaxTwist 55`, `MaxArmTuck 25`, `TravelArmAbduction 28` |
| **clavicle/shoulder** | **no** — the arm work starts at `RightUpperArm`; `TravelShoulderTilt` is 0 and is a spine-level value, not a clavicle rotation |
| suppressed | airborne, rolling; the arm tuck fades out as gait approaches sprint |

`ApplySwordArmTuck` rotates `RightUpperArm` by up to 25° to bring the sword arm in while
travelling — it takes the forearm, hand and weapon with it, so elbow and grip keep their shape.
`ApplyElbowGuard` refuses hyperextension below `MinElbowBend`.

**Consequence for authoring, and it is the good news of this audit: the authored clavicle survives
to screen.** Nothing at runtime rotates it. All the clavicle work in the Forward Walk R&D is
therefore real on the player, not something the runtime layer would have overwritten. It also means
an authored *upper-arm* abduction above 28° while travelling will be partly tucked back — so an arm
pose authored wide gets narrowed at runtime, and authoring it wide to compensate is a mistake.

### OffHandPose, TwistBoneDriver, HandGripPose, WeaponInertia

`OffHandPose` — left arm, `OnAnimatorIK`, **`Weight 0`**, `ElbowHintWeight 0.45`. Inert as shipped.
`TwistBoneDriver` — twist bones only. `HandGripPose` — fingers only.
`WeaponInertia` — order 60, spring-damper weapon weight plus arm drag; the last thing to touch the
weapon each frame.

## The two QA stages this implies

**RAW AUTHORING QA** — runtime gameplay IK disabled. Evaluates the `.anim` itself. Everything in
the Forward Walk R&D was this stage.

**RUNTIME CHARACTER QA** — the real Player, IK enabled, on flat AND uneven ground:

```
AccelerationLean -> Animator -> FootIK (feet + bodyPosition) -> OffHandPose
  -> UpperBodyAim (spine/head/right arm) -> TwistBoneDriver -> HandGripPose -> WeaponInertia
```

A clip that qualifies raw is not yet qualified on the character.

## `RuntimeIKPoseDeltaAudit` — designed, not built

Not built today, and deliberately: building it before there is a clip worth shipping would be
speculative. What it must do when it is built:

Sample the same clip twice on the real Player — once with the runtime writers disabled, once with
them live — and report position/angular deviation for pelvis, feet, chest, **both clavicles**,
upper arms, hands, head.

The hard part is not measuring, it is **classification**, so build that in from the start:

| observation | reading |
|---|---|
| feet move on uneven ground | expected — that is the system working |
| feet move on FLAT ground | suspicious — the clip's ground plane disagrees with the raycast |
| pelvis drops on a slope | expected |
| **any clavicle rotation at all** | **destructive** — nothing should be writing it |
| right upper arm tucks ≤25° while travelling | expected |
| right upper arm moves while standing | suspicious |
| head/chest rotate while aiming | expected |

Flat ground is the control. A delta that appears there is an override, not terrain correction.
