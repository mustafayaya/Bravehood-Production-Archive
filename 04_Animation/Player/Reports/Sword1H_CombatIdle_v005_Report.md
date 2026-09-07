# Sword1H_CombatIdle_v005 - Validation Report

> **ARTISTICALLY APPROVED by Mustafa, 2026-08-26.** This is the production 1H sword combat idle.
> Base pose: `PoseReferences/Sword1H_CombatIdle_ApprovedPose_v2.asset`.

Generated 2026-08-26 12:02:09Z | duration 3.20 s

## QUALITY LAYERS

```
BASE POSE HEALTH        PASS
SOLVER ROBUSTNESS       PASS
TECHNICAL VALIDATION    93 / 100
ARTISTIC APPROVAL       APPROVED (Mustafa, 2026-08-26)
```

These are independent. A high technical score says the curves are clean; it says
nothing about whether the pose was fit to animate, whether the solution is stable
under art direction, or whether the idle looks any good.

## BASE POSE HEALTH

**PASS**  (Sword1H_CombatIdle_ApprovedPose_v2)

- Every tracked joint has usable travel left; nothing is near its limit.

### Solver headroom

| Joint | Value | Margin to legal | Headroom |
|---|---|---|---|
| all tracked joints | - | healthy | HEALTHY |

Headroom is how much travel a joint has left. It is the reason a pose is fragile,
not just the fact that it is.

## SOLVER ROBUSTNESS

**PASS**

| | |
|---|---|
| Samples | 21 |
| Failed solves | 0 |
| Worst foot drift | 0.962 mm |
| Worst foot yaw | 0.321 deg |
| Worst solver residual | 0.399 mm |
| Worst muscle margin to limit | 0.2938 |
| Branch discontinuities | 0 |
| Sweep time | 3.6 s |

| Sample | Foot drift L/R | Foot yaw | Residual | Pelvis budget | Branch steps |
|---|---|---|---|---|---|
| breathing +20% | 0.962 mm / 0.663 mm | 0.321 deg | 0.387 mm | x1.000 | 0 |
| awareness -10% | 0.949 mm / 0.548 mm | 0.285 deg | 0.359 mm | x1.000 | 0 |
| breathing +10% | 0.937 mm / 0.630 mm | 0.274 deg | 0.393 mm | x1.000 | 0 |
| pelvisResponseCm +20% | 0.927 mm / 0.546 mm | 0.303 deg | 0.392 mm | x1.000 | 0 |
| pelvisResponseCm +10% | 0.845 mm / 0.554 mm | 0.301 deg | 0.399 mm | x1.000 | 0 |
| chestAmplitude +10% | 0.837 mm / 0.548 mm | 0.284 deg | 0.360 mm | x1.000 | 0 |
| chestAmplitude +20% | 0.811 mm / 0.551 mm | 0.288 deg | 0.360 mm | x1.000 | 0 |
| breathing -20% | 0.800 mm / 0.400 mm | 0.238 deg | 0.376 mm | x1.000 | 0 |
| breathing -10% | 0.797 mm / 0.464 mm | 0.259 deg | 0.377 mm | x1.000 | 0 |
| weightShift +10% | 0.772 mm / 0.549 mm | 0.287 deg | 0.360 mm | x1.000 | 0 |
| awareness +20% | 0.771 mm / 0.553 mm | 0.290 deg | 0.359 mm | x1.000 | 0 |
| weightShift +20% | 0.768 mm / 0.551 mm | 0.287 deg | 0.363 mm | x1.000 | 0 |
| weightShift -20% | 0.764 mm / 0.550 mm | 0.301 deg | 0.389 mm | x1.000 | 0 |
| chestAmplitude -10% | 0.763 mm / 0.550 mm | 0.292 deg | 0.359 mm | x1.000 | 0 |
| awareness +10% | 0.763 mm / 0.551 mm | 0.292 deg | 0.360 mm | x1.000 | 0 |
| chestAmplitude -20% | 0.761 mm / 0.551 mm | 0.296 deg | 0.359 mm | x1.000 | 0 |
| nominal | 0.758 mm / 0.549 mm | 0.297 deg | 0.361 mm | x1.000 | 0 |
| awareness -20% | 0.758 mm / 0.550 mm | 0.296 deg | 0.358 mm | x1.000 | 0 |
| weightShift -10% | 0.754 mm / 0.552 mm | 0.301 deg | 0.397 mm | x1.000 | 0 |
| pelvisResponseCm -10% | 0.745 mm / 0.547 mm | 0.265 deg | 0.378 mm | x1.000 | 0 |
| pelvisResponseCm -20% | 0.670 mm / 0.551 mm | 0.266 deg | 0.378 mm | x1.000 | 0 |

We want a stable solution NEIGHBOURHOOD, not one configuration that happens to
pass - art direction will move these parameters.

## TECHNICAL ANIMATION SCORE

**93/100** - this is a *technical animation* score, not an overall
quality score. It measures curve and constraint correctness only.

| Bucket | Points | Max |
|---|---|---|
| Loop | 20.0 | 20 |
| Foot stability | 16.6 | 20 |
| Root stability | 10.0 | 10 |
| Pose preservation | 15.0 | 15 |
| Weapon stability | 11.4 | 15 |
| Curve quality | 10.0 | 10 |
| Humanoid safety | 10.0 | 10 |

**WeaponHandStabilityScore: 76/100**

> Loop rotation continuity is below the float resolution of Quaternion.Angle.
> That means *indistinguishable*, not provably identical - do not read it as exact.

## Measurements

| Check | Value | |
|---|---|---|
| Loop position continuity | < 0.001 mm | PASS |
| Loop rotation continuity | < 0.001 deg | PASS |
| Loop velocity continuity | 0.84 mm/s (LeftFoot) | PASS |
| Root translation | < 0.001 mm | PASS |
| Root yaw | < 0.001 deg | PASS |
| Left foot drift (horizontal) | 0.758 mm | WARN |
| Right foot drift (horizontal) | 0.549 mm | WARN |
| Foot yaw range | L 0.30 / R 0.20 deg | WARN |
| Toe vertical range | 1.76 mm | WARN |
| Pelvis vertical range | 5.77 mm | PASS |
| Pelvis horizontal excursion | 2.12 mm | PASS |
| Head max angular deviation | 0.50 deg | PASS |
| Head positional travel | 27.83 mm | WARN |
| Chest max angular deviation | 2.17 deg | PASS |
| Sword-hand position variance (RMS) | 1.00 mm | PASS |
| Sword-hand rotation variance (RMS) | 0.76 deg | WARN |
| Sword-hand path length | 8.73 mm | PASS |
| Sword-hand max speed | 9.0 mm/s | PASS |
| Off-hand path length | 106.61 mm | PASS - reference - the off hand SHOULD move more |
| Max bone angular velocity | 8.0 deg/s (LeftLowerArm) | PASS |
| Solver branch steps | 0 | PASS |
| Humanoid muscle limit violations | 0 | PASS |
| Curve tangent discontinuities | 0 | PASS |
| Animated curves / total keys | 42 / 498 | PASS - sparse authoring |

## Failure-mode sweep

- **Dancing Feet** - measurable foot travel or yaw. The feet must be planted.

## Generation

- Foot-lock solve worst residual: 0.361 mm
- Curves: 42 animated, 60 holding the approved pose
- Keys per animated curve: 9 (0 refinement pass(es); worst between-key foot drift 0.480 mm)

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

From Sword1H_CombatIdle_ApprovedPose_v2: right knee 23.5 deg, 15.0 mm reach reserve.
