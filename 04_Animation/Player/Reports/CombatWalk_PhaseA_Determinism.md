# Phase A — determinism. STOPPED before Phase B (item 6).

Phase B was NOT entered. No v012. Shipped v011 remains installed and authoritative.

## THE PREMISE WAS WRONG — GENERATION IS DETERMINISTIC

The milestone assumed "the iterative solve is settling onto a different valid branch between
runs/sessions". Measured, it is not:

| comparison | worst per-channel difference |
|---|---|
| two generations back-to-back, same session | **0.000000** |
| generation before vs after an assembly recompile / domain reload | **0.000000** |
| current code vs the SHIPPED v011 asset | **0.288411** (Right Lower Leg Stretch) |

Run-to-run and reload-to-reload reproduction is exact across all 102 channels. There is no hidden
run state, no warm start leaking between generations, no ordering instability. The single thing
that does not reproduce is the shipped clip - and the profile asset diffs against git HEAD by
ADDED fields only, every one at its off value, so no pre-existing setting changed.

## WHICH MEANS SOMETHING WORSE

The difference is therefore a CODE difference: the generator changed after v011 was produced. And
it is not benign. Regenerating the v011 recipe with today's code gives a measurably worse clip:

| | knee L/R | shelf | >0.99 | L plant ank/toe/yaw | R plant ank/toe/yaw | sword |
|---|---|---|---|---|---|---|
| shipped v011 | 9.7 / 12.0 | 17 ms | 7.7% | 11.1 / 9.5 / 2.2 | 9.8 / 6.9 / **2.7** | 169 |
| regenerated | 9.1 / **16.1** | **50 ms** | 12.9% | 11.3 / 10.1 / 2.4 | **15.0** / 11.0 / **8.4** | 209 |

Right-foot support yaw 8.4 deg fails the <3 deg production gate outright, the right ankle sits on
the 15 mm limit, the knee shelf triples and the sword path grows 24%. **A regression was introduced
into the locomotion generator by the post-v011 code, even though every new mechanism ships gated
OFF.** Something in those edits changes the solve on a path that is supposed to be inert.

This is exactly the situation item 6 exists for, so Phase B was not started: building an upper-body
architecture on a generator that silently degrades the lower body would produce a candidate whose
measurements could not be trusted or reproduced.

## WHAT IS NOW COVERED

`O1_LocomotionGeneration_IsDeterministic` added: generates twice from identical inputs and compares
curve CONTENT - key times, values, in tangents and out tangents, per channel, at 1e-5 - rather than
file hashes, since the asset's embedded name makes the hash differ for reasons that are not motion.

**Caveat, stated plainly: this test currently SKIPS in the Test Runner.** It exits in ~3 ms on a
precondition, though the same lookups succeed when run directly through the editor (profile, pose
and the Player animator with isHuman=true all resolve). I could not get it to execute inside the
runner and did not want to spend the remainder of the milestone on test plumbing. The determinism
property itself is verified by direct measurement above; the guard is not yet active. Suite is
22 passed / 1 skipped.

## RECOMMENDED NEXT STEP

Find the post-v011 regression before any upper-body work. The candidates, all added after v011 and
all supposedly inert at their defaults:

1. `SolveWeaponPass` - the shoulder-anchor block, which now calls `approved.ToCurrentMuscleOrder()`
   and computes two muscle indices on every pass, plus the `!p.weaponPassNoWarmStart` guard.
2. `WholeCycleLegSolve` - the `isFemurAim` table and the femur curvature block.
3. `DisciplineUpperBody` - the posture call site, restructured to choose between the combined
   orientation pass and the sagittal pass.
4. `HumanoidFootLockSolver.CaptureRest` - the added chest lookup and chest-local hand capture.

A bisect over those four, regenerating and comparing against shipped v011 each time, will identify
it quickly. My inspection did not find the mechanism - each is guarded and should be inert - which
is itself a reason to bisect empirically rather than reason further.

Two structural options once found, and this is your call, not mine:
- **Repair** the regression so the current code reproduces v011 exactly, then proceed to Phase B.
- **Re-baseline**: accept a regenerated clip as the new v011-equivalent. NOT recommended on the
  numbers above - the regenerated result fails a production gate.

## STATE

v001-v011 byte-identical (v008 `7a7ca074`, v009 `5546958e`, v010 `7c44dd1d`, v011 `e3705c2f`).
No v012. Play Mode exited. Serialized controller from disk: `Light_Walk8` = v011 @ ts 1.00,
`Travel_Walk8` = v008 @ ts 0.50, `CombatWalkSpeedScale` 0.65. Travel did not drift. All temporary
clips removed. All Phase-B mechanisms from the previous milestone remain in the codebase, gated OFF.

**ARTISTIC APPROVAL: PENDING** (v011 remains the candidate)
