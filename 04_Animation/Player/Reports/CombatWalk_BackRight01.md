# `__freshHumanBackRight_01` — TRUE BACKRIGHT (+135°) research challenger (2026-09-04)

Research only. Not installed. Forward v012 / ForwardLeft v001 / Left v002 / BackLeft v001 / Back v001 / Travel untouched.

### BACKRIGHT01 MOTION DESIGN
A CONTINUOUS ALTERNATING DIAGONAL RETREAT to the sword side, built on the alternating-lane architecture (Left v002,
Back v001) rotated to travel +135°: each foot strides the same 220 mm along the diagonal in its own lane, half a cycle
apart, lands ball-first, and the pelvis sways laterally over each support foot. Not a mirror of BackLeft — that was
tested and set aside (below).

| dial | BackRight01 | Back v001 | BackLeft v001 |
|---|---|---|---|
| architecture | alternating diagonal | alternating | step-close |
| cycleSeconds / samples | 0.82 / 48 | 0.82 / 48 | 0.65 / 48 |
| travelDeg / drift / swingFrac | **+135 / 220 / 0.42** | 180 / 220 / 0.42 | −135 / 250 / 0.35 |
| laneOutMm / stanceWidenMm | 40 / **−40** (sign flips under the rotation: negative widens) | 40 / −15 | — |
| swingHeight / sway (across travel) / sink | 60 / 25 / 12 | 60 / 25 / 12 | 70 / 40 (along) / table |
| yawTowardTravelDeg / chestFollowFrac / headYawDeg | **+12 / −0.03 / −7** | +8 / −0.03 / −5 | −20 / −0.03 / +12 |
| strike / soleLoad / swingToesUp / worldPitch | 6 / 0.12 / 2 / true | same | 8 / 0.12 / 2 / true |
| stanceKnee / minKnee / armGain / reachMargin | 38 / 15 / 0.25 / 20+8 | same | same |

### LEAD / TRAIL LEG STRATEGY (the sword-side question, answered by test)
Two strategies were built with the same body dials and judged on the real Player:
- **A — asymmetric step-close, sword-side (right) rear foot leads** (BackLeft's mirror): native 0.592 m/s, stance
  422…736 mm, hips −16°, 27 % double support. Reads as a series of wide lunges to the right with the left guard foot
  closing; the pelvis parks over each landing.
- **B — continuous alternating diagonal (chosen)**: native 0.463 m/s, stance 183…457 mm, hips −16°, 24 % double
  support. Both legs take the same cautious step; the right (sword-side) foot leads the cycle (lands at 0.395) and the
  left follows (0.895); neither leg ever reaches past the other's lane, so the sword-side hip never has to collapse to
  cover a long reach. B keeps the rear hemisphere architecturally coherent (Back is alternating; Right will be) and
  keeps the continuous, non-segmented read the reviewer chose for Left.
Right shoulder identical in both (Down-Up 0.093…0.105, sword-pivot to chest ≥ 459 mm).

### FOOT LANES / NO CROSSOVER
Across-travel lane separation **177…179 mm** at every phase; along travel the left foot stays 40…422 mm on the threat
side of the right foot (never passes it); root-frame right-minus-left 155…424 mm; boot separation **183…457 mm**.
No crossover, no cross-behind, no midline interception. Runtime minimum gap 164 mm across the context runs.

### LANDING
Ball-first: toes 6° down at contact, sole loaded over the first 12 % of the stance, flat through support (world-pitch
solve; pitch vs flat −6…+2°). Audit: heel clearance +2 / +3 mm, toe pad −8 / −4 mm at strike, flat 60 / 63 % of the
cycle, no burial. Boot headings L −9.8° / R +1.5° mean (a natural toe-out around forward; Forward v012 −22 / −25).

### STEP LENGTH / STANCE WIDTH
220 mm per foot (380 mm between footfalls), base 178 mm across travel, separation 183…457 mm: a readable combat
base that never narrows to a line and never lunges (candidate A reached 736 mm).

### WEIGHT TRANSFER
Pelvis 709…746 mm (rhythm 36.8 mm, 12 mm acceptance trough after each landing), lateral sway 25 mm over the loaded
foot, world pelvis speed 0.24…0.57 m/s at native (no pause, no float). Knees L 43.8…71.9°, R 47.0…62.6° — softly
flexed throughout, never a column.

### PELVIS / TORSO / HEAD
Root yaw 0.00° / 0.00 mm in the clip; runtime root 354.29°, constant. Authored hip line −15.7° (−20.4…−11.1),
shoulders −8.4° (−9.5…−7.2): the pelvis blades slightly RIGHT — the idle's own blade direction and the natural rear-foot
retreat — and stays restrained; head vs the v012 reference +0.4° (−4.0…+4.7). Runtime relative to the root: pelvis
**−21.5°**, shoulders **−14.1°** (Back: −15.0 / −11.7; idle −34.5 / −39.4). Nothing turns toward +135°. This blade
evolves naturally toward Right (+90) and ForwardRight without a reversal.

### SWORD / SHOULDER
Right Shoulder Down-Up 0.093…0.105 (identical to Back and Left v002, no compression), sword-hand path 235 mm / cycle
(Back 255, Left 172), sword pivot never closer than 459 mm to the chest, armGain 0.25.

### FOOT CONTACT AUDIT (`LocomotionFootContactAudit`, 240 samples, rig-calibrated)
| leg | knee min | reach max | % below 15° | pitch vs flat | heel / toe min | support | flat | buried | support yaw drift |
|---|---|---|---|---|---|---|---|---|---|
| Left | 43.8° | 0.928 | 0 | −6…+2° | +2 / −8 mm | 63 % | 60 % | 0 % | 3.2° |
| Right | 47.0° | 0.917 | 0 | −6…+2° | +3 / −4 mm | 63 % | 63 % | 0 % | 16.9° |

Per-leg reach planning (reachMarginMm 20, rightReachExtraMm 8) — the shorter right leg tops out at 0.917 of its length.
Right planted yaw drift 17° = the family watch item (Back 9.9, Left v002 17).

### NATIVE SPEED / CYCLE / CADENCE
True heading **+135.0°** (both stance tracks, across-travel slide 0); native planted-foot speed **0.463 m/s**
(220 mm / (0.82 × 0.58)); cycle 0.82 s; 146 spm; step 220 mm per foot.

### SPEED STUDY (real Player, lock-on, loadout, research override into the +125.3° ring slot rated 0.463 / 0.405;
MaxStableMoveSpeed 2 in every diag)
| candidate | playback | world speed | cadence | 0.25 s | 0.5 s | 1.0 s | stride error | rel-root pelvis / shoulders |
|---|---|---|---|---|---|---|---|---|
| SLOW | 0.851× | 0.394 m/s | 124 spm | 0.10 m | 0.20 m | 0.39 m | 0.0 mm/s | −21.4 / −13.9 |
| **MEDIUM (= native)** | **1.001×** | **0.463 m/s** | **146 spm** | **0.12 m** | **0.23 m** | **0.46 m** | 0.0 mm/s | −21.5 / −14.1 |
| BRISK | 1.151× | 0.533 m/s | 168 spm | 0.13 m | 0.27 m | 0.53 m | 0.0 mm/s | −21.6 / −13.3 |
| current +135° gameplay (comparison) | 1.408× | 0.652 m/s | 206 spm | 0.16 m | 0.33 m | 0.65 m | 0.0 mm/s | −21.6 / −14.1 |

Study-rig note (as for Back01): the override slot sits at +125.3° while the clip travels at +135°, so the feet carry a
9.7° sideways component at runtime (25 mm per stance); along travel they are planted. A true +135° slot removes it.

### SELECTED PRESENTATION (my read; human review decides)
**MEDIUM — 1.00× / 0.463 m/s / 146 spm**, matching Back v001 exactly so the rear hemisphere retreats at one speed; the
steps read as deliberate diagonal ground-giving. BRISK (0.533, 168 spm) matches BackLeft's 0.535 and still reads as
steps; the current 1.41× is a scurry. Likely production data: +135 native 0.463, gameplay scale ≈ 0.405.

### BACK → BACKRIGHT CONTEXT
`BackRight01_C_Back_BackRight_Back.mp4` (Back 0.463 / 1.00× ↔ BackRight01 0.463 / 1.00× through the research gait, no
global multiplier): boundary foot speeds ≤ 1.0 m/s (steady 2.7–3.8), knee rates 129–228°/s (steady 233), pelvis-height
rate ≤ 0.55, sword ≤ 0.54, minimum foot gap 185 mm — the calmest transition in the family. Pelvis −15° → −21.5°.
`BackRight01_D_BackLeft_Back_BackRight.mp4`: BackLeft 0.535 / 0.904× → Back 0.463 / 1.00× → BackRight01 0.463 /
1.00×; boundaries clean (foot ≤ 1.97 m/s vs steady 3.5, gap ≥ 179 mm); pelvis +31° → −15° → −21.5°, root constant —
the fighter turns back toward his guard as the retreat crosses to the sword side, never away from the opponent.
`RearFamily_BackLeft_Back_BackRight01_gameplaySpeeds.mp4`: the three rear directions side by side at their presentations.

### LIKELY CYCLE OFFSET
BackRight01 lands the right foot at 0.395 and the left at 0.895 of its cycle — the same contact phases as Back v001
(0.395 / 0.895). With Back at co 0.87 (tree time L 0.025 / R 0.525, = BackLeft's), the likely BackRight offset is
**0.87** (exact match to both neighbours). Not written anywhere.

### TECHNICAL QA
Legality PASS (24 samples/span, 0 flattenings), temporal 0 hold snaps (largest delta L knee 3.4° @0.39), support
schedule 62 / 62 % with 24 % double support, swing 133 mm peak with the toe bone never below 47 mm, seam clean
(49 keys / curve), deterministic (102 bindings, max |Δ| 0), root canonical 0.00° / 0.00 mm, fully baked in-place contract.
No generator change this milestone.

### VIDEOS (`Artifacts/AnimationReview/`)
A/B `BackRight01Speed_br01_medium.mp4` (native = selected), `BackRight01Speed_br01_slow / _brisk / _current.mp4`,
`BackRight01Speed_2x2_slow_medium_brisk_current.mp4` (top-left slow, top-right medium, bottom-left brisk, bottom-right
current), `BackRight01Study_br_A.mp4` / `_br_B.mp4` (leg-strategy study), C `BackRight01_C_Back_BackRight_Back.mp4`,
D `BackRight01_D_BackLeft_Back_BackRight.mp4`, E `RearFamily_BackLeft_Back_BackRight01_gameplaySpeeds.mp4`.

### PRODUCTION SAFETY
Not installed. No controller reference to BackRight01 (or any research clip); `GuardGait_BackRight01Study.asset` is a
research asset under Generated; study candidates removed. Editor out of Play before every write; prefab 2 / 4 and scene
Player (no override) verified before the runtime sessions; controller cycle offsets on disk unchanged. VerifyProduction
TRUE. Tests 50 / 50.
