# UPPER-BODY WHOLE-CYCLE FOUNDATION QUALIFIED

No production v012. Weapon and shoulder deliberately untouched. Lower body frozen and verified
isolated. Clean milestone boundary as requested.

## ARCHITECTURE

The upper-body correction is now projected onto a **cyclic cubic B-spline basis** before it is
applied - the same representation that made the legs smooth. Each iteration proposes a per-key
(pitch, roll) correction against an absolute orientation target, fits the smallest band-limited
curve through those proposals, and applies a damped step of it. A band-limited cyclic signal cannot
contain a one-frame spike, so the jitter is excluded by the representation rather than tuned away,
and the loop seam is C2 by construction.

The absolute target itself is unchanged from the rejected experiment, because it was the only
formulation that ever preserved the sagittal posture: the deviation from the approved v005 chest is
split about the vertical, the intentional blade TWIST is carried through untouched, and only the
SWING is authored (sagittal intent + breath + a restrained fraction of the measured pelvis roll).
One coherent quaternion; no separate pitch/roll/yaw passes.

## LOWER-BODY ISOLATION — WHY IT CONTAMINATED, AND WHY IT NO LONGER DOES

Two mechanisms, one of them mine:

1. **Unavoidable and correct**: upper-body muscles move Unity's mass-weighted body frame, so the
   feet genuinely drift if nothing answers it. `ResolveLowerBody` answers it by moving that 6-DOF
   frame alone - it writes `body`/`bodyRot` and never touches a leg muscle.
2. **The actual bug**: the rejected pass ended each iteration with `RePin`, which routes to the full
   whole-cycle LEG solve and rewrites leg muscles. A torso edit therefore re-solved the legs. That
   is what moved the right ankle 26.1 mm and the right toe 27.7 mm off a frozen baseline.

The pass now runs LAST, after the qualified lower body is final, and only ever calls
`ResolveLowerBody` between iterations.

| max path deviation from canonical baseline | pelvis | L ank | R ank | L toe | R toe | L knee | R knee |
|---|---|---|---|---|---|---|---|
| rejected per-key pass | 6.1 | 19.7 | **26.1** | 21.6 | **27.7** | 10.8 | 14.6 |
| **cp12 (this milestone)** | **1.1** | **3.6** | **3.0** | **3.7** | **3.2** | **2.4** | **1.7** |

Every value is under the 5 mm flag threshold.

## BASIS COMPLEXITY STUDY

| control points | chest roll | head roll | head world tilt | chest frame step | head frame step | head Q-dev |
|---|---|---|---|---|---|---|
| baseline (no correction) | 20.0 | 22.9 | 13.3 | 6.11 | 6.48 | 14.3 |
| 6 | 13.8 | 5.5 | 6.2 | 5.92 | 3.97 | 2.5 |
| 8 | 16.3 | 5.4 | 7.9 | 5.82 | 4.22 | 1.8 |
| **12** | **15.4** | **3.6** | **5.2** | **5.85** | **3.00** | **1.2** |
| rejected per-key | 27.1 | 25.0 | 11.1 | **20.02** | **20.39** | 26.8 |

All three basis sizes behave coherently - a stable region, not a lucky point. 12 chosen: best head
result with chest continuity indistinguishable from 6 and 8.

## CHEST

| | roll range | frame step | quaternion deviation | sagittal pitch |
|---|---|---|---|---|
| baseline | 20.0 | 6.11 | 14.5 | +2.5 |
| **cp12** | **15.4** | **5.85** | 14.9 | **+2.3** |

Roll amplification reduced against a pelvis of 10.1 deg, and the frame step is slightly BETTER than
the uncorrected baseline. Sagittal posture held - the hard gate that killed two previous attempts.

## HEAD

| | local roll | world tilt | frame step | quaternion deviation | sagittal pitch |
|---|---|---|---|---|---|
| baseline | 22.9 | 13.3 | 6.48 | 14.3 | -2.5 |
| **cp12** | **3.6** | **5.2** | **3.00** | **1.2** | -0.1 |

Head world tilt 13.3 -> 5.2 deg, inside the requested "smooth 5-8 deg" band, with the frame step
more than halved. The head is now genuinely stabilised rather than riding the roll, and it got
there by moving LESS per frame, not more - the opposite of the gyroscopic snapping the rejected
version produced.

## JITTER / FREQUENCY

Worst upper-body rotational events, per 60 Hz frame: chest 5.85 deg, head 3.00 deg. Both below the
uncorrected baseline (6.11 / 6.48) and far below the 8 deg working limit. Nothing here should be
perceptible at gameplay distance - the correction lives at the gait fundamental and its low
harmonics by construction, because a 12-control-point cyclic basis over a 0.565 s cycle cannot
represent anything faster.

The rejected version's 20 deg spikes were exactly the high-frequency content this representation
cannot express; they are gone, not reduced.

## LOWER BODY / FEET / SUPPORT

| | L plant a/t/yaw | R plant a/t/yaw | knee L/R | shelf | >0.99 |
|---|---|---|---|---|---|
| baseline | 11.3 / 10.1 / 2.4 | 9.9 / 6.6 / 2.8 | 9.6 / 12.0 | 17 ms | 4.2% |
| **cp12** | **11.2 / 10.1 / 2.4** | **9.6 / 5.1 / 2.8** | **9.6 / 12.0** | **17 ms** | **4.2%** |

All planting gates pass. Semantic support L1/R1 unchanged. Timing 1.30 m/s / 150 spm untouched.

## SHOULDER — INTENTIONALLY UNCHANGED

Clavicle deviation 18.9 deg (baseline 18.8), shoulder muscle still saturated at -1.000, sword path
181 mm (baseline 173). The weapon target, translation follow, chest-space targeting and shoulder
anchor were all left disabled for this milestone, exactly as instructed. This is experimental
isolation, not approval.

## TECHNICAL

Interpolated Humanoid legality PASS. Loop seam 0.00000 with 0 tangent mismatches. Live profile
restored from the frozen canonical baseline and the zero-authority invariant re-verified: with the
upper body disabled, generation reproduces the canonical probe bit-exactly.

Regression suite: **26 passed / 0 failed / 0 skipped**. O1, O2, O3 PASS.
New: `O4_UpperBodySolve_StaysBandLimited` fails if chest or head exceeds 8 deg in one 60 Hz frame -
the specific regression that killed the previous attempt.

## FROZEN

`Assets/Bravehood/Animation/Profiles/UpperBodyWholeCycleFoundation.asset` - the canonical baseline
plus `upperBodyAuthority 1.0`, `upperBodyControlPoints 12`, `upperBodyIterations 6`,
`upperBodyDamping 0.6`, `chestRollFollow 0.25`, `headRollFollow 0.10`, `headPitchFollowDeg 0`.
Evidence clip kept at `__wc_cp12.anim`.

The live profile remains canonical (upper body OFF), so nothing about current production changed.

`UPPER-BODY WHOLE-CYCLE FOUNDATION QUALIFIED`
