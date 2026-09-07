# Sword1H_WalkForward_v011 — GENERATED AND INSTALLED

Anatomical rotation-distribution pass on v010. Timing, support schedule, whole-cycle solve,
posture pass and swing repair unchanged. 1.30 m/s, 150 spm, cycle 0.5652 s, 40 keys, stride
1025 mm, L/R stance-speed asymmetry 0.0%. New permanent diagnostic
`LocomotionAnatomicalRotationAudit` (swing-twist decomposition about each bone's own long axis,
not Euler - this rig's bone axes gimbal).

## SPINE — the suspected defect was NOT confirmed

| | segment spend | net chest change | **cancellation ratio** |
|---|---|---|---|
| idle v005 | 0.5 deg | 1.8 deg | **0.31** |
| source @150 spm | 1.6 deg | 2.7 deg | **0.58** |
| v010 | 5.7 deg | 13.5 deg | **0.42** |

v010 spends LESS rotation per degree of net torso change than the mocap source does. There is no
excessive counter-bending: the chain is coherent, and the upper chest is not fighting the chest.
Local sagittal ranges are small and ordered (spine 1.7, chest 5.1, upperChest 2.7 deg of twist;
swing 8.9 / 13.7 / 5.8). **No spine change was made** - item 6 says use the metric to find
unnatural compensation, not to optimise blindly, and it did not find any. v011 = v010 here.

## LEGS — the real defect, and what was fixed

Axial twist RANGE, degrees of real bone rotation:

| | L femur | L tibia | L foot | R femur | R tibia | R foot |
|---|---|---|---|---|---|---|
| source @150 spm | 16.3 | 7.6 | 12.8 | 12.1 | 11.4 | 17.4 |
| v010 | 33.8 | 18.0 | **31.5** | 16.2 | 19.1 | **30.9** |
| **v011** | 34.4 | **16.7** | **22.9** | **15.4** | **17.5** | **24.0** |

Axial twist p95 angular acceleration:

| | L femur | L foot | R foot |
|---|---|---|---|
| v010 | 14,620 | 8,854 | **26,986** |
| v011 (at tw 0.07 sample) | 14,142 | 6,492 | **15,300** |

Foot twist down 27% (L) and 22% (R), tibia down, right-foot twist acceleration roughly halved.
**Femur twist is unchanged** (34.4 vs source 16.3) - the prior did not reach it, and that remains
the largest outstanding anatomical deviation.

Knee plane: L range 46.1 deg, max frame step 13.6 deg (source: 157.5 / 93.4) - v010/v011 are far
smoother than the source here. The right knee plane reads 330 deg range in every clip including
the source; that is a measurement artefact, not motion - the bend-plane normal is ill-defined when
the leg is near-straight, and the right leg (17.3 mm shorter) passes closer to full extension.
Reported as a limitation, not chased.

Limb twist cancellation L 1.50 -> 1.56, R 1.22 -> 1.27 - essentially unchanged, so the twist that
was removed was net twist, not opposing segments cancelling.

## SHOULDER — saturation is REAL but LOAD-BEARING

| channel | approved pose | idle v005 | v010 | v011 |
|---|---|---|---|---|
| Right Shoulder Down-Up | **+0.099** | 0.069 / 0.141 | **-1.000 / -1.000** | -1.000 / -1.000 |
| Right Shoulder Front-Back | +0.534 | 0.540 / 0.561 | -0.184 / 0.322 | unchanged |

Answering item 16 directly: the approved pose does **not** use a near-limit shoulder. It sits at
+0.099; locomotion drove the channel to the negative limit and pinned it there for every frame.

Bone space, deviation from the approved pose (the authoritative measure per item 18):

| | clavicle dev min/mean/max | upper-arm dev min/mean/max |
|---|---|---|
| idle v005 | 0.0 / 0.7 / 1.2 deg | 0.4 / 0.7 / 1.0 deg |
| v010 | **18.2 / 18.9 / 20.9 deg** | 7.3 / 13.4 / 19.9 deg |
| with shoulder anchor 0.70 | 16.5 / **18.8** / 20.3 deg | 11.2 / **17.4** / 22.2 deg |

So it is a genuine ~19 deg static shoulder depression, not a rig quirk. But the anchor **does not
work**: at full strength the clavicle stays at 18.8 deg while the upper arm's deviation grows
13.4 -> 17.4 deg and the sword path inflates 170 -> 337 mm. The solve drives the shoulder straight
back, because holding the sword hand at its idle rest position while the torso is pitched forward
REQUIRES that clavicle angle. The saturation is load-bearing, not drift.

**The shoulder anchor was therefore NOT shipped** (`shoulderAnchor = 0`). Fixing this properly
means revisiting the weapon-hand stabilisation target so the hand is held relative to the moving
torso rather than the idle rest pose - that is upper-body art direction, which this pass is
explicitly not authorised to reopen. Flagged for a future milestone.

## TEMPORAL

| | knee frame step L/R | knee p95 accel | shelf | >0.99 | sword |
|---|---|---|---|---|---|
| v010 | 10.1 / 11.1 | 9,863 | 33 ms | 8.0% | 170 mm |
| **v011** | **9.7 / 12.0** | **9,146** | **17 ms** | **7.7%** | **169 mm** |

Both knees stay in the 9-12 deg region, well clear of the 15-18 deg warning. Shelf halved.
Stability check: tw 0.06 / 0.07 / 0.08 / 0.09 all land on the same plateau (knee 9.6-10.0 / 11.7-12.0,
shelf 17 ms), so this is not a lucky sample. Values at and below 0.05 and at/above 0.10 are worse.

## FEET / SUPPORT

Semantic topology **L1 / R1**, stance windows unchanged (L 0.68-0.20 wrapped, R 0.23-0.75, both
0.55 with 0.08/0.38/0.10 and 0.03/0.38/0.15). Stable-core planting:

| | ankle | toe | yaw |
|---|---|---|---|
| v011 L | 10.8 | 9.4 | 2.24 |
| v011 R | 9.7 | 6.6 | 2.73 |
| v010 L | 11.2 | 9.8 | 2.30 |
| v010 R | 9.0 | 6.7 | 2.50 |

Gates <15 / <15 / <3 PASS. Swing clearance and the v010 scuff fix are intact (`skip:clean` on the
right, repair 0.28 applied to the left, deepest sole -4 mm).

## PATH PRESERVATION (v010 -> v011, max deviation)

pelvis 3.8 mm, L ankle 7.9, R ankle 10.4, L toe 8.5, R toe 10.4, sword hand 8.8 mm,
L knee 14.3 mm, **R knee 50.4 mm**. The knee is the joint whose rotation was redistributed, so its
position moving is the intended consequence; every end-effector path is within ~10 mm.

## POSTURE (unchanged)

chest +2.5 vs +2.7, head -2.6 vs -2.2, chest-over-pelvis +18 vs +17 mm.

## TECHNICAL

Interpolated legality PASS. Loop seam 0.00000, tangent mismatches 0. Regression suite **22/22**
(two new: Light and Travel must not share a forward clip; both forward clips must resolve).
v001-v010 byte-identical (v008 `7a7ca074`, v009 `5546958e`, v010 `7c44dd1d`).

## SERIALIZED CONTROLLER VERIFICATION (item 29)

Read from the file, all 8 children of both trees compared before/after:

| tree | child 0 | ts | pos | children 1-7 |
|---|---|---|---|---|
| Light_Walk8 | **v011** (was v010) | 1 | (0,1) | all 7 unchanged |
| Travel_Walk8 | **v008** (was v010) | 0.5 | (0,1) | all 7 unchanged |

v009 and v010 are now referenced nowhere; v008 once; v011 once.

**Travel had drifted again.** Last milestone I restored it to v008 and verified that in the file;
this session it was found on v010. Both trees were rewritten explicitly, force-reimported, and
re-read from disk. Regression test N1 now fails if the two trees ever share a forward clip.

## REMAINING RISKS

1. **Femur axial twist is still ~2x the source** (34.4 vs 16.3 deg on the left) and was not
   improved. This is the largest remaining anatomical deviation and the most likely thing still
   readable as "solved" in the left leg.
2. **The sword shoulder remains saturated at -1.000**, ~19 deg from the approved posture. Not
   fixable without revisiting the weapon-hand target (out of scope). It is static, so it should
   read as a posture offset rather than an artefact.
3. Right knee position moved 50 mm from v010 - intended, but worth a look on the skeleton overlay.
4. The twist prior is only stable in a narrow band (0.06-0.09); outside it the solve jumps branch.
5. Travel's forward clip has drifted twice now by mechanisms I have not fully explained - the
   serialized check and test N1 catch it, but the cause is not yet root-caused.

**ARTISTIC APPROVAL: PENDING**
