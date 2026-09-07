# Bravehood — Dungeon 1 Enemy Roster

**The Black Veil Sanatorium.** Living document; enemies are designed one at a time and this file
is updated as each lands.

| # | Enemy | Tier | What it teaches | Status |
|---|---|---|---|---|
| 1 | Plague Thrall | Trash | nothing — it is the noise floor | shipped (pre-existing) |
| 2 | **Alchemical Aberration** | **Elite** | spacing, and respecting a telegraph | **implemented** |
| 3 | Quarantine Warden | Standard / Heavy Controller | spacing under displacement; hazards behind you | **implemented** |
| 4 | **Ward Matron** (nurse) | **Support** | target priority | **implemented** |
| 5 | Censer Wretch | Ranged | forces commitment | designed, not built |
| 6 | Mortuary Grinder | Elite | disengaging vs. greed | outline |
| 7 | **Veil Wraith** | **Stealth / Disruption** | hunting by observation; trusting your senses | **implemented** |
| 8 | The Pallid Physician | Boss | the final exam | outline |

---

## The constraints every one of these is designed against

These are measured from the project, not assumed. They are the reason the numbers below are what
they are.

- **The player has no block, no parry and no ranged attack.** The only defensive verb is a
  **2.28 m roll with 0.30 s of i-frames, costing 25 of 100 stamina, on a 1.38 s roll-to-roll
  cycle**. So nothing may threaten faster than ~1.4 s between committed attacks, or it stops
  being hard and becomes undodgeable.
- **The roll tucks the collision capsule to 1.2 m for its whole 0.98 s.** Chest-height horizontal
  swings therefore whiff by *geometry*, long after the i-frames have expired; ground-level and AoE
  attacks do not. **This is the game's real anti-roll-spam tool and it is an explicit design axis
  here** — see the Aberration's Swipe/Slam pair.
- **The player has 100 HP and zero mitigation.** Armour is a 17% speed swing and nothing else; it
  grants no defence and no poise. Every point of enemy damage comes straight off the bar.
- **Player poise is 45**, regenerating 25/s after 1.2 s. Enemy poise damage is what decides whether
  an attack merely hurts or takes control away.
- **Rooms are ~15.4 x 13.4 m typical**, smallest 11.5 x 7.7 m, side corridors 4–5 m.
  **Trash and standard abilities cap at 6 m. 7 m+ is boss-only.**
- **This is an extraction game.** Fight *duration* is the hidden cost — every extra second is a
  second a rival squad hears you and closes in. Design for fights that resolve.

## Two changes from the original design

**1. The support slot is the Ward Matron, and she does heal.**
I originally cut the healer — a healer's function is to make fights longer, and in an extraction
game longer fights are already the punishment. That reasoning holds for a *miniboss* and is much
weaker for a *regular* enemy, which is what she became: frail, quick to kill, resolved in seconds.
The narrative case is also plain — nurses in a sanatorium should be common, and a nurse should
treat people. See her section for the guards that keep the heal a priority rather than a wall.

The **shriek** idea from that first pass is still unused and still good: on aggro, ping your
location map-wide and pull nearby mobs. It is PvPvE-native in a way nothing else in the roster is.
It is not slotted anywhere yet — it can be its own cheap enemy or an addition to an existing one.

**2. The Aberration is an Elite, not standard trash.**
Four attacks plus a passive breaks the "1 basic / 1 heavy / 1 signature / 1 mechanic" rule the
original doc set for normal enemies — correctly. Rather than trim the dungeon's signature monster,
spawn it at roughly **1 per 2–3 rooms** and keep the lean rule for the actual trash tier.

---

## 2. Alchemical Aberration — IMPLEMENTED

An experiment that cannot control the chemicals being pumped through its body. **2.40 m tall**
(the Thrall is 1.83 m), with an oversized alchemical gauntlet on its **left** arm — every attack
leads with it.

**260 HP · Poise 40** (regen 15/s after 1.6 s, stagger 0.75 s, **ArmourWhileAttacking 0.35**).
Mid-swing it shrugs off pokes: a full light string or a heavy breaks it, a single jab does not.

**Walk speed 2.6 m/s.** Deliberately faster than the player's walk (1.94) and slower than the
player's sprint (3.88) — it can be kited, which is the entire reason the Frenzied Rush exists.

| Attack | Startup | Active | Recovery | Total | Damage | Poise | Beaten by |
|---|---|---|---|---|---|---|---|
| Long Arm Swipe | 0.76 | 0.14 | 0.55 | **1.45 s** | 18 | 22 (3 hits to stagger) | **movement** |
| Crushing Slam | 1.34 | 0.16 | 0.85 | **2.35 s** | 30 | **50 — staggers in one** | **timing** |
| Frenzied Rush | 0.95 | 0.24 | 0.40 | **1.59 s** | 23 | 30 (2 hits) | **sidestepping** |

**The Swipe/Slam pair is the heart of the design.** The Swipe's damage volume sits at chest height
(y 1.22 → 2.07), so a rolling player ducks it by geometry. The Slam's sits on the floor
(y −0.20 → 1.20), so ducking does nothing and only the 0.30 s i-frame window saves you. Two
different skills, taught by one enemy, using a rule the engine already had.

Eating a Slam is a real punish and never a stun-lock: it staggers for 0.55 s while the Aberration
owes 0.85 s of recovery plus a 0.9 s global cooldown.

**Frenzied Rush** — the anti-kite answer. It squares up (must be facing within 35°), revs for the
first 30% of the wind-up without closing, then commits. **Direction locks at the moment it commits
and never homes after that**, so a sideways dodge beats it. Charging into level geometry stops it
dead and costs an extra 0.45 s of recovery — baiting it into a pillar is a real, rewarded play.
Range 5–11 m, 7 m/s, 6 m cap, 7 s cooldown.

**Unstable Chemistry** (below 30% HP): Pacing drops to 0.87 (≈15% faster) while every recovery
stretches by 1.25×. It threatens sooner *and* becomes more punishable — scarier and fairer at once.
Raising its damage instead would just have made it a bigger number. No death explosion: in a PvPvE
extraction game those punish whoever won the fight.

**Brain:** >5 m Rush · <3.2 m Swipe (weight 55) / Slam (25) · 20% chance to reposition instead ·
0.35 s settle on arrival so the run-to-melee transition is readable · won't commit the Rush
without line of sight.

### Still to build (Stage C)
The **Alchemical Burst** — the 60° / 5.5 m poison cone with lingering contaminated ground — is
designed but not implemented. It needs three systems that do not exist yet: a cone/sphere hitbox
shape, a status-effect/DoT framework, and ground-telegraph rendering (URP decals are not enabled
and VFX Graph is not installed). Its source mocap is already converted to Humanoid and
ready to retarget (`MCU_am_Ready_Idle_Conjure_04_SideGrabsBlast`), but the trimmed clip is not
authored yet. The three-attack enemy is complete and shippable without it.

---

## 3. Quarantine Warden — IMPLEMENTED (Heavy Controller / Frontline Enforcer)

The containment fantasy: he does not chase you, he **herds** you. Blade prosthetic on the left
arm, chained censer-lantern in the right hand. Loop: ADVANCE → CONTAIN (Quarantine Line planted
*behind* the player) → FORCE MOVEMENT (Censer Swing shove / Chain Recall pull) → PUNISH (Thrust /
Chop, both weighted up when the target stands next to a hazard) → RECOVER. ~1.6x HP, very high
poise, slow turn so circling works. Below 33% he **malfunctions**: faster blade, sloppier
recovery, worse tracking. Full design, tuning, state diagram and QA list: `AI/QuarantineWarden.md`.

Files: `Assets/Editor/WardenSetup.cs` (one-button build), `Mob-QuarantineWarden.prefab`,
`QuarantineWardenMoveSet.asset`, `QuarantineWarden.controller`, rigged model
`Assets/Characters/Enemies/QuarantineWarden.fbx` (`Tools/Rigging/rig_quarantine_warden.py`).
New shared systems: per-attack **Displacement** (push/pull through the KCC, with a protection
window), **line-of-sight hitboxes**, situational attack weights (near-hazard / flanking /
surrounded), hazard forward-offset, hazard-on-impact, `ChainSimulator` (Verlet bone chains).

## 4. Ward Matron — IMPLEMENTED (regular support enemy)

A former senior nurse who still compulsively tries to *treat* everyone she meets. **1.95 m** —
deliberately human-scaled, so she reads as a dangerous *person* next to the 2.40 m Aberration.

**She is a REGULAR enemy, not a miniboss.** Nurses are common in a sanatorium; making her rare
fought the fiction. She now fills the support slot the Plague Attendant was written for.

**150 HP · Poise 26** (regen 12/s after 2.0 s, stagger 0.6 s, **ArmourWhileAttacking 0.90**).
Below the Plague Thrall's 200 on purpose: she is the one you are meant to kill first, so she has
to die quickly when you commit. 26 poise means roughly **two light hits break her**, which is what
makes her heal interruptible by any player. Walk 2.3 m/s.

| Ability | Kind | Startup | Active | Rec | Range | Effect |
|---|---|---|---|---|---|---|
| Surgical Jab | melee | 0.37 | 0.10 | 0.43 | 0–2.6 m | 12 dmg, 14 poise, applies **Palsy** |
| Hemorrhage Injection | melee | 1.08 | 0.12 | 0.60 | 0–2.8 m | 22 dmg, 30 poise, applies **Bleed**; cd 8 s |
| Emergency Treatment | support | **2.00** | 0.05 | 0.55 | 0–14 m | **heals an ally 60 HP**; reach 8 m; cd 12 s |

No rage phase — that was a miniboss beat, and a regular enemy does not need a second act.

**Statuses** (all new, all reusable):
| Status | Effect | Why these numbers |
|---|---|---|
| Bleed | 3/s for 5 s = **15 total** | The player has no cure item, no healing and no mitigation, so a DoT is unavoidable damage once applied. It **refreshes rather than stacks** — a stacking bleed on a miniboss would be a death sentence with no counterplay. |
| Palsy | −18% move speed, 4 s | No damage. It taxes **mobility**, the player's only defensive resource, so the real cost of eating a Jab is that her next zone is harder to leave. |
| Contaminated | −10% move, 2/s, 3 s | Applied by standing in a zone; lingers after you leave, so clipping the edge still costs. |
| Stimulant | +25% move, ×0.75 pacing, 2 HP/s burn, 12 s | What Triage actually does. |

### Emergency Treatment — why the heal works *here*

The standing objection to healers is that they stretch fights, and in an extraction game a
stretched fight is what gets you third-partied. That objection is real on a **miniboss** and much
weaker on a **regular** enemy: she is frail, dies in a couple of exchanges, and the whole
interaction resolves in seconds.

The numbers keep her a **priority, not a wall**: 60 HP on a 12 s cooldown is ~5 HP/s of sustained
output against a player who deals ~32 HP/s. Ignoring her is survivable; ignoring her while fighting
three other things is not. That is exactly the target-priority lesson the slot exists to teach.

Two guards make it read correctly rather than feeling cheap:

- **She will not channel on a healthy ally.** The ability ignores anyone above 85% HP and picks the
  *most wounded* target in range. Without this she burns a 2 s channel topping someone up who does
  not need it, which looks broken and wastes the opening.
- **The 2.0 s channel is the interrupt window, and it is genuinely interruptible.** Against 26 poise
  at 0.90 armour, two light hits break her. *Verified in play: breaking her mid-channel denied the
  heal entirely — the patient stayed at 40/200.*

### Reserved for a later enemy

**Quarantine Order** (telegraphed 3.5 m ground zone) and **Call the Attendants** (summon 2 Thralls,
health-threshold gated) are **removed from her move set but fully preserved in code**:
`MobAbilityKind.GroundHazard` and `.Summon`, `HazardField`, the `Bravehood/VFX/HazardZone` shader,
`Hazard_QuarantineZone.prefab` and `Status_Stimulant` are all intact and untouched. Dropping either
onto a future enemy is an entry in a move set, not new systems.

## 7. Veil Wraith — IMPLEMENTED

A spectral remnant of someone the Sanatorium consumed and the Veil refused to let go. **~2.0 m**,
thin, barefoot; a green-cyan ghost that is *invisible for most of the encounter*. Its rule:

> **The Wraith may be invisible, but it is never informationless.**

Loop: **Detect → Predict → Expose → Punish → Disappear.** Built on the same `MobBrain` /
`MobCombat` / `EnemyMoveSet` layer as the Aberration and the Matron, plus four new systems that
any later Black Veil enemy can borrow.

**120 HP · Poise 24** (regen 14/s after 1.8 s, stagger 0.7 s, ArmourWhileAttacking 0.6). Fragile
on purpose: its HP is the length of the exposure window, not the length of the fight. A stagger
*forces it visible* for its whole duration.

**Speed:** 2.0 m/s while stalking, 3.1 m/s when it commits, 3.4 m/s retreating.

| Attack | Startup | Active | Recovery | Total | Damage | Poise | Veil | Visible? |
|---|---|---|---|---|---|---|---|---|
| Veil Grasp (signature) | **1.05** | 0.14 | 0.72 | **1.91 s** | 24 | **46 — staggers in one** | **+22** | **must manifest**: visibility tracks the Startup clock exactly, so "it is solid" and "the grab is now" are the same frame |
| Spectral Swipe | 0.62 | 0.12 | 0.60 | 1.34 s | 12 | 16 | +6 | a **flicker** (38%) in the wind-up, never a body |

Retimes measured on its own rig: Grasp 1.05x / Swipe 1.03x. Grasp reaches the floor (y −0.0 → 1.9),
so a roll only beats it on the i-frames; the Swipe is chest-height and whiffs over a tucked roll.
After **any** attack it stays fully visible for **1.6 s** (the punish window), then fades over 0.9 s.

### The encounter beat (`WraithStalker`)
- **Stalking** 4–8 s: hidden, circles at ~4.8 m (strafe chance 0.8). Attacks are vetoed via the
  new `MobBrain.AttackFilter` hook. This is when the *room* is talking.
- **Committing**: closes to 2.1 m and the brain may attack. Times out after 6 s if it never gets
  there — a wraith that cannot reach you does not stand in the open.
- **Retreating** 2.2 s after the swing, while `WraithPhase` keeps it exposed; then it stalks again.
- **Hit while hidden** (a correct prediction): it *flashes* visible for 0.55 s, and then 60 % of
  the time it turns on you at once, 40 % it darts away. The reveal is certain; the consequence is not.

### Never informationless (`WraithPresence` → `VeilReactor`s)
Every 80 ms it stimulates every reactor within 6.5 m, scaled by distance² and by speed (a still wraith
is a faint draught, a moving one slams doors). Reactors are passive and know nothing about wraiths:
- `VeilReactiveFlame` — drop on any candle/torch prefab. Light gutters and leans away; above 0.45 (a walking pass within ~2 m) it
  **goes out** and relights 6–11 s later. A corridor of them goes out **in sequence** by geometry alone.
  A hidden wraith casts **no shadow** (`WraithPhase.CastShadowWhenHidden` is off; renderers are
  `ShadowCastingMode.Off` until it shows). The optional ShadowsOnly mode is kept for a variant that
  lamps are meant to expose.
- `VeilReactiveCloth` — real `Cloth` gets `externalAcceleration`; a plain sheet swings on its rail.
- `VeilReactiveRattle` — bottles/chains shiver and clink on a cooldown.
- `VeilReactiveWater` — ripples (the HazardZone ring shader, recycled) from its feet: the one
  environment that gives away its *exact* position.
- A **3-D whisper loop** that swells as it closes, **chain rattles** while it moves, a faint mote
  **wake**, and a 3.5 % **moving shimmer** a veteran can catch in fog.

### Revealing Dust (`RevealingDustThrower` on the Player, **G** / right shoulder, 3 charges)
Arcs toward the lock-on target or along the camera; bursts on impact or after 1.6 s into a 3.2 m cloud
for 1.4 s. Any wraith inside is **dusted for 7 s**: a 50 % visibility floor plus a bright speckle coat
(`_DustAmount` in the shader), and it becomes lockable. No damage — its whole value is information,
which is what keeps it a preparation reward instead of a required key.

### Veil (`VeilAffliction` + `VeilHallucinations` on the Player)
A 0–100 pool, decaying 3/s after 6 s quiet. Fed by Grasp (+22), Swipe (+6) and **proximity**
(0.9/s at point-blank while hidden, 2.0/s while manifest, falling off to 5.5 m). First pass had
30/8/1.6/3.2 and hit 100 inside 30 s of a passive fight — the meter is meant to be the cost of a
whole encounter, not of one exchange.
| Level | At | What happens |
|---|---|---|
| 1 | 25 | green-black vignette, desaturation, grain; **false footsteps** behind you, 5–11 s apart, built from *your own* footstep audio |
| 2 | 50 | **phantom wraith silhouettes** at 55–110° off the camera (the real model + ghost material, no logic) with a whisper from *elsewhere*; lens/chromatic **spikes**; the real wraith's whisper emitter is thrown 3.5 m off its body |
| 3 | 75 | everything faster; a **squad-mate** briefly reads as monstrous (the phantom overlay parented to them) |

Every lie is made of a real signal, so a fake and a real cue are indistinguishable on first read.
Post-processing is a private runtime `Volume` (priority 50) — the scene profile is untouched.

### Seen in play (test_general, passive player)
- Stalks 4–6 s → commits at ~2.3 m → Grasp: vis 0.03 at wind-up start, 0.88 at 0.9 s, Active at
  full manifest with the shadow mode flipped to On; exposed 1.6 s → retreats to 4.8 m → vis 0.2
  and hidden again 2.3 s after the swing. Loop repeated 7 times without drift.
- A 15-damage hit on the hidden body flashed it and it committed on the spot (the 60 % branch).
- A dust cloud on it: `IsDusted` true, visibility floor up, `Concealed` false (lockable).
- Veil: L1 after the first Grasp, L2 at ~15 s, L3 at ~19 s with the original numbers (now tuned
  down, see above); the runtime Volume reached weight 1.0 with five overrides live; phantom
  silhouettes spawned 55–110° off camera at the prefab's 0.5–0.7 peak.
- Body-part multipliers apply to it like any mob: a head-hit Grasp read 36 on the player.

### Assets
`Mob-VeilWraith.prefab`, `VeilWraithMoveSet.asset`, `VeilWraith.controller` (Undead pack: Idle_01,
Walk_02/Jog_01 1-D, Attack_07_BothHands = Grasp, Attack_01_RHand = Swipe, Hitreact_01, Hitreact_12
fall), 31 `M_Wraith_*` materials on `Bravehood/VFX/WraithGhost`, `Phantom_VeilWraith`,
`RevealingDust_Pouch/Cloud`, `WaterRipple` prefabs. **Audio is procedural placeholder** (synthesised
in `WraithSetup.BuildAudio` — whisper, chains, bottles, candle out/relight, manifest/vanish stingers,
shriek/hurt/death) because no generation key is configured; swap the `.wav`s in
`Assets/Core/Audio/VeilWraith` and the `AudioData` assets keep working. Test range:
`WraithTestRange` in test_general (candle row, curtains, bottle shelf, flooded patch).
Build/rebuild everything with **Bravehood/Enemies/Setup Veil Wraith**.

### Still open
- No HUD element for the Veil meter (a debug bar at the bottom of the screen stands in — UIAgent job).
- Revealing Dust is a component with a charge count, not an inventory item.
- `Candlelight_01.prefab` carries two missing-script components; the reactor drives its Light only.
- Level rooms need reactors placed (LevelDesignAgent: every candle/torch/curtain/basin in the ward wing).

## Unslotted idea — the Shriek

Frail, fast, cowardly. On aggro it **shrieks**: a map-wide audio ping revealing the fight to every
other squad, plus a pull and short enrage on nearby mobs. Retreats behind whatever is bigger than
it. Killing it fast matters because of who *else* is coming. Not assigned to an enemy yet.

## 5. Censer Wretch — Ranged

Lobs a burning censer in an arc, leaving a small burning pool. The anti-turtle enemy and the only
reason to close distance fast. Necessary because the player has **no way to trade at range at all**
— which makes projectile density the thing to keep low and telegraphs long.

## 6 & 8 — Outline only

**Mortuary Grinder** (Elite): heavy hitter, hook grab, bleed — Bleed now exists, and the OrcHammer
mocap set covers the kit almost 1:1. **The Pallid Physician** (Boss): three phases — telegraphs and
positioning, then target priority under summons, then area control. Every system his phases need
(zones, summons, status, health-threshold gating) is now built.

---

## Systems these still need

| Gap | Blocks |
|---|---|
| ~~Status effects / DoT~~ | **BUILT** — `StatusEffects` + `StatusEffectDefinition`; Bleed, Palsy, Contaminated, Stimulant |
| ~~Ground hazards + telegraphs~~ | **BUILT** — `HazardField` with a fill-in warning disc (no decal pipeline needed) |
| ~~Summoning / ally support / phase gating~~ | **BUILT** — `MobAbilityKind` on the move set: hazard, summon, ally injection, health thresholds |
| ~~Displacement (push/pull) + CC protection window~~ | **BUILT** — `HitPayload.Displacement`, `BaseCharacterController.ApplyDisplacement` |
| ~~Line-of-sight hitboxes~~ | **BUILT** — `Hitbox.RequireLineOfSight` |
| ~~Bone-chain physics~~ | **BUILT** — `ChainSimulator` |
| Cone/sphere hitbox shapes | the Aberration's Alchemical Burst |
| Projectiles + pooling | Censer Wretch, boss vials |
| Proper VFX (the zone discs are placeholder unlit geometry) | polish pass on every AoE |
| Boss health bar | `BossBarUI` exists with a Show/Hide API but is not wired to anything |
| Enemy spawners; no enemy markers exist in any dungeon scene | populating the floor |
| Floor 1 NavMesh is unbaked (176 verts) | **all AI is inert in the real dungeon today** |
| A NavMesh agent type for large enemies | the 2.4 m Aberration paths through 0.5 m-radius gaps it cannot fit |
| Perf/LOD strategy | the ~100-concurrent-AI target in GAMEPLAY.md |
