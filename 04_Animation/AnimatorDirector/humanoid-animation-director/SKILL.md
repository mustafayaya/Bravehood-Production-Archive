---
name: humanoid-animation-director
description: Author restrained, production-quality combat IDLE animations for Unity Humanoid characters from an approved hero pose — capture the pose, generate a seamless looping .anim with locked feet and a disciplined weapon hand, validate it numerically, and iterate from art-direction feedback. Use whenever the user asks for an idle/combat idle/breathing animation, wants a pose turned into an animation, reports foot sliding, foot skating, a visible loop seam, a floaty or bobbing character, a loose weapon hand, or wants to tune breathing/weight shift/awareness on an existing generated idle. Version 1 covers IDLE ONLY — not locomotion, attacks, dodges, turns, or hit reactions.
---

# Humanoid Animation Director — combat idle (v1)

> **Resuming work, or new to this?** Read [`CURRENT_STATE.md`](CURRENT_STATE.md) first. It carries
> the production asset, the current technical reference, the frozen architecture, and — most
> valuable — the list of **diagnoses that were tested and proved wrong**, so they are not
> re-derived. It is short on purpose.

**The approved pose is sacred.** The idle is built *around* it, never instead of it:

```
Approved Hero Pose
  + Breathing
  + Very small weight redistribution
  + Micro postural adjustment
  + Subtle target awareness
  = Combat Idle
```

The quality target is not "look how much animation there is." It is: *I almost don't notice the
animation, but the character feels alive, grounded, focused, and dangerous.*

When in doubt, **less motion**. Adding motion is the most common way to make an idle worse.

## What v1 does and does not do

Does: one restrained, seamless combat idle from one approved pose, with both feet locked, no root
motion, a disciplined weapon hand, technical validation, and numeric iteration (v001 → v002 → …).

Does NOT (do not build these, do not "architect for" them speculatively): walk/run cycles,
strafing, blend trees, root-motion locomotion, attacks, dodges, hit reactions, motion matching,
procedural footsteps, a general IK system, motion synthesis, ML.

## The system

Unity side, all editor-only, under `Assets/Bravehood/Animation/`:

| | |
|---|---|
| `Editor/HumanoidPoseSnapshot.cs` | the approved pose in Humanoid muscle space (retargetable) |
| `Editor/HumanoidPoseCapture.cs` | capture / re-apply a pose |
| `Editor/HumanoidPoseDelta.cs` | `ApprovedPose + SmallDelta`, tagged by channel and region; `MuscleMath`, muscle-name vocabulary `M` |
| `Editor/HumanoidIdleProfile.cs` | every knob: duration, channel weights, amplitudes, phases, motion budget |
| `Editor/IdleChannelMath.cs` | the periodic motion basis (loops by construction) |
| `Editor/HumanoidIdleGenerator.cs` | delta table → budget → solves → sparse muscle curves |
| `Editor/HumanoidFootLockSolver.cs` | the measured-skeleton solver: body neutral, feet, weapon hand, head |
| `Editor/HumanoidBasePoseHealth.cs` | is this pose fit to animate? headroom, safe vs legal range |
| `Editor/HumanoidSolverRobustness.cs` | is the solution stable, or one lucky configuration? |
| `Editor/HumanoidIdleValidator.cs` | world-space measurement, score, failure-mode sweep |
| `Editor/IdleBuildPipeline.cs` | generate -> validate -> sweep -> commit, as one transaction |
| `Editor/IdleDiagnostics.cs` | per-version `_Diagnostics.json` |
| `Editor/CharacterStateScope.cs` | exact save/restore of the character, even on a throw |
| `Editor/Measure.cs` | resolution floors - never print precision a metric does not have |
| `Editor/IdleAuthoringRules.cs` | rules enforced in code, not just remembered |
| `Editor/Tests/HumanoidIdleRegressionTests.cs` | 8 regression tests, one per hard-won discovery |
| `Editor/HumanoidAnimationPreview.cs` | scrubbing, base-pose comparison, scene-view trails |
| `Editor/HumanoidIdleAuthoringWindow.cs` | **Bravehood ▸ Animation ▸ Humanoid Idle Authoring** |
| `Profiles/` `PoseReferences/` `Generated/` `Reports/` | assets and output |

Outputs are versioned: `Generated/<Clip>_v001.anim`, `Reports/<Clip>_v001_Spec.md`,
`Reports/<Clip>_v001_Report.md`, `Reports/<Clip>_v001_Diagnostics.json`.
**Never overwrite an approved version.** The spec records the exact profile values that produced
that version, so an older one can always be reproduced.

## Four quality layers - never collapse them

```
BASE POSE HEALTH        PASS / WARN / FAIL
SOLVER ROBUSTNESS       PASS / WARN / FAIL
TECHNICAL VALIDATION    n / 100
ARTISTIC APPROVAL       PENDING
```

These are independent, and the report always prints all four. A technically clean clip generated
from a fragile base pose must not be able to hide behind one aggregate number - that is exactly
what happened before this existed: 97/100 on a pose with a hyperextended knee, whose foot drift
then swung between 0.2 mm and 2.3 mm on a breathing change no director would think twice about.

The technical number is the **Technical Animation Score**, never an "overall quality score". It
measures curve and constraint correctness only. **Artistic approval stays PENDING until a human
has looked at it** - never infer it from metrics.

## Base pose health - the gate

`HumanoidBasePoseHealth.Evaluate` runs BEFORE anything is generated and asks the right question:
not *is this pose legal* but *does it leave the solver enough headroom to animate safely*.

It distinguishes two thresholds, both configurable per rig on the profile (`poseSafety`):

- **Humanoid legal limit** - Unity's theoretical bound.
- **Production safe range** - conservative, leaves the constraint solver something to work with.
  A muscle can be perfectly legal at 0.995 and still be dangerous, because the foot-lock solver
  has to absorb pelvis motion through it.

A **FAIL blocks generation.** `Generate` throws `BasePoseUnhealthyException` and writes nothing.
The override (`IdleGenerationOptions.allowUnhealthyPose`, the window's "Generate Anyway...", or the
separate override menu item) is deliberately awkward - a second button plus a confirmation dialog.
Do not reach for it to get past a red box; fix the pose. An overridden run still records the
failing health in its report and diagnostics.

Report `SOLVER HEADROOM` when explaining a fragile pose - it answers *why*, not just *that*:

```
Right Lower Leg Stretch   1.005   margin to legal -0.005   CRITICAL
Left  Lower Leg Stretch   0.995   margin to legal  0.005   CRITICAL
```

## Solver robustness - a stable neighbourhood, not one lucky configuration

`HumanoidSolverRobustness.Run` perturbs the art-directable parameters one at a time
(breathing, chest amplitude, pelvis response, weight shift, awareness) by -20/-10/+10/+20 %,
re-solves each, and measures foot drift, foot yaw, residual, convergence, pelvis budget cut,
muscle margin and branch continuity. 21 solves, ~2 s.

One-at-a-time on purpose: the goal is detecting fragility, not proving stability over a product
space. This matters because **art direction will move these parameters** - a clip that only passes
at its nominal settings is not finished.

## Workflow

1. **Inspect** the character: Humanoid Animator, valid Avatar, `humanScale` sane (~0.83 for the
   Bravehood warrior). Non-Humanoid = stop; this system works in muscle space.
2. **Get the approved pose onto the rig**, then **Capture Current Pose**. Prefer capturing the real
   rig over interpreting reference images. If the user supplies images, read them for *intent* —
   silhouette, weight distribution, weapon orientation, shoulder relationship, foot orientation,
   head posture — never try literal IK from a 2D image.
3. Save `<Clip>_BasePose.asset`.
4. Write the spec (`templates/idle-spec.md`) — say what you intend before you generate.
5. **Check base pose health.** A FAIL is a pose problem - fix the pose, do not override.
6. **Generate**. Read `Reports/<Clip>_v001_Spec.md`: the sparse delta table is the animation, in
   readable degrees.
7. **Validate**, and read all four layers plus the robustness sweep. Fix anything the failure-mode
   sweep names.
8. **Present** what changed, with numbers. **Never claim artistic approval.**
9. **Review on the gameplay camera** (below), then iterate.

Generating from a script - use the pipeline, which is the transactional path:

```csharp
var outcome = IdleBuildPipeline.Run(animator, poseSnapshot, profile,
                  HumanoidIdleAuthoringWindow.NextVersionPath(profile.clipName),
                  HumanoidIdleAuthoringWindow.ReportFolder);
if (outcome.blockedBy != null) { /* base pose FAILED - nothing was written */ }
```

Nothing reaches disk until generate, validate and the sweep have all succeeded, so a throw leaves
the previous approved version, the scene and the character exactly as they were.

Through MCP `execute_code`: no `using` directives (the code is a method body — fully qualify
`Bravehood.AnimationAuthoring.*`), CodeDom is C# 6, and `AssetDatabase.DeleteAsset` is a blocked
pattern (delete files from the shell and `AssetDatabase.Refresh()` instead). A generate+validate
call often outlives the MCP response timeout — the work still completes, so read the report file
rather than re-running.

## Iteration

Corrections change the **profile**, then regenerate. Never hand-edit the clip; never rebuild from
scratch. Because every delta is tagged with its channel and region, feedback maps directly:

| Feedback | Change |
|---|---|
| "reduce chest breathing 25%" | `breathing` ×0.75 (or `chestAmplitude`) |
| "reduce right-hand movement 40%" | raise `weaponStability` |
| "keep the shoulders lower throughout" | `shoulderDropDeg` / a constant shoulder offset in the pose |
| "halve the awareness head movement" | `awareness` ×0.5 |
| "move the weight-shift event from 46% to 58%" | `phaseWeightShift` |
| "more asymmetry" | `offSideGain` |
| "don't change the base pose / duration" | leave `keyTimes`, `duration`, and the snapshot alone |

Each correction produces the next version. Then diff v001 vs v002 in the report numbers and say
what actually moved.

## Reading the validation report

Technical score is out of 100 (Loop 20, Foot 20, Root 10, Pose preservation 15, Weapon 15, Curve
10, Humanoid safety 10) plus a standalone `WeaponHandStabilityScore`.

**A score of 100 is not approval.** It means nothing is measurably broken. Whether it looks
dangerous, focused, and alive is the user's call, and only from the gameplay camera.

The failure-mode sweep names defects in the language of the craft — Mannequin Idle, MMO Idle,
Breathing Robot, Floating Character, Locked Torso, Loose Sword, Bobble Head, Dancing Feet,
Pendulum Idle, Visible Loop. `references/combat-idle.md` has the cause and the fix for each.

`scripts/validate_idle.py <clip.anim>` runs the curve-level checks (loop continuity, muscle
limits, tangent breaks, key density, root locks) straight off the YAML with no Unity running.
Use it when the editor is closed or in CI. It cannot see world space, so it never replaces the
in-editor validator.

## In-game camera review (required before approval)

The Animation Preview window lies: it shows a front elevation at a distance no player ever has.
Before calling an idle done, play it **on the actual player character, with the actual equipped
sword and normal armour, through the real Bravehood gameplay camera at gameplay FOV**. An idle
that reads beautifully head-on can be invisible or twitchy over the shoulder.

## Hard-won facts about Unity Humanoid

These were measured on this project's rig. They are not intuitions — each one cost a wrong build.
Detail and numbers in `references/animation-principles.md`.

1. **`HumanPose.bodyPosition/bodyRotation` is NOT the pelvis.** It is Unity's mass-weighted body
   frame. Bend the spine 2° holding it fixed and the whole lower body slides ~20 mm. Every idle
   that "skates" starts here. The generator solves all six body DOF per key so the pelvis *and*
   both feet land on their marks, then adds the authored pelvis motion deliberately on top.
2. **The Hips *bone* may not be the pelvis.** Bravehood's warrior maps `Hips → Root`. Pinning it
   measured 0.00 mm error while the feet travelled 24.9 mm. Use the upper-leg midpoint.
3. **Never author upper-leg twist or lower-leg stretch.** Humanoid has no ankle yaw, so leg twist
   pivots a planted foot with nothing able to undo it. The legs belong to the constraint solver.
4. **Redundant solves need minimum-norm regularisation and warm starting.** More muscles than
   residuals means each key can pick its own equally-valid answer; adjacent keys then disagree and
   the clip steps (measured: 1.2 mm foot jump inside one frame). Extra keys will not fix it.
5. **Head stabilisation must constrain orientation too.** Position-only solving made the neck flail
   12.8° to save 45 mm of travel. Target the *natural* orientation so the awareness glance survives.
6. **`Quaternion.Angle` returns exactly 0 below float precision.** Per-frame bone deltas in a
   restrained idle are under it — measure angular velocity across a stride of ~6 samples.
7. **Measure breathing symptoms pelvis-relative.** Absolute height is dominated by the shared
   pelvis lift, which makes every body part look like it peaks together.
8. **Feet outrank the pelvis.** When the approved pose has near-locked knees there is no extension
   left to absorb a pelvis lift; the generator scales the pelvis down and says so in the report.
9. **Muscle-limit violations are usually inherited** from the approved pose, not created. Fix the
   pose. The generator never silently clamps the hero pose.

## Canonical generation frame — architectural rule

**Procedural animation authoring runs in a canonical coordinate frame unless world placement is
explicitly part of the authored motion.** For Humanoid locomotion generation, inside the
transactional state scope:

```csharp
root.position = Vector3.zero;
root.rotation = Quaternion.identity;
```

Generation output must never depend on scene position, scene yaw, Play Mode displacement, or
prefab placement. `CharacterStateScope` restores the original transform afterwards, including on
a throw.

This is not defensive tidiness — it repairs a real defect that cost a milestone. Every measurement
in the generator is root-LOCAL, which *ought* to make placement irrelevant. It does not, because
world-to-local conversion is float arithmetic and the whole-cycle solve does not fully converge, so
round-off at the seventh decimal is amplified. Measured: moving the character changed the generated
clip by 0.2849 muscle units, yawing it by 0.2852, and a spread of five placements produced a
*continuum* of answers (0.0146, 0.11, 0.18, 0.25, 0.28, 0.31) rather than two branches. It lands in
the right leg because that chain is 17.3 mm shorter and works nearest its reach limit — the marginal
chain absorbs the conditioning.

The practical consequence is worse than a wrong number: a clip generated on Monday could not be
reproduced on Tuesday if the Player had been moved by a Play Mode session in between, and
`Sword1H_WalkForward_v011` is permanently unreproducible for exactly that reason. Guarded by
`O2_LocomotionGeneration_IsPlacementInvariant`.

Related rule: **a regression test that cannot find its fixture must fail, not skip.** The first
determinism test scanned the open scene for a Humanoid and quietly ignored itself whenever the
editor was on a UI scene — silent during precisely the sessions it was meant to protect. Tests that
need a character now instantiate the Player prefab themselves.

## An isolation measurement must observe its own isolation

Two milestones of "pre-solve" arm numbers were wrong because the weapon solve had not actually been
disabled: `DisciplineUpperBody` guards its root-space `SolveWeaponPass` inside the weapon/head loop,
and a second, unguarded call ran at the end regardless. Enabling an arm architecture therefore
*looked* like disabling the root-space solve and did not - and no number in the output said so,
because a clip cannot report which passes touched it.

`WeaponSolveExecutionTrace.BeginIsolation()` / `AssertIsolated()` now makes that class of error
fail loudly, naming the stage and chain that ran. Guarded by `Q1` and `Q2`.

The general rule: **a measurement that claims an operation was skipped must be able to observe that
it was skipped.** A plausible wrong number costs more than a loud failure. The same applies to any
future isolation test here.

Related: when a solve looks like it is failing, check what runs AFTER it before theorising about
the solver. The priced arm solve converged to 3.1 mm with a healthy clavicle inside the production
sequence; the pass that followed moved it to 103 mm and 18.2 deg. Three milestones read that output
as a solver pathology.

## Technical qualification is not artistic approval

These are two different gates with two different authorities, and collapsing them is the most
expensive mistake available here. The Forward Walk reached the end of its R&D **technically
qualified and artistically rejected** — a valid, expected outcome, and one the vocabulary has to be
able to express.

### TECHNICAL QUALIFICATION — granted by measurement

> Legal Humanoid curves? Deterministic? Loop clean? Feet planted? Support schedule correct? No
> pathological jitter? No controller corruption? Anatomical limits reasonable? Runtime integration
> safe?

Technical qualification means exactly one thing: **SAFE TO REVIEW.** It does not mean good.

### ARTISTIC MOTION APPROVAL — granted only by a human

> Does it look human? Is the weight transfer convincing? Do the joints overlap naturally? Is the
> spacing appealing? Does the motion carry inertia? Are the asymmetries believable? Does the class
> personality read? Does it feel hand-authored rather than solved? Does it look excellent from the
> gameplay camera?

No measurement grants this. No number is evidence for it.

### The rule that matters

**If a clip passes every technical gate and a human reviewer says "this still looks procedural",
the clip is not production-approved.** Do not defend it with residuals, jerk, foot metrics,
legality, or test count. Metrics diagnose and protect; they do not certify beauty. Answer the
observation, or accept the rejection — never argue the reviewer out of it with a table.

### `HumanMotionQualityReview` — the final gate

Not a score. A structure for looking, so review produces specific, actionable observations instead
of "something's off". Walk it in order:

```
weight acceptance        compression / recovery     hip rhythm
femur / knee relation    foot rollover              torso overlap
shoulder rhythm          elbow overlap              weapon inertia
head stabilisation       asymmetry                  timing texture
pose appeal between key phases                      absence of procedural / IK read
```

Record the verdict as two independent lines, always both:

```
TECHNICAL QUALIFICATION   QUALIFIED / BLOCKED - <reason>
ARTISTIC MOTION APPROVAL  APPROVED / REJECTED - <what specifically reads wrong> / PENDING
```

## Frozen architecture

Accepted, and not to be retuned without evidence that directly disproves one of them. Each cost at
least one milestone to establish:

| | |
|---|---|
| canonical identity-frame generation | placement changed output by 0.28 muscle units |
| deterministic generation | same pose + profile = identical curves |
| placement invariance | `O2` |
| transactional `CharacterStateScope` | exact restore, including on a throw |
| semantic support scheduling | weight-bearing is low sole AND low speed, a product |
| whole-cycle leg correction | per-key solves step; band-limiting restores continuity |
| whole-cycle upper-body B-spline correction | the qualified chest/head foundation |
| interpolation legality audit | keys legal is not curves legal |
| temporal / jitter audit | |
| posture drift audit | |
| swing-foot trajectory audit | |
| anatomical rotation audit | swing-twist, flexion-gated knee plane |
| serialized Animator/BlendTree verification | in-memory lookups have lied; read the file |
| `AnimationAssetSafety` / safe temp deletion | Travel drifted three times |
| weapon-pass execution tracing | see the isolation rule above |
| anatomically priced arm-chain solving | pricing frees the clavicle completely when told to |
| guard preventing the legacy unpriced weapon pass from overwriting an arm architecture | `Q1` |

Every experimental switch is inert at its default, and all of them are 0 in every saved profile.
Zero-authority behaviour is verified by regenerating the frozen canonical probe: **bit-identical,
102 curves, max abs difference 0.00000000.** Keep it that way — a mechanism that is not inert when
disabled cannot be left in the codebase.

## Runtime IK is a second QA stage, and the Player is not what it looks like

There is **no right-hand IK**; `OffHandPose` is the LEFT arm and ships at `Weight 0`; "Body IK" is
`UpperBodyAim`, a LateUpdate spine/head rotator. `FootIK` writes `Animator.bodyPosition` directly
(pelvis drop to 0.45 m), so an authored pelvis height is not final off flat ground.

**Nothing at runtime rotates the clavicle** — authored clavicle work survives to screen.
`UpperBodyAim` does tuck the right upper arm by up to 25° while travelling, so an arm authored wide
gets narrowed at runtime; authoring it wider to compensate is a mistake.

Full detail, execution order and the design for `RuntimeIKPoseDeltaAudit` in
`references/runtime-ik.md`. RAW AUTHORING QA (IK off) and RUNTIME CHARACTER QA (IK on, flat AND
uneven ground) are separate stages; passing the first does not pass the second.

## Adapting motion this system did not author

For a clip from another model, mocap, the marketplace, or a human animator, the job inverts:
preserve the motion, correct only what Bravehood requires. `references/external-motion-intake.md`
has the workflow — read-only audit first, modular authorities that default to preserve-source, a
motion preservation budget, and the three-way challenger A/B.

## Editor safety

Two Unity crashes during the first build, both now structurally prevented:

- **`EditorApplication.Exit` from an interactive path.** Every shutdown goes through one guarded
  helper (`if (Application.isBatchMode)`). An interactive code path must never be able to close the
  editor, and no entry point may open a scene under a live editor either.
- **A lambda that captured the variable it was later assigned to.** `ResidualFn r = t => base(t);
  base = r;` is infinite recursion, a native stack overflow, and no managed trace. Copy a delegate
  into its own local before wrapping it.

Generation is transactional: `CharacterStateScope` saves every bone's local TRS plus the root
transform and the selection, and restores them in a `finally` - measured exact (0.0000 mm /
0.0000 deg) after success, after validation and after a throw. Restoring via a `HumanPoseHandler`
Get/Set round trip is NOT exact; it drifts ~0.016 muscle units per run and accumulates.

## Numerical honesty

Never print precision a metric does not have, and never make a quality decision from a value
beneath its resolution floor. `Measure` owns this: sub-resolution values print as `< 0.001 mm` /
`< 0.001 deg`, not `0.000`. `Measure.AngleBetween` reports whether the answer was below resolution
at all - `Quaternion.Angle` returning 0 means *indistinguishable at float precision*, not
*identical*, and the report says so explicitly when that happens.

Units: mm, degrees, mm/s, normalised muscle units. Nothing else.

## Regression suite

`Assets/Bravehood/Animation/Editor/Tests/` - EditMode, ~2.5 s, run from the Test Runner or
`run_tests`. Each test guards one discovery that cost a wrong build, and each explains the failure
it protects against, because the wrong version passes casual inspection.

| | |
|---|---|
| A | six-DOF planted-body correction (a 2 deg spine bend displaces the feet ~20 mm; feet AND pelvis must both land) |
| B | authored channels never touch solver-owned leg muscles - and the solver still does |
| C | branch continuity: zero single-frame steps, and the detector can see a 1.2 mm one |
| D | loop metrics distinguish "below resolution" from "zero" |
| E | seam velocity passes a C1 seam (0.65 mm/s) and fails a broken one (3251 mm/s) |
| F | determinism: same pose + profile = identical curves |
| G | isolated parameter edit: only the intended channel moves, v001 is byte-identical afterwards |
| H | base pose health fails a hyperextended knee AND gates generation |

## Determinism

Same pose + same profile = same clip. No Perlin noise, no jitter, no random twitching — those are
shortcuts, not animation. All motion comes from intentional periodic curves. If variation is ever
added, it must be seeded.

## References

- `references/animation-principles.md` — muscle space, the loop-by-construction basis, sparse
  curves and cyclic tangents, the solver, the Unity traps in full.
- `references/combat-idle.md` — channels, phase offsets, asymmetry, motion budget, the ten failure
  modes with causes and fixes.
- `references/bravehood-style.md` — the Bravehood 1H stance, character feeling, profile values.
- `templates/idle-spec.md`, `templates/idle-review.md` — write the spec before generating, run the
  review after.
- `references/runtime-ik.md` — what actually runs on the Player: execution order, weights, which
  bones each system writes, and the design for `RuntimeIKPoseDeltaAudit`.
- `references/external-motion-intake.md` — adapting motion this system did not author: read-only
  audit, modular authorities that default to preserve-source, motion preservation budget, the
  three-way challenger A/B.
- `CURRENT_STATE.md` — the handoff. Where things stand, what is frozen, what was disproved.
