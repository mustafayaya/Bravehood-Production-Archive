# `__freshHumanForward_02` — handover continuation

## HANDOVER UNDERSTANDING

Inherited: Fresh01 as the human-selected motion foundation, `__seqRepair_c40` as a weapon-discipline
reference only, production frozen at v011/v008/0.65, and a Fresh02 that the previous session had
already generated from three amplitude dials plus a legality repair.

Deliberately NOT reopened: the gait itself, the phase structure, timing texture, overlap lags,
asymmetry, speed/cadence, the solver architecture, SeqRepair, runtime IK, foot planting.

What this pass added: an animator's pose-by-pose inspection of Fresh01 vs Fresh02 at six key phases
from the gameplay camera (the previous pass was number-led), the support-topology and jitter audits,
one bounded silhouette study, and the full regression suite.

## FRESH01 DIAGNOSIS — the writer of each excess

| symptom | authored writer | mechanism |
|---|---|---|
| pelvis excess (90 mm) | hip-height derivation: `minKneeBendDeg` sets the leg limit; no cap on mid-stance extension | crest at mid-stance rides the full 728 mm limb |
| march read | `FootY` swing arc (172 mm peak) + normal ~60 deg swing flexion foreshortened by the 15-deg-down camera | high foot AND folded knee viewed from behind/above |
| near-lock (4 deg) | nominal +/-90 mm hip offset in the reach calculation vs. real hip displaced by pelvis yaw | true span at contact/toe-off exceeds the reserve assumption |
| sword excess (333 mm / 15.8 deg) | `armGain` scales all arm amplitude terms; lags are separate constants | amplitude, not lag |

## REFINEMENT

Three amplitude dials, each defaulting to the Fresh01 value so Fresh01 stays reproducible:
`stanceKneeBendDeg 0 -> 32` (caps the crest, not a RootT.y scale - bodyPosition is mass-weighted),
`swingHeightScale 1.00 -> 0.62`, `armGain 1.00 -> 0.50`. Lags, spacing, sink, asymmetry untouched.
Interpolation legality repaired by local tangent flattening only.

Silhouette study this pass (`swingPeakForwardMm` 0 / 40 / 80): knee fold at swing peak 57 / 58 / 57
deg, thigh pitch 35 / 36 / 37. Inert, because at peak the foot sits ~40 mm behind the hip, so a
+40..+80 shift barely changes hip-to-foot distance. Opening the knee would need ~+150 mm, which
reshapes the step. Dial left at 0; Fresh02 unchanged. 57 deg peak swing flexion is textbook walking.

## FRESH01 -> FRESH02

| | Fresh01 | Fresh02 |
|---|---|---|
| playback for 1.30 m/s | 0.965x | 0.970x |
| effective speed | 1.30 m/s | 1.30 m/s |
| cadence / world cycle | 144.8 spm / 0.829 s | 145.5 spm / 0.825 s |
| pelvis vertical rhythm | 90 mm | **69 mm** |
| swing-foot peak | 172 mm | **134 mm** (min swing clearance 20 mm) |
| minimum knee bend | 4 deg | 4 deg (see below) |
| foot/world residual | 159 mm/s | **139 mm/s** |
| clavicle deviation | 1.1 deg | **0.5 deg** |
| Right Shoulder Down-Up | +0.062 .. +0.136 | **+0.081 .. +0.117** |
| sword path | 333 mm | **261 mm** |
| blade range | 15.8 deg | **10.1 deg** |
| interpolation violations (200/curve) | 36 | **0** |
| support: L / R stance, double, flight | 57 / 49 %, 9 %, 3 % | 57 / 51 %, 10 %, 2 % |
| support windows L / R | 1 / 6 | 2 / 9 |
| worst 1-frame step knee / head / hand | 20.8 / 2.4 / 3.4 mm | **15.7 / 2.3 / 2.7 mm** |
| worst knee accel | 244.8 m/s2 @ 0.67 | **212.5 m/s2 @ 0.94** |

**Knee reserve not achieved** (still 4 deg). Measuring the hip off the posed rig to close the reach
assumption destabilised the outer loop (pelvis 90 -> 106 mm, penetration 5 -> 19 mm), so it was
reverted per the brief's instruction not to buy the number with pelvis drop or skate.

**Right-foot support windows fragment (6 -> 9)** under the product rule: the right foot's planted
speed hovers at the speed threshold because of the 0.985 asymmetric shorten. Topology, not a visual
defect; flagged for the planting pass that this milestone deliberately did not run.

## MOTION QUALITY — what changed to the eye

Weight: the acceptance sink survives intact; the body still lands after the foot does.
Pelvis: the crest is lower, so the two loaded phases read as weight rather than bounce.
Legs: swing is lower and the arc flatter; the fold at peak is normal flexion, not a lift.
Feet: heel strike / sole load / roll / toe release unchanged; stance holds 71-73 mm on a 73 mm floor.
Torso overlap: identical lags; amplitude untouched.
Shoulder: tighter around the approved carry than Fresh01, zero saturation.
Arm / sword: half the amplitude, same sequence torso -> clavicle -> arm -> forearm -> wrist -> blade.

## LEGALITY

7 (at 40 samples/curve) = 36 (at 200) -> **0**. Repair effect on Fresh01's own settings: feet <= 6.3 mm,
hips/chest/head/hand 0.54 mm. Not material; accepted.

## TESTS

EditMode **30 passed / 0 failed / 0 skipped** with the new authoring, renderer and trace files present.
No test weakened.

## PRODUCTION STATE (serialized)

`Light_Walk8` = v011 @ ts 1.00, `Travel_Walk8` = v008 @ ts 0.50, `CombatWalkSpeedScale` 0.65,
no research clip referenced, no v012. Fresh01 `6f3a9945`, Fresh02 `a1630664`, SeqRepair `b9d978b7`.
Study temps `__sf40` / `__sf80` deleted after a zero-reference scan.

---
## SIDE-BY-SIDE IDENTITY — read after watching
**LEFT = Fresh01 · RIGHT = Fresh02**

`FRESH HUMAN FORWARD V02 READY FOR HUMAN REVIEW`
