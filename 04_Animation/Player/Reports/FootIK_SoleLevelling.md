# FootIK sole levelling — the rolled right boot (2026-09-07)

Report: "while walking the right foot IK looks rotated — the ground is flat but it rolls a bit to the left on X."

### DIAGNOSIS
FootIK was not rotating the foot at all on flat ground: the world rotation of either foot bone changes by ≤ 0.03°
between the Animator's write and the end of frame in every production run (lock-on and free travel, all eight
directions). The tilt is AUTHORED. The walk generator positions each foot and solves its world pitch, but never its
roll, so the splayed hips leave the sole standing on its edge. Measured against the boot's own sole plane (read off the
leg mesh's bind pose: vertices skinned to the foot / toe bones, lowest 15 mm, least-squares plane — 75 / 83 vertices,
0.0 / 0.7° from the mesh floor, elongation 2.2 / 2.0), planted-phase sole roll per clip:

| clip | left | right |
|---|---|---|
| CombatIdle_v005 | +4.7° | +8.2° |
| Forward_v012 | −23° | +30…+50° (heel-strike frames) |
| ForwardRight / ForwardLeft | −22 / −25° | +33 / +33° |
| Right / Left | −18 / −26° | +23 / +31° |
| BackRight / Back / BackLeft | −18 / −22 / −8° | +27 / +31 / +17° |

Both boots roll outward; the right one rolls more and is the one the shoulder camera sees. Neither the bone axes nor
Unity's muscle-zero pose can serve as a flat reference on this rig (the foot bone's axes are ~15° off the sole and
the Humanoid T-pose points the toes down 50°), which is why earlier audits never measured it.

### FIX (FootIK.cs, AnimatedBones solver)
`CalibrateSoles()` at Start reads each foot's sole normal and long axis in bone space from the skinned mesh bind pose
(down axis = the mesh's longest extent signed by hips → foot; NOT the hip-to-foot line, which is 16° off vertical on
this splayed bind pose and fitted a wedge of the boot's side). Gates: ≥ 24 band vertices, band width ≥ 8 mm, plane
within 6° of the rig's down, elongation ≥ 1.6 — a mesh that fails any gate keeps the old assumption and says why in
`SoleCalibration`. Per frame the foot is rolled about its own sole axis until the sole's lateral tilt matches world up
(`SignedAngle` of the sole normal against up, ⟂ the long axis), clamped to `MaxSoleRoll` 35°, faded out with sole
clearance (0.12 → 0.30 m) so a high swing keeps its authoring and a landing foot arrives level, smoothed on its own
60 ms (independent of the plant weight, so toe-off does not snap back in the 25 ms release), scaled by RotationWeight
and the master weight. The existing terrain tilt then rolls the LEVELLED sole onto a slope, so side slopes are not
counted twice; pitch is untouched (heel-strike and rollover stay as authored); a rotation about the ankle moves no
bone, so the two-bone solve, pelvis settle and clearance probes are unaffected (probes read the clean animated bones).
Inspector: `LevelSoleRoll` (on), `MaxSoleRoll`, `SoleRollSmooth`, `SoleRollFadeStart/End`; trace fields
`SoleRollDeg / SoleRollApplied / HasSole` for the harness.

### RESULT (real Player, production controller, lock-on, loadout 0.880, 60 fps)
Applied roll in stance: Forward L −17 (−27…−7) / R +25 (+22…+28)°; ring F/FR/R/BR/B/BL/L/FL right foot +25 / +22 /
+18 / +18 / +20 / +10 / +23 / +22°, left −17 / −16 / −14 / −13 / −17 / −5 / −20 / −18°; idle +4 / +5°. Largest
per-frame change 2.4°. World speeds, MotionSpeed, root yaw (range 0.00), stride and IK plant weights identical with
the levelling on and off (Forward 1.144 / 0.854 both). Mobs: calibration reports "no skinned mesh with the foot
bone" (their meshes are not CPU-readable) → unchanged behaviour. Visual (feet close-ups, `RTQA_feetcam`): the right
heel that dug in on its inner edge now sits flat; left boot level; idle level; heel-strike / rollover pitch preserved.

### VIDEOS (`Artifacts/AnimationReview/`)
`FootIK_SoleLevel_A_Forward_off_left_vs_on_right.mp4` (gameplay camera) ·
`FootIK_SoleLevel_B_feet_closeups_off_left_vs_on_right.mp4` (1 s per pair: right boot behind / front, left boot
behind / front, through the stride) · `FootIK_SoleLevel_C_ring_sweep_on.mp4`.

### STATE
Changed: `FootIK.cs` (sole levelling), `RuntimeQaDriver.cs` (trace fields, `RTQA_feetcam`, `RTQA_soleRoll` A/B
toggle, `soleCal` in the diag). No prefab, clip, controller or table changed (Player.prefab md5 unchanged; the new
fields take their code defaults). Tests 55 / 55. Git index untouched; nothing committed. The authored roll itself
remains in the clips — a generator-side roll solve (`Foot Twist In-Out` as a 5th leg DOF against the mesh sole plane)
would be the source fix if the walks are ever regenerated.
