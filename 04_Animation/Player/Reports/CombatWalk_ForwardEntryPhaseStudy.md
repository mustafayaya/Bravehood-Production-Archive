# Idle → Forward entry-phase study (2026-09-07)

Research only. CombatIdle_v005, Forward_v012, the production controller (offset 0, 0.15 s), GuardGait, the other seven
clips, Travel, FootIK and Player untouched; VerifyProduction TRUE. Candidates ran through research controller copies
(`Knight_Controller_PhaseStudy_P10 / P15 / P10_D18`, Idle → Locomotion offset / duration only) on the real Player,
gameplay camera, lock-on, Militia loadout 0.880, FootIK, all writers, 60 fps, MaxStableMoveSpeed 2.

### CURRENT FORWARD ENTRY PHASE
Idle → Locomotion has offset 0, so the Locomotion state starts at normalized 0 and Forward v012 (child cycleOffset 0.92)
enters at **clip phase 0.92**, reaching 0.03 by the end of the nine-frame fade. In the cycle map that is the worst
region for this purpose: left foot mid-swing at 1.6 m/s, right knee 4.5° at toe-off, left heel-strike impulse 3 frames
away, lead only 347 mm. The blend therefore takes a static stance straight into a landing impulse — 3.6 / 3.6 m/s feet,
69 mm/frame on the right shin, knee 1111°/s.

### IDLE STANCE vs FORWARD CYCLE MAP
Idle v005: L (−381, +150) / R (+249, −326), width 631, lead 475, headings −4.5 / +29.5, knees 27 / 26. Forward v012 in the
root frame (no baked translation, so the support foot always slides back at ~1.3 m/s — there is no phase where both feet
are slow; "quiet" means double support with the left foot ahead):

| phase | L / R planted | lead (mm) | distance to idle (mm) | foot speed L / R | knees | note |
|---|---|---|---|---|---|---|
| 0.00 | landing / P | 541 | 528 | 0.4 / 1.9 | 4 / 25 | left touchdown impulse |
| 0.04–0.08 | P / P→off | 559 → 520 | 496 → 482 | 1.3 / 0.2–1.3 | 13–36 / 5 | double support, right toe-off |
| **0.10** | P / swing start | 510 | 470 | 1.5 / 1.0 | 40 / 6 | right foot begins its swing |
| 0.15 | P / swing | 460 | 457 | 1.5 / 1.8 | 44 / 6 | closest positional match |
| 0.25–0.42 | P / swing | 170 → −356 | 580 → 985 | 1.3 / 2.5–2.9 | 30 / 33–52 | right foot passing, lead reversing |
| 0.46–0.62 | swing / P | −457 → −504 | 1071 → 1123 | 0.5–1.6 / 0.9–1.7 | 4–41 / 5–46 | right foot ahead: wrong ordering for the idle |
| 0.71–0.88 | swing / P | −306 → +218 | 919 → 587 | 2.1–3.0 / 1.3 | 36–57 / 10–21 | left foot swinging forward |
| **0.92 (current)** | swing / P | 347 | 558 | 1.6 / 1.1 | 25 / 4.5 | left mid-swing, right knee straight |

### CANDIDATE ENTRY PHASES
BEST 0.10 (transition offset 0.18): left foot just planted and loaded, right (rear) foot leaving the ground — the
first step from a left-forward idle is the rear foot, as it should be; SECOND 0.15 (offset 0.23): the same stance a
beat later, right foot already mid-swing; CURRENT 0.92 (offset 0). The double-support window 0.04–0.08 was not taken:
it sits on the left touchdown impulse.

### FOOT REPOSITION METRICS (entry, 0.15 s fade unless stated)
| candidate | entry phase | foot L / R m/s | max mm/frame | planted 0.5 s after the fade L / R (steady 37 / 30) | width through blend | crossing | stable gait after |
|---|---|---|---|---|---|---|---|
| CURRENT | 0.92 | 3.56 / 3.64 | **69** (R shin) | 86 / 3 % | 631 → 166 | no | 0.13 s |
| **BEST 0.10** | 0.10 | **2.96** / 3.49 | **47** (L shin) | 52 / 17 % | 631 → 167 | no | 0.13 s |
| SECOND 0.15 | 0.15 | 3.20 / 3.39 | 46 (R shin) | 31 / 17 % | 631 → 166 | no | 0.18 s |
| BEST @ 0.18 s | 0.10 | 3.07 / 3.49 | 47 | 52 / 28 % | 631 → 167 | no | 0.17 s |

The right foot's 3.4–3.5 m/s is the walk's own swing speed (steady maximum 3.7–3.8 m/s): from an idle one foot has to
take a first step at gait speed whatever the phase. What the phase changes is which foot and in what state: the current
entry throws the left foot into a landing, the candidates let the loaded left foot stay down and the rear foot step.
Responsiveness identical: KCC velocity 0.76 → 0.95 m/s over the first six frames in every run (movement from frame one).

### KNEE / PELVIS / SWORD METRICS
| candidate | knee rate | peak pelvis / shoulders | pelvis vertical | sword | Δpelvis |
|---|---|---|---|---|---|
| CURRENT | **1111°/s** | 300 / 240°/s | 1.13 m/s | 1.14 m/s | +33° |
| BEST 0.10 | 921 | 282 / 219 | 1.35 | 1.29 | +33 |
| SECOND 0.15 | 868 | 273 / 214 | 1.30 | 1.22 | +33 |
| BEST @ 0.18 s | 976 | 234 / 182 | 1.34 | 1.30 | +33 |

Body facing is untouched by the phase (same idle, same walk); pelvis and sword move slightly more at the candidates
because the loaded-left-foot phase carries the walk's weight transfer into the blend.

### VISUAL COMPARISON
`PhaseStudy_D_3up_current_best_second.mp4`. CURRENT: the front (left) foot lifts and swings mid-blend while the rear
leg locks, then lands — the reorganisation read: **B**. BEST 0.10: the left foot stays planted, the rear right foot
takes the first step, no locked knee: **A / B** (the remaining read is the normal first stride). SECOND 0.15: the
same but the right foot is already mid-flight at the fade: **B**.

### BEST ENTRY PHASE
**0.10** — Idle → Locomotion offset 0.18 with the steady-state child offsets untouched.

### 0.15 vs 0.18 RESULT
At the best phase 0.18 s lowers the pelvis / shoulder peaks by ~17 % (282 → 234, 219 → 182) and raises the right
foot's early planted fraction (17 → 28 %); feet and step unchanged (3.07 / 3.49, 47 mm); two more frames and ~40 mm of
blended travel; visually indistinguishable. Optional complement; 0.15 remains acceptable with the phase fix alone.

### RECOMMENDED ARCHITECTURE
**A — the Idle → Locomotion transition offset** (one controller number). It is the smallest intervention and it changes
nothing about steady-state gait, world speed, MotionSpeed, cadence or the child cycle offsets. Because the whole family
is phase-aligned by cycleOffset (every direction lands the left foot at tree phase 0.06–0.12), a single offset of 0.18
enters *every* direction just after its left-foot landing with the rear foot free to step — coherent with the idle's
left-forward stance for all eight, though only Forward was tested here. B (a direction-aware entry chooser via
`Animator.CrossFade(state, duration, layer, normalizedTimeOffset)` from CharacterAnimator) is feasible if later
directions want different phases, and would keep the controller untouched; not needed for Forward.

### WHETHER STEADY-STATE CYCLEOFFSET MUST CHANGE
**No.** The steady-state offsets (Forward 0.92 and the ring) are qualified and unchanged; the entry phase is a property
of the transition, not of the children.

### PRODUCTION SAFETY
Nothing written to production: controller offset 0 / 0.15 s, Idle v005, Forward v012, GuardGait, prefab 2 / 4 read back
unchanged; research controllers under Generated only; VerifyProduction TRUE; Git index untouched; nothing committed.
Videos: `PhaseStudy_A_current_Idle_to_Forward.mp4`, `PhaseStudy_B_best_entry_phase_0p10.mp4`,
`PhaseStudy_C_second_entry_phase_0p15.mp4`, `PhaseStudy_D_3up_current_best_second.mp4`, `PhaseStudy_E_best_0p15_vs_0p18.mp4`.
