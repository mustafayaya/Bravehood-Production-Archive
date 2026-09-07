# COMBAT-ARM PRECONDITIONING BLOCKED — the premise is not true: the sword arm does not inherit the mocap walking swing

Phase A completed and shipped. Phase B measured the premise before building on it, and the premise
does not hold. No v012. Production untouched.

## PHASE A — TEMP-ASSET SAFETY (DONE)

`AnimationAssetSafety` added, with three entry points, all reading the SERIALIZED controller because
an in-memory lookup is exactly what reported Travel as unchanged while the file said otherwise:

- `FindReferences(clipPath)` - every production BlendTree child pointing at a clip.
- `SafeDelete(clipPath)` - refuses, loudly, naming the tree and child index, if anything references it.
- `VerifyProduction()` - fails if ANY `__*` research clip is referenced anywhere in the controller.

Lifecycle rule now followed and documented: scan references, restore intended references, save,
force reimport, re-read from disk, confirm no temp GUID remains, only then delete, then re-read again.

**Root cause not proven.** I did not reproduce the remapping in a minimal case, and per the brief's
own guidance I stopped short of unlimited Unity forensics once the preventive rule closed the hole.
What is established: the corruption appeared only when temp clips were deleted while the controller
was loaded, and this milestone deleted six temp clips under the new rule with the controller
verified clean before and after.

Permanent coverage: `P1_NoResearchClipIsReferencedByProduction`,
`P2_SafeDelete_RefusesWhileReferenced`.

P2 initially failed for an unrelated reason worth recording: `Knight_Controller` carries a **broken
PPtr (local id 3908002699880397750)** that is present in git HEAD and in every backup - it predates
all of this work. Loading the controller logs it and NUnit fails on unexpected error logs. It is now
scoped out inside that test with a comment. It is a real pre-existing asset defect and worth a
separate look.

## PHASE B — THE PREMISE, MEASURED

The brief's model was: the Blend lets the sword arm inherit the mocap's normal walking swing, which
puts the hand 57-133 mm from the combat carry, and the solver then has to undo it.

`Blend` already computes `approvedPose + sourceDeviation x regionWeight`, with a dedicated
`SwordArm` region. **That weight is already 0.10** - the sword arm is 90% approved combat pose, not
an inherited walking arm.

Pre-solve hand gap, with the downstream weapon/arm correction disabled entirely:

| candidate | shoulders | swordArm | gap mean | gap max | rot gap | clavicle dev | arm dev | elbow range | hand path |
|---|---|---|---|---|---|---|---|---|---|
| P0 (current) | 0.40 | 0.10 | **91 mm** | 143 | 8.7 | 18.9 | 12.5 | 17.6 | 164 |
| P1 | 0.15 | 0.10 | **90 mm** | 144 | 8.7 | 18.9 | 12.5 | 17.1 | 191 |
| P2 | 0.05 | 0.05 | **91 mm** | 144 | 8.6 | 18.9 | 12.4 | 17.8 | 162 |

Cutting the shoulder region 8x and the arm region 2x moves the gap by 1 mm. There is no walking-arm
swing in there to remove.

## TWO FINDINGS THAT CHANGE THE PICTURE

**1. The ~18.9 deg clavicle deviation is present PRE-SOLVE.** It is 18.9 in all three candidates
above, with no weapon or arm correction running at all. So it is not caused by the weapon solver -
it follows the torso solve, because the clavicle's local rotation is measured against an approved
pose whose chest orientation has since been re-authored. The muscle saturation at -1.000 IS caused
by the weapon solve and is a separate effect. Previous milestones treated these as one symptom;
they are two, and only one of them belongs to the arm.

**2. Most of the 91 mm gap is built into the target, not the pose.** Measured against weapon-frame
translation follow, with everything else fixed:

| follow | gap mean | gap max |
|---|---|---|
| 0.00 (pelvis-anchored) | 113 mm | 159 |
| 0.25 | 104 | 150 |
| 0.75 (current) | 89 | 133 |
| 1.00 (pure chest-relative) | **83** | 125 |

The gap falls as the target follows the chest, which is the opposite of what an inherited arm swing
would produce - an arm swinging away from the body would show a gap independent of where the target
is anchored. A quarter of the pelvis-anchored component alone accounts for roughly 25 mm of it.

## WHY THIS BLOCKS THE MILESTONE AS SPECIFIED

The instruction was to precondition the arm so the downstream correction shrinks toward ~20-40 mm,
where the pricing solver was proven effective. The arm cannot be preconditioned closer than it
already is: it is already 90% approved pose, and the residual gap is a property of the target
construction and of the torso having moved, not of the arm's blend weight.

Reducing the gap therefore means changing the TARGET - either accepting a fully chest-relative
weapon frame (83 mm, and the previous milestone showed that costs sword travel) or re-deriving the
approved hand relationship against the walking torso rather than the idle one. Both are changes to
what "combat carry" means during locomotion, which is art direction, and neither is what this
milestone authorised.

## SMALLEST NEXT PROBLEM

Re-derive the combat-carry hand relationship **against the walking torso**. The current target is
the idle pose's hand-relative-to-chest, reconstructed on a chest that now has authored lean, roll
and a different position relative to the pelvis. An approved carry captured or authored under
walking conditions would start near the blended hand, and then the 37 mm regime where the pricing
solver demonstrably works would be the normal case rather than an isolation test.

## STATE

Live profile restored to `CombatWalkCanonicalBaseline`; zero-authority invariant re-verified at
**0.000000**. Regression suite **28 passed / 0 failed / 0 skipped** (O1-O4, P1-P2 all PASS).
v008 `7a7ca074`, v009 `5546958e`, v010 `7c44dd1d`, v011 `e3705c2f` unchanged. No v012.
Serialized controller, read from disk after all deletions: `Light_Walk8` = v011 @ ts 1.00,
`Travel_Walk8` = v008 @ ts 0.50, `CombatWalkSpeedScale` 0.65, **no research clip referenced**.

Research clips kept: `__wf75`, `__sw25`, `__wc_cp12`, `__Sword1H_WalkForward_CanonicalProbe`.

`COMBAT-ARM PRECONDITIONING BLOCKED — the sword arm already blends at 0.10 (90% approved combat pose); cutting the shoulder and arm region weights 8x/2x moves the pre-solve gap by 1 mm, so there is no inherited walking swing to remove. The residual 83-91 mm is built into the target, and the 18.9 deg clavicle deviation exists pre-solve.`
