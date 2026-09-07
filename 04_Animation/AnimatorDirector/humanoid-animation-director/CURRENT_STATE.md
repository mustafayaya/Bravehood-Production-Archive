# CURRENT STATE — Bravehood humanoid animation, handoff

Read this before starting anything. It exists so the next session does not spend a day
re-deriving hypotheses that were already tested and disproved.

Last updated: 2026-08-27, end of the consolidation milestone.

## The one-paragraph situation

The procedural combat-walk architecture is technically sound and artistically insufficient. Twelve
milestones of solver work produced a clip that passes every technical gate and still reads as
procedural to a human reviewer. Both statements are authoritative and neither cancels the other.
The technical machinery is worth keeping; what it cannot manufacture is motion craft. **The next
move is a better source, not a better solver.**

## What is solved

- Foot planting, support scheduling, ground plane, swing arc, temporal continuity
- Sagittal and frontal posture; chest/head band-limited whole-cycle correction (lower body isolated
  to ~4 mm)
- Determinism, placement invariance, transactional state, controller/asset safety
- The shoulder pathology — root cause found and repaired (below)
- Anatomically priced arm solving: the clavicle can be freed completely on demand

## FOOT REALISM ROOT CAUSE (2026-09-03) — read before touching Forward / ForwardLeft again

`Reports/CombatWalk_FootRealism_Forensic.md`. Human review: HOLD W looks wrong, rapid TAP W looks better. Measured:
tapping never leaves Locomotion at 4 Hz; it only runs the clip at the 0.6 clamp floor under-driven. The defects
are in the SOURCE and old QA never measured them: (1) the foot pitch is an ankle-local table, so the boot follows
the shin — world pitch +27 → −55° through stance, sole flat 9–11 % of stance at runtime, no heel rise, toe
plunges 57 mm below flat at push-off (flat toe bone is 52–61 mm up on this rig, not FootIK's 20 mm);
(2) `WantHipY` plans against a nominal hip 16–30 mm short of the posed one and one left-leg `legLimit` for a
17 mm-shorter right leg → knee 4° / reach 0.999 at strike and push-off, right knee < 6° a third of the cycle;
(3) planted boot yaws 12–24° per stance (static Foot Twist, pelvis yaw pivots it). Diagnostic fix that works on
the real Player: `plantedFootRoll` + `reachMarginMm 20` (per-leg limits) — `__diag_AB_both`: knee < 6° 0/2 %,
sole flat 78 %, toe never buried, rhythm 66, HOLD ≥ old TAP. NOT installed; no v013 yet. `trueHipReach` was
tested and does nothing (rejected).

## FOOT REALISM REPAIR (2026-09-03) — `__freshHumanForward_03` / `__freshHumanForwardLeft_03`, HUMAN REVIEW PENDING

`Reports/CombatWalk_FootRealism_Repair.md`. Research only; v012 / ForwardLeft_v001 untouched. Generator (`v11-planted`,
all default-inert): `plantedFootRoll` (world-flat sole, heel at strike, heel-off pivoting about the toe, Foot Up-Down
solved per frame AFTER the leg solve; profile chosen visually = MEDIUM: strike 12°, heel-off 32°, sole-load 0.07,
heel-off 0.46), `reachMarginMm 20` + `rightReachExtraMm 8` with PER-LEG limits, `stanceYawStabilize 1.0` (thigh
twist as a 4th leg DOF against a world heading, run AFTER the pitch pass — Foot Twist ROLLS the boot, it cannot yaw
it — held until toe release 0.58–0.66, not heel-off: releasing at heel-off swung the planted boot 16°). Results
(Edit-Mode audit): knee < 6° L 0 % / R 1 %, sole flat 43–45 %, heel-off 19 %, no burial, support yaw 0.8 / 1.8°
(v012 10.4 / 36.1), ForwardLeft03 0.6 / 1.7° (was 30.5 / 25.8). `LocomotionFootContactAudit` is the reusable audit
(rig-calibrated sole geometry; flat toe bone 61 / 52 mm — FootIK.ToeBottomHeight 0.02 is wrong by ~35 mm, debt).
Phase 0: Travel forward = `Sword1H_WalkForward_v012_Travel` (key-identical copy, own guid), N1 passes.

## What is NOT solved

- **The motion does not read as hand-authored.** This is the whole remaining problem.
- The sword-discipline / shoulder-freedom tradeoff is a genuine kinematic frontier, not a bug. See
  the frontier table below; pick a point, do not expect to escape it.
- Left femur axial twist 34.4° vs source 16.3° — deliberately deferred, never worked on.

## Current production asset — do not change without instruction

**ForwardLeft (2026-09-03): `Sword1H_WalkForwardLeft_v001`** (`Reports/CombatWalk_ForwardLeft_v001_Install.md`) =
`Object.Instantiate(__freshHumanForwardLeft_02)` + the v012 in-place settings, key-identical. Installed as
`Light_Walk8` child 6 at the TRUE −45° position (−0.7071068, 0.7071068), ts 1, cycleOffset 0.92, replacing the mocap
`Sword1h_StrafeLeftLoop` ring child that sat at −52.8° (child 7 `Strafe45Left` stays parked at the origin — never
pick the slot by clip name). `GuardGait_Knight1H` −52.8:1.82 → **−45:1.24**. Runtime: −45° tree input → v001 weight
1.000, 1.224 m/s unarmoured (forwardness strafe tax 0.941 × 1.30) at 0.987×, 1.082 m/s gameplay loadout at 0.872×;
blend v012→v001 crosses between −20° and −30°; pelvis lead 31.8° / chest 10.1° survive the whole runtime stack.
Human-approved motion is LOCKED (no ForwardLeft03).

**Travel (free, unlocked) forward = v012 too (2026-09-03, user request for in-game testing):** `Travel_Walk8` child 0
`Sword1H_WalkForward_v008 @ ts 0.5` → `Sword1H_WalkForward_v012 @ ts 1.0, cycleOffset 0.92`. v008 is no longer
referenced. The travel walk lane is rated `TravelWalkRefSpeed 1.33` and free forward runs at 2.0 × load (1.77 m/s
with the starting loadout), so the Travel_Gait blends v012 0.78 with Loco_Jog_Fwd 0.21 at MotionSpeed 1.0 — the
same walk/jog mix v008@0.5 produced; planted-foot slip ~0.8–0.9 m/s there is the pre-existing travel-lane design,
not a v012 defect.

**Left (2026-09-03): `__freshHumanLeft_01`** (`Reports/CombatWalk_Left01.md`, research, review pending). A true −90° strafe is a
LATERAL STEP-CLOSE gait, not a rotated walk: `HumanWalkPerformance.Options.lateralStepClose` (default-inert) authors
lead-reach / trail-close foot paths in the travel frame with two double-support phases, re-timed sink/sway tables.
Hard geometry: max stance width = min width + speed × cycle, so 0.8 s at 0.8 m/s needed a 1.0 m stance and the solve
diverged; 0.6 s at 0.70 m/s (0.42 m steps, 0.34–0.76 m stance) is the answer. Pelvis 38° / chest 12°; blade 4.9°.
Gameplay lateral tax 0.8 → 1.04 / 0.92 m/s → playback 1.49× / 1.31× (clamp 1.5). Trap: `Samp` tables must ascend in
phase — an unsorted re-timed table extrapolated +25 m of sink. Nothing installed.

Verified from the **serialized** controller file, not from memory (installed 2026-09-03,
`Reports/CombatWalk_Fresh02_v012_Install.md`):

```
Light_Walk8  Forward = Sword1H_WalkForward_v012  @ ts 1.00, cycleOffset 0.92   (= Fresh02 curves, fully baked in-place)
Travel_Walk8 Forward = Sword1H_WalkForward_v008  @ ts 0.50
Player.prefab CombatWalkSpeedScale = 0.65 · CharacterAnimator.GuardGait = Assets/Core/Animations/GuardGait_Knight1H.asset (0° = 1.34)
Player.prefab FootIK Solver: 0 (AnimatedBones, explicitly serialized) · EquipmentManager autoFitGrip: 0
Light_Walk8 child 7 (Strafe45Left) parked at (0,0) until the 8-way family
no __* research clip referenced
```

md5: v008 `7a7ca074`, v009 `5546958e`, v010 `7c44dd1d`, v011 `e3705c2f`, **v012 `530b8133`** (102/102 curves
key-identical to Fresh02 `a1630664`; world pose ≤ 1.1 mm / 0.00° at 480 phases). Guarded by test `U1`.

`Sword1H_WalkForward_v011` is permanently unreproducible — it predates the canonical-frame rule and
was generated from a placement that no longer exists.

## Current technical reference — `__seqRepair_c40`

**BEST CURRENT TECHNICAL REFERENCE. NOT production-approved.** Not installed. Both of the following
are authoritative:

| | v011 (production) | `__seqRepair_c40` |
|---|---|---|
| clavicle mean / max | 18.8 / 20.6° | **6.6 / 9.0°** |
| Right Shoulder Down-Up | −1.000 (100% saturated) | **−0.396 … −0.154 (0%)** |
| sword path | 167 mm | 200 mm |
| lateral / fore-aft | 9 / 7 mm | 13 / 12 mm |
| blade range | 12.9° | **5.8°** |
| lower-body isolation | — | 4.4 mm vs canonical baseline |

And: **human review says the overall walk still reads visibly procedural and is below the desired
hand-authored quality.** Do not resolve that tension by quoting the table.

## Known false diagnoses — do not re-derive these

1. **"The arm solver saturates the clavicle in the pipeline."** Wrong, and believed for three
   milestones. The priced solver converged correctly *inside production* (hand 3.1 mm, clavicle
   0.8°, shoulder +0.072). An unguarded final `SolveWeaponPass` at S7 then overwrote it (103 mm,
   18.2°, −0.972). **The general lesson: when a subsystem appears to fail, trace the final writer
   of the affected channels before redesigning the subsystem.**
2. **"A mass-frame feedback loop drives the saturation."** Refuted by measurement: `bodyPos.y`
   unchanged to four decimals while the clavicle moved 17.4°.
3. **"Re-pin corrupts the arm."** Refuted: S6 leaves every arm figure bit-identical.
4. **"The target moves during the solve."** Refuted: the target is stable; a later stage drove to a
   *different* target.
5. **"The correction magnitude drives the saturation."** Refuted: it saturated identically at
   13.7 mm, 51 mm and 89 mm.
6. **"The idle carry is wrongly transplanted onto the walking torso."** Refuted: with the weapon
   solve genuinely off, the arm sits **0.4°** from the approved carry on the walking torso.
7. **Two milestones of "pre-solve" numbers (83–91 mm, 18.9°) were measured on post-solve hands.**
   Enabling an arm architecture *looks* like disabling the root-space solve and does not.
   `WeaponSolveExecutionTrace` now makes that fail loudly.
8. **Component-wise quaternion Fourier fitting** blows up at the cyclic seam (chest yaw 357°). Use
   the log map. Band-limiting the *body* rotation diverges entirely — left off.
9. **md5-comparing differently-named assets** reports false differences. Compare curve content.

## The sword/shoulder frontier — a real tradeoff, not a defect

Root-space target, rising clavicle price. The solver does exactly what it is told at every point:

| clavicle price | clavicle | shoulder | hand off root anchor |
|---|---|---|---|
| 1 | 19.2° | −0.951 | 16.9 mm |
| 20 | 13.6° | −0.703 | 30.7 mm |
| 40 | 6.6° | −0.396 | ~45 mm ← `__seqRepair_c40` |
| 60 | 4.4° | −0.092 | 57.0 mm |
| 1000 | 0.4° | +0.096 | 68.6 mm |

Anchoring the sword in root space while the torso translates under it **requires** shoulder
depression. Free the clavicle completely and the sword pumps (711–750 mm path, worse than the
already-rejected `__sw25`). Choose a point on the curve; there is no way around it.

## Important Unity / Humanoid facts

The nine hard-won facts are in SKILL.md and `references/animation-principles.md`. The two that
cost the most: `HumanPose.bodyPosition` is the mass-weighted body frame, not the pelvis; and this
rig maps `Hips → Root`, so the Hips *bone* is not the pelvis either.

Rig: right leg **17.3 mm shorter** than the left (true asymmetry — the marginal chain absorbs
conditioning error, which is why artefacts land there). `humanScale` 0.8312, arm chain 582 mm,
player ~1.59 m. World scale is LOCKED; never resize.

MCP `execute_code`: no `using` directives, CodeDom C# 6, `AssetDatabase.DeleteAsset` blocked
(delete from the shell then refresh), and `refresh_unity` reliably returns "Connection closed
before reading expected bytes" while having succeeded.

## Runtime motion transport — measured on the Player (2026-09-02)

Fresh02 (`__freshHumanForward_02`, human-selected) run through the real controller, three Play Mode
sessions. Full numbers: `Reports/CombatWalk_Fresh02_Transport.md`. What is settled:

- **The Animator reproduces the pose exactly**: FootIK off, every bone within 3.4 mm / 0.9° of the
  RAW clip at 12 fixed phases; child phase = `frac(normalizedTime + cycleOffset)`.
- **Timing**: `MotionSpeed = Speed / SampleAngular(WalkLightRef)` (`CharacterAnimator.Update`) rates
  the guard walk at 1.796 m/s (mocap table) while Fresh02 travels 1.34 natively → 0.639× at the
  armour-loaded 1.148 m/s body speed → 1.27 s cycle / 94 spm instead of 0.825 s / 145.5. Nothing
  else scales time (Animator.speed 1, state speed 1, child timeScale 1; a 6.6 % strafe blend from a
  5.7° lock-target offset stretches the tree length 1.7 %).
- **Pelvis**: Fresh02 alone ships `m_LoopBlendPositionY: 0` (`HumanWalkPerformance.cs:360` does not
  bake Y; `HumanoidWalkGenerator` does — v008/v011 are baked). The 60.3 mm `RootT.y` rhythm becomes
  root-motion `deltaPosition.y` (59.9 mm/cycle measured) and `CharacterAnimator.OnAnimatorMove` drops
  it; the 11.7 mm pelvis motion inside the pose survives every later stage within 1.6 mm.
- **1.145 m/s is the design speed with Warrior Base**: 2.0 × 0.65 × LoadSpeedMultiplier 0.884
  (TotalWeight 15.5 / 24). 1.30 is unarmoured.
- **FootIK on goal-less clips** (Fresh02, v008, v011 have no `LeftFootT/Q` curves) hoists the *stance*
  foot 100–170 mm with the knee up 150 mm at goal weight 0.6–0.77 — the largest pose deviation in the
  whole chain. Not tuned; needs its own milestone.
- Corrected false claim: "every generated clip is unbaked" — only Fresh02 is.

### Transport repair (2026-09-02, `Reports/CombatWalk_Fresh02_TransportRepair.md`) — QUALIFIED

`__freshHumanForward_02_Transport` = Fresh02 with only `m_LoopBlendPositionY: 1` (102/102 curves
identical). On the real Player, FootIK off: pelvis **69.1 mm** at every stage; unarmoured 1.300 m/s →
**0.970× / 145.5 spm**; Warrior Base 1.149 m/s → **0.857× / 128.6 spm** (load response, stride-matched);
real lock-on target 5.7° off → 0.824× (−2.3 % rating interpolation, −1.7 % tree-length averaging).
Mechanism: `GuardGaitReference` ScriptableObject on `CharacterAnimator.GuardGait` (per-heading native
speeds; research asset `Generated/GuardGait_Fresh02.asset`, forward 1.34). Production prefab still has
it null → legacy table + one warning. `HumanWalkPerformance.ApplyInPlaceClipSettings` now bakes Y.
Runtime preview is an in-memory `AnimatorOverrideController` (RuntimeQaDriver `RTQA_override`) — the
controller file is no longer touched for previews.

**Erratum:** `GameplayCameraPreview` doubled the vertical rhythm of every RAW review video before this
date (it re-added RootT.y on top of SampleAnimation's own root move). Fixed; the numeric 69 mm authority
was never affected, but earlier visual approvals saw ~2× bob.

Still open: the constant 125 mm runtime sword re-seat in the hand (grip / attachment, present on
every clip, not transport).

### FootIK goal contract (2026-09-03, `Reports/CombatWalk_Fresh02_FootIK.md`) — REPAIRED

Root cause, measured: on a goal-less clip (every generated clip, v008/v011 included)
`Animator.GetIKPosition` is a parked point at the root (~86 mm above the body centre, pose-invariant);
FootIK's fallback rebuilt a goal from the ankle bone read inside `OnAnimatorIK` (stale AND already
IK-corrected), un-corrected it by its own previous offset (self-feeding estimator), and anchored it
in ROOT space while weighted (a planted foot's goal rides forward with the body). Fixed point:
pelvis −88…−169 mm, feet 200–500 mm off, toes up to 148 mm under the floor — **production v011/v008
had this too.**

Repair: `FootIK.Mode.AnimatedBones` (new default, LateUpdate −50): read the authored feet from the
bones the Animator just wrote, probe from them (foot forward = ankle→toe bone), solve the legs on the
transforms (`SolveTwoBone`), keep the authored foot world rotation. Flat ground, Fresh02: pelvis
settle ≤ 4 mm, ankles ≤ 15 mm, toe penetration gone (−12 → +20 mm), rhythm 69.7, no new jitter;
v011 ≤ 8 mm feet; 6° ramp adapts with ≤ 17 mm pelvis settle. Imported mocap clips now get
bone-based correction instead of being pulled 80–140 mm toward the source-rig goal (behaviour
change, read-only measured). An animated-foot baseline is impossible inside `OnAnimatorIK` — the
bones there are last frame's corrected pose; the estimator's pole equals the IK weight.

Harness: `RtqaQueue` (EditorPrefs `RTQA_queue`, `|`-separated `key=value` lists; float keys
`RTQA_side/ramp/footik`) runs Play sessions back to back; `RTQA_ramp=<deg>` builds a 4 m up / 4 m
down ramp on the lane. Prefab trap: a NEW serialized field on a component keeps the default that was
current at the prefab's last import — reimport `Player.prefab` after changing a field default.

### Final runtime QA (2026-09-03, `Reports/CombatWalk_Fresh02_FinalRuntimeQA.md`)

Whole stack on, real Player: Fresh02 → final pose within 3.4 mm pelvis, 5.3 head, 4.7 clavicle
(0.0°), 4.2 hand (0.6°), ankles 15.6 / 4.7, sword 9 mm / 1.7°; rhythm 69.7; idle↔forward transitions
clean (steps = steady walk). Blockers / dependencies found:
- **Grip**: the 125 mm sword re-seat is `EquipmentManager` palm auto-fit reacting to the **gameplay
  starting loadout's Militia torso** (palm centroid 72 mm off the Warrior torso's). Warrior Base = the
  prefab's authored grip (≤ 8 mm, correct). Militia = hilt at the elbow → GRIP ATTACHMENT DEFECT
  (equipment domain; `autoFitGrip=0` or a Militia grip offset).
- **Lock-on asymmetry**: `Light_Walk8` has the Strafe45Left child at 9.3° left of forward; a target
  5.7° to the RIGHT hands 61 % of the walk to the old strafe (left target: 6.6 %). 8-way family job.
- **Load**: swapper Warrior Base weighs 9 (1.212 m/s, 0.905×); the inventory's starting loadout
  (Militia torso+helmet + Warrior legs) weighs 15.5 (1.149, 0.857×). Stride matching exact either way.
- Production family with `AnimatedBones`: step metrics equal to FootIK-off in every direction; the
  only signature is a 44–47 mm pelvis settle on hovering mocap strafes. Hand/sword pops in the
  forward-left blend and back→forward flips pre-exist (DirectionSmoothTime 0.06 s crossfades).
- Harness traps: the debug swapper listens to digit keys (a stray `1` re-equips mid-run); queue runner
  scripts use `;`; the editor's delayCall chain only ticks when pumped (call anything via MCP).

### Forward blockers cleared (2026-09-03, `Reports/CombatWalk_Fresh02_ForwardBlockers.md`)

- **Grip**: `EquipmentManager.autoFitGrip` measured each torso's hand-weighted vertex centroid (Militia's is
  a 41 mm cuff 72 mm from the bone) and re-seated the sword by mesh differences. Repair: `autoFitGrip: 0`
  on the Player (all 13 items share the 45-bone rig and `R_Hand/Sword`), Militia's stale item offset
  zeroed; Militia V2 keeps its real per-item tune. Gameplay loadout now 12 mm / 1.6° from the authored
  grip. `EquipmentDebugSwapper` hotkeys compiled out of non-dev builds.
- **Light_Walk8**: every mocap child sits on its measured travel heading, all rotated +35–42° by the
  `orientationOffsetY −35` square-up import; the generated forward at a true 0° left `Strafe45Left` 9.3°
  away and an 86° hole on the right. Repair: child 7 parked at the origin (zero weight for unit inputs);
  Fresh02 now 0.904 at 5° right (was 0.462), 0.804 at 10° (was 0), left side unchanged. Proper coverage
  still needs the 8-way family (rotate or re-author the mocap ring).
- Harness: queued sessions chain inside Play Mode via scene reload (`PopQueueIntoPrefs`), `RTQA_items`
  equips an explicit loadout after Start, `RTQA_swapperOff`, `RTQA_noswap`.

## Runtime IK — the Player is not what it looks like

**No right-hand IK exists.** `OffHandPose` is the LEFT arm at `Weight 0`. "Body IK" is
`UpperBodyAim` (LateUpdate spine/head + right-arm tuck ≤25° + elbow guard). `FootIK` writes
`Animator.bodyPosition` directly, pelvis drop to 0.45 m.

**Nothing at runtime rotates the clavicle** — so the clavicle work is real on the player.
Full detail: `references/runtime-ik.md`.

## Experimental code — KEEP / EXPERIMENTAL / DEAD

All experimental switches are 0 in every saved profile and inert at default.

**KEEP (production-default):** canonical frame, semantic support, whole-cycle leg solve, ground
plane, extension reserve, knee ceiling, swing-arc repair, sagittal posture, contact scheduling,
all audits, `AnimationAssetSafety`, `WeaponSolveExecutionTrace`, the S7 guard.

**EXPERIMENTAL (works, off by default, needs a named reason to enable):**
`upperBodyAuthority` (qualified, but only used by the foundation profile) ·
`weaponChainAnatomicalPricing` (the proven anatomy fix; produced `__seqRepair_c40`) ·
`combatArmArchitecture` (converges correctly; pumping sword at low clavicle cost) ·
`weaponFrameTranslationFollow` · `armClavicleCost` / `armElbowCost` / `armWristCost` /
`armUpperArmTwistCost` · `weaponPositionAuthority` / `weaponOrientationAuthority`.

**DEAD (disproven — kept documented so nobody repeats them, but do not enable):**

| | why it is dead |
|---|---|
| `chestSpaceWeaponBlend` | frees the clavicle but hands the sword the chest's whole translation — 648 mm pumping |
| `excludeClavicleFromWeaponChain` | superseded by pricing, which is continuous instead of binary |
| `shoulderAnchor` | a mitigation for saturation whose real cause (the S7 overwrite) is now fixed |
| `weaponPassNoWarmStart` | same — aimed at a symptom |
| `torsoOrientationAuthority` | absolute-orientation torso targeting; never beat the sagittal pass |
| `frontalPostureAuthority` | superseded by the whole-cycle upper-body solve |
| `weaponFrameArchitecture` | produced `__wf75` / `__sw25`, both rejected by human review |

## The recurring `Travel_Walk8` drift — read this before committing a clip

`Travel_Walk8`'s forward slot has now silently drifted onto a research clip **four times**, the
most recent during the consolidation milestone itself. It is invisible in the editor: the walk
plays, it just plays the wrong clip. `P1` is what catches it.

Root cause still unproven, but the evidence narrowed sharply this time:

- `HumanoidWalkGenerator.Commit` calls `AssetDatabase.SaveAssets()`, which flushes **whatever the
  editor already had dirty in memory** — the controller included. The bad write is not authored by
  this code; committing a clip is what makes it reach disk.
- The one slot that drifts is the one that is structurally different. `Travel_Walk8` forward is a
  **`type: 2`** reference (a standalone `.anim`); almost every other motion in the controller is
  `type: 3` (an FBX sub-asset). Only three `type: 2` motion references exist in the whole
  controller, and committing a new standalone `.anim` is exactly the event that precedes a drift.
- Timing trap that cost time here: a check run *immediately* after generating can still read
  correct, because the flush lands later. **Verify from the serialized file, and verify again at
  the end of the milestone.**

`Commit` now calls `AnimationAssetSafety.VerifyProduction` and logs a loud error if a research clip
was installed. That removes the silence; it does not fix the cause. Restoring is a one-line edit of
the `m_Motion` guid at the `Travel_Walk8` child back to v008 `12f2b3e78266548d6a5fc831c012cafa`.

Reference guids: v011 `f11e74bd045df43c4a7d7c5a5417381e` (Light forward),
v008 `12f2b3e78266548d6a5fc831c012cafa` (Travel forward).

## Technical debt

Broken PPtr in `Knight_Controller`, local id `3908002699880397750`. Present in git HEAD and every
backup; predates all of this work. `P2` scopes it out with `LogAssert.ignoreFailingMessages`.
Untouched by instruction.

## Test suite

**EditMode, 30 tests.** A–H (idle), J–K (interpolation, B-spline), L–M (support, swing arc),
N (controller invariants), O1–O4 (determinism, placement invariance, state restoration, upper-body
band limit), P1–P2 (temp-asset safety), Q1–Q2 (weapon-pass sequencing, isolation self-check).

Q1 asserts *which pass ran*, not what the output looked like — the bug it guards produces output
indistinguishable from a solver that failed.

## Research clips kept (none referenced by production)

`__seqRepair_c40` (the reference) · `__repaired_f25` (freed shoulder, pumping sword — the other end
of the frontier) · `__carryK1` · `__wf75` · `__sw25` · `__wc_cp12` ·
`__Sword1H_WalkForward_CanonicalProbe` (the frozen determinism fixture — **do not delete**).
`__freshHumanForward_02` (the v012 source, `a1630664`) · `__freshHumanForward_02_Transport` (Y-baked
research copy) · `__freshHumanForwardLeft_01` (ForwardLeft −45° challenger, ACCEPTED foundation) · `__freshHumanForwardLeft_02`
(its body-orientation refinement, review pending — see below).

## ForwardLeft research (2026-09-03) — `__freshHumanForwardLeft_01`, HUMAN REVIEW PENDING

`Reports/CombatWalk_ForwardLeft01.md`. Built from the Fresh02 dials with the new diagonal options on
`HumanWalkPerformance.Options` (`travelDeg −45, yawTowardTravelDeg −18.5, chestFollowFrac −0.195,
headYawDeg +8, strideScale 0.94, stanceWidenMm 6`). Measured: ground track −46/−44°, native 1.241 m/s,
pelvis 36° toward travel / chest 9° / head 2° off the threat, feet 158–175 mm apart, 0 legality
violations, temporal texture = v012. Production untouched (Light_Walk8 = v012, Travel = v008, no
ForwardLeft installed; GuardGait entry `{−45, 1.241}` measured but NOT installed).

Three traps met on the way, each costing a build — do not re-derive:
- rotating the Forward foot lines about the root crosses the inner foot (31–103 mm) on a diagonal; the
  lines go under each yawed hip (±h cos d, ±h sin d, d = travel − yaw);
- RootQ is the mass-weighted body frame: −28° RootQ + 22° chest counter-twist = pelvis −45°, chest −18°
  (≈0.44/0.56). Solve the yaw dials from one measurement;
- orientation / XZ left as root motion made Edit-Mode sampling turn the root by the pelvis yaw (track
  read −17°). `ApplyInPlaceClipSettings` now bakes orientation, Y and XZ, Based Upon Original (= v012).
Probe artefacts: the knee-angle probe reads 4.1/4.2° on v012 too; phase-window stance probes include
heel-off. Run every probe on v012 first.

**ForwardLeft01 ACCEPTED as the motion foundation (human review 2026-09-03)** — lower body approved, do not
reopen. `__freshHumanForwardLeft_02` (`Reports/CombatWalk_ForwardLeft02.md`) is the one surgical refinement:
pelvis lead 36° → 32°, chest 10°, pelvis-to-shoulder separation 27° → 22°, by lowering the chest counter-twist
(yaw −17.7, chestFollowFrac 0.034, headYawDeg +8) — the micro-study showed hips ≈ 1.0·yaw + 0.78·twist,
shoulders ≈ 0.75·yaw − 0.22·twist. Feet / stride / rhythm / sword numerically unchanged. Blind A/B:
`Artifacts/AnimationReview/ForwardLeft_01_vs_02_sideBySide_BLIND.mp4` (+ `_KEY.txt`). Tests V1–V3 added
(fully baked in-place root contract = v012 + determinism; root canonical at local identity during sampling —
`SampleAnimation` writes the Animator's local transform absolutely, v012 included; explicit head yaw touches only
the neck, legs/RootT within the mass-weighted-body-frame coupling of 8e-4 / 1.7e-6). Human review pending; no
ForwardLeft installed.

## NEXT RECOMMENDED EXPERIMENT

**Do not spend another milestone polishing the current Forward source mathematically.** The
remaining defect is not mathematical. Twelve milestones of evidence say the solver architecture is
not the limiting factor.

Instead: A/B a genuinely different high-quality motion source. Evaluate **RAW** first, then
**MINIMALLY ADAPTED**, both against `__seqRepair_c40`. Follow
`references/external-motion-intake.md` — read-only audit first, correction authorities off by
default, motion preservation budget reported.

The question to answer:

> Can a better source supply the human motion craft — weight, overlap, inertia, asymmetry, natural
> joint rhythm — while this skill preserves Bravehood's technical and gameplay constraints?

A challenger wins only on all three of: keeps Bravehood combat identity · human review clearly
prefers its motion · technical gates remain acceptable.

## TRUE LEFT gameplay-speed study (2026-09-03)
Principle: believable leg motion > fast playback; movement and playback change together (stride-matched presentation).
`__freshHumanLeft_01` kept unchanged; tested on the real Player at 0.72×/0.51 m/s, 0.87×/0.61 m/s, 1.01×/0.71 m/s
and the old fast 1.27×/0.89 m/s. SELECTED 0.874× / 0.612 m/s / 175 spm. Gait KEPT (no Left02). Report
`Reports/CombatWalk_Left01_GameplaySpeed.md`. Harness: `RTQA_speedMul` (StatusSpeedMultiplier), `RTQA_laneZ`;
research `Generated/GuardGait_Left01Study.asset` (−97.1° = 0.70) makes MotionSpeed stride-match for any speed.
INSTALLED 2026-09-03: `Sword1H_WalkLeft_v001` in Light_Walk8 child 5 at (−1,0) ts1 co0.35; GuardGait −90:0.70; Player StrafeSpeedMultiplier 0.53 (−90° = 0.606 m/s @ 0.866×; ForwardLeft now 0.987 m/s @ 0.796×). BackLeft not started. Rate-independent notes: right knee 1.9° ~14 % of moving time, stance ankle drift 15–23 mm.

## Left v001 productionized (2026-09-04) — see Reports/CombatWalk_Left_v001_Install.md
Approved research Left01 → `Sword1H_WalkLeft_v001` (key-identical, X1). Light_Walk8 child 5 (−1,0) ts1 co0.35 (was mocap −97.1°).
GuardGait −90:0.70 (measured 0.694). Player StrafeSpeedMultiplier 0.53. 45/45. VerifyProduction TRUE.
BLOCKER outside the pipeline: Player.prefab MaxStableMoveSpeed 2→1.2 / MaxSprintSpeed 4→3 arrived from an in-game session and
were committed in eb4f9c73; at 1.2 every guard-walk heading clamps at MotionSpeed 0.6 and skates. Qualification runs reproduced
the 2.0 baseline via RTQA_speedMul=1.667. RESOLVED 2026-09-04: human restored MaxStable 2 / Sprint 4 (accidental change). Real-prefab re-qualification: Forward 1.144 m/s @0.854x, ForwardLeft 0.987 @0.796x, Left 0.606 @0.866x, none at the 0.6 floor, stride error <0.3 mm/s. SWORD1H WALK LEFT v001 PRODUCTION QUALIFIED. BackLeft not started.

## BackLeft01 research (2026-09-04) — Reports/CombatWalk_BackLeft01.md
`__freshHumanBackLeft_01`: step-close retreat (lateralStepClose, travel −135, drift 250, cycle 0.65, lead 300 / trail −20,
yaw −20, head +12, armGain 0.25, reachMargin 20/8) + NEW inert-default options `lateralStrikeToesDownDeg 8`,
`lateralSoleLoadFrac 0.12`, `lateralSwingToesUpDeg 2`, `lateralWorldPitch` (world-pitch solve for the lateral gait: the
trail foot on a flexed knee ahead of the hip inherited the shin lean — toes down 22–31°, toe buried 32 mm — fixed).
Native 0.592 m/s. Speed study on the real Player: SLOW 0.76×/0.45, MEDIUM 0.90×/0.535 (my pick), BRISK 1.06×/0.63,
current gameplay 1.13×/0.67. Not installed; human review pending. Generator fix: `stanceCapSide` now cleared with
`legLimitSide` (a default build after a margin build inherited caps → 0.48 units on Left01's recipe).
Player.prefab MaxStableMoveSpeed was found rewritten 2→1 again at 02:35 (no script writes it); restored to 2.

## BackLeft v001 productionized (2026-09-04) — Reports/CombatWalk_BackLeft_v001_Install.md
`Sword1H_WalkBackLeft_v001` (key-identical promotion) in Light_Walk8 child 4 at (−0.7071,−0.7071) ts1 co0.35 (= Left's; both
step-close clips share the support schedule). GuardGait −135:0.592 native. NEW per-heading gameplay scale:
`GuardGaitReference.Entry.gameplayScale` (0:1.0, −45:0.862, −90:0.53, −135:0.468, mocap headings = legacy formula) sampled by
CharacterAnimator at `MoveInputHeadingDeg` → `BaseCharacterController.DirectionalSpeedScale/Blend` (lerped by stance over the
strafe/backpedal interpolation). Real prefab: BackLeft 0.535 m/s @0.904× 167 spm loadout, 0.608 @1.028× unarmoured; F/FL/L
unchanged. Tests Y1 + Y2 (47). Watch item: left support-yaw drift ~17° (Left01 14°). Production family: 0 / −45 / −90 / −135.
TRAP: an external editor session keeps saving combat clips (and the research BackLeft01) into Travel_Walk8's left children;
restored to HEAD each time — Travel is NOT to be edited from this pipeline; ask the user.

## Left02 research (2026-09-04) — Reports/CombatWalk_Left02.md
`__freshHumanLeft_02`: FULL ALTERNATING lateral cycle (`lateralAlternating`, laneOut 70, drift 230, swingFrac 0.42,
cycle 0.8, analytic sink/sway/yaw/roll, lateralWorldPitch, reachMargin 20/8). No crossover (R−L ≥ 109 mm), separation
190–530, knees ≥37°, planted drift 0–3 mm at runtime. Native 0.496 m/s / 150 spm. Study: 0.80×/0.40, 0.95×/0.47,
**1.10×/0.546 (pick)**, 1.22×/0.606. Blind pair vs Left v001 (key in scratchpad l2/blind_key.txt). Not installed.
TRAP: test_general.unity PlayKit instance overrides Player MaxStableMoveSpeed = 1 (saved) → every Play run at 1 m/s
(harness compensates with RTQA_speedMul ×2). Controller cycle offsets were zeroed by an external save → restored from
HEAD + Travel install re-applied (Travel_Walk8: v012_Travel / FL / L / BL _Travel copies at true angles, co 0.92/0.92/0.35/0.35).

## Left03 research (2026-09-04) — Reports/CombatWalk_Left03.md
Left02 lower body + yawTowardTravelDeg −13 / head +7: authored hip 25 / shoulders 6.8 / head −1; runtime rel-root pelvis
+19 / shoulders +1 (Left02 +32 / +6); feet within 12.6 mm of Left02, no crossover (≥116 mm), separation ≥214. Idle blades
−29 / −34 by the same measure (family-wide idle→walk swing). Scene override MaxStable=1 reverted + scene saved.
Unattended Play sessions appear right after a queue finishes (driver exits Play at the end; something re-enters it — not isolated). Check/exit Play before asset writes.

## Left v002 PRODUCTION (2026-09-04) — Reports/CombatWalk_Left_v002_Install.md
Approved Left03 promoted to `Sword1H_WalkLeft_v002` (key-identical); Light_Walk8 child 5 (−1,0) ts1 **co 0.34** (measured:
left foot lands at tree time 0.068 vs ForwardLeft 0.07 / BackLeft 0.04). GuardGait −90 = **0.496** native, gameplayScale
**0.477** → loadout 0.546 m/s @ 1.100× / 165 spm (unarmoured 0.620 @ 1.25×). Runtime rel-root pelvis +19.5°, shoulders +1.1°,
root 354.29 constant. Idle→Left pelvis yaw peak 387°/s (spread over the crossfade, no pop) — IDLE / LOCOMOTION
BODY-FACING TRANSITION WATCH ITEM (idle blades −34.5 / −39.4). Tests 48/48 (X1 → v002, Y1 tolerance 0.02, Y2 0.477).
Travel_Walk8 still carries Left_v001_Travel (rated 0.70/0.53) — not promoted. Left_v001 retired from Light.

## Back01 research (2026-09-04) — Reports/CombatWalk_Back01.md
`__freshHumanBack_01`: alternating backward walk (`lateralAlternating`, travel 180, drift 220, cycle 0.82, swingFrac 0.42,
laneOut 40, stanceWiden −15 (sign flips under the 180° rotation: negative WIDENS), sway 25 across travel via new
`lateralSwayAcrossTravel`, strike 6 ball-first (ankle lift now also in the alternating target, gated), yaw +8 (bladed
right like the idle), head −5, armGain 0.25). Native 0.463 m/s / 146 spm, lanes ±104 (gap 208), boots −13/−6.
Runtime rel-root pelvis −14.9 / shoulders −11.7 / root constant. Study: 0.85×/0.39, **1.00×/0.463 (pick)**, 1.15×/0.53,
current 1.42×/0.66. Likely install: co ≈ 0.88 (left lands 0.92 → tree 0.04 = BackLeft), gameplayScale ≈ 0.405 at ±180.
Study-rig trap: overriding into the 170.2° ring slot gives a 9.8° sideways slide (0.079 m/s) — not the clip.
Not installed. Family videos + BackLeft→Back context recorded.

## Back v001 PRODUCTION (2026-09-04) — Reports/CombatWalk_Back_v001_Install.md
Approved Back01 promoted to `Sword1H_WalkBack_v001` (key-identical); Light_Walk8 child 3 (0,−1) ts1 **co 0.87** (both
landings coincide with BackLeft's tree time 0.025 / 0.525). GuardGait 180 = **0.463** native, gameplayScale **0.405** →
loadout 0.463 m/s @ 1.001× / 146 spm (unarmoured 0.526 @ 1.137×). Wrap ±179 continuous (velocity step 2.5 mm/s).
Runtime rel-root pelvis −15.0 / shoulders −11.7; idle→back change +19.5 / +27.7 (smallest in family), pelvis yaw peak
442°/s in the crossfade, right foot repositions at up to 3.95 m/s for ~4 frames at entry (watch item, not a pop).
Tests 50/50 (AA1 contract, AA2 wrap). Family now: F v012 / FL v001 / L v002 / BL v001 / B v001; right side mocap.
Travel untouched. Nothing committed.

## BackRight01 research (2026-09-04) — Reports/CombatWalk_BackRight01.md
`__freshHumanBackRight_01`: alternating diagonal retreat (travel +135, drift 220, cycle 0.82, laneOut 40, stanceWiden −40
(negative widens under the rotation), sway 25 across, strike 6, yaw +12 (bladed right), head −7, armGain 0.25). Chosen
over the step-close mirror of BackLeft (wide lunges, pelvis parks). Native 0.463 / 146 spm; lanes 178 mm apart, separation
183–457; runtime rel-root pelvis −21.5 / shoulders −14.1. Study: 0.85×/0.39, **1.00×/0.463 (pick)**, 1.15×/0.53, current
1.41×/0.65. Contacts identical to Back (R 0.395 / L 0.895) → likely co 0.87, scale 0.405. Not installed.

## BackRight v001 PRODUCTION (2026-09-04) — Reports/CombatWalk_BackRight_v001_Install.md
Approved BackRight01 promoted to `Sword1H_WalkBackRight_v001` (key-identical); Light_Walk8 child 2 (0.7071,−0.7071) ts1
**co 0.87** (contacts identical to Back). GuardGait 135 = **0.463** native, gameplayScale **0.405** → loadout 0.463 m/s
@ 1.001× / 146 spm (unarmoured 0.526 @ 1.137×). Seam +179 now 0.463 toward BackRight (MotionSpeed step 0.013).
Runtime rel-root pelvis −21.5 / shoulders −14.1; idle→backright change +13.0 / +25.3 (smallest pelvis change in family),
peak yaw 108°/s; right foot repositions ≤ 4.0 m/s at entry (watch item). Tests 51/51 (AB1). Family: F v012 / FL v001 /
L v002 / BL v001 / B v001 / BR v001; remaining legacy 86.3° strafe + parked child. Travel untouched. Nothing committed.

## Right01 research (2026-09-05) — Reports/CombatWalk_Right01.md
`__freshHumanRight_01`: Left v002 architecture at +90 with three sword-side differences: stagger −110 (LEFT foot stays
forward, right leads laterally as the rear foot), yaw +8 / head −5 (blade, not opening; runtime pelvis −15 / shoulders −12),
and NEW inert-default `lateralMirror` (along-travel sway + yaw swing phased to the lead lane; without it the pelvis swayed
away from the loaded foot and paused at 0.016 m/s). Native 0.496 / 150 spm; lanes 119–517, separation 273–572; right boot
heading −0.1° with 5° support drift (best in family). Study 0.85/1.0/1.10 (pick = Left's 0.546 m/s, 165 spm)/1.15/1.29.
Contacts L 0.90 / R 0.40 → likely co 0.87 (= BackRight), scale 0.477. Not installed. Trap: TEMPORAL flags a right-ankle
hold/release (9° @ 108°/s) from the flat stance pitch target — not visible.

## Right v001 PRODUCTION (2026-09-05) — Reports/CombatWalk_Right_v001_Install.md
Approved Right01 promoted to `Sword1H_WalkRight_v001` (key-identical); Light_Walk8 child 1 (1,0) ts1 **co 0.87** (Δ 0.005
vs BackRight, 0.030 vs Left). GuardGait 90 = **0.496** native, gameplayScale **0.477** (= Left) → loadout 0.546 m/s @
1.100× / 165 spm (unarmoured 0.620 @ 1.250×). Runtime rel-root pelvis −14.7 / shoulders −11.6; idle→right +19.8 / +27.8,
peak 172°/s, entry foot ≤ 2.37 m/s. Planted drift 0 on the true slot; right boot pitch ≤ 0.7°/frame (no snap).
Tests 52/52 (AC1; U1 updated: the 86.3° mocap rating assertion became the +90 authored rating). Family: F / R / BR / B / BL / L / FL installed; ONLY ForwardRight (+45) missing — that sector now
interpolates Forward↔Right (no active child). Travel untouched. Nothing committed. SCRATCHPAD WAS WIPED between sessions
(analysis scripts lost; `transport/right_qa.py` recreated).

## ForwardRight01 research (2026-09-06) — Reports/CombatWalk_ForwardRight01.md
`__freshHumanForwardRight_01`: Forward v012 architecture rotated to +45 as its own gait (not a ForwardLeft mirror). The
+45 advance NEEDS the sword-side blade: yaw 0 / 8 bury the right toe 37 mm (shorter right leg reaching forward-right from a
square hip); yaw 11 is the smallest blade that plants it flat (bracketed 14 / 18, all clean). Recipe: minKnee 15, stanceKnee
32, swingHeight 0.62, armGain 0.4, stride 0.90, widen 15, chestFollow 0.034, head −5, plantedFootRoll, reach 20 + 8,
**stanceYawStabilize 1, yawReleaseSpan 0.16**. Native 1.208 / 150 spm; lanes ±88 mm, separation 165–604; runtime pelvis
−19.3 / shoulders −13.7 (Forward −3.6 / −7.3, Right −14.7 / −11.6); support yaw drift 0.3°, planted drift 2–5 mm.
Study 0.80 / 0.85 / 0.90 + **pick 0.816× / 0.986 m/s / 122 spm = ForwardLeft's presentation (scale 0.862)**; current +45
gameplay = skating Forward/Right blend 0.845 m/s (no planted foot). Runtime tree-time landings: family L ≈ 0.06–0.12 /
R ≈ 0.55–0.63 → likely **co 0.95**. Not installed; future slot = parked child 7 → (0.7071, 0.7071).
GENERATOR REPAIRS (inert at default): Jacobian probe at a saturated muscle gave NaN (probe toward room, skip non-finite
steps); yaw-hold reference now resolved in a pre-pass (the right stance wraps the seam — in-loop resolution left
cycle 0.0–0.16 unheld → 0.35 twist snap); hold release ends at TOE lift (holding a dangling foot twisted the thigh at
6800°/s; ankle lift is only heel-off). TRAPS: a leaked `HideAndDontSave` build holder (`__fr/Player`) hijacked the RTQA
driver AND the inventory bootstrap (loadout 0.9325 instead of 0.880) — queues now STOP on a Player outside the loaded
scene; lanes (24,−8) and (24,0) are not flat/clear for +45, use (30,−11); a clip-side toe-threshold landing detector
mislabels v012's heel strike — use runtime foot-bone landings in tree time.

## ForwardRight v001 PRODUCTION (2026-09-07) — Reports/CombatWalk_ForwardRight_v001_Install.md — LIGHT_WALK8 8/8
Approved ForwardRight01 promoted to `Sword1H_WalkForwardRight_v001` (key-identical, guid 46854fcc…); the parked origin
child 7 (Strafe45LeftLoop) replaced IN PLACE → (0.7071,0.7071) ts1 **co 0.94** (measured: install-convention landings
L 0.023 / R 0.505 → tree 0.083 / 0.565, smallest worst-case Δ vs Forward 0.088/0.623 and Right 0.030/0.530). GuardGait
**45 = 1.208 native / gameplayScale 0.862 (= −45)** → loadout 0.986 m/s @ 0.816× / 122 spm (unarmoured 1.121 @ 0.928×).
Runtime pelvis −19.3 / shoulders −13.8; idle→FR +15.2 / +25.6, peak 180°/s, entry foot 3.51 m/s. Sweeps 0→45→90
monotonic, no third child; 45–90 mid-sector plants 5–19 % (2.2× native ratio — FAMILY-WIDE cleanup item, not a
blocker). Tests 54/54 (AD1 contract, AD2 all-eight-headings). FAMILY COMPLETE: F v012 / FR v001 / R v001 / BR v001 /
B v001 / BL v001 / L v002 / FL v001; no legacy motion in Light_Walk8; Travel untouched; nothing committed.
Watch items for the family review: ForwardLeft v001 runtime support knees 4.2/1.9° + planted yaw up to 20°; the
45–90 blend-sector slide; the family swing toe plunge; idle/locomotion facing pass now unblocked (all 8 exist).
Lanes: +45 runs from (30,−11); a prop at (30.9,−3.5) and walls at z≈3.4 break long +z sweeps — split sweeps into
≤3 segments or start further back.

## ForwardLeft cleanup Phase 1 research (2026-09-07) — Reports/CombatWalk_ForwardLeft_Cleanup01.md
Production FL v001 (= FL02, not reproducible from dials: rebuild |Δ| 1.05) forensics: support knees 4.2/4.3°, toe buried
−48/−29 mm (32 %), support yaw 30.5/25.8°, stance tracks 1.14 vs 1.28 m/s (within-stance slide), heel-strike +34°,
touchdown −1260 mm/s. Causes: no per-leg reach, no world-pitch plant, no stance-heading hold (FL02 predates all three).
CHALLENGER = existing `__freshHumanForwardLeft_03` (Sep 3 foot-realism repair): 91/102 curves bit-identical to v001
(entire upper body / root / head), only legs + RootT.y (−24 mm) + thigh twist changed. Knees 12.1/23.2°, toe +2 mm,
yaw 0.6/1.7° (runtime 0.1/0.4°), native 1.261 → 0.986 m/s @ 0.782× / 117 spm, facing identical (+26.7/+4.6),
F→FL→L boundaries unchanged. Likely co 0.93 (0.92 within 0.01). No FL04 built. Research gait `GuardGait_FL03Study`.
Not installed. Next after human review: promote FL03 → `Sword1H_WalkForwardLeft_v002` (GuardGait −45 native 1.261,
scale 0.862 unchanged), then the Idle / family-facing pass.

## ForwardLeft v002 PRODUCTION (2026-09-07) — Reports/CombatWalk_ForwardLeft_v002_Install.md
FL03 promoted (CopyAsset, key-identical, guid 94278d5b…) into the −45 child of Light_Walk8, replacing v001's reference
only (position/ts unchanged, **co 0.92 kept** — measured best, worst Δ 0.040 vs Forward/Left). GuardGait −45 native
**1.261** (was 1.24), scale 0.862 unchanged → 0.986 m/s @ **0.782×** / 117 spm (unarmoured 1.121 @ 0.889×). 91/102
curves identical to v001 (legs + RootT.y + thigh twist changed). Runtime knees 12.1/23.2°, planted yaw 0.1/0.4°, toe 33 mm;
facing +26.7/+4.4 unchanged; idle entry 492/316°/s unchanged; F→FL→L boundaries unchanged; family8 sweep clean. v001
kept on disk (its _Travel copy still serves Travel_Walk8; TravelGait −45 stays 1.24). Tests 54/54 (U2 → v002 contract,
seven ring dictionaries + rating asserts updated). Family: F v012 / FR v001 / R v001 / BR v001 / B v001 / BL v001 /
L v002 / FL v002. Nothing committed. NEXT: family-wide Idle / body-facing pass; 45–90 blend-sector slide; swing plunge.

## Family continuity audit (2026-09-07) — Reports/CombatWalk_FamilyContinuityAudit.md — DIAGNOSTIC, nothing changed
Runtime facing map: Idle −29/−34; sword side F +4/−1, FR −14/−8, R −10/−6, BR −16/−8, B −10/−6; left group BL +37/+11,
L +25/+7, FL +32/+10. Idle entries: BL/FL/L class C (+53…65° pelvis at 390–500°/s, "turns to run"); F class B/C (idle
stance 631 mm wide / 475 staggered collapses to 178 mm in 0.13 s, feet 3.6 m/s, 69 mm/frame); BR best (+13°, 106°/s).
Moving: only B↔BL (Δ45°, 220–250°/s) and FL↔F (28–30°) break; ring sweep otherwise ≤ 7°/15°. Crossfade 0.15→0.35 s
halves rates, cannot change the 65° destination. Body-hold diagnostic = torso/legs disconnect (procedural layer NOT
recommended). RECOMMENDATION (F): 1) Idle v006 — narrower stance (~380/250) + blade eased to ~−18/−15; 2) Idle→Loco
0.15→0.22 s; 3) left-group opening reduced to ~+10…+15 (BL first, then L; FL needs a curve-level edit — FL03 base not
rebuildable); 4) procedural only if C entries remain. Blend-space stride items: FR↔R (+55…85) AND L↔FL (−65…80), 5–23 %
planted mid-sector. Research assets: Knight_Controller_XfadeStudy_A/B; harness TransitionDiag (RTQA_diag). Lane trap:
(25,2) hits the wall at z≈3.4 — ring sweeps run from (34,−8).

## CombatIdle v006 research (2026-09-07) — Reports/CombatIdle_v006_Research.md — NOT installed
`__freshHumanCombatIdle_06` (= candidate A): v005 regenerated with the SAME profile from a surgically edited base pose
(`Sword1H_CombatIdle_Pose_v3A_research`): stance 366/270 mm (v005 631/475), hip line −18 / shoulders −15 (−29/−34), boots
−1/+28, knees 35/29, pelvis +36 mm (the price of a narrow base with soft knees), upper-body muscles preserved (hand/sword
in chest space 0.0 mm), head kept on the threat via neck/head turn. Diff vs v005: 60/102 curves identical, legs/torso
twist/neck are constant offsets with breathing shape kept (Δ ≤ 0.009). Reproduction test first: pipeline rebuilds v005
with only right-arm curves ≤ 0.008 off. Micro-study B (402/324, −21/−18) and C (336/240, −15/−12) kept as alternatives.
RUNTIME (06A, production walks, 0.15 s): sword-side entries halve (F +22° 229°/s, FR +4.5°, R +8° feet 1.7 m/s, BR +2°
36°/s, B +8°), exits all gentler, head within 1° of target (v005 sat 10° off). FORWARD FOOT COST UNCHANGED (3.6 m/s,
74 mm/frame) — it is Forward's entry PHASE (feet mid-stride at co 0.92), not stance width → next lever = Idle→Locomotion
transition OFFSET (enter at double support), Forward untouched. Left group still class C (BL +55°, FL +50°, L +42°).
Crossfade 0.15/0.18/0.22 with 06A: −15…−20 % per step; recommend 0.18 (2 frames, 40 mm travel), not 0.22. Pose builder
recipe (execute_code): foot-lock solver targets are PELVIS-relative (feet follow the pelvis drift), foot yaw needs the
TOE targets (yaw residual is weak), hip-line yaw drifts after every leg solve (damp 0.5), body height set by the
straighter knee. Research assets: 06/06A/06B/06C, Pose_v3A/B/C_research, XfadeStudy_E18/E22 controllers.

## Idle06 REJECTED by human (2026-09-07) — v005 stays FINAL. Forward entry-phase study — Reports/CombatWalk_ForwardEntryPhaseStudy.md
Idle→Locomotion offset 0 enters Forward v012 at clip phase 0.92 (left foot mid-swing, right knee 4.5° at toe-off, left
heel strike 3 frames away) → the "foot reorganisation" is a blend into a landing impulse. Cycle map vs idle stance: the
only left-ahead double-support window is 0.96–0.10. BEST = clip phase 0.10 (transition offset 0.18): feet 2.96/3.49 m/s
(cur 3.56/3.64), max step 47 mm (69), knee 921°/s (1111), rear foot steps first, class A/B; SECOND 0.15 (offset 0.23).
Right-foot 3.5 m/s = the walk's own swing speed (steady max 3.8) — irreducible first step. 0.18 s fade: peaks −17 %,
visually same. KCC responsiveness identical (0.76→0.95 m/s in 6 frames). RECOMMENDATION: architecture A = one
controller number (Idle→Locomotion offset 0.18), steady-state cycleOffsets untouched; the family is phase-aligned so the
same offset enters every direction after its left landing (only Forward tested). Research controllers
Knight_Controller_PhaseStudy_P10/P15/P10_D18. Nothing installed.

## Idle→Light entry offset 0.18 PRODUCTION (2026-09-07) — Reports/CombatWalk_IdleToLight_EntryOffset_Install.md
All-eight preflight at offset 0 vs 0.18 (same session, production clips): Forward step 69→47 mm, knee 1111→815°/s
(B→A/B); FR feet 4.1→3.0, root-X crossing in the blend gone; BR step 61→46, pelvis peak 106→89; R neutral (A→A);
B mixed-small (B→B); BL pelvis vertical 1.14→0.57 (C→C); L, FL neutral (C→C). Runtime confirms every direction's
state phase shifts +0.18 past its left landing. KCC responsiveness identical. WRITTEN: Knight_Controller Idle→Locomotion
m_TransitionOffset 0 → 0.18, duration 0.15 kept, exit untouched; ring/child offsets/clips/GuardGait/prefab/Travel
hash-identical. Tests 55/55 (AE1 contract). Remaining class-C = left group facing (BL/L/FL) → next milestone.

## BackLeft facing study (2026-09-07) — Reports/CombatWalk_BackLeft_Facing01.md — NOT installed
`__freshHumanBackLeft_Facing01` = BackLeft v001 recipe (re-proven key-identical) with ONLY `yawTowardTravelDeg −20 → −7`
and `headYawDeg 12 → 4.2`; legs re-solved to the same foot targets (uniform 21 mm lane shift, drift 0, knees +2°),
arms bit-identical, native/speed/cadence identical. Runtime pelvis +31 → +9.4, shoulders +5 → −2.6; Idle→BL Δ65.6 → 44.4
(462 → 335°/s); Back→BL Δ44 → 23 (261 → 139°/s); BL→Left Δ −11 → +9 (104 → 55°/s). Study A (yaw −13, +19) still turns;
C (yaw −1, −0.3) squares and re-jumps into Left. Video read: B keeps the chest on the threat while hips lead the
retreat. Next if approved: promote as BackLeft v002 (same slot/co 0.35/GuardGait), then Left, then ForwardLeft (FL needs
a curve-level edit — FL03 base not rebuildable).

## BackLeft v002 installed (2026-09-07) — Reports/CombatWalk_BackLeft_v002_Install.md
`Sword1H_WalkBackLeft_v002` = CopyAsset of the approved `__freshHumanBackLeft_Facing01` (102/102 key-identical, sampled
pose 0.0005 mm). Light_Walk8 −135 slot only: v001 → v002, position / ts 1 / co 0.35 unchanged (support phases measured
identical to v001: L lands 0.33, R lands 0.825). GuardGait −135 0.592/0.468 unchanged; runtime 0.535 m/s, 0.904×, 167 spm.
Facing pelvis +9.4 / shoulders −2.6 (v001 +30.6 / +5.3); Idle→BL Δ44 at 337°/s; Back→BL Δ24 at 136°/s (was 44 / 261);
BL→Left Δ+10 at 55°/s. 17/102 curves differ from v001 (torso twists, neck turn, RootQ, leg re-solve); arms bit-identical.
Tests 55/55 (Y1 renamed to v002 contract; AA1/AB1/AC1/AD1/AE1 dictionaries → v002). Travel keeps its own v001_Travel copy.
Harness note: the lock step's NRE (`Health.OnDeath` null on the runtime target) is benign and pre-existing; if the editor
is not running, launch 6000.5.6f1 on the project before queuing. Scratchpad scripts are per-session — recover them from
the transcript heredocs (right_qa / fr_qa / ci_qa / phase_qa / analyze_diag / mkvid) when a session starts fresh.
Next (human decision): Left facing cleanup, then ForwardLeft (FL needs a curve-level edit — FL03 base not rebuildable).

## Travel_Walk8 = full authored family (2026-09-07) — Reports/CombatWalk_TravelFamily_Install.md
Free-travel ring now carries eight key-identical `_Travel` copies (FR v001, R v001, BR v001, B v001 new; FL / L / BL
bumped to v002 / v002 / v002) at the canonical headings with the family cycleOffsets; TravelGait walk table = GuardGait
(eight entries). Free-travel sweep reproduces every lock-on world speed (root constant, weights sum to 1). Z1 asserts
eight copies + table parity with GuardGait. Editor-side controller writes can be refused by the permission classifier —
patch the serialized YAML block (line-count preserved) and force-reimport instead. Lane trap: BR at 135° from (26, 6)
hits the wall at z ≈ 4.6; use (24, 0). Still mocap: Travel_Jog8 / run / heavy trees.

## FootIK sole levelling (2026-09-07) — Reports/FootIK_SoleLevelling.md
The "rolled right boot" was authored, not IK: every walk stands the soles on their outer edges (L −20…−26°, R
+23…+33° vs the mesh sole plane; idle +5 / +8) because the generator solves foot pitch but never roll. FootIK now
calibrates each sole from the leg mesh bind pose and rolls the foot level about its own sole axis (pitch untouched,
own 60 ms smoothing, clearance fade, gates for unreadable / non-flat meshes → mobs unchanged). Transport identical
on / off. If the walks are regenerated, solve roll at the source (Foot Twist In-Out is ~0.9 roll on this rig).

## RUN family started: RunForward01 research (2026-09-07) — Reports/CombatRun_ForwardRun01.md — NOT installed
`HumanWalkPerformance.Options.runGait` (inert at default; Fresh02 → v012 rebuild 3.8e-6): scheduled stance per foot
(`runToeOffPhase`), flight with interpolated hip height (+ballistic rise), contact/toe-off distances, swing lift, deeper
sink, ±7° pelvis yaw, lean dial (~0.2°/° on this rig; Front-Back NEGATIVE = forward; Neck Nod positive = DOWN), off-hand
gain. `__freshHumanRunForward_01`: 0.66 s, stance 0.45, flight 13 %, native 2.05 m/s, 182 spm, knees 27/18, chest +5.3°
vs walk, head 1.0°. Speed study 1.85 / 2.05 / 2.35 all stride-clean; current 4 m/s sprint clamps MotionSpeed at 1.5×
and skates 0.93 m/s — the sprint authority is beyond this run's range. Micro-study stance 0.42/0.45/0.48 → 0.45.
Harness: `run<deg>` segments hold sprint (stamina refilled), `RTQA_sprintSpeed` rewrites MaxSprintSpeed live (world
m/s), `RTQA_walkRef/jogRef` re-anchor the rating; research controller `Knight_Controller_RunFwd01Study` (Light_Run8 fwd
child → clip @ (0,1) co 0.11, Gait_Light 1.3/1.6) + `GuardGait_RunFwd01Study` (jog@0 = native). Entry/exit to the run
untuned (feet 6–11 m/s at the seams): entry-phase study after approval. Next after approval: install, then Run ring.

## Run tier BLOCKED — legacy sprint restored (2026-09-07) — Reports/CombatRun_TierArchitecture.md
Traced input → writer: the Sprint action is the ONLY selector of Light_Run8 (Gait_Light on raw Speed 1.85/3.655; two
sustained speeds 1.30 walk / 4.0 sprint; MotionSpeed = speed ÷ lerp(walk, jog natives) clamp 0.6–1.5). No player state
distinguishes Run from Sprint → product decision needed (B stance-driven: lock-on sprint = Run 2.05 / free sprint =
Sprint 4.0; C sprint→run rule; D new input). Production restored to pre-run state: Light_Run8 fwd = `Sword1h_RunLt45Loop`
(−8.7°, co 0.93), jog −8.7:3.56; controller/GuardGait/prefab md5 = Travel-install state. `Sword1H_RunForward_v001`
stays on disk unreferenced, qualified at 2.05/1.00×/182. Sprint regression: 3.52 MS 1.044, 4.0 MS 1.131, skate ≈ 0.
AF1 pins the restored state. Do NOT reinstall the run child until the tier exists (4 m/s sprint would skate 0.93).
