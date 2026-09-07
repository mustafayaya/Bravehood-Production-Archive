# Generator forensics — root cause found, repaired. v011 still not reproducible.

**NOT READY for v012.** The reproduction gate is unmet, for a reason that cannot be fixed by code.

## ROOT CAUSE — there was no post-v011 code regression

All four suspected areas were reverted TOGETHER first, to test whether the cause was among them
at all:

| state | max curve diff vs shipped v011 | worst channel |
|---|---|---|
| current code | 0.288411 | Right Lower Leg Stretch |
| **A+B+C+D all reverted** | **0.288411** | Right Lower Leg Stretch |

Byte-for-byte the same difference. The cause is not `SolveWeaponPass`, not the femur block, not the
posture call site, not `CaptureRest` - the reverts were undone and those mechanisms restored.
(`ToCurrentMuscleOrder` was also read and is pure: it allocates a fresh array.)

Structure was never the issue either: same clip length, same 102 curves, same key counts, same key
times to 1e-7. Only 31 of 102 VALUES differed, right-side dominant.

**The generator depended on where the character was standing in the scene.**

| perturbation | max curve diff | channel |
|---|---|---|
| two runs, same placement | 0.000000 | - |
| across a domain reload, same placement | 0.000000 | - |
| character MOVED only | 0.284911 | Right Lower Leg Stretch |
| character YAWED only | 0.285231 | Right Lower Leg Stretch |

And across five placements the answers form a CONTINUUM, not two branches:

| | pl0 | pl1 | pl2 | pl3 | pl4 | v011 |
|---|---|---|---|---|---|---|
| pl0 | 0.0000 | 0.2847 | 0.2850 | 0.1101 | 0.3128 | 0.2884 |
| pl1 | | 0.0000 | **0.0146** | 0.2797 | 0.2526 | 0.1833 |
| pl3 | | | | 0.0000 | 0.2744 | 0.2884 |

Mechanism: every measurement in the generator is root-LOCAL, which ought to make placement
irrelevant - but world-to-local conversion is float arithmetic, and the whole-cycle Gauss-Newton
does not fully converge (established back in the v009 milestone, where its step size wandered
rather than decaying). So round-off at the 7th decimal is amplified into ~0.3 muscle units, and it
lands in the right leg because that chain is 17.3 mm shorter and works nearest its reach limit -
the marginal chain absorbs the conditioning.

**Play Mode had moved the Player** (found at (39.63, -0.008, -9.21), yaw 3.376, with 27 instance
overrides including Transform overrides on Root, Hip, Pelvis and the leg bones). That is why the
"regression" appeared exactly when it did. No gameplay component executes in edit mode, so nothing
was perturbing bones - only the transform mattered.

## REPAIR

One change, in `HumanoidWalkGenerator.Generate`, inside the existing `CharacterStateScope`:

```
root.position = Vector3.zero;
root.rotation = Quaternion.identity;
```

Generating from a canonical identity transform makes the world-to-local conversions exact and
removes the perturbation at its source. `CharacterStateScope` restores the transform afterwards.

Verified - three deliberately different placements, including a 180 deg yaw:

| | __fx0 (as-found) | __fx1 (origin) | __fx2 (-13,0,22 @180) |
|---|---|---|---|
| pairwise max diff | **0.000000** | **0.000000** | **0.000000** |

Generation is now reproducible from any scene state.

## v011 REPRODUCTION — CANNOT BE MET

Repaired generator vs shipped v011: **0.183271** on Right Lower Leg Stretch.

v011 was generated at whatever placement the Player happened to occupy that day. That placement
was never recorded, no longer exists, and is not the canonical one. The shipped clip is one sample
from the pre-fix placement-dependent distribution; it is not recoverable except by rediscovering
its exact transform, which would be fitting noise.

Per the stop condition I did NOT retune the solver to make the numbers resemble v011. That would
hide the conditioning problem rather than repair it.

So the honest position: **shipped v011 remains authoritative and installed, but it is not
reproducible.** Every clip generated from now on will be.

## WHAT I DID NOT DO

- **`O1_LocomotionGeneration_IsDeterministic` still SKIPS in the Test Runner.** Item 11 required
  this fixed. I did not get to it - the forensic bisect consumed the milestone. The determinism
  property is verified by direct measurement above, but the permanent guard is still inactive.
- **No golden-master regression test** (item 12). Blocked in any case: there is currently no
  reproducible golden master to assert against.
- **No zero-strength invariance tests** (item 7). The bisect showed the four disabled mechanisms
  ARE already inert, so the rule holds today - but it is unguarded.

## STATE

v008 `7a7ca074`, v009 `5546958e`, v010 `7c44dd1d`, v011 `e3705c2f` - all unchanged.
Controller: `Light_Walk8` = v011 @ ts 1.00, `Travel_Walk8` = v008 @ ts 0.50, scale 0.65.
All temporary clips removed. Source backup at `scratchpad/bisect_backup/`.

**One thing to check when you leave Play Mode:** the bisect moved the Player around the scene and
the final restore was interrupted when Play Mode started, so the gameplay scene's Player transform
may sit at (-13, 0, 22) yaw 180 instead of (39.6345, -0.0080, -9.2065) yaw 3.376. The change was
never saved to disk, so reloading the scene discards it - but verify before saving that scene.

## DECISION REQUIRED

The generator is repaired and now deterministic. v011 cannot be reproduced. Two ways forward:

1. **Accept v011 as a frozen artistic artifact** - keep it installed, and treat the next generated
   clip as the reproducible baseline for all future work. Requires checking that clip against the
   production gates before adopting it.
2. **Regenerate the baseline now** under canonical placement and re-qualify it. My last measurement
   of a non-canonical regeneration failed the support-yaw gate (8.4 deg), so the canonical clip
   must be measured before it is trusted - it may be better or worse; it is a different sample.

Either way this is your call, not mine.

**NEXT STEP: NOT READY for the v012 upper-body architecture.**
