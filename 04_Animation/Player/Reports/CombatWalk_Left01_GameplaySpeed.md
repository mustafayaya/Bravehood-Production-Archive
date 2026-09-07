# TRUE LEFT (−90°) — `__freshHumanLeft_01` motion + gameplay speed study

Principle applied: **believable leg motion > fast animation playback**. The clip was NOT rebuilt. Each candidate is a
*stride-matched presentation*: playback rate and character translation change together, so the feet never skate
(displacement = native planted speed × playback, verified to 1 mm/s on every candidate).

## How the study ran (real Player, gameplay camera, lock-on, Warrior Base loadout)

- Clip: `Generated/__freshHumanLeft_01.anim` (md5 88cba72b, unchanged). Research override into the ring slot
  `Sword1h_Strafe135LeftLoop` (−97.1°) in memory only; tree input driven to exactly −97.1° so the clip weight is 1.000.
- Native speed rating: research asset `Generated/GuardGait_Left01Study.asset` = production table with the −97.1° entry
  set to Left01's measured native 0.70 m/s. With that, `CharacterAnimator.MotionSpeed = movement / 0.70` stride-matches
  automatically — no clip-specific Animator speed hack.
- Movement speed per candidate: `StatusSpeedMultiplier` (existing gameplay multiplier) via the harness pref
  `RTQA_speedMul`; new `RTQA_laneZ` lane start so the pure-left path stays on the floor.
- Script `idle:120; ang-102.8:360; idle:120`, forensic transport + pose CSVs, 600 frames at 60 fps.

## Candidates

| candidate | playback (MotionSpeed) | movement speed | cadence | cycle | 0.25 s | 0.5 s | 1.0 s | stride check (0.70 × rate) |
|---|---|---|---|---|---|---|---|---|
| slow `left_r075` | 0.722× | 0.506 m/s | 144 spm | 0.831 s | 0.13 m | 0.25 m | 0.51 m | 0.505 ✓ |
| **medium `left_r090`** | **0.874×** | **0.612 m/s** | **175 spm** | **0.686 s** | **0.15 m** | **0.31 m** | **0.61 m** | **0.612 ✓** |
| brisk `left_r105` | 1.014× | 0.710 m/s | 203 spm | 0.592 s | 0.18 m | 0.35 m | 0.71 m | 0.709 ✓ |
| previous fast (comparison) `left_fast` | 1.267× | 0.887 m/s | 253 spm | 0.474 s | 0.22 m | 0.44 m | 0.89 m | 0.887 ✓ |

The fast row is what the production controller would do today at −90° with the loadout (1.149 m/s × 0.8 strafe tax).

Foot-contact forensic (stage C, moving frames): every geometric metric is identical across the four rates (knee min
L 1.9–4.2° / R 1.9°, reach max 1.000, toe height min L 51 / R 33 mm, pelvis range 61 mm, max fore/aft separation
790 mm). Only the timing changes — which is the point: speed is a presentation choice, not a gait change.

## Human motion read (my read from the gameplay frames; the human reviewer decides)

- **slow 0.72× / 0.51 m/s, 144 spm** — reads as a deliberate guard sidestep. Weight arrives on the lead foot and the
  step-close is visible as two events. Most human of the four; slow for a lock-on circle.
- **medium 0.87× / 0.61 m/s, 175 spm — SELECTED.** Still reads as steps (land, load, close), the closing foot no longer
  ticks, swing time 0.24 s is in the human lateral range. Gameplay keeps 61 cm/s of lateral travel.
- **brisk 1.01× / 0.71 m/s, 203 spm** — the authored native cadence; the closing foot starts to tick and the pelvis
  sway looks metronomic.
- **fast 1.27× / 0.89 m/s, 253 spm** — a scurry. This is the presentation that made Left01 look rushed in the
  first review; the gait itself is not the cause.

Verdict: the slower presentation fixes the read, so the gait is **KEPT** — no Left02.

Rate-independent observations for the reviewer (not fixed here, present at every speed):
- right knee touches 1.9° for ~14 % of moving time (right leg 17 mm shorter, reach ratio 1.000 at lead landing);
- mid-stance ankle drift 15–23 mm and support yaw drift 8–14° on both feet (step-close boot settle).
If the human review names either as a defect, that is the trigger for a Left02, not the speed.

## Family context at selected gameplay speeds

`Artifacts/AnimationReview/Family_Forward_ForwardLeft_Left_gameplaySpeeds.mp4` — left to right:
Forward v012 at its production gameplay presentation (1.149 m/s, 0.857×), ForwardLeft v001 at production
(−45°, ~1.05 m/s, ~0.84×), Left01 at the selected 0.87× / 0.61 m/s. All three on the real Player with lock-on.

Other videos: `LeftSpeed_left_r075.mp4`, `LeftSpeed_left_r090.mp4`, `LeftSpeed_left_r105.mp4`,
`LeftSpeed_left_fast.mp4`, `LeftSpeed_2x2_slow_medium_brisk_fast.mp4` (top-left slow, top-right medium,
bottom-left brisk, bottom-right fast).

## Technical QA

- Stride match: measured displacement equals 0.70 × MotionSpeed within 0.001 m/s on all four candidates; clip weight
  1.000 throughout the move segment; no transition contamination after the 9 entry frames.
- Clip untouched (md5 88cba72b). Forward v012 (530b8133) and ForwardLeft v001 (1fb5e646) untouched.
- Regression suite 44/44 (job 30781197), Player prefab restored to HEAD (md5 67ef0650).

## Production safety

- `AnimationAssetSafety.VerifyProduction` = TRUE: no research clip referenced by the production controller (70 clips,
  0 research-prefixed). Production GuardGait unchanged (md5 3bc3ebda; −97.1° still 1.82 mocap).
- Nothing installed. `GuardGait_Left01Study.asset` is a research asset under `Generated/`, referenced by nothing.

## Proposed install (not done — awaiting human approval of the motion and the speed)

Existing architecture only:
1. GuardGait gets a true −90° entry at Left01's native 0.70 m/s (MotionSpeed then stride-matches by construction).
2. Gameplay lateral speed at −90° set to 0.61 m/s through the directional movement multiplier
   (today the strafe tax gives 0.80 → 0.92 m/s with the loadout; the selected presentation needs ≈0.53 at −90°,
   i.e. a per-heading speed scale on the gait reference rather than the fixed 0.8 + 0.2·cosθ formula).
3. Left01 promoted to `Sword1H_WalkLeft_v001` in a true −90° Light_Walk8 child; the −97.1° mocap ring child parked.
GuardGait after install describes the final family: Forward 1.34 native (played at 0.857×), ForwardLeft 1.24
native (≈0.84×), Left 0.70 native (0.874×).

Permanent rule recorded: never judge locomotion at native rate only; the animation dictates its believable cadence
and the controller supports it (GuardGait native speed + MotionSpeed + directional movement multiplier).

## INSTALLED (user approved, 2026-09-03)

- `Generated/Sword1H_WalkLeft_v001.anim` (guid 0b13a919…) = byte-identical promotion of `__freshHumanLeft_01`.
- `Knight_Controller` ▸ Light_Walk8 child 5: mocap `Sword1h_Strafe135LeftLoop` (−97.1°, co 0.52) replaced by
  `Sword1H_WalkLeft_v001` at a true (−1, 0), timeScale 1, cycleOffset 0.35 (left-foot landing aligned to
  ForwardLeft v001's for the −45…−90 blend region). Child 7 stays parked.
- `GuardGait_Knight1H`: −97.1 : 1.82 → **−90 : 0.70** (Left native). Family now Forward 1.34 / ForwardLeft 1.24 / Left 0.70.
- `Player.prefab` StrafeSpeedMultiplier 0.8 → **0.53** (the existing directional movement multiplier; YAML patch, no bake).

Play verification on the production configuration (no override, no research gait, no speed pref, lock-on, loadout):

| heading | clip weight | displacement | MotionSpeed | cadence | native × MS |
|---|---|---|---|---|---|
| −90° | WalkLeft_v001 1.000 | 0.606 m/s | 0.866× | 173 spm | 0.606 ✓ |
| −45° | WalkForwardLeft_v001 1.000 | 0.987 m/s | 0.796× | 119 spm | 0.987 ✓ |

Side effect to be aware of: the strafe multiplier also shapes the diagonals (0.8 + 0.2·cosθ → 0.53 + 0.47·cosθ), so
ForwardLeft now travels 0.99 m/s at 0.80× instead of ~1.05 m/s at ~0.84×, and the back-diagonals drop from
0.66 to 0.58 of full speed. Forward (cos 0 = 1) is unchanged at 1.149 m/s / 0.857×. If the diagonal should keep its
old presentation, a per-heading speed scale on GuardGait is the clean follow-up.

Video: `Artifacts/AnimationReview/Left_v001_PRODUCTION_lockon_left.mp4`.
