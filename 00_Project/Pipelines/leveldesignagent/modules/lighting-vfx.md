# Lighting & Fire VFX — the room lighting system (sessions 30–32, 2026-07-31 → 2026-08-06)

Everything needed to light a Bravehood room to the concept-art standard, maintain the existing
fixtures, and audit the user's hand edits. Self-contained: a fresh session on any machine can
execute this without prior chat context. Binding rules live in [../LEARNED.md](../LEARNED.md)
(R048, R063–R069); this module is the operational recipe book built on them.

## 1. The palette (R066 — applies to EVERY room, user-confirmed "from now on")

| Fixture | Role | Light color | Flame/ember/glow/magic coloring |
|---|---|---|---|
| `Torch_01_Lit` (wall/pillar torches) | GREEN lanterns | (0.35, 1, 0.45) | **material kit** `*_Green` (see below), FireColor/GlowTint = WHITE |
| `Floor_Lamp_01_Lit` (floor lamps) | PURPLE crystal accents | (0.65, 0.35, 1) | **material kit** `*_Purple`, FireColor/GlowTint = WHITE |
| Candles, candelabras, chandeliers | WARM candlelight | (1, 0.55, 0.12) | warm base materials + FlameTint (1, 0.55, 0.12) |

**2026-08-06 R070 — colors live in MATERIAL VARIANTS, not tints.** The user rejected tinted-warm
flames ("if it's green, make it completely green"): tinting the warm gradient reads muddy. Each
color has a full kit in `Torch_VFX/Materials/`: `M_FlameOuter_/M_FlameCore_` (full 3-stop
gradient in the color), `M_Ember_` (T_SoftRadial texture — the orange T_Ember texture muddies
tints), `M_Glow_`, `M_Magic_` × `_Green`/`_Purple`. Fixture instances get the kit swapped onto
Flame_Outer/Flame_Core/Glow renderers + Ember/Magic particle materials & startColor, with
controller FireColor and flicker GlowTint set WHITE (they'd double-tint otherwise). ⚠ 134 of
the user's copied torches have NO controller/flicker on the root (stripped in copying — lights
still work, no flicker animation); classify those by their Light color and swap materials
directly.

Reference: the user's concept art (green wall lanterns + purple crystal pedestals + warm candle
chandelier over pews). W1 IntakeCathedral was the first room lit to it and is the color donor.
**R048 is HARD: every point light shadowless (`shadows = None`).** The user occasionally
re-enables Soft shadows in editor passes — audit and flip back (2 found on the W16 chandeliers).

## 2. Asset inventory — `Assets/Levels/Dungeon1/NewAssets/Torch_VFX/`

- **`Torch_01_Lit.prefab`** — full torch rig: `Torch_Model` (rX270 s150) + `Fire_VFX`
  (Flame_Outer/Flame_Core low-poly mesh flames) + `Ember_VFX` + `Smoke_VFX` + `Magic_Accent`
  (teal ~8%) + `Heat_Distortion` + `Glow` billboard + shadowless `Point_Light` + `Audio_Source`
  (empty) + `TorchFlicker` + `TorchVFXController` on the root. ⚠ The prefab ASSET ships warm —
  green is a per-instance override; when rolling out torches, CLONE an existing green instance.
- **`Floor_Lamp_01_Lit.prefab`** — same rig on the Floor_Lamp_01 FBX (model s100, material
  `Floor_Lamp_01.mat` bound manually — the raw FBX has none).
- **`FlameOuter_LowPoly.asset` / `FlameCore_LowPoly.asset`** — procedural faceted teardrop
  meshes (7-sided 210 verts / 6-sided 180 verts), used standalone for candle/chandelier wicks.
- **Materials/** `M_LowPolyFlameOuter`, `M_LowPolyFlameCore` (shader `Bravehood/VFX/LowPolyFlame`
  — opaque unlit 3-stop HDR gradient, height²-weighted vertex wobble, `_Tint` MPB), ember/glow/
  smoke/magic/heat materials, and the crystal set `M_CrystalGlow_C01/C02/C03/_Azure` +
  `M_CrystalSparkle` (see Crystal VFX below).
- **Scripts/**
  - `TorchVFXController.cs` — all artist parameters; **`Apply()` runs in `Awake` and
    `OnValidate`**, so `FireColor`, light settings etc. are durable (serialized fields drive
    MPBs on load).
  - `TorchFlicker.cs` — Perlin 2-octave light flicker (never off), glow pulse, per-instance
    `GlowTint`.
  - `FlameTint.cs` — `ExecuteAlways` serialized `Tint` re-applied via MPB in
    `OnEnable`/`OnValidate`. **Required on every standalone flame mesh** (no controller):
    naked MaterialPropertyBlocks are NOT saved with the scene and silently revert on editor
    restart (R065 — this bit us once).
  - `BillboardFace.cs` — `ExecuteAlways` camera-facing quad (glow halos). In headless POV
    renders quads may read edge-on (they orient per editor tick) — still-render artifact only.

## 3. Fixture recipes

### Torches (R063 geometry + green; R068 hallways; R069 stairs)
All mesh `Torch_01` were swapped to `Torch_01_Lit` at identical transforms
(`root.rotation = meshRotation × Inverse(Euler(270,0,0))`, root scale 1, bounds-compare each
spawn vs its source before deleting the original). Rooms: door-flank pillar stacks (R063).
Hallways (R068, user exemplar Floor_Corr (4)): facing pairs on both flank walls, 0.23 m off
the wall face, root Y = floorTop + 3.74, yaw into the corridor, ~8.8 m pitch, ~2.5 m end
margins. Stairs (R069, user exemplar W21↔W22): pair mid-run on both flank walls, root Y =
local stair surface + 3.834. ~298 torches scene-wide, all green.

### Floor lamps (R067)
- Canon transform: **localScale (1.5, 2.0, 1.5)** (user-set), yaw 0, scene root,
  **rootY = roomFloorTopY + 0.98** — base then seats exactly (verified empirically via
  renderer-bounds min vs floor top; don't re-derive from pivot math).
- **2 per room** standard; **W16 hub gets ≥4** (user instruction); W29 has the user's 3.
  Rooms only — `Floor_W21_Pharmacy (1)` is a CORRIDOR piece (R051), never gets lamps.
- Purple recipe always (W29's warm holdouts were conformed on request 2026-08-05).
- Boss room W30: edges only (R059 — wall-adjacent placement satisfies it).

### Candelabra wick flames (R064)
Every upright `Candlelight_01`-mesh cluster (mesh name `Mesh_0`, 143 scene instances incl.
inactive staging) carries the user's 7-flame arrangement — 2 full Outer+Core pairs on the tall
candles, single flames on the 3 lower candles. **Never re-derive: clone the donor
`Props_Dressing/W1_IntakeCathedral/Candlelight_Lit_1` children in LOCAL space** (survives yaw
and both scale variants s275/s160). Tipped/lying instances (2 exist) stay unlit. All flames
carry `FlameTint (1, 0.55, 0.12)`. The user later added their own amber Point Lights
(1, 0.665, 0, i3, r12) to ~29 plain clusters — leave them.

### Chandeliers
`Chandelier_01` (s250, rX270) — 12 ring candles at local radius ≈2.15, candle tops at
pivot-rel y band −0.99..−0.50 ·(s/250). **Donor-clone from `W1/Chandelier_01 (1)`**: 24 flame
children (12 Outer+Core pairs, ×2.6 scale, FlameTint warm) copied in local space. Local
coords are model-space, so the clone transfers across ANY uniform scale (verified on W2's
s200 vs donor s250 — flames land on-ring, proportionally sized); guard only against
rotation mismatch (must be rX270 yaw-only). Otherwise re-detect via the radial band (the 4
clusters at r≈1.56 are chain brackets — exclude; the top-vertex cut finds the CEILING MOUNT,
not candles — never use it). Set the fixture light to warm (1, 0.55, 0.12) + shadows None.
Lit so far: W1 ×2, W13 ×1, W16 ×2.

### Crystal VFX (2026-08-06)
Every crystal-mesh object (`Crystal_01/02/03`, `Azure_Crystal_01`/`Hero_AzureCrystal` — 46 in
scene incl. inactive staging) carries its own `Crystal_VFX` child: a `BillboardFace` glow quad
(bounds-sized ×1.15 then ×0.7 after the fog fix — **halo smaller than the crystal, alpha
0.32**) + a local-space sparkle-mote particle system (rate 3–8/s by size, prewarm, lifetime
1.8–3 s, slight upward drift, fade in/out, `M_CrystalSparkle`, startColor per type). Colors
are per-TYPE material assets (durable, no MPBs): `M_CrystalGlow_C01` green-teal
(0.33, 1.3, 1.06), `_C02` blue (0.26, 0.6, 1.3), `_C03` cyan-blue (0.26, 0.78, 1.3), `_Azure`
(0.3, 0.6, 1.5, a 0.5) — brighter on purpose, the hero focal. Colors were MEASURED from each
material's texture (GPU blit 32×32 → average of saturated bright pixels), not guessed. No
point lights on crystals (budget). Idempotent: skip crystals already owning a `Crystal_VFX`
child.

## 4. Placement pipeline (lamps or any floor fixture)

1. Room floor bounds from `_Blockout/Floors/Floor_W#_*` renderer-bounds union (active only).
2. Candidates: perimeter ring (corners + wall mids, inset ~1.7) or 0.5 m grid for dense rooms.
3. Filters, in order:
   - **Corridor-mouth clearance**: reject candidates inside any corridor floor bounds expanded
     +2.5..3 m (catches doorways); shift or re-pick.
   - **Blocker occupancy**: all renderer AABBs 0.4 m < height, footprint < 30 m — INCLUDING
     `_Blockout` walls (diagonal/rotated walls have fat AABBs and DO clip lamps; prop-only
     clearance missed one).
   - **Openness**: candidate + 8 surrounding samples at 1.8 m (4-dir at 1.1 m is fooled by
     shelf-aisle canyons — W16 buried a lamp twice).
   - **Spacing**: ≥4–5 m from other lamps (relax to 2.5 in small rooms).
4. Spawn via `PrefabUtility.InstantiatePrefab` (or clone a green instance for torches), set
   canon transform, paint the recipe (serialized fields + editor-preview MPB),
   **bounds-verify vs a healthy reference (±30%)**.
5. **POV screenshots are mandatory** — the data checks false-positive on sprawling rotated
   AABBs (rubble, sand) and false-negative on visual issues (door-axis blocking, cramped
   nooks). Frame from the room-center direction at the fixture's own y (never guess camera
   heights — guessed heights produced black/underground shots twice).
6. Save, console check, `AI/TASKS.md` entry.

## 5. Audit checklist (run after every user "I moved/copied things" pass)

Per lamp/fixture: room floor under it (else corridor = error); base seat |renderer.bounds.min.y
− floorTop| ≤ 0.12; scale = (1.5, 2, 1.5); no pair < 0.6 m (duplicate); recipe colors intact.
Scene-wide: 0 shadowed point lights (R048); envelope sweep (|bounds| > 45 or out of x −135..105
/ z −95..95 / y −30..30 = corruption, R049/R054); flame-tint components present on standalone
flames. Copy-paste mistakes to expect: source-room Y kept after cross-room paste, missed scale
axis, lamps dropped on corridor floors. The user's edits are otherwise canon — verify, fix
objective errors only, never "improve" their arrangement (R062).

## 6. Scene census (as of 2026-08-06)

~70 Floor_Lamp_01_Lit (all purple) · ~298 Torch_01_Lit (all green; rooms + hallways + stairs)
· 140 flame-dressed candelabras + donor (~29 with user amber lights) · 5 lit chandeliers ·
46 crystals with Crystal_VFX · ~502 point lights, ALL shadowless · envelope clean · lightmap
bake predates all of this (re-bake pending, user's call) · DungeonCulling is the light-budget
safety valve — if play-mode GPU time chugs, wire fixture lights into it.

## 7. Editor connection (both computers)

Native MCP tools first. After an editor restart they drop — check `Get-Process Unity` + port
8080, then use the HTTP hub fallback (`http://127.0.0.1:8080/mcp`, JSON-RPC: initialize →
capture `mcp-session-id` header → notifications/initialized → tools/call, SSE `data:` lines;
generic runner pattern: a `run_cs.py <file.cs>` that re-handshakes on 404/no_unity_session).
After editor launch, wait for imports — execute_code returns success:false with null message
until ready (~1–2 min; poll with a ping script).
⚠ The HTTP hub's codedom compile may NOT reference the project assembly — `Bravehood.VFX.*`
types fail to resolve there; use name-based lookups or resolve types via
`System.AppDomain.CurrentDomain.GetAssemblies()` scan (works for `AddComponent`).
C#6 codedom rules apply on both paths (no `using`, fully-qualify, MarkSceneDirty + SaveOpenScenes).

## ⚠ Keep this file committed

This module and the SKILL.md routing row were LOST ONCE to a working-tree discard
(2026-08-06, same failure mode as the old AI/TASKS.md rollback). After any edit here:
commit promptly. If the routing row for lighting-vfx is missing from SKILL.md's module
table, re-add it.
