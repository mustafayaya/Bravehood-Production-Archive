# Sword1H_WalkForward_v009 — NOT GENERATED (gate 28)

Timing held at the locked 1.30 m/s / 150 spm / 0.800 s world cycle / 520 mm step throughout.
No production clip created. No rig, Travel, controller, prefab or speed change.
Previous assets byte-identical (v008 `7a7ca074…`). Regression suite 14/14.

## Defect #2 — sagittal posture drift: DIAGNOSED AND FIXED

`LocomotionPostureDriftAudit` added as a permanent diagnostic (item 5), measuring pelvis /
lower-spine / chest / neck / head pitch, chest-over-pelvis and pelvis-over-support from
reconstructed bone transforms in the stance frame, against `Sword1H_CombatIdle_v005`.

What it found, and why nothing had caught it:

| deltas vs idle | pelvis | l.spine | chest | neck | head | chest>pelvis |
|---|---|---|---|---|---|---|
| v008 | +14.6 | +15.1 | +14.7 | +14.6 | +14.7 | +23 mm |
| B_s0.40 | +14.2 | +14.9 | +14.4 | +14.3 | +14.4 | +23 mm |
| mocap source | +15.0 | +15.5 | **+4.8** | **+2.9** | **+8.7** | +24 mm |

The five segments drift by the SAME amount, which is the signature of a whole-body tilt rather
than a hinge. Mechanism: the walk correctly inherits the source's forward body-frame tilt (the
pelvis must face travel), but the upper body keeps the APPROVED POSE's muscle values - and those
were authored against an untilted body. Muscle channels are joint-local, so the torso faithfully
reproduces the idle's joint angles ON TOP OF a tilted frame and rides the tilt instead of
recovering from it. The source does recover, through its upper spine, to +4.8. We do not.
Chest yaw, sword path and head yaw are all yaw measures, which is why every previous report
called the upper body "preserved".

Fix: `SagittalPosturePass` drives the chest's ABSOLUTE sagittal pitch toward
idle + mean intent + a phase-shaped breath (deepest at Down, tall at Passing, twice per cycle),
distributed across spine/chest/upper-chest, solved by Newton; neck and head recovered after.

Measured at `sagittalPostureAuthority 0.5` on the B_s0.40 recipe:

| | chest | head | chest>pelvis | knee frame step | ankle L/R | sword | gate |
|---|---|---|---|---|---|---|---|
| off | +14.4 | +14.4 | +23 mm | 29.5 / 22.2 | 6.5 / 12.5 | 258 | PASS |
| **0.5** | **+2.6** | **-2.2** | **+17 mm** | 29.7 / 22.5 | 7.6 / 14.4 | 239 | **PASS** |
| 1.0 | +14.6 | +18.2 | +23 mm | 29.7 / 22.5 | 6.4 / 19.0 | 273 | FAIL |

Authority 1.0 diverges (the pass runs twice and re-solves); 0.5 is effectively a damped Newton
and is the working setting. The fix is ready and costs essentially nothing.

## Defect #1 — temporal continuity: ARCHITECTURE WORKS, BLOCKED ON CONTACT

Built the whole-cycle solve asked for in items 8/10/11/12. The correction is no longer solved
per key and smoothed afterwards - post-hoc smoothing PROJECTS THE SOLUTION OUT OF THE FEASIBLE
SET, which is exactly why planting failed off a cliff between sigma 0.40 (6 mm) and 0.50 (41 mm).
Instead the correction is restricted to a cyclic cubic B-spline space and the foot constraint is
solved INSIDE it by damped Gauss-Newton, coarse-to-fine, with support authority entering as the
least-squares WEIGHT (never as a multiplier on a converged pose - item 12).

Continuity result, and it exceeds the target by a wide margin:

| | knee frame step L/R | knee p95 accel | shelf | >0.99 | sword |
|---|---|---|---|---|---|
| B_s0.40 (current best) | 29.5 / 22.2 | 46,823 | 150 ms | 36% | 258 |
| source @150 spm | 11.5 / 8.5 | 10,549 | - | - | - |
| **whole-cycle** | **8.8 / 11.3** | **9,387** | **17 ms** | **5%** | **255** |

That is past the 15-22 deg goal and better than the timing-matched mocap source, with the knee
shelf essentially gone and the sword path back on budget.

**Blocker: the left foot's contact collapses.** Independent confirmation from
`HumanoidLocomotionAnalyzer` (not the study harness):

| | foot | support % | plant drift max | yaw drift |
|---|---|---|---|---|
| B_s0.40 | Left | 14% | 21.0 mm | 5.3 deg |
| B_s0.40 | Right | 44% | 34.8 mm | 1.9 deg |
| whole-cycle | Left | **62-64%** | **174-188 mm** | **32-35 deg** |
| whole-cycle | Right | 44% | 27.9-29.2 mm | 8.8-9.1 deg |

The right foot is fine - actually better planted than B_s0.40. The left is detected as planted
for two thirds of the cycle and drifts 174 mm while rotating 35 deg, which would read as a glued,
skating, twisting left foot. Production gates (ankle/toe <15 mm, yaw <3 deg) fail.

Ruled out, each by measurement, none of which moved the ~25 mm floor:
- bandwidth (P14 -> P40, i.e. up to full per-key resolution)
- step damping (0.3 / 0.5 / 0.7)
- swing weight floor (0.02 / 0.05 / 0.10 / 0.25)
- warm start on/off
- iterations (8 -> 24)
- band-limiting the body too (diverges: 30% flight, 92-146 mm residual - kept behind a flag)
- Fourier vs B-spline basis (Fourier rings globally; B-spline is strictly better but not enough)
- re-pinning at the finest level instead of restarting coarse-to-fine (fixed the upper body -
  sword 390 -> 255, chest yaw 44 -> 26 - but not the left foot)

The convergence trace never decays; it wanders. Combined with the left/right asymmetry, the
evidence points at the left foot's contact SCHEDULE, not the smooth solve: the left already had
the degenerate topology (one true stance plus shallow grazes) that has followed this rig since
v005, and the whole-cycle solve, which reads support as a continuous weight, merges those grazes
into one 62% pseudo-stance and then dutifully tries to plant the foot for all of it.

## Recommendation

Next step is the contact schedule, not the solver: give the whole-cycle solve an authored,
single, unambiguous stance window per foot rather than one derived from a sole-height envelope
that the left foot's grazes corrupt. The temporal architecture is done and measured; it is being
fed a bad support signal.

Held back for your call: the posture fix alone is a validated, gate-passing improvement to
B_s0.40. Gate 28 is conjunctive, so I did not ship it as v009.

**ARTISTIC APPROVAL: PENDING**
