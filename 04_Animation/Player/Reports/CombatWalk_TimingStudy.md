# Bravehood Combat Walk — Timing / Art Direction Study

Temporary, non-production. No numbered production clip was created. Production state
(`Sword1H_WalkForward_v008`, `CombatWalkSpeedScale 0.75`, `Travel_Walk8`) is unchanged.

## Candidates — true speed matching, not playback slowdown

Each clip was re-derived natively from `Sword1H_CombatIdle_ApprovedPose_v2` at its own cycle
length. `MotionSpeed = speed / 1.84` (the forward guard reference), so clip cycle =
world cycle x MotionSpeed and the feet travel at the real world speed.

| | speed | cadence | world cycle | MotionSpeed | clip cycle | step | frames/step @60Hz |
|---|---|---|---|---|---|---|---|
| v008 (production) | 1.50 | 167 | 0.719 s | 0.8152 | 0.5858 s | 539 mm | 21.6 |
| A | 1.35 | 155 | 0.774 s | 0.7337 | 0.5680 s | 523 mm | 23.2 |
| B | 1.30 | 150 | 0.800 s | 0.7065 | 0.5652 s | 520 mm | 24.0 |
| C | 1.25 | 145 | 0.828 s | 0.6793 | 0.5622 s | 517 mm | 24.8 |

All three MotionSpeeds sit inside the `[0.6, 1.5]` clamp. Note that pinning both speed and
cadence pins step length: A/B/C are all within 4% of v008's stride.

## Gates (unchanged)

ankle < 15 mm, toe < 15 mm, support yaw < 3 deg, interpolated Humanoid muscle range legal.

## Results — RAW and best legal smoothed

| | knee frame step L/R | knee p95 accel | ankle L/R | toe L/R | yaw L/R | knee-lock shelf | >0.99 | gate |
|---|---|---|---|---|---|---|---|---|
| v008 | 37.2 / 25.2 | 52,975 | 4.8 / 5.7 | 3.3 / 3.6 | 0.1 / 2.0 | 234 ms | 40% | PASS |
| A raw | 36.2 / 28.2 | 55,400 | 4.1 / 6.4 | 3.1 / 3.5 | 0.2 / 0.6 | 236 ms | 42% | PASS |
| A s0.40 | 33.1 / 25.3 | 51,861 | 6.4 / 12.4 | 4.4 / 4.2 | 0.5 / 1.0 | 135 ms | 36% | PASS |
| B raw | 31.5 / 24.6 | 51,951 | 4.0 / 5.4 | 3.0 / 3.3 | 0.1 / 0.7 | 250 ms | 40% | PASS |
| **B s0.40** | **29.5 / 22.2** | **46,823** | 6.5 / 12.6 | 4.4 / 4.4 | 0.4 / 1.0 | 150 ms | 36% | **PASS** |
| C raw | 34.0 / 26.8 | 50,633 | 4.2 / **20.7** | 3.1 / 10.4 | 0.2 / 1.0 | 265 ms | 43% | FAIL |
| C s0.40 | 30.8 / 24.1 | 47,023 | 6.5 / 12.5 | 4.4 / 4.3 | 0.5 / 1.0 | 149 ms | 39% | PASS |

Flight 3-5%, double support 13-15%, pelvis excursion 57-60 mm across all rows.

## Smoothing feasibility

sigma ceiling is 0.40 for all three candidates, and the failure is a cliff, not a slope:

| sigma | B knee frame step | B ankle L/R | gate |
|---|---|---|---|
| 0.00 | 31.5 / 24.6 | 4.0 / 5.4 | PASS |
| 0.40 | 29.5 / 22.2 | 6.5 / 12.6 | PASS |
| 0.42 | 28.9 / 21.5 | 17.7 / 13.0 | FAIL |
| 0.44 | 28.2 / 20.8 | 20.3 / 15.3 | FAIL |
| 0.80 | 19.2 / 12.6 | 23.1 / 27.1 | FAIL |
| 1.20 | 16.6 / 8.7 | 26.0 / 34.6 | FAIL |

The ceiling did not move with cadence. It sat at 0.40 at 1.50/167 in the v009 study and it sits
at 0.40 at 1.25/145, 1.30/150 and 1.35/155.

## Timing-matched source comparison

The mocap source was scaled to each candidate's own world cycle before comparison.

| | source (matched) | candidate s0.40 | ratio |
|---|---|---|---|
| v008 @0.719 s | 12.8 / 9.4, accel 13,075 | 37.2 / 25.2, 52,975 | 2.91x / 2.68x, 4.05x |
| A @0.774 s | 11.9 / 8.8, accel 11,264 | 33.1 / 25.3, 51,861 | 2.78x / 2.88x, 4.60x |
| B @0.800 s | 11.5 / 8.5, accel 10,549 | 29.5 / 22.2, 46,823 | **2.57x / 2.61x, 4.44x** |
| C @0.828 s | 11.1 / 8.2, accel 9,857 | 30.8 / 24.1, 47,023 | 2.77x / 2.94x, 4.77x |

## Negative result worth recording

Hypothesis: the sigma ceiling is set by step length, so a shorter stride at the same cadence
should buy smoothing headroom. Tested as a labelled diagnostic (1.10 m/s at 150 spm, step
440 mm, not a candidate): raw knee frame step got **worse** (38.6 / 29.0), contact topology
degraded to 6 left intervals, and sigma 0.80 still failed planting (40.0 / 25.1 mm).
Shortening the stride is not the lever either. Refuted.

## Travel coupling (documented, not redesigned)

`Light_Walk8` and `Travel_Walk8` are two separate BlendTree sub-assets of
`Knight_Controller`, but their forward children point at **the same clip asset**
(`Sword1H_WalkForward_v008`, guid `12f2b3e7...`), both at `timeScale 1.00`.
Consequence: replacing the clip asset in place retimes travel locomotion as well.
Any future production retime of the combat walk must either give Travel its own forward clip
or accept that both change together. `WalkTimingPreview` therefore edits only the
`Light_Walk8` tree.

## Preview

`Bravehood > Animation > Walk Timing Preview > {A|B|C} ({raw|smoothed s0.40})`, and
`Restore Production (v008, 1.50 m-s)`. Each entry swaps only the `Light_Walk8` forward child
and text-patches `CombatWalkSpeedScale` (A 0.675, B 0.65, C 0.625; restore 0.75).
Verified reversible: Travel untouched, restore returns the exact production state.

## Technical recommendation

**B (1.30 m/s, 150 spm), smoothed at sigma 0.40** — best on every measured axis, and the only
candidate that passes the gates both raw and smoothed. **Artistic approval: PENDING.**
