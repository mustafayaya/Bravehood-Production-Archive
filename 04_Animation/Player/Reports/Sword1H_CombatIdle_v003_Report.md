# Sword1H_CombatIdle_v003 - Validation Report

Generated 2026-08-26 01:57:29Z | duration 3.20 s

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

**PASS**  (Sword1H_CombatIdle_ApprovedPose)

- Every tracked joint has usable travel left; nothing is near its limit.

### Solver headroom

| Joint | Value | Margin to legal | Headroom |
|---|---|---|---|
| all tracked joints | - | healthy | HEALTHY |

Headroom is how much travel a joint has left. It is the reason a pose is fragile,
not just the fact that it is.

## SOLVER ROBUSTNESS

**WARN**

| | |
|---|---|
| Samples | 21 |
| Failed solves | 0 |
| Worst foot drift | 1.315 mm |
| Worst foot yaw | 0.214 deg |
| Worst solver residual | 4.372 mm |
| Worst muscle margin to limit | 0.0501 |
| Branch discontinuities | 12 |
| Sweep time | 3.2 s |

- Nominal animation is valid but fragile. `weightShift +10%`: foot drift 1.315 mm.
- 12 branch discontinuity(ies) across the sweep - the solver is changing solution branch between adjacent keys. Check warm start and minimum-norm regularisation; MORE KEYS WILL NOT FIX THIS.
- Likely cause: base pose has insufficient joint headroom (worst margin to limit 0.0501). See BASE POSE HEALTH.

| Sample | Foot drift L/R | Foot yaw | Residual | Pelvis budget | Branch steps |
|---|---|---|---|---|---|
| weightShift +10% | 1.315 mm / 0.547 mm | 0.178 deg | 0.365 mm | x1.000 | 2 |
| weightShift +20% | 1.267 mm / 0.481 mm | 0.172 deg | 0.330 mm | x1.000 | 4 |
| chestAmplitude +10% | 1.239 mm / 0.658 mm | 0.198 deg | 0.317 mm | x1.000 | 2 |
| chestAmplitude -10% | 1.225 mm / 0.665 mm | 0.195 deg | 0.314 mm | x1.000 | 2 |
| pelvisResponseCm -10% | 1.050 mm / 0.654 mm | 0.200 deg | 0.361 mm | x1.000 | 1 |
| pelvisResponseCm -20% | 1.005 mm / 0.642 mm | 0.213 deg | 0.380 mm | x1.000 | 0 |
| pelvisResponseCm +20% | 1.002 mm / 0.617 mm | 0.198 deg | 0.392 mm | x0.550 | 0 |
| breathing -20% | 0.988 mm / 0.523 mm | 0.183 deg | 0.366 mm | x1.000 | 0 |
| pelvisResponseCm +10% | 0.975 mm / 0.622 mm | 0.189 deg | 4.372 mm | x1.000 | 0 |
| breathing -10% | 0.954 mm / 0.572 mm | 0.201 deg | 0.392 mm | x1.000 | 1 |
| breathing +20% | 0.948 mm / 0.738 mm | 0.214 deg | 0.392 mm | x0.550 | 0 |
| breathing +10% | 0.947 mm / 0.699 mm | 0.202 deg | 0.370 mm | x0.550 | 0 |
| weightShift -20% | 0.843 mm / 0.621 mm | 0.195 deg | 0.397 mm | x0.550 | 0 |
| awareness +10% | 0.795 mm / 0.626 mm | 0.181 deg | 0.383 mm | x0.550 | 0 |
| awareness -20% | 0.782 mm / 0.622 mm | 0.181 deg | 0.383 mm | x0.550 | 0 |
| chestAmplitude -20% | 0.762 mm / 0.625 mm | 0.180 deg | 0.384 mm | x0.550 | 0 |
| chestAmplitude +20% | 0.756 mm / 0.624 mm | 0.181 deg | 0.384 mm | x0.550 | 0 |
| nominal | 0.745 mm / 0.623 mm | 0.181 deg | 0.382 mm | x0.550 | 0 |
| awareness -10% | 0.717 mm / 0.622 mm | 0.181 deg | 0.385 mm | x0.550 | 0 |
| awareness +20% | 0.707 mm / 0.620 mm | 0.183 deg | 0.380 mm | x0.550 | 0 |
| weightShift -10% | 0.695 mm / 0.622 mm | 0.179 deg | 0.394 mm | x0.550 | 0 |

We want a stable solution NEIGHBOURHOOD, not one configuration that happens to
pass - art direction will move these parameters.

## TECHNICAL ANIMATION SCORE

**92/100** - this is a *technical animation* score, not an overall
quality score. It measures curve and constraint correctness only.

| Bucket | Points | Max |
|---|---|---|
| Loop | 20.0 | 20 |
| Foot stability | 16.1 | 20 |
| Root stability | 10.0 | 10 |
| Pose preservation | 15.0 | 15 |
| Weapon stability | 11.3 | 15 |
| Curve quality | 10.0 | 10 |
| Humanoid safety | 10.0 | 10 |

**WeaponHandStabilityScore: 75/100**

> Loop rotation continuity is below the float resolution of Quaternion.Angle.
> That means *indistinguishable*, not provably identical - do not read it as exact.

## Measurements

| Check | Value | |
|---|---|---|
| Loop position continuity | < 0.001 mm | PASS |
| Loop rotation continuity | < 0.001 deg | PASS |
| Loop velocity continuity | 0.89 mm/s (LeftUpperArm) | PASS |
| Root translation | < 0.001 mm | PASS |
| Root yaw | < 0.001 deg | PASS |
| Left foot drift (horizontal) | 0.745 mm | WARN |
| Right foot drift (horizontal) | 0.623 mm | WARN |
| Foot yaw range | L 0.18 / R 0.13 deg | WARN |
| Toe vertical range | 3.13 mm | WARN |
| Pelvis vertical range | 4.76 mm | PASS |
| Pelvis horizontal excursion | 2.09 mm | PASS |
| Head max angular deviation | 0.54 deg | PASS |
| Head positional travel | 30.44 mm | FAIL |
| Chest max angular deviation | 2.17 deg | PASS |
| Sword-hand position variance (RMS) | 0.92 mm | PASS |
| Sword-hand rotation variance (RMS) | 0.79 deg | WARN |
| Sword-hand path length | 10.86 mm | PASS |
| Sword-hand max speed | 10.9 mm/s | PASS |
| Off-hand path length | 107.67 mm | PASS - reference - the off hand SHOULD move more |
| Max bone angular velocity | 7.5 deg/s (LeftLowerArm) | PASS |
| Solver branch steps | 0 | PASS |
| Humanoid muscle limit violations | 0 | PASS |
| Curve tangent discontinuities | 0 | PASS |
| Animated curves / total keys | 42 / 1128 | PASS - sparse authoring |

## Failure-mode sweep

- **Dancing Feet** - measurable foot travel or yaw. The feet must be planted.

## Generation

- Foot-lock solve worst residual: 0.382 mm
- Curves: 42 animated, 60 holding the approved pose
- Keys per animated curve: 24 (2 refinement pass(es); worst between-key foot drift 0.565 mm)
- Motion budget clamps:
  - Pelvis translation scaled x0.550 so the feet stay planted (feet outrank the pelvis).

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

First production candidate from the approved 1H sword art-direction reference sheet, authored on PlayKit/Player.
