# `__freshHumanBackLeft_Facing01` — BackLeft body-facing study (2026-09-07)

Research only. Not installed. `Sword1H_WalkBackLeft_v001`, CombatIdle_v005, the production transition (offset 0.18 /
0.15 s), the other seven clips, GuardGait, gameplay scales, Travel, FootIK and Player untouched (read back from disk;
VerifyProduction TRUE). Candidates reached the Player through an in-memory name override on the production controller,
so the production entry offset and GuardGait (−135 = 0.592 / 0.468) were the ones in use.

### BACKLEFT PRODUCTION FACING BASELINE
Runtime (lock-on, loadout 0.880, all writers): pelvis **+30.6°** (+24…+37), shoulders **+5.3°** (+3.5…+7.2) relative to
the root, right boot heading −48°. Idle → BackLeft with the production 0.18 entry offset: Δpelvis **+65.6°** at
462°/s (7.7° per frame), Δshoulders +44.8° at 298°/s, feet 2.7 / 2.8 m/s, sword 0.99 m/s. Back → BackLeft: pelvis −13
→ +31 (Δ44) at 261°/s, sword 1.35 m/s; BackLeft → Left: +30 → +19 at 104°/s. Authored: hip line 30…43° (mean 36),
shoulders 9…13°, head on the v012 reference (−0.3°).

### THREE-CANDIDATE FACING STUDY
Method: the production clip rebuilds key-identical from its recorded recipe (re-proven in this run: 102 bindings,
max |Δ| < 0.0005, feet / knees / pelvis-Y / hand-in-chest deviation 0.0), so the candidates are that recipe with only
the facing dials changed — `yawTowardTravelDeg` −20 → −13 / −7 / −1 and `headYawDeg` scaled to keep the head on the
threat. Foot targets, cycle 0.65 s, phase, stride, step-close architecture and every arm curve are unchanged by
construction; the legs are re-solved to the same marks under the new hip yaw (not repinned, no upper-body leg pass).

| | yaw dial | runtime pelvis | runtime shoulders | authored hip / shoulders | head vs ref | curves changed | feet vs v001 (max, uniform lane shift) | knees vs v001 |
|---|---|---|---|---|---|---|---|---|
| v001 | −20 | +30.6 | +5.3 | 36.3 / 9…13 | −0.3° | – | – | – |
| A (small) | −13 | +19.2 | +1.1 | 24.9 / 5…9 | −0.2° | 17 / 102 | 12 mm | 1.9° |
| **B (moderate)** | **−7** | **+9.4** | **−2.6** | 15.1 / 1…5 | −0.1° | 17 / 102 | 21 mm | 2.8° |
| C (stronger) | −1 | −0.3 | −6.2 | 5.4 / −2…1 | 0.0° | 17 / 102 | 31 mm | 3.4° |

The 17 changed curves are the 12 leg channels (re-solve), Spine / Chest / UpperChest twist, Neck / Head turn and the
root rotation; every arm and hand curve is bit-identical.

### SELECTED CHALLENGER
**B → `__freshHumanBackLeft_Facing01`** (key-identical copy). It halves the opening (+31 → +9) while the pelvis still
leads the leftward retreat; the shoulders sit 3° threat-side of square, ahead of the pelvis as required. A barely
changes the read; C squares the pelvis (the brief's "not square") and re-creates a facing step into Left (+19).

### LOWER-BODY PRESERVATION
Foot world trajectories deviate at most 21 mm — a uniform horizontal shift present equally in stance and swing (the
lane frame follows the hip yaw), not skating: planted mid-stance drift 0 mm at runtime (v001 0), support-yaw drift
20.1 / 7.0° (v001 18.7 / 6.1, authored 21.9 / 5.0 vs 20.3 / 3.6), knee curves within 2.8°, knee minimum 26.7 / 39.2 (v001
24.0 / 37.1), pelvis-Y rhythm within 0.1 mm, stance separation 426 mm (414), touchdown timing unchanged (same cycle, same
lateral schedule), stride and native unchanged (stance-track speed −0.418 / −0.418 m/s in both), toes 59 / 51 mm, buried
0 %, no crossover. Runtime: 0.535 m/s at 0.904×, 167 spm, stride error 0.0 — the approved presentation, no speed
study needed.

### PELVIS / SHOULDERS / HEAD
Pelvis +9.4° (+3…+16 through the cycle), shoulders −2.6° (−4…−1), head within 0.1° of the reference and on the lock
target at runtime; root constant. The right boot heading follows the lane frame (−48 → −26°) with its planted drift
unchanged.

### SWORD / SHOULDER PRESERVATION
Arm, shoulder and hand curves identical; hand-in-chest 0.0 mm; sword pivot 459 mm from the chest (v001 459); sword path
317 mm / cycle (303) from the chest riding the smaller torso yaw; runtime sword peak through the idle entry 0.72 m/s
(v001 0.99). No compression, no whip.

### IDLE → BACKLEFT (production offset 0.18 / 0.15 s)
| | Δpelvis | Δshoulders | peak pelvis | peak shoulders | feet L / R | max step | sword |
|---|---|---|---|---|---|---|---|
| v001 | +65.6° | +44.8° | 462°/s (7.7°/fr) | 298°/s | 2.71 / 2.83 | 55 mm | 0.99 |
| A | +54.2 | +40.6 | 383 (6.4) | 268 | 2.55 / 2.80 | 52 | 0.85 |
| **B** | **+44.4** | **+37.0** | **335 (5.6)** | 254 | 2.39 / 3.24 | 55 | 0.72 |
| C | +34.7 | +33.3 | 301 (5.0) | 242 | 2.25 / 3.00 | 53 | 0.63 |

Exits (BackLeft → Idle): v001 306°/s → B 213°/s. The remaining +44° is the Idle's own blade against a left retreat;
the lead-foot swap of the step-close architecture is unchanged by design.

### BACK → BACKLEFT
Production: pelvis −12.9 → +30.6 (Δ43.5) at 261°/s, shoulders −10.9 → +5.2 at 98°/s, sword 1.35 m/s. Challenger:
−13.8 → +9.4 (**Δ23**) at **139°/s**, shoulders −11.3 → −2.7 at 52°/s, return 62°/s (138), foot / hips / sword at the
boundaries unchanged (≤ 1.54 / 1.31 / 1.34 m/s, separation ≥ 208 mm). The family's largest moving seam roughly halves.

### BACKLEFT → LEFT
Production: +30.3 → +19.1 (Δ −11) at 104°/s. Challenger: +9.9 → +19.0 (Δ +9) at **55°/s**, return 77°/s (117). Same
magnitude, opposite sign, gentler; no new jump. Left v002 untouched.

### FOOT / LEG REGRESSION
`LocomotionFootContactAudit` (B): knee 26.7 / 40.2 (v001 24.0 / 38.2), toe −2 / 0 mm (same), buried 0 %, support-yaw
drift 21.9 / 5.0 (20.3 / 3.6), stance-track speed unchanged, legality PASS with 0 fixes, phase unchanged (same recipe
schedule; runtime landings at the same tree times). Runtime planted drift 0 mm, separation 426 mm, no crossover.

### VISUAL JUDGMENT (gameplay camera)
Production: the whole body opens to the left as the retreat starts — the "turns to run" read. A: still turns.
**B: the chest stays on the threat while the hips lead the back-left step; reads as a retreat, not a turn.** C: square
hips, the retreat loses its lean and the step into Left brings the opening back. Class: Idle → B still C-minus on the
numbers (+44°) but the identity change is gone; Back ↔ B reads continuous (was the family's worst moving seam).

### VIDEOS (`Artifacts/AnimationReview/`)
A `BLFacing_A_production_BackLeft_v001.mp4` · B `BLFacing_B_study_A_B_C.mp4` · C `BLFacing_C_production_vs_challenger.mp4` ·
D `BLFacing_D_Idle_to_BackLeft_v001_vs_challenger.mp4` · E `BLFacing_E_Back_BackLeft_Back_v001_vs_challenger.mp4` ·
F `BLFacing_F_challenger_Left_challenger_vs_v001.mp4` · `BLFacing_challenger_pure_loop.mp4`.

### PRODUCTION SAFETY
Research assets: `__freshHumanBackLeft_Facing01` (= B), study clips `__blf_A / __blf_C` (`__blf_B` is the challenger's
source copy). Production controller: Idle → Locomotion offset 0.18 / 0.15 s, ring and offsets unchanged, no research
reference; GuardGait −135 0.592 / 0.468; Player 2 / 4; VerifyProduction TRUE. Editor out of Play before every asset
write. Git index untouched; nothing committed.
