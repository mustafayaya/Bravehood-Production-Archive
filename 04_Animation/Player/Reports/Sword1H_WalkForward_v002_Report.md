# Sword1H_WalkForward_v002 - Technical Report

ARTISTIC APPROVAL: PENDING

```
=== v002 vs v001 ===
FS residual  L: 50.0 -> 9.4 mm   R: 66.7 -> 4.0 mm
  L along/across 8.3/4.6   R 3.6/3.0
  toe FS  L 63.2 -> 9.5   R 79.7 -> 3.6
BROAD        L: 94.7 -> 61.4   R: 131.2 -> 115.5
FS yaw       L: 9.6 -> 0.2 deg   R: 13.7 -> 0.7 deg
FS window    L 13% -> 14%   R 16% -> 19%
roll: ankleRise L 45->36mm  R 43->46mm ; toeRise L 36->36 R 36->38
penetration < 0.001 mm/< 0.001 mm

native L=1.971 R=1.910 mean=1.940 asym=3.1% (v001 was 8.0%)
playback=1.031 effective=2.001 vs 2.000 -> mismatch +0.0% (v001 was +5.4%)
cycle=0.776s stride=1.506m width 20.7-43.2cm (range 22.4, v001 35.9)
pelvis V=39.1mm (v001 56.3) lat=24.4mm (v001 21.8)
head=-0.01/0.30 sword path=184mm RMS=7.9 blade=6.0
branch=0 curveDisc=0 muscleViol=0 loopPos=0.000mm loopVel=34.8
```

### Sword1H_WalkForward_v002
cycle 0.776 s | native 1.849 m/s | controller 2.000 m/s | playback x1.031 | reconstruction 1.940 m/s (animation-time)

**Left foot** - heel contact 0.783, full support 0.867-0.000, toe-off ends 0.108

| window | point | root-relative travel | expected translation | residual excursion | along | across |
|---|---|---|---|---|---|---|
| broad contact | ankle | 468.9 mm | 489.2 mm | **61.4 mm** | 27.2 | 58.0 |
| broad contact | toe | 468.6 mm | 489.2 mm | **55.2 mm** | 25.4 | 52.1 |
| **full support** | ankle | 201.3 mm | 200.7 mm | **9.4 mm** | 8.3 | 4.6 |
| **full support** | toe | 203.0 mm | 200.7 mm | **9.5 mm** | 8.4 | 4.8 |

yaw drift broad 6.357 deg, full support 0.163 deg | ankle rise 36 mm, toe rise 36 mm | penetration < 0.001 mm, hover 29.970 mm

**Right foot** - heel contact 0.267, full support 0.350-0.538, toe-off ends 0.692

| window | point | root-relative travel | expected translation | residual excursion | along | across |
|---|---|---|---|---|---|---|
| broad contact | ankle | 562.3 mm | 639.8 mm | **115.5 mm** | 82.6 | 84.7 |
| broad contact | toe | 568.4 mm | 639.8 mm | **116.8 mm** | 77.6 | 88.2 |
| **full support** | ankle | 279.9 mm | 282.3 mm | **4.0 mm** | 3.6 | 3.0 |
| **full support** | toe | 282.1 mm | 282.3 mm | **3.6 mm** | 2.0 | 3.0 |

yaw drift broad 2.528 deg, full support 0.706 deg | ankle rise 46 mm, toe rise 38 mm | penetration < 0.001 mm, hover 29.615 mm

