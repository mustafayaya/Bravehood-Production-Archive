# `__freshHumanRunForward_01` — TRUE RUN FORWARD research challenger (2026-09-07)

Research only. Not installed. Production controller, Light_Walk8, Travel_Walk8, CombatIdle_v005, Idle entry offset 0.18,
GuardGait, FootIK, grip and Player values untouched (md5 read back; VerifyProduction TRUE). The Player reached the run
through a research controller copy (`Knight_Controller_RunFwd01Study`: Light_Run8's forward child → the research clip at
(0, 1), Gait_Light thresholds 1.3 / 1.6 so the run tier is fully selected above 1.6 m/s) and a research gait table
(`GuardGait_RunFwd01Study`, jog @ 0° = the measured native 2.05 m/s), both under Generated; session-only harness
overrides (WalkRef / JogRef 1.3 / 1.6, sprint world speed) never touched the prefab.

### RUNFORWARD01 MOTION DESIGN
A new RUN mode in `HumanWalkPerformance` (`Options.runGait`, inert at default: the Fresh02 walk recipe rebuilds v012 to
3.8e-6, i.e. unchanged), built on the walk generator's principles — feet are authored as world paths, hip height is a
consequence of the stance leg's geometry, arms are an overlap chain — with the biomechanics a run needs and a walk
cannot express: each foot is scheduled on the ground for 45 % of the cycle (`runToeOffPhase 0.45`), so the two stances
no longer overlap and a short flight separates them; the foot lands 260 mm ahead of its hip and pushes off 360 mm behind
it (`runContactMm / runToeOffMm`, the planted track = the native speed); the recovery lifts the ankle 190 mm
(`runSwingLiftMm`, peak 45 % into the swing); acceptance compresses 24 mm (`runSinkMm`, deeper and earlier than the
walk's 14); the hip height through flight is interpolated between push-off and landing plus a 10 mm ballistic rise
instead of standing on two airborne feet; pelvis yaw ±7° (walk ±5), lateral shift ±9 mm (walk ±17: a run does not sway);
forward lean dial 25 split spine / chest / upper chest (measured on this rig: ~0.2° of upper-chest pitch per dial
degree; result +5.3° of chest pitch over the walk) with the neck lifting the gaze back; off-hand swing ×1.6 of the walk
formula. Same fighter: the combat carry, the sword-arm lag chain, the pelvis-roll table, the world-flat sole planting
(`plantedFootRoll`: midfoot strike 5° toes-up, heel-off from 0.24 pivoting 32° about the toe), per-leg reach planning
(margin 28 mm, +14 right), stance-yaw stabiliser (release 0.36 / 0.12). Cycle 0.66 s, 48 samples, minKnee 15,
stanceKnee 32, armGain 0.6. Legality PASS (7 interpolation fixes). Generator repairs: the walk's 0.62 toe-off and its
pitch windows are now `ToeOff(o)`-driven for the run while the walk keeps its literal path; a run-only toe-relax span
(0.09) and early-swing lift (0.6 of the peak) keep the pitched push-off toe clear of the floor.

### RUN GAIT / CONTACT SCHEDULE
Left contact 0.97 → toe-off 0.40 (43 % of the cycle), right contact 0.45 → 0.88 (43 %); double support 0 %; flight 13.3 %
(two flights of ~6.6 % ≈ 44 ms at native; the semantic detector with its 30 mm band reads 0.98–0.43 / 0.46–0.91, flight
16.3 %). LEFT CONTACT → compression (sink peaks 0.09 after contact) → propulsion (heel-off from 0.24) → flight →
RIGHT CONTACT → … as specified; no double support was enforced or needed.

### STRIDE / FOOTFALL
Contact 260 mm ahead of the hip, toe-off 360 mm behind (right foot ×0.985); planted track 2.048 / 2.042 m/s along
travel (audit, ±0.065 / 0.035), lateral drift 7 / 11 mm, support-yaw drift 3.3 / 0.6°. Step length at native
2.05 × 0.33 = **0.68 m** (walk v012: 0.54 m at its native); no bounding stride: stance 620 mm, boot separation ≥ 170 mm
at runtime, feet on their lanes (FootX table, lateral ±95 mm).

### FLIGHT PHASE
Present, short and measured, not forced: 13.3 % of the cycle in two flights; hip rise through flight 10 mm on top of
the interpolated line. The stance micro-study (below) brackets it: 0.42 → 17 %, 0.45 → 13 %, 0.48 → 10 %.

### KNEE / REACH
Knee minimum **27.2° / 18.2°** (L / R; walk v012 shipped at 4° right), reach max 0.972 / 0.987 of the per-leg limit,
0 % of the cycle under 6°; the 17 mm shorter right leg is planned against its own limit (margin 28 + 14 mm). Without
the margin split the first build put the right knee at 4.4° — the old WalkForward failure, caught before review.

### SWING FOOT
Ankle clearance peaks 190 mm above the floor at 0.69 (L) / 0.18 (R); toe bone never below its flat height (61 / 53 mm),
heel / toe minimum 7 / 1 and 4 / 3 mm (audit, buried 0 %); touchdown vertical velocity −162 / −179 mm/s (a placed foot,
not a stab); push-off pitch −32° relaxes over 0.09 of the cycle while the ankle lifts 60 % of its peak in the first
12 % of swing, so the pitched toe clears; strike 5° toes-up (midfoot). No skim, no cut-through, no high-knee.

### PELVIS / WEIGHT / BOUNCE
Pelvis 695…765 mm — 70 mm of vertical rhythm (walk v012 ~70, FR 58) from the geometry plus the 24 mm acceptance sink and
the 10 mm flight rise; the trough sits after contact, the crest through mid-stance. Reads heavy: the support knee never
straightens past 27 / 18°, the body drops as the mass arrives, and the flight is short.

### TORSO / HEAD
Upper-chest pitch mean **7.5°** forward (idle 3.8, walk v012 2.2 → +5.3° over the walk), range 6.4…8.6 (2.2° of bob,
the walk's 2/cycle chest breath unchanged); the pelvis still leads with ±7° yaw counter-rotated up the spine (spine
0.55 / chest 0.70 / upper chest 0.80 with the walk's lags). Head pitch mean **1.0°** (walk 1.4, idle 5.7: chin slightly
down), bounce −0.9…2.0°; head yaw on the lock target throughout (−10.2° constant at runtime). Tall, not hunched.

### SWORD / SHOULDER / OFF-HAND
Sword-hand travel along z 57 mm per cycle, off-hand 164 mm (the off-hand does the balancing, ×1.6); sword path 332 mm /
cycle (walk ~300), peak sword speed 1.69 m/s just after the right contact (walk-family idle-entry peaks ~1.0–1.3), sword
never closer than 449 mm to the upper chest (walk 455–459), hand-to-chest distance within 17 mm of the idle's; clavicle
±1.5° × 0.6 as the walk, no compression, no cross-chest counter-swing (ArmFwd ±5° × 0.6 from the yaw table).

### FOOT CONTACT AUDIT (`LocomotionFootContactAudit`, 240 samples)
| leg | knee min | reach | <6° | world pitch vs flat | heel / toe min | buried | support yaw drift | lateral drift | planted along speed |
|---|---|---|---|---|---|---|---|---|---|
| Left | 27.2° | 0.972 | 0 % | −32…+5° | 7 / 1 mm | 0 % | 3.3° | 7 mm | 2.048 ± 0.065 m/s |
| Right | 18.2° | 0.987 | 0 % | −32…+5° | 4 / 3 mm | 0 % | 0.6° | 11 mm | 2.042 ± 0.035 m/s |

Runtime (real Player, FootIK AnimatedBones with sole levelling): planted mid-stance drift 5 / 4 mm (max 6 / 4), support
yaw drift 5.8 / 2.0°, knee 27.3 / 18.2°, toe bone minimum 55 / 45 mm, boot separation ≥ 170 mm, root 354.29° constant.

### NATIVE SPEED / CYCLE / CADENCE
Native planted-foot speed **2.05 m/s** (audit 2.048 / 2.042; schedule 2.088 before the heel-off pivot advances the
ankle), cycle **0.66 s**, cadence **182 spm**, step 0.68 m. Walk v012 for reference: 1.34 m/s native, 0.80 s, 150 spm.

### SPEED STUDY (real Player, lock-on, loadout 0.880, research rating; stride-matched by construction)
| candidate | playback | world speed | cadence | 0.25 s | 0.5 s | 1.0 s | stride error |
|---|---|---|---|---|---|---|---|
| CONTROLLED | 0.902× | 1.850 m/s | 164 spm | 0.46 m | 0.92 m | 1.85 m | +0.4 mm/s |
| **MEDIUM (native)** | **1.000×** | **2.050 m/s** | **182 spm** | 0.51 m | 1.02 m | 2.05 m | +0.4 mm/s |
| FAST | 1.146× | 2.350 m/s | 208 spm | 0.59 m | 1.17 m | 2.35 m | +0.5 mm/s |
| CURRENT GAMEPLAY 4 m/s (unarmoured sprint, diagnostic) | **1.500× (clamp)** | 4.000 m/s | 273 spm | 1.00 m | 2.00 m | 4.00 m | **+925 mm/s** |

The 4 m/s row is the finding, not a candidate: this run would need 1.95× playback to keep up with the current sprint
authority; MotionSpeed clamps at 1.5× and the feet skate 0.93 m/s. At the loadout the current sprint is 3.52 m/s (1.72×).
Either the run tier presents at ~1.85–2.35 m/s (stride-matched, as the walks are), or a genuinely faster clip is needed
for the top of the sprint range. Facing identical across candidates (pelvis +3.1…+3.7, shoulders −1.1 in the root
frame); root constant in every run.

### SELECTED PRESENTATION
Recommended for review: **MEDIUM = native, 2.05 m/s, 182 spm, 0.68 m steps** — the run as authored, no retime. CONTROLLED
(1.85, 164 spm) reads heavier and closer to a fast walk on the numbers; FAST (2.35, 208 spm) is still stride-clean but
the cadence approaches frantic. The human decides; nothing is encoded.

### STANCE MICRO-STUDY (the one ambiguity: how grounded)
| | stance / foot | flight | native | knee min L / R | runtime |
|---|---|---|---|---|---|
| A `__runfwd_A` | 0.42 | 17.1 % | 2.19 m/s | 27.3 / 22.5° | 2.190, MS 1.000, err 0.0 |
| **B = RunForward01** | **0.45** | **13.3 %** | **2.05 m/s** | 27.2 / 18.2° | 2.050, MS 1.000, err +0.4 |
| C `__runfwd_C` | 0.48 | 10.0 % | 1.92 m/s | 29.8 / 13.2° | 1.920, MS 1.000, err 0.0 |

B is the challenger: A is airborne a third longer and reads lighter; C's right knee falls to 13° (the longer stance
pushes the short leg toward its reach) and its flight is barely there. Video I shows all three.

### WALK vs RUN
Same session, same lane: walk v012 1.144 m/s / 0.854× / 128 spm / 0.54 m steps, pelvis +4.1 shoulders −0.9 (root frame);
run 2.050 / 1.000× / 182 spm / 0.68 m, pelvis +3.7 shoulders −1.1 — the facing does not change, the chest leans 5° more,
the pelvis rhythm is 70 mm in both, the sword path grows 300 → 332 mm. It is not a sped-up walk: stance 62 → 45 %,
flight 0 → 13 %, knee lift 172 → 263 mm ankle height, contact 325 → 260 mm ahead, push-off 325 → 360 behind.

### IDLE → WALK → RUN → WALK → IDLE FAMILY CONTEXT (rn_family)
Walk → Run: pelvis +1.4 → +3.6 (Δ +2.2°) at 241°/s, shoulders 47°/s, chest pitch 2.2 → 7.5, cadence 128 → 182, world
1.144 → 2.050 (acceleration ~0.23 s in the KCC), sword 1.63 m/s, feet through the gait crossfade 4.7 / 8.5 m/s, knee
1145°/s, boot separation ≥ 166 mm. Run → Walk: Δ +0.9° at 123°/s, feet 3.9 / 4.7, sword 1.35. Idle → Walk and Walk → Idle
as production (2.97 / 3.49 and 2.53 / 3.71 m/s). The same fighter accelerating: nothing in the facing, sword or head
moves at the seams; the foot speeds at the walk ↔ run crossfade are the Gait_Light blend's own phase mismatch (the two
rings are not phase-aligned — a cycleOffset study, not the clip).

### RUN START (Idle → RunForward01, research only, untuned)
Δpelvis +32° at 291°/s, shoulders 218°/s, feet 6.3 / 10.7 m/s, knee 726°/s, sword 1.64 m/s; the exit (run → idle) 8.0 /
8.6 m/s, knee 1949°/s. With the production entry offset 0.18 and the study cycleOffset 0.11 the run enters at clip phase
0.29 (left mid-stance). Deferred by design: the entry phase / cycleOffset study follows the motion's approval, as with
the walk.

### TECHNICAL QA
Humanoid legality PASS (interpolation, 24 samples per span); deterministic (two builds max |Δ| 0; rebuild vs the saved
asset 0); canonical root (sampled root 0.0000 / yaw 0.00; RootT.x 1…19 mm, RootT.z constant −10 mm, RootT.y 886…962 mm
BAKED into the pose, |RootQ.y| 0.061 = ±7° pelvis yaw, no root translation); clip contract loop ON / blend OFF /
orientation-Y-XZ baked / Based Upon Original / heightFromFeet OFF, 0.66 s, 60 fps, 102 curves; temporal audit at 1.0×:
right ankle p95 701 / max 1150°/s, right knee p95 583 / max 1240, left knee 686 / 828, hips ≤ 552, largest single-frame
step 20.5° (right knee at toe-off), 0 hold-snaps; semantic support schedule and swing-foot metrics above; foot contact
audit above; bodyPosition compensation: the pelvis rhythm arrives on screen (70 mm at runtime, pelvis speed 1.3–1.5 m/s
at the seams only). FootIK: raw audit first (buried 0 %), then the real Player with FootIK on — the IK repaired nothing
(planted drift ≤ 6 mm, root constant).

### VIDEOS (`Artifacts/AnimationReview/`)
A `RunForward01_A_native_2p05.mp4` (Idle → run → Idle, native) · B `RunForward01_B_controlled_1p85.mp4` ·
C `RunForward01_C_medium_2p05_native.mp4` · D `RunForward01_D_fast_2p35.mp4` ·
E `RunForward01_E_2x2_controlled_medium_over_fast_4ms.mp4` (bottom-right = current 4 m/s sprint, skating) ·
F `RunForward01_F_WalkForward_v012_left_vs_RunForward01_right.mp4` · G `RunForward01_G_Idle_Walk_Run_Walk_Idle.mp4` ·
H `RunForward01_H_gameplay_selected_medium_2p05.mp4` · I `RunForward01_I_stance_study_A042_B045_C048.mp4` ·
X `RunForward01_X_4ms_current_gameplay_diagnostic.mp4` · ref `RunForward01_ref_WalkForward_v012.mp4`.
All on the real Player, gameplay camera, lock-on, loadout 0.880 (4 m/s row unarmoured), FootIK, UpperBodyAim,
WeaponInertia, 60 fps, MaxStableMoveSpeed 2 / MaxSprintSpeed 4 on the prefab and no scene override.

### PRODUCTION SAFETY
Research assets under Generated: `__freshHumanRunForward_01`, `__runfwd_A`, `__runfwd_C`, `Knight_Controller_RunFwd01Study`,
`GuardGait_RunFwd01Study`. Production controller, GuardGait, TravelGait, Player prefab, CombatIdle_v005, Forward v012
md5-identical to the Travel install; VerifyProduction TRUE; Light_Walk8 / Gait_Light / Light_Run8 read back unchanged
(mocap run ring, thresholds 1.85 / 3.655). Harness prefs reset (controller, gait, refs, sprint speed, lock, load,
queue). Code changed (working tree only): `HumanWalkPerformance.cs` (run mode), `RuntimeQaDriver.cs` (`run<deg>` sprint
segments with stamina refill, `RTQA_walkRef / RTQA_jogRef / RTQA_sprintSpeed` session overrides). Git index untouched;
nothing added, nothing committed. Editor out of Play Mode for every write.
