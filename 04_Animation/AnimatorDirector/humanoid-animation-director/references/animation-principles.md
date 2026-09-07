# Animation principles — Unity Humanoid, technically

## Why muscle space

A pose captured as bone transforms belongs to one skeleton. A pose captured as Humanoid muscle
values belongs to the *proportions-normalised* human, and retargets. `HumanPoseHandler.GetHumanPose`
gives `bodyPosition`, `bodyRotation` and 95 muscle values; an `.anim` for a Humanoid rig is exactly
that data as float curves named `RootT.x/y/z`, `RootQ.x/y/z/w`, and the `HumanTrait.MuscleName`
strings ("Chest Front-Back", "Left Shoulder Down-Up", …).

Muscle values are normalised: `+1` is the bone's default max limit, `-1` its min. Convert with
`HumanTrait.GetMuscleDefaultMin/Max`. `MuscleMath.DegreesToUnits` does this, so profiles and delta
tables are written in **degrees**, which is what an animator can actually reason about.

Snapshots store the muscle *names* alongside the values and rebuild into the current
`HumanTrait` ordering on load, so a pose captured under one Unity version still applies under
another.

## The body frame is not the pelvis (the big one)

`HumanPose.bodyPosition`/`bodyRotation` is Unity's mass-weighted body reference, somewhere between
hips and chest. It is **not** the pelvis and **not** the root.

Measured on the Bravehood warrior, holding the body frame fixed:

```
Spine Front-Back   +2°  ->  feet move 21.0 mm
Chest Front-Back   +2°  ->  feet move 17.4 mm
UpperChest F-B     +2°  ->  feet move  9.6 mm
Spine Left-Right   +2°  ->  feet move 23.0 mm
Left Shoulder D-U  +2°  ->  feet move  0.9 mm
Head Turn L-R      +2°  ->  feet move  0.0 mm
```

So *breathing alone* drags the feet by centimetres. This is the origin of most generated foot
skate, and no leg solve can absorb it — a knee that is already near-straight has no extension left.

The fix is to solve the **body transform itself**, all six DOF, per key, so that the pelvis and both
feet land back on their approved marks. Three cheaper targets were tried and each failed in a
distinct, instructive way:

| Target | Result |
|---|---|
| the Hips **bone** | hips error 0.00 mm, feet still 24.9 mm out — `Hips` maps to `Root` here, above the real leg parent |
| the foot midpoint | feet pinned, but the spine bend swung the pelvis 35 mm and the head 90 mm — a textbook MMO idle |
| pelvis **position** only | pelvis 0.00 mm, feet still 24.8 mm out — the spine bend *rotates* the pelvis and the feet swing on the ends of the legs |
| pelvis position **+** both feet, 6 body DOF | pelvis and feet both pinned. Correct. |

The authored pelvis motion is then added on top of that neutral transform, deliberately, and the
legs absorb it.

## Loop by construction, not by patching

Every motion channel is an exactly periodic function of normalised time with zero-slope turning
points, so value *and* velocity match across the seam automatically:

- **Breath**: `-cos(2π·warp(t))` with a piecewise-linear time warp that gives the inhale a shorter
  share of the cycle than the exhale. The warp kinks exactly where `-cos` has zero slope, so the
  result is still C¹.
- **Wave**: `sin(2π t)` — weight redistribution.
- **Settle**: `sin(4π t)` — second harmonic; breaks perfect periodicity so the eye cannot latch on.
- **Dwell**: `sin(½π·sin(2π t))` — dwells at the extremes; reads as a deliberate glance.
  Never shape a dwell with `pow(abs(w), k)`: it kinks at the zero crossing and the kink lands on
  the seam.

Keys are then written with **wrap-around auto tangents** — the slope at t=0 and t=duration is
computed from the same pair of neighbours — so the loop is C¹ with no seam patching, no ringing and
no tangent discontinuities.

## Sparse curves

Muscles that carry no motion get a two-key constant curve at the approved value: the pose is held
exactly, and the clip stays editable. Only muscles that actually move get multi-key curves.
A finished Bravehood idle runs ~30 animated curves against ~70 holding the pose.

Key density is *measured*, not assumed. After building, the generator samples the midpoint of every
key span and checks whether a foot left its mark; only spans that fail get subdivided. Every
refinement pass is scored and the best one wins — subdivision is not monotonic, because these
solves are redundant and a denser key set can land on a worse branch.

## The solver

One small Levenberg-Marquardt core (`HumanoidFootLockSolver`) serves four passes: body neutral,
foot lock, weapon-hand discipline, head stabilisation. It measures the *real posed skeleton*
(`SetHumanPose` → read bone transforms), never a model of it.

Three things it must have, each learned the hard way:

- **LM, not a per-parameter step.** A Jacobi step diverges: five parameters each "fully" correcting
  the same residual overshoot by ~5×, and the solve oscillates (measured 5 mm error growing to
  6.1 mm over 12 iterations).
- **Minimum-norm regularisation.** Every chain has more muscles than residuals. Without a small
  price on correction magnitude the answer is not unique, adjacent keys disagree, and the clip
  steps — a 1.2 mm foot jump inside one frame that no extra keys will smooth, because it is not an
  interpolation error.
- **Warm starting, twice around the loop.** Each key starts from the previous key's answer, and the
  last key copies the first's verbatim so the seam closes. A second lap re-solves key 0 warm from
  the end of lap one, which closes the cycle on itself.

Ordering matters: feet → weapon hand → head → feet again. The middle passes move arm and neck
muscles, and *any* muscle shifts Unity's mass-weighted body frame, which nudges the feet again.

## Measuring honestly

- `Quaternion.Angle` between consecutive 60 Hz samples of a restrained idle is **below float
  precision on the dot product** and returns exactly 0. Measure angular velocity across a stride of
  ~6 samples.
- Breathing-robot and bobble-head symptoms must be measured **pelvis-relative**. Absolute height is
  dominated by the shared pelvis lift, so every part appears to peak together even when the phase
  offsets are working correctly.
- The true pelvis for measurement is the **upper-leg midpoint**, not the `Hips` bone.
- Muscle values outside ±1 in the output are almost always **inherited from the approved pose**.
  The generator does not clamp the hero pose to hide them; the validator flags them as inherited so
  the fix happens on the pose.

## Base pose headroom

A joint sitting at its limit has no travel left in one direction, so the constraint solver cannot
absorb pelvis motion through it. Measured on a provisional pose with `Right Lower Leg Stretch` at
1.005 (outside the legal limit) and the left at 0.995:

```
pelvis budget scaled to        x0.166 automatically
foot drift across a small breathing change   0.2 mm  <->  2.3 mm
technical score                97/100   (the clip looked fine)
```

Softening both knees to 0.90 held foot drift under 0.9 mm across every breathing value tried, with
scores of 92-97. The pose was the defect, not the generator - which is why `BasePoseHealth` runs
before generation and gates it, and why "legal" and "production safe" are separate thresholds.

## Determinism

No noise, no jitter, no randomness anywhere. Same pose + same profile = same clip. If seeded
variation is ever wanted, it must be explicit and reproducible.
