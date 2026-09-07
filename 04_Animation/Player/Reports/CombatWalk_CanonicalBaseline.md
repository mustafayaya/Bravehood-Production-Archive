# Canonical baseline qualified. READY FOR v012 UPPER-BODY ARCHITECTURE.

No v012 created. No upper-body architecture implemented. v011 untouched and still installed.

## TEST INFRASTRUCTURE

| test | status |
|---|---|
| `O1_LocomotionGeneration_IsDeterministic` | **PASS** (was SKIP) |
| `O2_LocomotionGeneration_IsPlacementInvariant` | **PASS** (new) |
| `O3_Generation_RestoresCharacterTransform_EvenOnFailure` | **PASS** (new) |
| full suite | **25 passed / 0 failed / 0 skipped** |

**Why O1 was skipping.** It scanned the open scene for a Humanoid animator. That works when the
gameplay scene is open and silently ignores itself when it is not - and the editor was on
`UI_InventoryScreen`. A guard that disables itself exactly when the project is being worked on
elsewhere is worse than no guard. All three tests now instantiate the Player prefab themselves and
destroy it afterwards, so they run under any open scene, and a missing fixture is an Assert.Fail
rather than an Ignore.

O2 generates at four placements - origin, translated, yawed 137 deg, translated+yawed 180 deg -
and compares key times, values and both tangents on every channel at 1e-5. It is the permanent
guard for the defect this milestone repaired.

O3 covers item 5: the character is placed at a known transform, generation runs, and the transform
must come back - including on the exception path, since generation now canonicalises to the origin
and must never leave the gameplay Player standing there.

## CANONICAL PROBE vs v011 — QUALITY COMPARISON

`__Sword1H_WalkForward_CanonicalProbe`, generated from the frozen recipe under canonical placement.
Curve difference from v011 is 0.183 and is NOT reported as a failure; the question is whether the
canonical result is production-worthy.

| | knee L/R | knee p95 accel | shelf | >0.99 | legality | seam / tangents |
|---|---|---|---|---|---|---|
| v011 | 9.7 / 12.0 | 9,156 | 17 ms | 7.7% | PASS | 0.00000 / 0 |
| **PROBE** | **9.6 / 12.0** | **9,278** | **17 ms** | **4.2%** | **PASS** | **0.00000 / 0** |

| planting (semantic stable core) | ankle | toe | yaw |
|---|---|---|---|
| v011 L | 11.1 | 9.5 | 2.2 |
| v011 R | 9.8 | 6.9 | 2.7 |
| **PROBE L** | **11.3** | **10.1** | **2.4** |
| **PROBE R** | **9.9** | **6.6** | **2.8** |

| | chest pitch | head pitch | chest>pelvis | chest roll | head roll | head world tilt |
|---|---|---|---|---|---|---|
| v011 | +2.4 | -2.6 | +18 mm | 19.9 | 22.8 | 13.2 |
| PROBE | +2.5 | -2.5 | +18 mm | 20.0 | 22.9 | 13.3 |

| | sword path | blade range | clavicle dev | shoulder muscle |
|---|---|---|---|---|
| v011 | 169 mm | 10.5 | 18.8 deg | -1.00 / -1.00 |
| PROBE | 173 mm | 10.8 | 18.8 deg | -1.00 / -1.00 |

Support: semantic **L1 / R1**, stance windows identical (L 0.68-0.20 wrapped, R 0.23-0.75, both
0.55 with acquire/stable/release 0.08/0.38/0.10 and 0.03/0.38/0.15), double support 2.5%,
flight 5.0%. Swing: repair applied to the left, right correctly skipped as clean, deepest sole
-4 mm (v011 -5). Stride 1034 mm, native 1.830 m/s, L/R stance-speed asymmetry 0.0%.

**Every difference is within run-to-run noise of the metric sampling, and >0.99 knee occupancy is
actually better (4.2% vs 7.7%).** All qualification gates pass: ankle <15, toe <15, yaw <3,
L1/R1, no scuff regression, posture retained, legality PASS, loop clean.

## REQUALIFICATION

**None needed.** Decision Gate A. No solver or profile parameter was changed to reach this result -
the canonical frame alone produced a clip equal to v011 in every production measure. The earlier
alarming regeneration (support yaw 8.4 deg) was a non-canonical sample from the placement-dependent
distribution, not a property of the recipe.

## CANONICAL BASELINE — FROZEN RECIPE

Frozen as `Assets/Bravehood/Animation/Profiles/CombatWalkCanonicalBaseline.asset`, byte-identical
to the live profile at time of freezing.

| | |
|---|---|
| timing | fixedCycle 0.5652174 s, 40 keys, stance 0.55, target/imposed speed 1.840 m/s |
| whole-cycle solve | harmonics 3 -> fine control points 20, damping 0.7, iterations 8 |
| support | supportAware ON, swing floor 0.15 |
| leg priors | axialTwistRegularization 0.06, swingArcRepair 0.28 (scuff threshold 0.020) |
| posture | sagittalPostureAuthority 0.5, intent 3 deg, breathe 2 deg, downPhase 0, head recovery ON |
| upper body | weaponStability 0.80, headForwardAlignment 0.90 |
| geometry | trackWidth 0.24, turnout 16 deg, footYawWeight 0.014, extensionReserve 0.015, groundPlane ON |
| solver | warmStart ON, minimumNorm 0.004 |
| all v012 experiments | torsoOrientation 0, chestSpaceWeapon 0, femurSteering 0, frontalPosture 0, shoulderAnchor 0, weaponPassNoWarmStart 0, legCorrectionSmoothing 0, kneeCeiling 0, pelvisAllowance 0 |

Plus the permanent generation rule, now documented in the skill: generation canonicalises the
character to `position = zero, rotation = identity` inside the transactional state scope.

## ASSET STATE

v008 `7a7ca074`, v009 `5546958e`, v010 `7c44dd1d`, v011 `e3705c2f` - all unchanged from the
hashes recorded at the start of the forensics milestone. No v012 exists.

Controller, read from the serialized file: `Light_Walk8` forward = **v011 @ ts 1.00**,
`Travel_Walk8` forward = **v008 @ ts 0.50**, `CombatWalkSpeedScale` **0.65**.
The probe is **not** referenced by the controller.

Scene state: the gameplay scene was closed during Play Mode without saving, so the forensic
transform changes were discarded - no scene file carries them. The probe was generated from a
temporarily instantiated Player prefab which was destroyed afterwards, so no scene was modified at
all this milestone.

## NEXT

**READY FOR v012 UPPER-BODY ARCHITECTURE**
