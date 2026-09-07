# Sword1H_WalkForward_v012 — production install of the approved Fresh02 Forward walk

No artistic change. `__freshHumanForward_02` (`a1630664`) untouched; v012 is a new asset. Every runtime
number below is the real Player in Play Mode with the PRODUCTION assets (no override controller, no research
gait pref, prefab FootIK / GuardGait / grip as serialized), all normal writers on, the pure gameplay equip path.

## v012 ASSET

`Assets/Bravehood/Animation/Generated/Sword1H_WalkForward_v012.anim`, guid `b9d47c80ecac1432f9ae31a8ae48591f`,
md5 `530b8133`. Built as `Object.Instantiate(Fresh02)` + clip settings only.

Source equivalence (Fresh02 → v012): 102 / 102 curve bindings, 1661 / 1661 keys, **maximum curve value
deviation 0** (time, value, tangents key-identical). Edit-Mode sampled world pose at 480 phases (Fresh02's root
motion applied vs v012's baked pose): pelvis 0.49 mm, feet 0.98, knees ≤ 0.98, chest 1.09, head 0.98,
clavicle 0.98, hand 1.09, sword 0.98 mm — **0.00° on every bone and the blade**. Motion-equivalent.

## CLIP SETTINGS (serialized)

`m_LoopTime 1 · m_LoopBlend 0 · m_CycleOffset 0 · m_LoopBlendOrientation 1 · m_LoopBlendPositionY 1 ·
m_LoopBlendPositionXZ 1 · m_KeepOriginalOrientation 1 · m_KeepOriginalPositionY 1 · m_KeepOriginalPositionXZ 1 ·
m_HeightFromFeet 0`, stop 0.8 s — the qualified `HumanoidWalkGenerator` in-place architecture (identical to
v008/v011). Every root channel is in the pose; the Animator hands `OnAnimatorMove` nothing to drop.

One consequence to look at in the videos: the Y-only Transport research copy dropped Fresh02's authored pelvis
**lateral sway (RootT.x ±29 mm) and yaw/roll (RootQ ±5°)** as root motion; production v012 carries them in the
pose, exactly as the approved Edit-Mode RAW reference did (v012 vs Fresh02 world pose ≤ 1.1 mm). Interpolated
Humanoid legality unchanged (same curves).

## GUARDGAIT

`Assets/Core/Animations/GuardGait_Knight1H.asset` (guid `60ab6bda…`), serialized on
`Player.prefab › CharacterAnimator.GuardGait`. Walk: −141.6 : 1.84 · −97.1 : 1.82 · −52.8 : 1.82 · **0 : 1.34** ·
86.3 : 1.81 · 125.3 : 1.84 · 170.2 : 1.80. Jog: the existing table (−141.5 : 3.61 · −99.6 : 3.63 · −55.6 : 3.62
· −8.7 : 3.56 · 37.9 : 3.44 · 80.8 : 3.64 · 124.5 : 3.60 · 171.5 : 3.62). Runtime: no "No GuardGaitReference"
warning during any production session (last such line in the log predates the install); MotionSpeed =
speed / 1.34 to four decimals.

## FOOTIK

`Player.prefab` now carries `Solver: 0` (AnimatedBones) explicitly; after reimport the serialized property reads
0 and the runtime component reports `AnimatedBones` in every session. Flat ground (v012, gameplay loadout):
FootIK moves the pelvis ≤ 3.7 mm, ankles 14.7 / 4.8 mm, knees 60.9 / 14.2 mm (the straight-leg response to a
15 mm ankle lift), toes 14.7 / 4.8; foot frame steps median 20 / max 43 mm (= FootIK off); toe clearance
min +20.6 / +28.4 mm. No parked-goal behaviour, no hoist, no jitter.

## CONTROLLER (from disk)

`Light_Walk8` child 0 → `Sword1H_WalkForward_v012` (guid `b9d47c80…`, count 1), position (0, 1), timeScale
1.00, cycleOffset 0.92. v011 guid `f11e74bd…` no longer referenced. `Travel_Walk8` child 0 → v008 (`12f2b3e7…`)
@ ts 0.50, untouched. Child 7 (Strafe45Left) at (0, 0) as qualified. No `__*` research guid anywhere in the
controller (all 10 research metas checked). `AnimationAssetSafety.VerifyProduction` CLEAN.

## CYCLE OFFSET — kept at 0.92

Measured, not assumed. At child phase 0.92 the entry pose is left foot forward / right foot back for both
clips (v011 L z +82 / R −246 mm; v012 L +221 / R −126), which is the CombatIdle_v005 stance (L +150 / R −326):
the idle → walk crossfade starts on the matching support side. The tree's clock is not contact-synchronised
anyway: left-foot-lowest lands at tree time 0.83 (v011), 0.27 (Strafe45Right), 0.15 (StrafeLeft), 0.06
(WalkBwd) — the mocap children never shared a phase with the forward clip, so re-authoring v012's offset
against them would be an 8-way-family job, not a v012 one. Runtime idle ↔ forward transitions at 0.92: below.

## GRIP (gameplay starting loadout: Militia torso 9 + Militia helmet 2.5 + Warrior legs 4)

`autoFitGrip 0`, Militia offsets 0, swapper disabled: sword hand-local (−37, 65, −40) mm vs authored
(−43, 63, −45) — 8 mm, i.e. the WeaponInertia lag; hilt in the fist, pommel below the hand
(`v012_PRODUCTION_gameplay.mp4`). No re-seat, no hotkey interference (`EquipmentDebugSwapper` polling is
compiled out of non-development builds and disabled by the harness).

## RUNTIME (real Player, production assets)

| | unarmoured (load 1.0) | gameplay starting loadout (15.5, LoadSpeedMultiplier 0.884) |
|---|---|---|
| movement (requested / Motor / displacement) | 1.3000 / 1.3000 / 1.3000 m/s | 1.1489 / 1.1489 / 1.1489 |
| normalizedTime rate (fit rms) | 1.21267 /s (0.00000) | 1.07172 /s (0.00001) |
| v012 playback | **0.9701×** | **0.8574×** (= 1.1489 / 1.34) |
| cadence | **145.5 spm** | 128.6 spm |
| pelvis rhythm (final) | **69.8 mm** | 69.8 mm |
| final pose vs v012 (12 phases, max / rms mm) | pelvis 4.2 / 1.9 · chest 4.2 · head 4.3 · clavicle 4.2 (0.0°) · upper arm 4.3 · forearm 4.3 · hand 4.3 (0.6°) · ankles 15.1 / 4.7 · toes 15.0 / 5.6 · **sword 10.9 / 8.2 mm, 1.7°** | pelvis 3.8 · clavicle 3.9 (0.0°) · hand 3.8 (0.6°) · ankles 14.7 / 4.4 · sword 10.7 mm, 1.6° |
| frame steps median / max (feet · hips · hand · sword) | 23 / 49 · 5 / 26 · 6 / 26 · 7 / 25 mm | 20 / 43 · 5 / 23 · 5 / 23 · 5 / 22 |

Shoulder: clavicle 0.0° deviation from the source at the final stage (UpperBodyAim adds ≤ 0.9 mm with the
target ahead). Sword: 11 mm / 1.7° from the source path incl. 0.6° of WeaponInertia lag — controlled.

## LOCK-ON (gameplay loadout, production tree)

| target | v012 weight | neighbour | MotionSpeed | pelvis rhythm | ankles L/R vs v012 | sword vs v012 |
|---|---|---|---|---|---|---|
| 10° right | 0.804 | StrafeLeft 0.188 | 0.800 | 54 mm | 134 / 145 | 122 mm / 23° |
| 5.7° right | **0.890** | StrafeLeft 0.108 | 0.825 | 61 mm | 80 / 82 | 67 mm / 13° |
| 0° | 1.000 | – | 0.857 | 69.6 mm | 15 / 8 | 12 mm / 1.7° |
| 5.7° left | **0.931** | Strafe45Right 0.066 | 0.837 | 67 mm | 45 / 54 | 31 mm / 10° |
| 10° left | 0.877 | Strafe45Right 0.115 | 0.821 | 64 mm | 78 / 94 | 59 mm / 18° |

v012 dominant on both sides of the forward sector; the temporary tree repair holds in production.

## TRANSITIONS (`p_seq`: idle 2 s → forward 3 s → idle 2.5 s → forward 2.5 s → idle 2 s, unarmoured)

Single-frame steps (max mm) per segment, each segment including its entry transition — the steady
Fresh02 walk itself steps feet 49 / hips 25 / hand 26 / sword 25 mm per frame:

| segment | state / clips | hips · feet · knee · hand · clavicle · sword | toes min | FootIK settle |
|---|---|---|---|---|
| idle | CombatIdle_v005 | 0.1 · 0.1 · 0.2 · 0.2 · 0.3 · 0.2 | +62 | 0 |
| **idle → forward** | v012 1.000, 1.300 m/s, 0.970× | 25 · 49 · 59 · 26 · 26 · 25 (= steady) | +17 | −6 |
| **forward → idle** | CombatIdle | 17 · 47 · 32 · 18 · 18 · 16 | +22 | −5 |
| idle → forward (2nd) | v012 | 25 · 49 · 58 · 26 · 26 · 25 | +17 | −6 |
| forward → idle (2nd) | CombatIdle | 16 · 42 · 38 · 17 · 16 · 16 | +30 | −6 |

No foot teleport, knee snap, sword pop, shoulder pop or vertical discontinuity at either transition, in
either direction, twice.

## TERRAIN (`p_ramp`, 6° up / down, gameplay loadout)

| segment | toes min | pelvis rhythm | FootIK settle | foot step p99 / max | knee step max |
|---|---|---|---|---|---|
| up 6° | +32 mm | 80 mm | −17.5 mm | 42.8 / 43.8 | 77 |
| down 6° | +2.4 mm | 63 mm | −18.8 mm | 44.2 / 46.0 | 69 |
| flat after | +21 mm | 71 mm | −8.3 mm | 42.5 / 42.9 | 66 |

Foot steps on the slopes equal the flat walk's; settle ≤ 19 mm; no knee pop, no pelvis collapse, no foot snap.

## VIDEOS (`Artifacts/AnimationReview/`)

A `v012_PRODUCTION_gameplay.mp4` · B `v012_PRODUCTION_unarm.mp4` · C `v012_PRODUCTION_L57.mp4` ·
D `v012_PRODUCTION_R57.mp4` · E `v012_PRODUCTION_idle_forward_idle.mp4` · F `v012_PRODUCTION_ramp6deg.mp4`.
Reference for the artistic comparison: `Fresh02_RAW_reference_0970x.mp4` (Edit-Mode, 0.970×).

## TESTS

37 passed / 0 failed / 0 skipped: the 36 unchanged (O/P/Q/R/S/T intact) + `U1_ProductionForward_v012_Contract`
(Light_Walk8 forward = v012 at (0,1) ts 1 with Y/XZ/orientation baked and 0.8 s length; `GuardGait` serialized with
0° = 1.34 and 86.3° = 1.81; FootIK `Solver` = AnimatedBones AND literally present in the prefab YAML;
`autoFitGrip: 0`).

## ASSET INTEGRITY

v008 `7a7ca074` · v009 `5546958e` · v010 `7c44dd1d` · v011 `e3705c2f` · Fresh01 `6f3a9945` · Fresh02
`a1630664` · Transport copy `43233d7c` — all unchanged; v012 `530b8133` new. `git status` on the Generated
folder shows only the new v012 files.

## PRODUCTION STATE

```
Light_Walk8  Forward = Sword1H_WalkForward_v012 @ ts 1.00, cycleOffset 0.92
Travel_Walk8 Forward = Sword1H_WalkForward_v008 @ ts 0.50
CombatWalkSpeedScale = 0.65
CharacterAnimator.GuardGait = GuardGait_Knight1H (0° = 1.34)
FootIK Solver = AnimatedBones (serialized `Solver: 0`)
EquipmentManager.autoFitGrip = 0 · Militia grip offsets = 0
no __* research clip referenced · harness disarmed
```
