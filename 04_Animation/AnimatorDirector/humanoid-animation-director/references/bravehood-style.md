# Bravehood style — one-handed sword combat idle

## Character feeling

Calm. Prepared. Dangerous. Experienced. Grounded. Restrained. Watchful.

The character must look capable of attacking *immediately*.

Avoid: heroic posing, theatrical swaying, bouncing, exaggerated breathing, constantly looking
around, nervous twitching, MMO idle behaviour.

## Default 1H stance

The user's approved pose always wins. These are the defaults to fall back on and to sanity-check
a captured pose against:

- stance slightly wider than walking width
- knees softly bent — **check this explicitly**; a pose with near-straight knees leaves the legs no
  extension to absorb pelvis motion, and the generator will scale the pelvis down and say so
- roughly 55–60% rear-leg bias
- front foot oriented toward the threat, rear foot ~20–30° outward
- slight forward body inclination, head slightly forward, chin subtly down
- asymmetrical shoulders; sword-side shoulder slightly forward and down
- hips and chest subtly counter-rotated
- sword controlled close to the body; off hand relaxed but useful
- no exaggerated crouch

## Off hand

Relaxed, lightly flexed, ready to react. Not fully extended, not limp, no repeated opening and
closing, no rhythmic swinging. **Do not animate fingers in v1** — if finger animation is not
reliably supported by the current setup, leave them stable.

## Profile: `BravehoodCombatIdleProfile`

```
Duration          3.2 s @ 30 fps
Breathing         0.35   preset Subtle (v1 only needs Subtle to be good)
Weight shift      0.15
Awareness         0.08
Settlement        0.25
Weapon stability  0.90
Head stability    0.70
Off-side gain     1.45
Rear-leg bias     0.575
Feet              locked      Root  locked (all root channels baked into pose)

Warm start        on          Minimum-norm weight  0.004
Pose safe limit   0.90        Knee locked  0.97    Knee comfortable  0.92
Robustness sweep  on          -20/-10/+10/+20%, warn 1.0 mm, fail 2.0 mm
```

Knees are the joint that matters here. `< 0.92` extension is comfortable; `>= 0.97` fails health
and blocks generation.

## Project specifics

- Player rig: the warrior master rig, 45 bones, `Warrior_BaseAvatar`, `humanScale ≈ 0.831`,
  hip world y 0.780. See the `equipmentagent` skill for the rig itself.
- **`Hips` maps to the `Root` bone on this avatar.** Never treat the `Hips` bone as the pelvis —
  use the upper-leg midpoint.
- Existing clips live in `Assets/Core/Animations/` and `Assets/Animations/`; the OneHandedSwordV5
  set is under `Assets/Animations/OneHandedSwordV5/`.
- Generated idles stay in `Assets/Bravehood/Animation/Generated/` until approved.
- Gameplay camera is the shoulder-cam rig — review there, never from the Animation Preview alone.
