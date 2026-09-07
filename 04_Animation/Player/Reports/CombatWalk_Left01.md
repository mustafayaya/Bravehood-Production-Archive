# `__freshHumanLeft_01` — first true-Left (−90°) research challenger (human review pending)

Research clip only. Authorities: `Sword1H_WalkForward_v012` (0°) and `Sword1H_WalkForwardLeft_v001` (−45°), both
untouched, both still installed; no Left is installed, no GuardGait entry added. Every number is the finished clip
sampled in Edit Mode at 480 phases on the Player prefab, holder-relative, root canonical during sampling
(|yaw| 0.00°, |pos| 0.00 mm).

```
TECHNICAL QUALIFICATION   QUALIFIED - safe to review (see TECHNICAL QA)
ARTISTIC MOTION APPROVAL  PENDING - human review on the gameplay-camera videos
```

## MOTION DESIGN

A true −90° strafe is NOT the Forward walk turned sideways: an alternating fore/aft gait at 90° would have to
cross its legs or turn the pelvis to the travel direction (the legacy mocap ring child does exactly that — hips
80–91° toward travel, shoulders 48–64°, functional reference only). The Knight instead uses a **lateral step-close
gait**: the LEFT (travel-side) foot opens along travel, the body transfers onto it, the RIGHT foot pushes and
closes, with two double-support phases per cycle. The Player root keeps facing the threat; the pelvis leads
into travel by ~38°, the chest stays at ~12°, the head on the threat.

Authored with a new, default-inert lateral mode on `HumanWalkPerformance.Options` (`lateralStepClose`), on top
of the same leg solver, reach-derived hip height, weight beat, overlap chain, arm-lag architecture and the v012
in-place clip contract:

| dial | value | meaning |
|---|---|---|
| cycleSeconds | **0.60** | see NATIVE TRAVEL SPEED — a step-close's maximum stance width is its minimum width plus speed × cycle |
| lateralDriftMm / swingFrac | 273 / 0.35 | stance drift per foot along travel; swing 35 % of the cycle per foot |
| leadLandMm / trailLandMm | 340 / −40 | lead lands 340 mm along travel from the root, trail closes to 40 mm behind |
| lateralSwingHeightMm | 80 | low lateral step (swing peak 153 mm ankle) |
| lateralStaggerMm | 100 | lead foot toward the threat, trail behind it (fighting stance kept while strafing) |
| lateralSwayMm | 50 | pelvis shifts along travel over the loaded foot |
| yawTowardTravelDeg / chestFollowFrac / headYawDeg | −21 / −0.03 / +10 | body-frame yaw; twist; head back to the threat |
| stanceKneeBendDeg / minKneeBendDeg / armGain | 38 / 15 / 0.3 | crest cap (a shuffle rides lower than a walk); Forward reserve; quieter arms |

## LEAD / TRAIL LEG STRATEGY

| phase | lead (left) | trail (right) | support |
|---|---|---|---|
| 0.00–0.35 | swings out along travel (reach 340 mm) | stance, drives the body left | single, right |
| 0.35–0.50 | lands, accepts weight (sink 14 mm peaks at 0.48) | stance, pushes | **double** |
| 0.50–0.85 | stance, drifts back under the body | swings in, closes to 40 mm behind the root | single, left |
| 0.85–1.00 | stance | lands (sink peaks at 0.98) | **double** |

The left leg is the opening/reaching leg (hip In-Out 0.01..0.24 = abduction only), the right leg the driving and
recovering leg (In-Out −0.20..0.16, adduction through its push). Neither foot ever passes the other: lead ahead
of trail along travel by **337..759 mm** across the whole cycle. No grapevine, no crossover.

## FOOT TRACKS

| | value |
|---|---|
| stance ground-track heading, L / R | **−91.1° / −90.2°** |
| lead-ahead-of-trail along travel | 337 .. 759 mm (stance width min / max) |
| stagger toward the threat (L − R) | 112 .. 136 mm |
| stance foot height L / R (floor 73) | 72.4..85.3 / 72.2..85.2 mm (12 mm ankle rise at the widest reach — solver residual at the reach limit, see FOOTIK READINESS) |
| toes lowest L / R | 56.4 / 25.7 mm |
| swing peak L / R | 153 / 153 mm |
| perpendicular (toward-threat) stance slide | mean 109 mm/s, max 1172 (probe includes landing/lift frames; v012's own probe reads 129) |
| planted-foot twist | none authored; feet stay pointed at the threat |

## SUPPORT / WEIGHT TRANSFER

Contact → acceptance → compression → transfer → propulsion → recovery, twice per cycle: pelvis height
**707..767 mm, rhythm 59.5 mm** (ForwardLeft 59, Forward 69), the troughs at the two landings (0.48 / 0.98) after
the feet arrive (mass arrives after the foot, as in Forward), pelvis shifting **105 mm along travel** across the
cycle — over the trail foot while the lead reaches, over the lead foot once it has taken the load. Knee bend
peaks at 48.6° / 49.7° at the widest stance. The pelvis does not float sideways over cycling feet: the body drops
onto the lead leg and rises through the trail push.

## PELVIS / TORSO / HEAD

| | Left01 | ForwardLeft_v001 | v012 |
|---|---|---|---|
| hip-line yaw range · mean (+ = toward travel) | 31.8..44.2 · **38.0°** | 21.4..42.4 · 31.9 | −6.8..14.3 · 3.8 |
| shoulder-line yaw range · mean | 9.8..13.5 · **11.6°** | 7.0..13.1 · 10.1 | −4.1..2.0 · −1.0 |
| pelvis-to-shoulder separation | 26° | 22° | 5° |
| head vs v012 head | −2.9° (toward travel) | −3.3 | 0 |

More lateral pelvic commitment than ForwardLeft, as expected, but nowhere near the mocap's 85°: the chest stays
combat-readable and the spinal twist (26° pelvis→shoulders) is inside what the ForwardLeft review accepted as
"connected".

## SWORD / SHOULDER

Right Shoulder Down-Up 0.092..0.106 (Forward 0.081..0.117 — no compression, no drop), blade orientation range
**4.9°** (Forward 8.7, ForwardLeft 9.2), sword-hand path 360 mm / cycle (ForwardLeft 473). armGain 0.3 keeps the
arm-lag architecture but quiets the swing — a shuffle does not pump its arms; the guard rides the moving body.

## NATIVE TRAVEL SPEED

- native planted-foot travel: **0.700 m/s** by construction (273 mm / (0.6 s × 0.65)); the schedule-window probe
  reads 0.677 because it includes the landing/lift frames
- heading: −91.1° / −90.2° (root frame)
- cycle: **0.600 s** · cadence: **200 spm** (one step per foot per cycle)

Why not 0.8 s / 1.3 m/s like the walks: for a step-close gait each foot must travel speed × cycle per cycle in
ONE step, and the maximum stance width = minimum width + speed × cycle. At 0.8 s the first build needed 0.64 m
steps and a 1.0 m stance, which is beyond this rig's 731 mm leg — the solve diverged. 0.6 s at 0.70 m/s gives
0.42 m steps and a 0.34–0.76 m stance: a committed lateral step, not a lunge.

## EXPECTED GAMEPLAY PLAYBACK

The controller taxes lateral movement by forwardness (0.8 + 0.2·cos θ → **0.80 at −90°**):

| | ground speed | playback = speed / 0.70 | cadence |
|---|---|---|---|
| A. unarmoured (1.30 m/s forward) | 1.040 m/s | **1.49×** | 297 spm |
| B. gameplay starting loadout (1.149) | 0.919 m/s | **1.31×** | 263 spm |

Both inside `MotionSpeedClamp` (0.6–1.5); A sits at the clamp edge. This is the honest cost of a no-crossover
strafe at the controller's lateral speed — the slower micro-study candidate (0.62 m/s) would have needed 1.68×
and skated. Human review should judge whether the gameplay-rate read is acceptable or whether the lateral
speed tax is the thing to revisit (a controller decision, not an animation one).

## TECHNICAL QA

- Interpolated Humanoid legality: **0 violations**, 95 informational (= v012); build-time span flattenings 6.
- Temporal audit 60 Hz at 1.0×: **0 hold snaps**; top per-frame deltas L knee 22.1 mm @0.25, R knee 16.4 @0.31,
  L hip 11.3, R hip 9.9, ankles 1.5, pelvis 1.2, chest 0.5 — v012: 23.5 / 23.4 / 14.8 / 9.8 / 3.6 / 1.3 / 0.46;
  ForwardLeft_v001: 24.3 / 23.3 / 14.7 / 10.3 / 3.6 / 1.3 / 0.46. Same texture at a shorter cycle.
- Support schedule: two single-support and two double-support phases as authored; swing-foot trajectory: 153 mm
  peak, 80 mm authored bell, no toe penetration (toes ≥ 25.7 mm).
- Seam: periodic basis, 36 keys / curve at 60 fps over 0.6 s, cyclic smoothing, loop clean.
- Deterministic generation: two builds, 102 bindings, max |diff| **0**.
- Root canonical during sampling: 0.00 mm / 0.00°.
- Clip contract: loopTime, loopBlend off, orientation / Y / XZ baked, Based Upon Original, heightFromFeet off.
- Regression suite 41 / 41 after the generator change (lateral mode inert at defaults — every earlier build
  reproduces).

## FOOTIK READINESS

Feet are planted flat with the ankle on the 73 mm floor and toes ≥ 25.7 mm above it through every stance; the
only raw blemish is a 12 mm ankle rise for a few frames at the widest reach (solver residual at the leg limit),
which `FootIK.Solver = AnimatedBones` will settle at runtime — nothing is being rescued.

## GAMEPLAY VIDEOS (`Artifacts/AnimationReview/`)

- `Left01_RAW_native.mp4` — real gameplay camera rig, Warrior Base review set, character translated at the native
  0.70 m/s along −90°, clip at 1.0×, 60 fps, 8 s.
- `Left01_RAW_gameplayLoadout.mp4` — translated at the starting-loadout lateral speed 0.919 m/s, clip at 1.313×.
- `Family_Forward_ForwardLeft_Left01_context.mp4` — Forward (Fresh02 RAW reference at 0.97×) | ForwardLeft02
  native | Left01 native, side by side: 0° → −45° → −90°, for coherence, not as an A/B.

## FAMILY COMPARISON

| | Forward v012 | ForwardLeft v001 | Left01 |
|---|---|---|---|
| gait | alternating walk | alternating walk, hip-relative diagonal lines | step-close |
| cycle / native | 0.8 s / 1.29 m/s | 0.8 s / 1.24 | 0.6 s / 0.70 |
| pelvis lead / chest | 4° / −1° | 32° / 10° | 38° / 12° |
| pelvis rhythm | 69 mm | 59 | 60 |
| swing peak | 134 mm | 134 | 153 |
| blade range / R shoulder | 8.7° / 0.081..0.117 | 9.2° / same | 4.9° / 0.092..0.106 |
| foot separation / crossing | 166..182 across | 157..174 across | 337..759 along, 112..136 across; none |

## PRODUCTION SAFETY

From disk: `Knight_Controller.controller` Light_Walk8 child 0 = v012 (`b9d47c80…`) at 0°, child 6 = ForwardLeft_v001
(`910fd344…`) at −45.0°, child 5 = `Sword1h_Strafe135LeftLoop` at −97.1° untouched, child 7 parked; Travel_Walk8
unchanged. `GuardGait_Knight1H` walk: −141.6 : 1.84 · −97.1 : 1.82 · −45 : 1.24 · 0 : 1.34 · 86.3 : 1.81 ·
125.3 : 1.84 · 170.2 : 1.80 (no Left entry). Left01 guid `f4d02cdf…` referenced nowhere; no `__fresh*` guid in the
controller. md5: Fresh02 `a1630664`, ForwardLeft01 `612bccab`, ForwardLeft02 `b4c2f052`, v012 `530b8133`, v001
`1fb5e646`, v008 `7a7ca074` unchanged; controller `d16eb7f7`, GuardGait `3bc3ebda` unchanged since the ForwardLeft
install; Player.prefab `67ef0650` (a Unity re-serialisation of the FootIK component during the test run — field
order and explicit defaults, `Solver: 0` / `Policy: 1` unchanged — was reverted to HEAD). `AnimationAssetSafety.VerifyProduction` CLEAN ("no research clip referenced by the production controller"). Regression suite 41 / 41.

`FRESH HUMAN LEFT 01 READY FOR HUMAN REVIEW`
