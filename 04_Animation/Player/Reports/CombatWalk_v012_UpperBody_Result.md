# v012 NOT GENERATED — clavicle relief and weapon pumping are the same degree of freedom

Audit completed uninterrupted in Edit Mode. No candidate qualified. The three existing sweep
candidates were used as-is (verified intact: 0.56522 s, 102 curves, 41 keys, unreferenced by the
controller). No regeneration, no new architecture, no interpolation candidate - the sweep did not
show a stable region to interpolate into.

## CANDIDATE TABLE

| | chest roll | head roll | head world tilt | head world pitch | chest P | head P | clavicle | shoulder | sword | chest frame step | R ankle dev | verdict |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| canonical baseline | 20.1 | 23.1 | 13.5 | 18.5 | +2.5 | -2.5 | 18.8 | -1.00 | 173 | **6.11** | - | reference |
| v011 | 20.0 | 22.9 | 13.5 | 18.5 | +2.4 | -2.6 | 18.8 | -1.00 | 169 | 6.11 | - | reference |
| __ub0 | **27.1** | **25.0** | 11.1 | **40.6** | +2.3 | +0.5 | 18.5 | -1.00 | **337** | **20.02** | **26.1** | FAIL |
| __ub1 | **26.9** | **25.5** | 10.9 | **40.4** | +2.2 | +0.5 | 18.5 | -1.00 | **337** | **19.43** | 22.0 | FAIL |
| __ub2 | **27.2** | **25.0** | 11.0 | **40.5** | +2.3 | +0.6 | 18.5 | -1.00 | **341** | ~20 | 24.7 | FAIL |

All three behave almost identically despite spanning chestRollFollow 0.25-0.35 and
weaponFrameTranslationFollow 0.25-0.45. That is not a stable region - it is a region where the
authorities barely reach the result, which is itself the symptom described below.

## THE BLOCKING RELATIONSHIP

**Clavicle relief tracks weapon TRANSLATION follow, not orientation.** Three measured points:

| weapon target | translation followed | clavicle deviation | sword path |
|---|---|---|---|
| root space (v011/baseline) | 0% | 18.8 deg | 169-173 mm |
| stabilised WeaponTorsoFrame | 25-45% | **18.5 deg** | **337 mm** |
| raw chest space | 100% | **0.2 deg** | **648 mm** |

The architectural hypothesis this milestone was built on - that the shoulder is freed by making the
weapon target body-relative in ORIENTATION, while translation can be stabilised away - is refuted.
At 25% translation follow we paid double the sword path and bought 0.3 deg of clavicle. The
clavicle depresses because the hand is held at a position the torso has translated away from;
only letting the hand travel with the torso relieves it, and that is exactly what produces the
pumping. They are not two problems to be traded off - they are one degree of freedom.

## SECOND, INDEPENDENT BLOCKER — THE UPPER BODY HAS NO WHOLE-CYCLE CONTINUITY

| | chest dev from approved | head dev | **chest max frame step** | **head max frame step** | clavicle frame step |
|---|---|---|---|---|---|
| canonical baseline | 14.5 / 22.1 | 14.3 / 24.3 | **6.11** | **6.48** | 2.1 |
| __ub0 | 17.3 / 29.0 | 26.8 / 29.4 | **20.02** | **20.39** | 6.9 |

20 deg of chest and head rotation inside one 60 Hz frame is a hard reject under the jitter priority,
and it would be plainly visible at gameplay distance - roughly a third of a full head-turn appearing
in a single frame, twice per cycle. It is not a tuning artefact: the legs have a band-limited
whole-cycle B-spline solve, the upper body does not. It is still solved independently per key inside
a two-pass loop with a body re-pin between passes. Driving per-key ABSOLUTE targets through a
per-key solver exposes that directly - each key converges to its own answer and the differences
land between frames.

Note also that the chest moved FURTHER from the approved orientation (14.5 -> 17.3 deg mean), so
the absolute target is not being reached at all; the solve is fighting the re-pin rather than
settling on the authored posture.

## WHAT DID WORK

The absolute-target formulation protected the sagittal posture, which every previous attempt
destroyed: chest pitch +2.5 -> +2.3, head -2.5 -> +0.5. The hard sagittal gate held. Head world
tilt also improved (13.5 -> 11.1). Those two results are worth keeping when this is next attempted.

## OTHER GATES

Frontal: FAILED - chest roll rose 20.1 -> 27.1 and head roll 23.1 -> 25.0. The amplification got
worse, not better; only the world-tilt component improved.
Weapon: FAILED - 337 mm with blade range 10.8 -> 15.2.
Shoulder: FAILED - still saturated at -1.000 for the whole clip, clavicle 18.5 vs 18.8.
Lower body: FAILED - right ankle 26.1 mm and right toe 27.7 mm deviation from the canonical
baseline, past the 25 mm reject threshold. Contamination from the upper-body solve moving the
shared body frame.

## SMALLEST NEXT ARCHITECTURAL PROBLEM

**Give the upper-body correction the same whole-cycle band-limited representation the legs already
have.** Not another target formulation - the absolute target is correct and proved itself on the
sagittal axis. The correction needs to be solved as a smooth cyclic trajectory rather than per key,
so that reaching a per-key target cannot produce per-frame steps, and so the body re-pin cannot
undo it between passes.

Only after that is the weapon question worth reopening, and it should be reopened as an ARTISTIC
decision rather than a technical one, because the measurement above shows there is no free lunch
in it: some sword travel must be accepted to get an anatomically credible shoulder. The useful
question for you is how much - 337 mm bought almost nothing, but the curve between 0% and 100%
translation follow has not been sampled between 45% and 100%.

## STATE

v008 `7a7ca074`, v009 `5546958e`, v010 `7c44dd1d`, v011 `e3705c2f` - unchanged. No v012.
Live profile restored from the frozen canonical baseline and **verified to regenerate the canonical
probe bit-exactly (worst difference 0.000000)**, so the zero-authority invariant holds: the new
architecture is genuinely inert when disabled.

Controller from disk: `Light_Walk8` = v011 @ ts 1.00, `Travel_Walk8` = v008 @ ts 0.50,
`CombatWalkSpeedScale` 0.65. No `__ub` clip referenced.

Regression suite: **25 passed / 0 failed / 0 skipped** (O1, O2, O3 all PASS).

`v012 NOT GENERATED — clavicle relief requires the weapon-hand translation follow that also causes pumping; upper body additionally lacks whole-cycle continuity (chest/head 20 deg per-frame jitter)`
