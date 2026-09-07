# `__freshHumanLeft_02` — FULL-CYCLE TRUE LEFT research challenger (2026-09-04)

Research only. Not installed. `Sword1H_WalkLeft_v001` untouched and still the production Left.

### LEFT02 MOTION DESIGN
A continuous ALTERNATING lateral strafe instead of Left01's step-close. New generator mode
`HumanWalkPerformance.Options.lateralAlternating` (inert by default; Left01's recipe still rebuilds key-identical):
both feet stride the same distance along travel, half a cycle apart, each in its OWN lane outside its own hip, so the
read is LEFT STEP → RIGHT STEP → LEFT STEP with no closing foot and no settle. Pelvis channels are analytic and
continuous (sinusoidal sway / yaw / roll, one raised-cosine acceptance sink after each landing) — nothing holds.

| dial | Left02 | Left01 |
|---|---|---|
| cycleSeconds / samples | **0.80** / 48 | 0.60 / 48 |
| lateralAlternating / laneOutMm | **true / 70** | — |
| lateralDriftMm / swingFrac | **230 / 0.42** | 273 / 0.35 |
| lateralSwingHeightMm / stagger / sway | 70 / 110 / **30** | 80 / 100 / 50 |
| lateralSinkMm / sinkPeak / yawSwing / rollSwing | 12 / 0.18 / 3 / 3 | table beats |
| yawTowardTravelDeg / chestFollowFrac / headYawDeg | −21 / −0.03 / +10 | same |
| stanceKneeBendDeg / minKneeBendDeg / armGain | 38 / 15 / 0.3 | same |
| lateralWorldPitch / strike / swingToesUp | **true / 0 / 4** | false |
| reachMarginMm / rightReachExtraMm | **20 / 8** | — |

### ALTERNATING STEP STRATEGY
Left swings [0, 0.42), lands at 0.41 (measured), stance 0.42–1.0; right swings [0.5, 0.92), lands at 0.91. Each foot lands
at lane + 115 mm along travel and drifts 230 mm under the body through its stance. Double support 22 % (two × 11 %),
no flight. One left contact + one right contact per cycle — the same continuity as Forward / ForwardLeft.

### LEFT FOOT ROLE
Steps outward left from its lane (−269…−39 mm root X), accepts weight (sink trough 18 % into the stance), carries the
body through its stance, releases while the right foot is already loading.

### RIGHT FOOT ROLE
No longer a close: it pushes from its stance, unloads, swings the same 230 mm left, plants in its own lane
(+39…+269 mm root X), accepts weight with the same sink beat and drives the next left step. Symmetric by design.

### FOOT LANES / NO CROSSOVER
Right foot minus left foot along travel: **109…507 mm, never below 109** (lanes do not overlap). Across travel the
left foot stays 155 mm toward the threat. Minimum foot separation 190 mm, so the boots never intersect.

### STANCE WIDTH
Separation 190…530 mm, continuous (Left01 reached ~790 mm at its open). Root-frame lanes ±39…269 mm.

### SUPPORT / WEIGHT TRANSFER
Stance 61 % per foot, double support 22 %. Runtime forensic on the real Player: mid-stance ankle drift **0–3 mm** on both
feet at every presentation (Left01 production: 1–4 mm), i.e. the feet are planted, and each foot carries a visible
sink (pelvis rhythm 37 mm authored, 35–37 at runtime).

### PELVIS CONTINUITY
World pelvis speed at native never drops below 0.26 m/s (0.26…0.80) — no pause; the sway is one sinusoid per cycle,
peaking over each loaded foot at mid-stance. Compare the step-close, where the pelvis parks over the loaded foot.

### PELVIS / TORSO / HEAD
Hip line 31…45° toward travel, mean 38.0° (Left01 38°); shoulder line 10…13°, mean 11.6° (Left01 11.6°); head vs the
v012 reference mean −2.9° (−7.7…+1.9). Root yaw and position 0.00 through the clip; root on the lock target at runtime
(354.29°, constant). Sword-hand path 172 mm / cycle (Left01 360), blade calm.

### FOOT CONTACT (`LocomotionFootContactAudit`, 240 samples, rig-calibrated)
| leg | knee min | reach max | % below 15° | pitch vs flat | heel / toe min | support | flat | buried | support yaw drift |
|---|---|---|---|---|---|---|---|---|---|
| Left | 37.8° | 0.946 | 0 | 0…+4° | +7 / +5 mm | 60 % | 43 % | 0 % | 0.5° |
| Right | 37.2° | 0.948 | 0 | 0…+4° | +4 / +2 mm | 62 % | 60 % | 0 % | 17.1° |

Both support knees stay softly flexed through the whole cycle (no straight-leg singularity: Left01 runtime hit 1.9°).
World-pitch solve keeps both boots flat through support; no burial. Right support yaw drift 17° = the family-wide
planted-boot pivot watch item (Left01 L 14° at runtime), unchanged, not corrected here per the standing rule.

### NATIVE SPEED / CYCLE / CADENCE
Native planted-foot speed 0.496 m/s (230 mm / (0.80 s × 0.58)), cycle 0.80 s, 150 spm.

### SPEED STUDY (real Player, lock-on, Warrior Base loadout, research override into the −90° production slot, research
GuardGait rating it 0.496; movement through the status multiplier — doubled because the test scene's PlayKit instance
overrides MaxStableMoveSpeed to 1, see PRODUCTION SAFETY)

| candidate | playback | world speed | cadence | 0.25 s | 0.5 s | 1.0 s | stride error | planted drift |
|---|---|---|---|---|---|---|---|---|
| SLOW | 0.80× | 0.397 m/s | 120 spm | 0.10 m | 0.20 m | 0.40 m | +0.1 mm/s | 0–1 mm |
| MEDIUM | 0.95× | 0.471 m/s | 142 spm | 0.12 m | 0.24 m | 0.47 m | +0.1 mm/s | 0–2 mm |
| **BRISK** | **1.10×** | **0.546 m/s** | **165 spm** | **0.14 m** | **0.27 m** | **0.55 m** | +0.1 mm/s | 0–2 mm |
| current gameplay (comparison) | 1.22× | 0.606 m/s | 183 spm | 0.15 m | 0.30 m | 0.61 m | +0.3 mm/s | 0–4 mm |

### SELECTED PRESENTATION (my read; human review decides)
**BRISK — 1.10× / 0.546 m/s / 165 spm.** The alternating steps stay individually readable, the pelvis flows, and the
cadence sits with the family (Left v001 173 spm, BackLeft 167). At 1.22× the steps start to blur into a shuffle; at
0.95× (142 spm) the gait is calmest but 10 % slower than production Left and reads deliberate again. MEDIUM is the
alternative if the reviewer wants more weight per step. Whatever is chosen, production would rate −90° at 0.496 native
and set the −90° gameplay scale to hit the chosen world speed (0.546 → 0.477).

### LEFT01 vs LEFT02 BLIND REVIEW
`Artifacts/AnimationReview/Left_BLIND_A_vs_B_full.mp4` and `Left_BLIND_A_vs_B_lowerbody.mp4` — same Player, camera,
loadout, lock-on, lane, direction, runtime writers, 60 fps, 10 s. One side is production Left_v001 at its approved
0.606 m/s / 0.866×, the other Left02 at 1.10× / 0.546 m/s. Side assignment is random; the key is held outside the
report and released on request.

### TECHNICAL QA
Legality PASS (24 samples/span), temporal 0 hold snaps (largest frame delta L knee 4.5° @0.88), support schedule as
authored, swing 143 mm peak with the toe never below flat, seam clean (49 keys/curve, cyclic), deterministic
(102 bindings, max |Δ| 0), root canonical (0.00° / 0.00 mm), fully baked in-place contract (loop on, loop blend off,
orientation / Y / XZ baked, Based Upon Original, heightFromFeet off). Generator change inert at defaults.

### PRODUCTION SAFETY
`Sword1H_WalkLeft_v001` untouched; Forward v012, ForwardLeft v001, BackLeft research/production, Travel family, GuardGait
production Left entry (−90 = 0.70 / scale 0.53) untouched. Player.prefab 2 / 4 untouched. No controller reference to
Left02; `GuardGait_Left02Study.asset` is a research asset under Generated. VerifyProduction TRUE. Tests 48 / 48.

Observed and NOT changed: `Assets/Scenes/test_general.unity` carries a saved PlayKit instance override
`MaxStableMoveSpeed = 1` on the Player (the prefab says 2). Every Play session in that scene runs the Player at 1 m/s,
which pins the stride matcher at its 0.6 floor and slides the feet — this is what the in-game "weird feet" were. The
study compensated through the status multiplier; the override itself is yours to remove.
Also restored this session: the controller's cycle offsets (all zeroed by an external save) and the Travel_Walk8
install on top of HEAD.

### VIDEOS (`Artifacts/AnimationReview/`)
`Left02_RAW_native.mp4`, `Left02Speed_l2_slow / _medium / _brisk / _current.mp4`, `Left02Speed_2x2_slow_medium_brisk_current.mp4`
(top-left slow, top-right medium, bottom-left brisk, bottom-right current), `Left02Speed_ab_A_left01.mp4` (production
Left at its presentation), `Left_BLIND_A_vs_B_full.mp4`, `Left_BLIND_A_vs_B_lowerbody.mp4`.
