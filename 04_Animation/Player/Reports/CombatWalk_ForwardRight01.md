# `__freshHumanForwardRight_01` — TRUE FORWARDRIGHT (+45°) research challenger (2026-09-06)

Research only. Not installed. Forward v012 / ForwardLeft v001 / Left v002 / BackLeft v001 / Back v001 / BackRight v001 /
Right v001 / Travel untouched. VerifyProduction TRUE, tests 52 / 52, Git index untouched.

### FORWARDRIGHT01 MOTION DESIGN
An ADVANCING DIAGONAL on the Forward v012 architecture (alternating heel-to-toe steps, 0.80 s cycle, analytic
pelvis channels, world-pitch planted feet, per-leg reach margins) rotated to +45°, built as its own gait — not a
mirror of ForwardLeft. Three sword-side decisions:
1. **The pelvis blades toward the threat, not toward travel** (`yawTowardTravelDeg +11`, ForwardLeft used −45°-side
   opening). A +45 advance with the pelvis square cannot plant the right boot: at yaw 0 and 8 the right toe buried
   36–37 mm with 0 % flat contact and 65 % heel-off (the shorter right leg reaching forward-right from a square hip).
   Yaw 11 is the smallest blade that keeps the right boot flat; the facing study brackets it (14, 18).
2. **Right boot heading held through its whole stance** (`stanceYawStabilize 1`, `yawReleaseSpan 0.16`): without the
   hold the right boot pivoted 80° in stance on this diagonal (noYaw diagnostic). The hold now survives the loop
   seam and releases over the forefoot pivot — two generator repairs below.
3. **Sword-side reach** (`rightReachExtraMm 8`, `reachMarginMm 20`, `strideScale 0.90`, `stanceWidenMm 15`):
   both legs stay flexed (21.5° / 29.6° minimum) and the boots never cross.

| dial | ForwardRight01 | ForwardLeft v001 |
|---|---|---|
| travelDeg / cycle / samples | **+45** / 0.80 / 48 | −45 / 0.80 / 48 |
| yawTowardTravelDeg / chestFollowFrac / headYawDeg | **+11 / 0.034 / −5** | (opening toward travel) |
| strideScale / stanceWidenMm | 0.90 / 15 | — |
| minKnee / stanceKnee / swingHeightScale / armGain | 15 / 32 / 0.62 / 0.4 | — |
| plantedFootRoll / reachMarginMm / rightReachExtraMm | true / 20 / 8 | not installed |
| stanceYawStabilize / yawReleaseSpan | **1.0 / 0.16** | — |

### GENERATOR REPAIRS (this milestone, all inert at default; production recipes do not use the stabiliser)
- **Jacobian probe at a saturated muscle**: `StabilizeStanceYaw` probed +0.01 and clamped to ±1; a muscle at the
  limit gave a zero step, a division by zero and NaN in six curves (candidates A / B). The probe now steps toward
  the side with room, and a non-finite solve keeps the last finite iterate. Yaw-14 rebuilds key-identical (102
  bindings, max |Δ| 0) before and after.
- **Heading reference resolved before the frame loop**: the right foot's stance wraps the loop seam (lands at 0.51,
  still planted at 0.0), so a reference set inside the loop left cycle 0.00–0.16 unheld: the thigh twist ramped
  through stance and dropped 0.35 muscle in one key at the seam (boot yaw 2700°/s). A pre-pass resolves both
  references from the same sole-load window; the left leg is unchanged (max |Δ| 0).
- **Release ends where the toes lift, not at the nominal phase**: this gait lifts both feet at foot-phase ~0.5 while
  the window ran to 0.66; holding a heading on a dangling toes-down foot twisted the thigh at 6800°/s. Lift is the
  toe bone 12 mm above its planted height (heel-off keeps the forefoot pivot), the span is unchanged.
- `yawReleaseSpan 0.08 → 0.16` on this recipe: right-foot angular peak 2399 → 955°/s (Forward v012 850, ForwardLeft
  1036), thigh 2450 → 902°/s (v012 1036), twist key step 0.169 → 0.064. A dial, not a code change.

### LEG STRATEGY
Left foot lands at 0.03 of the cycle, planted 0.04–0.69 (64 %), stance track 493 mm along travel at 43.5°; right
foot lands at 0.51, planted 0.53–0.99 (47 %), 460 mm at 47.4°. Double support 20 %. LEFT STEP → RIGHT STEP with
966 mm per foot per cycle at native. Both are real weight-bearing steps — knee 21.5° / 29.6° minimum, never straight,
no toe-drag beyond the family plunge (below).

### FOOT TRACKS / LANES
Lanes straddle the travel line at −88 mm (left, −95…−82) and +89 mm (right, +75…+94): 165–181 mm apart across travel
at every phase, boot separation **165…604 mm**, root-frame right-minus-left −168…534. Runtime minimum boot
separation 165–177 mm in every run, 166–273 mm through the Forward ↔ FR ↔ Right blends. No crossover, no interception.

### RIGHT FOOT ORIENTATION
Planted heading +10…+13° (mean 10.5, toward travel while the pelvis blades the other way), support yaw drift **0.5°**
authored / **0.2–0.3°** at runtime on the true +45 slot (family watch band 15–17°; ForwardLeft v001 runtime 8.6°).
Swing: the boot hangs toes-down (−49° pitch, Forward v012 −44°, ForwardLeft −48/−50°: the known family plunge) and
its ankle→toe heading swings to −83° while near-vertical — a projection of the plunge, not a whip: right-foot bone
angular peak 854°/s (v012 850). Right toe bone dips to 51 mm at toe-off against a 53 mm flat (2 mm skim, 0 % buried).

### STRIDE / STANCE WIDTH
Stride solved from the motion, not copied: 966 mm per foot per cycle at native (Forward v012 ~1070, Right's lateral
230); stance tracks 493 / 460 mm along travel. Feet 165–181 mm apart across the line of travel, a slightly narrower base than Right v001's lateral 273 mm minimum
and wider than ForwardLeft v001's runtime 159 mm; no lunge, no straddle.

### WEIGHT TRANSFER
Pelvis 700…758 mm (rhythm 58 mm; v012 70, ForwardLeft ~57), 24 mm sway across travel, world pelvis speed at native
0.87…2.04 m/s with no pause; contact → acceptance → propulsion → opposite contact with 20 % overlap.

### PELVIS / TORSO / HEAD
Root 0.00° / 0.00 mm in the clip; runtime root 354.29° constant in every run. Facing study on the real Player,
lower body identical, lock-on, gameplay camera:

| candidate | yaw dial | runtime pelvis rel. root | runtime shoulders | right boot angular peak |
|---|---|---|---|---|
| **D = ForwardRight01** | **+11** | **−19.3°** (−29.9…−8.9) | **−13.7°** (−16.7…−10.7) | 854°/s |
| C | +14 | −24.1° | −15.6° | 955°/s |
| E stronger blade | +18 | −30.5° | −18.1° | 858°/s |

D is chosen: the modest right blade — 4.6° more than Right v001 (−14.7 / −11.6) so the sword side leans into the
diagonal and relaxes into the strafe, far from the idle's −34.5 / −39.4. Progression measured in one session:
Forward v012 −3.6 / −7.3 → **ForwardRight −20.1 / −13.6** → Right v001 −14.9 / −11.7, root constant throughout.
The body never rotates toward travel. Head vs the v012 reference +2.0°. Pelvis-to-shoulder separation 7.3° mean
(hip line −24…−3, shoulders −11…−5) — connected, not counter-rotated. Candidates A (yaw 0) and B (yaw 8) are
contract failures (right toe −37 mm, 0 % flat) and were not filmed.

### SWORD / SHOULDER
Right Shoulder Down-Up 0.084…0.114 (Forward family band), sword pivot never closer than 455 mm to the upper chest,
sword-hand path 298 mm / cycle (armGain 0.4, the forward-family carry). The blade does not drag the shoulder.

### FOOT CONTACT AUDIT (`LocomotionFootContactAudit`, 240 samples, rig-calibrated)
| leg | knee min | reach max | pitch vs flat | heel / toe min | support | flat | heel-off | buried | support yaw drift |
|---|---|---|---|---|---|---|---|---|---|
| Left | 21.5° | 0.982 | 0…+4° | +1 / −4 mm | 63 % | 37 % | 18 % | 0 % | 3.7° |
| Right | 29.6° | 0.967 | 0…+4° | +2 / +1 mm | 59 % | 44 % | 3 % | 0 % | 0.5° |

Runtime (true +45 slot, FootIK on): planted mid-stance drift 4 / 2 mm (max 5 / 2), support yaw drift 0.3 / 0.3°,
knee minimum 21.5 / 20.7°, toe bone minimum 44 / 40 mm. ForwardLeft v001 in the same harness: knee 4.2 / 1.9°, yaw
drift 19.7 / 8.6°, toe 12 / 17 mm — the diagonal pair is now asymmetric in quality as well as design (watch item).

### NATIVE SPEED / CYCLE / CADENCE
Heading **+45.0°** (both stance tracks, across-travel slide ±27 mm/s), native planted-foot speed **1.208 m/s**
(runtime 1.210 at MotionSpeed 1.000, stride error −0.1 mm/s), cycle 0.80 s, 150 spm, 966 mm per foot.

### SPEED STUDY (real Player, lock-on, loadout 0.880, research controller slot (0.7071, 0.7071) + research gait
rated 1.208 native; MaxStableMoveSpeed 2 in every diag; no baseline compensation)
| candidate | playback | world speed | cadence | 0.25 s | 0.5 s | 1.0 s | stride error | planted drift L / R | pelvis / shoulders |
|---|---|---|---|---|---|---|---|---|---|
| SLOW | 0.800× | 0.968 m/s | 120 spm | 0.24 m | 0.48 m | 0.97 m | +1.5 mm/s | 5 / 6 mm | −19.1 / −13.7 |
| **SELECTED (= ForwardLeft's presentation by data)** | **0.816×** | **0.986 m/s** | **122 spm** | **0.25 m** | **0.49 m** | **0.99 m** | −0.1 mm/s | 4 / 4 mm | −19.3 / −13.8 |
| MEDIUM | 0.850× | 1.029 m/s | 128 spm | 0.26 m | 0.51 m | 1.03 m | +1.6 mm/s | 5 / 4 mm | −19.4 / −13.7 |
| BRISK | 0.900× | 1.089 m/s | 135 spm | 0.27 m | 0.54 m | 1.09 m | +1.7 mm/s | 5 / 4 mm | −18.8 / −13.7 |
| native (reference) | 1.000× | 1.210 m/s | 150 spm | 0.30 m | 0.61 m | 1.21 m | −0.1 mm/s | 4 / 2 mm | −19.3 / −13.7 |
| unarmoured, native gait | 1.137× | 1.375 m/s | 171 spm | 0.34 m | 0.69 m | 1.38 m | +2.1 mm/s | 4 / 3 mm | −19.5 / −13.8 |
| current +45 gameplay (Forward 0.44 + Right 0.44 + parked mocap 0.12) | 0.920× | 0.845 m/s | 134 spm | 0.21 m | 0.42 m | 0.84 m | −267 mm/s | **no planted foot** (min foot speed 0.24 m/s) | — |

### SELECTED PRESENTATION (my read; human review decides)
**0.816× / 0.986 m/s / 122 spm — the ForwardLeft v001 presentation by data** (`gameplayScale 0.862` at +45, the −45
value), so the diagonal pair reads at one speed in both directions while the steps stay individually readable.
Medium (0.85×) is the upper comfortable edge; brisk starts to hurry the right plant. The current +45 gameplay is a
skating Forward/Right blend at 0.845 m/s; the challenger is 17 % faster and plants every step. Likely production
data: +45 native 1.208, gameplay scale 0.862.

### FORWARD → FORWARDRIGHT → FORWARD
`ForwardRight01_C_Forward_ForwardRight_Forward.mp4`: Forward 1.144 / 0.854× → FR 0.986 / 0.816× → Forward.
Boundary foot speeds ≤ 2.57 m/s (steady 3.68), hips ≤ 1.31, sword ≤ 1.25 m/s, minimum boot separation 168 mm — no
pop; pelvis −3.6 → −20.1 → −3.6, root constant. Blend sweep 0 → 45° (research slot, co 0.92):

| input | world | MotionSpeed | v012 | FR01 | planted frames L / R | pelvis rhythm |
|---|---|---|---|---|---|---|
| 0° | 1.144 | 0.854 | 1.000 | – | 34 / 34 % | 70 mm |
| 15° | 1.091 | 0.842 | 0.668 | 0.332 | 27 / 15 % | 57 mm |
| 30° | 1.039 | 0.830 | 0.335 | 0.665 | 27 / 18 % | 54 mm |
| 45° | 0.986 | 0.816 | – | 1.000 | 32 / 55 % | 56 mm |

### RIGHT → FORWARDRIGHT → RIGHT
`ForwardRight01_D_Right_ForwardRight_Right.mp4`: Right 0.546 / 1.10× → FR 0.986 / 0.816× → Right. Boundaries
≤ 2.17 m/s foot, ≤ 0.90 hips, ≤ 0.92 sword, separation ≥ 193 mm; pelvis −14.9 → −20.3 → −14.9. Sweep 45 → 90°:

| input | world | MotionSpeed | FR01 | Right | planted frames L / R | pelvis rhythm |
|---|---|---|---|---|---|---|
| 60° | 0.840 | 0.864 | 0.668 | 0.332 | 8 / 24 % | 44 mm |
| 75° | 0.693 | 0.944 | 0.335 | 0.665 | 12 / 11 % | 37 mm |
| 90° | 0.546 | 1.099 | – | 1.000 | 49 / 68 % | 37 mm |

The 45–90 sector slides more than 0–45: its natives differ by 2.2× (1.208 vs 0.496) so the blended stride cannot match
the interpolated world speed mid-sector — the same tree-space limitation every lateral sector has, not a phase error.

### FORWARD → FORWARDRIGHT → RIGHT (combined)
`ForwardRight01_E_Forward_ForwardRight_Right.mp4`: 1.144 → 0.986 → 0.546 m/s, boundaries ≤ 2.57 / 2.22 / 1.59 m/s
foot, separation ≥ 168 mm; pelvis −3.2 → −20.1 → −15.1, shoulders −6.9 → −13.6 → −11.7: square advance → sword-side
lean → strafe with the root never turning. 
### FORWARDLEFT vs FORWARDRIGHT
`ForwardRight01_F_ForwardLeft_v001_vs_ForwardRight01.mp4`: both at
0.986 m/s; ForwardLeft opens the pelvis +26.6° toward travel, ForwardRight blades −19.3° away from it — the intended
asymmetry, not a mirror.

### IDLE → FORWARDRIGHT
Pelvis change from idle +15.2° / shoulders +25.6° (Right 19.8 / 27.8, Left 54 / 41); peak yaw rates through the
crossfade 184 / 195°/s (3.1° per frame), FR → idle 131 / 115°/s, steady 66 / 22°/s; foot reposition at entry 3.71 m/s
maximum inside the 0.25 s crossfade (Right 2.37, Back 3.95) — no single-frame discontinuity.

### LIKELY CYCLE OFFSET
Runtime landings in tree phase (foot bone lowest): Forward v012 L 0.08–0.12 / R 0.63; Right v001 L 0.06 / R 0.555;
Left v002 L 0.08 / R 0.58; ForwardRight01 on the research slot (co 0.92) L 0.125 / R 0.62. **co 0.95** centres it
(L 0.095 / R 0.59: Δ Forward −0.02 / −0.04, Δ Right +0.03 / +0.03). Not written.

### FUTURE SLOT TOPOLOGY
Serialized `Light_Walk8` on disk: 0 Forward_v012 (0, 1) co 0.92 · 1 Right_v001 (1, 0) co 0.87 · 2 BackRight_v001
(0.71, −0.71) · 3 Back_v001 (0, −1) · 4 BackLeft_v001 (−0.71, −0.71) · 5 Left_v002 (−1, 0) · 6 ForwardLeft_v001
(−0.71, 0.71) · **7 `Sword1h_Strafe45LeftLoop` parked at (0, 0) co 0.98** — the only unused family slot. The research
copy proves the install: child 7 → (+0.7071068, +0.7071068) ts 1 gives a true +45 slot (rel-root travel 45.0°, the
clip at weight 1.000, planted drift 2–5 mm) with no other child within 45° of it. Nothing moved in production.

### TECHNICAL QA
Legality PASS (0 flattenings, 0 NaN); semantic support 63 / 59 % with 20 % double support (low sole AND low speed);
swing-foot trajectory: both feet CLEAN SINGLE ARC, 0 reversals, 64 mm peak, mid-swing minimum 24 / 21 mm, touchdown
−192 / −247 mm/s; anatomical rotation at 0.816×: femur twist range 9.7 / 15.2° (max frame step 1.0 / 2.3°, the held
heading), clavicles 0.0°, spine cancellation 0.61, knee-plane max step 3.0 / 4.2°; planted yaw 0.5° / 3.7° authored;
achieved knee reserve 21.5 / 29.6°, per-leg reach 0.982 / 0.967; temporal audit at 0.816× — 0 hold-snap events, largest lower-body frame steps
R ankle 9.9° @0.07 (toe-off) / L knee 8.4° / R knee 6.2°; seam max |first − last| 0.040 muscle (loop-blended);
deterministic (rebuild max |Δ value / tangent| 0; the study clip D and the fresh build are key-identical); root
canonical; baked in-place contract (loopTime, orientation / Y / XZ baked, Based Upon Original, heightFromFeet off).
Family-watch items carried, not corrected here: the swing toe plunge (−49° pitch, 2 mm toe skim at toe-off) shared
with Forward and ForwardLeft; ForwardLeft v001's straight support knees at runtime (4.2 / 1.9°).

### HARNESS FINDINGS (fixed, no production effect)
- A `HideAndDontSave` build holder with an instantiated Player survived into Play: the driver bound to it (motor
  never awake, no diag, Play never ended) and the inventory bootstrap provisioned it instead of the real Player, so
  the loadout read 0.9325 instead of 0.880. Destroyed; every queue now STOPs on a Player outside the loaded scene.
  The driver also tolerates a motor without Awake (transform fallback).
- Lanes: (24, −8) climbs a 0.25 m prop at (26, −5.5); (24, 0) at +45 hits a wall at (27, 3.4). Flat +45 lane used:
  (30, −11) → (34.5, −5.5).

### VIDEOS (`Artifacts/AnimationReview/`)
A `ForwardRight01Study_rt_D.mp4` (native, selected facing) · B `ForwardRight01_B_selected_0p816x.mp4` ·
`ForwardRight01Speed_frv_slow.mp4` (0.80×) / `..._frv_brisk.mp4` (0.90×) / `ForwardRight01Speed_native_unarmoured.mp4` /
`ForwardRight01Speed_current_gameplay_blend.mp4`, `ForwardRight01Speed_2x2_slow_selected_brisk_current.mp4` · facing study `ForwardRight01Study_rt_C / _E.mp4`,
`ForwardRight01Study_3up_D_C_E.mp4` · C `ForwardRight01_C_Forward_ForwardRight_Forward.mp4` ·
D `ForwardRight01_D_Right_ForwardRight_Right.mp4` · E `ForwardRight01_E_Forward_ForwardRight_Right.mp4` ·
F `ForwardRight01_F_ForwardLeft_v001_vs_ForwardRight01.mp4`, `ForwardLeft_v001_PRODUCTION_pair_for_ForwardRight01.mp4`.

### PRODUCTION SAFETY
Not installed. No controller reference to ForwardRight01 or any research clip (verified from the file on disk);
`Knight_Controller_FRStudy.controller` and `GuardGait_FR01Study.asset` are research copies under Generated (the
production GuardGait has no +45 entry). Study candidates and diagnostics deleted. Editor out of Play before every
write; prefab 2 / 4, TravelWalkSpeedScale 0.65 and scene Player (no override) verified before every session; production
clips, controller cycle offsets and GuardGait unchanged on disk. VerifyProduction TRUE. Tests 52 / 52. Git index left
untouched; nothing added or committed.
