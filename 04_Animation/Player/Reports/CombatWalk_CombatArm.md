# COMBAT-ARM ARCHITECTURE BLOCKED

The arm solver was built as specified and its mechanism verified working in isolation. It cannot
help in the pipeline, for a reason the isolation test makes precise. No v012. Production untouched.

## WHAT WAS BUILT

1. **Anatomically-priced chain.** Minimum-norm regularisation used to charge every channel the same
   price per muscle unit, so the solve spent whichever was cheapest in MUSCLE units - clavicle
   depression. `SolveChain` now takes per-channel costs; the combat chain prices the clavicle high
   and the elbow low, and extends to forearm twist and both wrist channels.
2. **Separable authorities.** `weaponPositionAuthority` and `weaponOrientationAuthority` split what
   was one `weaponStability`. The sword's DIRECTION is what reads as intent; its exact coordinate is
   not, so orientation can stay firm while the hand occupies a region rather than a point.
3. **Whole-cycle band-limited arm correction**, projected onto the same cyclic B-spline basis as the
   torso, so a redistributed arm cannot arrive as per-key pops.

## THE MECHANISM WORKS — IN ISOLATION

Single solve, hand target moved 37 mm from where the gait puts it:

| clavicle cost | clavicle delta | elbow delta |
|---|---|---|
| 1 (old behaviour) | -0.1385 | +0.1252 |
| 8 | **-0.0552** (-60%) | +0.2155 |
| 20 | **-0.0261** (-81%) | +0.2444 |

Workload moves off the clavicle and onto the elbow, exactly as intended. The diagnosis that the
clavicle was being chosen because it was *cheap* was correct, and pricing fixes that choice.

## IT CHANGES NOTHING IN THE PIPELINE

| candidate | clavicle mean/max | saturated | upper arm | forearm | elbow range | sword | lat | fwd |
|---|---|---|---|---|---|---|---|---|
| `__wf75` (reference: good weapon) | 18.4 / 19.5 | 100% | 12.6 | 9.9 | 17.6 | 198 | 19 | 17 |
| `__sw25` (reference: good arm) | 13.0 / 17.1 | 0% | 11.2 | 1.8 | 5.3 | 448 | 69 | 97 |
| armA (elbow .25 / pos .50) | 18.5 / 19.9 | 100% | 12.2 | 11.2 | 16.8 | 205 | 19 | 18 |
| armB (elbow .10 / pos .50) | 18.5 / 19.9 | 100% | 12.2 | 11.1 | 17.2 | 207 | 19 | 17 |
| armC (elbow .25 / pos .25) | 18.5 / 19.5 | 100% | 12.4 | 10.7 | 17.5 | 206 | 19 | 17 |
| armD (1 iteration, cost 8) | 18.5 / 19.9 | 97% | 12.1 | 11.2 | 14.7 | 209 | 18 | 18 |
| armE (1 iteration, cost 20) | 18.5 / 19.9 | 100% | 12.2 | 11.1 | 16.6 | 206 | 19 | 18 |

Every variant lands on 18.5 deg and ~100% saturation. The weapon character is preserved (198 -> 205-209 mm,
lateral 19, fore/aft 17-18 - `__wf75`'s discipline, not `__sw25`'s swing), but the shoulder does not move.

## WHY — THE CORRECTION IS TOO LARGE FOR PRICING TO MATTER

Pricing changes which joint pays for a correction. It cannot make a large correction cheap.

**Required hand correction per key, from where the gait puts the sword hand to the authored combat
rest: min 57 mm, mean 89 mm, max 133 mm.** The isolation test that priced beautifully was moving
37 mm.

At 89 mm mean the clavicle's share is smaller in proportion but still large in absolute terms - and
absolute is what saturates a channel at -1.000. Cost 20 buys an 81% reduction in the clavicle's
*share*; the remaining 19% of a correction 2.4x larger still pins it.

Everything else was eliminated by measurement first:

| hypothesis | test | result |
|---|---|---|
| target out of reach | shoulder->target 515 mm vs 582 mm arm | 88.8%, never over 100% - not reach |
| clavicle is in the chain | excluded it entirely | solver correctly returns no shoulder correction, clip still 18.5 |
| pose round-trip corrupts channels | Get/Set identity check | clean, shoulder preserved to 5 decimals |
| corrections accumulate over iterations | 1 vs 2 vs 6 iterations | 18.5 at every count |
| target reference frame | 45-100% translation follow (previous milestone) | flat |

## THE REMAINING STRUCTURAL LIMITATION

The sword hand's authored combat rest is 57-133 mm away from where the walk's own arm motion puts
it, every frame. Closing that distance is what costs the shoulder, and no redistribution inside the
arm changes the size of the gap - only which joints pay for it, and at 89 mm every joint pays
enough to matter.

That is why the two endpoints exist. `__sw25` is cheap because it stops closing the gap; `__wf75`
is anatomically expensive because it closes it fully.

**The next lever is upstream: what the gait blend puts the sword arm at in the first place.** The
arm currently inherits the mocap's walking arm swing and is then corrected 89 mm back to a combat
carry. If the blend authored the arm closer to the combat carry - so the correction were 20-40 mm
rather than 57-133 - the pricing mechanism built here would work, because the isolation test shows
it works at that magnitude. That is a change to `Blend`'s arm handling, not to the arm solver, and
it is outside this milestone's scope.

## STATE

Live profile restored to `CombatWalkCanonicalBaseline`; zero-authority invariant re-verified at
**0.000000**. Regression suite **26 passed / 0 failed / 0 skipped**.
v008 `7a7ca074`, v009 `5546958e`, v010 `7c44dd1d`, v011 `e3705c2f` unchanged. No v012.

**Travel drifted again and was caught.** Mid-milestone the serialized controller showed
`Travel_Walk8` forward pointing at `__wf75` - a temporary research clip - after temp clips were
deleted from disk. It was reset to v008 @ ts 0.50, force-reimported, and re-verified from the file:
`Light_Walk8` = v011 @ ts 1.00, `Travel_Walk8` = v008 @ ts 0.50, no temp clip referenced anywhere in
the controller. This is the third time Travel has moved without being targeted; the pattern now
looks like Unity remapping references when generated clips are deleted from disk while the
controller is loaded, which is worth pinning down before the next milestone that creates temp clips.

Research clips kept for review: `__wf75`, `__sw25`, `__armD`, `__armE`, `__wc_cp12`,
`__Sword1H_WalkForward_CanonicalProbe`.

`COMBAT-ARM ARCHITECTURE BLOCKED`
