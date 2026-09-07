# ForwardLeft cleanup — Phase 1 biomechanics (2026-09-07)

Research only. Production `Sword1H_WalkForwardLeft_v001`, Light_Walk8, GuardGait, gameplay scale, the other seven
directions, Travel, CombatIdle, FootIK, grip and Player untouched (read back from disk: v001 max |Δ| 0 against its
source, controller free of research references, GuardGait −45 1.24 / 0.862, prefab 2 / 4). VerifyProduction TRUE.
Git index untouched; nothing added or committed.

### BASELINE FORWARDLEFT v001 FORENSICS
The production curves (= `__freshHumanForwardLeft_02`, 102 / 102 identical) are the authority; no reconstructed recipe
reproduces them (a rebuild from the recorded FL02 dials differs by 1.05 muscle, so the historical build state is not
recoverable and was not used).

| metric (authored, rig-calibrated) | Left | Right |
|---|---|---|
| support knee minimum | **4.2°** | **4.3°** |
| reach max | 0.999 | 0.999 |
| toe / heel minimum vs flat | **−48 / −5 mm** | **−29 / −9 mm** |
| buried fraction | **32 %** | 6 % |
| flat sole fraction | 13 % | 29 % |
| support-yaw drift (whole support) | **30.5°** | **25.8°** |
| boot pitch range | −59…+34° | −48…+31° |
| stance-track speed along −45° | 1.141 m/s | 1.283 m/s |

The two stance tracks disagree by 0.14 m/s: the straight right leg pushes the root faster than the left plants —
a within-stance slide, not a stride. Toe-off/touchdown are a heel-strike at +34° toes-up and a −1260 mm/s left
touchdown; knee frame steps at the 0.795× presentation reach 20.8° / 19.2° per 60 Hz frame. Runtime (production, this
session): support knees 4.2° / 1.9°, planted yaw 19.7° / 8.6°, toe bone 12 / 17 mm, planted drift 3 / 3 mm. Upper body:
hip line 31.9° mean, shoulders 10.1°, head −3.3° vs v012, sword pivot ≥ 452 mm from the chest.

Cause, in order: the FL02 generation predates per-leg reach planning (both legs solved to 0.999 of reach, the shorter
right leg included), world-space planted pitch (feet plant heel-first at ankle-local pitch and bury the toe through
mid-stance) and the stance-heading hold (the planted boot yaws with the opening pelvis, 26–31°). Not authored stance
heading, not release timing: the boot heading itself is −47 / −52° mean, on the travel line.

### EXISTING FORWARDLEFT03 AUDIT
`__freshHumanForwardLeft_03` (Sep 3, foot-realism repair, ARTISTIC PENDING) is the FL02 dials plus the contact options
(`plantedFootRoll`, `reachMarginMm 20`, `rightReachExtraMm 8`, `stanceYawStabilize 1`, release 0.58–0.66), built on
the same code path in the same session as the approved clip. Diff against production: **91 of 102 curves
bit-identical** — every upper-body, arm, head, root-XZ and root-rotation curve; changed only the eight leg channels,
`RootT.y` (pelvis 24 mm lower on average for the reach margin) and the two thigh-twist curves. It is a surgical leg
repair of the approved motion, not a different animation.

| metric | v001 | **FL03** |
|---|---|---|
| knee minimum L / R | 4.2° / 4.3° | **12.1° / 23.2°** |
| reach max | 0.999 / 0.999 | 0.994 / 0.980 |
| toe / heel minimum | −48 / −5 · −29 / −9 mm | **+2 / +6 · +2 / +5 mm** |
| buried | 32 % / 6 % | **0 / 0 %** |
| flat sole | 13 / 29 % | **44 / 45 %** |
| support-yaw drift | 30.5° / 25.8° | **0.6° / 1.7°** |
| boot pitch | −59…+34 / −48…+31° | −40…+4 / −49…−5° |
| stance tracks | 1.141 / 1.283 m/s | **1.271 / 1.251** (native 1.261) |
| pelvis height / rhythm | 718…777 / 59 mm | 694…758 / 65 mm |
| hip line / shoulders / head | 31.9° / 10.1° / −3.3° | **identical** |
| foot angular peak L / R | 929 / 1036°/s | 406 / 563°/s |
| thigh twist range / key step | 0 / 0 | 0.213 / 0.070 |
| knee frame step @0.795× | 20.8° / 19.2° | 10.7° / 6.9° |
| touchdown vertical | −1260 / −457 mm/s | −179 / −248 mm/s |
| swing | clean single arc, peak 65, mid-min 20 / 18 | clean single arc, peak 64, mid-min 23 / 21 |

Built with the pre-repair stabiliser: checked for its known failure modes — no NaN, thigh-twist key step 0.070 (the
seam-snap signature was 0.35), foot/thigh angular peaks lower than v001's own. The stabiliser repairs of 2026-09-06 do
not change a converging build of this kind (the release window 0.58–0.66 ends after this gait's toe lift at ~0.5, but
the FL geometry tolerates it: the free swing heading of both boots stays within 15° of the held heading).

### CHALLENGER DECISION
**FL03 is the primary challenger. No ForwardLeft04 was created.** It preserves the approved ForwardLeft character
(identical upper body, pelvis opening, sword restraint, head, step timing), threat facing, the shared 0.986 m/s
presentation, and materially improves knee reserve, planted yaw and boot contact.

### KNEE RESERVE
Support geometry, not cosmetic bend: per-leg reach planning lowers the hips ~24 mm and plans each leg against its
own thigh+shin (right 17 mm shorter, `rightReachExtraMm 8`). Authored 12.1° / 23.2°, runtime 12.1° / 23.2° at the
presentation (v001 4.2° / 1.9°). Knee range 12…71° / 23…58°. Family class: Right 36 / 42, ForwardRight 21.5 / 29.6.

### PLANTED YAW
Cause: no stance-heading hold in the FL02 build — the boot rotated with the opening pelvis (hip line swings 21…43°).
FL03 uses the existing stance-heading stabilisation; whole-support drift 0.6° / 1.7° authored, **0.1° / 0.4°** at
runtime (mid-stance drift 3 / 6 mm, v001 3 / 3). Boot headings −54 / −53° mean, on the travel line. No new subsystem.

### FOOT CONTACT / TOE CLEARANCE
Rig-calibrated flat toe 61 / 53 mm. FL03: toe bone never below +2 mm of flat (v001 −48 / −29), heel +6 / +5, sole flat
44 / 45 % of the cycle, heel-off 18 / 16 %, no burial; runtime toe bone 33 / 34 mm (v001 12 / 17). Swing not flattened:
peak 64 mm, mid-swing minimum 23 / 21 mm, the family plunge (−40 / −49° swing pitch) unchanged.

### BODY FACING PRESERVATION
Runtime pelvis +26.7° (+15.7…+36.7), shoulders +4.6° (+1.6…+7.5), head on the target, root 354.29° constant — v001
+26.6 / +4.4. Hip line, shoulders and head curves are bit-identical. Not solved here by design.

### SWORD / SHOULDER PRESERVATION
Right Shoulder Down-Up and every arm curve identical to v001; sword pivot ≥ 452 mm from the chest in both. The sword
path shortens 414 → 326 mm per cycle only because the pelvis rides 24 mm lower and steadier — no arm change.

### SPEED / STRIDE MATCH
Native 1.261 m/s (v001 rated 1.24 while its two stance tracks read 1.14 / 1.28). At the approved 0.986 m/s the
challenger plays at **0.782×** (v001 0.795×), 117 spm (119), stride error 0.0 mm/s, planted drift 3 / 6 mm. No speed
study: the visual cadence is unchanged. Likely production data if approved: −45 native 1.261, gameplayScale 0.862
(unchanged) → 0.986 m/s.

### FORWARD → FORWARDLEFT → LEFT
| segment | v001 run | FL03 run |
|---|---|---|
| Forward | 1.144 m/s, pelvis −1.9 / −6.6 | same |
| ForwardLeft | 0.986 @ 0.795×, +25.6 / +4.0 | 0.986 @ 0.782×, +25.7 / +3.9 |
| Left v002 | 0.546 @ 1.09×, +18.5 / +0.9 | +18.6 / +0.9 |
| boundary F→FL: foot / hips / sword | 2.63 / 1.32 / 1.26 m/s | 2.67 / 1.27 / 1.24 |
| boundary FL→L | 2.25 / 1.11 / 1.09 | 2.24 / 0.75 / 0.81 |
| minimum boot separation | 146 mm | 158 mm |

Transitions unchanged or slightly softer; Forward and Left untouched.

### V001 vs CHALLENGER VISUAL COMPARISON
`FLCleanup_C_v001_vs_FL03_0p986.mp4` (both at 0.986 m/s, individually stride-matched) and the lower-body close-up
`FLCleanup_F_lowerbody_closeup_v001_vs_FL03.mp4`. Same silhouette, timing, pelvis opening, guard and head; the
challenger's support legs stay flexed, the planted boot holds its heading and the sole sits on the floor where v001
buries the toe and locks the knee. Idle → ForwardLeft entry: peak pelvis / shoulder yaw 492 / 317°/s in both — not
worse (the dedicated idle pass is later).

### LIKELY PHASE
Install-convention landings: FL03 L 0.021 / R 0.504 (v001 0.988 / 0.542). Against Forward (tree L 0.088 / R 0.623)
and Left v002 (L 0.06 / R 0.56): **co 0.93** gives L 0.100 / R 0.580 (worst deviation 0.043); the current 0.92 gives
0.110 / 0.590 (0.050). Either; not written.

### TECHNICAL QA
Legality PASS (0 NaN); temporal at 0.795× — 0 hold-snap events, top frame steps L knee 10.7° / R knee 6.9°; semantic
support 73 / 73 % with the sole-and-speed product; swing trajectory clean single arcs, 0 reversals; foot-contact audit
above; achieved knee reserve 12.1° / 23.2°; per-leg reach 0.994 / 0.980; world foot pitch −40…+4 / −49…−5°; planted
yaw 0.6° / 1.7°; canonical root (0.00° / 0.00 mm); baked in-place contract (loopTime, orientation / Y / XZ baked, Based
Upon Original, heightFromFeet off). Determinism: the clip is a fixed asset whose promotion would be a key-identical
copy; its base recipe is not reproducible from dials (recorded above), so regeneration is not the path — the asset is.

### VIDEOS (`Artifacts/AnimationReview/`)
A `FLCleanup_A_ForwardLeft_v001_pure.mp4` · B `FLCleanup_B_ForwardLeft03_pure.mp4` · C `FLCleanup_C_v001_vs_FL03_0p986.mp4` ·
D `FLCleanup_D_Forward_ForwardLeft03_Left.mp4` · E `FLCleanup_E_Forward_ForwardLeft_v001_Left.mp4` ·
F `FLCleanup_F_lowerbody_closeup_v001_vs_FL03.mp4`.

### PRODUCTION SAFETY
Research only: FL03 reached the Player through an in-memory name override on the production controller with a
research GuardGait copy (`GuardGait_FL03Study`, −45 = 1.261 / 0.862); nothing written to production. Editor out of
Play before the study-asset write; controller, GuardGait and prefab read from disk after. Player 2 / 4, scene Player
without override, Militia loadout 0.880, one Player in the loaded scene, every session diag MaxStable 2. VerifyProduction
TRUE. Nothing added or committed.
