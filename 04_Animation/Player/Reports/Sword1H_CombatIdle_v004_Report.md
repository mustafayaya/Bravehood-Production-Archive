# Sword1H_CombatIdle_v004 - Validation Report

Generated 2026-08-26 02:09:43Z | duration 3.20 s

## QUALITY LAYERS

```
BASE POSE HEALTH        PASS
SOLVER ROBUSTNESS       WARN
TECHNICAL VALIDATION    81 / 100
ARTISTIC APPROVAL       APPROVED  (Mustafa, 2026-08-26)
```

These are independent. A high technical score says the curves are clean; it says
nothing about whether the pose was fit to animate, whether the solution is stable
under art direction, or whether the idle looks any good.

## BASE POSE HEALTH

**PASS**  (Sword1H_CombatIdle_ApprovedPose_Aligned)

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
| Worst foot drift | 1.529 mm |
| Worst foot yaw | 0.221 deg |
| Worst solver residual | 0.397 mm |
| Worst muscle margin to limit | 0.0543 |
| Branch discontinuities | 4 |
| Sweep time | 3.0 s |

- Nominal animation is valid but fragile. `breathing +10%`: foot drift 1.529 mm.
- 4 branch discontinuity(ies) across the sweep - the solver is changing solution branch between adjacent keys. Check warm start and minimum-norm regularisation; MORE KEYS WILL NOT FIX THIS.
- Likely cause: base pose has insufficient joint headroom (worst margin to limit 0.0543). See BASE POSE HEALTH.

| Sample | Foot drift L/R | Foot yaw | Residual | Pelvis budget | Branch steps |
|---|---|---|---|---|---|
| breathing +10% | 1.529 mm / 0.769 mm | 0.215 deg | 0.366 mm | x1.000 | 2 |
| weightShift +10% | 1.509 mm / 0.672 mm | 0.200 deg | 0.395 mm | x1.000 | 0 |
| chestAmplitude -10% | 1.496 mm / 0.673 mm | 0.193 deg | 0.382 mm | x1.000 | 0 |
| awareness +20% | 1.495 mm / 0.674 mm | 0.194 deg | 0.388 mm | x1.000 | 0 |
| chestAmplitude +20% | 1.491 mm / 0.678 mm | 0.195 deg | 0.394 mm | x1.000 | 0 |
| awareness -10% | 1.491 mm / 0.675 mm | 0.194 deg | 0.391 mm | x1.000 | 0 |
| pelvisResponseCm +10% | 1.483 mm / 0.690 mm | 0.193 deg | 0.396 mm | x1.000 | 2 |
| chestAmplitude -20% | 1.472 mm / 0.675 mm | 0.191 deg | 0.387 mm | x1.000 | 0 |
| weightShift -20% | 1.468 mm / 0.674 mm | 0.196 deg | 0.391 mm | x1.000 | 0 |
| nominal | 1.453 mm / 0.673 mm | 0.195 deg | 0.380 mm | x1.000 | 0 |
| awareness +10% | 1.452 mm / 0.674 mm | 0.192 deg | 0.381 mm | x1.000 | 0 |
| weightShift -10% | 1.414 mm / 0.676 mm | 0.193 deg | 0.379 mm | x1.000 | 0 |
| breathing -20% | 1.392 mm / 0.480 mm | 0.204 deg | 0.326 mm | x1.000 | 0 |
| awareness -20% | 1.386 mm / 0.676 mm | 0.190 deg | 0.395 mm | x1.000 | 0 |
| weightShift +20% | 1.378 mm / 0.678 mm | 0.194 deg | 0.397 mm | x1.000 | 0 |
| chestAmplitude +10% | 1.369 mm / 0.673 mm | 0.195 deg | 0.390 mm | x1.000 | 0 |
| pelvisResponseCm -20% | 1.351 mm / 0.610 mm | 0.211 deg | 0.318 mm | x1.000 | 0 |
| breathing +20% | 1.330 mm / 0.647 mm | 0.221 deg | 0.378 mm | x0.550 | 0 |
| pelvisResponseCm -10% | 1.248 mm / 0.643 mm | 0.199 deg | 0.388 mm | x1.000 | 0 |
| breathing -10% | 1.171 mm / 0.561 mm | 0.190 deg | 0.383 mm | x1.000 | 0 |
| pelvisResponseCm +20% | 0.969 mm / 0.725 mm | 0.164 deg | 0.370 mm | x0.550 | 0 |

We want a stable solution NEIGHBOURHOOD, not one configuration that happens to
pass - art direction will move these parameters.

## TECHNICAL ANIMATION SCORE

**81/100** - this is a *technical animation* score, not an overall
quality score. It measures curve and constraint correctness only.

| Bucket | Points | Max |
|---|---|---|
| Loop | 20.0 | 20 |
| Foot stability | 8.1 | 20 |
| Root stability | 10.0 | 10 |
| Pose preservation | 11.7 | 15 |
| Weapon stability | 11.0 | 15 |
| Curve quality | 10.0 | 10 |
| Humanoid safety | 10.0 | 10 |

**WeaponHandStabilityScore: 73/100**

> Loop rotation continuity is below the float resolution of Quaternion.Angle.
> That means *indistinguishable*, not provably identical - do not read it as exact.

## Measurements

| Check | Value | |
|---|---|---|
| Loop position continuity | 0.001 mm | PASS |
| Loop rotation continuity | < 0.001 deg | PASS |
| Loop velocity continuity | 1.12 mm/s (RightFoot) | PASS |
| Root translation | < 0.001 mm | PASS |
| Root yaw | < 0.001 deg | PASS |
| Left foot drift (horizontal) | 1.453 mm | WARN |
| Right foot drift (horizontal) | 0.673 mm | WARN |
| Foot yaw range | L 0.20 / R 0.19 deg | WARN |
| Toe vertical range | 5.28 mm | FAIL |
| Pelvis vertical range | 7.60 mm | WARN |
| Pelvis horizontal excursion | 2.05 mm | PASS |
| Head max angular deviation | 0.78 deg | PASS |
| Head positional travel | 35.91 mm | FAIL |
| Chest max angular deviation | 2.32 deg | PASS |
| Sword-hand position variance (RMS) | 1.00 mm | PASS |
| Sword-hand rotation variance (RMS) | 0.87 deg | WARN |
| Sword-hand path length | 11.09 mm | PASS |
| Sword-hand max speed | 14.1 mm/s | PASS |
| Off-hand path length | 112.67 mm | PASS - reference - the off hand SHOULD move more |
| Max bone angular velocity | 16.0 deg/s (RightUpperLeg) | WARN |
| Solver branch steps | 0 | PASS |
| Humanoid muscle limit violations | 0 | PASS |
| Curve tangent discontinuities | 0 | PASS |
| Animated curves / total keys | 42 / 1086 | PASS - sparse authoring |

## Failure-mode sweep

- **Dancing Feet** - measurable foot travel or yaw. The feet must be planted.

## Generation

- Foot-lock solve worst residual: 0.380 mm
- Curves: 42 animated, 60 holding the approved pose
- Keys per animated curve: 23 (3 refinement pass(es); worst between-key foot drift 0.784 mm)

## ARTISTIC APPROVAL: APPROVED

**Approved by Mustafa on 2026-08-26**, reviewed on the real player through the Bravehood
gameplay camera after the directional correction. This clip is the production 1H sword
combat idle and is installed in `Knight_Controller` -> `Base Layer/Idle`.

The review questions below were the basis of that sign-off; they are kept for the record.

## ARTISTIC REVIEW (completed)

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

v004 = v003 with an ISOLATED directional correction only: HumanPose.bodyRotation yawed +66.0 deg so the combat stance is bladed relative to Player Forward (root +Z), and Head/Neck Turn re-aimed so the gaze lands on Player Forward. Profile identical to v003.
