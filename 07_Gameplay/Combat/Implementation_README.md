# Bravehood melee combat

Frame-data combat. The move set is the truth; the animator is told what to play and how fast.

## Where things live

| File | Job |
| --- | --- |
| `WeaponMoveSet.cs` | `AttackDefinition` (frame data, cancels, motion, damage) + the class's chains and entry points |
| `PlayerCombat.cs` | The state machine. Owns the clock, the cancel matrix, the combo tree, the animator |
| `System/CombatTypes.cs` | Enums, `ComboRef`/`ComboLink`, `ActionTiming`, `HitPayload`, `HitReport` |
| `System/CombatInputBuffer.cs` | The buffer. A press is never discarded for being early |
| `System/Poise.cs` | Poise pool + stagger. Decides whether a hit interrupts |
| `System/HitStop.cs` | Per-character impact freeze (never `Time.timeScale`) |
| `System/CombatEvents.cs` | One-way broadcast of hits and actions, for feel / UI / future netcode |
| `Hitbox.cs` | Swept, team-filtered damage volume |
| `Targetable.cs` | `TeamId` + `SquadId` + the single `AreHostile` rule |

## The timing model

Every attack is `Startup → Active → Recovery`, authored in **seconds**. The clip is fitted to
those numbers, not the other way round:

- `ClipStart` … `HitWindowStart` is squashed or stretched to fill `Startup`
- `HitWindowStart` … `ClipEnd` is fitted to `Active + Recovery`
- the ratio is pushed to the animator as the `AttackSpeed` parameter, which every attack state
  uses as its speed multiplier

**Frame data is authored to the clip's own natural timing.** `Startup` is however long the clip
takes to travel from its trimmed `ClipStart` to its contact frame, and `Active + Recovery` is
whatever the follow-through actually takes. Authored that way, both retime factors come out at
exactly **1.00x** — nothing is sped up, and the mocap reads at its natural weight.

To make the whole class faster or slower, use **`WeaponMoveSet.Pacing`** rather than editing six
attacks by hand. 1.0 is the authored data at natural animation speed; 1.15 reads heavier (clips
stretch to fit); 0.85 reads snappier (clips speed up). The playback rate is simply `1 / Pacing`,
so anything past roughly 0.85–1.2 starts to look visibly re-timed.

Per-attack, `Startup` still overrides everything — just remember that shortening it below the
clip's natural wind-up speeds that clip up by the same ratio.

Current Knight numbers, all at 1.00x playback:

| Attack | clip | Startup | Active | Recovery | Total | chain@ | on hit@ | free@ |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Fast Cut | 1.27 s | 0.253 | 0.100 | 0.457 | 0.811 | 0.605 | 0.468 | 0.509 |
| Left Cut | 1.70 s | 0.306 | 0.100 | 0.478 | 0.884 | 0.669 | 0.526 | 0.569 |
| Low Cut | 1.90 s | 0.323 | 0.110 | 0.498 | 0.931 | 0.707 | 0.558 | 0.602 |
| Thrust | 1.50 s | 0.330 | 0.110 | 0.550 | 0.990 | 0.759 | 0.578 | 0.627 |
| Overhead | 1.97 s | 0.511 | 0.130 | 0.696 | 1.337 | 1.073 | 0.815 | 0.878 |
| Whirl | 3.00 s | 0.510 | 0.220 | 0.710 | 1.440 | 1.170 | 0.908 | 0.971 |

A full 4-hit light combo runs **2.97 s** (~1.3 hits/sec). Heavies land in 0.51 s.

### The number that sets the cadence

`ChainAtRecovery` (0.55 by default) is where in the RECOVERY the chain window opens. Because a
held button takes every window the moment it opens, this *is* the combo's rhythm: one swing every
`startup + active + recovery x ChainAtRecovery`.

Opening it during the active frames — which an earlier version did — cuts the previous swing off
before it has visibly finished and the animation machine-guns. If attacks feel spammy, this is the
number to raise, not `Startup`.

`HitConfirmChainAtRecovery` (0.25) is the same point after the swing has **connected**: landing a
hit buys roughly 0.14 s of follow-up, which reads as reward without becoming a machine gun.

## The cancel matrix

| Phase | Chain into another attack | Dodge | Move | Turn |
| --- | --- | --- | --- | --- |
| Startup | no | **yes, until `FeintUntil`** (feint) | `StartupMove` (0.30) | `StartupTurn` (0.55) |
| Active | from `ChainFrom` (mid-active) | **no — committed** | `ActiveMove` (0.06) | `ActiveTurn` (0.05) |
| Recovery | from `ChainAtRecovery` through it | yes, from `DodgeDelay` after the hitbox shuts | ramps to full | ramps to full |
| After `MoveCancelFrom` | yes | yes | full — walking blends out of the swing | full |

Two rules carry most of the feel:

- **Nothing is committed except the active frames.** A whiff is punishable, a connecting swing
  can't be escaped, and everything else is yours.
- **Landing a hit opens the chain window earlier** (`HitConfirmChain`, at
  `HitConfirmChainAtRecovery` instead of `ChainAtRecovery`). Connecting buys the faster follow-up;
  whiffing does not.

Dodge presses buffer too (`BaseCharacterController.RollInputBuffer`), so pressing dodge during
committed frames fires it on the first legal tick instead of being dropped.

## Combos

Default rules, no authoring needed: light advances the light chain and loops; heavy advances the
heavy chain and stops at the finisher; crossing over drops you at the other chain's opener.

Author `AttackDefinition.Links` to branch. The Knight's Thrust (light finisher) links
`Heavy → Heavy[1]`, so finishing the light string gives you the Whirl without paying its wind-up.

Entry points on the move set pick the opener by situation:

- `NeutralLight` / `NeutralHeavy` — standing or walking
- `SprintLight` / `SprintHeavy` — while sprinting (Low Cut / Whirl)
- `DashLight` / `DashHeavy` — **dash attacks**: press attack during a dodge past
  `PlayerCombat.DashAttackFrom` (0.22 of the roll) and the dash converts into the swing
  (Thrust / Overhead). Before that point the dodge is still committed, so i-frames can't be
  cancelled on the launch frame.

`ComboMemoryTime` (0.7 s) keeps the string alive after a swing ends, so a slightly late press
continues the combo instead of restarting it.

**Holding the attack button keeps the combo going** (`AutoChainWhileHeld`, on by default): every
window that opens is taken, looping the light chain until stamina runs out. It deliberately only
*continues* a live combo and never starts one from cold neutral, so resting a finger on the button
never swings by itself.

## Poise, stagger and hit-stop

`Poise` is the **only** thing allowed to decide that a hit interrupts — for the player and for
mobs alike. Nothing else may play the flinch or lock a character out.

| Poise state | What a landing hit does |
| --- | --- |
| holds | hyper-armour: VFX, camera impulse, `ProceduralHitManager` bone shove, `HitStop`, and `HitStun` (an attack-only lockout). The victim keeps acting |
| breaks | stagger: `HitReaction` crossfades the flinch, movement is zeroed, the current action dies, the pool refunds `BreakRefill`, `OnPoiseBroken` fires |
| already staggered | never re-breaks. Extends by `0.35 x StaggerDuration`, hard-capped at `MaxStaggerChain x StaggerDuration` from the original break |

`ArmourWhileAttacking` multiplies incoming poise damage while the victim is mid-swing
(`BaseCharacterController.IsAttacking`). That is what makes poise a decision rather than a stat:
at 0.5 a thrall's wind-up survives a poke and dies to a heavy or a full string. The `x0.50` in
the F2 readout is this multiplier, live.

The flinch clips run 1.4–2.0 s while a stagger lasts 0.55–0.7 s, so `HitReaction.RecoverState`
crossfades back to the locomotion hub the moment the stagger expires. Point it at **`Idle`**, not
at the movement blend tree — `Idle` is the state that owns the transitions into attacks, turns
and air, so recovering anywhere else silently strands the character.

Current numbers:

| | pool | regen | stagger | armour mid-swing |
| --- | --- | --- | --- | --- |
| Player | 45 | 25/s after 1.2 s | 0.55 s | 1.00 (off) |
| Plague Thrall | 22 | 18/s after 1.4 s | 0.70 s | 0.50 |

Light hits cost 10–18 poise, heavies 30–38, mob hits 16. Against an unarmoured thrall that is
**a break on the 3rd light or on any heavy**; catch it mid-wind-up and it takes 4 lights instead.
A break also pays `SimpleMobAI.StaggerRecoveryBeat` (0.25 s) and a full attack cooldown on top,
so breaking a mob is a real opening rather than a cosmetic wobble.

`HitStop` freezes only that character's animator, never global time — a global freeze would
stutter every other player in a PvPvE match and desync a server. A break adds
`HitReaction.BreakHitStop` on top of the attack's own, so a break reads differently from a hit
that was shrugged off.

## PvPvE

`Targetable.AreHostile(a, b)` is the only allegiance rule in the game, and lock-on, soft aim and
hitboxes all call it:

- different `TeamId` (Player vs Monster) → always hostile
- same team, different `SquadId` → hostile (rival parties)
- same team, same squad → allies, unless `Targetable.FriendlyFire` is on
- monsters ignore each other unless `Targetable.MonsterInfighting` is on

Spawn each party with its own `SquadId` and PvP works with no combat-code changes.

**Netcode readiness** (no transport is wired — there is no netcode package in the project):

- the action clock ticks in `FixedUpdate`, so client and server can agree on it
- input is a `CombatCommand` struct in a buffer, not a direct call — a network receiver can push
  into the same buffer
- one hit carries one `HitPayload` and produces one `HitReport`, so a server validates a hit in
  one place
- `CombatEvents` is one-way, so the simulation runs headless with presentation stripped out

## Calibrating hit windows

`HitWindowStart` is the landmark everything hangs off — the retime aligns it to the end of
`Startup`, and the hitbox opens there. Authored by eye it was wrong by up to **0.10 normalized**,
which meant the hitbox opened *after* the blade had passed its furthest point and stayed open
while it travelled back behind the character.

**Tools > Bravehood > Combat > Calibrate Hit Windows From Clips** measures it instead. It samples
each clip across its length on a temporary Player instance, tracks the forward reach of the weapon
volume's tip in character space, and takes the peak of that curve as the strike frame. The active
window is placed to lead the peak slightly (`LeadFraction`, 0.4) so it covers the sweep through
rather than just the apex, and `Startup`/`Recovery` are rewritten to keep playback at 1.00x.
**Report Hit Windows** runs the same measurement without writing.

Measured corrections on the Knight set:

| Attack | peak@ | was | now |
| --- | --- | --- | --- |
| Fast Cut | 0.254 | 0.280 | 0.223 |
| Left Cut | 0.358 | 0.300 | 0.335 |
| Low Cut | 0.321 | 0.300 | 0.298 |
| Thrust | 0.392 | 0.340 | 0.362 |
| Overhead | 0.396 | 0.380 | 0.369 |
| Whirl | 0.263 | 0.350 | 0.233 |

Re-run it whenever an attack's clip changes. It flags any attack whose wind-up ends up under
`MinReadableStartup` (0.22 s) — that means `ClipStart` is trimming too close to the strike and the
swing has no telegraph.

The reach figures it prints come from an edit-time pose sample, which includes root motion the game
discards (the motor owns the transform; the advance comes from `StepDistance`). Use them to compare
attacks against each other, not as absolute in-game reach — for that, read the live `reach=` field
in the frame trace.

## Traps worth knowing about

These were all live bugs; the notes are here so they don't come back.

**Never play a hit reaction with a trigger.** A Unity trigger stays set until some transition
consumes it, and the `AnyState -> GetHit` transitions in this project had `canTransitionToSelf`
off — so a hit landed *during* a flinch could not consume the trigger, it sat there armed, and
it fired the moment the flinch's exit transition handed back to locomotion. On screen: the mob
took a hit, finished the first flinch, paused, then flinched again with nothing hitting it.
`HitReaction` uses `CrossFadeInFixedTime` at offset 0 instead, which restarts the clip from any
state and queues nothing. The `GetHit` trigger and both `AnyState -> GetHit` transitions have
been deleted from `PlagueDoctor` and `Knight_Controller` so this cannot be rebuilt by accident.

**One stagger authority, not two.** `SimpleMobAI` used to stagger on any hit over 5 damage and
hand out "hyper armour" on every third hit via a counter that never decayed — running alongside
`Poise`, which the thrall also had. A light poke cancelled a wind-up, and the armour came and
went with no relation to how hard the mob was being hit. The AI now listens to
`Poise.OnPoiseBroken` and does nothing else with damage.

**`CrossFadeInFixedTime`'s offset is in *scaled* state seconds.** Its "seconds" are measured
against `clipLength / speed`, not `clipLength`. Passing the raw clip time started every attack
`(speed - 1)` deeper into its clip than authored — at the ~1.3x these attacks run at, that ate a
third of every wind-up. `PlayerCombat.TryStart` divides by the entry speed to compensate.

**Hit-stun must not refuse an attack.** Gating `TryStart` on `Health.IsHitStunned` meant a mob
landing a hit every ~0.2 s refused practically every chain, so the follow-up only fired once the
swing had fully ended — which read as *"the second attack starts from the end of the first"*.
Poise is the interrupt mechanism: hold it and you keep swinging, break it and you stagger.
`HitStunUntil` remains exposed as state for AI and netcode, but nothing gates the player on it.

**The chain window belongs in the recovery, not the active frames.** Opening it mid-active meant
a held button produced a swing every ~0.22 s — each attack was cut off a fifth of the way in and
the animation read as spam. See *The number that sets the cadence* above.

**A trimmed `ClipStart` eats the wind-up twice.** Light1 trimmed 0.101 s off the front of its clip
and then buried another 0.090 s under the crossfade, leaving 0.163 s of readable telegraph on a
0.253 s startup — the swing appeared to begin already mid-strike. Budget `startup - BlendIn` as
what the player actually sees, and keep the opener's trim at or near zero.

**`RotationOverride` cannot be used to aim the character from outside.** `PlayerCombat` rewrites it
every fixed tick (soft-aim during startup, zero otherwise). A test harness that sets it to face a
target is silently ignored and every swing "misses" for reasons that have nothing to do with combat.
Place the target along `Motor.CharacterForward` instead.

**Don't blend out to locomotion when a follow-up is buffered.** The recovery's exit-to-locomotion
blend plus the chain's own crossfade is two blends in a handful of frames, and the chained attack
visibly stutters on the way in. The exit blend now checks the buffer first.

## Debugging

### On-screen readout (F2)

`CombatDebugHUD` sits on the Player and draws a live panel in the bottom-left:

```
Fast Cut  (Light 1)
Startup   t=0.160s / 0.560s
[====startup====|=active=|========recovery========]   <- to scale, with chain tick + playhead
startup 0.170  active 0.090  recovery 0.300
chain@0.215  dodge@0.260  free@0.362  feint<0.111
anim  Light1  0.10n  x1.34
entry 0.100n   authored ClipStart 0.100          <- green when they agree, red when they don't
buffer Light   move x0.30   turn x0.55   dodge no
declined: -
hp 100  stamina 88/100  poise 45/45
L4 Thrust > L1 Fast Cut > L2 Left Cut > L3 Low Cut > L4 Thrust
F2 toggle
```

What each line is for:

- **name + slot** — which attack and which chain step is actually running
- **phase bar** — startup / active / recovery drawn to scale, with a tick at the chain window and
  a playhead. Watching the playhead pass the chain tick with nothing happening is the fastest way
  to spot a refused follow-up.
- **entry vs authored ClipStart** — where the clip really started, unwound from the action clock
  (exact, not sampled). Red means the crossfade offset is landing in the wrong place.
- **buffer / move / turn / dodge** — what input is queued and how much control you currently have
- **declined** — why the last chain was refused (stamina, no def, …), which is normally the answer
- **history** — the last few attacks, so you can see the chain actually cycling

### Frame trace

`PlayerCombat.LogFrames` fills `PlayerCombat.Trace`, a static 400-line ring buffer, with a
per-frame sample (`_t`, chain index, phase, animator normalized time / length / speed, transition
target) plus a `START` line per action and a `DECLINE` line whenever a chain is refused, with the
reason. Read it from a debugger or a tooling hook — it is in memory rather than the console
because a per-frame trace drowns the console and `Debug.Log` is not readable from every path.

Expected healthy trace: chains fire at each attack's `ChainFrom` (0.20–0.29 s into the previous
swing), and each attack's entry normalized time matches its authored `ClipStart` (0.10 / 0.15 /
0.16 / 0.14 for the light chain).

## Controls

Bound in `Assets/InputSystem_Actions.inputactions`, Player map.

| Action | Gamepad | Keyboard / mouse |
| --- | --- | --- |
| Light attack | **R1** (right shoulder) | Left mouse |
| Heavy attack | **R2** (right trigger) | Right mouse |
| Dodge / sprint | **Circle / B** — tap rolls, hold sprints | Left Shift |
| Jump | Cross / A | Space |
| Lock on | R3 (right stick press) | Middle mouse |
| Interact | Triangle / Y | E |
| Crouch | D-pad down | C |

Two things were wrong here and are worth remembering:

- **Light attack had no shoulder binding at all** — it was on `buttonWest` (Square/X), which is
  not where any action game puts it.
- **`Attack` and `Roll` were both bound to `buttonWest`**, so one button asked for a swing and a
  dodge at the same time. Roll moved to `buttonEast` and crouch moved off it to the d-pad.

`PlayerInputHandler` enables these actions directly and reads them per frame, so bindings are live
without a `PlayerInput` component being involved.

**Watch out for the stray `PlayerInput`** on `PlayKit/GameObject (1)` — a bare object carrying
nothing but that component. It auto-switches control schemes on the *shared* action asset, which
applies an asset-wide binding mask: while it sits on Gamepad, the keyboard/mouse bindings resolve
to nothing (Attack drops from 2 controls to 1), and vice versa. It has no receivers for its
`SendMessages` notifications, so it does nothing useful. Deleting it removes the masking.

## Known gaps

- **No block / parry / guard-break.** Needs clips that aren't in the set yet.
- **Mob attacks are still animator-driven** (`EnemyCombat` + `SimpleMobAI`). They deal *and* take
  poise through the same path the player does, and they stagger off the same `Poise` component,
  but they don't get frame data, cancels or combos — so their armour window is the whole attack
  state rather than an authored startup + active. Porting a mob onto `PlayerCombat`'s state
  machine is the natural next step.
- **One flinch clip, no direction.** Every stagger plays `Sword1h_Hit_Torso_Front` regardless of
  where the blow came from. `DamageInfo` already carries the hit point and normal, so picking
  between front/back/left/right flinches is a `HitReaction` change and nothing else.
- The attack clips carry a `SendEvent` animation event with no receiver — harmless console spam
  that predates this work.
