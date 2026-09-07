# Sword1H_CombatIdle_v001 - Validation Report

Generated 2026-08-26 01:02:51Z | duration 3.20 s

## Technical score: **97/100**

> A technical score is not artistic approval. A clip can score 100 and still look terrible.

| Bucket | Points | Max |
|---|---|---|
| Loop | 20.0 | 20 |
| Foot stability | 20.0 | 20 |
| Root stability | 10.0 | 10 |
| Pose preservation | 15.0 | 15 |
| Weapon stability | 12.1 | 15 |
| Curve quality | 10.0 | 10 |
| Humanoid safety | 10.0 | 10 |

**WeaponHandStabilityScore: 81/100**

## Measurements

| Check | Value | |
|---|---|---|
| Loop position continuity | 0.000 mm | PASS |
| Loop rotation continuity | 0.000 deg | PASS |
| Loop velocity continuity | 2.34 mm/s (RightUpperArm) | PASS |
| Root translation | 0.000 mm | PASS |
| Root yaw | 0.000 deg | PASS |
| Left foot drift (horizontal) | 0.270 mm | PASS |
| Right foot drift (horizontal) | 0.234 mm | PASS |
| Foot yaw range | L 0.09 / R 0.06 deg | PASS |
| Toe vertical range | 0.69 mm | PASS |
| Pelvis vertical range | 0.65 mm | PASS |
| Pelvis horizontal excursion | 0.25 mm | PASS |
| Head max angular deviation | 0.60 deg | PASS |
| Head positional travel | 24.79 mm | WARN |
| Chest max angular deviation | 2.28 deg | PASS |
| Sword-hand position variance (RMS) | 1.17 mm | PASS |
| Sword-hand rotation variance (RMS) | 0.36 deg | PASS |
| Sword-hand path length | 10.74 mm | PASS |
| Sword-hand max speed | 30.1 mm/s | WARN |
| Off-hand path length | 86.00 mm | PASS - reference - the off hand SHOULD move more |
| Max bone angular velocity | 8.0 deg/s (RightShoulder) | PASS |
| Humanoid muscle limit violations | 1 | WARN - 1 inherited from the APPROVED POSE - fix the pose, not the clip |
| Curve tangent discontinuities | 0 | PASS |
| Animated curves / total keys | 30 / 1044 | PASS - sparse authoring |

## Failure-mode sweep

None of the known idle failure modes were detected.

## Generation

- Foot-lock solve worst residual: 0.357 mm
- Curves: 30 animated, 72 holding the approved pose
- Keys per animated curve: 30 (2 refinement pass(es); worst between-key foot drift 0.696 mm)
- Approved-pose warnings (properties of the pose, not clip defects):
  - `Left Lower Leg Stretch` = 0.995 - this knee is locked straight. A combat stance wants softly bent knees; a locked knee leaves the leg nothing to absorb pelvis motion with, and the idle gets fragile.
  - `Right Lower Leg Stretch` = 1.005 is OUTSIDE the humanoid limit. The pose itself is out of range.
- Motion budget clamps:
  - Pelvis translation scaled x0.166 so the feet stay planted (feet outrank the pelvis).

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
