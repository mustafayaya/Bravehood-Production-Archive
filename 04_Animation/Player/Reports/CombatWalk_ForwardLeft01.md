# `__freshHumanForwardLeft_01` — first ForwardLeft research challenger (human review pending)

Research clip only. `Sword1H_WalkForward_v012` is the motion-language authority; Fresh02 (`a1630664`) and v012
(`530b8133`) untouched; production Light_Walk8 Forward = v012, Travel_Walk8 Forward = v008; **no production
ForwardLeft was replaced**. Every number below is the finished clip, sampled in Edit Mode at 480 phases on the
Player prefab (holder-relative, root untouched: 0.0 mm / 0.0° root motion during sampling), the same probe run
on v012 for the like-for-like column.

```
TECHNICAL QUALIFICATION   QUALIFIED - safe to review (see TECHNICAL QA)
ARTISTIC MOTION APPROVAL  PENDING - human review on the gameplay-camera videos
```

## MOTION DESIGN

Forward intent with lateral displacement, not a side shuffle: the Forward walk's authored foot paths, weight
beat, pelvis rhythm, arm and sword discipline are kept, and the whole gait is **re-aimed along the true gameplay
travel heading of −45°** while the root (and the character's threat facing) stays where it is. The character
walks toward front-left with the pelvis leading the turn and the chest and eyes held on the threat.

Dials (all Fresh02 values kept: minKnee 15, stanceKnee 32, swingHeight 0.62, armGain 0.5), new diagonal
options on `HumanWalkPerformance.Options`:

| option | value | meaning |
|---|---|---|
| `travelDeg` | −45 | foot lines and lateral sway run along this heading |
| `yawTowardTravelDeg` | −18.5 | RootQ (body-frame) yaw toward travel |
| `chestFollowFrac` | −0.195 | spine/chest counter-twist = yaw × (1 − frac) = 22.1 muscle-deg |
| `headYawDeg` | +8 | explicit NeckTurn back toward the threat |
| `strideScale` | 0.94 | along-travel stride (micro-study below) |
| `stanceWidenMm` | 6 | extra stance width per foot |

Legacy mocap ForwardLeft was used only as a functional reference (pelvis ~10° toward travel, outer foot reaching
out, inner foot never crossing the midline); nothing was authored to its rotated import heading.

## FOOT / SUPPORT STRATEGY

Each foot's line runs along travel **under its own hip**. The hips sit on the pelvis line, yawed by the body-frame
yaw, so in the travel frame a hip projects to (±h cos d, ±h sin d), d = travel − yaw, h = the 90 mm nominal hip
`WantHipY` already uses; the Forward pattern's lateral placement (−95..−78 mm, i.e. 5–12 mm inside the hip) is
applied relative to that hip, exactly as it is on a straight walk. The right leg keeps its 0.985 step shortening.

Why not simply rotate the Forward foot lines about the root (first build): with the pelvis yawed only part-way
toward travel the inner (right) foot line crossed 31–103 mm past the outer one, and both knees sat at their
straight-leg limit. Rejected on measurement, not taste.

Measured (this clip · v012 with the same probe):

| | ForwardLeft_01 | v012 |
|---|---|---|
| stance ground-track heading, L / R | **−46.1° / −43.9°** (over 588 mm) | 0° by construction |
| foot separation ⟂ travel (R − L) | 158 .. 175 mm | 166 .. 182 |
| stance foot height L / R (floor 73) | 69.7..78.6 / 69.8..94.7 (heel-off) | 66.4..73.6 / 66.8..90.2 |
| toes lowest L / R | 0.9 / 22.5 mm | 2.6 / 20.3 |
| swing peak L / R | 134 / 134 mm | 134 / 134 |
| planted-foot perpendicular slide (probe window incl. heel-off / strike) | mean 154, max 829 mm/s | mean 129, max 879 |
| knee-angle probe min L / R | 4.1 / 4.2° | 4.1 / 4.2 (probe artefact of the rig's bone skew, identical) |

No crossing, no skating beyond the Forward clip's own probe noise, no penetration.

## PELVIS / TORSO STRATEGY

RootQ is Unity's mass-weighted body frame, **not the pelvis**: a −28° RootQ yaw with a 22° chest counter-twist
put the pelvis at −45° and the chest at −18° (measured 0.44 pelvis / 0.56 chest weighting). The dials were then
solved for the intended pose and re-measured:

| | ForwardLeft_01 (yaw range · mean) | v012 |
|---|---|---|
| hip-line yaw (+ = toward travel/left) | 25.2 .. 46.3 · **35.8** | −6.8 .. 14.3 · 3.8 |
| shoulder-line yaw | 5.9 .. 12.0 · **9.0** | −4.1 .. 2.0 · −1.0 |
| head yaw vs v012 head at the same phase | −2.3 .. −2.1 (2° toward travel) | 0 |
| pelvis vertical rhythm | 59.2 mm | 69.2 |
| lateral sway (RootT, baked) | ±29 mm, along ⟂ travel | ±29 mm |

So: pelvis leads ~32° into the travel direction with the Forward clip's own ±10° oscillation on top, chest
~10° (mostly square to the threat), head on the threat within 2°. The pelvis rhythm is 10 mm shallower than
Forward — a consequence of the shorter diagonal stride through the reach-derived hip height.

## SWORD / SHOULDER STRATEGY

Untouched from Forward: Right Shoulder Down-Up 0.081..0.117 (identical), blade orientation range 9.2°
(Forward 8.7), sword-hand path 477 mm / cycle in the root frame (Forward 435; the extra is the pelvis-relative
counter-twist carrying the hand). No new arm authoring: the guard stays a guard while the body turns under it.

## NATIVE TRAVEL SPEED

Planted-foot ground track along −45°: **1.241 m/s** (Forward v012 same probe: 1.287). Cadence 150 spm at 1.0×.
GuardGait entry measured from the finished clip: `{ angleDeg −45, metresPerSecond 1.241 }` — NOT installed
anywhere; at the gameplay starting loadout (1.149 m/s) the runtime rate would be 0.926×, unarmoured (1.30) 1.048×.

## TECHNICAL QA

- Interpolated Humanoid legality (`InterpolatedHumanoidLimitCheck`, 24 samples/span): **0 violations**, 95
  informational findings — the same count as v012. Build-time span flattenings: 6 (v012's family: 7–8).
- Temporal audit (`LocomotionTemporalAudit`, 60 Hz): 0 hold snaps; top per-frame deltas L knee 24.1 mm @0.92,
  R knee 20.6 @0.17, hips 13.2 / 10.2, ankles 3.6, pelvis 1.4, chest 0.46, sword hand 0.18 — v012: 23.5 / 23.4 /
  14.8 / 9.8 / 3.6 / 1.3 / 0.46 / 0.19. Same texture.
- Loop: authored periodic basis; 102 bindings, 0.8 s, 49 keys per curve.
- Clip settings: loopTime, loopBlend off, orientation / Y / XZ baked, Based Upon Original, heightFromFeet off —
  the v012 production architecture. (The first build had orientation and XZ as root motion: Edit-Mode sampling
  turned the root by the pelvis yaw and the ground track read −17°; fixed in `ApplyInPlaceClipSettings`.)
- Stride micro-study (one bounded study, two builds, all else equal): strideScale 0.88 → 1.152 m/s, pelvis
  rhythm 50.4 mm, separation 170..187; 0.94 → 1.241 m/s, 59.2 mm, 158..175. 0.94 kept (closer to Forward's
  rhythm and speed, still no crossing).

## GAMEPLAY VIDEO (`Artifacts/AnimationReview/`)

- `ForwardLeft01_RAW_native_1241.mp4` — real gameplay camera rig (eye 1.72, dist 1.55, side −0.52, pitch 15,
  FOV 80), Warrior Base review set, character displaced along −45° at the native 1.241 m/s, clip at 1.0×.
- `ForwardLeft01_RAW_gameplayLoadout_0926x.mp4` — displaced at the gameplay starting-loadout 1.149 m/s with the
  clip at 0.926×, i.e. what a GuardGait −45 : 1.241 entry would produce.
- Compare with `Fresh02_RAW_reference_0970x.mp4` / `v012_PRODUCTION_*.mp4`. Note the sword sits on the far
  side of the body from this camera in the Forward reference as well.

## PRODUCTION SAFETY

From disk: `Knight_Controller.controller` Light_Walk8 child 0 = `b9d47c80…` (v012), Travel_Walk8 child 0 =
`12f2b3e7…` (v008); the research clip's guid is referenced nowhere in the controller. md5: Fresh02 `a1630664`,
v012 `530b8133`, v008 `7a7ca074` unchanged. `Player.prefab` untouched. No ForwardLeft installed.

## WHAT TO LOOK AT

Weight acceptance on the outer (left) foot reaching out along the diagonal; whether the 32° pelvis lead with a
10° chest reads as a fighter tracking a threat while stepping off-line, or as a rotated forward walk; the
right (inner) foot's push-off; the head holding the threat.

`FRESH HUMAN FORWARDLEFT 01 READY FOR HUMAN REVIEW`
