# WALKING COMBAT CARRY AUTHORITY BLOCKED — there is nothing to re-author, and the gap was never the cause

Two of my own earlier measurements were wrong and are corrected below. No v012. Production untouched.

## CORRECTION 1 — THE "PRE-SOLVE" MEASUREMENTS WERE POST-SOLVE

`SolveWeaponPass` is only skipped when the frame or arm architecture is enabled. Every clip I called
"pre-solve" last milestone still had it running, so the 83-91 mm gap and the 18.9 deg clavicle were
both measured on a hand the weapon solver had already moved.

Measured with the weapon solve genuinely disabled:

| | clavicle local | upper arm | forearm | hand | shoulder muscle |
|---|---|---|---|---|---|
| **true pre-solve** | **0.4 / 0.9 deg** | 0.4 | 0.2 | 0.1 | **+0.069 .. +0.118** |
| wc_cp12 | 18.9 / 21.0 | 12.5 | 11.0 | 0.1 | -1.000 |
| wf75 | 18.4 / 19.5 | 12.6 | 9.9 | 0.1 | -1.000 |

## CORRECTION 2 — ITEM 11 ANSWERED: A, NOT B

The ~18.9 deg is **true local clavicle depression**, not an artefact of comparing against an idle
parent frame. With the solver off it is 0.4 deg. The torso re-authoring contributes essentially
nothing; the weapon solve contributes all of it. My previous report suggested the opposite and was
wrong.

## THE MILESTONE'S PREMISE DOES NOT HOLD

The brief's model was that the idle carry, transplanted onto a re-authored walking torso, sits far
from where the arm naturally is - so a carry authored *for the walking torso* would close the gap.

Measured: **on the qualified walking torso, with no weapon correction, the arm already sits 0.4 deg
from the approved combat carry.** The Blend plus the torso foundation reproduce the approved arm
almost exactly. There is no displaced arm to re-author, and authoring one could only move it away
from a relationship that is already correct.

True pre-solve hand gap to the existing target:

| weapon-frame follow | gap mean | gap max | rotation gap |
|---|---|---|---|
| 0.00 (pelvis-anchored) | **51.2 mm** | 67.3 | 0.6 deg |
| 0.75 | **13.7 mm** | 23.4 | 0.6 deg |
| 1.00 (chest-relative) | **6.3 mm** | 11.9 | 0.6 deg |

All well inside - two of them far inside - the 20-40 mm regime the pricing solver was proven on.
The orientation gap is 0.6 deg at every anchoring, so the carry's *attitude* was never wrong either.

## AND THE GAP WAS NEVER THE CAUSE

With the gap now known to be small, the solver was run against it:

| candidate | follow | clavicle cost | clavicle mean/max | saturated | arm | forearm | elbow rng | sword | lat | fwd | blade |
|---|---|---|---|---|---|---|---|---|---|---|---|
| wf75 (reference) | - | - | 18.4 / 19.5 | 100% | 12.6 | 9.9 | 17.6 | 198 | 19 | 17 | 9.2 |
| sw25 (reference) | - | - | 13.0 / 17.1 | 0% | 11.2 | 1.8 | 5.3 | 448 | 69 | 97 | 16.6 |
| K1 | 0.00 (51 mm) | 8 | 18.7 / 21.1 | 97% | 10.8 | 15.9 | 13.8 | 223 | 20 | 22 | **7.7** |
| K2 | 0.00 (51 mm) | 20 | 18.8 / 22.6 | 97% | 10.5 | 16.5 | 13.1 | 230 | 20 | 23 | 7.8 |
| K3 | 0.25 | 20 | 18.7 / 21.2 | 97% | 11.1 | 14.6 | 14.5 | 224 | 19 | 21 | 8.1 |

The clavicle saturates at 51 mm exactly as it did at the 89 mm I previously believed - and an
earlier candidate saturated at a **13.7 mm** true gap. Correction magnitude does not predict the
outcome. The hypothesis that shrinking the gap into the isolation regime would let the pricing work
is therefore refuted directly, not merely unachieved.

What the isolation test and the pipeline differ in is not the distance: it is that the isolation
test performs ONE solve from the approved pose, while the pipeline's solve runs inside a sequence
that has already established a body frame and re-pins after. That is where the remaining answer is.

## ONE GENUINE GAIN WORTH KEEPING

K1 is the most disciplined sword measured in this entire investigation: **blade range 7.7 deg
against wf75's 9.2**, lateral 20 mm, fore/aft 22 mm, path 223 mm. The weapon character the human
review wanted is comfortably reachable; it is only the shoulder that will not move.

## WHY NO CARRY POSE WAS AUTHORED

Items 4-7 asked me to pick a walking phase and hand-author the arm there. I did not, because the
measurement that precedes it says the authored result would be the pose the arm already holds -
0.4 deg away. Authoring a carry to fix a 0.4 deg discrepancy would be inventing a difference in
order to have something to capture, and it could not affect the clavicle, which is set downstream
by the solve regardless of the target.

## SMALLEST NEXT PROBLEM

Why does one solve from the approved pose price correctly (clavicle -0.026 at cost 20) while the
same solve inside the generation sequence saturates, at every correction magnitude from 13.7 to
89 mm? The difference is the surrounding sequence - body-frame state and the re-pin - not the
target. That is a solver-integration question and it is now the only thing standing between the
disciplined sword K1 already produces and a healthy shoulder.

## STATE

Phase A safety used throughout: five research clips deleted under the reference-scan rule with the
controller verified clean before and after. Live profile restored to `CombatWalkCanonicalBaseline`;
zero-authority invariant **0.000000**. Regression suite **28 passed / 0 failed / 0 skipped**.
v008 `7a7ca074`, v009 `5546958e`, v010 `7c44dd1d`, v011 `e3705c2f` unchanged. No v012.
Serialized controller from disk: `Light_Walk8` = v011 @ ts 1.00, `Travel_Walk8` = v008 @ ts 0.50,
`CombatWalkSpeedScale` 0.65, no research clip referenced.

Technical debt recorded, untouched as instructed: broken PPtr in `Knight_Controller`, local id
`3908002699880397750`, present in git HEAD and every backup.

Research clips kept: `__carryK1` (best sword discipline yet), `__wf75`, `__sw25`, `__wc_cp12`,
`__Sword1H_WalkForward_CanonicalProbe`.

`WALKING COMBAT CARRY AUTHORITY BLOCKED — with the weapon solve off the arm already sits 0.4 deg from the approved carry on the walking torso, so there is no carry to re-author; and the true pre-solve gap (6-51 mm, not 83-91) does not predict the clavicle outcome, which saturates even at 13.7 mm`
