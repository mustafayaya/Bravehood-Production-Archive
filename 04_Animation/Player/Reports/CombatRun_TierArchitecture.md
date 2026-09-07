# Walk / Run / Sprint tier architecture — forensic map and decision (2026-09-07)

Result: **RUN TIER ARCHITECTURE BLOCKED** on a product decision. The legacy sprint forward child and its rating were
restored; production sprint behaviour is regressed to nothing (measured). `Sword1H_RunForward_v001` stays on disk,
production-ready, unreferenced.

### CURRENT WALK / RUN / SPRINT ARCHITECTURE (traced input → final writer)
- **Input**: `InputSystem_Actions` has one locomotion modifier, `Sprint` (plus the roll button held ≥ 0.25 s):
  `PlayerInputHandler` → `CharacterInputs.SprintHold`. No Run / Jog / walk-toggle action; `MoveVector` magnitude is used
  for direction only (the input is normalised into the KCC).
- **Character state** (`BaseCharacterController`): `_isSprinting = SprintHold` gated by stamina (`Stamina.Drain`,
  `SprintExhausted`, re-engage 0.15). Speed target = crouch ? `MaxCrouchSpeed` : (sprint ? **`MaxSprintSpeed` 4.0** :
  **`MaxStableMoveSpeed` 2.0 × `CombatStanceSpeedMultiplier`**) × `LoadSpeedMultiplier` × `StatusSpeedMultiplier` ×
  `CombatMoveScale` × directional tax (or the guard table's `DirectionalSpeedScale` in the stance). Two speed states.
  `CombatStanceSpeedMultiplier` is written every frame by `CharacterAnimator` = lerp(TravelWalkSpeedScale 0.65,
  CombatWalkSpeedScale 0.65, stance) → 1.30 base walk, 1.144 at the loadout. Sprint = 4.0 × load = 3.52 at the loadout.
- **A. World movement speed final writer**: `BaseCharacterController.UpdateVelocity` (KCC), from the target above.
- **Animator**: one `Locomotion` state (speed parameter `MotionSpeed`) → `Stance_Blend` (Stance 0 = `Travel_Gait`,
  1 = `Gait_Light`). `Gait_Light` = 1D on the raw **`Speed`** parameter (smoothed KCC planar speed): `Light_Walk8` @ 1.85,
  `Light_Run8` @ 3.655. `Travel_Gait` = 1D on **`Gait`** (computed by CharacterAnimator from the travel walk / jog
  natives): `Travel_Walk8` @ 0, `Travel_Jog8` @ 1, `Loco_Sprint_Fwd` @ 1.2. `IsRunning` = "is moving", not sprint.
- **B. Run / Sprint tier selection final writer**: the `Speed` (guard) and `Gait` (travel) parameters, i.e. the ground
  speed itself; the only thing that raises it above the walk anchor is the Sprint input. Run and Sprint are one tier.
- **C. Native rating**: `GuardGait_Knight1H` — `walk` (8 authored entries) and `jog` (8 legacy mocap headings:
  −141.5 … 171.5, forward = **−8.7° : 3.56**); TravelGait `walk` (8 authored), `jog` empty → legacy code table
  (`TravelJogAng/Ref`, forward 3.37); `TravelSprintRefSpeed` 3.74 for the 1.2 child.
- **D. MotionSpeed final writer**: `CharacterAnimator.Update`: expected = lerp(walk native, jog native,
  InverseLerp(`WalkRefSpeed` 1.85, `JogRefSpeed` 3.655, speed)) (guard) / lerp(walk, jog, gait) (travel), mixed by
  stance; MotionSpeed = speed ÷ expected, clamped **0.6 … 1.5**; the `Locomotion` state's speed parameter.

### ARCHITECTURAL CHANGE
None applied. A distinct Run tier needs a selector that existing player state does not provide (below), and the brief
forbids guessing one. The earlier informal pass's global sprint multiplier and gait retime remain reverted.

### RUN TIER
Not created. The approved asset (`Sword1H_RunForward_v001`, native 2.05, contract verified, key-identical to the
research clip) is unreferenced. The seven off-forward run children are the legacy mocap set in both rings.

### SPRINT TIER
Restored to its pre-install configuration: `Light_Run8` forward child = `Sword1h_RunLt45Loop` (Sword1h_Walks.fbx,
fileID 7400064) at (−0.15126081, 0.98849386) = −8.7°, timeScale 1, cycleOffset 0.93, mirror off; guard jog rating
−8.7° : 3.56. Controller, GuardGait and Player prefab are md5-identical to the Travel-install state (cf50e6e0… /
9f322f42… / c7a19f4a…). No legacy asset deleted.

### INPUT / STATE SELECTION
Existing state offers no Run vs Sprint distinction. The options, each a product decision:
- **A speed-driven Walk → Run → Sprint**: impossible as is — there are only two sustained world speeds (1.30 / 4.0);
  Run would exist only during the ~0.3 s acceleration.
- **B stance-driven**: lock-on + Sprint = Run (2.05, "advancing on the threat"), free travel + Sprint = Sprint (4.0).
  Uses only existing state (the stance already selects whole animation sets), but it lowers the lock-on sprint from
  3.52 / 4.0 to 1.80 / 2.05 m/s — a gameplay change.
- **C Sprint enters Run then Sprint above a threshold**: Run would be transient; needs a hold-time or stamina rule
  (e.g. Sprint → Run when exhausted instead of dropping to walk) — a gameplay rule.
- **D a new input** (walk/run toggle, analog band): no such action or magnitude semantics exist.
**Decision required**: which player state means RUN (B, C, or a new input D). Nothing else in the chain blocks the
tier once that is decided.

### WORLD SPEED AUTHORITY
Once a selector exists the addition is small and data-driven: a Run speed target on the character
(e.g. a third state in the speed-target expression, 2.0 / 4.0 untouched) or the jog table's existing
`gameplayScale` (0° : 0.5125) sampled in the run branch the way the walk table's scale is sampled for the stance walk;
`Gait_Light` (or a third child) and the rating anchors at the presentations; the walk / run / sprint choice made on a
load-normalised speed (else a loaded knight at 1.80 mixes walk into his run). Not applied.

### GAIT DATA SEPARATION
Not changed: `GuardGait.walk` = walk family (1.34 forward), `GuardGait.jog` = the legacy run ring (3.56 forward). A Run
tier would need its own native list (or a third table) so Sprint never divides by 2.05 and Run never by 3.56.

### MOTIONSPEED CONTRACT
Unchanged: speed ÷ expected native of the active tier, clamp 0.6 … 1.5. Verified below.

### RUNFORWARD PRODUCTION RESULT
Not installed. Its qualification numbers (research controller copy, session-only speed override): 2.050 m/s, 1.000×,
182 spm, 0.660 s, stride error +0.4 mm/s, planted drift 5 / 4 mm, authored flight 13.3 % — `Reports/
CombatRun_ForwardRun_v001_Install.md`.

### SPRINT REGRESSION RESULT (real Player, production, no overrides)
| run | world | MotionSpeed | ring weights | blended native | skate | first movement |
|---|---|---|---|---|---|---|
| lock-on sprint, loadout | 3.520 m/s | 1.044 | RunLt45 0.75 + RunFwd 0.17 + walk 0.075 | 3.39 | −0.02 m/s | frame 2 (33 ms) |
| lock-on sprint, unarmoured | 4.000 m/s | 1.131 | RunLt45 0.81 + RunFwd 0.19 | 3.56 | −0.03 m/s | frame 1 (17 ms) |
| free-travel sprint, loadout | 3.520 m/s | 1.000 | Loco_Jog_Fwd 0.60 + Loco_Sprint_Fwd 0.40 | — | — | — |

Sprint activates on the input, reaches the 4.0 authority, starts immediately, uses the legacy rating (not 2.05) and
the stamina state cycles as before (`_isSprinting` true through the sprint segment, false after). Pre-existing legacy
art notes, not touched: under lock-on the mocap forward run turns the head +38° and the shoulders +11° against the lock
target; the forward input blends two mocap children (−8.7° and +38°).

### WALK REGRESSION RESULT
Walk 1.144 m/s / 0.854× / 128 spm before, during and after the sprint (sp_family), Light_Walk8 eight children and
offsets, walk ratings and scales, Idle v005, Idle → Locomotion 0.18 / 0.15, Travel_Walk8 / TravelGait: unchanged
(md5-identical files, AF1 + the existing contracts, 56 / 56).

### WALK → RUN → SPRINT DIAGNOSTIC
Not possible without a Run tier. Recorded Walk → Sprint → Walk instead: 1.144 (walk, Speed 1.14) → 3.520 (legacy ring
0.925 + walk 0.075, MotionSpeed 1.044, native 3.39) → 1.149.

### TESTS
`AF1_RunForward_v001_AssetReady_LegacySprintForwardRestored`: the run asset exists, baked, key-identical, and is NOT
referenced; legacy forward child at its serialized config (position, co 0.93, ts 1); jog −8.7 : 3.56 and no 0° entry;
Gait_Light on Speed 1.85 / 3.655; WalkRef / JogRef 1.85 / 3.655; walk 1.34 with eight entries; Player 2 / 4; no research
or improvised parameter in the controller. No speculative tier tests. Suite **56 / 56**.

### VIDEOS (`Artifacts/AnimationReview/`)
A `RunTier_A_WalkForward_v012.mp4` · B `RunTier_B_RunForward_v001_2p05_presentation_diagnostic_only.mp4` (override
session, not production) · C `RunTier_C_Sprint_legacy_lockon_loadout_3p52.mp4`, `_C2_…unarmoured_4ms.mp4`,
`_C3_…free_travel_3p52.mp4` · D `RunTier_D_Walk_Sprint_Walk_lockon_production.mp4`. D / E of the brief (Walk → Run →
Sprint) need the tier.

### PRODUCTION STATE
Production files identical to the Travel-install state (controller, GuardGait, TravelGait, Player.prefab, all clips);
only the test file changed. Unreferenced under Generated: `Sword1H_RunForward_v001`, the research clips and study /
presentation controller copies. VerifyProduction TRUE, harness disarmed, 0 strays. Git index untouched.
