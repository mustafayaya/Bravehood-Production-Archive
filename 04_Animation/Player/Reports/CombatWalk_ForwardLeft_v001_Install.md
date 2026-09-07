# Sword1H_WalkForwardLeft_v001 — production install of the approved ForwardLeft02 diagonal walk

No artistic change. `__freshHumanForwardLeft_02` (`b4c2f052`) untouched; v001 is a new asset. Every runtime number
below is the real Player in Play Mode with the PRODUCTION assets (serialized controller, GuardGait, FootIK, grip),
all normal writers on, the pure gameplay equip path, no override controller, lock-on held.

## BLIND KEY CONFIRMATION

`Artifacts/AnimationReview/ForwardLeft_01_vs_02_sideBySide_BLIND_KEY.txt`: `LEFT = ForwardLeft01, RIGHT =
ForwardLeft02`. Human review selected RIGHT → ForwardLeft02 is the approved motion. Proceeded.

## FORWARDLEFT v001 ASSET

`Assets/Bravehood/Animation/Generated/Sword1H_WalkForwardLeft_v001.anim`, guid `910fd344eefa54d588ac07de7bdda9e9`,
md5 `1fb5e646`. Built as `Object.Instantiate(ForwardLeft02)` + `ApplyInPlaceClipSettings` only.

Source equivalence (ForwardLeft02 → v001): **102 / 102 bindings, 1661 / 1661 keys, max |Δvalue| 0, max
|Δtangent| 0, max |Δtime| 0**. Sampled world pose at 96 phases: pelvis (hips) 0.00 mm / 0.00°, pelvis
(upper-leg mid) 0.00 mm, chest 0.00 / 0.00°, head 0.00 / 0.00°, clavicle 0.00 / 0.00°, knees 0.00 / 0.00°,
ankles 0.00 / 0.00°, toes 0.00 / 0.00°, sword hand 0.00 / 0.00°, sword / blade 0.00 mm / 0.00°. Motion-identical.

Clip settings (serialized): loopTime 1 · loopBlend 0 · cycleOffset 0 · orientation baked / Based Upon Original ·
Position Y baked / Original · Position XZ baked / Original · heightFromFeet 0 · 0.8 s — the v012 in-place contract.

## CONTROLLER INSTALL

Serialized `Light_Walk8` before: child 6 = `Sword1h_StrafeLeftLoop` (fileID 7400026 in `Sword1h_Walks.fbx`,
guid `971c05bc…`) at (−0.7965, 0.6046) = **−52.8°**, the ring child nearest the true −45° direction. Child 7
(`Sword1h_Strafe45LeftLoop`) sits parked at (0, 0) — NOT touched, it was not chosen by name.

After (from disk): child 6 = `Sword1H_WalkForwardLeft_v001` (guid `910fd344…`, fileID 7400000) at
**(−0.7071068, 0.7071068) = −45.0°**, timeScale 1.00, cycleOffset 0.92. Child 0 = v012 at (0, 1) unchanged.
Children 1–5 (mocap ring) and 7 (parked origin) unchanged. `Travel_Walk8` byte-identical (v008 @ 0.5 forward).

## GUARDGAIT

`GuardGait_Knight1H.asset`: the −52.8 : 1.82 StrafeLeft entry REPLACED by **−45 : 1.24**. Walk table now
−141.6 : 1.84 · −97.1 : 1.82 · **−45 : 1.24** · 0 : 1.34 · 86.3 : 1.81 · 125.3 : 1.84 · 170.2 : 1.80 (jog untouched).
Native speed measured on the finished production clip's planted-foot track along −45°: 1.246 (phase window),
1.236 (mid-stance), 1.262 (floor+4 mm) m/s → 1.24. Runtime confirms: MotionSpeed = Speed / 1.24 to four decimals.

## RUNTIME MOVEMENT (real Player, −45° tree input, lock-on held)

The gameplay controller applies a forwardness strafe tax at 45° (0.8 + 0.2·cos 45° = 0.941), so the diagonal does
NOT move at the forward speed:

| | unarmoured (LoadSpeedMultiplier 1.0) | gameplay starting loadout (15.5, 0.884) |
|---|---|---|
| requested movement vector (H, F) | (−0.707, 0.707) = −45.0° | (−0.707, 0.707) = −45.0° |
| Motor.Velocity / world displacement | 1.224 / **1.224 m/s** | 1.082 / **1.082 m/s** |
| actual heading (world → rel. facing) | −50.7° → **−45.0°** | −50.7° → −45.0° |
| MotionSpeed | 0.9869 | 0.8722 |
| normalizedTime rate (fit rms) | 1.2337 /s (0.0000) | 1.0903 /s (0.0000) |
| v001 playback | **0.987×** | **0.872×** |
| cadence | 148.0 spm | 130.8 spm |
| v001 weight | 1.000 | 1.000 |

Stride contract: native 1.246 m/s × 0.987 = 1.230 vs 1.224 displaced (0.5 %); × 0.872 = 1.087 vs 1.082. Planted-foot
world slip (runtime probe, incl. heel-off/strike frames) L 181 / R 133 mm/s mean — the same probe reads 115–212 on
v012 in the same sessions.

## TRUE HEADING CHECK

Lock-on target sits 5.7° left of the lane (harness geometry, as in every v012 session), so Player root yaw =
−5.7° world, held to 0.0° range throughout; movement −50.7° world = −45.0° relative to the root; animation
ground track in the root frame L −45.8° / R −43.7°. The character never rotates toward travel; the pelvis leads as
authored (hip line 21.4..42.4°, mean 31.8°).

## FORWARD ↔ FORWARDLEFT BLEND (gameplay loadout, measured tree inputs)

| tree input | v012 | ForwardLeft_v001 | parked Strafe45Left | MotionSpeed | displacement |
|---|---|---|---|---|---|
| 0° | **1.000** | – | – | 0.857 | 1.149 m/s |
| −5° | **0.887** | 0.111 | 0.002 | 0.864 | 1.148 |
| −10° | **0.772** | 0.220 | 0.007 | 0.869 | 1.145 |
| −20° | **0.540** | 0.431 | 0.029 | 0.876 | 1.135 |
| −30° | 0.328 | **0.655** | 0.017 | 0.878 | 1.118 |
| −45° | 0.001 | **1.000** | – | 0.872 | 1.082 |

Coherent v012 → v001 progression; no old mocap clip enters the forward-left sector (Strafe135Left 0 everywhere,
parked child ≤ 0.03). v012 dominant to −20°, v001 dominant from −30°, exact at −45°.

## CYCLE OFFSET — kept at 0.92

Same phase architecture as Forward: at child phase 0.92 v001 is left foot forward / right back along travel
(L +256 / R −163 mm; v012 L +227 / R −130), the CombatIdle_v005 entry stance. Left-foot-lowest at 0.529 (v012
0.59), right at 0.971 (v012 0.01) — within 0.06 cycle. Measured transitions (below) show no defect, so it stays.

## FOOTIK (Solver AnimatedBones, serialized `Solver: 0`)

Flat ground, −45°: FootIK pelvis −2.2 mm mean / 7.4 mm range (v012 flat: ≤ 3.7), toe clearance min L 20.9 / R
25.8 mm, foot frame steps median 22 / max 46–49 mm (= v012 49), no stance-foot hoist, no parked-goal behaviour,
no jitter, rollover intact. 6° terrain (gameplay loadout, `E`, diagonal across a 32 m-wide 4 m up / 4 m down ramp from x = 40):

| segment | v001 weight | toes min | pelvis rhythm | FootIK settle mean / range | foot step p99 / max | knee step max |
|---|---|---|---|---|---|---|
| flat before | 1.000 | +19.5 mm | 66 mm | −2.4 / 7.5 mm | 40.8 / 45.0 | 81 |
| up 6° (4.7 s) | 1.000 | +32.4 mm | 66 mm | −5.2 / 12.2 mm | 42.3 / 43.6 | 85 |
| down 6° (1.2 s clear) | 0.99 | +12.6 mm | 62 mm | −9.2 / 18.7 mm | 42.2 / 42.2 | 75 |

(v012 on the same ramp: up +32 / −17.5 / 42.8–43.8, down +2.4 / −18.8 / 44.2–46.0.) Foot steps on the slopes equal
the flat walk's; no knee pop, pelvis collapse or foot snap. Between the crest and the down-slope the diagonal path
ran under a scene `Shelf` collider (x 28.9–31.1, y 0.97–1.03, z −3.2..−2.8) and was held for 2.3 s sliding left —
the tree correctly blended Strafe135Left for the sideways velocity; scene geometry, not animation, and the video
shows it.

## TRANSITIONS (unarmoured, per segment incl. its entry transition; max mm per frame)

`C`: forward 3 s → −14° 2.5 s → forward 2.5 s → −45° 3.5 s → forward 2.5 s

| segment | clips | feet · hips · knees · hand · clavicle · sword | toes min | FootIK pelvis range |
|---|---|---|---|---|
| forward | v012 1.000 | 49 · 25 · 59/79 · 25 · 25 · 25 | +16.8 | 5.7 |
| **→ −14°** | v012 0.680 / v001 0.306 | 49 · 25 · 60/88 · 25 · 25 · 24 | +19.1 | 4.6 |
| → forward | v012 1.000 | 51 · 25 · 59/74 · 25 · 25 · 24 | +19.7 | 4.5 |
| **→ −45°** | v001 0.999 | 49 · 25 · 62/92 · 25 · 25 · 25 | +20.7 | 7.9 |
| → forward | v012 0.999 | 50 · 25 · 58/74 · 26 · 25 · 24 | +19.7 | 3.9 |

`D`: idle 2 s → −45° 5 s → idle 3 s: idle → v001 entry 46 · 24 · 62/87 · 25 · 25 · 25 (= steady); v001 → idle
56 · 6 · 23/36 · 16 · 6 · 13 (v012 → idle was 47 · 17 · 32 · 18 · 18 · 16). No foot snap, phase mismatch, pelvis,
knee, shoulder or sword pop, no torso twist. The right-knee 79–92 mm/frame maximum is the clip's own swing at
phase 0.25 (stage A, before FootIK, 82 mm; v012 shows 74–79 in the same probe), not an integration artefact.

## ARTISTIC PRESERVATION (final runtime vs approved ForwardLeft02, Edit-Mode)

| | ForwardLeft02 (approved) | runtime v001 (C_final) |
|---|---|---|
| hip-line yaw range · mean | 21.4..42.4 · 31.9° | 21.4..42.4 · **31.8°** |
| shoulder-line yaw range · mean | 7.0..13.1 · 10.1° | 7.0..13.1 · **10.1°** |
| pelvis rhythm | 59.2 mm | 61.5 mm (FootIK settle) |
| ground-track heading | −46.1 / −43.9° | −45.8 / −43.7° |
| swing peak / toes lowest | 134 mm / 1.9 mm | toe clearance ≥ 20.9 mm with FootIK |

The runtime stack neither squares the pelvis nor rotates the character toward travel: the pelvis lead, chest and
threat-facing head survive to the final pose to 0.1°.

## VIDEOS (`Artifacts/AnimationReview/`, real Player, real camera, all writers on)

A `ForwardLeft_v001_PRODUCTION_A_unarm_minus45.mp4` · B `…_B_gameplayLoadout_minus45.mp4` ·
C `…_C_fwd_slightFL_fwd_FL_fwd.mp4` · D `…_D_idle_FL_idle.mp4` · E `…_E_ramp6deg.mp4` ·
F `…_F_v012Forward_vs_ForwardLeft_sameTree.mp4` (left: v012 forward, right: v001 diagonal, both the production tree).
Note: the sword sits on the far side of the body from the gameplay camera on the diagonal, as it does in
`v012_PRODUCTION_gameplay.mp4`.

## TESTS

**41 passed / 0 failed / 0 skipped** (EditMode, run after every edit in this milestone): the 40 unchanged (A–H, J–N, O1–O4,
P1–P2, Q1–Q2, R1–R2, S1–S2, T1–T2, U1, V1–V3) + U2.

`U2_ProductionForwardLeft_v001_Contract` (new): Light_Walk8 contains exactly one `Sword1H_WalkForwardLeft_v001`;
its position is (−0.7071068, 0.7071068) within 1e-4; timeScale 1; cycleOffset equals child 0's; Forward stays
v012; the slot references the production asset path, not a `__` research clip; loop / orientation / Y / XZ baked,
Based Upon Original, heightFromFeet off, 0.8 s; `GuardGait.SampleWalk(−45) = 1.24`, `SampleWalk(0) = 1.34`, and no
−52.8° entry remains beside it.

## ASSET INTEGRITY

Before = after: Fresh02 `a1630664` · ForwardLeft01 `612bccab` · ForwardLeft02 `b4c2f052` · v008 `7a7ca074` ·
v009 `5546958e` · v010 `7c44dd1d` · v011 `e3705c2f` · v012 `530b8133`. New: v001 `1fb5e646`. Controller
`337ff430 → d16eb7f7`, GuardGait `1d3cd5d6 → 3bc3ebda`, Player.prefab `67ef0650` unchanged. Research-reference
scan: none of the six `__fresh*` guids appears in the controller; no `__v1A/__v2/__v3/__place/__det` temp assets
remain. `AnimationAssetSafety.VerifyProduction` CLEAN.

Harness-only changes (no production effect): `RuntimeQaDriver` gained `ang<deg>` script segments, an
`RTQA_laneX` start-x pref and a 24 m-wide ramp; `RtqaQueue` treats `RTQA_laneX` as a float. The test scene has a
corridor wall at x ≈ 16.6, low walls at z ≈ −5.3 (x 16–29) and a backboard at x 38–41 / z −8.5: pure diagonals
run from x = 40, forward-heavy sequences from x = 35.

## PRODUCTION STATE

```
Light_Walk8  Forward     = Sword1H_WalkForward_v012      @ (0, 1)                 ts 1.00, cycleOffset 0.92
Light_Walk8  ForwardLeft = Sword1H_WalkForwardLeft_v001  @ (-0.7071068, 0.7071068) ts 1.00, cycleOffset 0.92
Light_Walk8  children 1-5 mocap ring unchanged; child 7 Strafe45Left parked at (0,0), zero weight
Travel_Walk8 unchanged (Forward = v008 @ ts 0.50)
GuardGait_Knight1H walk: -141.6:1.84  -97.1:1.82  -45:1.24  0:1.34  86.3:1.81  125.3:1.84  170.2:1.80
FootIK Solver = AnimatedBones (serialized `Solver: 0`)
EquipmentManager.autoFitGrip = 0 (authored grip)
no research clip referenced · no temp guids · harness disarmed · nothing committed
```
