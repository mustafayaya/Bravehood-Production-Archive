# Forward runtime blockers — equipment grip and Light_Walk8 forward sector

`__freshHumanForward_02` untouched (`a1630664`). No v012, no Fresh03, no curve or clip change. Every
measurement is the real Player in Play Mode (Transport research copy via the in-memory override,
`GuardGait_Fresh02`, FootIK `AnimatedBones`, all writers on); sessions chained by the harness inside one
Play Mode (`RuntimeQaDriver` now reloads the scene between queued sessions instead of dropping to Edit
Mode, which only ticks while the editor is focused).

## GRIP ROOT CAUSE

Writer: `EquipmentManager.UpdateWeaponGrip → ApplyGrip` with `autoFitGrip = 1`. It measures the equipped
torso's **skinned-palm centroid** (vertices ≥ 0.5 weighted to `R_Hand`, in hand-bone space) and re-seats
the sword to `palm(current) + (authoredGrip − palm(first equipped))`, then adds the item's declared
`gripPositionOffset / gripEulerOffset`.

Architecture facts (every wearable item asset inspected): all 13 items skin to the same 45-bone master rig,
the same `R_Hand`, the same `Sword` socket (`R_Hand/Sword`, authored local (−0.043, 0.063, −0.045)) —
the hand skeleton is shared. But the "palm" the auto-fit measures is a property of each MESH, not of the
hand:

| torso | hand-weighted verts | centroid (hand space, mm) | extents (mm) |
|---|---|---|---|
| Warrior | 199 | (+3, 50, −9) | 176 × 73 × 131 (a hand) |
| **Militia** | 172 | **(−69, 60, −48)** | 41 × 40 × 88 (a cuff, not a palm) |
| Militia V2 | 554 | (−11, 64, −12) | 90 × 130 × 111 |
| Armor B | 256 | (−24, 33, −11) | 114 × 36 × 90 |
| Old Knight | 123 | (−40, 79, −42) | 138 × 119 × 130 |

So the "fit" moved the sword by whatever geometry difference the artist happened to weight to the hand:
Militia 72 mm, Armor B 27 mm, V2 15 mm — plus Militia's declared offset (−53, −13, +27 mm / 4°), which had
been tuned under an earlier first-equip order and now stacked on top. Result on the gameplay starting
loadout (Militia torso + helmet + Warrior legs, equipped by `PlayerEquipment` after the prefab default):
hilt 125 mm up the forearm.

## GRIP POLICY STUDY (hand-local sword position, mm; standing locked, then walking with shots)

| loadout | A current (auto-fit + item offset) | B authored + item offset (`autoFitGrip 0`) | B0 authored, Militia offset cleared |
|---|---|---|---|
| Warrior Base | (−43, 63, −45) = authored ✓ fist | (−43, 63, −45) ✓ | same |
| **gameplay Militia** | **(−168, 60, −57)** hilt at the elbow | (−96, 50, −18): still 5 cm up the forearm, hilt not in the fist | **(−37, 66, −40) ✓ pommel below the fist** (`Grip_repaired_gameplayLoadout_Militia.mp4`) |
| Militia V2 | (−65, 24, −8) | (−51, 10, −5) ✓ pommel at the fist (its declared offset (−8, −53, +40)/(6, −19, −7)° is a real per-item tune) | unchanged |
| Armor B | (−70, 46, −47) | (−43, 63, −45) ✓ | unchanged |

## SELECTED GRIP REPAIR (applied)

1. `Player.prefab` `EquipmentManager.autoFitGrip: 1 → 0` — one line. The authored socket transform is
   the authority; the shared hand skeleton makes it valid for every set.
2. `Equip_Militia_Torso.asset` `gripPositionOffset / gripEulerOffset → 0` — the stale offset. Explicit
   per-item metadata stays the mechanism for pieces that genuinely need it (Militia V2 keeps its tune).
3. The swapper's digit keys can no longer change the loadout in a shipping player (below).

Verified in the pure gameplay equip path (no debug swapper): sword 12 mm / 1.6° from the authored grip
through a full walk (`sweep1_0`), Warrior/Armor B unchanged, Militia V2 unchanged.

## LIGHT_WALK8 FULL CHILD MAP (serialized, before the repair)

`FreeformDirectional2D`, params `Horizontal / Forward` (unit vector from the velocity heading), every
child magnitude 1.000, timeScale 1:

| # | clip | position | angle from Forward | cycleOffset | clip's measured travel heading (`averageSpeed`) | native speed |
|---|---|---|---|---|---|---|
| 0 | `Sword1H_WalkForward_v011` (generated) | (0, 1) | 0° | 0.92 | 0° | – |
| 1 | `Sword1h_Strafe45RightLoop` | (0.998, 0.065) | **+86.3°** | 0.80 | +86.3° | 1.807 |
| 2 | `Sword1h_StrafeRightLoop` | (0.816, −0.578) | +125.3° | 0.78 | +125.3° | 1.838 |
| 3 | `Sword1h_Strafe135RighttLoop` | (0.170, −0.985) | +170.2° | 0.65 | +170.2° | 1.802 |
| 4 | `Sword1h_WalkBwdLoop` | (−0.621, −0.784) | −141.6° | 0.72 | −141.6° | 1.838 |
| 5 | `Sword1h_Strafe135LeftLoop` | (−0.992, −0.124) | −97.1° | 0.52 | −97.1° | 1.817 |
| 6 | `Sword1h_StrafeLeftLoop` | (−0.797, 0.605) | −52.8° | 0.92 | −52.8° | 1.823 |
| 7 | `Sword1h_Strafe45LeftLoop` | (−0.162, 0.987) | **−9.3°** | 0.98 | −9.3° | 1.789 |

Every mocap child sits exactly on its clip's measured travel heading. The intended ring (Forward, FL, L,
BL, B, BR, R, FR at 45° steps) is instead: 0, −9.3, −52.8, −97.1, −141.6, +170.2, +125.3, +86.3 — the seven
mocap headings are all rotated **+35–42°** from their names, uniformly (45→86, 90→125, 135→170,
180→−142, −135→−97, −90→−53, −45→−9). Ordering and handedness are correct; radial magnitudes are correct;
the defects are a **9.3° neighbour on Forward's left** and an **86° hole on its right**.

## STRAFE45LEFT 9.3° ROOT CAUSE

**B + C, not corruption or drift.** All seven `Sword1h_Walks.fbx` clips are imported with
`orientationOffsetY = −35` (the "square-up" that turns the bladed capture to face the character's forward;
`CharacterAnimator` documents it and its `WalkLightAng` table carries the same numbers). The square-up
rotates the BODY, so every clip's travel heading shifts by +35° (the actor walked ~7° right of his blade
line, hence 41.7° for the original forward clip). The tree was then built — correctly for stride matching —
by placing each child at its measured travel heading, so feet never skate. `Sword1h_Strafe45LeftLoop`
therefore really travels 9.3° left of forward on this rig; its 45° is a source-frame name. The ring was
coherent (rotated uniformly) until the generated forward clip, which travels at a true 0°, replaced the
mocap forward that had sat at 41.7°. That single correct placement created the asymmetric forward sector.
Git history is one squashed baseline; the controller has not drifted since.

## BLENDTREE REPAIR (applied, one serialized line)

`Light_Walk8` child 7 (`Sword1h_Strafe45LeftLoop`) position `(−0.16160382, 0.98685575) → (0, 0)`. With
`Horizontal/Forward` always a unit vector, a child at the origin receives zero weight: the near-forward
strafe leaves the ring, and the left-front sector interpolates Forward (0°) ↔ `StrafeLeft` (−52.8°) — a
mirror of the existing right side (0° ↔ 86.3°). Nothing else moved; no curve, clip, timing or import was
touched; the clip stays referenced (reversible by restoring the line). Moving the child outward instead
(e.g. to −45°) was rejected: the clip's feet travel −9.3°, so any other position would trade the forward
defect for a 36° foot skate in the left-front sector. The proper coverage repair remains the 8-way family.

## FORWARD SECTOR BEFORE / AFTER (unarmoured, Fresh02 override; window frames 60–280)

| target | Fresh02 weight before → **after** | adjacent child before → after | MotionSpeed | pelvis rhythm after | ankles L/R vs Fresh02 (mm) after | sword pos / blade vs Fresh02 after |
|---|---|---|---|---|---|---|
| 20° right | 0.000 → **0.603** | Strafe45Left 0.754 → StrafeLeft 0.368 | 0.844 | 39 mm | 264 / 289 | 260 mm / 48° |
| 10° right | 0.000 → **0.804** | Strafe45Left 0.984 → StrafeLeft 0.188 | 0.906 | 54 mm | 135 / 144 | 123 mm / 23° |
| **5° right** | **0.462 → 0.904** | Strafe45Left 0.538 → StrafeLeft 0.095 | 0.938 | **62 mm** | 71 / 72 | 58 mm / 11° |
| 0° | 1.000 → 1.000 | – | 0.970 | 69.2 mm | 15 / 7 | **12 mm / 1.6°** (grip repaired; was 125 mm) |
| 5° left | 0.942 → 0.940 | Strafe45Right 0.058 | 0.950 | 66 mm | 40 / 50 | 27 mm / 9° |
| 10° left | 0.884 → 0.877 | Strafe45Right 0.116 | 0.929 | 63 mm | 78 / 99 | 61 mm / 18° |
| 20° left | 0.768 → 0.746 | Strafe45Right 0.232 | 0.886 | 53 mm | 151 / 189 | 136 mm / 37° |

Fresh02 is dominant to ±10° on both sides and still 60 % at 20° right / 75 % at 20° left. MotionSpeed
falls with the offset because the `GuardGait` rating interpolates toward the neighbouring clip's 1.8 m/s
(by design of the per-heading table). The remaining left/right difference is the source clips: the
left-front neighbour (`StrafeLeft`, −52.8°) is nearer than the right-front one (`Strafe45Right`, 86.3°).

## VISUAL LOCK-ON RESULT

`Artifacts/AnimationReview/Fresh02_LOCKON_repaired_0.mp4`, `…_L57.mp4`, `…_R57.mp4` — gameplay loadout,
Warrior legs + Militia torso/helmet as the player spawns, all writers on. Numbers for the three (final
pose vs Fresh02, 12 phases): see the final message. The ±5.7° pair now blends 6–10 % of a neighbour on
either side instead of 6 % vs 61 %.

## DEBUG SWAPPER

`EquipmentDebugSwapper.Update` polled `Keyboard.current` digits 1–9 every frame in every build; a stray
key press re-equipped a set mid-session (weight, renderers and grip changed under the harness). Smallest
safe behaviour, applied: the hotkey poll is compiled out of non-development builds
(`#if !(UNITY_EDITOR || DEVELOPMENT_BUILD) return;`). In the editor it still works for reviews; the QA
harness disables the component for its sessions.

## PRODUCTION FOOTIK / GUARDGAIT (prepared, not installed)

`FootIK.Solver = AnimatedBones` must be serialized on the Player at v012 install (today the prefab has no
`Solver:` line and relies on the C# default). `GuardGait_Fresh02.asset` is the production reference to
assign to `CharacterAnimator.GuardGait`: walk `0°: 1.34` (Fresh02/v012), other headings at the mocap
clips' measured speeds, jog = existing table; null today → legacy 1.80 table + warning.

## TESTS

`T1_EquipTorso_KeepsAuthoredGrip_UnlessItemDeclaresOffset` — Player ships with the auto-fit off; equipping
Warrior leaves the authored grip, equipping Militia moves it by exactly its declared offset (now zero),
swapping back restores it (no history dependence).
`T2_LightWalk8_ForwardSector_HasNoRingChildInsideThirtyDegrees` — every ring child (magnitude > 0.5) keeps
≥ 30° from Forward; a child parked at the origin is not on the ring. Fails on the pre-repair controller.
Suite: **36 passed / 0 failed / 0 skipped** (34 unchanged + T1 + T2; T1 runs the manager's Awake/Start on
an edit-mode Player instance with the edit-mode Destroy() warning scoped out).

Visual lock-on numbers (final pose vs Fresh02, 12 phases, gameplay loadout): dead ahead pelvis 4.4 mm,
sword 10.8 mm / 1.5°, rhythm 70.7; **5.7° left** Fresh02 0.93 + Strafe45Right 0.066: pelvis 10.1, ankles
45 / 54, head 9 mm 2.5°, sword 28 mm / 10°, rhythm 66.9; **5.7° right** Fresh02 0.89 + StrafeLeft 0.108:
pelvis 14.8, ankles 72 / 76, head 13 mm 6.3°, sword 65 mm / 12.7°, rhythm 61.1 (was 79 / 420–468 / 79 mm
30° / 417 mm 82° / 36.8).

## PRODUCTION STATE

Serialized changes this milestone, each one line: `Knight_Controller.controller` child 7 position;
`Player.prefab` `autoFitGrip: 0`; `Equip_Militia_Torso.asset` grip offsets zeroed; `EquipmentDebugSwapper.cs`
compile guard. `Light_Walk8` forward = v011 @ ts 1.00, `Travel_Walk8` forward = v008 @ ts 0.50,
`CombatWalkSpeedScale 0.65`, `GuardGait` null, no research clip referenced (`VerifyProduction` CLEAN), no
v012, harness disarmed.
