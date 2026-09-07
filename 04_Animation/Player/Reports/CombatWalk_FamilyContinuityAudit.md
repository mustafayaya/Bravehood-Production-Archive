# Light_Walk8 family continuity audit — Idle ↔ locomotion and ring transitions (2026-09-07)

DIAGNOSTIC ONLY. No animation, controller, GuardGait, Travel or Idle asset was modified. Every run: real Player,
production controller / clips / GuardGait (the crossfade study used research controller copies under Generated),
lock-on, gameplay camera, Militia loadout 0.880, FootIK, UpperBodyAim, WeaponInertia, 60 fps, MaxStableMoveSpeed 2,
one Player in the loaded scene. Idle authority `Sword1H_CombatIdle_v005`.

### FINAL 8-DIRECTION BODY-FACING MAP (runtime, relative to the root; hip line / shoulder line; root constant)
| | Idle | 0 F | +45 FR | +90 R | +135 BR | 180 B | −135 BL | −90 L | −45 FL |
|---|---|---|---|---|---|---|---|---|---|
| pelvis | **−28.8** | +4 | −14 | −10 | −16 | −10 | **+37** | **+25** | **+32** |
| shoulders | **−33.8** | −1 | −8 | −6 | −8 | −6 | +11 | +7 | +10 |
| Δ pelvis from Idle | – | +33 | +15 | +19 | +13 | +19 | **+65** | **+53** | **+61** |
| Δ shoulders from Idle | – | +33 | +26 | +28 | +26 | +28 | +45 | +40 | +44 |

Two families: the sword side (F, FR, R, BR, B) sits between −16 and +4 pelvis / −8…−1 shoulders and the Idle at −29 /
−34 belongs to it, 13–33° away; the left group (BL, L, FL) opens +25…+37 / +7…+11, the opposite sign, 53–65° from the
Idle. Head on the lock target in every run (−10.2° constant, UpperBodyAim), root never rotates.

### IDLE → EACH DIRECTION METRICS (crossfade 0.15 s = 9 frames; window = crossfade + 0.5 s)
| dir | Δpelvis | Δsh | peak pelvis °/s (°/fr) | peak sh °/s | foot L / R m/s | boot sep mm | knee °/s | pelvis vert m/s | sword m/s | max bone step mm/fr |
|---|---|---|---|---|---|---|---|---|---|---|
| F | +33 | +33 | 300 (5.0) | 240 | 3.56 / 3.64 | 169 | **1111** | 1.13 | 1.14 | **69** (R shin) |
| FR | +15 | +26 | 181 (3.0) | 194 | 4.14 / 3.10 | 166 | 359 | 0.88 | 0.91 | 58 |
| R | +19 | +28 | 174 (2.9) | 200 | 2.77 / 1.79 | 278 | 317 | 0.76 | 0.65 | 40 |
| BR | +13 | +26 | **106 (1.8)** | 172 | 3.03 / 3.05 | 182 | 244 | 0.62 | 0.59 | 61 |
| B | +19 | +28 | 431 (7.2) | 287 | 2.92 / 3.20 | 209 | 260 | 0.55 | 0.60 | 63 |
| BL | **+65** | +45 | **499 (8.3)** | 310 | 2.95 / 3.23 | 415 | 340 | 1.14 | 1.20 | 58 |
| L | +53 | +40 | 387 (6.5) | 286 | 1.87 / 2.17 | 214 | 309 | 0.77 | 0.91 | 41 |
| FL | +61 | +44 | 491 (8.2) | 318 | 2.44 / 3.21 | 162 | 431 | 0.97 | 1.02 | 46 |

No single-frame pop of a bone beyond the ~60 mm/frame foot repositions listed; head, root and sword continuous.

### EACH DIRECTION → IDLE METRICS (crossfade 0.22 s = 13 frames)
| dir | Δpelvis | peak pelvis °/s | peak sh °/s | foot L / R m/s | sep mm | knee °/s | sword m/s | max step mm/fr |
|---|---|---|---|---|---|---|---|---|
| F | −38 | 181 | 139 | 2.24 / 3.16 | 167 | 480 | 0.81 | 54 |
| FR | −18 | 133 | 119 | 2.45 / 2.30 | 243 | 281 | 0.50 | 44 |
| R | −20 | 127 | 131 | 1.19 / 0.99 | 476 | 137 | 0.61 | 23 |
| BR | −12 | **83** | 122 | 1.76 / 1.77 | 283 | 227 | 0.48 | 31 |
| B | −19 | 113 | 133 | 1.70 / 1.80 | 236 | 218 | 0.47 | 31 |
| BL | **−66** | **280** | 208 | 1.84 / 2.11 | 415 | 193 | 0.84 | 35 |
| L | −54 | 286 | 187 | 2.29 / 2.06 | 238 | 150 | 0.78 | 39 |
| FL | −57 | 331 | 220 | 2.06 / 1.42 | 458 | 476 | 0.73 | 35 |

Exits are uniformly gentler than entries (longer blend, slower feet); the same three directions dominate.

### VISUAL CLASSIFICATION (gameplay camera, grids A / B and the crossfade strips)
| dir | entry | exit | what reads |
|---|---|---|---|
| BR | **A** | A | smallest facing change; feet settle without a visible reorganisation |
| R | B | A | modest orientation change; wide idle stance narrows over the blend |
| FR | B | B | modest; left foot reaches forward-right quickly (4.1 m/s) |
| B | B | B | small facing change but a fast pelvis snap (431°/s) as the right foot reorganises |
| F | **B/C** | B | facing fine; the 631 mm idle stance collapses to 178 mm in 0.13 s — the legs, not the torso, are the tell |
| L | **C** | C | the body opens to the left in ~0.3 s: reads as turning to run |
| FL | **C** | C | same opening, larger and faster |
| BL | **C** | C | worst: 65° opening at 8°/frame plus the rear right foot becoming the lead foot |

No transition is class D (no teleport / single-frame pop); the three class-C entries are all the left group.

### FOOT-STANCE TRANSITION MAP (root-relative mm; +z forward, +x right; heading = ankle→toe yaw; knee °)
Idle final frame, every direction: **L (−382, +150) / R (+249, −326): width 631, left foot 475 ahead, headings L −4 / R +29,
knees 26 / 25**. Steady locomotion stances:

| dir | L | R | lead | width | fore-aft | headings L / R | knees | what changes |
|---|---|---|---|---|---|---|---|---|
| F | (−94, +199) | (+84, −306) | L | **178** | 505 | −25 / −45 | 44 / 2 | width 631 → 178 in the blend (both feet 3.6 m/s) |
| FR | (+77, +211) | (−81, −200) | L | (feet cross the root X, lanes ±88 along the diagonal) | 411 | −4 / −51 | 32 / 30 | left foot travels 460 mm forward-right |
| R | (−140, +122) | (+150, −123) | L | 290 | 245 | −7 / +8 | 72 / 50 | closest stance to Idle; gentle |
| BR | (−202, +76) | (+221, −95) | L | 423 | 171 | −8 / +12 | 51 / 46 | width kept; fore-aft 475 → 171 |
| B | (−104, +136) | (+104, −159) | L | 208 | 295 | −7 / 0 | 50 / 45 | width 631 → 208; right foot swings forward 170 mm |
| BL | (−339, −130) | (+285, +76) | **R** | 624 | 205 | −72 / −45 | 48 / 43 | **lead foot swaps (right becomes lead)**, both boots turn −45…−70° |
| L | (−147, +90) | (+152, −90) | L | 299 | 179 | −39 / −34 | 69 / 48 | width 631 → 299, boots turn −35° |
| FL | (−275, +150) | (+193, −78) | L | 468 | 228 | −55 / −49 | 27 / 34 | boots turn −50°, right foot 250 mm forward |

The Idle's stance is 210–450 mm wider than every walk except BL and 200–300 mm more staggered: every entry rebuilds
the feet at 3–4 m/s inside 9 frames. Locomotion-side problems on top: BL swaps the lead foot; the left group turns
both boots 35–70° toward its travel.

### WORST TRANSITIONS
BL (+65° / 499°/s, lead-foot swap), FL (+61° / 491°/s), L (+53° / 387°/s) — the left-open group. Forward is the worst
foot reorganisation (3.6 m/s both feet, 69 mm/frame shin step, knee 1111°/s) with an acceptable facing.

### BEST TRANSITIONS
BR (+13° / 106°/s entry, 83°/s exit), R (+19° / 174°/s, 40 mm/frame), FR (+15° / 181°/s). The sword-side group is
already in the Idle's fighting identity; what remains there is the stance width.

### BODY VS FOOT CAUSAL ANALYSIS (BackLeft, runtime-only diagnostics, nothing saved)
| variant | peak pelvis | foot L / R | max bone step | read |
|---|---|---|---|---|
| current | 499°/s | 2.95 / 3.23 | 58 | opening + lead swap together |
| body held near Idle for crossfade + 0.35 s, legs free | 249°/s | 2.08 / 2.73 | 51 | the legs already walk the left stance under a still-bladed torso: a torso / legs disconnect, not an improvement |
| legs held near Idle, body free | 502°/s | 3.73 / 3.24 | 61 | the opening is fully visible with the Idle feet; the feet then snap harder |

Both components are real and separable: the **body** cost is the authored facing difference (Idle −29 vs BL +37);
the **foot** cost is the Idle stance geometry (631 mm wide, 475 staggered) against every walk's stance. Holding either
one without the other reads worse than the current blend, so a transition-only procedural body layer is not the answer
for the left group.

### CROSSFADE DURATION STUDY (Idle→Locomotion 0.15 / 0.25 / 0.35 s; exits 0.22 / 0.30 / 0.40; research controller copies)
| | BL peak pelvis | BL foot L / R | BL step mm/fr | FL peak pelvis | FL foot | FL step |
|---|---|---|---|---|---|---|
| CURRENT 0.15 s | 499°/s | 2.95 / 3.23 | 58 | 491°/s | 2.44 / 3.21 | 46 |
| 0.25 s | 302°/s | 2.11 / 2.12 | 38 | 299°/s | 1.20 / 3.11 | 36 |
| 0.35 s | 210°/s | 2.02 / 1.89 | 29 | 214°/s | 2.21 / 3.14 | 36 |

Timing halves the rates and never changes the 60–65° destination: at 0.35 s the opening is still the whole read, spread
over 0.35 m of travel at 0.986 m/s (blend-mush cost). Timing alone does not fix the identity change; a modest
0.15 → ~0.22 s entry is a cheap complement once the geometry is addressed.

### LOCOMOTION RING SWEEP
Direction-to-direction (2 s per heading, both senses) — pelvis before → after, peak rate, foot / sword peak:

| pair | forward sense | reverse sense |
|---|---|---|
| F ↔ FR | −18° @ 69°/s, foot 3.7, sword 1.0 | +16° @ 138°/s |
| FR ↔ R | +4° @ 65°/s | −6° @ 60°/s |
| R ↔ BR | −7° @ 68°/s | +7° @ 58°/s |
| BR ↔ B | +7° @ 36°/s | −6° @ 38°/s |
| **B ↔ BL** | **+45° @ 253°/s, sword 1.33** | **−47° @ 221°/s, sword 1.32** |
| BL ↔ L | −10° @ 90°/s | +11° @ 60°/s |
| L ↔ FL | +7° @ 112°/s | −9° @ 55°/s |
| FL ↔ F | −30° @ 106°/s, foot 3.8 | +28° @ 149°/s |

Slow 360° analog sweep (5° per 0.25 s, 15° windows): pelvis −5 → −8 → −14 → −21 → −19 → −15 → −16 → −18 → −20 → −21 →
−19 → −16 (sword side, ≤ 7° per window) → **−7 → +8 → +23 → +29** (Back → BackLeft, +39° over 45° of input) → +25 →
+22 → +20 → +23 → +24 → +20 → +15 → **0** (ForwardLeft → Forward, −15° per window). Shoulders follow with a quarter of the
amplitude. MotionSpeed and world speed are continuous everywhere (0.463…1.14 m/s). The only body-orientation
discontinuities while moving are the two seams of the left group, both authored asymmetry, not blend-tree artefacts.

### BLEND-SPACE WATCH ITEMS
- **BLEND-SPACE STRIDE WATCH ITEM (FR ↔ R, +55…+85):** planted frames 12–23 % mid-sector (pure headings 29–59 %). On the
  gameplay camera the ring video shows a soft-footed sector, not a pop; unchanged conclusion from the FR install.
- The mirror sector **L ↔ FL (−65…−80)** reads 5–21 % planted for the same reason (Left 0.496 vs ForwardLeft 1.261
  native). Same class; record for the tree-space cleanup.
- Not solved here.

### COMBATIDLE CONTRIBUTION
The Idle contributes two things to every entry. (1) Its blade: −29 / −34 is 13–33° beyond the sword-side walks and
53–65° from the left group — the mean of the eight walks is about −5 / +0 and of the sword-side five about −9 / −6, so
the Idle is the most bladed pose in the family. (2) Its stance: 631 mm wide and 475 mm staggered, 1.5–3.5× wider than
any walk; this alone accounts for the 3–4 m/s foot repositions on the good transitions (F, B, BR, R) where the facing
change is modest. The Idle is the common factor of the foot cost on all eight entries and of a third of the body cost
on the sword side; it is not the cause of the left group's opening.

### RECOMMENDED ARCHITECTURAL SOLUTION
**F — a combination, in this order of evidence:**
1. **D + A (Idle):** the Idle's stance geometry and, to a lesser degree, its blade are the shared cost. A targeted
   Idle revision (narrower, less staggered stance in the 300–420 mm family band; blade eased from −29 / −34 toward the
   sword-side family band ~−15 / −10; head, sword and breathing untouched) removes the foot reorganisation from all eight
   entries and roughly halves the facing cost of the five sword-side entries.
2. **B (left group):** BL, L and FL open +25…+37 — the opposite sign to the Idle and to the rest of the family. This
   is the whole of the class-C read and the only break while moving (B ↔ BL, FL ↔ F). Their opening should come down to
   a modest left lean (order +10…+15) — a dial change on Left (yaw −13) and BackLeft (yaw −20) recipes; ForwardLeft
   v002 cannot be regenerated from dials (base not reproducible) and would need a curve-level RootQ / hip edit or a
   fresh build — the costliest item, flagged.
3. **C (timing):** Idle → Locomotion 0.15 → ~0.22 s as a complement; halves peak rates at no identity cost. Longer than
   0.25 s is not recommended (0.2–0.35 m of blended travel).
4. **E (procedural transition layer): not recommended** — the body-hold diagnostic reads as a torso / legs disconnect
   and cannot address the stance geometry.

### MINIMUM-INTERVENTION PLAN
1. Idle v006 research: same approved pose language, stance width ~380 mm / stagger ~250 mm, blade −29 → ~−18 pelvis /
   −34 → ~−15 shoulders; re-run this audit's eight entries (expected: sword-side entries → class A, foot speeds < 2 m/s).
2. Idle → Locomotion crossfade 0.15 → 0.22 s (controller only; one number).
3. Left-group opening reduction, BackLeft first (worst entry and the moving seam), then Left, then ForwardLeft;
   re-verify each with the existing production contracts (speed, contacts, foot tracks unchanged in kind).
4. Only if 1–3 leave a class-C entry: revisit a transition-only body layer.
Nothing implemented; nothing changed.

### VIDEOS (`Artifacts/AnimationReview/`)
A `Continuity_A_Idle_to_each8_F_FR_R_BR_over_B_BL_L_FL.mp4` · B `Continuity_B_each8_to_Idle_…mp4` ·
C `Continuity_C_ring360_slow_sweep.mp4` · D `Continuity_D_worst3_Idle_to_BL_FL_L.mp4` · E `Continuity_E_best3_Idle_to_BR_R_FR.mp4` ·
F `Continuity_F_BL_entry_current_vs_bodyHeld_vs_feetHeld.mp4` · G `Continuity_G_crossfade_study_BL_FL_0p15_0p25_0p35.mp4`.

### PRODUCTION SAFETY
Production controller, GuardGait, all eight clips, Travel and CombatIdle untouched (read back from disk, VerifyProduction
TRUE). Research assets: `Knight_Controller_XfadeStudy_A/B.controller` under Generated (transition durations only).
Harness: `RuntimeQaDriver` gained the `TransitionDiag` runtime-only component (`RTQA_diag` body / feet); never
serialized. Player 2 / 4, scene Player without override, Militia loadout 0.880 in every diag. Git index untouched;
nothing added or committed. One invalid sweep (lane (25, 2) meets the wall at z ≈ 3.4) was discarded and re-run on the
open floor at (34, −8).
