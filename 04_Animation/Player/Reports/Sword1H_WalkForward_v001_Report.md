# Sword1H_WalkForward_v001 - Locomotion Validation Report

```
BASE / POSTURE HEALTH    PASS   (approved combat pose, Sword1H_CombatIdle_v004)
SOLVER ROBUSTNESS        WARN   (stride-scale sweep 1.10/1.20/1.30 - see below)
TECHNICAL VALIDATION     see table
ARTISTIC APPROVAL        PENDING
```

## Architecture

**IN-PLACE.** `CharacterAnimator.OnAnimatorMove` drops position deltas on purpose and the
KinematicCharacterMotor owns the transform; only turn-clip YAW is forwarded. All root
channels are baked into pose. Speed is reconciled by the project's existing stride matcher.

| Check | Value | |
|---|---|---|
| Loop position continuity | < 0.001 mm | PASS |
| Loop rotation continuity | < 0.001 deg | PASS |
| Loop velocity continuity | 36.20 mm/s | PASS |
| Branch discontinuities | 0 | PASS |
| Curve discontinuities | 0 | PASS |
| Humanoid limit violations | 0 | PASS |
| Left foot plant drift (max/avg) | 48.481 mm / 24.607 mm | WARN |
| Right foot plant drift (max/avg) | 75.803 mm / 36.112 mm | WARN |
| Foot yaw drift L/R | 12.6 / 14.8 deg | WARN |
| Foot penetration / hover | < 0.001 mm / 29 mm | PASS |
| Step length L/R | 0.903 / 0.903 m | PASS |
| Stride length / asymmetry | 1.503 m / 0.0 % | PASS |
| Stride width mean (min-max) | 36.7 cm (18.9-54.8) | PASS |
| Pelvis vertical excursion | 56.3 mm | WARN |
| Pelvis lateral excursion | 21.8 mm | PASS |
| Controller speed | 2.000 m/s | - |
| Animation native speed | 1.937 m/s | - |
| Playback multiplier (MotionSpeed) | 1.031 | - |
| Effective cadence vs controller | 1.997 m/s (-0.1 %) | PASS |
| Travel heading vs Player Forward | +2.5 deg | PASS |
| Sword-hand path / RMS | 241 / 9.5 mm | PASS |
| Blade orientation range | 9.4 deg | PASS |
| Head forward alignment mean/max | -0.01 / 0.29 deg | PASS |

### Gait measurements - Sword1H_WalkForward_v001

| | |
|---|---|
| Cycle | 0.776 s |
| Native speed | 1.937 m/s |
| Native heading vs Player Forward | +2.5 deg |
| Root motion in clip | no (in-place) |
| Step length L / R | 0.903 / 0.903 m |
| Stride length | 1.503 m |
| Stride asymmetry | 0.0 % |
| Stride width mean / min / max | 36.7 / 18.9 / 54.8 cm |
| Pelvis vertical / lateral | 56.3 / 21.8 mm |
| Pelvis yaw range | 18.8 deg |
| Head alignment mean / max | +0.0 / 0.3 deg |
| Head vertical bob | 60.7 mm |
| Sword hand path / RMS | 241.4 / 9.5 mm |
| Blade orientation range | 9.4 deg |

| Foot | Contact | Toe-off | Support | Plant drift max / avg | Yaw drift | Penetration | Hover | Swing clearance |
|---|---|---|---|---|---|---|---|---|
| Left | 0.817 | 0.125 | 32 % | 48.481 mm / 24.607 mm | 12.600 deg | < 0.001 mm | 29.282 mm | 141 mm |
| Right | 0.308 | 0.667 | 36 % | 75.803 mm / 36.112 mm | 14.842 deg | < 0.001 mm | 29.044 mm | 109 mm |


## Reference comparison

| | current combat walk | source gait | **v001** |
|---|---|---|---|
| Travel heading | +40.5 deg | -3.8 deg | **+2.5 deg** |
| Stride width range | 85.0 cm | 31.9 cm | **35.9 cm** |
| Plant drift L/R | 208 / 163 mm | 37 / 73 mm | **48 / 76 mm** |
| Head alignment | +4.1 deg | +0.1 deg | **+0.0 deg** |
| Sword-hand path | 1109 mm | 1403 mm | **241 mm** |

## Robustness (stride-scale sweep, all other parameters fixed)

| strideScale | stride | cycle | plant drift L/R | pelvis vert | head |
|---|---|---|---|---|---|
| 1.10 | 1.401 m | 0.723 s | 54 / 67 mm | 47 mm | +0.0 deg |
| **1.20 (shipped)** | **1.503 m** | **0.776 s** | **48 / 76 mm** | **56 mm** | **+0.0 deg** |
| 1.30 | 1.581 m | 0.816 s | 54 / 81 mm | 67 mm | +0.0 deg |

Head alignment, travel heading and speed match are flat across the sweep. Plant drift and
pelvis bounce move with stride, as expected. WARN, not PASS, because plant drift sits above
the gate this skill set for itself (15 mm) - see Remaining risks.

ARTISTIC APPROVAL: **PENDING**
