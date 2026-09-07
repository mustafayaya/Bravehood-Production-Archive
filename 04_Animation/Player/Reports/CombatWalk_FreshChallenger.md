# FRESH HUMAN-MOTION CHALLENGER — `__freshHumanForward_01`

Authored, not solved. Shares no code and no values with the procedural pipeline.

## FIRST: THE REVIEW VIDEO I SENT LAST MILESTONE WAS WRONG

`__seqRepair_c40` is 0.5652 s long and its planted foot tracks backwards at **2.03 m/s**. I rendered
it at native rate while translating the character at 1.30 m/s, so **the feet skated 0.73 m/s and the
cadence read 212 spm instead of ~150.** Any judgement of that video was made against an artefact I
introduced. `CombatWalkSpeedScale = 0.65` in the Player prefab is almost exactly the 0.640 rate that
makes it travel 1.30 m/s, which is what production actually does.

A corrected render is included. Please judge the reference from that one.

## MOTION DESIGN

**Weight acceptance.** The authored table is the *extra sink* the body takes as its mass lands -
14 mm, peaking at phase 0.13, deliberately after contact at 0.00, because mass arrives after the
foot does. The knee yield that absorbs it is not authored at all: it is whatever the leg needs, which
is the correct causal order. Measured pelvis rhythm: **87 mm**, two dips per cycle.

**Compression / recovery.** Hip 713 mm at each acceptance, 800 mm at each mid-stance. Unequal energy
by construction - the body falls, catches, and rises again twice per cycle.

**Pelvis rhythm.** Lateral shift ±17 mm peaking over the loaded foot, yaw ±5 deg with the leading
hip, and a 3 deg Trendelenburg roll toward the swing side. The legs emerge from weight transfer
rather than cycling under a platform.

**Leg mechanics.** Hip and knee are never authored. The foot path and hip height are; the joints are
solved to realise them, so the chain stays connected by construction.

**Foot rollover.** Heel strike at -11 deg toes-up, sole loading through 0.14, roll to +19 deg by
heel-off, toe release at 0.62. Stance holds the sole flat at the floor: **71-73 mm against a 73 mm
floor, 5 mm maximum penetration**; swing peaks at 172 mm, late and low.

**Torso overlap.** Nothing moves at once. Spine lags the pelvis 0.06 of a cycle, chest 0.09, upper
chest 0.12, with counter-rotation gains of 0.55/0.70/0.80. The neck cancels ~55 % of the chest so
the head keeps its watch while the body works underneath.

**Arm overlap and sword inertia.** The lag chain continues: clavicle 0.14, upper arm 0.17, forearm
0.21, wrist 0.25. The wrist lags most, so the blade trails the hand and the hand trails the shoulder.

**Timing texture.** Keys are deliberately unevenly spaced - clustered through weight acceptance
(0.00, 0.06, 0.14) and thinned through mid-stance (0.30, 0.45). The spacing IS the texture.

**Asymmetry.** The right leg takes a 1.5 % shorter step and reaches toe-off 0.015 of a cycle earlier,
honouring the rig's real 17.3 mm leg-length difference instead of forcing a mirror.

## WHAT WAS CREATED FRESH

Everything except the approved combat carry, which every unauthored muscle holds so the Bravehood
silhouette is the starting point. Authored independently: foot path, foot pitch, hip-height/sink,
pelvis lateral/yaw/roll, all seven overlap lags and their gains, the arm inertia amplitudes, the key
phases. **Nothing was measured against, fitted to, or initialised from `__seqRepair_c40`.**

## TECHNICAL AUDIT — reported, not fixed

| | `__freshHumanForward_01` | `__seqRepair_c40` |
|---|---|---|
| clavicle deviation | **1.1 deg** | 6.6 |
| Right Shoulder Down-Up | **+0.062 .. +0.136** (approved +0.099) | -0.396 .. -0.154 |
| saturation | 0 % | 0 % |
| speed / cadence | **1.33 m/s / 150 spm / 0.80 s** | 2.03 native, 1.30 at 0.64x -> 136 spm |
| pelvis rhythm | **87 mm** | - |
| foot plant / penetration | 71-73 mm, **5 mm** | - |
| sword path | 333 mm | **183 mm** |
| blade range | 15.8 deg | **5.8 deg** |

Known defects, left alone deliberately:

1. **7 illegal interpolation samples** - curves cross +/-1 between keys. Needs a legality pass.
2. **Minimum knee bend 4 deg** at contact and toe-off. 1.30 m/s at 150 spm on a 731 mm limb demands
   near-full extension at the reach extremes; this is a real tension between the locked timing and
   the rig, not an oversight.
3. **Blade range 15.8 deg vs 5.8** - the authored arm inertia costs weapon discipline. This is the
   sharpest regression against the reference and the most likely thing to want reduced.

## SOURCE PRESERVATION

No production clip touched. v008 `7a7ca074`, v009 `5546958e`, v010 `7c44dd1d`, v011 `e3705c2f`,
`__seqRepair_c40` `b9d978b7` - all unchanged. No v012.

## CONTROLLER

From the serialized file: `Light_Walk8` = v011 @ ts 1.00, `Travel_Walk8` = v008 @ ts 0.50,
`CombatWalkSpeedScale` 0.65, no research clip referenced. The challenger is **not installed**.

`FRESH HUMAN-MOTION CHALLENGER READY FOR HUMAN REVIEW`
