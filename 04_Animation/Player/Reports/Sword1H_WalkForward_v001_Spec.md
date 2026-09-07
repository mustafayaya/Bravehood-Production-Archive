# Sword1H_WalkForward_v001 - Spec

| | |
|---|---|
| Movement architecture | **IN-PLACE** (controller-driven translation) |
| Controller speed | 2.00 m/s (MaxStableMoveSpeed) |
| Cycle duration | 0.779 s (derived) |
| Native animation speed | 1.937 m/s |
| Playback multiplier (MotionSpeed) | 1.031 |
| Effective foot cadence | 1.997 m/s (-0.1 % vs controller) |
| Phase timing | Contact/Down/Passing/Up at 0 / .125 / .25 / .375 and +0.5, from the source gait |
| Foot contact windows | L 0.817-0.125 (32 %), R 0.308-0.667 (36 %) |
| Step length | 0.907 / 0.907 m  (stride 1.509 m) |
| Stride width | 36.7 cm mean (19.0-54.8) |
| Pelvis vertical budget | 56.4 mm |
| Pelvis lateral budget | 21.8 mm |
| Combat posture source | Sword1H_CombatIdle_v004 approved pose |
| Weapon discipline | stability 0.80 -> path 236 mm, RMS 9 mm |
| Head stability | alignment 0.90 -> +0.0 deg mean, 0.3 max |
| Expected feel | Calm controlled forward pressure; the approved idle advancing |

## Source strategy: HYBRID (source correction)

Lower body from `Loco_Walk_Fwd` (MoCapCentral), upper body from the approved combat pose.
Chosen by measurement, not taste - the two candidate walks measured:

| | Sword1h_WalkFwdLoop (current combat walk) | Loco_Walk_Fwd (chosen) |
|---|---|---|
| Travel heading | **+40.5 deg** (walks sideways) | -3.8 deg |
| Stride width | 0.7-85.7 cm (**85 cm accordion**) | 17.8-49.7 cm |
| Plant drift | 208 / 163 mm | 37 / 73 mm |
| Foot yaw drift | 28.9 / 82.2 deg | 6.2 / 36.6 deg |
| Head alignment | +4.1 deg | +0.1 deg |

The current combat walk is the source of the excessive lateral spread already noticed.
