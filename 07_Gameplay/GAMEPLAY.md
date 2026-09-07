# GAMEPLAY

> The dungeon's spatial/gameplay layout is documented. Systems (economy, combat, progression) are **not yet documented** — marked TBD.

## Core Loop
PvPvE extraction: enter the dungeon → fight AI enemies and rival players → collect loot → reach an extraction point and extract. Dying loses your run.

## Map-level Gameplay (DungeonA Floor 1)

> **Corrected 2026-08-21 against the live scene.** The previous entry ("4 corner spawns, 5 extractions") was wrong: the scene held **2** spawn markers and **2** extractions, and Spawn A stood in empty space at (-8, 4.3, 84.8) — ~8 m north of the last floor slab. Full analysis: `AI/DungeonA_Floor01_Gameplay_Analysis_2026-08-21.pdf`.

### Player spawns — 6, placed 2026-08-21
Under `Gameplay/Spawns`. Each marker is the conventional sphere at floorY+6 (scale 4, spawn blue) with a child empty **`SpawnPoint_Ground`** carrying the exact ground position + facing yaw — that child is what a spawn manager should read.

| ID | Marker | Room | Ground pos (x,y,z) | Yaw | Exits |
|---|---|---|---|---|---|
| A | `Spawn_A_WatchPost_W3` | W3 Watch Post (NW) | -87.00, 0.33, 75.00 | 144.5 | W2 (E), W7 (S) |
| B | `Spawn_B_Purification_W4` | W4 Purification (NE) | 40.43, -3.67, 73.25 | 185.5 | W1 (W), W5 (E), W10 (S) |
| C | `Spawn_C_Isolation_W23` | W23 Isolation (E) | 89.00, -7.67, -3.70 | 253.3 | W13 (N), W22 (W), W31 (S, gated) |
| D | `Spawn_D_Servants_W32` | W32 Servants (SE) | 49.73, -5.67, -69.00 | 335.5 | W30 (N), W28 (W) |
| E | `Spawn_E_Recovery_W26` | W26 Recovery (SW) | -88.00, -5.88, -65.00 | 46.5 | W25 (N), W29 (E), W27 (S) |
| F | `Spawn_F_Convalescent_W14` | W14 Convalescent (W) | -86.00, -5.67, -1.20 | 90.0 | W8 (N), W17 (E), W25 (S) |

Verified: real floor collider at each point; 0.55 m x 1.05 m capsule clearance; **all 15 pairs LOS-blocked** at eye height; separation 64-199 m (closest E-F); hub distance 78-111 m (+/-17%).

Selection rules: perimeter only, backed to the map edge facing inward; never an objective/extraction/boss room; >=2 exits. **W1 Intake Cathedral and W28 Chapel were deliberately NOT used** — both sit on the central nave, so spawning there would hand someone the fastest lane to the hub. The spine stays unowned contested ground.

Legacy markers kept but superseded: `Spawn_A_IntakeHall_LEGACY_VOID` (stood in void), `Spawn_B_Entrance_LEGACY`.

### Extractions — 2 built, 5 proposed
Built: `Extract_WasteDisposal_W6` (90,-1.7,60.8, behind LockedDoor) and `Descent_to_Floor2` (0,-5.7,-87.2).
Problem measured: nearest-exit distance ranges 51 m (Spawn B) to 116 m (Spawn F) — a 2.3x start-position lottery.
Proposed (see PDF §4.3): add E3 W12 scullery hatch, E4 W29 corpse-cart tunnel (behind the existing one-way gate), E5 W8 sally port; **only 2 of the 5 open at a time on a phase rotation**, which fixes the lottery structurally rather than geometrically. Every extraction = 20 s uninterrupted channel + map-wide audio cue + light pillar.

### Objective ladder (proposed, PDF §4.2)
1. **Ward Seals** — 3 of the 7 existing objective markers (W2/W5/W9/W18/W19/W21/W28) activate per match, randomised. 45 s loud channel + beacon; yields a Ward Sigil + cache.
2. **Ward Matron** (W30) — gate opens once **any** 3 Sigils are banked by anyone in the lobby. Drops best floor loot + Descent Key.
3. **Infected Containment** (W31, optional) — greed pocket, one way in/out, deepest room (-11.67).
4. **Opportunistic loot** — value = distance-from-spawn x gate count.

Match shape: 6 players, 15 min, three 5-min phases (Descent / Contest / Collapse).

### Enemies (proposed, PDF §5)
Density rings keyed to `SimpleMobAI` (detect 15 m, attack 2.5 m): Ring 0 spawn rooms = **0 enemies within 25 m of a spawn**; Ring 1 perimeter 2-3/room (~28); Ring 2 mid/objective 4-5 + elite (~48); Ring 3 hub pack of 6 (90 s respawn) + gated wing 8 + boss + 4 elites. Floor budget ~95-105 concurrent AI. Placement: stair **tops** not bottoms, corridor **mouths** not middles, never within 15 m of a blind doorway, anchor to the existing pillars/`Cover_Crates`, nothing on the central nave.

### POIs
Miniboss W30 Ward Matron; Optional Containment W31; 6 ladders, 5 balconies (**balcony geometry does not exist yet — markers only**), 4 locked doors, 3 one-way gates.

## Economy
TBD.

## Resources
- **Stamina (player only, 2026-07-24):** souls-style pool gating roll (25) + attack (20) + sprint (15/s while moving). Max 100, regen 35/s after a 0.8 s post-spend delay (sprint blocks regen); at 0 actions are denied (heavy-breathing audio cue) and sprint can't re-engage until 15% refills. Mobs have no stamina. No HUD yet — `Stamina.OnStaminaChanged(current,max)` is the ready UI hook. Component: `Assets/Core/Scripts/Gameplay/Combat/Stamina.cs`.

## Progression
TBD.

## Buildings
TBD.

## Heroes
TBD.

## Enemies
Miniboss + containment POIs are placed as markers only. Enemy roster/behavior TBD.

## Combat Systems
- **Class move sets (2026-07-25):** `WeaponMoveSet` ScriptableObject per class (`Assets/Core/Data/Combat/`) — ordered `AttackDefinition[]` (clip, damage multiplier vs `BaseDamage`, lunge force/duration, camera shake, normalized hit window). `PlayerCombat` owns combo advancement AND stamina payment (a press only costs stamina when it actually becomes a swing: fresh start, or one accepted chain press per swing from 40% of the current swing — mash is free and ignored) (`ComboStep` int + `Attack` trigger), drives code-timed hitbox windows (no animation events needed on mocap clips) and per-attack feel. Animator states `Attack1..3` play the set's clips; `AttackStateLunge`/`CharacterAttackState` behaviours unchanged.
- **Knight (first class, 1h right sword):** LIGHT (LMB) 4-hit looping chain — fast R cut → L cut → low cut → thrust (1.0-1.3×, 15-18 stamina); HEAVY (RMB/RT) 2-hit chain — overhead → whirl (1.8×/2.2×, 30-35 stamina), reachable fresh or branching from any light swing; heavy→light forbidden. Early presses input-buffered. `KnightMoveSet.asset`.
- Planned: more classes = new `WeaponMoveSet` + (if rig differs) animator override controller. Weapon types beyond 1h sword TBD.
