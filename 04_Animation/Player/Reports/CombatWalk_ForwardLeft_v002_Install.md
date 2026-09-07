# `Sword1H_WalkForwardLeft_v002` — production install of the approved FL03 leg repair (2026-09-07)

Surgical replacement of ForwardLeft_v001 in the −45° child of `Light_Walk8`. Production integration only; the approved
artistic motion is preserved, the lower body is the FL03 repair.

### FORWARDLEFT v002 ASSET
`Assets/Bravehood/Animation/Generated/Sword1H_WalkForwardLeft_v002.anim` (guid 94278d5b…), a `CopyAsset` promotion of
`__freshHumanForwardLeft_03` — not regenerated (the FL02 / v001 recipe is not reconstructible; FL03 itself is the
source authority). Contract: loopTime on, loopBlend off, orientation / Y / XZ baked, Based Upon Original, heightFromFeet
off, 0.80 s, canonical root (0.00° / 0.00 mm), controller-driven translation. `Sword1H_WalkForwardLeft_v001` (guid
910fd344…) preserved on disk, unreferenced by the Light ring (its `_Travel` copy still serves Travel_Walk8).

### FL03 SOURCE EQUIVALENCE
102 / 102 bindings, 0 missing; max |Δ value| 0, |Δ time| 0, |Δ tangent| 0; sampled feet / head / root within 0.0004 mm
over 120 samples. Key-identical.

### V001 → V002 CURVE DIFFERENCES
**91 / 102 curves bit-identical** to v001. Changed (11): Left / Right Foot Up-Down (1.04 / 0.72), Left / Right Upper
Leg Front-Back (0.32 / 0.39), Left / Right Upper Leg In-Out (0.14 / 0.14), Left / Right Lower Leg Stretch (0.45 / 0.50),
RootT.y (0.048 — the pelvis rides ~24 mm lower for the reach margin), Left / Right Upper Leg Twist In-Out (own key
layout, the stance-heading hold). Every arm, shoulder, spine, chest, neck, head, root-XZ and root-rotation curve is
unchanged.

### CONTROLLER SLOT
Ring read from disk before the edit: child 6 `Sword1H_WalkForwardLeft_v001 @ (−0.7071, 0.7071) ts 1 co 0.92`. Only its
motion reference was replaced → `Sword1H_WalkForwardLeft_v002`; position, timeScale and mirror untouched. On disk
after: eight authored children, one per canonical heading, no research guid, v001's guid absent from the file.
Travel_Walk8 block byte-identical to before the edit.

### CYCLE OFFSET
Measured on the finished production asset with the install convention (foot bone lowest, 12 mm, 400 samples): v002
lands L 0.020 / R 0.505 (v001 0.988 / 0.543). Neighbours in tree phase: Forward v012 L 0.088 / R 0.623, Left v002
L 0.060 / R 0.560. co 0.92 → v002 at 0.100 / 0.585, worst deviation **0.040**; 0.93 → 0.048; 0.91 → 0.050. The current
offset is the measured best and the zero-change option: **co 0.92 kept**.

### GUARDGAIT
−45 : **1.261 m/s** native (v002's measured stance tracks 1.271 / 1.251; runtime 0.986 at 0.782× → 1.261), replacing
1.24. Table −135 0.592 / −90 0.496 / −45 **1.261** / 0 1.34 / 45 1.208 / 90 0.496 / 135 0.463 / 180 0.463; scales
unchanged. No playback encoded. TravelGait untouched (−45 = 1.24 for the v001 Travel copy).

### GAMEPLAY SPEED / MOTIONSPEED
`gameplayScale` −45 = 0.862, unchanged: 2.0 × 0.88 × 0.65 × 0.862 = 0.986 m/s → 0.986 / 1.261 = **0.782×** (was 0.795× on
v001's 1.24). World presentation is the authority; the diagonal pair still shares 0.986 m/s.

### RUNTIME (real Player, production controller and GuardGait, no override, no pref)
| condition | load | world | MotionSpeed | cadence | 0.25 / 0.5 / 1.0 s | stride error | planted drift L / R |
|---|---|---|---|---|---|---|---|
| gameplay loadout | 0.880 | **0.986 m/s** | **0.782** | **117 spm** | 0.25 / 0.49 / 0.99 m | −0.0 mm/s | 3 / 6 mm |
| unarmoured | 1.000 | 1.121 m/s | 0.889 | 133 spm | 0.28 / 0.56 / 1.12 m | +0.0 mm/s | 3 / 6 mm |

Travel −45.0° relative to the root, root 354.29° constant, the production clip at weight 1.000; MaxStableMoveSpeed 2,
note empty, one Player in the loaded scene.

### KNEE RESERVE
RAW authored 12.1° / 23.2° (reach 0.994 / 0.980); runtime 12.1° / 23.2° loadout, 12.1° / 23.5° unarmoured (v001
runtime 4.2° / 1.9°). Support geometry from per-leg reach planning against the asymmetric rig; no cosmetic bend.

### PLANTED YAW
RAW whole-support drift 0.6° / 1.7°; runtime **0.1° / 0.4°** loadout, 0.1° / 0.1° unarmoured (v001 19.7° / 8.6°).
Boot headings on the travel line (−54 / −53° mean).

### FOOT CONTACT
RAW: heel +6 / +5, toe +2 / +2 mm of the calibrated flat, sole flat 44 / 45 %, heel-off 18 / 16 %, **buried 0 %**
(v001 −48 / −29 mm, 32 % / 6 %). Runtime toe bone 33 / 34 mm loadout, 39 / 38 unarmoured (v001 12 / 17); swing clean
single arcs (peak 64 mm, mid-swing minimum 23 / 21). FootIK moved planted feet by terrain-scale amounts only; the
raw audit already passes.

### BODY FACING / UPPER BODY PRESERVATION
Runtime pelvis **+26.7°** (+15.7…+36.7), shoulders **+4.4°** (+1.3…+7.4), head on the target, root constant — v001
+26.6 / +4.4. Arm, shoulder, head and sword curves bit-identical; sword pivot ≥ 452 mm from the upper chest; no
collision. The ForwardLeft opening is untouched by design (family facing pass later).

### FORWARD → FORWARDLEFT → LEFT
| segment | v001 baseline | **v002** |
|---|---|---|
| Forward | 1.144 m/s, pelvis −1.9 / −6.6 | same |
| ForwardLeft | 0.986 @ 0.795×, 119 spm, +25.6 / +4.0 | 0.986 @ 0.782×, 117 spm, +25.7 / +3.9 |
| Left v002 | 0.546 @ 1.09×, +18.5 / +0.9 | same |
| boundary F→FL foot / hips / sword | 2.63 / 1.32 / 1.26 m/s | 2.63 / 1.32 / 1.26 |
| boundary FL→L | 2.25 / 1.11 / 1.09 | 2.25 / 0.89 / 0.93 |
| minimum boot separation | 146 mm | 158 mm |

No transition spike, stance width unchanged, pelvis rhythm 63 vs 60 mm. `..._D_Forward_ForwardLeft_Left.mp4`.

### FORWARDLEFT v002 vs FORWARDRIGHT
`..._E_ForwardLeft_v002_vs_ForwardRight_v001.mp4`, both production, both 0.986 m/s:

| | ForwardLeft v002 | ForwardRight v001 |
|---|---|---|
| playback / cadence | 0.782× / 117 spm | 0.816× / 122 spm |
| runtime knee reserve L / R | 12.1° / 23.2° | 21.5° / 17.8° |
| runtime planted yaw | 0.1° / 0.4° | 0.3° / 4.9° |
| runtime toe bone minimum | 33 / 34 mm | 34 / 34 mm |
| pelvis / shoulders | +26.7 / +4.4 | −19.3 / −13.8 |
| sword pivot from chest | ≥ 452 mm | ≥ 455 mm |

The lower-body quality gap is closed; the body-language asymmetry (opening left vs blading right) is the intended
one and untouched.

### IDLE → FORWARDLEFT
Peak entry yaw 492 / 316°/s (v001 489 / 317), exit 328 / 221 (337 / 214), foot reposition 2.74 m/s inside the
crossfade (v001 2.74). Not worse; recorded for the family-wide Idle / body-facing pass. CombatIdle untouched.

### FAMILY REGRESSION
MD5 of Forward v012, ForwardRight v001, Right v001, BackRight v001, Back v001, BackLeft v001, Left v002,
ForwardLeft v001, the four `_Travel` copies, `TravelGait_Knight1H` and `Player.prefab` identical before and after (14
files); Travel_Walk8 serialized block identical. Eight-direction production sweep with v002 installed
(`..._G_Family8_sweep.mp4`): every heading plays its own child at ≥ 0.992, −45 at 0.986 / 0.782× / 117 spm with
planted frames 43 / 39 % (v001 38 / 35 %), boundaries ≤ 2.57 m/s foot, separation ≥ 162 mm, root constant.

### VIDEOS (`Artifacts/AnimationReview/`)
A `ForwardLeft_v002_PRODUCTION_A_lockon_forwardleft.mp4` · B `..._B_unarmoured.mp4` · C `..._C_v001_vs_v002_0p986.mp4` ·
D `..._D_Forward_ForwardLeft_Left.mp4` · E `..._E_ForwardLeft_v002_vs_ForwardRight_v001.mp4` ·
F `..._F_Idle_ForwardLeft_Idle.mp4` · G `..._G_Family8_sweep.mp4` · `ForwardRight_v001_PRODUCTION_pair_for_ForwardLeft_v002.mp4`.

### TESTS
`U2_ProductionForwardLeft_v002_Contract` (true −45 slot references v002, key-identical to FL03, baked contract,
cycleOffset shared with Forward, GuardGait −45 = 1.261 native, scale 0.862, v001 unreferenced, no research clip, no
legacy −52.8 entry); the seven other contracts' ring dictionaries and rating assertions updated (v002, 1.261);
`AD2_LightWalk8_AllEightCanonicalHeadings` unchanged and passing. Suite **54 / 54**, no regressions.

### PRODUCTION STATE
Light_Walk8: Forward v012 (0.92) · ForwardRight v001 (0.94) · Right v001 (0.87) · BackRight v001 (0.87) · Back v001
(0.87) · BackLeft v001 (0.35) · Left v002 (0.34) · **ForwardLeft v002 (0.92)**. GuardGait −45 1.261 / 0.862. Travel,
CombatIdle, FootIK, grip, Player 2 / 4 untouched. No research clip referenced; VerifyProduction TRUE. Editor out of
Play before every write; controller, GuardGait and prefab read back from disk. Git index untouched; nothing added or
committed.
