# Sword1H_WalkForward_v010 — GENERATED AND INSTALLED

Surgical swing-foot polish on v009. Timing, support architecture, whole-cycle solve, posture pass
and upper body all unchanged. 1.30 m/s, 150 spm, 0.800 s world cycle, clip 0.5652 s, 40 keys,
stride 1020 mm, L/R stance-speed asymmetry 0.0%.

## ORIGIN OF THE DEFECT (item 8)

**A - the SOURCE motion itself.** Left sole over the mocap's own cycle:

| | | | | | | | | | |
|---|---|---|---|---|---|---|---|---|---|
| source | 15 | **59** | 42 | 11 | **0** | 28 | **62** | 56 | 15 |
| v009 | 18 | **64** | 42 | 10 | **-5** | 21 | **58** | 55 | 21 |

Two peaks with a valley between, in `Loco_Walk_Fwd`. Every stage of the pipeline reproduced it
faithfully. It was never caught because during swing the correct support authority is zero, so the
planting audit is deliberately not looking there and the support audit correctly classifies the
graze as swing and moves on. New permanent diagnostic `SwingFootTrajectoryAudit` closes that gap.

**The RIGHT foot has the same double-bounce** (peak 89 -> dip 31 -> peak 89). It reads as clean
only because it never approaches the floor. So the visible defect is the SCUFF, not the bounce -
which is what set the target: bring the left's mid-swing clearance up to the right's.

## LEFT SWING - v009 vs v010

| | v009 | v010 | right foot (control) |
|---|---|---|---|
| classification | MID-SWING SCUFF (below floor) | DOUBLE-BOUNCE (no scuff) | DOUBLE-BOUNCE (no scuff) |
| **mid-swing minimum** | **-4.9 mm** | **+13.2 mm** | +26.9 mm |
| peak clearance | 63.9 mm @0.35 | 70.0 mm @0.38 | 88.8 mm @0.83 |
| toe minimum | -4.9 mm | +13.2 mm | +26.9 mm |
| heel minimum | 39.6 mm | 40.9 mm | 37.8 mm |
| descent onset | 0.63 | 0.38 | 0.15 |
| touchdown vertical | -779 mm/s | -723 mm/s | -806 mm/s |
| max vert vel / acc | 1378 / 56,970 | 1358 / 51,905 | 1057 / 43,532 |
| forward / lateral | 689 / 225 mm | 694 / 224 mm | 682 / 87 mm |

The foot no longer reaches or crosses the floor mid-swing - it clears by 13 mm where it used to
penetrate by 5 mm. Peak rose only 6 mm, so this is not a high step. The residual rise-dip-rise
remains but no longer touches down.

## RIGHT SWING (control)

Untouched by the repair - the pass is evidence-gated and skipped it (`midMin=23 skip:clean`),
so no right-leg change was authored. Its arc is identical in shape; planting improved slightly
as a side effect of the shared solve (11.2 -> 9.0 mm ankle).

## TEMPORAL

| | knee frame step L/R | knee p95 accel | ankle max frame | shelf | >0.99 |
|---|---|---|---|---|---|
| B_s0.40 (superseded) | 29.5 / 22.2 | 46,817 / 36,311 | 15.4 | 150 ms | 36% |
| v009 | 8.8 / 12.2 | 10,129 / 13,626 | 15.8 | 17 ms | 3.5% |
| **v010** | **10.1 / 11.1** | **9,863 / 15,648** | **15.2** | 33 ms | 8.0% |

Breakthrough retained - both knees stay in the 10-11 deg region, far from the 22-30 deg the
safeguard rejects. No new ankle snap. Shelf and >0.99 occupancy rose slightly (33 ms, 8%); at
higher repair strengths they blow out (183 ms, 31% at 1.00), which is what set the strength.

## SUPPORT

Semantic topology **L1 / R1**, unchanged. Left 0.68-0.20 (wrapped), Right 0.23-0.75, both
duration 0.55 with acquisition/stable/release 0.08/0.38/0.10 and 0.03/0.38/0.15.
Raw floor-contact topology improved from L2/R1 to L1/R1 as a consequence of the repair.

Stable-core planting - gates <15 mm / <15 mm / <3 deg:

| | ankle | toe | yaw |
|---|---|---|---|
| v010 L | 11.2 | 9.8 | 2.3 |
| v010 R | 9.0 | 6.8 | 2.5 |
| v009 L | 10.5 | 9.6 | 2.6 |
| v009 R | 11.2 | 9.8 | 2.0 |

## POSTURE (unchanged, as required)

| deltas vs idle | pelvis | chest | neck | head | chest>pelvis |
|---|---|---|---|---|---|
| v009 | +14.9 | +2.7 | +0.5 | -2.2 | +18 mm |
| **v010** | +14.8 | +2.7 | +0.5 | -2.2 | +17 mm |

## UPPER BODY (monitor only)

| | sword path | sword RMS | blade range | head mean/max | chest yaw | chest p95 ang vel |
|---|---|---|---|---|---|---|
| v009 | 165 mm | 8.7 | 11.5 deg | 0.03 / 0.29 | 25.7 deg | 154 |
| **v010** | 170 mm | 8.5 | 12.2 deg | 0.01 / 0.29 | 26.0 deg | 151 |

Chest motion materially unchanged, as item 17 asks - not independently reduced.

## GAIT

1.30 m/s, 150 spm, world cycle 0.800 s, step 510 mm, pelvis vertical 53.7 mm, track 314 mm,
stride asymmetry 0.0%. Semantic double support 2.5%, flight 5.0% - documented, not chased.

## TECHNICAL

Interpolated Humanoid legality PASS. Loop seam value gap 0.00000, tangent mismatches 0.
Regression suite 20/20 (two new: swing double-bounce detected, clean arc not flagged).
v001-v009 byte-identical (v008 `7a7ca074`, v009 `5546958e`).

## INSTALL

`Light_Walk8` forward -> v010 (ts 1.00). `Travel_Walk8` forward -> v008 (ts 0.50).
`CombatWalkSpeedScale` 0.65. Backup `Knight_Controller.controller.bak_pre_v010`.

**Correction to the v009 report:** that install also changed Travel's forward clip to v009, and
its verification misreported Travel as "v008 @ts1.00" because it read the wrong object. Verified
from the controller YAML this time: Travel is back on v008 at its original ts 0.50, and v009 is
now referenced nowhere.

## REMAINING RISKS

1. **The left swing still has a rise-dip-rise** (peak 70 -> dip 13 -> peak 61). It no longer
   touches the floor, but a slight mid-swing hesitation may still read on camera. Repair strength
   is capped at 0.28 by the right foot's support yaw, which crosses the 3 deg gate above it
   (3.4 deg at 0.34, 3.6 deg at 0.40) through the shared body frame.
2. **The right foot has the same underlying double-bounce**, unrepaired by instruction. It clears
   by 27 mm so it should not read, but the two legs are now asymmetric in how they were treated.
3. Knee near-extension time rose slightly (shelf 17 -> 33 ms, >0.99 3.5% -> 8.0%).
4. Semantic double support 2.5% remains below the 8-15% region.

**ARTISTIC APPROVAL: PENDING**
