# v012 NOT GENERATED — the tradeoff is real, but its curve is unfavourable at every point

The brief was re-issued, so the `weaponFrameTranslationFollow` sweep was not repeated: generation is
deterministic and the clips were still on disk, so it would reproduce the identical flat table
(clavicle 18.9 -> 18.4 deg, 100% saturated at every sample from 45% to 100%). That result stands.

Instead this turn went after the blocker that stood between us and the milestone's actual objective.
It found the real dial - and with it, the real tradeoff curve.

## THE SWEPT VARIABLE WAS THE WRONG ONE

`weaponFrameTranslationFollow` is inert for the shoulder. The variable that governs it is
**`weaponStability`** - how hard the hand is pulled toward its rest target. That was never the
subject of a sweep because it had been fixed at 0.80 since long before the weapon frame existed.

| weaponStability | shoulder min/max | saturated | clavicle mean/max | arm dev | forearm dev | sword | vert | lat | fwd | blade | hand step | clav step |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| baseline (root-space) | -1.00 / -1.00 | 100% | 18.9 / 21.0 | 12.5 | 11.0 | 181 | 42 | 10 | 10 | 8.9 | 7.9 | 1.3 |
| **0.25** | **-0.93 / -0.42** | **0%** | **13.0 / 17.1** | **11.2** | **1.8** | **448** | 53 | **69** | **97** | **16.6** | **19.8** | **5.0** |
| 0.40 | -1.00 / -0.72 | 50% | 17.2 / 18.5 | **15.2** | 3.0 | 361 | 45 | 55 | 70 | 12.8 | 16.3 | 2.2 |
| 0.55 | -1.00 / -0.80 | 93% | 18.1 / 18.8 | 14.4 | 5.2 | 287 | 38 | 39 | 49 | 9.6 | 14.2 | 2.3 |
| 0.80 (current) | -1.00 / -1.00 | 100% | 18.4 / 19.5 | 12.6 | 9.9 | 198 | 39 | 19 | 17 | 9.2 | 10.5 | 1.1 |

## THE CURVE, AND WHY IT DOES NOT CONTAIN A USABLE POINT

Anatomical gain is not gradual - it is almost entirely concentrated below 0.25:

- **0.80 -> 0.55**: sword grows 198 -> 287 mm (+45%) and the clavicle improves 18.4 -> 18.1 deg.
  Saturation still 93%. **89 mm of extra sword travel bought 0.3 deg.**
- **0.55 -> 0.40**: sword 287 -> 361 mm, clavicle 18.1 -> 17.2, saturation 93% -> 50%. But upper-arm
  deviation gets WORSE than baseline (15.2 vs 12.5) - the arm starts compensating, which item 20
  rejects outright.
- **0.40 -> 0.25**: the shoulder finally comes free - 0% saturation, clavicle 13.0 deg, and the
  forearm becomes the healthiest in the whole study (1.8 vs 11.0 at baseline). The price is a sword
  path of 448 mm with **lateral swing 69 mm and fore/aft 97 mm against the baseline's 10 and 10** -
  seven to ten times the excursion - blade range 16.6 vs 8.9, and hand travel of 19.8 mm per 60 Hz
  frame.

So there IS a knee, and it is at 0.25 - but it is on the wrong side of the weapon gate. The added
path at 0.25 is not smooth body-follow; it is dominated by lateral and fore/aft excursion, which is
the diagnostic signature of pumping rather than life. The clavicle's own frame step also rises to
5.0 deg (baseline 1.3), so the shoulder that finally moves does so less smoothly.

## CLASSIFICATION (item 17)

| candidate | shoulder | weapon |
|---|---|---|
| 0.25 | **natural** (0% saturation, clavicle 13.0, forearm 1.8) | **pumping** (lat 69, fwd 97, blade 16.6, hand step 19.8) |
| 0.40 | visibly strained (50% saturated) **+ arm compensation** | excessive (361 mm) |
| 0.55 | pathological (93% saturated, no real gain) | lively but acceptable (287 mm) |
| 0.80 | pathological (100% saturated) | controlled (198 mm) |

Nothing sits where both are acceptable. 0.55 is the only sample inside the 250-350 mm band you said
you would accept, and at that setting the clavicle has improved by 0.8 deg and is still saturated
93% of the cycle - it buys the travel and gets nothing for it.

## WHAT DID HOLD

The qualified torso foundation survived every weapon variant, which is the milestone's other
requirement:

| | chest roll | head roll | head tilt | chest step | head step | chest pitch |
|---|---|---|---|---|---|---|
| foundation | 15.4 | 3.6 | 5.2 | 5.85 | 3.00 | +2.3 |
| 0.25 | 17.1 | 3.6 | 5.4 | 5.83 | 3.05 | +3.0 |
| 0.80 | 16.9 | 3.6 | 5.1 | 5.88 | 3.03 | +2.7 |

Lower-body deviation 9.3-12.5 mm across the weapon variants - above the 5 mm flag, inside the 10 mm
investigate band, below the 15 mm reject. It comes from `WeaponFramePass` re-solving the 6-DOF body
frame after the arm moves the mass-weighted centre; the legs themselves are never re-solved.

## THE HONEST POSITION

Two milestones have now searched for a setting where the sword stays disciplined and the shoulder
looks human. The measurements say that setting does not exist in the current arm architecture: the
shoulder only relaxes when the hand is allowed to stop tracking a stabilised rest position, and the
moment it stops tracking, it swings.

That points at the arm chain rather than the target. The clavicle is doing work the arm should be
doing - at 0.80 the forearm deviation is 9.9 deg while the clavicle sits 18.4 deg from approved; at
0.25 that inverts (forearm 1.8, clavicle 13.0). A chain that could reach the same hand positions
with more elbow and less clavicle would move this curve, and that is a solver-structure question,
not an art-direction one.

## STATE

Live profile restored to `CombatWalkCanonicalBaseline`; zero-authority invariant re-verified at
**0.000000**. Regression suite **26 passed / 0 failed / 0 skipped** (O1-O4 PASS).
v008 `7a7ca074`, v009 `5546958e`, v010 `7c44dd1d`, v011 `e3705c2f` unchanged. No v012.
Controller from disk: `Light_Walk8` = v011 @ ts 1.00, `Travel_Walk8` = v008 @ ts 0.50, scale 0.65.

Evidence kept: `__wc_cp12` (qualified foundation), `__sw25` (shoulder-free / sword-loose end),
`__wf75` (sword-controlled / shoulder-saturated end), `__Sword1H_WalkForward_CanonicalProbe`.

`v012 NOT GENERATED — the sword/shoulder curve has its knee at weaponStability 0.25, where the shoulder is finally natural (0% saturation, clavicle 13.0 deg) but the sword pumps (448 mm, lateral 69 mm, fore/aft 97 mm); every setting with a disciplined sword leaves the clavicle saturated 93-100% of the cycle`
