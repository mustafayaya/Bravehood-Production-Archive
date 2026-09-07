# <CLIP NAME> — Spec

Write this **before** generating. It is the contract the clip is judged against.

Approved pose: `<PoseReferences/..._BasePose.asset>` — captured from `<character>` on `<date>`.
Approved by: `<user>` / **NOT YET APPROVED**

| | |
|---|---|
| Duration | `<3.2>` s @ `<30>` fps |
| Loop | Yes |
| Root motion | None |
| Feet | Both locked |
| Pose preservation | Very high |
| Breathing | `<Subtle>` |
| Weight redistribution | Very subtle |
| Target awareness | Minimal |
| Weapon discipline | High (`<Right>` hand) |
| Primary motion | Chest expansion and settlement |
| Secondary motion | Shoulders |
| Tertiary motion | Pelvis and head compensation |
| Weapon-hand motion | Minimal |
| Off-hand motion | Low |

Expected visual feeling: **calm, focused, dangerous, alive.**

## Phase offsets

```
Chest       0.00
Shoulders  +0.06
Sword arm  +0.09
Head       +0.11
Off hand   +0.14
Pelvis     -0.05
```

## Timeline (normalised — adjustable after visual review)

```
0.00  approved pose / neutral pressure
0.18  beginning inhale
0.32  chest near maximum expansion
0.46  subtle pressure redistribution
0.61  exhale / shoulders settle
0.73  tiny pelvis settlement
0.84  very small awareness adjustment
0.93  return toward baseline
1.00  exact seamless return
```

## Motion budget

State the per-region ceilings this clip is held to, and note anything deliberately different from
the profile default.

## Known risks for this pose

e.g. knee extension available for pelvis motion; muscle values already at their limits; asymmetry
that must not be flattened.
