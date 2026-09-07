# Idle → Light locomotion entry offset 0.18 — all-eight preflight and production install (2026-09-07)

One production number changed: the Idle → Locomotion transition's **destination offset 0 → 0.18**. Duration stays
0.15 s; every clip, child position, child cycleOffset, GuardGait entry, gameplay scale, Travel child, FootIK and Player
value is unchanged (eleven production files hash-identical before and after the write; ring read back from disk).

### ALL-8 OFFSET 0.18 REGRESSION CHECK (real Player, production clips, lock-on, loadout 0.880, FootIK, 60 fps;
offset 0 on the production controller and 0.18 on a copy, same session)
| dir | entry clip phase 0 → 0.18 | feet L / R m/s | max mm/frame | knee °/s | pelvis / shoulders °/s | pelvis vert · sword m/s | min sep mm | planted after fade L / R (steady) | stable | class |
|---|---|---|---|---|---|---|---|---|---|---|
| F | 0.92 → 0.10 | 3.56 / 3.64 → **2.94 / 3.49** | **69 → 47** | **1111 → 815** | 300 / 240 → 281 / 218 | 1.13 · 1.14 → 1.35 · 1.28 | 169 → 167 | 86 / 3 → 52 / 17 (37 / 29) | 0.13 → 0.13 s | B → **A/B** |
| FR | 0.94 → 0.12 | 4.14 / 3.10 → **3.04 / 2.92** | 58 → 48 | 359 → 383 | 181 / 194 → 172 / 174 | 0.88 · 0.91 → 0.88 · 0.88 | 166 → 167 | 93 / 0 → 69 / 21 (42 / 41) | 0.18 → 0.13 | B → B (root-X crossing during the blend gone) |
| R | 0.87 → 0.05 | 2.77 / 1.79 → 2.42 / 2.02 | 40 → 37 | 317 → 326 | 174 / 200 → 185 / 197 | 0.76 · 0.65 → 0.80 · 0.70 | 278 → 293 | 72 / 34 → 45 / 62 (47 / 68) | 0.18 → 0.18 | A → A |
| BR | 0.87 → 0.05 | 3.03 / 3.05 → **2.55 / 2.30** | 61 → 46 | 244 → 274 | 106 / 172 → **89** / 168 | 0.62 · 0.59 → 0.60 · 0.59 | 182 → 235 | 83 / 24 → 52 / 55 (64 / 47) | 0.18 → 0.18 | A → A |
| B | 0.87 → 0.05 | 2.92 / 3.20 → 3.01 / 2.89 | 63 → 53 | 260 → 327 | 431 / 287 → 442 / 283 | 0.55 · 0.60 → 0.65 · 0.65 | 209 → 211 | 76 / 28 → 45 / 55 (63 / 47) | 0.18 → 0.18 | B → B |
| BL | 0.35 → 0.53 | 2.95 / 3.23 → 2.71 / 2.83 | 58 → 55 | 340 → 474 | 499 / 310 → 462 / 298 | **1.14 → 0.57** · 1.20 → 0.99 | 415 → 415 | 76 / 41 → 48 / 69 (49 / 75) | 0.18 → 0.18 | C → C (feet helped) |
| L | 0.34 → 0.52 | 1.87 / 2.17 → 2.11 / 2.10 | 41 → 47 | 309 → 297 | 387 / 286 → 418 / 287 | 0.77 · 0.91 → 0.75 · 1.10 | 214 → 214 | 79 / 28 → 52 / 55 (49 / 66) | 0.13 → 0.18 | C → C (neutral) |
| FL | 0.92 → 0.10 | 2.44 / 3.21 → 2.13 / 2.97 | 46 → 48 | 431 → 434 | 491 / 318 → 470 / 293 | 0.97 · 1.02 → 0.97 · 1.07 | 162 → 161 | 90 / 0 → 66 / 10 (37 / 46) | 0.13 → 0.13 | C → C (neutral) |

No crossing or interception introduced (FL's root-X foot crossing during its diagonal blend exists at both offsets and is
the clip's lane geometry, not the entry). No pop. The alignment assumption holds at runtime, not only on paper: the state
phase at the end of the nine-frame fade moves from 0.11–0.16 to 0.29–0.34 for all eight, i.e. every direction enters
after its left-foot landing (family landings at tree 0.06–0.12) with the rear foot free to step.

### FORWARD RESULT
Confirmed on the final family: entry clip phase 0.92 → 0.10; max step 69 → 47 mm/frame, knee 1111 → 815°/s, left foot
2.94 m/s (3.56), pelvis peak 281°/s (300). The left foot stays loaded and the rear right foot takes the first step
instead of the front foot swinging into its own landing. Visual B → A/B. Pelvis vertical / sword rise 1.13 → 1.35 /
1.14 → 1.28 m/s: the loaded-foot phase carries the walk's weight transfer into the blend (a first stride, not a snap).

### FORWARDRIGHT RESULT
Improved: left foot 4.14 → 3.04 m/s, step 58 → 48, first-step planted state 93 / 0 → 69 / 21 %, stable gait 0.18 → 0.13 s,
and the feet no longer cross the root X axis during the blend. Facing identical. B → B (cleaner).

### RIGHT RESULT
Neutral: feet 2.77 / 1.79 → 2.42 / 2.02, step 40 → 37, pelvis peak +11°/s (0.2° per frame), separation 278 → 293.
Visually indistinguishable; A → A. Not sacrificed.

### BACKRIGHT RESULT
Improved: feet 3.03 / 3.05 → 2.55 / 2.30, step 61 → 46, pelvis peak 106 → 89°/s, separation 182 → 235. A → A.

### BACK RESULT
Mixed, small: step 63 → 53 and right foot 3.20 → 2.89 better; knee rate 260 → 327°/s and pelvis vertical 0.55 → 0.65
worse; the 431 → 442°/s pelvis snap is unchanged (Back's own). Visually identical in the strips; B → B.

### BACKLEFT RESULT
Feet helped: pelvis vertical velocity 1.14 → 0.57 m/s, sword 1.20 → 0.99, feet 2.95 / 3.23 → 2.71 / 2.83, pelvis peak
499 → 462; knee rate 340 → 474. Facing residual unchanged (+65°). C → C.

### LEFT RESULT
Neutral: step 41 → 47 mm, pelvis peak 387 → 418°/s, feet 1.9 / 2.2 → 2.1 / 2.1, knee 309 → 297; sword 0.91 → 1.10.
Facing residual unchanged (+53°). C → C.

### FORWARDLEFT RESULT
Neutral-to-better: feet 2.44 / 3.21 → 2.13 / 2.97, pelvis 491 → 470, shoulders 318 → 293, step 46 → 48. Facing residual
unchanged (+61°). C → C.

### RESPONSIVENESS
KCC velocity over the first six frames after input is identical at both offsets in every direction (Forward 0.76 → 0.95
m/s, lateral 0.46 → 0.50, rear 0.40 → 0.44): movement starts on frame one; the offset only selects the animation phase.
Time to steady gait 0.13–0.18 s in every run, unchanged.

### PRODUCTION TRANSITION CHANGE
`Knight_Controller.controller`, Base Layer, Idle → Locomotion: `m_TransitionOffset 0 → 0.18`; duration 0.15 s fixed,
interruption Destination, condition Speed > 0.3 unchanged. Locomotion → Idle untouched (offset 0, 0.22 s). The
working-tree diff on the controller shows this one line beyond the earlier ring installs.

### CHILD CYCLEOFFSET INTEGRITY
Light_Walk8 on disk after the write: Forward v012 0.92 · ForwardRight v001 0.94 · Right v001 0.87 · BackRight v001
0.87 · Back v001 0.87 · BackLeft v001 0.35 · Left v002 0.34 · ForwardLeft v002 0.92; positions and clip references
unchanged; Travel_Walk8 byte-identical. Steady-state gait, world speed, MotionSpeed and cadence are untouched by a
transition offset.

### VIDEOS (`Artifacts/AnimationReview/`)
A `EntryOffset_A_Forward_offset0_vs_0p18.mp4` · B `EntryOffset_B_grid_all8_offset0p18_F_FR_R_BR_over_B_BL_L_FL.mp4` ·
C `EntryOffset_C_best3_BR_R_FR_before_over_after.mp4` · D `EntryOffset_D_leftgroup_BL_L_FL_before_over_after.mp4` ·
E `EntryOffset_E_full_sequence_Idle_each8_Idle_offset0p18.mp4` (the candidate controller, all eight in sequence).

### TESTS
`AE1_IdleToLight_EntryOffset_Contract` added: Idle → Locomotion offset 0.18, duration 0.15 fixed, exit transition
untouched, exactly eight ring children with their eight cycle offsets and production asset paths. Suite **55 / 55**.

### PRODUCTION STATE
Idle v005 · Light_Walk8 eight authored children (offsets above) · Idle → Locomotion offset 0.18 / 0.15 s ·
Locomotion → Idle 0 / 0.22 s · GuardGait −135 0.592 / −90 0.496 / −45 1.261 / 0 1.34 / 45 1.208 / 90 0.496 / 135 0.463 /
180 0.463 · Player 2 / 4 · Travel untouched · no research reference · VerifyProduction TRUE. Research controllers
`Knight_Controller_PhaseStudy_*` remain under Generated. Editor out of Play for the write; controller, GuardGait and
prefab read from disk after. Git index untouched; nothing added or committed.
