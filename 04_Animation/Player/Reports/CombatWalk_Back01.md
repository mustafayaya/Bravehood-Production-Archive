# `__freshHumanBack_01` — TRUE BACK (180°) research challenger (2026-09-04)

Research only. Not installed. Forward v012 / ForwardLeft v001 / Left v002 / BackLeft v001 / Travel family untouched.

### BACK01 MOTION DESIGN
A CONTINUOUS ALTERNATING BACKWARD WALK on the alternating-lane architecture that Left02/03 proved, rotated to travel
180°: each foot strides the same 220 mm straight back in its own lateral lane, half a cycle apart, lands ball-first
and settles flat, and the pelvis flows laterally over each support foot (new inert-default option
`lateralSwayAcrossTravel`; the ball-first ankle lift was added to the alternating foot target, gated by the strike
dial — Left02/03 use strike 0, so they are unchanged). No step-close, no reset, no forward-walk reversal.

| dial | Back01 | why |
|---|---|---|
| cycleSeconds / samples | **0.82** / 48 | a cautious continuous beat (Left 0.80, BackLeft 0.65) |
| lateralAlternating / travelDeg | true / **180** | full alternating cycle straight back |
| lateralDriftMm / swingFrac | **220** / 0.42 | shorter than Left's 230 and far shorter than Forward's stride |
| laneOutMm / stanceWidenMm | 40 / −15 | fighting stagger ±40 mm fore-aft; lanes ±104 mm = 208 mm base (Forward v012 174) |
| lateralSwingHeightMm / sway / sinkMm | 60 / 25 (across travel) / 12 | low careful step, lateral weight transfer, acceptance beat |
| yawTowardTravelDeg / chestFollowFrac / headYawDeg | **+8 / −0.03 / −5** | see PELVIS / TORSO / HEAD (candidate B of the study) |
| stanceKneeBendDeg / minKneeBendDeg / armGain | 38 / 15 / **0.25** | crest cap; Forward reserve; retreat pumps the arms less than Forward (0.5) or Left (0.3) |
| lateralStrikeToesDownDeg / soleLoadFrac / swingToesUp | **6** / 0.12 / 2 | ball-first backward landing |
| lateralWorldPitch / reachMarginMm / rightReachExtraMm | true / 20 / 8 | world-pitch solve; per-leg reach reserve |

### LEFT / RIGHT LEG STRATEGY
Right foot swings [0.5, 0.92) and lands at 0.42 of the clip's own cycle (measured), left swings [0, 0.42) and lands
at 0.92; each then carries the body through a 0.48 s stance while the other steps. Stance 60 % per foot, double support
19 %, no flight. Both feet take the same real step — the right foot is not a closing move (lanes ±104 mm, fore-aft
range left −58…+163 mm, right −163…+58 mm: the left foot stays the forward foot of the stance, the right the rear).

### FOOT LANES / NO CROSSOVER
Lateral gap right-minus-left **208…209 mm at every phase**; foot separation 208…361 mm. No crossover, no cross-behind,
no midline intersection. Runtime (real Player) minimum gap 178 mm across the BackLeft → Back → BackLeft run.

### BACKWARD LANDING
Ball-first: toes 6° down at contact, sole loads over the first 12 % of the stance, flat through support (world pitch
solved per frame). Audit: heel clearance +7 / +5 mm, toe −4 / −4 mm at strike (the toe pad touches first), flat 61 / 63 %
of the cycle, no burial. Both boots keep the family's combat heading: ankle→toe yaw L −12.7° / R −6.1° mean (Forward
v012: −21.8 / −25.2) — slightly toe-out, forward-facing, never pointed backward.

### STEP LENGTH / STANCE WIDTH
Per-foot stride 220 mm (Forward ≈ 650, Left 230). Base 208 mm lateral, fore-aft separation up to 361 mm — a readable
combat base that never collapses to a line and never lunges; the Knight can reverse to Forward from any phase.

### WEIGHT TRANSFER
Pelvis 709…746 mm (rhythm 36.8 mm, one 12 mm acceptance trough 18 % into each stance), lateral sway 69 mm peak to
peak over the loaded foot, fore-aft 16 mm. World pelvis speed at native never below 0.26 m/s (0.26…0.61) — no float,
no pause. Knees 43…72° (L) / 46…64° (R): softly flexed throughout, never a column.

### PELVIS / TORSO / HEAD (bounded 3-candidate facing study, lower body identical, judged on the real Player)
| candidate | yaw dial | authored hip line | authored shoulders | runtime pelvis rel. root | runtime shoulders rel. root | boots L / R |
|---|---|---|---|---|---|---|
| A square | 0 | +3.8° | −1.1° | −1.7° | −6.8° | −24.9 / −19.8 |
| **B = Back01** | **+8** | **−9.2°** | **−5.9°** | **−14.9°** | **−11.7°** | **−12.7 / −6.1** |
| C left-open | −8 | +16.8° | +3.7° | +11.4° | −2.0° | −37.1 / −33.5 |

B keeps the pelvis bladed slightly RIGHT — the direction of the approved idle's own blade (idle −34.5° / −39.4° by
the same measure) — so the retreat starts from the fighting stance instead of squaring or opening left, and its boots
are the most forward-pointing of the three. Head vs the v012 reference: **−0.1°** (−4.5…+4.3) after the −5° dial.
Root yaw 354.29°, constant, in every session. Idle → Back therefore swings the pelvis ~20° and the shoulders ~28°
(Left v002: 54° / 41°) — the smallest facing change in the family.

### SWORD / SHOULDER
Sword-hand path 255 mm / cycle (Left 172, BackLeft 306, ForwardLeft 473), blade calm, minimum sword-pivot to
upper-chest 459 mm (no collision), Right Shoulder Down-Up 0.093…0.105 (healthy, no compression), armGain 0.25.

### FOOT CONTACT AUDIT (`LocomotionFootContactAudit`, 240 samples, rig-calibrated)
| leg | knee min | reach max | % below 15° | pitch vs flat | heel / toe min | support | flat | buried | support yaw drift |
|---|---|---|---|---|---|---|---|---|---|
| Left | 43.2° | 0.930 | 0 | −6…+2° | +7 / −4 mm | 63 % | 61 % | 0 % | 7.8° |
| Right | 45.6° | 0.922 | 0 | −6…+2° | +5 / −4 mm | 63 % | 63 % | 0 % | 9.9° |

Planted-foot yaw drift 8–10° (Left v002 0.5 / 17, BackLeft 20 / 3.6) — inside the family watch band.

### NATIVE SPEED / CYCLE / CADENCE
True travel heading **180.0°** (both stance tracks +Z at +0.436 m/s in the root frame; lateral 0.000). Native planted-foot
speed **0.463 m/s** (220 mm / (0.82 s × 0.58); raw-clip stance-window velocity +0.4627 / +0.4625 m/s; runtime planted
feet move 7–9 mm/s along travel at that rating, i.e. within 2 %). Cycle 0.82 s, cadence 146 spm, step length 220 mm
per foot (380 mm between consecutive footfalls).

### SPEED STUDY (real Player, lock-on, Warrior Base loadout, research override into the 170.2° ring slot, research
GuardGait rating that slot 0.463; movement through the status multiplier; MaxStableMoveSpeed 2 verified in every diag)
| candidate | playback | world speed | cadence | 0.25 s | 0.5 s | 1.0 s | stride error | rel-root pelvis / shoulders |
|---|---|---|---|---|---|---|---|---|
| SLOW | 0.851× | 0.394 m/s | 125 spm | 0.10 m | 0.20 m | 0.39 m | 0.0 mm/s | −14.9 / −11.6 |
| **MEDIUM (= native)** | **1.000×** | **0.463 m/s** | **146 spm** | **0.12 m** | **0.23 m** | **0.46 m** | 0.0 mm/s | −14.9 / −11.7 |
| BRISK | 1.150× | 0.532 m/s | 168 spm | 0.13 m | 0.27 m | 0.53 m | 0.0 mm/s | −14.9 / −11.7 |
| current gameplay 180° (comparison) | 1.421× | 0.658 m/s | 208 spm | 0.16 m | 0.33 m | 0.66 m | 0.0 mm/s | −15.0 / −11.7 |

Study-rig note: the override slot sits at 170.2° while the clip travels at 180°, so the feet carry a 9.8° sideways
component at runtime (0.463 × sin 9.8° = 0.079 m/s, measured 0.078–0.081); along travel they are planted. A true 180°
slot removes it; the speeds, cadences and body angles above are unaffected.

### SELECTED PRESENTATION (my read; human review decides)
**MEDIUM — 1.00× / 0.463 m/s / 146 spm.** The retreat reads as deliberate ground-giving: each backward landing is
visible, the pelvis transfers, and it sits 13 % slower than BackLeft (0.535) and 15 % slower than Left (0.546) —
"probably slower than the others" without becoming a cinematic walk (23 cm in half a second). BRISK (0.532, 168 spm)
matches BackLeft's speed and still reads as steps; the current 1.42× is the frantic backpedal the brief warns of. The
likely production data would be −180 (and +180) native 0.463 with a gameplay scale of ≈0.405 (0.463 / 1.144).

### BACKLEFT → BACK FAMILY CONTEXT
`Back01_C_BackLeft_Back_BackLeft.mp4` — BackLeft at its production 0.535 / 0.904× and Back01 at 0.463 / 1.00× in one
run (research gait carrying both ratings, no global multiplier). Boundary scan: foot speeds 1.1–1.9 m/s (steady
2.7–3.5), knee rates 137–441°/s (steady 362), pelvis-height rate ≤ 1.28 (steady 1.28), sword ≤ 1.35 (steady 1.35),
minimum foot gap 182 mm — no teleport, snap or pop. Body language: pelvis +30.6° → −14.9° (through square), shoulders
+5.2° → −11.7°, root constant — the fighter turns back toward his own guard as the retreat becomes straight, never away
from the opponent.
`Family_Forward_ForwardLeft_Left_BackLeft_Back01_gameplaySpeeds.mp4` — 2×3 grid: Forward (1.144 / 0.854×),
ForwardLeft (0.987 / 0.796×), Left v002 (0.546 / 1.100×), BackLeft (0.535 / 0.904×), Back01 (0.463 / 1.000×).

### LIKELY PHASE / CYCLE OFFSET
Back01 lands the right foot at 0.42 and the left at 0.92 of its cycle; BackLeft (co 0.35) lands left at tree time 0.04
and right at 0.54. Aligning the left landings gives **cycleOffset ≈ 0.88** (right landing then at 0.54 — exact match).
Not written anywhere.

### TECHNICAL QA
Legality PASS (24 samples/span, 0 flattenings), temporal 0 hold snaps (largest delta R knee 3.6° @0.33), support
schedule 60 / 60 % with 19 % double support, swing 133 mm peak with the toe bone never below 47 mm (flat 53),
deterministic (102 bindings, max |Δ| 0), root canonical 0.00° / 0.00 mm, fully baked in-place contract. Generator
additions inert at defaults (Left02/03 recipes unaffected: strike 0, sway along travel).

### VIDEOS (`Artifacts/AnimationReview/`)
A/B `Back01Speed_bk01_medium.mp4` (native = selected), `Back01Speed_bk01_slow / _brisk / _current.mp4`,
`Back01Speed_2x2_slow_medium_brisk_current.mp4` (top-left slow, top-right medium, bottom-left brisk, bottom-right
current), `Back01Study_bk_A / _B / _C.mp4` (facing study, Idle → Back → Idle), C `Back01_C_BackLeft_Back_BackLeft.mp4`,
D `Family_Forward_ForwardLeft_Left_BackLeft_Back01_gameplaySpeeds.mp4`, `BackLeft_v001_PRODUCTION_pure_1366.mp4`.

### PRODUCTION SAFETY
Not installed. No controller reference to Back01 (or Left02/03/BackLeft01); `GuardGait_Back01Study.asset` is a research
asset under Generated. Study candidates `__bk_A/B/C` removed. Editor: Play checked and exited before every write;
prefab 2 / 4 and the scene Player (no override) verified before the runtime sessions; controller cycle offsets on disk
unchanged (59 zero of 80, production entries 0.92 / 0.35 / 0.34 / 0.35). VerifyProduction TRUE. Tests 48 / 48.
