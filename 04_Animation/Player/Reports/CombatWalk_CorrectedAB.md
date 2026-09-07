# CORRECTED FORWARD-WALK A/B

Neither clip was modified. Both md5s unchanged. No v012.

## PLAYBACK CORRECTION

Measured with one metric for both: the backward drift of whichever foot is planted, which is the
speed that makes a planted foot stationary in world space.

| | `__seqRepair_c40` | `__freshHumanForward_01` |
|---|---|---|
| clip length | 0.5652 s | 0.8000 s |
| native planted-foot speed | **2.025 m/s** | **1.354 m/s** |
| playback multiplier for 1.30 m/s | **0.642x** | **0.960x** |
| resulting world cycle | **0.880 s** | **0.833 s** |
| resulting cadence | **136.3 spm** | **144.0 spm** |
| effective travel speed | 1.30 m/s | 1.30 m/s |
| residual foot/world mismatch | **437 mm/s** | **168 mm/s** |

Both are rendered at 1.30 m/s, so neither skates from a playback mismatch. Cadence was NOT forced
to 150 - doing so would have reintroduced skate, which is what invalidated the previous review.

The residual mismatch is the slip that remains at the best possible speed, i.e. how truly planted
each clip's stance foot is. It is reported because it is measured, not to pick a winner.

## REVIEW SETUP — identical for both

Same Player prefab, same armour and sword, same scale, same shoulder-camera reconstruction
(eye 1.72 m, 1.55 m behind, 0.52 m to the character's left, 15 deg down, **FOV 80**), same grid
ground, same key/fill lighting, 1280x720, 60 fps, 8 s, no skeleton overlay.

Runtime pose-modifying IK is **absent, not merely disabled**: `FootIK`, `UpperBodyAim`,
`OffHandPose`, `AccelerationLean`, `TwistBoneDriver`, `HandGripPose` and `WeaponInertia` are all
play-mode-only components, and this renders in Edit Mode. There is nothing for a runtime system to
flatter.

One presentation note in the interest of honesty: the humanoid root Y is re-applied by the renderer,
because `SampleAnimation` does not apply it in Edit Mode. Without that the fresh clip's 87 mm pelvis
rhythm renders as 13 mm and its planted feet appear to sink through the floor. Both clips get the
same treatment.

## KNOWN TECHNICAL DIFFERENCES

| | `__seqRepair_c40` | `__freshHumanForward_01` |
|---|---|---|
| clavicle deviation | 6.6 deg | 1.1 deg |
| Right Shoulder Down-Up | -0.396 .. -0.154 | +0.062 .. +0.136 (approved is +0.099) |
| shoulder saturation | 0 % | 0 % |
| pelvis vertical rhythm | - | 87 mm |
| sword path | 183 mm | 333 mm |
| blade range | 5.8 deg | 15.8 deg |
| minimum knee bend | - | 4 deg |
| illegal interpolation samples | 0 | 7 |

The 7 illegal samples were checked for visible corruption before rendering and produce no glitch,
pop or limb snap in either view. They remain unrepaired, as instructed.

## THE QUESTIONS TO JUDGE

1. **Weight acceptance** - does the body land and carry mass?
2. **Compression / recovery** - useful energy contrast, or bouncy?
3. **Hip / leg naturalism** - whole-body locomotion, or legs cycling under a torso?
4. **Knee** - is near-extension visibly stiff?
5. **Foot rollover** - believable boot mechanics?
6. **Torso overlap** - human, or over-animated?
7. **Sword arm** - natural inertia, or too loose for combat?
8. **Knight identity** - calm, dangerous, prepared, controlled forward pressure?
9. **Overall craft** - which reads as hand-authored?

## PRODUCTION SAFETY

From the serialized files: `Light_Walk8` = `Sword1H_WalkForward_v011` @ ts 1.00,
`Travel_Walk8` = `Sword1H_WalkForward_v008` @ ts 0.50, `Player.prefab CombatWalkSpeedScale: 0.65`,
no research clip referenced. `__seqRepair_c40` `b9d978b7`, `__freshHumanForward_01` `6f3a9945` -
both unchanged. Neither is installed.

---

## SIDE-BY-SIDE IDENTITY — read after watching

The side-by-side carries no labels so it can be watched blind.

**LEFT = `__seqRepair_c40`** (the procedural reference, corrected playback).
**RIGHT = `__freshHumanForward_01`** (the fresh authored challenger).

They are not phase-synchronised - the cycles are 0.880 s and 0.833 s - so the two figures drift in
and out of step. That is a property of the clips, not the render.

`CORRECTED FORWARD-WALK A/B READY FOR HUMAN REVIEW`
