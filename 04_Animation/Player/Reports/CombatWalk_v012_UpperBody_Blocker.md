# Sword1H_WalkForward_v012 — NOT GENERATED (gate 22)

Both authorised architectural changes were built and both WORK on their own target. They could not
be combined without destroying the sagittal posture item 6 requires preserved, so v011 stands.
No production clip created. Lower body, timing, support, swing, pelvis and Travel untouched.

## WHAT WAS BUILT

**1. Chest-space weapon rest** (`HumanoidFootLockSolver.chestSpaceWeaponBlend`). The rest target is
now also captured as the approved pose's hand-relative-to-CHEST relationship and reconstructed
against wherever the chest actually is, blended against the old root-space target.

**2. Combined 3D torso/head orientation solve** (`TorsoOrientationPass`). Replaces the two
independent scalar passes. The gait's deviation from the approved chest orientation is split
swing/twist about the vertical, the blade TWIST is kept whole, the SWING is damped uniformly in
3D, and a 2x2 finite-difference Newton drives the residual ROTATION VECTOR - so total orientation
error is the objective and one axis cannot be bought at the cost of the whole.

The swing/twist split was necessary and is worth recording: damping the whole deviation aims the
solve at an unreachable target, because the chest's deviation contains the intentional combat
blade yaw while the controls (sagittal and lateral spine shares) cannot produce yaw at all. The
Newton then stalls - measured, chest orientation error moved only 14.6 -> 13.7 deg where a retain
of 0.12 should have given about 2. After the split it responds properly (14.6 -> 11.1).

## RESULT 1 — WEAPON / SHOULDER: SOLVED, AT A PRICE

| | shoulder muscle | clavicle dev | upper-arm dev | sword path | blade range |
|---|---|---|---|---|---|
| approved v005 | +0.099 | 0.7 deg | 0.7 deg | - | - |
| v011 | **-1.000 / -1.000** | 18.8 deg | 12.5 deg | 169 mm | 10.5 |
| **chest-space 1.00** | **+0.09 / +0.11** | **0.2 deg** | **0.2 deg** | **648 mm** | 22.2 |
| chest-space 0.35 | -1.00 / -0.86 | 18.4 | 17.8 | ~300 mm | 12.7 |

At full chest-space the diagnosis is confirmed completely: the clavicle returns to the approved
pose to within 0.2 deg and the muscle lands on +0.09 against the approved +0.099. The saturation
was entirely an artefact of the root-space target. **But the hand then rides the chest's whole
translation** - lateral sway and vertical bob - and the sword path goes from 169 to 648 mm, which
is weapon pumping, not discipline (item 17). Blending back to keep the sword controlled restores
the clavicle depression almost fully: at 0.35 the shoulder is saturated again and the upper arm is
WORSE than v011 (17.8 vs 12.5). There is no blend value that keeps both.

The reason is now clear and is a real finding: **the shoulder is paying for the torso's
translation, not its orientation.** A chest-space target only helps once the chest itself is
stabilised - which is exactly what result 2 could not deliver.

## RESULT 2 — FRONTAL SWAY: SOLVED, BUT COSTS THE SAGITTAL POSTURE

| | chest roll | head roll | **head world tilt** | chest pitch | head pitch |
|---|---|---|---|---|---|
| source @150 spm | 2.4 | 3.1 | 3.1 | - | - |
| v011 | 19.9 | 22.8 | **13.2** | **+2.4** | **-2.6** |
| retain 0.45 | 11.1 | 10.8 | **7.7** | +6.9 | +7.2 |
| retain 0.30 | 9.4 | 10.1 | **8.0** | +12.6 | +13.6 |
| retain 0.15 | 9.8 | 13.0 | **2.3** | +16.1 | +22.9 |
| retain 0.10 | 10.2 | 13.3 | **2.1** | +16.0 | +23.2 |

The frontal target is met and the previous attempt's central failure is fixed: head tilt from
world up now FALLS (13.2 -> 2.1) where the two-scalar-pass version raised it to 16.2. That is the
combined-objective architecture doing its job.

But sagittal posture collapses in every configuration: chest pitch +2.4 -> +16, head pitch -2.6 ->
+23. That is a full return of the hunch, which item 6 forbids, plus a head pitched sharply up.
Lowering the retain makes it WORSE, not better, which rules out simple mis-tuning - the target
construction is damping the gait deviation proportionally, so it has no way to hold an ABSOLUTE
sagittal result the way the old scalar pass did by construction. Preserving +2.7 deg of chest
pitch and damping roll are not both expressible in the target as currently formulated.

## WHAT THE NEXT ATTEMPT NEEDS

The two results point at the same missing piece. The orientation target must be built from an
ABSOLUTE desired chest orientation - the approved combat orientation plus an authored lean,
expressed as a quaternion - rather than as a damped fraction of whatever the gait produced. Damping
a deviation cannot hold a target; it can only shrink toward the reference. With the chest then
genuinely stabilised in orientation AND its translation damped, the chest-space weapon target
should stop costing sword discipline, because the frame it rides would no longer be swaying.

## LOWER BODY / STATE (unchanged)

Ablations for the femur, run and reported for the record: foot-yaw weight has NO effect on left
femur twist (34.4 either way), narrowing the track makes it slightly worse (35.0), and freeing the
turnout limit helps mainly the RIGHT femur (15.4 -> 9.2). Documented per item 19, not solved.

Shipped assets byte-identical: v008 `7a7ca074`, v009 `5546958e`, v010 `7c44dd1d`, v011 `e3705c2f`.
No v012. Serialized controller from disk: `Light_Walk8` = v011 @ ts 1.00, `Travel_Walk8` = v008 @
ts 0.50, `CombatWalkSpeedScale` 0.65. Travel did not drift.

## REMAINING RISKS

1. **The profile no longer regenerates v011 bit-exactly.** After restoring every field to its v011
   value, a regenerated clip differs by up to 0.28 muscle units, concentrated in the RIGHT LEG
   (Upper Leg Front-Back 0.280, Lower Leg Stretch 0.272) and RootQ.y - i.e. the iterative solve
   settled on a slightly different branch, not an upper-body difference from this milestone's
   edits. The profile asset diffs against HEAD by added fields only, all at their off values, so
   no pre-existing setting changed. **The shipped v011 asset is untouched and verified**, so this
   affects reproducibility, not the installed animation - but it means the walk generator is not
   reliably deterministic across sessions, which the regression suite does not currently cover.
   Worth its own investigation before the next candidate.
2. Regression suite could not be run at the end of this session - the Editor is in Play Mode. It
   passed 22/22 earlier in the session, before the code changes above. **Re-run before trusting
   this milestone's code.**
3. Both new mechanisms ship defaulted OFF, so v011's behaviour is unaffected by their presence.

**ARTISTIC APPROVAL: PENDING** (v011 remains the candidate)
