# `__freshHumanRight_01` — TRUE RIGHT (+90°) research challenger (2026-09-05)

Research only. Not installed. Forward v012 / ForwardLeft v001 / Left v002 / BackLeft v001 / Back v001 / BackRight v001 /
Travel untouched.

### RIGHT01 MOTION DESIGN
A CONTINUOUS FULL-CYCLE LATERAL STRAFE on the approved Left v002 architecture (alternating lanes, 230 mm stride, 0.80 s,
analytic pelvis channels, world-pitch feet, per-leg reach), rotated to +90° — with three deliberate sword-side
differences, none of them a mirror:
1. **Stance stagger keeps the LEFT foot forward** (`lateralStaggerMm −110`): the idle and every rear direction carry the
   left foot ahead, so the right foot leads the strafe laterally while remaining the rear foot of the fighting stance
   (a right-handed fighter side-stepping to the sword side). Left v002 mirrored would have swapped the front foot.
2. **Body blade, not opening**: `yawTowardTravelDeg +8` (Left03 −13). Opening the pelvis toward a right travel adds
   to the idle's right blade, so the dial is smaller and the result is the Back / BackRight relationship.
3. **Mirror-aware pelvis channels** (new inert-default option `lateralMirror`): the alternating sway and yaw swing were
   phased to the side-0 foot, which is the lead on a left travel but the trail on a right travel — the first build
   swayed the pelvis away from the loaded foot and its world speed dipped to 0.016 m/s (a pause). With the mirror the
   pelvis flows toward the loaded lane (world speed 0.16…0.66, no pause). Left03's recipe rebuilds key-identical with the
   option present (102 / 102, max |Δ| 0).

| dial | Right01 | Left v002 |
|---|---|---|
| travelDeg / cycle / drift / swingFrac / laneOut | **+90** / 0.80 / 230 / 0.42 / 70 | −90 / same |
| lateralStaggerMm / lateralMirror | **−110 / true** | +110 / — |
| sway / sink / yawSwing / rollSwing | 30 / 12 / 3 / 3 | same |
| yawTowardTravelDeg / chestFollowFrac / headYawDeg | **+8 / −0.03 / −5** | −13 / −0.03 / +7 |
| strike / swingToesUp / worldPitch / reachMargin | 0 / 4 / true / 20+8 | same |
| stanceKnee / minKnee / armGain | 38 / 15 / 0.3 | same |

### RIGHT / LEFT LEG STRATEGY
Right foot (lead, travel side) swings [0, 0.42) and lands at 0.40 of the cycle, accepts weight (12 mm sink trough 18 %
into the stance), supports and drives; the left foot swings [0.5, 0.92), lands at 0.90 and takes an equal 230 mm
lateral step — a real recovery step, not a close. Stance 62 % per foot, double support 23 %. RIGHT STEP → LEFT STEP →
RIGHT STEP with no pause.

### FOOT LANES / NO CROSSOVER
Root-frame right-minus-left 119…517 mm (lanes ±44…274 mm), never below 119 (Left v002: 109); the left foot stays
245 mm forward of the right (the fighting stagger); boot separation **273…572 mm** (Left v002 190…530). Runtime
minimum boot separation 273 mm in Right, 183 mm through the BackRight ↔ Right blend. No crossover, no interception.

### RIGHT FOOT ORIENTATION
Ankle→toe world heading of the right boot −10…+8°, mean **−0.1°** (points forward through its stance; Left v002's
lead boot −13°), support yaw drift **5.3°** authored / 2.6–3.4° at runtime — the most stable planted foot in the
family (Left v002 right foot 15–17°, BackRight 17°). No inward twist, no toe-out; swing reorientation ±10°.
The left (trail) boot heads −13° with 12° support drift (runtime 10–11°) — inside the family watch band.

### STANCE WIDTH
Lateral lanes 119…517 mm apart, stagger 245 mm, separation 273…572 mm: a controlled base, slightly wider than Left
because the sword-side blade keeps the left foot forward; no lunge (Left01's step-close reached 790).

### WEIGHT TRANSFER / PELVIS CONTINUITY
Pelvis 709…746 mm (rhythm 36.6), sway 30 mm over the loaded lane, world pelvis speed at native **0.16…0.66 m/s** —
continuous; contact → acceptance → propulsion → opposite contact with 23 % overlap.

### PELVIS / TORSO / HEAD
Root 0.00° / 0.00 mm in the clip; runtime root 354.29° constant. Facing study (lower body identical, real Player):

| candidate | yaw dial | runtime pelvis rel. root | runtime shoulders |
|---|---|---|---|
| A square | 0 | −2.1° | −6.9° |
| **B = Right01** | **+8** | **−15.0°** | **−11.7°** |
| C stronger blade | +14 | −24.9° | −15.3° |

B is chosen: the modest right blade the approved Back (−15 / −12) and BackRight (−21 / −14) already use, smaller in
magnitude than Left v002's +19.5° opening, and the idle's own blade direction — legs travel right, body stays on the
threat. A is the square alternative for the reviewer. Head vs the v012 reference −0.1° (−3.8…+3.6).

### SWORD / SHOULDER
Right Shoulder Down-Up 0.092…0.106 (Left v002 0.093…0.105 — identical band, no compression), sword pivot never
closer than 458 mm to the upper chest, sword-hand path 178 mm / cycle (Left 172), armGain 0.3. The pelvis blade does
not drag the shoulder: shoulder line −4…−8° through the cycle.

### FOOT CONTACT AUDIT (`LocomotionFootContactAudit`, 240 samples, rig-calibrated)
| leg | knee min | reach max | % below 15° | pitch vs flat | heel / toe min | support | flat | buried | support yaw drift |
|---|---|---|---|---|---|---|---|---|---|
| Left | 36.0° | 0.951 | 0 | 0…+4° | +1 / +1 mm | 62 % | 62 % | 0 % | 12.0° |
| Right | 42.2° | 0.933 | 0 | 0…+4° | −1 / +1 mm | 63 % | 62 % | 0 % | 5.3° |

Both support knees softly flexed (runtime minimum 25°); no burial; planted drift at runtime 9–10 mm at every rate =
the 3.7° study-slot slide (0.496 × sin 3.7° = 0.032 m/s over a 0.33 s stance), along travel planted; Left v002 in the
same harness reads 1–2 mm on its true slot.

### NATIVE SPEED / CYCLE / CADENCE
Heading **+90.0°** (both stance tracks, across-travel slide 0); native planted-foot speed **0.496 m/s** (= Left v002),
cycle 0.80 s, 150 spm, stride 230 mm per foot.

### SPEED STUDY (real Player, lock-on, loadout, research override into the +86.3° ring slot rated 0.496 / 0.434;
MaxStableMoveSpeed 2 in every diag; no compensation of the baseline)
| candidate | playback | world speed | cadence | 0.25 s | 0.5 s | 1.0 s | stride error | pelvis / shoulders |
|---|---|---|---|---|---|---|---|---|
| SLOW | 0.851× | 0.422 m/s | 128 spm | 0.11 m | 0.21 m | 0.42 m | 0.0 mm/s | −15.0 / −11.7 |
| MEDIUM (native) | 1.001× | 0.496 m/s | 150 spm | 0.12 m | 0.25 m | 0.50 m | 0.0 mm/s | −15.0 / −11.7 |
| **SELECTED (= Left's presentation)** | **1.101×** | **0.546 m/s** | **165 spm** | **0.14 m** | **0.27 m** | **0.55 m** | 0.0 mm/s | −14.7 / −11.6 |
| BRISK | 1.151× | 0.571 m/s | 173 spm | 0.14 m | 0.29 m | 0.57 m | 0.0 mm/s | −14.9 / −11.7 |
| current +90° gameplay (comparison) | 1.291× | 0.641 m/s | 194 spm | 0.16 m | 0.32 m | 0.64 m | 0.0 mm/s | −14.9 / −11.7 |

### SELECTED PRESENTATION (my read; human review decides)
**1.10× / 0.546 m/s / 165 spm — identical to the approved Left v002 presentation**, so the lateral pair reads at one
cadence in both directions; the alternating steps stay individually readable and the right foot lands flat and quiet.
Brisk (1.15×, 173 spm) is the upper edge; the current 1.29× is the rushed shuffle. Likely production data: +90 native
0.496, gameplay scale 0.477 (= Left).

### LEFT vs RIGHT CONTEXT
`Right01_C_Left_v002_vs_Right01.mp4` (Left production 0.546 / 1.10× | Right01 0.546 / 1.10×, same Player, camera,
loadout, lock-on, writers, 60 fps). Same native speed, cycle, stride, cadence and sword restraint; the asymmetries are
the intended ones: stagger keeps the left foot forward in both (Left's lead foot is the front foot, Right's lead foot
is the rear foot), pelvis +19.5° toward travel on Left vs −15° blade on Right, shoulders +1° vs −12°, base 190…530 vs
273…572 mm.

### BACKRIGHT → RIGHT CONTEXT
`Right01_D_BackRight_Right_BackRight.mp4` (BackRight production 0.463 / 1.00× ↔ Right01 0.496 / 1.00× through the
research gait): boundary foot speeds ≤ 1.06 m/s (steady 2.6–3.7), knee rates 199–288°/s (steady 285), pelvis-height
rate ≤ 0.71, sword ≤ 0.62, minimum boot separation 183 mm — no pop; pelvis −21.5° → −15.0°, shoulders −14 → −12, root
constant: retreat → lateral with no facing reversal. `Right01_E_BackRight_Right_family.mp4` side by side.

### LIKELY CYCLE OFFSET
Right01 lands left at 0.900 and right at 0.400 of its cycle. BackRight (co 0.87) lands at tree time L 0.025 / R 0.525
→ Right co **0.87** (Δ 0.005); Left v002 (co 0.34) lands at 0.060 / 0.560 → within 0.035 of the same frames. Not written.

### TECHNICAL QA
Legality PASS (0 flattenings), temporal: one flag — the right ankle holds a flat pitch through the last 0.10 s of its
stance and releases 9.3° at 108°/s (1.8° per frame) into the swing (world-pitch target flat → toes-up); Left v002 audits
0 holds with the same profile. Not a visible snap; noted. Support schedule 62 / 62 % with 23 % double support, swing
143 mm peak with the toe bone never below 52 mm, seam clean, deterministic (102 bindings, max |Δ| 0), root canonical,
fully baked in-place contract. Generator addition `lateralMirror` inert at default (Left03 recipe key-identical).

### VIDEOS (`Artifacts/AnimationReview/`)
A `Right01Speed_rt01_medium.mp4` (native) · B `Right01_B_selected_1p10x.mp4` · `Right01Speed_rt01_slow / _brisk / _current.mp4`,
`Right01Speed_2x2_slow_medium_brisk_current.mp4` · `Right01Study_rt_A / _B / _C.mp4` (facing study, pre-mirror sway) ·
C `Right01_C_Left_v002_vs_Right01.mp4` · D `Right01_D_BackRight_Right_BackRight.mp4` · E `Right01_E_BackRight_Right_family.mp4` ·
`Left_v002_PRODUCTION_ctx_for_Right01.mp4`.

### PRODUCTION SAFETY
Not installed. No controller reference to Right01 (or any research clip); `GuardGait_Right01Study.asset` is a research
asset under Generated; study candidates removed. Editor out of Play before every write; prefab 2 / 4 and scene Player
(no override) verified before the runtime sessions; production clips unchanged by hash; controller cycle offsets on
disk unchanged. VerifyProduction TRUE. Tests 51 / 51. Git index left untouched; nothing added or committed.
