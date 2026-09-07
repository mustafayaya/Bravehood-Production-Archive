# `__freshHumanForwardLeft_02` — surgical body-orientation refinement of ForwardLeft01

ForwardLeft01 is the accepted motion foundation; this clip changes ONLY the body-frame / pelvis orientation
relationship. Foot strategy, stride, arms, sword and head architecture untouched. Research clip only.

```
TECHNICAL QUALIFICATION   QUALIFIED - at least as clean as ForwardLeft01 (see TECHNICAL QA)
ARTISTIC MOTION APPROVAL  PENDING - blind A/B for human review
```

Every number is the finished clip sampled in Edit Mode at 480 phases on the Player prefab, holder-relative,
root untouched during sampling (0.0 mm / 0.0°), with the same probe as ForwardLeft01 and v012.

## BODY ORIENTATION CHANGE

Dials (everything else identical to ForwardLeft01: travelDeg −45, strideScale 0.94, stanceWidenMm 6,
headYawDeg +8, Fresh02 knee / swing / arm dials):

| | ForwardLeft01 | **ForwardLeft02** |
|---|---|---|
| `yawTowardTravelDeg` (RootQ body-frame yaw) | −18.5 | **−17.7** |
| `chestFollowFrac` → spine/chest counter-twist | −0.195 → 22.1 muscle-deg | **0.034 → 17.1 muscle-deg** |

Micro-study (the one bounded study, three builds around 01, all foot dials fixed):

| candidate | yaw / twist | hip-line mean | shoulder-line mean | head vs v012 |
|---|---|---|---|---|
| a | −17.8 / 21.5 | 34.7° | 8.5° | −1.7° |
| b | −17.7 / 20.0 | 33.7° | 9.0° | −2.2° |
| c | −18.3 / 24.0 | 36.8° | 8.1° | −1.3° |

The study calibrated the body-frame response (hip line ≈ 1.0·yaw + 0.78·twist, shoulder line ≈ 0.75·yaw −
0.22·twist; RootQ is the mass-weighted frame, ~0.39 pelvis / 0.61 chest). All three candidates kept a 25–29°
pelvis-to-shoulder separation — the thing the reviewer sees as "rotated underneath". The judgment call was to
take the twist down rather than the yaw: 02 is built at yaw −17.7 / twist 17.1, which puts the pelvis in the
30–33° guide AND closes the pelvis-to-shoulder separation from 27° to 22°, so the body reads connected instead
of counter-wound. Candidates were deleted; 02 is the only new asset.

## FOOT PRESERVATION

| | ForwardLeft01 | **ForwardLeft02** |
|---|---|---|
| stance ground-track heading L / R | −46.1° / −43.9° | **−46.1° / −43.9°** |
| foot separation ⟂ travel (R − L) | 158 .. 175 mm | **157 .. 174 mm** |
| swing peak L / R | 134 / 134 mm | 134 / 134 |
| stance foot height L / R (floor 73) | 69.7..78.6 / 69.8..94.7 | 69.7..76.4 / 69.1..90.7 |
| toes lowest L / R | 0.9 / 22.5 mm | 1.9 / 21.2 |
| perpendicular slide (probe window incl. heel-off) mean / max | 154 / 829 mm/s | 146 / 829 |
| pelvis vertical rhythm | 59.2 mm | **59.2 mm** |
| strideScale | 0.94 | 0.94 |

No crossing, no new skate, stride untouched. The hip-relative foot solution was not reopened.

## PELVIS / CHEST / HEAD

| | ForwardLeft01 | **ForwardLeft02** | v012 |
|---|---|---|---|
| hip-line yaw range · mean (+ = toward travel) | 25.2..46.3 · 35.8° | **21.4..42.4 · 31.9°** | −6.8..14.3 · 3.8 |
| shoulder-line yaw range · mean | 5.9..12.0 · 9.0° | **7.0..13.1 · 10.1°** | −4.1..2.0 · −1.0 |
| pelvis-to-shoulder separation (means) | 26.8° | **21.8°** | 4.8 |
| head yaw vs v012 head at the same phase | −2.2° (toward travel) | **−3.3°** | 0 |

Pelvis still leads the locomotion (32° toward −45° travel with Forward's own ±10° oscillation on top), chest
~10° toward travel, head threat-facing within ~3°. The neck was NOT re-counter-rotated: `headYawDeg` stays at
+8 (natural alignment first); the extra 1° toward travel comes from the chest carrying 1° more.

## SWORD / SHOULDER

Frozen and verified: Right Shoulder Down-Up 0.081..0.117 (identical to 01 and Forward), blade orientation
range 9.2° (01: 9.2), sword-hand path 473 mm / cycle (01: 477; the 4 mm is the smaller counter-twist carrying
the hand less). armGain, arm lags, clavicle and inertia code paths unchanged.

## NATIVE SPEED

Planted-foot ground track along −45°: **1.246 m/s** (01: 1.241; same feet, the 5 mm/s is probe noise on the
stance window). Cadence 150 spm at 1.0×. GuardGait entry `{ −45, 1.24 }` measured, NOT installed.

## TECHNICAL QA

| | ForwardLeft01 | **ForwardLeft02** |
|---|---|---|
| interpolated Humanoid legality (24 samples/span) | 0 violations, 95 informational | **0 violations, 95 informational** |
| build-time span flattenings | 6 | 7 (Forward family 7–8) |
| temporal audit 60 Hz: hold snaps | 0 | **0** |
| top per-frame deltas (L knee / R knee / R hip / L hip / ankles / pelvis / chest / sword hand) | 24.1 / 20.6 / 13.2 / 10.2 / 3.6 / 1.4 / 0.46 / 0.18 mm | 24.3 / 23.3 / 14.7 / 10.3 / 3.6 / 1.3 / 0.46 / 0.18 (v012: 23.5 / 23.4 / 14.8 / 9.8 / 3.6 / 1.3 / 0.46 / 0.19) |
| support / swing (stance heights, toes, swing peak, separation) | above | above, equal or better |
| seam | periodic basis, 49 keys / curve, loop settings baked | same |
| deterministic generation | — | `V1` builds twice, curves key-identical |
| root canonical during sampling | 0 mm / 0° | 0 mm / 0° (+ `V2`) |
| clip settings | orientation / Y / XZ baked, Based Upon Original | same (`V1` asserts = v012) |

## GENERATOR REGRESSIONS

Added to `HumanoidIdleRegressionTests` (narrow, causal, no expansion beyond these):

- `V1_HumanWalkPerformance_UsesTheFullyBakedInPlaceRootContract_Deterministically` — a freshly built
  HumanWalkPerformance clip carries the v012 in-place root contract (loopTime, no loop blend, orientation / Y /
  XZ baked, Based Upon Original, heightFromFeet off; asserted field-by-field against the production v012
  settings), and two builds from identical inputs are key-identical.
- `V2_HumanWalkPerformanceClip_LeavesTheRootCanonicalWhenSampled` — sampling a built clip on the Player at an
  off-origin, yawed placement moves the root < 0.1 mm and < 0.001°.
- `V3_ExplicitHeadYaw_TouchesOnlyTheNeck` — headYawDeg 0 vs 8: Neck Turn Left-Right moves, every other
  binding (RootT / RootQ / solver-owned legs / spine / arms) is key-identical.

Suite: **40 passed / 0 failed / 0 skipped** (37 unchanged + V1–V3). First run of the draft tests failed twice,
usefully: V2 had displaced the character itself, and `SampleAnimation` writes the Animator's local transform
absolutely (v012 resets a 12.9 m local offset to zero too) — the contract is now measured at local identity
under a placed, yawed holder (baked: 0.000 mm / 0.000°; unbaked Fresh02: 56.6 mm / 5.0°). V3 found the head
yaw coupling into the legs by ≤ 8e-4 muscle units and RootT.y by 1.7e-6 m through the mass-weighted body
frame; the test now allows exactly that coupling (2e-3 / 1e-5) and demands key-identity everywhere else.

## BLIND A/B (`Artifacts/AnimationReview/`)

`ForwardLeft_01_vs_02_sideBySide_BLIND.mp4` — 1920×540, 480 frames, 60 fps, 8 s. Both halves: the same
gameplay camera rig (eye 1.72, dist 1.55, side −0.52, pitch 15°, FOV 80), same Player + Warrior Base review set,
same −45° travel, same 1.241 m/s translation, same 1.0× playback, same ground, rendered by the same
`GameplayCameraPreview.Render` call with only the clip path different. Left/right assignment was drawn by a coin
flip and is NOT stated here; it is in `ForwardLeft_01_vs_02_sideBySide_BLIND_KEY.txt` — open it after judging.
No winner is declared.

Singles: `ForwardLeft01_RAW_native_1241.mp4`, `ForwardLeft02_RAW_native_1241.mp4`. Context only (not a
candidate): `Fresh02_RAW_reference_0970x.mp4` / `v012_PRODUCTION_unarm.mp4`.

## PRODUCTION SAFETY

From disk: `Knight_Controller.controller` Light_Walk8 child 0 = `b9d47c80…` (v012), Travel_Walk8 child 0 =
`12f2b3e7…` (v008); none of the `__fresh*` research guids (Forward_01/02, Transport, Plant, ForwardLeft_01/02)
is referenced. md5: Fresh02 `a1630664`, v012 `530b8133`, v008 `7a7ca074`, ForwardLeft01 `612bccab` unchanged;
ForwardLeft02 `b4c2f052` new. `Player.prefab` and the controller show no working-tree change. No ForwardLeft
installed. `AnimationAssetSafety.VerifyProduction` CLEAN ("no research clip referenced by the production controller").

`FRESH HUMAN FORWARDLEFT 02 READY FOR HUMAN REVIEW`
