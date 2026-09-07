# WEAPON-SEQUENCE ROOT CAUSE FOUND — RESEARCH RESULT READY FOR HUMAN REVIEW

The arm solver was never failing inside the pipeline. Its answer was being overwritten by one
unguarded line, and every measurement for three milestones read the overwrite as the solve.

## TRUE CALL SEQUENCE

`HumanoidWalkGenerator.DisciplineUpperBody`, read from the code, not assumed:

| | operation |
|---|---|
| S1 | whole-cycle leg solve -> ground plane (x2, RePin) -> extension reserve (x2, RePin) -> knee ceiling (x6) |
| S2 | torso blade (RePin) -> sagittal posture (x2, RePin) |
| S3 | **x2 loop:** `SolveWeaponPass` *(skipped under an arm architecture)* -> `AlignHeadForward` -> `RePin` |
| S4 | `UpperBodyPass` - band-limited chest/head, `ResolveLowerBody` only |
| S5 | `CombatArmPass` **or** `WeaponFramePass` |
| S6 | `SmoothLegCorrection` -> `ResolveLowerBody` |
| S7 | **`SolveWeaponPass` again - unconditionally** |

S7 carried no guard. S3's guard exists precisely to keep the unpriced root-space solve away from an
arm architecture, and S7 ran it anyway, last.

## FIRST DIVERGENCE

Combat-arm configuration, mean over four gait phases (left stance, passing, right stance, toe-off):

| stage | hand->target | rot | clavicle | upper arm | forearm | Right Shoulder Down-Up | bodyPos.y |
|---|---|---|---|---|---|---|---|
| S4 | 49.3 mm | 0.55 deg | 0.4 deg | 0.4 | 0.1 | +0.098 | 0.9427 |
| **S5 CombatArmPass** | **3.1 mm** | **0.07** | **0.8** | 2.8 | 6.9 | **+0.072** | 0.9428 |
| S6 | 3.1 mm | 0.07 | 0.8 | 2.8 | 6.9 | +0.072 | 0.9428 |
| **S7 unguarded** | **103.3 mm** | **8.99** | **18.2** | 10.9 | 15.1 | **-0.972** | 0.9428 |

The priced solver converged: 49.3 -> 3.1 mm, clavicle 0.8 deg, shoulder at the approved carry,
reach paid by the forearm exactly as designed. S7 then undid all of it in one step.

Invocation trace: `S5 CombatArmPass x160`, `S7 FINAL unconditional SolveWeaponPass x40`.

## ISOLATION VS PIPELINE

There was no contradiction to explain. The same priced solve behaves identically in both - S5 is
the isolation result, produced inside the production sequence. What differed was that in the
pipeline something ran afterwards.

## PRIME SUSPECTS, BOTH REFUTED

**Body frame.** `bodyPos.y` is 0.9428 at S5, S6 and S7 - unchanged to four decimals while the
clavicle moves 17.4 deg. There is no mass-frame feedback loop. It was a sound hypothesis; the
measurement does not support it.

**Re-pin.** S6 (`SmoothLegCorrection` + `ResolveLowerBody`) leaves every arm figure bit-identical:
3.1 -> 3.1 mm, 0.8 -> 0.8 deg. It changes nothing about the arm.

**Target frame.** The target never moved. A later stage drove to a *different* target: root-space
distance falls 119.2 -> 26.9 mm across S7 while chest-frame distance rises 3.1 -> 103.3. Two
targets, not one moving target.

**Costs.** They survive - S5 proves it by working. The pricing never reached S7, which had none.

## ROOT CAUSE

> `DisciplineUpperBody` guarded the root-space `SolveWeaponPass` inside its weapon/head loop but
> not the identical call at the end of the method. Under either arm architecture the guarded call
> was skipped and the unguarded one still ran, so the last pass to touch the sword arm was always
> the unpriced six-muscle root-space chain - the exact pass the guard exists to avoid.

It also explains the invalid "pre-solve" measurements: enabling an architecture looks like
disabling the root-space solve, and does not.

## REPAIR

The same guard on the same call. Two lines.

Verified inert for production: the legacy path (both flags false, i.e. how v011 and v008 were made)
regenerates the frozen canonical probe **bit-identical - 102 curves, max abs difference
0.00000000**, still 120 weapon solves.

## AND A SECOND, SEPARATE FINDING

With the overwrite gone, the freed arm produces a **711-750 mm** sword path - worse than `__sw25`
(441 mm), which was already rejected. The saturation was the mechanism holding the sword still.

Target frame and joint pricing had been accidentally coupled: the root-space target had only ever
run **unpriced**, and the priced chain had only ever run against a **body-following** target. The
fourth combination was never tested. Adding `weaponChainAnatomicalPricing` decouples them, and the
frontier is clean and monotonic:

| clavicle price | clavicle | shoulder | hand off root anchor |
|---|---|---|---|
| 1 | 19.2 deg | -0.951 | 16.9 mm |
| 20 | 13.6 | -0.703 | 30.7 |
| 60 | 4.4 | -0.092 | 57.0 |
| 200 | 1.0 | +0.066 | 66.3 |
| 1000 | 0.4 | +0.096 | 68.6 |

So the residual depression is **not** a solver pathology - the solver frees the clavicle completely
when told to. It is the price of anchoring the sword in root space while the torso translates under
it. A real tradeoff, and now a continuous dial rather than a cliff.

## POST-REPAIR RESULT — `__seqRepair_c40`

| | v011 (production) | `__seqRepair_c40` |
|---|---|---|
| clavicle mean / max | 18.8 / 20.6 deg | **6.6 / 9.0** |
| Right Shoulder Down-Up | -1.000 .. -1.000 | **-0.396 .. -0.154** |
| saturated frames | **100 %** | **0 %** |
| sword path | 167 mm | 200 |
| lateral / fore-aft | 9 / 7 mm | 13 / 12 |
| blade range | 12.9 deg | **5.8** |
| hand step (jitter) | 4.4 mm | 7.2 |
| lower-body isolation | - | **4.4 mm** vs canonical baseline |

Shoulder anatomically healthy, blade steadier than production, sword slightly looser in
translation, lower body still isolated. Torso/head foundation untouched.

## VISUAL EXPECTATION — HONEST

I expect the shoulder to look **materially better**: a clavicle pinned at its limit for every frame
of the cycle is a silhouette defect a viewer reads immediately as a dead, hunched shoulder, and it
is gone. I expect the blade to read slightly calmer.

I do **not** expect this to close the gap to hand-animated quality. It removes a defect; it does
not add craft. The things the last human review disliked - the walk reading as procedural - are not
addressed here, and nothing in these numbers speaks to them. If `__seqRepair_c40` looks structurally
right but still lifeless, that is the expected outcome, and it is the point at which the Fable
challenger is the better use of effort than further solver work.

## TOOLING — `WeaponSolveExecutionTrace`

An isolation measurement must be able to observe its own isolation. `BeginIsolation()` /
`AssertIsolated()` fails loudly if any weapon solve ran under a claim that none did, naming the
stage and chain. This is what would have caught the wrong "pre-solve" numbers immediately.

## STATE / TESTS

Suite **30 passed / 0 failed / 0 skipped** (28 + Q1, Q2). O1-O4 and P1-P2 all PASS.

- **Q1** asserts no unpriced chain runs while an arm architecture is active - it checks *which pass
  ran*, because the output of this bug looks exactly like a solver that failed.
- **Q2** asserts a false isolation claim throws.

Research clips deleted under the reference-scan rule (7 scanned, 0 blocked); controller verified
clean before and after. From disk: `Light_Walk8` = v011 @ ts 1.00, `Travel_Walk8` = v008 @ ts 0.50,
`Player.prefab` `CombatWalkSpeedScale: 0.65`, no `__*` referenced. v008 `7a7ca074`, v009 `5546958e`,
v010 `7c44dd1d`, v011 `e3705c2f` unchanged. **No v012.** Runtime IK untouched.

Technical debt, untouched as instructed: broken PPtr in `Knight_Controller`, local id
`3908002699880397750`.

Kept for review: `__seqRepair_c40` (the result), `__repaired_f25` (freed shoulder, pumping sword -
the other end of the frontier), `__carryK1`, `__wf75`, `__sw25`, `__wc_cp12`,
`__Sword1H_WalkForward_CanonicalProbe`.

`WEAPON-SEQUENCE ROOT CAUSE FOUND — RESEARCH RESULT READY FOR HUMAN REVIEW`
