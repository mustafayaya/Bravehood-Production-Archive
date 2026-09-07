# Sword1H_WalkForward_v009 — GENERATED AND INSTALLED

Timing locked: 1.30 m/s, 150 spm, 0.800 s world cycle, 513 mm step, clip cycle 0.5652 s,
MotionSpeed 0.7065, 40 keys, stride 1026 mm, L/R stance-speed asymmetry 0.0%.

## SUPPORT SCHEDULE

New permanent architecture: `SemanticSupportSchedule`. Contact detection is now a diagnostic
only; solver authority comes from a semantic schedule. The discriminator the old signal lacked is
foot WORLD speed - a loaded foot is world-stationary, a grazing swing foot is moving fast - so
support requires low AND slow (a product, not a sum).

| foot | stance (phase) | dur | acquire | stable | release | swing | raw runs -> semantic |
|---|---|---|---|---|---|---|---|
| Left | 0.68 - 0.20 (wrapped) | 0.55 | 0.08 | 0.38 | 0.10 | 0.45 | 1 -> 1 |
| Right | 0.23 - 0.75 | 0.55 | 0.03 | 0.38 | 0.15 | 0.45 | 1 -> 1 |

Double support 2.5%, flight 5.0%. The detected core (0.38) is the exactly-planted region;
acquisition and release extend it to the authored 0.55, gated by the ramp speed limit so the
extension cannot re-admit a graze. Envelopes are quintic (C2 at both ends, including where they
reach zero).

## TEMPORAL

| | knee frame step L/R | knee p95 vel | knee p95 accel | shelf >=0.999 | >0.99 |
|---|---|---|---|---|---|
| B_s0.40 | 29.5 / 22.2 | 1537 / 1222 | 46,817 / 36,311 | 150 ms | 36% |
| whole-cycle (no schedule fix) | 8.8 / 11.3 | - | 9,387 | 17 ms | 5% |
| **v009** | **8.8 / 12.2** | 465 / 638 | **10,129 / 13,626** | **17 ms** | **3.5%** |

Top adjacent-frame lower-body changes, ranked across both clips - every v009 entry sits below
B_s0.40's third-worst:

| rank | clip | joint | deg | phase |
|---|---|---|---|---|
| 1 | B_s0.40 | L knee | 29.5 | 0.54 |
| 2 | B_s0.40 | R knee | 22.2 | 0.96 |
| 3 | B_s0.40 | L hip | 17.5 | 0.96 |
| 4 | **v009** | L ankle | 15.8 | 0.25 |
| 5 | B_s0.40 | R ankle | 15.4 | 0.21 |
| 6 | **v009** | R ankle | 13.3 | 0.21 |
| 8 | **v009** | R knee | 12.2 | 0.77 |
| 10 | **v009** | L hip | 10.9 | 0.25 |

## FEET

Measured over the semantic STABLE CORE - the region the schedule says is load-bearing. The same
window applied to the older clips for a fair comparison.

| clip | foot | ankle | toe | yaw |
|---|---|---|---|---|
| **v009** | L | **10.5** | **9.6** | **2.6** |
| **v009** | R | **11.2** | **9.8** | **2.0** |
| B_s0.40 | L | 54.8 | 43.6 | 5.8 |
| B_s0.40 | R | 21.6 | 6.2 | 1.6 |
| v008 | L | 63.2 | 49.9 | 6.3 |
| v008 | R | 18.4 | 9.3 | 2.0 |

Gates <15 mm / <15 mm / <3 deg: PASS both feet, in the 10-14 mm band asked for.
Over the FULL semantic window (core + ramps) drift is 104/124 mm and 18/35 deg - that is heel
strike and heel-rise/toe-off roll, which is correct gait, not slip.

Peak swing clearance L 64 mm / R 89 mm. Deepest sole penetration -5 mm (B_s0.40: -25 mm).

## POSTURE

| deltas vs idle v005 | pelvis | l.spine | chest | neck | head | chest>pelvis |
|---|---|---|---|---|---|---|
| B_s0.40 | +14.2 | +14.9 | +14.4 | +14.3 | +14.4 | +23 mm |
| **v009** | +14.9 | +11.4 | **+2.7** | **-0.1** | **-2.2** | **+17 mm** |
| source (recovers) | +15.0 | +15.5 | +4.8 | +2.9 | +8.7 | +24 mm |

The pelvis keeps participating in locomotion, as intended; the chest, neck and head no longer
ride its tilt. Chest breathes between -4.7 and +7.5 deg across the cycle rather than sitting
permanently folded.

## GAIT / UPPER BODY

Pelvis vertical 52.8 mm (B_s0.40 59.7), lateral 24.0 mm, stride asymmetry 0.0%, track 314 mm.
Sword path 165 mm, RMS 8.7 mm, blade range 11.5 deg, head alignment 0.03 deg mean / 0.29 max.

## TECHNICAL

Interpolated Humanoid legality PASS (no stored-key violation, nothing outside [-1,1] between
keys, no post-hoc clamping). Loop seam value gap 0.00000, tangent mismatches 0.
Regression suite 18/18 - four new cases cover wrapped cyclic stance, shallow swing graze,
threshold-split stance and genuine double contact.

## INSTALL

`Light_Walk8` forward -> v009. `Travel_Walk8` forward still v008, untouched.
`CombatWalkSpeedScale` 0.75 -> 0.65 (1.30 m/s against MaxStableMoveSpeed 2.0).
Controller backup: `Knight_Controller.controller.bak_pre_v009`.
v005-v008 and all WalkTiming assets byte-identical.

## REMAINING RISKS

1. **Left foot double-bounce (inherited).** The left sole traces two arcs per cycle (peaks 64 and
   58 mm) with a mid-swing dip to -5 mm, where the right foot has one clean arc peaking 89 mm.
   This is the same L3 topology the rig has carried since v005 - v009 reduces it (B_s0.40 dipped
   to -25 mm) but does not remove it. May read as a slight left-foot scuff. It lives in the source
   blend's foot targets, not the solver.
2. **Double support 2.5%, below the 8-15% sanity region**, with 5% flight. The two semantic
   windows are 0.45 apart rather than 0.50, so the overlap lands on one side only.
3. Chest now moves considerably more than B_s0.40 (p95 angular velocity 154 vs 22) - that is the
   intended posture breath replacing a locked torso, but it is a visible change.
4. Track width 314 mm vs 287 mm - slightly wider stance.

**ARTISTIC APPROVAL: PENDING**
