# Sword1H_CombatIdle_SoftKneeAcceptance_v001 - Validation Report

Generated 2026-08-26 01:30:23Z | duration 3.20 s

## QUALITY LAYERS

```
BASE POSE HEALTH        PASS
SOLVER ROBUSTNESS       WARN
TECHNICAL VALIDATION    92 / 100
ARTISTIC APPROVAL       PENDING
```

These are independent. A high technical score says the curves are clean; it says
nothing about whether the pose was fit to animate, whether the solution is stable
under art direction, or whether the idle looks any good.

## BASE POSE HEALTH

**PASS**  (Sword1H_CombatIdle_SoftKnee_TestPose)

- Nothing blocks generation, but 2 joint(s) sit inside the production-safe margin (0.90). See solver headroom - those are the joints that will run out first if the animation is pushed.

### Solver headroom

| Joint | Value | Margin to legal | Headroom |
|---|---|---|---|
| Left Lower Leg Stretch | 0.9000 | 0.1000 | LOW |
| Right Lower Leg Stretch | 0.9000 | 0.1000 | LOW |

Headroom is how much travel a joint has left. It is the reason a pose is fragile,
not just the fact that it is.

## SOLVER ROBUSTNESS

**WARN**

| | |
|---|---|
| Samples | 21 |
| Failed solves | 0 |
| Worst foot drift | 1.117 mm |
| Worst foot yaw | 0.126 deg |
| Worst solver residual | 0.768 mm |
| Worst muscle margin to limit | 0.0621 |
| Branch discontinuities | 0 |
| Sweep time | 2.2 s |

- Nominal animation is valid but fragile. `pelvisResponseCm +20%`: foot drift 1.117 mm.
- Likely cause: base pose has insufficient joint headroom (worst margin to limit 0.0621). See BASE POSE HEALTH.
- 21 of 21 samples had the pelvis budget cut below half (worst x0.303) to keep the feet planted. The pelvis is doing far less than the profile asks for.

| Sample | Foot drift L/R | Foot yaw | Residual | Pelvis budget | Branch steps |
|---|---|---|---|---|---|
| pelvisResponseCm +20% | 1.117 mm / 0.336 mm | 0.105 deg | 0.768 mm | x0.303 | 0 |
| breathing +20% | 1.116 mm / 0.356 mm | 0.103 deg | 0.767 mm | x0.303 | 0 |
| breathing +10% | 0.876 mm / 0.377 mm | 0.126 deg | 0.666 mm | x0.303 | 0 |
| awareness +10% | 0.856 mm / 0.339 mm | 0.090 deg | 0.518 mm | x0.303 | 0 |
| awareness -10% | 0.845 mm / 0.337 mm | 0.080 deg | 0.519 mm | x0.303 | 0 |
| chestAmplitude +20% | 0.840 mm / 0.337 mm | 0.083 deg | 0.519 mm | x0.303 | 0 |
| weightShift -20% | 0.839 mm / 0.342 mm | 0.089 deg | 0.549 mm | x0.303 | 0 |
| nominal | 0.838 mm / 0.335 mm | 0.091 deg | 0.519 mm | x0.303 | 0 |
| weightShift +20% | 0.837 mm / 0.339 mm | 0.092 deg | 0.526 mm | x0.303 | 0 |
| chestAmplitude -10% | 0.818 mm / 0.335 mm | 0.089 deg | 0.519 mm | x0.303 | 0 |
| chestAmplitude -20% | 0.813 mm / 0.334 mm | 0.082 deg | 0.519 mm | x0.303 | 0 |
| chestAmplitude +10% | 0.809 mm / 0.335 mm | 0.088 deg | 0.519 mm | x0.303 | 0 |
| pelvisResponseCm +10% | 0.808 mm / 0.377 mm | 0.089 deg | 0.665 mm | x0.303 | 0 |
| awareness -20% | 0.782 mm / 0.339 mm | 0.082 deg | 0.571 mm | x0.303 | 0 |
| weightShift +10% | 0.778 mm / 0.336 mm | 0.072 deg | 0.568 mm | x0.303 | 0 |
| awareness +20% | 0.772 mm / 0.336 mm | 0.074 deg | 0.571 mm | x0.303 | 0 |
| weightShift -10% | 0.768 mm / 0.339 mm | 0.076 deg | 0.576 mm | x0.303 | 0 |
| breathing -10% | 0.733 mm / 0.319 mm | 0.093 deg | 0.392 mm | x0.303 | 0 |
| pelvisResponseCm -10% | 0.730 mm / 0.340 mm | 0.075 deg | 0.392 mm | x0.303 | 0 |
| pelvisResponseCm -20% | 0.627 mm / 0.269 mm | 0.103 deg | 0.384 mm | x0.303 | 0 |
| breathing -20% | 0.626 mm / 0.244 mm | 0.098 deg | 0.383 mm | x0.303 | 0 |

We want a stable solution NEIGHBOURHOOD, not one configuration that happens to
pass - art direction will move these parameters.

## TECHNICAL ANIMATION SCORE

**92/100** - this is a *technical animation* score, not an overall
quality score. It measures curve and constraint correctness only.

| Bucket | Points | Max |
|---|---|---|
| Loop | 20.0 | 20 |
| Foot stability | 14.8 | 20 |
| Root stability | 10.0 | 10 |
| Pose preservation | 15.0 | 15 |
| Weapon stability | 12.2 | 15 |
| Curve quality | 10.0 | 10 |
| Humanoid safety | 10.0 | 10 |

**WeaponHandStabilityScore: 82/100**

> Loop rotation continuity is below the float resolution of Quaternion.Angle.
> That means *indistinguishable*, not provably identical - do not read it as exact.

## Measurements

| Check | Value | |
|---|---|---|
| Loop position continuity | < 0.001 mm | PASS |
| Loop rotation continuity | < 0.001 deg | PASS |
| Loop velocity continuity | 0.68 mm/s (LeftHand) | PASS |
| Root translation | < 0.001 mm | PASS |
| Root yaw | < 0.001 deg | PASS |
| Left foot drift (horizontal) | 0.838 mm | WARN |
| Right foot drift (horizontal) | 0.335 mm | PASS |
| Foot yaw range | L 0.05 / R 0.09 deg | PASS |
| Toe vertical range | 3.68 mm | WARN |
| Pelvis vertical range | 1.64 mm | PASS |
| Pelvis horizontal excursion | 1.66 mm | PASS |
| Head max angular deviation | 0.61 deg | PASS |
| Head positional travel | 28.86 mm | WARN |
| Chest max angular deviation | 2.28 deg | PASS |
| Sword-hand position variance (RMS) | 1.06 mm | PASS |
| Sword-hand rotation variance (RMS) | 0.38 deg | PASS |
| Sword-hand path length | 9.92 mm | PASS |
| Sword-hand max speed | 29.9 mm/s | PASS |
| Off-hand path length | 84.07 mm | PASS - reference - the off hand SHOULD move more |
| Max bone angular velocity | 9.2 deg/s (LeftUpperLeg) | PASS |
| Solver branch steps | 0 | PASS |
| Humanoid muscle limit violations | 0 | PASS |
| Curve tangent discontinuities | 0 | PASS |
| Animated curves / total keys | 42 / 582 | PASS - sparse authoring |

## Failure-mode sweep

- **Dancing Feet** - measurable foot travel or yaw. The feet must be planted.

## Generation

- Foot-lock solve worst residual: 0.519 mm
- Curves: 42 animated, 60 holding the approved pose
- Keys per animated curve: 11 (1 refinement pass(es); worst between-key foot drift 0.790 mm)
- Motion budget clamps:
  - Pelvis translation scaled x0.303 so the feet stay planted (feet outrank the pelvis).

## ARTISTIC REVIEW NEEDED

Technical validation says nothing about whether this looks good. Review on the character,
with the real sword, from the gameplay camera:

1. Does the character still look dangerous?
2. Has the hero pose been preserved?
3. Is breathing visible but unobtrusive?
4. Does the character look grounded?
5. Does the weapon remain controlled?
6. Does the head remain focused?
7. Can you detect the loop?
8. Does anything feel repetitive?
9. Does the silhouette collapse?
10. Does the idle look good from the gameplay camera?

Artistic approval is the user's call, never the tool's.

## Revision note

Hardening pass acceptance TEST B (both knees softened to 0.90).
