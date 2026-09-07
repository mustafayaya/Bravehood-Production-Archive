# Fresh02 planting cleanup — candidate REJECTED on the preservation budget; Fresh02 stands

`__freshHumanForward_02` untouched (`a1630664`). Motion design locked. No v012. Nothing installed.

## SUPPORT DIAGNOSIS

The R "9 windows" was **classifier fragmentation**, not physical re-contact. With per-bone floors
(ankle 73/72 mm, toe 61/53 mm - the toe bone sits ~65 mm below the ankle, so a shared floor was
misreading the low restrained swing as stance) and a 12.5 ms hysteresis merge:

| | low windows | raw support windows | merged |
|---|---|---|---|
| LEFT | 1 | 2 | **1** |
| RIGHT | 1 | 4 (7 by the contact-aware scan) | **1** |

One physically continuous stance per foot. The flicker is the right foot's planted speed sitting at
the 450 mm/s threshold under the intentional 0.985 asymmetric shorten. Classification repaired by
hysteresis; the animation was not moved to satisfy the classifier.

## ACTUAL PLANTING ERROR (Fresh02, world space, 0.970x, 1.30 m/s)

| | LEFT | RIGHT |
|---|---|---|
| stance (phase) | 0.03 -> 0.60 (58 %) | 0.56 -> 0.94 (38 %) |
| ankle world drift, max / net | 33.6 mm / -24 fwd, -11 lat | 18.3 mm / -6 fwd, +13 lat |
| toe world drift, max | 53.4 mm | 19.2 mm |
| yaw drift | 13.8 deg | 10.8 deg |
| **penetration** | **58.6 mm (toe, through heel-off)** | 5.2 mm |
| worst single-frame slide | 6.0 mm | 5.5 mm |

**Worst visible event: the left toe going 58.6 mm through the floor during heel-off** (phase
~0.45-0.62). Cause: the authored path holds the ANKLE at floor height for the whole stance, but foot
pitch is shin-local and the shin leans forward through late stance, so as the heel is meant to
release the toe drives into the ground instead. The right foot's single-frame lurch (-11.6 mm vs a
-5.4 mm planted baseline at clip phase 0.665-0.670, where the knee straightens 46->21 deg as the
hip-height constraint hands from left leg to right) is ~6 mm - under one pixel at gameplay scale -
and was left alone per item 7.

## CORRECTION TRIED

`toeFloorClamp`: measure per frame how far the toe would penetrate its floor with the path as-is,
low-pass that deficit around the cycle (7-tap binomial, twice), then lift the authored ankle target
by it and re-run the normal leg solve. Stance-local by construction; never touches a swing frame.
A first version applied the raw per-frame deficit as a gate and wrecked tracking (ankle residual
156 -> 627 mm/s) - a gated lift is a step in the foot path, and the leg solve turns steps into
lurches. The smoothed version is what was measured.

## PRESERVATION — Fresh02 -> Plant

| | budget | measured |
|---|---|---|
| pelvis | <= 3 mm | **10.0 mm @ phase 0.55** |
| chest / head | <= 2 mm | **10.0 mm** each (rigid vertical shift, 0.00 deg rotation) |
| swing foot outside transition | <= 5 mm | **L 10.5 / R 7.1 mm** |
| sword hand | <= 3 mm | 10.0 mm (same rigid shift) |
| blade rotation | unchanged | 0.00 deg |
| clavicle rotation / shoulder range | unchanged | 0.00 deg / +0.081 .. +0.117 (identical) |

The lift raises the whole body ~10 mm at heel-off because the lifted foot is the leg that sets the
hip ceiling there. That is a change to the locked pelvis rhythm, three times the budget.

## BEFORE -> AFTER

| | Fresh02 | Plant |
|---|---|---|
| ankle residual (planted-foot drift) | 156 mm/s | worse (see note) |
| penetration L / R | 58.6 / 32.3 mm | 35.5 / 12.3 mm - halved, not removed |
| merged support windows L / R | 1 / 1 | **2 / 3** - worse |
| stance % L / R | 58 / 51 | 47 / 42 |
| worst 1-frame slide L / R | 6.0 / 5.5 mm | see note |
| legality violations | 0 | 0 |

Note on residuals: a contact-aware residual (toe while heel-up, ankle otherwise) was built this
milestone and turned out to manufacture a ~60 mm "slide" every time the contact point switches
bones. Those columns are not trusted and are not reported. The ankle-based figures above are.

## DECISION

Item 6: "If planting requires larger body changes: STOP." It does. Item 12: not clearly better -
penetration halved, topology and swing budget worse, pelvis moved. **Fresh02 wins; the planting
correction is rejected.** The candidate asset is kept for the record only, unreferenced.
A blind A/B was not rendered because the gate before it failed; it can be rendered on request.

## TECHNICAL QA (Fresh02, unchanged)

Interpolated legality 0 violations. Temporal: worst 1-frame step knee 15.7 mm / head 2.3 / hand
2.7; worst knee accel 212 m/s2 @ 0.94. Seam: C1 by construction. Support: one merged window per
foot; double 10 %, flight 2 %. Swing peak 134 mm, minimum swing clearance 20 mm.

## TESTS

EditMode **30 passed / 0 failed / 0 skipped**. No test weakened.

## PRODUCTION SAFETY (serialized)

`Light_Walk8` = v011 @ ts 1.00, `Travel_Walk8` = v008 @ ts 0.50, `CombatWalkSpeedScale` 0.65,
no research clip referenced, no v012.

`FRESH02 PLANTING CLEANUP READY FOR HUMAN REVIEW`
