# Sword1H_RunForward_v001 — production install and qualification (2026-09-07, formal brief)

Human review: `__freshHumanRunForward_01` APPROVED at native 1.00× (~2.05 m/s, 182 spm, 0.66 s, 0.68 m steps, ~13 % flight).
Classification: RUN FORWARD, not the 4 m/s Sprint. This milestone supersedes the earlier informal "apply the run" pass:
the improvised sprint presentation of that pass (a `RunSpeedScale` / `SprintSpeedMultiplier` global multiplier, an
`IntentSpeed` gait parameter with retimed Gait_Light thresholds and rating anchors, a Travel_Jog8 run child and a
TravelGait jog table) collapsed Run and Sprint into one tier and reached outside the brief's scope; all of it was
reverted before this qualification (scripts and Player.prefab restored to their pre-install state, Travel_Jog8 forward
back to `Loco_Jog_Fwd`, TravelGait jog list empty, the `_Travel` run copy removed).

### RUNFORWARD v001 ASSET
`Generated/Sword1H_RunForward_v001.anim` (guid 7777f3cf…), `AssetDatabase.CopyAsset` of `__freshHumanRunForward_01`
(untouched, guid 10b73aea…). Contract: loopTime ON, loopBlend OFF, orientation / Y / XZ baked, Based Upon Original,
heightFromFeet OFF, 0.66 s at 60 fps, 102 curves, canonical root (sampled root 0.0000 / yaw 0.00, RootT.z constant:
no positional root motion, translation is the controller's).

### SOURCE EQUIVALENCE
102 / 102 bindings, 0 missing, max |Δvalue| / |Δtime| / |Δtangent| 0; sampled pose on the Player prefab 120 samples ×
22 bones: 0.0005 mm / 0.0000°. Legality PASS.

### LIGHT_RUN8 FORWARD SLOT
Serialized `Light_Run8` inspected by POSITION before the edit: the active child nearest 0° was `Sword1h_RunLt45Loop`
(fileID 7400064) at (−0.151, 0.988) = −8.7°, co 0.93 (the mocap ring is placed at measured headings; `RunFwdLoop` sits
at +37.9°). That child alone now holds **`Sword1H_RunForward_v001` at (0, 1)**, timeScale 1, mirror off. The other
seven run children are unchanged (37.9 / 80.8 / 124.5 / 171.5 / −141.5 / −99.6 / −55.6°, their offsets intact); no active
child within 30° of forward.

### RUN / SPRINT ARCHITECTURE (inspected, not redesigned)
- Gameplay has TWO speed states: walk = `MaxStableMoveSpeed 2.0 × CombatStanceSpeedMultiplier` (CombatWalkSpeedScale
  0.65 → 1.30, ×load) and sprint = `MaxSprintSpeed 4.0` (×load) while SprintHold. There is no third "run / jog" movement
  state and no data-driven run presentation value: the walk table's `gameplayScale` is sampled only for the stance walk
  (`DirectionalSpeedScale`), the jog list's `gameplayScale` field exists but nothing reads it.
- The animator's run ring (`Light_Run8`, lock-on) is reached only through `Gait_Light`, a 1D blend on the raw `Speed`
  parameter with thresholds 1.85 (Light_Walk8) / 3.655 (Light_Run8): the run ring is selected by sprinting. At the
  loadout sprint (3.52 m/s) the tree mixes 7.5 % walk; unarmoured (4.0) it is 100 % run.
- `MotionSpeed` = real speed ÷ expected native, expected = lerp(walk native, jog native, InverseLerp(WalkRefSpeed 1.85,
  JogRefSpeed 3.655, speed)), clamped to [0.6, 1.5]. Natives come from `GuardGait_Knight1H` (walk and jog tables) under
  lock-on and from `TravelGait` walk + the legacy code jog table in free travel.
- **Run and Sprint are the same tier in code.** The approved 2.05 m/s presentation therefore cannot be selected by
  production gameplay data without an architectural addition, and the brief forbids improvising one. Reported below.

### RUN CYCLE OFFSET
Measured on the production asset (root-local foot / toe heights, 200 samples): left foot lands at clip **0.970**, lifts
0.405; right lands 0.450, lifts 0.880. The legacy forward run child it replaces (`Sword1h_RunLt45Loop`, 0.70 s, co 0.93)
lands its left foot at clip 0.970 too, so the same offset keeps every phase relationship of the existing ring:
**cycleOffset 0.93** (left landing at tree 0.90, exactly where the legacy forward run landed). Not the research slot's
experimental 0.11. Standalone looping is clean at any offset; the walk ↔ run alignment is the later phase study.

### NATIVE RUN GAIT RATING
`GuardGait_Knight1H.jog`: the forward mocap entry (−8.7° : 3.56) → **0° : 2.05** (the production audit's planted-foot
speed 2.048 / 2.042 m/s). Walk table untouched (0° : 1.34, the eight walk entries and scales identical); TravelGait
untouched. No playback rate is encoded anywhere in the Animator (timeScale 1, MotionSpeed does the matching).

### GAMEPLAY PRESENTATION
No distinct data-driven Run presentation value exists in the current architecture (see above), so per the brief none
was improvised. Production gameplay today: sprint = 3.52 m/s at the loadout / 4.0 unarmoured selects the run ring with
MotionSpeed pinned at the 1.5 clamp (required 1.76× / 1.95×) → 0.53 / 0.93 m/s of skate (measured, videos D / E / G).
What the approved presentation would need, stated for the architecture decision (none of it applied):
1. a run-tier presentation value — the natural home is the jog table's existing `gameplayScale` at 0° (0.5125 = 2.05 /
   4.0), sampled in the SPRINT branch of `BaseCharacterController` the way the walk table's scale is sampled for the
   stance walk (`DirectionalSpeedScale`), so MaxSprintSpeed 4.0 stays the authority and the run presents at 2.05 (1.80
   at the loadout, load scaling playback like the walk);
2. `Gait_Light` thresholds and `WalkRefSpeed / JogRefSpeed` at the two presentations (1.30 / 2.05), and the walk / run
   choice made on a load-normalised speed (else a loaded knight at 1.80 m/s mixes 33 % walk into his run);
3. a separate SprintForward motion if 4 m/s stays the top-speed presentation (watch item below).
The approved presentation was demonstrated with session-only harness overrides (a controller COPY with Gait_Light at
1.3 / 2.05, rating anchors 1.3 / 2.05, sprint world speed 2.05 rewritten on the live instance; nothing saved).

### RUNTIME SPEED / CADENCE / FLIGHT (real Player, lock-on, loadout 0.880, FootIK, UpperBodyAim, WeaponInertia, 60 fps; Player 2 / 4, no scene override)
| run | config | world | MotionSpeed | cadence | cycle | stride error | run weight |
|---|---|---|---|---|---|---|---|
| approved presentation (diagnostic) | copy 1.3 / 2.05, sprint 2.05 | **2.050 m/s** | **1.000** | **182 spm** | **0.660 s** | **+0.4 mm/s** | 1.000 |
| approved presentation, unarmoured | idem, load 1.0 | 2.050 | 1.000 | 182 | 0.660 | +0.4 | 1.000 |
| production sprint, loadout | as shipped | 3.520 | 1.500 (clamp) | 268 | 0.447 | **+445 mm/s** (0.53 skate) | 0.925 (+0.075 walk) |
| production sprint, unarmoured | as shipped | 4.000 | 1.500 (clamp) | 273 | 0.440 | **+925 mm/s** (0.93 skate) | 1.000 |

Load does not change the presentation at 2.05 (the override fixes the world speed); planted drift 5 / 4 mm (max 6 / 4),
support-yaw drift 5.4 / 2.4°, boot separation ≥ 170 mm, root 354.29° constant. Flight: authored 13.3 % (semantic
detector 16 %); the same 12 mm toe/ankle band at runtime reads 21 % (FootIK sole levelling and settle move the toe
bones; the authored schedule is the reference).

### KNEE / REACH
Production audit: knee minimum **27.2° / 18.2°**, reach 0.972 / 0.987 of each leg's own limit, 0 % under 6°; runtime
27.1 / 18.2°. The right leg keeps its own reach margin (+14 mm on the 28 mm planning margin) — no shared limit.

### FOOT CONTACT
`LocomotionFootContactAudit` on the production asset: pitch −32…+5° (heel-off 32°, midfoot strike 5°), heel / toe
minimum 7 / 1 and 4 / 3 mm, buried 0 %, support-yaw drift 3.3 / 0.6°, planted track 2.048 ± 0.065 / 2.042 ± 0.035 m/s.
Runtime planted drift 5 / 4 mm, toe bone minimum 54 / 44 mm, touchdown −162 / −179 mm/s authored. FootIK stayed a
terrain correction (drift ≤ 6 mm, root constant).

### TORSO / HEAD
Upper-chest pitch 7.5° (walk v012 2.2 → +5.3°), 2.2° of bob; head pitch 1.0° (walk 1.4), 3° bounce, yaw −10.2° on the
lock target throughout; root constant. Unchanged from the approved research clip (key-identical).

### SWORD / SHOULDER
Sword path 332 mm / cycle, peak 1.69 m/s after the right contact, pivot ≥ 449 mm from the upper chest, hand-to-chest
within 17 mm of idle; clavicle ±1.5° × 0.6, no compression; off-hand 164 mm of travel vs the sword hand's 57 (restrained
balance, no pump). Unchanged (key-identical).

### WALK ↔ RUN BASELINE (WalkForward_v012 → RunForward_v001 → WalkForward_v012, nothing tuned)
Production (sprint 3.52): walk → run Δpelvis +1.8° at 191°/s, shoulders 64°/s, **feet 8.0 / 7.8 m/s**, knee 1178°/s,
sword 1.52; run → walk Δ +5.1° at 189°/s, feet 7.6 / 5.4, knee 1293°/s, sword 1.74, min separation 166 mm.
Approved presentation (2.05): walk → run Δ +1.8° at 131°/s, feet 5.0 / 5.3 m/s, knee 1326°/s, sword 1.29; run → walk
Δ +1.6° at 128°/s, feet 3.8 / 4.6. Facing, head and sword continuous; the foot speeds are the un-aligned phase of the two
rings (walk lands left at tree ~0.08, run at tree 0.90) — the dedicated Walk ↔ Run phase study. Walk cycleOffset, run
cycleOffset and the transition offset untouched.

### IDLE → RUN BASELINE (Idle entry offset 0.18 untouched)
Production sprint: Δpelvis +32° at 282°/s, feet 7.7 / 9.0 m/s, knee 1077°/s; exit 7.5 / 3.9 m/s. Presentation 2.05:
feet 5.0 / 5.9 m/s, knee 841°/s, sword 1.08; exit 3.1 / 4.1. Entry clip phase with offset 0.18 + co 0.93 = 0.11 (left
foot loaded, right foot leaving: the walk family's entry rule, by coincidence of the shared 0.97 landing). Recorded only.

### 4 M/S SPRINT WATCH ITEM
`SPRINT FORWARD MOTION REQUIRED FOR 4 M/S TOP SPEED` — RunForward native 2.05; required playback at 4.0 = 1.95× (1.72× at
the loadout); MotionSpeed clamp 1.5×; skate 0.93 m/s unarmoured / 0.53 m/s loadout (measured on the real Player). The
old mocap forward run (3.56 native) matched 4 m/s at 1.12×; the authored run replaces it on the ring, so sprinting
now plays RunForward at the clamp until either a run-tier presentation (above) or a SprintForward motion exists.

### VIDEOS (`Artifacts/AnimationReview/`)
A `RunForward_v001_PRODUCTION_A_pure_forward_2p05_presentation.mp4` · B `…_B_WalkForward_v012_left_vs_RunForward_v001_right.mp4` ·
C `…_C_Idle_Walk_Run_Walk_Idle_2p05.mp4` · D `…_D_Walk_Run_Walk_production_sprint_3p52_baseline.mp4` ·
E `…_E_Idle_Run_Idle_production_sprint_3p52_baseline.mp4` · F `…_F_gameplay_camera_2p05_unarmoured.mp4` ·
G `…_G_run_2p05_left_vs_sprint_4ms_right.mp4` (+ `…_sprint_4ms_unarmoured_diagnostic.mp4`). G makes the point: good
at 2.05, skating at 4.0 without a separate Sprint gait.

### TESTS
`AF1_ProductionRunForward_v001_Contract` (rewritten to this brief): production asset exists and is the true forward
child at (0, 1), timeScale 1, baked in-place contract, 0.66 s, key-identity to the research clip, no other active run
child within 30° of forward, the other seven children still the legacy run set, guard jog@0 ≈ 2.05, walk@0 1.34 and the
eight walk entries unchanged, −8.7° entry gone, MaxStable 2 / MaxSprint 4, Gait_Light on `Speed` at 1.85 / 3.655,
WalkRef / JogRef 1.85 / 3.655, Light_Walk8 eight children and offsets, Travel_Jog8 without a run clip, Travel_Walk8
forward copy, TravelGait without a jog table, Idle v005, Idle → Locomotion 0.18 / 0.15, no `IntentSpeed` parameter, no
research run clip in the controller. Suite **56 passed, 0 failed, 0 skipped**.

### PRODUCTION STATE
Changed for this install: `Knight_Controller.controller` (one Light_Run8 child), `GuardGait_Knight1H.asset` (jog 0°),
the new production clip, the test file. Reverted to their pre-install state: `CharacterAnimator.cs`,
`BaseCharacterController.cs`, `Player.prefab` (md5 equal to the Travel-install state; WalkRef 1.85 / JogRef 3.655,
MaxStable 2 / MaxSprint 4), `TravelGait_Knight1H.asset` (jog list empty; walk table as installed), Travel_Jog8.
Untouched: Light_Walk8, all eight walk clips, CombatIdle_v005, walk GuardGait values, Travel_Walk8, FootIK, grip.
VerifyProduction TRUE; harness prefs cleared; research assets (`__freshHumanRunForward_01`, `__runfwd_A / _C`, the
study controllers / gait copy, the presentation controller copy) under Generated, unreferenced. Editor out of Play for
every write; controller, tables and prefab read back from disk. Git index untouched; nothing added, nothing committed.
