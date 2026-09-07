# Combat idle — channels, budget, failure modes

## The five channels

Independent, phase-offset, each with its own job. They are summed as small deltas on the approved
pose, never blended as whole poses.

**A — Breathing** (primary). Chest expansion and thoracic elevation, distributed across
Spine → Chest → UpperChest so no single bone carries the rotation. Shoulders respond; the pelvis
responds a hair; the arms ride the ribcage rather than swinging. The neck counter-rotates most of
the ribcage pitch so the head does not bob. Breathing must not read as the whole torso moving
vertically.

**B — Weight redistribution**. Pressure, not a stance change: a small lateral pelvis shift with the
upper body compensating so the silhouette stays over the base of support. **Only the pelvis is
authored** — the legs belong to the foot-lock solver, and the feet do not move at all. Rear-leg bias
enters as a smooth second harmonic so the sway dwells on the loaded side; scaling the two halves of
the sway differently would kink the velocity at the crossing.

**C — Postural settlement**. A second-harmonic shoulder drop, chest adjustment, pelvis settle, hand
relaxation. Its purpose is to break perfect periodicity so the cycle boundary cannot be predicted.

**D — Target awareness**. Tiny head yaw and pitch with the neck following and a trace of upper-chest
twist. The character is watching a threat, not searching a room. This is the channel most often
overdone; when in doubt halve it.

**E — Weapon discipline**. Not additive motion — a *subtraction*. The sword arm is counter-animated
so the weapon hand does not simply ride the breathing ribcage. Blended, never pinned: a hand frozen
in world space reads as a bug. At stability 0.9 the hand keeps about a tenth of its natural travel.

## Phase offsets

Nothing peaks at the same instant. Body parts react to each other, not as one rigid unit.

```
Chest       0.00
Shoulders  +0.06
Sword arm  +0.09
Head       +0.11
Off hand   +0.14
Pelvis     -0.05
```

Values are tunable; the principle is not.

## Asymmetry

The character is carrying a sword, so nothing should mirror:

- the sword side is damped relative to the off side (`offSideGain`, default 1.45)
- the sword-side shoulder moves least
- the rear leg carries more load than the front (`rearLegBias`, default 0.575)
- the approved pose's shoulder asymmetry survives untouched

Idle movement must never erase the combat pose.

## Motion budget

Each region has a ceiling. This exists to stop "improving" the animation by adding motion.

```
Pelvis      LOW          vertical ~4.5 mm, lateral ~9 mm
Chest       LOW-MEDIUM   ~2.2°   (the primary channel)
Spine       LOW          ~0.9°
Shoulders   LOW          ~1.6°
Off arm     LOW          ~1.4°
Neck        LOW          ~1.1°
Sword arm   VERY LOW     ~0.7°
Head        VERY LOW     ~1.0°
Legs        VERY LOW     ~0.8°   (authored: none — solver only)
Feet        LOCKED       0
```

Over-budget regions are scaled **uniformly across the whole clip**, never clamped per key —
clamping flattens the curve tops and reintroduces exactly the robotic look the budget exists to
prevent. Every clamp is reported.

Profile amplitudes are *authored* degrees. Their measured effect on the chest bone runs roughly 2×
higher, because pinning the pelvis makes the body frame counter-rotate and the spine chain
accumulates. Tune against the **measured** number in the report, not the authored one.

## Duration

2.5–4.0 s. Do not assume 1.5 s — for Bravehood the longer restrained cycle reads more premium.
Default 3.2 s.

## The ten failure modes

| Mode | What you see | Cause | Fix |
|---|---|---|---|
| **Mannequin Idle** | statue | almost no biomechanical response | raise `breathing` a little; check the chest is not budget-clamped to nothing |
| **MMO Idle** | theatrical swaying | breathing / weight shift too large | lower both; check the measured chest angle against budget |
| **Breathing Robot** | everything rises and falls together | phase offsets too small or all channels sharing one phase | spread `phase*`; verify the detector is measuring pelvis-relative |
| **Floating Character** | pelvis moves, weight never lands | legs not absorbing the pelvis motion | foot-lock solver disabled, or the approved pose has locked knees |
| **Locked Torso** | rigid chest | chest amplitude ~0 | breathing has to live somewhere — raise chest, not pelvis |
| **Loose Sword** | weapon hand swings with the breath | `weaponStability` too low | raise it; the sword-hand path should be a fraction of the off hand's |
| **Bobble Head** | head rides the chest one-for-one | head stabilisation off or too weak | raise `headStability` / `headCompensation` |
| **Dancing Feet** | feet slide or pivot | leg twist authored, or the body frame not solved | never author leg muscles; check the body-neutral solve |
| **Pendulum Idle** | rocks left/right | `weightShift` too large, or lateral sway is a pure single sine | lower it; keep the rear-leg-bias harmonic |
| **Visible Loop** | you can predict the boundary | curves not periodic, or a solver branch step at the seam | check loop velocity continuity and the seam-closing warm start |

The validator sweeps for all ten. A clean sweep means nothing is *measurably* broken — it does not
mean the idle is good.

## What the numbers should look like

A finished Bravehood combat idle, measured on the character:

```
Loop position / rotation continuity   0.000 mm / 0.000°
Foot drift (each)                     < 0.5 mm
Foot yaw range                        < 0.15°
Pelvis vertical / horizontal          < 1.5 mm
Chest max angular deviation           ~2.2°
Head max angular deviation            < 1.5°
Sword-hand path length                a small fraction of the off hand's
Muscle limit violations               0 generated (inherited = fix the pose)
Curve tangent discontinuities         0
```
