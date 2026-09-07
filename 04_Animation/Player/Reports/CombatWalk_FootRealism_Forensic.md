# Steady-walk vs W-tap foot realism forensic — `Sword1H_WalkForward_v012` / `Sword1H_WalkForwardLeft_v001`

Diagnosis only. No production clip, controller slot, GuardGait entry or FootIK setting was changed. Every runtime
number is the real Player in Play Mode with the production assets (gameplay starting loadout, lock-on held,
FootIK AnimatedBones, all writers on), 60 Hz, forensic capture through `RuntimeQaDriver`. The diagnostic clips
(`__diag_A_footRoll`, `__diag_B_hipReach`, `__diag_AB_both`) reached the Player only through an in-memory
`AnimatorOverrideController` and are referenced nowhere.

```
TECHNICAL QUALIFICATION   n/a - forensic
ARTISTIC MOTION APPROVAL  PENDING - the diagnostic 2x2 is for looking, no winner declared
```

## USER-REPORTED PHENOMENON

Reproduced on the Player, same camera / equipment / direction / writers:

| | HOLD W (idle 2 s → 6 s → idle) | TAP W, 4 Hz (0.25 s press / 0.25 s release ×12) |
|---|---|---|
| ground speed | 1.108 m/s steady | oscillates 0.28 → 1.06 m/s, mean 0.715 |
| MotionSpeed (playback) | 0.834× | **0.60–0.71×** (mean 0.625; 0.60 is `MotionSpeedClamp.x`) |
| Animator state | Locomotion, v012 weight 1.000 | **Locomotion the whole time** — the 0.25 s release never lets `IsRunning` drop (Motor still at 0.28 m/s), so Idle is never entered after the first press; 9 transition frames total, idle weight 0 |
| phases played | all | all (uniform histogram, deciles 8–11 % each) |

At 1 Hz taps (0.5 s / 0.5 s) the idle DOES enter (74 transition frames, idle weight > 0.2 for 20 frames); at 0.67
Hz (0.75 s / 0.75 s) the Player stands in Idle for 56 frames and blends for 97. `w_tap`, `w_tap30`, `w_tap45`.

## WHY W-TAPPING LOOKS BETTER

Measured, in order of evidence:

1. **Playback rate.** Tapping keeps the walk at the clamp floor: 0.60–0.71× instead of 0.834×. Every event below
   (stiff strike, toe plunge, straight push-off) happens 15–28 % slower and covers less ground per event.
2. **Ground speed below stride-match.** For most of each tap the Motor moves slower than the clip's feet: the
   planted boot is pushed *backward* relative to the body rather than dragged forward, which reads as grip.
3. **Idle re-entries (slow taps only).** At ≤ 1 Hz the CombatIdle pose (both soles flat, both knees bent) is
   blended in for 20–100 frames per 6 s and masks the heel-only strikes.

Not the reason, proven by isolation (all with FootIK on unless stated):

| test | result |
|---|---|
| B: Locomotion forced to 100 % on the first moving frame (no idle crossfade) | identical to HOLD (9 vs 14 transition frames, knees / feet unchanged) — transitions are not the difference |
| C: constant velocity, `IsRunning` faked 15/15 | the state machine never left Locomotion either; identical to HOLD |
| D: TAP with MotionSpeed pinned at the HOLD value 0.857 | plant gets WORSE (mid-stance ankle drift 20–27 mm, ankle speed rms 161–191 mm/s vs 105–113) — the tap's benefit is the slower playback, not the input pattern |
| HOLD with MotionSpeed pinned at the TAP value 0.62 | feet skate (drift 21–34 mm, rms 208 mm/s): slow playback under full speed is not a fix either |
| FootIK OFF, hold and tap | knee reserve, foot pitch sweep, plant yaw all unchanged (R knee < 6°: 33.7 % vs 31.9 %) |

So tapping does not make the feet better; it makes the source clip's defects pass slower and under-driven.

## BAD WALK PHASES (HOLD W, stage C_final, phase = frac(stateNorm + 0.9208), 0 = left heel contact)

Contact sheets (16 frames each): `scratchpad/left/sheet3_*.png` — L push-off 0.55, R heel strike 0.47, R push-off
0.05, L heel strike 0.97.

| phase | support | what the numbers say |
|---|---|---|
| 0.92–0.06 L strike | R → L | left boot arrives **35° toes-up above flat** (toe bone 117 mm, flat is 61), heel 10 mm in the air, knee 4.1–4.5°, hip→ankle 0.999 of the leg: a stiff, straight-leg, heel-in-the-air landing |
| 0.12–0.30 L | L | sole briefly flat (toe bone 44–57 vs flat 61) — the only genuinely loaded window |
| 0.37–0.62 L | L → R | **toe plunge**: the boot pitches toes-down 33 → 49° below flat while the heel stays at 0–3 mm; toe bone 4 mm = 57 mm below flat, i.e. the boot's front is inside the floor; knee re-straightens to 4.1° by 0.58 with reach 0.999; no heel rise at all |
| 0.42–0.50 R strike | L → R | right boot **hovers 10–18 mm above its authored path** (achieved y 90–98 vs target 73–80): the shorter right leg cannot reach heel-strike, lands late and straight (1.9° at runtime) |
| 0.04–0.13 R push-off | R → L | right foot **40–47 mm short of its authored push-off target** (achieved z −271 vs −311 at 0.083): the planted right boot is dragged forward through late stance while the leg is straight (5.4°) |

The 12–13° of planted-foot yaw drift per stance (foot twist is a static constant in the clip; the pelvis yaw
pivots the planted boot) overlays all of it.

## KNEE RESERVE

Raw v012 (Edit Mode, 480 phases) and HOLD (runtime, moving frames):

| below | raw L | raw R | HOLD L | HOLD R | TAP L | TAP R |
|---|---|---|---|---|---|---|
| 4° | 0 % | 0 % | 0 % | 18.8 % | 1.4 % | 22.5 % |
| 6° | 15.2 % | 34.2 % | 6.7 % | 31.9 % | 6.1 % | 32.3 % |
| 8° | 19.2 % | 36.2 % | 9.7 % | 35.4 % | 8.9 % | 34.0 % |
| 10° | 20.6 % | 38.3 % | 10.5 % | 37.8 % | 9.8 % | 35.7 % |
| 12° | 22.3 % | 40.2 % | 11.5 % | 39.9 % | 11.2 % | 39.2 % |
| 15° | 25.0 % | 43.8 % | 14.2 % | 43.7 % | 13.3 % | 42.7 % |

hip→ankle / (thigh + shin): **0.999 both legs** at heel strike AND push-off — a singular straight leg, twice per
cycle per leg. The generator asked for a 15° reserve (`minKneeBendDeg`) and lost it for two reasons, both
measured: (a) `WantHipY` plans the hip height against a nominal ±90 mm hip while the posed hip sits 16–30 mm
further fore/aft (the body frame is mass-weighted; the pelvis slides under it as the legs swing) — 16 mm of
extra span is the entire 15° reserve; (b) the single `legLimit` is derived from the LEFT leg's segments, and this
rig's right leg is 17 mm shorter, so the right leg is planned against a limit it cannot reach and goes straight
at every extreme (right knee < 6° for a third of the cycle). Runtime FootIK then lifts the right ankle 10–15 mm
at heel-off and straightens it further, 4.3° → 1.9°.

## FOOT ROLL

Source: `Left/Right Toes Up-Down` are constant 0 (2 keys), `Foot Twist In-Out` constant (−0.475 / −0.567),
`Foot Up-Down` is the only articulated foot channel (0.64 muscle units). The toe bone has no effect on this rig
anyway (documented), so the boot IS one rigid piece — but the contact still could read human if the sole loaded.
It does not: the foot's pitch is authored as an ankle-local table (−11 → +19°), so in the world the boot follows
the shin. Measured world pitch through stance: **+27° (strike) → −21° (mid) → −55° (push-off)**, an 80° sweep,
where a planted human foot stays within a few degrees of flat until heel-off. Flat on this rig = −8.5° (L) /
−17° (R) ankle→toe, toe bone 61 / 52 mm above the ground (NOT 20 mm — `FootIK.ToeBottomHeight 0.02` is
therefore letting the toe go ~35 mm into the floor before it reacts; noted, not changed).

Rollover stages, raw v012 left stance: heel strike toes-up 35° above flat, heel 10 mm up → sole load at 0.12
(toe 4 mm below flat, already pressing) → mid stance flat to slightly buried → **no heel release**: from 0.37
the toe drives down 28 → 57 mm below flat while the heel stays at 0–3 mm → toe-off from a fully buried toe
with a straight knee. Answer: no genuine rollover; the boot pivots as a rigid lever about the ankle in the wrong
direction (toe down instead of heel up).

Runtime sole-loading per stance (heel = ankle − 70 mm, toe vs the measured flat height):

| | sole flat | heel-only, toe hovering | heel-up rollover | toe buried > 20 mm | toe min vs flat |
|---|---|---|---|---|---|
| v012 HOLD L / R | 46 / 64 % | 9 / 4 % | 4 / 0 % | **35 / 6 %** | −41 / −23 mm |
| v012 TAP L / R | 43 / 96 % | 7 / 0 % | 7 / 0 % | 40 / 0 % | −28 / −6 mm |

## PLANT STABILITY (mid-stance, first/last 4 stance frames excluded)

| | ankle drift mean / max | yaw drift | ankle speed rms / max |
|---|---|---|---|
| HOLD L | 12 / 24 mm | **12.5°** | 105 / 300 mm/s |
| HOLD R | 10 / 12 mm | **13.5°** | 113 / 293 mm/s |
| TAP L | 13 / 25 mm | 4.9° | 112 / 233 |
| TAP R | 13 / 25 mm | 6.4° | 150 / 262 |

Old audits tolerated ~10 mm drift as "no catastrophic skate"; the 12–13° of boot yaw per stance and the 40–47 mm
right push-off drag were never measured.

## STRIDE / REACH

Step 650 mm (±325 authored), max fore/aft foot separation 790 mm at runtime, knee 25.7 / 25.2° at that moment
(the legs are bent at max separation because the hips are lowest there); the straight-leg singularity is at
strike and push-off, not at max separation. The step is not too long for the rig once the hip height is planned
against the true reach: the AB variant keeps the same 650 mm step and the same 1.35 m/s native speed with 15.8°
minimum left knee. Idle blending does not shorten the apparent step at 4 Hz (never enters); at ≤ 1 Hz it does.

## FOOTIK CONTRIBUTION

Before (stage A) vs after (stage C) on HOLD: knee min L 4.2 → 4.2, R 4.3 → **1.9** (the straight-leg response to
the 10–15 mm ankle lift), toe height min L 13 → 18 mm, ankle min 34 → 35, foot pitch sweep unchanged, plant yaw
unchanged. FootIK OFF: every defect present. FootIK amplifies the right knee by 2.4°, nothing else; it is not the
cause and the diagnostic AB variant behaves identically with it on or off.

## FORWARDLEFT CROSS-CHECK (`Sword1H_WalkForwardLeft_v001`, HOLD −45°)

Same structure: R knee < 6° **36.8 %**, knee min L 4.2 / R 1.9, reach 0.999 / 1.000, foot pitch sweep −59 ..
+34°, sole flat 9 / 15 %, toe hovering 64 / 73 % (old reference), **left plant yaw drift 23.6°** (the diagonal
pelvis yaw pivots the planted boot more). Tapping: same numbers at 0.63×. No separate theory is needed: it
inherits the same foot table and the same nominal-hip reach planning, and the fix options apply through the same
code path (the diagonal `FootTarget` branch).

## WHAT OUR OLD QA MISSED

- **Foot pitch in world terms.** We measured ankle heights and toe-BONE heights against a 20 mm assumption; the
  flat toe bone is 52–61 mm up on this rig, so a 40–57 mm toe plunge read as "toes lowest +2.6 mm".
- **Sole loading.** No metric asked whether heel and toe were on the ground together, or whether the heel ever
  lifted. "Feet planted" was ankle-only.
- **Knee reserve as achieved, not as requested.** The 15° dial passed review; the achieved minimum was 4°
  (Edit Mode) / 1.9° (runtime) and we filed it as a probe artefact.
- **Planted-foot yaw.** Never measured; 12–24° per stance.
- **Per-leg reach.** One `legLimit` for two legs of different length.

## MINIMUM FIX HYPOTHESIS (diagnostic variants, NOT installed)

Three single-concept variants were built from the v012 dials through new default-inert `HumanWalkPerformance`
options; each was measured in Edit Mode and the winner on the real Player under HOLD and TAP.

| variant | concept | knee < 6° L / R | reach max | pelvis rhythm | toe min vs flat | fixes |
|---|---|---|---|---|---|---|
| v012 (baseline) | — | 15 / 34 % | 0.999 / 0.999 | 69 mm | −57 mm (buried) | 7 |
| A `plantedFootRoll` | world-flat sole, heel touching at strike, heel-off pivoting about the toe (ankle rises and advances), foot pitch solved per frame after the leg solve | 6 / 30 % | 0.999 | 44 mm (loses the weight beat) | −3 mm | 5 |
| B `reachMarginMm 20` + per-leg limits | hip height planned against each leg's own limit minus the measured hip wander | 5 / 5 % | 0.999 | 88 mm | −80 mm (plunge deeper) | 6 |
| **AB** | both | **0 / 2 %** (L min 15.8°) | 0.990 / 0.999 | **66 mm** | **−3 mm** | **0** |

A and B are complementary: A alone flattens the weight rhythm because the rising ankle at heel-off relaxes the
reach constraint; B alone lowers the hips and buries the toe deeper. Together the sole loads, the heel lifts, the
knee keeps its reserve and the pelvis rhythm returns to Forward's. Native speed unchanged (1.346 m/s); swing peak
134 / 135; 0 legality violations; 0 hold snaps.

**AB on the real Player** (override controller, everything else production):

| | v012 HOLD | AB HOLD | v012 TAP | AB TAP |
|---|---|---|---|---|
| knee < 6° L / R | 6.7 / 31.9 % | **0.0 / 1.6 %** | 6.1 / 32.3 % | 0.0 / 2.3 % |
| knee < 15° L / R | 14.2 / 43.7 % | **0.0 / 3.5 %** | 13.3 / 42.7 % | 0.0 / 9.2 % |
| sole flat L / R | 46 / 64 % | **78 / 78 %** | 43 / 96 % | 83 / 88 % |
| heel-up rollover L / R | 4 / 0 % | **9 / 8 %** | 7 / 0 % | 6 / 7 % |
| toe buried > 20 mm | 35 / 6 % | **0 / 0 %** | 40 / 0 % | 0 / 0 % |
| plant yaw drift L / R | 12.5 / 13.5° | **2.5 / 10.8°** | 4.9 / 6.4° | 3.3 / 10.1° |
| mid-stance ankle speed rms L / R | 105 / 113 mm/s | 104 / 82 | 112 / 150 | 112 / 160 |
| pelvis rhythm | 70 mm | 66 mm | 72 | 66 |
| FootIK off | same defects | identical to FootIK on | | |

HOLD on AB is now at least as planted as TAP was on v012 on every measure, which is the user's exact signal.
Remaining after AB, for the artistic repair to judge: right plant yaw ~11° (the pelvis-yaw pivot; a Foot Twist
counter-rotation during stance is the untested third concept), right knee still touching 4.3° for 1.6 % of the
cycle at push-off (the 17 mm shorter leg at the far reach; `strideScale` 0.95 would clear it), and the
heel-off angle (32°) / strike toes-up (12°) are first-guess profile values, not art-directed ones.

## VIDEOS (`Artifacts/AnimationReview/`)

- `FootForensic_2x2_v012HOLD_v012TAP_diagABHOLD_diagABTAP.mp4` — top-left v012 HOLD, top-right v012 TAP,
  bottom-left diagnostic AB HOLD, bottom-right diagnostic AB TAP (the user's signal on the candidate); same
  camera, lane, loadout, speed, 60 fps, 10 s. `FootForensic_2x2_LOWERBODY_60fps.mp4` — the same, lower-body crop.
- `FootForensic_v012_HOLD_vs_TAP_legs.mp4` — legs only, side by side. Singles: `FootForensic_v012_w_hold.mp4`,
  `FootForensic_v012_w_tap.mp4`, `FootForensic_diagAB_ab_hold.mp4`, `FootForensic_diagAB_ab_tap.mp4`.
- Note the lighting: the start of the lane sits under the candle lights, so the slower TAP runs stay gold-lit
  longer; it is scene lighting, not a material change.

## PRODUCTION SAFETY

Unchanged through the diagnosis: `Sword1H_WalkForward_v012` `530b8133`, `Sword1H_WalkForwardLeft_v001`
`1fb5e646`, Fresh02 `a1630664`, ForwardLeft02 `b4c2f052`, Left01 `88cba72b`; `Knight_Controller.controller`
`816e431d` and `GuardGait_Knight1H.asset` `3bc3ebda` as they were at the start of this milestone; FootIK
settings untouched; `VerifyProduction` CLEAN (the `__diag_*` clips are referenced nowhere). The test run
re-serialised the FootIK component in `Player.prefab` again (field order + explicit defaults, semantics identical);
reverted to HEAD. Harness additions (diagnostic only): transition / input / state-weight columns, `RTQA_forceLoco`,
`RTQA_fakeBlend`, `RTQA_lockMS`, override now targets v012. Generator additions, all default-inert:
`plantedFootRoll` (+ profile dials), `reachMarginMm` with per-leg limits, `trueHipReach` (tested, rejected — an
analytic hip cannot follow the mass-weighted body frame). Regression suite **40 passed / 1 failed / 0 skipped**. The failure is
`N1_LightAndTravelWalk_DoNotShareTheirForwardClip` and it pre-dates this forensic: the user-requested install of
v012 into `Travel_Walk8` (free-travel forward, previous task) made both trees reference the same clip asset, which
that test forbids because a clip-level swap reaches both trees (the diagnostic override did exactly that: "1 slot"
replaced v012 in Light AND Travel). Two ways out, neither taken here: give Travel its own key-identical copy
(`Sword1H_WalkForward_v012_Travel`), or restore v008 in Travel. Every other test, including the four generator
contracts (V1–V3, U2), passes with the new options inert.

`FOOT REALISM ROOT CAUSE ISOLATED — READY FOR ARTISTIC REPAIR`
