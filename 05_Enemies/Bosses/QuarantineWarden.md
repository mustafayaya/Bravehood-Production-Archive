# Quarantine Warden — implementation notes

Dungeon 1 enemy #3. Heavy Controller / Frontline Enforcer. The fantasy is **containment**: he
decides where you may stand, then punishes you for standing there.

## A. Architecture

Closest existing enemy: the **Alchemical Aberration** (MobCombat/MobBrain/EnemyMoveSet stack, KCC
motor, hazard drops). The Warden is that stack plus five generic extensions — nothing Warden-specific
lives in runtime code; his identity is entirely in `QuarantineWardenMoveSet.asset` and the prefab.

| Layer | Component | Role for the Warden |
|---|---|---|
| Data | `EnemyMoveSet` → 7 `EnemyAttack` | timings, ranges, weights, cooldowns, displacement, hazard params, situational weights |
| Execution | `MobCombat` | Startup/Active/Recovery clock, hitbox windows, hazard drops (`HazardForwardOffset`, `HazardOnImpact`), copies `Displacement` into the hit payload |
| Decision | `MobBrain` | perception, LOS, legality, weighted pick with `SituationalWeight()` (near-hazard / flanking / surrounded), rage = malfunction, post-attack retarget |
| Victim side | `HitReaction` + `BaseCharacterController.ApplyDisplacement` | push/pull through the KCC (collides; pull stops 1.4 m short), `HardDisplacementThreshold` 0.75 m → `DisplacementImmunity` 1.2 s (×0.25 strength) |
| Hazard | `HazardField` + `Status_Quarantine` | Line zone (6 s) and Slam residue (2.5 s); contamination DoT 3/s for 4 s, move ×0.85 |
| Physics | `ChainSimulator` ×3, `Cloth` | censer chain + head, left-arm chain + lantern, belt lantern; tabard cloth |
| Build | `Assets/Editor/WardenSetup.cs` | `Bravehood/Enemies/Setup Quarantine Warden` rebuilds clips, controller, move set, prefab; `Warden - Place In Test Range` |

Gaps found and filled (all reusable): per-attack displacement + CC protection window, line-of-sight
hitboxes, situational weights, hazard placed *ahead* of the target, hazard spawned by a melee impact,
rage jitter/turn-scale, post-attack target switching, Verlet bone chains.

Skipped on purpose: prosthetic damage/break (no limb-state system exists; the body-part multipliers
already make the prosthetic arm a slightly juicier target, which is the cheap version of the idea).

## B. Files

New: `Assets/Editor/WardenSetup.cs`, `Assets/Core/Scripts/Gameplay/ChainSimulator.cs`,
`Tools/Rigging/rig_quarantine_warden.py`, `Assets/Characters/Enemies/QuarantineWarden.fbx`,
`Assets/Core/Animations/QuarantineWarden.controller`, `Assets/Core/Data/Combat/QuarantineWardenMoveSet.asset`,
`Assets/Core/Data/Combat/Status_Quarantine.asset`, `Assets/Core/Prefabs/Gameplay/Mob-QuarantineWarden.prefab`.

Modified: `CombatTypes.cs` (HitPayload.Displacement), `IDamageable.cs` (DamageInfo.Displacement),
`Hitbox.cs` (RequireLineOfSight / LineOfSightBlockers / LineOfSightEyeHeight), `HitReaction.cs`
(applies displacement), `BaseCharacterController.cs` (ApplyDisplacement, immunity),
`EnemyMoveSet.cs` (Displacement, HazardForwardOffset, HazardOnImpact, WeightWhenTargetNearHazard /
Flanking / Surrounded), `MobCombat.cs` (payload displacement, hazard offset/impact, RecoveryJitter),
`MobBrain.cs` (situational weights, retarget, rage extras), `HazardField.cs` (static `All`),
`Assets/Scenes/test_general.unity` (Warden placed in `AberrationTestRange` at local (-9, 0, 8)).

## C. State diagram

```
            target seen
 IDLE ───────────────────► ADVANCE (walk 1.7 m/s, turn sharpness 3.2)
                              │ in range + settle 0.3 s + global cd 0.8 s
                              ▼
                         SELECT (legal set → base weight × situational weight)
            ┌──────────────┼──────────────────┬─────────────────┐
            ▼              ▼                  ▼                 ▼
        CONTAIN       FORCE MOVEMENT        PUNISH           PRESSURE
     Quarantine Line  Censer Swing (push)   Thrust / Chop     Sweep / Censer Slam
     (zone behind     Chain Recall (pull,   (×1.8 / ×3.0      (Sweep ×3 when flanked,
      target, 3 m)    LOS required)          near hazard)      ×2.5 when surrounded)
            └──────────────┴──────────────────┴─────────────────┘
                              ▼
                         RECOVER (Recovery + jitter) ──30%──► maybe switch target
                              │
               HP ≤ 33% ─────► MALFUNCTION (one-shot Roar; pacing 0.92, recovery ×1.15 ±0.35, turn ×0.7)
```

## D. Abilities

| Ability | Limb | Kind | Startup / Active / Recovery | Range | Dmg | Poise | Extra |
|---|---|---|---|---|---|---|---|
| Containment Thrust | blade L | Hitbox | 0.46 / 0.12 / 0.64 | 0–3.4 m, 35° | 24 | 34 | ×1.8 near hazard, ×0.25 if flanked |
| Warden Sweep | blade L | Hitbox (chest height) | 0.76 / 0.18 / 0.80 | 0–3 m, 110° | 20 | 26 | ×3.0 if flanked (45°), ×2.5 surrounded; ducked by the roll tuck |
| Execution Chop | blade L | Hitbox (full height) | 1.16 / 0.14 / 1.30 | 0–2.8 m | 40 | 50 | ×3.0 near hazard; timing only |
| Quarantine Swing | censer R | Hitbox, **Displacement +3 m** | 0.76 / 0.20 / 0.78 | 0–4 m, 100° | 14 | 20 | ×2.0 surrounded, ×0.6 near hazard |
| Censer Slam | censer R | Hitbox + **HazardOnImpact** (r 2.0, 2.5 s) | 1.06 / 0.15 / 1.10 | 0–3.5 m | 30 | 32 | residue cloud |
| Quarantine Recall | chain R | Hitbox, **Displacement −3 m**, LOS | 0.70 / 0.25 / 0.90 | 4.5–9 m, 30° | 12 | 14 | HitStun 0.3; never through walls |
| Quarantine Line | censer R | GroundHazard r 3.2, 6 s, **+3 m forward offset** | telegraph 1.0 s (+1.1 s cast) | 3–9 m | DoT 3/s ×4 s | — | cd 12 s, ×0.15 if target already near a hazard |

## E. Tuning (all on the prefab / move set)

| Knob | Value | Where |
|---|---|---|
| Health / Poise | 420 / 60 (regen 12/s after 2 s, armour 0.6 while attacking) | prefab |
| Speed / turn | 1.7 m/s / OrientationSharpness 3.2 | BaseCharacterController |
| Ranges | Preferred 2.4, TooClose 1.5, Detect 18, Lose 30 | MobBrain |
| Pacing | Settle 0.3, GlobalAttackCooldown 0.8, StrafeChance 0.08 | MobBrain |
| Malfunction | RageHealthFraction 0.33, Pacing 0.92, RecoveryScale 1.15, RecoveryJitter 0.35, TurnScale 0.7 | MobBrain |
| CC fairness | HardDisplacementThreshold 0.75 m, DisplacementImmunity 1.2 s, ImmuneDisplacementScale 0.25 | BaseCharacterController (victim) |
| Duo | RetargetAfterAttackChance 0.3, RetargetRecentDamageWindow 4 s | MobBrain |
| Contamination | 3 dmg/s, 4 s, move ×0.85, refresh | Status_Quarantine |
| Censer chain | Damping 0.03, TipMass 4, bend 75° | ChainSimulator #1 |
| Arm chain / belt lantern | Damping 0.10 / 0.08, Unrest 0.25 / 0.2 | ChainSimulator #2 / #3 |
| Cloth | stretch 0.95, bend 0.6, damping 0.5, top 30 % pinned, 3 trigger capsules | Cloth on `softcloth` |

## F. Multiplayer

- Hazards and hitboxes use `Targetable.AreHostile` — rival squads are contaminated and shoved too.
- Displacement goes through each victim's own motor; in a networked build it must be applied on
  the owning client (same path as knockback today). The immunity window is per-victim.
- Target switching only after a *committed* sequence (post-attack, 30 %, biased to whoever hit him
  in the last 4 s) — no mid-swing flips.
- Sweep's surrounded/flank weighting handles a duo pincer; the Line's forward offset targets one
  player but the zone is large enough to split a pair.

## G. Animation / VFX / SFX hooks

Animation (all retargeted stand-ins today): Thrust/Sweep/Chop ← OrcHammer mirrored (left arm);
CenserSwing/Slam ← OrcHammer unmirrored; Recall ← Staff Blast_02; Line ← Staff FloorBlast_04;
Roar = malfunction one-shot; GetHit/GetHitHeavy/Death; 8-way Walk/Jog. Minimum bespoke set if
mocap is commissioned: blade thrust, blade sweep, overhead chop, censer arc, censer slam, chain
cast, ground cast, malfunction twitch idle — 8 clips.

VFX hooks: `HazardField` warn/live colours (Line and Slam residue), `MobCombat` cast-charge orb +
point light on R_Hand (Recall / Line), `MobBrain.OnRageBegan` UnityEvent (leak/spark emitters),
`ChainSimulator.Kick()` for impact rattles. SFX: `CharacterVoicePlayer`/`FootstepPlayer` are wired
with PlagueThrall placeholders — wants armour-clank footsteps, chain rattle (drive from censer chain
velocity), mechanism hiss on malfunction, zone hum.

## H. QA checklist

- [x] Thrust / Sweep / Chop land and respect facing cones
- [x] Quarantine Swing shoves ~3 m; immunity window blocks a follow-up Recall chain (observed 2.0 → 4.7 m, immune=true)
- [x] Recall pulls ≤3 m and stops short (observed 6.0 → 3.1 m)
- [x] Recall not used without line of sight (wall test: he pathed around instead)
- [x] Quarantine Line spawns behind the player with ~2 s warning; contamination ticks; he advances after
- [x] Censer Slam leaves a 2.5 s residue hazard
- [x] Malfunction fires at 33 % (`IsRaging` true, Roar state entered)
- [x] Chains swing with motion, cloth stable (no stretch beyond 1.4 m local radius across 60 s)
- [x] Rig re-verified 2026-08-23: both arms bend, blade is one prosthetic unit from the elbow, censer on its chain from the right hand, lanterns on the belt, chest chain in place (posed Blender renders + in-game shots)
- [x] v2 remodel rigged from scratch 2026-08-23 (bone heat on clean shells, blade/helmet rigid, chains per-link, cloth on both tabards); verified in Blender posed renders + play
- [x] Clip mirror flags confirmed by hand-position sampling: blade attacks lead left, censer attacks lead right
- [x] No console errors/warnings in a 60 s fight
- [ ] Duo retarget — needs a second player in the range (logic shared with the Aberration)
- [ ] Sweep whiff on the roll tuck — verify by hand (hitbox is at 1.2–2.1 m height)
- [ ] Real dungeon: needs the Floor-1 NavMesh bake and a large agent type (radius 0.7)
