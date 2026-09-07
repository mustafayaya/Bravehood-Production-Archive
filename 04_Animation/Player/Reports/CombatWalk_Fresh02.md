# `__freshHumanForward_02` — surgical restraint pass

Fresh01 unchanged (`6f3a9945`). `__seqRepair_c40` unchanged (`b9d978b7`). No v012. Nothing installed.

## WHAT CHANGED — three amplitudes and a legality repair

No phase lag, key spacing, support schedule or acceptance structure was touched. Fresh01 is still
reproducible from the same code: every change is an Options value whose default is the v01 setting.

| dial | v01 | v02 | what it restrains |
|---|---|---|---|
| `stanceKneeBendDeg` | 0 (uncapped) | **32** | pelvis crest, hence vertical rhythm |
| `swingHeightScale` | 1.00 | **0.62** | swing-foot clearance, hence the marching read |
| `armGain` | 1.00 | **0.50** | sword amplitude - lags untouched |
| `minKneeBendDeg` | 13 | 15 | intended support reserve (see below) |

**The pelvis was not scaled.** `RootT.y` is the mass-weighted frame, so scaling it does not scale
pelvis travel. Instead the stance leg is forbidden to extend past 32 degrees of knee bend, which
lowers the mid-stance crest while leaving the trough alone - so the delayed acceptance sink and the
two loaded phases survive and only the excursion comes down.

## FRESH01 -> FRESH02

| | Fresh01 | Fresh02 | brief's target |
|---|---|---|---|
| pelvis vertical rhythm | 90 mm | **69 mm** | 60-70 ✓ |
| swing-foot peak | 172 mm | **134 mm** | reduced ✓ |
| minimum knee bend | 4 deg | **4 deg** | 10-15 ✗ |
| clavicle deviation | 1.1 deg | **0.5 deg** | frozen ✓ |
| Right Shoulder Down-Up | +0.062 .. +0.136 | **+0.081 .. +0.117** | ~+0.099 ✓ |
| sword path | 333 mm | **261 mm** | 220-260 ✓ (top of range) |
| blade range | 15.8 deg | **10.1 deg** | 8-10 ✓ (top of range) |
| interpolation violations | 36 | **0** | 0 ✓ |
| native speed | 1.347 m/s | 1.340 m/s | - |
| playback for 1.30 m/s | 0.965x | **0.970x** | - |
| world cycle / cadence | 0.829 s / 144.8 | **0.825 s / 145.5** | - |
| residual foot/world mismatch | 159 mm/s | **139 mm/s** | not solved this pass ✓ |

## THE KNEE RESERVE WAS NOT ACHIEVED — and why

Target was 10-15 deg. It is still **4 deg**, and raising `minKneeBendDeg` from 13 to 15 did not move
it. The reason is worth recording rather than papering over:

The hip's horizontal offset from each foot is computed from a NOMINAL +/-90 mm, not from the posed
rig. Pelvis yaw swings each hip up to 40 mm fore and aft, so at the reach extremes the true span is
larger than the reserve calculation assumes and the leg quietly straightens past it.

I tried the obvious repair - measure the hip off the posed rig each iteration - and it made the clip
worse, not better: the hip position then depends on its own output, the outer loop destabilised, and
**pelvis excursion rose from 90 mm to 106 and foot penetration from 5 mm to 19**. That is a real
coupling problem, not a coding slip, and solving it properly is a solver-stability task rather than
an art-direction one.

Per the brief's instruction not to force this numerically at the cost of pelvis drop, stride or
skate, it is reported unachieved. The 4 deg occurs at contact and toe-off, the two reach extremes.

## LEGALITY REPAIR — verified non-invasive

Fresh01's key values are all legal; the violations are pure interpolation overshoot between keys.
The repair halves the tangents of only the two keys bracketing an offending span, and only until
that span is legal, with a flat-tangent fallback that cannot overshoot by construction.

Measured effect of the repair alone, on Fresh01's own settings:
**feet <= 6.3 mm, hips / chest / head / sword hand 0.54 mm.** Nothing in weight acceptance, hip
rhythm, overlap, arm inertia or leg silhouette moves at that scale, so it was accepted rather than
escalated. Violations 36 -> 0 at 200 samples per curve.

(The earlier figure of 7 violations was measured at 40 samples per curve. The denser scan finds 36
in the same clip - the clip did not change, the measurement got stricter.)

## PRESERVED, DELIBERATELY UNTOUCHED

Phase structure (contact -> acceptance -> compression -> mid-stance -> propulsion -> release ->
swing), the delayed sink after contact, all seven overlap lags (spine 0.06, chest 0.09, upper chest
0.12, clavicle 0.14, upper arm 0.17, forearm 0.21, wrist 0.25), the uneven key spacing, and the
rig-aware asymmetry.

## REVIEW CONDITIONS

Both rendered identically: same Player, armour, sword, scale; same shoulder camera (eye 1.72 m,
1.55 m behind, 0.52 m to the character's left, 15 deg down, FOV 80); same grid ground and lighting;
1280x720, 60 fps, 8 s; 1.30 m/s effective travel; runtime pose modifiers absent (Edit Mode).

## PRODUCTION SAFETY

`Light_Walk8` = v011 @ ts 1.00, `Travel_Walk8` = v008 @ ts 0.50, `CombatWalkSpeedScale` 0.65,
no research clip referenced. Verified from the serialized files.

---

## SIDE-BY-SIDE IDENTITY — read after watching

**LEFT = `__freshHumanForward_01`**  ·  **RIGHT = `__freshHumanForward_02`**

`FRESH HUMAN FORWARD V02 READY FOR HUMAN REVIEW`
