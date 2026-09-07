# Fresh02 final runtime qualification — the whole Bravehood Player stack, real Play Mode

`__freshHumanForward_02` untouched (`a1630664`); Transport copy curves untouched; no v012; no
production edit. Every session: real Player prefab, real controller through an in-memory
`AnimatorOverrideController` (forward slot → Transport copy), research `GuardGait_Fresh02` rating,
real KCC, lock-on, Cinemachine camera, FootIK `AnimatedBones`, **all normal writers on** (AccelerationLean,
UpperBodyAim, TwistBoneDriver, HandGripPose, WeaponInertia, OffHandPose, IK passes, equipment).
Sessions chained by `RtqaQueue`; analysis `finaldelta.py` / `seqcheck.py` / `fpcompare.py`.

## COMPLETE RUNTIME STACK (verified order this milestone)

```
Update            RuntimeQaDriver(-1000) → CharacterAnimator (params, MotionSpeed = Speed/GuardGait) → KCC motor
Animator          evaluate (override controller) → OnAnimatorMove (yaw only) → IK pass: FootIK releases goals, OffHandPose W0
LateUpdate -100   [capture A: Animator output]
LateUpdate  -50   FootIK.AnimatedBones: bones → probe → pelvis settle → two-bone leg solve → foot world rot   [capture A2]
LateUpdate  -10   AccelerationLean
LateUpdate    0   UpperBodyAim · TwistBoneDriver · HandGripPose (· HitStop, Footsteps, LockOnReticle: no pose)   [capture B, +5]
LateUpdate   60   WeaponInertia (re-reads the authored socket pose whenever equipment re-seats it)   [capture C, +100]
LateUpdate  600   WeaponTrail · 1000 BodyPartHurtbox · render   [capture F, end of frame = C]
Equipment         EquipmentManager.Start (prefab defaultLoadout) → PlayerEquipment (inventory loadout) → palm auto-fit re-seats the sword
```

## FINAL POSE DELTA — Fresh02 (raw Transport copy) → Animator → final Player, 12 fixed phases

Unarmoured, target dead ahead (`fq_fwd`, 1.300 m/s, 0.970×, 145.5 spm). max / rms mm (max rot °):

| bone | RAW→A Animator | A→A2 FootIK | A2→B lean/aim/twist/grip | B→C WeaponInertia | **RAW→C total** |
|---|---|---|---|---|---|
| pelvis / Hips | 1.0 / 0.5 | 3.3 / 1.8 | 0 | 0 | **3.4 / 1.9** |
| chest | 1.0 / 0.5 | 3.3 / 1.8 | 0.3 (0.2°) | 0 | **3.5 / 2.0 (0.2°)** |
| head | 1.0 / 0.5 | 3.3 / 1.8 | 4.3 / 3.4 (0.1°) | 0 | **5.3 / 4.0 (0.1°)** |
| right clavicle | 1.1 / 0.6 | 3.3 / 1.8 | 3.3 / 2.7 (0.0°) | 0 | **4.7 / 3.3 (0.0°)** |
| right upper arm / forearm | 1.0 / 0.5 | 3.3 / 1.8 | 3.8 / 3.1 · 0.8 / 0.5 | 0 | 5.4 / 3.8 · 4.2 / 2.4 |
| right hand | 1.1 / 0.5 | 3.3 / 1.8 | 2.4 / 1.9 | 0.0 (0.6°) | **4.2 / 2.7 (0.6°)** |
| knees L / R | 2.7 / 2.6 | 62.3 / 14.2 (20.4° / 4.2°) | 0 | 0 | 61.9 / 14.3 |
| ankles L / R | 3.1 / 2.1 | 14.8 / 4.5 (10.6° / 2.1°) | 0 | 0 | **15.6 / 4.7** |
| toes L / R | 3.0 / 2.2 | 14.8 / 4.5 (0.0°) | 0 | 0 | 15.5 / 4.7 (0.0°) |
| sword position / blade | 8.6 (1.2°) | 3.3 (0.3°) | 3.1 (0.6°) | 4.5 (0.4°) | **9.2 / 6.3 mm, 1.7°** |

Pelvis rhythm 69.7 mm (Animator 69.0); toe-bone clearance min +19.7 / +28.0 mm; 1-frame steps
(median / max): feet 23.6 / 49.6, hips 5.2 / 25.5, hand 6.4 / 25.8, sword 6.7 / 24.6 mm — identical to
FootIK-off. Upper-body writers together move the shoulder chain ≤ 5.4 mm / ≤ 0.6°: **UpperBodyAim
preserves the clavicle and shoulder; WeaponInertia adds 0.4–0.6° of controlled lag.** The foot's world
rotation is the authored one (toe local rotation delta 0.00°): rollover intact.

Warrior Base via the debug swapper (`fq_wb`): identical picture (pelvis 3.8, ankles 15.3 / 4.5, sword
10.9 mm / 1.4°, rhythm 69.8, toes +20.1 / +28.0) at 1.2123 m/s / 0.905× / 137 spm — see LOAD below.

## LOAD — what "Warrior Base" weighs depends on who equips it

| loadout | items | weight | LoadSpeedMultiplier | combat speed | Fresh02 playback | cadence |
|---|---|---|---|---|---|---|
| debug swapper set 0 only | Warrior torso 5 + legs 4 | 9.0 | 0.9325 | **1.212 m/s** | 0.905× | 137 spm |
| **gameplay starting loadout** (`PlayerEquipment` → inventory) | Militia torso 9 + Warrior legs 4 + Militia helmet 2.5 | 15.5 | 0.8838 | **1.149 m/s** | 0.857× | 128.6 spm |
| unarmoured | – | – | 1.0 | 1.300 | 0.970× | 145.5 |

The "~1.149 m/s Warrior Base" figure of the previous milestones was measured while the inventory's
starting loadout (15.5) was equipped underneath the swapper's Warrior renderers. Stride matching is
exact in every case (MotionSpeed = speed / 1.34 to 4 decimals), so the transport is right for any load;
the design question of which load is canonical belongs to the inventory, not to the animation.

## FOOTIK

**Flat** (above): pelvis settle ≤ 4 mm, ankles ≤ 15 mm, no new jitter, penetration gone.
**Terrain** (`fq_ramp`, 4 m up / 4 m down at 6°, all writers on, gameplay camera): up — toes min
+30 mm, pelvis rhythm 80, settle ≤ 19 mm, foot step p99 43.6 (flat 43.9), knee step max 66, hips-Y step
max 23; down — toes min +3.7 mm (FootIK off: −17), settle ≤ 19 mm, knee delta 90 mm max; flat after —
back to +20 mm / settle ≤ 8. No knee pop, no pelvis collapse, no foot snap (steps equal the flat
walk). Video E.

## GRIP / SWORD ATTACHMENT — the 125 mm re-seat, resolved

Writer: **`EquipmentManager.UpdateWeaponGrip` → `ApplyGrip`** (palm auto-fit, `autoFitGrip = 1` on the
prefab). Mechanism, measured with reflection every session: the first equipped piece's skinned-palm
centroid defines `_gripFromPalm = authoredGrip − palm`; every later equip re-seats the sword to
`palm(new piece) + _gripFromPalm`. Warrior torso palm (hand-bone space) = (+3, 50, −9) mm; **Militia
torso palm = (−69, 60, −48) mm** → a 72 mm palm difference becomes the sword displacement (plus
rotation), i.e. the 125 mm / 4.3°. `WeaponInertia` then adopts the re-seated pose as "authored" and
carries it. No scene / PlayKit override, no item grip offsets, nothing in HandGripPose.

Why it looked constant before: the harness equips Warrior Base in `Awake`, then the game's
`PlayerEquipment` applies the **inventory starting loadout — Militia torso + Militia helmet + Warrior
legs** — in `Start`, and later a stray digit-key event (the debug swapper listens to `1`–`9`) put
Warrior Base back mid-run in some sessions. Bypass sessions saw the Militia re-seat; writers-on
sessions saw whichever equip was last.

Verdict:

- **Warrior Base (the review set): runtime grip = the prefab's authored grip within 8 mm** (mean
  sword-to-hand offset (−51, −61, +31) vs prefab (−51, −61, +38)). The hilt sits in the fist, pommel
  below the hand — the authored grip is authoritative and correct (`crop_fwd_wide150.png`, video A/B).
- **Gameplay starting loadout (Militia torso): GRIP ATTACHMENT DEFECT.** The auto-fit moves the hilt
  ~125 mm up the forearm; in the gameplay video the sword is visibly held at the elbow, not in the
  hand (`Fresh02_FINAL_gameplayStartingLoadout_MilitiaTorso_grip.mp4`, `crop_gameplay_wide150.png`).
  Cause: the Militia torso's hand-weighted vertices (gauntlet cuff geometry) bias the palm centroid;
  the auto-fit trusts that centroid. Smallest fix (not applied — no prefab/equipment edits this
  milestone): either `autoFitGrip = false` on the Player (the prefab grip is already right for the
  master rig's hand bone, which every set shares) or a per-item `gripPositionOffset` on
  `Equip_Militia_Torso`. It is equipment-domain, one field, independent of Fresh02 — but it is the
  grip the player sees at spawn today.

Tooling: `RuntimeQaDriver` now logs every equip change with the equipped list and the sword local
pose (`<tag>_equip.txt`, `<tag>_grip.txt`) and can run the pure gameplay path (`RTQA_noswap`), so a
RAW-vs-runtime sword delta is attributed to equipment before it is called an animation defect.

## UPPER BODY

Head 4.3 mm, clavicle 3.3 mm / 0.0°, upper arm 3.8 mm, hand 2.4 mm from UpperBodyAim + lean + twist
+ grip with the target dead ahead; WeaponInertia 0.4–0.6° lag at the hand, sword path preserved
(RAW→C 9 mm / 1.7°). Shoulder authority (+0.081…+0.117, clavicle 0.5°) reaches the screen.

## LOCK-ON — Fresh02 inside the old 8-way tree

| target | tree children | MotionSpeed / cadence | pelvis rhythm | legs vs Fresh02 | head / sword vs Fresh02 |
|---|---|---|---|---|---|
| dead ahead | Fresh02 1.00 | 0.857 (WB 15.5) / 128.6 | 69.8 | ≤ 15 mm | 4 / 11 mm |
| **1.2 m LEFT** at 12 m (velocity 5.7° right of facing) | Fresh02 0.934 + Strafe45Right 0.066 | 0.837 / 123.5 | 67.2 | ankles 44–53, knees 27–88 mm | head 9 mm 2.4°, hand 21 mm 5.9° |
| **1.2 m RIGHT** (velocity 5.7° left of facing) | **Fresh02 0.386 + Strafe45Left 0.614** | 0.825 / 122 | **36.8** | ankles 420–468 mm | head 79 mm 30°, sword 417 mm 82° |

The asymmetry is the serialized `Light_Walk8` geometry: the re-imported `Sword1h_Strafe45Left` child
sits at **9.3° left of forward** ((−0.162, 0.987)), the right-side child at 86° — so a target slightly
to the RIGHT hands 61 % of the walk to the old strafe, while a target to the LEFT costs 6.6 %. Watch
video C (left target: Fresh02 reads through) against C2 (right target: the old strafe takes over).
This is the next locomotion-family dependency; the tree is not redesigned here.

## IDLE TRANSITIONS and START / STOP / REPEAT (`fq_seq2`, unarmoured, lock dead ahead)

`idle:2 s → fwd:3 s → idle:2.5 s → fwd:2 s → slight-left:1 s → slight-right:1 s → fwd:2 s → back:2.5 s → fwd:2.5 s`

| segment | state / clips | 1-frame step max: hips · feet · knee · hand · sword (mm) | toes min | FootIK settle |
|---|---|---|---|---|
| idle | CombatIdle_v005 | 0.1 · 0.1 · 0.2 · 0.2 · 0.1 | +62 | 0 |
| **idle → fwd** | Fresh02 1.00 | 25 · 50 · 59 · 26 · 26 (= steady walk) | +17 | −6 |
| **fwd → idle** | CombatIdle | 17 · 48 · 33 · 18 · 16 | +22 | −5 |
| slight-left (velocity 14° left) | Strafe45Left 0.89 + StrafeLeft 0.11 | 22 · 83 · 51 · 52 · 69 | +7 | −15 |
| slight-right | Fresh02 0.84 + Strafe45Right 0.16 | 34 · 176 · 40 · 148 · 145 | −4 | −12 |
| fwd | Fresh02 | 25 · 52 · 59 · 26 · 25 | +11 | −11 |
| back | Strafe135Right 0.80 + WalkBwd 0.20 | 17 · 199 · 121 · 68 · 69 | −29 | −44 |
| **back → fwd** | Fresh02 | 26 · 79 · 56 · 72 · 144 | +3 | −22 |

No foot teleport, knee snap, sword pop, shoulder pop or vertical discontinuity at idle→forward or
forward→idle: every step at those two transitions is at or below the steady walk's own. The large
single-frame steps live in the old-family blends: slight-right (foot 176 / sword 145) and back→forward
(sword 144, frames 967–978, during the 4-frame `DirectionSmoothTime 0.06 s` crossfade
Strafe135Right → StrafeRight → Strafe45Right → Fresh02) and the mocap backpedal (feet 199, toes −29).
**Production v011 shows the same or larger values in the same places** (below) — tree behaviour, not
Fresh02.

## PRODUCTION FAMILY FOOTIK CHECK — current clips, scripted fwd/back/left/right/fwd-left, ON vs OFF

Light / locked (v011 forward + imported strafes), foot-step p99 / max mm, ON vs OFF:

| seg | foot ON | foot OFF | hips p99 ON/OFF | knee p99 ON/OFF | settle | toes ON/OFF | note |
|---|---|---|---|---|---|---|---|
| fwd v011 | 73.7 / 113 | 74.9 / 113 | 11.1 / 10.8 | 47 / 47 | −3 | +52 / +53 | identical |
| back | 138 / 196 | 117 / 205 | 12.6 / 11.6 | 93 / 86 | −44 | **−27 / +3** | pelvis settle to the hovering foot lowers the swing toe |
| left | 58 / 118 | 63 / 154 | 5.1 / 7.5 | 41 / 56 | −47 | +25 / +26 | smoother ON |
| right | 128 / 179 | 132 / 200 | 12.6 / 10.4 | 89 / 101 | −45 | +7 / −2 | smoother ON |
| fwd-left | 61 / 109 | 62 / 97 | 10.7 / 12.6 | 35 / 49 | −45 | +27 / +43 | hand/sword 343–395 mm/frame pops **exist OFF too** (UpperBodyAim/blend) |

Travel / unlocked (v008 + Loco mocap): every step metric equal ON vs OFF (fwd 61/98 vs 61/98, back
62/70 vs 62/70, right 73/155 vs 73/155, fwd-left 80/199 vs 75/203); settle −10…−31; toes within 1 mm of
OFF except fwd-left (−21 vs −6).

`AnimatedBones` introduces no knee popping, no bend-plane error (knee steps ≤ OFF), no floating, no
foot snapping (foot steps ≤ OFF), no rollover change (toe local 0°), no jitter. Its one visible
signature on the imported strafes is the **−44…−47 mm pelvis settle** (the retargeted mocap feet hover
above the floor; the bone solver reaches for the ground); the swing toe of the backpedal then dips 27
mm. Human call from the family videos: better / neutral / worse.

## PRODUCTION INSTALL REQUIREMENTS (for the v012 milestone — none applied here)

1. **Clip**: `Sword1H_WalkForward_v012.anim` = Fresh02 curves, `loopBlendPositionY = 1`, keepOriginal Y,
   XZ root motion as today (`HumanWalkPerformance.ApplyInPlaceClipSettings`).
2. **Controller**: `Light_Walk8` child 0 motion → v012 (timeScale 1, keep cycleOffset 0.92 or re-author
   the phase), serialized-file verification + `AnimationAssetSafety.VerifyProduction`.
3. **GuardGait**: serialize a production `GuardGaitReference` on `CharacterAnimator.GuardGait`
   (currently null → legacy 1.796 + warning). Asset to promote: `GuardGait_Fresh02.asset` — walk
   `[−141.6:1.84, −97.1:1.82, −52.8:1.82, 0:1.34, 86.3:1.81, 125.3:1.84, 170.2:1.80]`, jog = the
   existing table `[−141.5:3.61, −99.6:3.63, −55.6:3.62, −8.7:3.56, 37.9:3.44, 80.8:3.64, 124.5:3.60,
   171.5:3.62]`; forward native speed 1.34 = v012's.
4. **FootIK mode**: `Solver: 0` (AnimatedBones) must be **explicitly serialized** on the Player prefab
   (today it is the C# default; the prefab file has no `Solver:` line, and an already-imported prefab
   keeps whatever default was current at its last import — measured: two sessions ran the old mode
   until a reimport).
5. **Grip**: `autoFitGrip = 0` on the Player's `EquipmentManager` (or a Militia grip offset) — see above.
6. Retire the `Debug` digit-key swapper from the shipped Player, or the review set can be swapped by a
   stray key press mid-play.

## VIDEOS (real Player, real camera, all writers on)

- A `Fresh02_FINAL_A_pureForward_unarmoured.mp4` — 1.30 m/s, 0.970×, Warrior Base renderers.
- B `Fresh02_FINAL_B_WarriorBase.mp4` — swapper set 0 (weight 9): 1.212 m/s, 0.905×.
- C `Fresh02_FINAL_C_lockTarget_left5.7deg.mp4` — 6.6 % Strafe45Right; C2
  `Fresh02_FINAL_C2_lockTarget_right5.7deg.mp4` — 61 % Strafe45Left.
- D `Fresh02_FINAL_idle_forward_idle_sequence.mp4` — the scripted sequence above.
- E `Fresh02_FINAL_E_ramp6deg_allWritersOn.mp4`.
- `Fresh02_FINAL_gameplayStartingLoadout_MilitiaTorso_grip.mp4` — the grip defect as the player sees it.
- `ProductionFamily_LIGHT_locked_FootIKBones.mp4`, `ProductionFamily_TRAVEL_unlocked_FootIKBones.mp4`.

## HUMAN MOTION QUALITY REVIEW — for the reviewer, not answered by numbers

Fresh02 weight visible? body restrained? FootIK invisible? grip intentional (Warrior Base yes; gameplay
loadout no)? sword inertia controlled? shoulder preserved? lock-on blend acceptable (left-side target
yes; right-side target replaces the walk)? idle transitions acceptable? production-ready in gameplay?

## TESTS

34 passed / 0 failed / 0 skipped (unchanged suite; no new defect in animation code to guard).

## PRODUCTION STATE

Controller `186fb872` = backup, Player.prefab `a03bf275` = backup, Fresh02 `a1630664`, Transport
`43233d7c`; `VerifyProduction` CLEAN; prefab `GuardGait` null, FootIK mode C# default, `autoFitGrip`
1; harness and queue disarmed; no v012.
