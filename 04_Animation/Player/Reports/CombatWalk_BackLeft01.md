# `__freshHumanBackLeft_01` — TRUE BACKLEFT (−135°) research challenger (2026-09-04)

Research only. Not installed. Forward v012 / ForwardLeft v001 / Left v001 / Travel v012_Travel untouched (md5 verified).

### MOTION DESIGN
"Giving ground while staying ready to fight." A −135° retreat is NOT a backward walk on the travel line: an alternating
backward stride would swing the closing (right) foot past the planted left foot on a diagonal line, i.e. a cross-behind.
It is a **step-close retreat** built on the Left01 lateral architecture (`lateralStepClose`) rotated to travel −135°:
the left foot opens back-left along the travel line, the body transfers onto it, the right foot pushes and recovers to
the guard, and both feet stay on their own side of the pelvis. Two double-support phases per cycle, shorter and lower
than Left01, quieter arms.

| dial | BackLeft01 | Left01 | why |
|---|---|---|---|
| cycleSeconds / samples | **0.65** / 48 | 0.60 / 48 | a more cautious beat |
| lateralDriftMm / swingFrac | **250** / 0.35 | 273 / 0.35 | shorter stance drift = shorter steps |
| leadLandMm / trailLandMm | **300 / −20** | 340 / −40 | shorter reach back-left; trail recovers to the guard |
| lateralSwingHeightMm / stagger / sway | **70** / 110 / 40 | 80 / 100 / 50 | low careful step; lead foot kept toward the threat |
| yawTowardTravelDeg / chestFollowFrac / headYawDeg | **−20 / −0.03 / +12** | −21 / −0.03 / +10 | pelvis opens toward the retreat, chest returns, eyes on the threat |
| stanceKneeBendDeg / minKneeBendDeg / armGain | 38 / 15 / **0.25** | 38 / 15 / 0.3 | crest cap; Forward reserve; retreat reduces arm amplitude |
| reachMarginMm / rightReachExtraMm | 20 / 8 | — | approved per-leg reach reserve from the foot-realism repair |
| **lateralStrikeToesDownDeg / soleLoadFrac / swingToesUpDeg** | **8 / 0.12 / 2** | 0 / — / 6 | NEW: a backward step lands ball first and settles flat; toes hang low in swing |
| **lateralWorldPitch** | **true** | false | NEW: the boot's world pitch is solved per frame (see FOOT CONTACT AUDIT) |

New generator options are inert at their defaults: the Left01 recipe rebuilds key-identical (102/102 curves, max |Δ| 0)
with the new generator, including when built immediately after a BackLeft build.

### LEAD / TRAIL LEG STRATEGY
Sword in the right hand, guard toward the threat: the **left leg opens** (it is the leg on the retreat side and the one
the body can fall onto without turning), the **right leg keeps the guard** and recovers under the body — it never
reaches behind the left. Asymmetric by design, not by symmetry.

| phase | lead (left) | trail (right) | support |
|---|---|---|---|
| 0.00–0.35 | swings back-left along travel (reach 300 mm), toes pre-positioned down | stance, drives | single, right |
| 0.35–0.47 | lands ball first (−8° → flat over 12 % of the stance), accepts weight (sink peak 0.48) | stance, pushes | **double** |
| 0.50–0.85 | stance, drifts under the body | recovers to 20 mm behind the root along travel | single, left |
| 0.85–1.00 | stance | lands ball first (sink peak 0.96–0.98) | **double** |

### FOOT TRACKS (root frame, generated clip, 480 samples)
- Both stance tracks head exactly −135.0° at 0.592 m/s; across-travel slide 0 mm/s.
- Root-frame X gap right−left 414…687 mm, foot separation 414…738 mm — **no crossover at any phase**. At the left landing
  the feet sit at L (−361, −152) / R (+326, +117) mm; at the right landing L (−225, −16) / R (+190, −19).
- Swing peak ankle 143 mm both feet (low, careful). Toe bone minimum 60 / 52 mm = the flat reference (61 / 53): no burial.
- Stance ankle at 72 mm both feet (flat).

### WEIGHT TRANSFER
Pelvis 675…746 mm, rhythm 70.9 mm, troughs at 0.49 and 0.96 (the two landings), along-travel sway 84 mm over the loaded
foot. Knee L 24…64°, R 38…59° — the right (guard) leg stays flexed through its whole stance, which is the retreat
posture; no knee reaches lock (0 % below 15° on either leg).

### PELVIS / TORSO / HEAD
Root yaw 0.00° and root position 0.00 mm through the whole clip (canonical, controller-driven). Hip line 30…43°
toward travel, mean 36.4° (Left01 38°). Shoulder line 9…13°, mean 11.0° (Left01 11.6°) — the chest stays on the
threat. Head yaw vs the v012 reference: mean −0.3°, range −5.7…+5.1° — eyes on the threat, no counter-twist added.
At runtime the root stays on the lock target (root yaw constant 354.29° in every session).

### SWORD / SHOULDER
Sword-hand path 306 mm / cycle (Left01 360, ForwardLeft 473), blade heading range 4.9°. armGain 0.25. Shoulder Down-Up
inside the family band (no collapse; the clavicle is not authored beyond the lag chain).

### NATIVE HEADING / SPEED
Heading −135.0° (both stance tracks). Native planted-foot speed **0.592 m/s** (authored 250 mm / (0.65 s × 0.65) = 0.592;
measured 0.592 / 0.592 L / R). Native cadence 185 spm.

Controller math at −135° (real `BaseCharacterController`): forwardness = cos 135° = −0.707 →
lerp(Strafe 0.53, Backpedal 0.6, 0.707) = 0.58; target = 2.0 × 0.65 × load 0.880 × 0.58 = **0.663 m/s** loadout
(0.753 unarmoured) → BackLeft01 would play at **1.12×** loadout / 1.27× unarmoured today. Measured on the real Player:
0.669 m/s (the ring slot sits at −141.6°, forwardness −0.784 → 0.585).

### SPEED STUDY (real Player, lock-on, Warrior Base loadout, research override into the −141.6° ring slot, research
GuardGait rating that slot 0.592 so MotionSpeed stride-matches; movement scaled by the status multiplier)

| candidate | playback | world speed | cadence | 0.25 s | 0.5 s | 1.0 s | stride error |
|---|---|---|---|---|---|---|---|
| SLOW | 0.757× | 0.448 m/s | 140 spm | 0.11 m | 0.22 m | 0.45 m | 0.0 mm/s |
| **MEDIUM** | **0.904×** | **0.535 m/s** | **167 spm** | **0.13 m** | **0.27 m** | **0.54 m** | 0.0 mm/s |
| BRISK | 1.062× | 0.629 m/s | 196 spm | 0.16 m | 0.31 m | 0.63 m | 0.0 mm/s |
| current gameplay (comparison) | 1.130× | 0.669 m/s | 209 spm | 0.17 m | 0.33 m | 0.67 m | 0.0 mm/s |

Clip weight 1.000 on the override slot in every run; root yaw constant; foot forensic identical across rates
(knee min L 24 / R 25, toe min 51–57 mm, pelvis range 71 mm) — only timing changes.

### SELECTED PRESENTATION (my read; human review decides)
**MEDIUM — 0.90× / 0.535 m/s / 167 spm.** Slower than Left (0.606 m/s, 173 spm), as a retreat should be, but still
27 cm in half a second. At this rate the open-and-close reads as two deliberate events with weight arriving on the
left foot; at 1.06× and above the closing foot starts to tick and the retreat reads as a nervous backpedal. SLOW
(0.45 m/s, 140 spm) is the most deliberate and the most "cautious", at the cost of responsiveness (11 cm in 0.25 s) —
worth a look if the reviewer wants BackLeft to feel heavier than Left. To land MEDIUM in production the −135° gameplay
speed would need ≈0.535 m/s (a directional factor of ≈0.47 instead of the current 0.58) — a data change, not authored.

### FOOT CONTACT AUDIT (`LocomotionFootContactAudit`, 240 samples, rig-calibrated)
| leg | knee min | reach max | % below 6/8/10/12/15° | pitch vs flat | heel / toe min clearance | support | flat | heel-off | buried | support yaw drift |
|---|---|---|---|---|---|---|---|---|---|---|
| Left | 24.0° | 0.978 | 0/0/0/0/0 | −8…+2° | +1 / −2 mm | 71 % | 63 % | 1 % | 0 % | 20.3° |
| Right | 38.2° | 0.945 | 0/0/0/0/0 | −8…+2° | 0 / 0 mm | 70 % | 63 % | 3 % | 0 % | 3.6° |

The first build (ankle-local foot pitch as in Left01) had the RIGHT foot toes-down 22–31° through its stance with the toe
buried 32 mm for 63 % of the cycle: the trail foot spends its stance forward of the hip on a flexed knee, and a foot
rigid to a forward-leaning shin points down. `lateralWorldPitch` solves the boot's world pitch per frame (flat sole
through stance, ball-first landing) and removed it entirely. Left support yaw drift 20° is the planted-boot pivot the
lateral gait has always had (Left01 runtime 14°); not treated here — a simpler motion that works is preferred.

### FAMILY CONTEXT
`Artifacts/AnimationReview/Family_Forward_ForwardLeft_Left_BackLeft01_gameplaySpeeds.mp4` — 2×2: top-left Forward v012
(1.144 m/s, 0.854×), top-right ForwardLeft v001 (0.987, 0.796×), bottom-left Left v001 (0.606, 0.866×), bottom-right
BackLeft01 at the selected MEDIUM (0.535, 0.904×). All on the real Player with lock-on and the loadout. Reading the
progression 0° → −45° → −90° → −135°: the pelvis opens 0 → 18 → 36 → 36°, the chest stays within 12° of the threat
throughout, stride shortens and cadence drops toward the retreat.

### TECHNICAL QA
- Interpolated Humanoid legality (24 samples/span): **PASS**, no muscle leaves [−1, 1]; build-time span flattenings 0.
- Temporal audit 60 Hz at 1.0×: **0 hold snaps**; largest per-frame joint deltas L knee 8.2° @0.31, L ankle 7.1° @0.28,
  R knee 6.0° @0.56; p95 angular velocities pelvis 62, chest 23 deg/s.
- Support schedule: two single / two double phases as authored (audit: support 71 / 70 %).
- Swing-foot trajectory: 143 mm peak, toe never below the flat reference.
- Seam: periodic basis, cyclic tangents, 49 keys / curve, loop clean.
- Deterministic generation: two builds, 102 bindings, max |Δ| 0.
- Root canonical: root yaw 0.00°, position 0.00 mm at every sample.
- Fully baked in-place contract: loopTime on, loopBlend off, orientation / Y / XZ baked, Based Upon Original, heightFromFeet off.
- Generator inertness: Left01 recipe rebuilds identical (102/102). A pre-existing order dependence was found and fixed
  along the way: after a build with `reachMarginMm > 0`, `stanceCapSide` was not cleared, so the next default build
  inherited its caps (0.48 muscle units on Left01's leg curves). Both arrays now clear together.

### PRODUCTION SAFETY
`AnimationAssetSafety.VerifyProduction` TRUE. No research clip referenced by the controller; the legacy −141.6° ring
child untouched; no GuardGait BackLeft entry (`GuardGait_BackLeft01Study.asset` is a research asset under Generated,
referenced by nothing). Tests 45 / 45. Nothing committed.

Note on the Player baseline: `Player.prefab` MaxStableMoveSpeed was found rewritten 2 → 1 at 02:35 (between the
verified Left restore and this study; no project script writes that field). It was restored to 2 before the study.

### VIDEOS (`Artifacts/AnimationReview/`)
`BackLeft01_RAW_native.mp4` (authoring render at native 0.592 m/s), `BackLeftSpeed_bl_slow / _medium / _brisk / _current.mp4`,
`BackLeftSpeed_2x2_slow_medium_brisk_current.mp4`, `Family_Forward_ForwardLeft_Left_BackLeft01_gameplaySpeeds.mp4`.
