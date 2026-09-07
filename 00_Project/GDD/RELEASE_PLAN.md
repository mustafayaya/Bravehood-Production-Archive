# Bravehood — Release Development Plan

**Date:** 2026-08-03 · **Branch snapshot:** `leveldesign/dungeonA`
**Game:** PvPvE extraction dungeon crawler — enter dungeon, fight AI + rival players, collect loot, extract; death loses the run.

---

## 1. Where we are today

**Strong / done:**
- DungeonA Floor 01 ("Black Veil Sanatorium") — 36 rooms blocked out and largely dressed, 4 spawns / 5 extraction markers, room-culling system baked in.
- Souls-like combat core: KCC movement (sprint/jump/crouch), roll with i-frames + stamina gating, data-driven light/heavy combo chains (WeaponMoveSet SOs), hitbox/hurtbox with per-limb multipliers, hit reactions, camera feel.
- One working enemy (PlagueThrall: NavMesh chase/attack/wander, stagger, hyper-armor).
- Audio foundation (AudioManager, footsteps per surface, character voices, real WAVs).
- HUD prefab (health/stamina/boss bars, minimap + compass, quick slots).

**Missing entirely:**
- Networking (the PvPvE pillar) — no netcode package installed.
- The extraction loop itself: no loot items, inventory, pickups, spawners, extraction logic, or run/game-state manager. These exist only as scene markers.
- All menus (main, pause, settings, death/extraction screens), save/load, progression/economy.
- Build config (scene list empty, product name still "My project", platform TBD).
- NavMesh bake for Floor 1, enemy roster beyond one mob, boss encounter (Ward Matron is a marker only).

**Read:** content is ahead of systems. The release risk is not the dungeon — it's that the core game loop has never been playable end-to-end.

---

## 2. Guiding priority

> **Get one full run playable offline first** (spawn → fight → loot → extract → results screen), **then** add multiplayer, **then** meta-progression. Every phase ends with something you can playtest.

A major early decision gates everything: **pick the netcode stack** (Netcode for GameObjects vs Photon Fusion vs Fish-Net) and **target platform** (PC/Steam assumed below). Character controller and combat should be written server-authoritative-friendly from Phase 2 onward, or the netcode phase becomes a rewrite.

---

## 3. Phases

### Phase 1 — Playable core loop, offline (≈ 4–6 weeks)
The single most important phase. Everything here is single-player against AI.

1. **NavMesh bake for Floor 1** — blocks all AI; do first. *(already in TASKS.md)*
2. **Run/Game-state manager** — states: Spawn → InRun → Dead / Extracted → Results. Owns timer, win/lose, scene reload.
3. **Loot system** — Item ScriptableObject (id, icon, rarity, value, stack), world pickup prefab (interact prompt), loot container prefab for the placed loot markers.
4. **Inventory** — lightweight run-inventory data model + panel UI (the HUD quick slots already exist; wire them). On death: inventory drops; on extraction: inventory banks.
5. **Spawners** — enemy spawner + loot spawner components driven by the existing scene markers; simple spawn tables.
6. **Extraction points** — replace marker spheres with an interactable extraction volume (hold-to-extract with channel bar, cancel on damage).
7. **Death & extraction result screens** — minimal: what you lost / what you banked, retry button.

**Milestone M1: one complete run is playable start-to-finish in the editor.**

### Phase 2 — Combat depth & enemy roster (≈ 3–5 weeks)
1. **2–3 more enemy types** built on SimpleMobAI variants (e.g. ranged caster, fast lunger, armored heavy) — enough variety for 36 rooms.
2. **Ward Matron miniboss** — real encounter: boss health bar (BossBarUI exists), 2-phase moveset, arena gating in room W30.
3. **Second weapon moveset** — proves the WeaponMoveSet pipeline generalizes (e.g. greataxe or spear alongside the Knight sword).
4. **Combat polish** — lock-on targeting, block/parry decision (in or cut — decide now), poise tuning, healing consumable usable from quick slots.
5. **Enemy/room difficulty pass** — spawn tables tuned per room tier; optional containment room as a high-risk/high-loot area.

**Milestone M2: a full run is *fun* solo — variety, a boss, a loot-risk decision.**

### Phase 3 — Multiplayer (PvPvE) (≈ 6–10 weeks, highest risk)
1. **Choose and integrate netcode stack**; Unity Authentication is already installed — pair with Lobby/Relay or the chosen stack's matchmaking.
2. **Networked character** — movement + roll replication (KCC needs care here), server-authoritative combat hits, health/stamina sync.
3. **Networked run loop** — shared match state, per-player inventory/extraction, loot sync, enemy AI authority on host/server.
4. **Session flow** — lobby scene, matchmaking/join, 4–8 player sessions (match spawn count to the 4 spawn rooms).
5. **NetworkAudioComponent** — finish the existing placeholder.
6. **PvP tuning pass** — TTK vs players, third-party risk at extractions, spawn protection.

**Milestone M3: two+ real players complete a contested run over the network.**
*De-risk early: stand up a walking-skeleton network test (two capsules syncing) in week 1 of this phase — or even during Phase 2 — before betting the schedule on it.*

### Phase 4 — Meta layer & front end (≈ 3–4 weeks)
1. **Persistence/save system** — banked loot stash, settings, profile (cloud save if platform supports).
2. **Progression & economy v1** — keep minimal for release: stash value, a vendor or simple unlocks; per docs these are TBD, so scope-check hard here.
3. **Main menu + boot scene** — play, stash, settings, quit; loading screen.
4. **Pause & settings menus** — graphics quality, resolution, audio sliders (bus mixer), keybind rebinding, sensitivity.
5. **Onboarding** — first-run tips or a 5-minute solo tutorial corridor; extraction games are opaque to new players.

**Milestone M4: game is navigable like a product, not an editor project.**

### Phase 5 — Content & art completeness (parallel with 3–4)
1. Replace the ~15 placeholder props on the TASKS.md list (bell, fountains, statues, telescope, water).
2. Lighting/post final pass per room; performance pass with the culling system (frame-time budget on min-spec).
3. Music: ambient dungeon layer, combat stinger, boss track, menu theme.
4. VFX pass: extraction beam, loot rarity glints, boss telegraphs.
5. **Decision point:** Floor 2 — recommend **cutting from 1.0** and shipping Floor 1 deep rather than two floors thin; keep the descent marker as a tease.

### Phase 6 — Release hardening (≈ 3–4 weeks)
1. **Build pipeline** — fix PlayerSettings (product/company name, version), populate build scene list, CI build if possible.
2. **Platform/store setup** — Steam page, builds, achievements optional; playtest via Steam Playtest branch.
3. **Stability** — crash/error reporting (e.g. Unity Cloud Diagnostics), soak tests, memory/leak pass, reconnect handling for netplay.
4. **Closed playtest → open beta** — at least two external playtest rounds with a feedback form; tune economy/TTK from data.
5. **Analytics** (runs started/completed, death causes, extraction rates) — you can't balance an extraction game blind.
6. Legal/ops: EULA/privacy (online game), age ratings, server cost plan if dedicated servers.

**Milestone M5 / Release Candidate: external players complete networked runs on store-distributed builds with no blockers.**

---

## 4. Cut-line (if the schedule slips)

Ship without, in this order: Floor 2 → progression/economy beyond a stash → parry system → extra weapon movesets → localization. **Never cut:** the offline-complete loop, settings menu, save of banked loot, and network stability.

## 5. Immediate next actions (this week)

1. Bake NavMesh for Floor 1.
2. Decide netcode stack + target platform (writes constraints into everything after).
3. Build the RunManager + a first lootable item + one working extraction point — even ugly, get M1 moving.
4. Fix PlayerSettings/build scene list (10 minutes, removes a whole class of "works in editor only" surprises).
