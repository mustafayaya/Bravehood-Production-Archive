# Foot realism artistic repair — `__freshHumanForward_03` / `__freshHumanForwardLeft_03`

Research clips built on the proven AB foundation (planted world-space foot roll + per-leg reach), art-directed
rollover, and planted-foot yaw stabilisation. `Sword1H_WalkForward_v012` and `Sword1H_WalkForwardLeft_v001` are
untouched and still installed; nothing new is installed; no GuardGait / FootIK / prefab change.

```
TECHNICAL QUALIFICATION   QUALIFIED - safe to review (0 legality violations, 0 hold snaps, deterministic, baked)
ARTISTIC MOTION APPROVAL  PENDING - four-way and HOLD/TAP videos for human review
```

## TRAVEL ASSET ISOLATION

`Sword1H_WalkForward_v012_Travel.anim` (guid `6cc18a07…`) = `Object.Instantiate(v012)` + the same clip settings:
102 / 102 bindings, 1661 keys, max |Δ| 0 in value, tangent and time, settings equal. `Travel_Walk8` child 0 now
references it (position (0, 1), timeScale 1, cycleOffset 0 — the slot exactly as it was on disk); `Light_Walk8`
child 0 stays `Sword1H_WalkForward_v012` (`b9d47c80…`). `N1_LightAndTravelWalk_DoNotShareTheirForwardClip` passes
again; `W1_TravelForwardCopy_IsKeyIdenticalToProductionForward` guards the copy's motion identity.

## FORWARD03 CONTACT DESIGN

v012's own dials (minKnee 15, stanceKnee 32, swingHeight 0.62, armGain 0.5, the same 650 mm step and 0.8 s cycle)
plus, on `HumanWalkPerformance.Options` (generator stamp `v11-planted`, every option default-inert):

| option | value | concept |
|---|---|---|
| `plantedFootRoll` + profile | strike 12° toes-up · sole-load 0.07 · heel-off from 0.46 to 32° at toe-off 0.62 | HEEL CONTACT → SOLE ACCEPTANCE → LOADED MID-STANCE → HEEL RELEASE → FOREFOOT SUPPORT → TOE RELEASE. The Foot Up-Down muscle is solved per frame AFTER the leg solve so the boot's WORLD pitch follows this profile (flat = the planted approved pose's ankle→toe pitch, −8.5° L / −17.0° R). The ankle rises and advances as the heel lifts about the planted toe, and rides up at strike so the heel touches. |
| `reachMarginMm 20`, `rightReachExtraMm 8` | per-leg limits | `WantHipY` plans each leg against its own thigh+shin (716 vs 733 mm) minus the hip-wander margin |
| `stanceYawStabilize 1.0`, release 0.58–0.66 | planted heading | see PLANTED YAW |

Rollover visual study (three profiles, everything else fixed; sheets in `scratchpad/left/roll_*_sheet.png`,
videos `RollStudy_{restrained,medium,strong}_RAW.mp4`): RESTRAINED (8° / 24°, load 0.06, heel-off 0.48) still reads
flat-footed and stiff at heel release; STRONG (16° / 40°, 0.08, 0.44) lifts the heel high enough to read as a brisk
march; **MEDIUM (12° / 32°, 0.07, 0.46) shows a clear heel release with the forefoot still down and a moderate
toes-up strike — the grounded armoured Knight. Selected by eye**, not by the sole-flat percentage (all three sit at
40–47 %).

Preserved (Edit-Mode, Forward03 vs v012): hip-line mean 3.8° (3.8), shoulder-line −1.1° (−1.0), pelvis rhythm 66 mm
(69), swing peak 134 / 135 mm, blade orientation range 9.1° (8.7), Right Shoulder Down-Up 0.081..0.117 (identical),
native planted-foot speed 1.30–1.33 m/s, 0.8 s cycle, 102 bindings.

## PER-LEG REACH

| | v012 L / R | Forward03 L / R |
|---|---|---|
| knee minimum | 4.2° / 4.3° | **15.8° / 4.3°** |
| below 6 / 10 / 15° (of cycle) | 15 / 20 / 25 % · 34 / 38 / 43 % | **0 / 0 / 0 % · 1 / 2 / 3 %** |
| hip→ankle / own leg length, max | 0.999 / 0.999 | 0.991 / 0.999 |

## WORLD-SPACE ROLLOVER (rig-calibrated audit, 240 samples)

| | v012 L / R | Forward03 L / R |
|---|---|---|
| boot pitch vs flat through the cycle | −48..+36° · −29..+51° | **−32..+12° · −32..+12°** |
| heel min / toe min (bottom clearance) | 3 / −53 mm · −14 / −27 | 4 / −2 · −11 / 2 |
| sole flat (heel & toe ≤ 15 mm) | 5 % · 30 % | **43 % · 45 %** |
| heel-only (toe hovering) | 9 % · 14 % | 0 % · 6 % |
| heel-off (heel up, toe down) | 43 %* · 22 %* | 19 % · 7 % |
| toe buried > 20 mm | **34 % · 6 %** | **0 % · 0 %** |

*v012's "heel-off" is the toe plunge (toe below the floor with the heel down), not a heel rise.

## PLANTED YAW

Foot Twist In-Out ROLLS the boot (measured: −1..+1 changes the heading by 1° and the roll by 0.9), so it cannot hold a
heading. The authority is thigh axial rotation: while a foot is loaded the four leg muscles (Front-Back, In-Out,
Stretch, Twist) are re-solved together against the ankle position the leg solve achieved plus a world-heading
target — the boot's heading at sole-load — ramping in over 0.07–0.12, held through mid-stance AND heel-off (the
forefoot pins the boot until toe release), released over 0.58–0.66. Twist muscle excursion ±10°. Two lessons cost a
build each: the pass must run AFTER the planted-pitch pass (a rolled boot's projected heading shifts with its pitch;
solving inside the leg solve held the wrong heading, −35° instead of −24°), and releasing at heel-off let the boot
swing 16° with the toe still flat on the floor.

Yaw study (50 / 75 / 100 % on the medium profile, release at heel-off): Forward support yaw drift L 6.1 / 5.2 / 4.3°,
R 3.0 / 0.6 / 1.8° (baseline 7.9 / 7.9). 100 % was needed for the diagonal; with the release moved to toe-off:

| support yaw drift (middle 70 % of support) | v012 / ForwardLeft02 | Forward03 / ForwardLeft03 |
|---|---|---|
| left boot | 10.4° / **30.5°** | **0.8° / 0.6°** |
| right boot | 36.1° / 25.8° | **1.8° / 1.7°** |

The swing foot still re-orients freely (Forward03's right boot turns ~30° in early swing, as v012's does).

## RIGHT LEG RESIDUAL

With per-leg limits and 20 mm the right knee still touched 4.3° for 2 % of the cycle at far push-off; `rightReachExtraMm 8`
takes it to 1 % (about one 60 Hz frame per cycle) without touching the stride. Stride, step and native speed are
unchanged. Whether that frame reads on screen is for the review videos; the next minimal step, if it does, is a few
more millimetres of right reserve, not `strideScale`.

## FORWARDLEFT03

`__freshHumanForwardLeft_02`'s approved dials (travel −45, yaw −17.7, chestFollow 0.034, head +8, stride 0.94, widen 6)
plus the identical contact options — same philosophy, same code path. Preserved: hip-line mean 31.9° (31.9), shoulders
10.1° (10.1), swing 134 / 135, blade 9.2°, shoulder curve identical, pelvis rhythm 55 mm (59). Contact: knee min L 12.1°
/ R 23.2° (was 4.2 / 4.3; below 6°: 0 / 0 % vs 7 / 38 %), pitch −32..+12° (was −50..+42 / −31..+49), toe buried 0 %
(was 32 / 6 %), sole flat 44 / 45 % (13 / 29), support yaw **0.6° / 1.7°** (was 30.5 / 25.8).

## HOLD vs TAP

Real Player, production stack, gameplay starting loadout, lock-on held, FootIK AnimatedBones, all writers on; Forward03 /
ForwardLeft03 through the in-memory override (Light slot only — Travel now carries its own clip). HOLD = idle 2 s → 6 s
→ idle; TAP = 0.25 s press / 0.25 s release ×12 (the user's rhythm; the Animator never leaves Locomotion at it).

| | v012 HOLD | Forward03 HOLD | v012 TAP | Forward03 TAP |
|---|---|---|---|---|
| knee < 6° L / R (moving frames) | 6.7 / 31.9 % | **0.0 / 1.1 %** | 6.1 / 32.3 % | 0.0 / 2.0 % |
| knee < 15° L / R | 14.2 / 43.7 % | **0.0 / 2.4 %** | 13.3 / 42.7 % | 0.0 / 2.9 % |
| sole flat L / R | 46 / 64 % | **78 / 79 %** | 43 / 96 % | 83 / 88 % |
| heel-up rollover L / R | 4 / 0 % | **8 / 8 %** | 7 / 0 % | 6 / 7 % |
| toe buried > 20 mm L / R | 35 / 6 % | **0 / 0 %** | 40 / 0 % | 0 / 0 % |
| toe min vs flat L / R | −41 / −23 mm | +4 / +7 | −28 / −6 | +3 / +5 |
| planted-boot yaw drift L / R | 12.5 / 13.5° | **0.3 / 2.0°** | 4.9 / 6.4° | 0.4 / 8.5° |
| mid-stance ankle drift L / R | 12 / 10 mm | 11 / 11 | 13 / 13 | 14 / 16 |
| pelvis rhythm | 70 mm | 75 mm | 72 | 75 |

| | ForwardLeft_v001 HOLD | ForwardLeft03 HOLD | ForwardLeft_v001 TAP | ForwardLeft03 TAP |
|---|---|---|---|---|
| knee < 6° L / R | 6.2 / 36.0 % | **0.0 / 0.0 %** | 5.3 / 36.5 % | 0.0 / 0.0 % |
| knee < 15° L / R | 11.8 / 47.6 % | **3.5 / 0.0 %** | – | 3.5 / 0.0 % |
| sole flat L / R | 49 / 63 % | **80 / 77 %** | 46 / 98 % | 79 / 89 % |
| toe buried > 20 mm L / R | 30 / 13 % | **0 / 0 %** | 39 / 0 % | 0 / 0 % |
| planted-boot yaw drift L / R | **23.5 / 11.0°** | **0.2 / 1.1°** | 8.2 / 6.0° | 0.2 / 4.9° |
| pelvis rhythm | 67 mm | 66 mm | – | 64 |

HOLD on the repaired clips is now more planted than TAP ever was on production, and TAP on the repaired clips
differs from HOLD in speed and cadence only (MotionSpeed 0.625 vs 0.834; the contact numbers match). Tapping is no
longer the way to make the walk look human. FootIK on / off on the AB foundation was identical (forensic); the
repaired clips keep the same per-frame foot steps (46–49 mm) and FootIK settle (pelvis −2 mm) as v012.

## FOOT CONTACT AUDIT

`Editor/LocomotionFootContactAudit.cs` — `Run(animator, clip, plantedPose, samples)` → per leg: leg length, flat
ankle / toe-bone heights and flat pitch (calibrated from the planted approved pose, heel and toe bottoms carried in
each foot bone's local frame), achieved knee minimum, % of cycle below 6 / 8 / 10 / 12 / 15°, hip→ankle / own leg,
world pitch vs flat, heel / toe bottom clearance minima, support / flat / heel-only / heel-off / buried fractions,
support yaw drift, lateral drift and along-track speed over the middle 70 % of the longest support run. Reports only;
no artistic thresholds. Rig calibration: flat toe bone 61 mm (L) / 53 mm (R) above the floor — `FootIK.ToeBottomHeight
0.02` under-reads the boot's toe by ~35 mm; recorded as terrain-work debt, not changed here.

## TECHNICAL QA

Forward03 and ForwardLeft03: interpolated legality 0 violations (95 informational, = v012), build-time flattenings 0,
temporal audit 0 hold snaps (top deltas R knee 29 mm @0.48 / R ankle 16 — the heel-off pivot; FL03 L knee 13.7),
deterministic (two builds, max |Δ| 0), root canonical, orientation / Y / XZ baked, 0.8 s, 102 bindings.

Tests: **44 passed / 0 failed / 0 skipped** — the 41 previous (N1 passing again after Phase 0) + `W1_TravelForwardCopy_IsKeyIdenticalToProductionForward`,
`W2_PerLegReachMargin_RaisesAchievedKneeReserve` (per-leg limits at least halve the time below 6° on both legs; fixture asserts the
right leg is the shorter one), `W3_FootContactAudit_RunsWithCalibratedRigGeometry` (flat toe bone 40–80 mm, finite metrics, support found,
character restored). No rollover angles are encoded. Note for future runs: the generator keeps small static calibration state
(`legLimitSide`, `flatPitchRef`); running `execute_code` builds concurrently with the test runner once made V3 read a 0.009 coupling
that is 1.5e-4 when the suite runs alone.

## HUMAN REVIEW VIDEOS

`Artifacts/AnimationReview/`, real gameplay camera, Player, starting loadout, lock-on, stride-matched playback, 60 fps, 10 s:

- `FootRepair_4way_v012HOLD_F03HOLD_FLv001HOLD_FL03HOLD.mp4` — top-left production v012 HOLD W, top-right Forward03 HOLD W,
  bottom-left production ForwardLeft_v001 HOLD, bottom-right ForwardLeft03 HOLD.
- `FootRepair_4way_LOWERBODY_60fps.mp4` — the same four, lower-body crop.
- `FootRepair_F03_HOLD_vs_TAP.mp4`, `FootRepair_FL03_HOLD_vs_TAP.mp4` — the user's original test on the repaired clips.
- Singles: `FootRepair_f03_hold / f03_tap / fl03_hold / fl03_tap / flp_hold.mp4`; rollover study `RollStudy_{restrained,medium,strong}_RAW.mp4`.

## PRODUCTION SAFETY

`Sword1H_WalkForward_v012` `530b8133` and `Sword1H_WalkForwardLeft_v001` `1fb5e646` unchanged; Light_Walk8 child 0 = v012,
child 6 = ForwardLeft_v001; GuardGait `3bc3ebda` unchanged; FootIK settings untouched; Player.prefab at HEAD. The only
controller edit is Travel_Walk8 child 0 → `Sword1H_WalkForward_v012_Travel` (Phase 0). `VerifyProduction` CLEAN ("no research clip referenced by the production controller"); Light_Walk8 child 0 v012 / child 6 ForwardLeft_v001, Travel_Walk8 child 0 v012_Travel confirmed in memory and on disk.
Forward03 / ForwardLeft03 reached the Player only through the in-memory override controller.
