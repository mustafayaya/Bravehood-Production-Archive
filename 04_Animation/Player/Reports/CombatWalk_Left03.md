# `__freshHumanLeft_03` — forward-facing full-cycle TRUE LEFT (2026-09-04)

Research only. Not installed. Left02 kept as the direct source; production `Sword1H_WalkLeft_v001` untouched.

### BODY-FACING CHANGE
One dial: `yawTowardTravelDeg` −21 → **−13** (head dial +10 → +7 to keep the eyes on the threat). Every lower-body dial is
Left02's (alternating cycle, 230 mm stride, 0.80 s, lanes, stagger, sway, sink, knees, world foot pitch, reach margins).
The generator applies the steady pelvis yaw as the body frame, so the hip line and the chest counter-rotation both follow it.

Baseline that explains the complaint (same measure, root frame): the approved idle blades the pelvis **−29°** and the
shoulders **−34°** (right hip / right shoulder back). Left02 turned the pelvis to +38° and the shoulders to +11.6°, i.e. a
67° hip swing and a 45° shoulder swing the moment Left is pressed. Forward v012 sits at +4° / −1° (square); ForwardLeft
+32° / +10°; BackLeft +36° / +11°.

Bounded study (three candidates, lower body identical, judged on the real Player with lock-on at Left02's brisk presentation):

| candidate | yaw dial | authored hip line | authored shoulders | head vs v012 | runtime pelvis rel. root | runtime shoulders rel. root |
|---|---|---|---|---|---|---|
| Left02 (reference) | −21 | 38.0° | 11.6° | −2.9° | +32.3° | +6.0° |
| **A = Left03** | **−13** | **25.0° (18…32)** | **6.8° (5…9)** | **−1.0°** | **+19.2°** | **+1.1°** |
| B | −8 | 16.8° | 3.7° | +0.1° | +11.1° | −1.9° |
| C | −3 | 8.7° | 0.7° | +1.1° | +2.9° | −5.0° |

A lands on the requested region (pelvis 20–25°, shoulders 5–8°, head 0–3°) and keeps enough pelvic opening for the
lateral abduction; B and C read squarer still and are in the 2×2 for the reviewer. Feet are the same in all three.

### ROOT / PELVIS / SHOULDERS / HEAD — before vs after (real Player, lock-on)
- Player root yaw: **354.29°, constant** (range 0.00) in every session — the root faces the lock target; nothing rotates it toward travel.
- Pelvis line relative to the root, moving: Left02 **+32.3°** → Left03 **+19.2°** (authored 38.0 → 25.0).
- Shoulders relative to the root, moving: Left02 **+6.0°** → Left03 **+1.1°** (authored 11.6 → 6.8; UpperBodyAim tucks the chest further at runtime).
- Head: authored −1.0° vs the v012 reference (Left02 −2.9°); at runtime UpperBodyAim holds the head on the target (the bone-Euler read is
  a constant −15.9 axis offset in both clips and in idle, i.e. unchanged).
- Idle at runtime by the same measure: pelvis −34.5°, shoulders −39.4°. The swing on pressing Left drops from 67° / 45° to **54° / 41°**.

### LOWER-BODY PRESERVATION
Root-frame foot positions differ from Left02 by at most **12.6 mm** at any phase (the hips move with the body-frame yaw; the
lanes and stride are unchanged). No crossover: right minus left along travel ≥ **116 mm**, minimum boot separation
**214 mm**. Pelvis height 709…746 mm as Left02. Native speed unchanged (0.496 m/s): runtime 0.546 m/s at 1.100× / 165 spm,
stride error 0. Foot audit (below) matches Left02's within a degree.

### IDLE → LEFT CONTINUITY
`Artifacts/AnimationReview/Idle_to_Left02_vs_Idle_to_Left03.mp4` — first 4.5 s of the same Idle → Left script, side by
side. The chest silhouette on Left03 stays within a few degrees of square through the transition instead of turning left;
the residual difference to the idle is the idle's own right-side blade (−34°), which all four family clips leave
(Forward included) — a family-wide question, not a Left03 one.

### SWORD / SHOULDER
Right Shoulder Down-Up **0.092…0.106**, identical to Left02 (healthy, no compression). Sword-hand path 164 mm / cycle
(Left02 172). Minimum sword-pivot to upper-chest distance 458 mm — no torso collision. armGain 0.3 unchanged.

### SPEED
Left02's brisk presentation kept: 0.546 m/s, 1.100×, 165 spm, stride error +0.1 mm/s, planted drift 0–2 mm. No new
speed study.

### FOOT CONTACT QA (`LocomotionFootContactAudit`, 240 samples)
| leg | knee min | reach max | % below 15° | pitch vs flat | heel / toe min | support | flat | buried | support yaw drift |
|---|---|---|---|---|---|---|---|---|---|
| Left | 38.2° | 0.945 | 0 | 0…+4° | +5 / +4 mm | 61 % | 58 % | 0 % | 2.0° |
| Right | 40.4° | 0.939 | 0 | 0…+4° | +2 / +1 mm | 62 % | 60 % | 0 % | 16.6° |

Legality PASS, temporal 0 hold snaps (largest delta L knee 4.5°), deterministic (recipe rebuild vs promoted asset max
|Δ| 1e-5), canonical root, fully baked in-place contract.

### VIDEOS (`Artifacts/AnimationReview/`)
`Left02_vs_Left03_full.mp4` (A = Left02 left, B = Left03 right; same Player, MaxStable 2.0, camera, lock, loadout, lane,
0.546 m/s, 1.10×, writers, 60 fps), `Left02_vs_Left03_upperbody.mp4`, `Idle_to_Left02_vs_Idle_to_Left03.mp4`,
`Left03Study_2x2_Left02_A_B_C.mp4` (top-left Left02, top-right A = Left03, bottom-left B, bottom-right C),
`Left03Study_l3_ref / _A / _B / _C.mp4`.

### PRODUCTION SAFETY
Left_v001 md5 159e54d7 unchanged; Forward / ForwardLeft / BackLeft / Travel / production GuardGait / Player.prefab (2 / 4)
untouched. No controller reference to Left02 or Left03. VerifyProduction TRUE.

Scene: the saved PlayKit instance override `MaxStableMoveSpeed = 1` in `test_general.unity` was reverted to the prefab
value (2) and the scene saved, as instructed; every review session logged MaxStableMoveSpeed = 2. Correction to the
earlier report: the unattended Play sessions were not user activity. They appear right after a harness queue finishes
(Play time counts from the moment the last session ended, velocity 0, editor unfocused, no driver in the scene). The
driver does call `EditorApplication.isPlaying = false` at the end of its last session, so something re-enters Play after
that exit; the cause is not isolated yet. Until it is, Play is checked and exited explicitly before every asset write.
