# ASSETS

Location: `Assets/Levels/Dungeon1/NewAssets/Models/` (models) and `.../Prefabs/` (kit prefabs).

## Produced Assets
### Kit / structural
- Wall_01, Wall_02, Wall_03
- Floor_01 · Ceiling_01 · Ceiling_02
- Column_01 · Column_02 · Corner_Pillar_01
- Stone_Archway_01
- Stone_Stairs_01 (5-step) · Stone_Stairs_02 (13-step)

### Props
- Armory_Rack_01 · Sword_Rack_01 · Sword_01
- Barrels_01 · Pottery_Trio_01
- Candlelight_01
- Toolbench_01 · Toolbench_02 · Toolbench_03
- Knight_Armor_01 · Knight_Helmet_01/02 · Knight_Gloves_01/02 · Knight_Greaves_01 · Knight_Vambrace_01
- Steel_Sheild_01 · Wooden_Sheild_01
- Crown_Tower_01
- Skull_01 · Fallen_Warrior_01 · Wrapped_Body_01 · Blood-stained shroud_01

### Room prefabs
Room1–Room17, BaseRoom, NewRoom_01, NewRoom_02, Plane (floor). Reference: `NewRoom_01 (7).prefab`.
- **`Floor_W28_Chapel.prefab`** (`NewAssets/`, 2026-07-14) — **room-template prefab, THE kit reference for wall/floor/roof/column updates** (see LEARNED R026): plane floor + 9 tile-wall panels + ArcCeiling vault + flat tile ceilings + gable end caps + border/bordersroof + 4 Column_01 + DoorStructure arches + candles/probe + treasure-chapel dressing. Names normalized to convention.

## Placeholder Assets (⚠ stand-ins in scene — need real models)
- **Bell** (W2) — currently a bronze-vessel stand-in.
- **Fountain** (W4) — Treasury_Platform + crystal stand-in.
- **Basin / water** (W5) — platform + azure crystal stand-in.
- **Drainage basin + water** (W6) — Treasury_Platform_02 + Azure_Crystal stand-in (no water mesh).
- **Wall hero crystal** (W8) — Azure_Crystal used in place of the big floating wall crystal.
- **Telescope** (W9) — Topglobe_01 used as stand-in.
- **Pool + water + flame spouts** (W10) — Treasury_Platform_01 + Azure/Crystal stand-in (no water mesh).
- **Wash basins + drying lines** (W11) — Toolbench+Bowls / folded-fabric stand-ins (no basin or clothes-line asset).
- **Pantry crystal fountain** (W13) — Treasury_Platform_02 + Azure_Crystal stand-in.
- **Rune-ring / healing crystal** (W15) — Treasury_Platform_01 + Azure_Crystal + Ritual_Altar stand-in (no big vertical rune-disc asset).
- **Apothecary scales** (W16) — Alchemy_Set/Mortar stand in (no balance-scale asset).
- **Ward crystal fountains** (W17, W18) — Treasury_Platform_02 + Azure_Crystal stand-in.
- **Medical rune circle** (W19) — Blood_Symbol_01 (reddish ritual rune) stands in for the blue medical/caduceus rune.
- **Tiered dais + hanging chained crystal** (W20) — Treasury_Platform_01 + floating Azure_Crystal stand-in (no stepped dais or chain-hung crystal asset).
- **Statues** (W21/W22/W23/W25) — Knight_Armor_01 used as an upright statue stand-in (no angel/saint statue asset).
- **Stone obelisk / monolith** (W22) — Azure_Crystal on a platform stands in (no obelisk asset; Column_01 unusable — see gotcha).
- **Fountain water** (W21) — Treasury_Platform_01 + Azure_Crystal stand-in.
- **Altar** (W25) — Ritual_Table_01 stands in for the chapel altar.
- **Trees / hedges / garden planters** (W26/W28/W30) — Bouquet_01 flower clusters stand in (no tree or hedge asset).
- **Sacred tree of life** (W28) — big Azure_Crystal stands in.
- **Bell** (W29) — Boiler_01 stands in (still no dedicated bell; same gap as W2).
- **Gravestones / obelisk grave markers** (W32) — low Treasury_Platform_02 + flower bowls stand in.
- **Orbiting rings around crystals** (W28/W31/W32) — omitted (no ring asset).

## Missing Assets (needed, not yet produced)
- Dedicated bell model.
- Fountain / pool + running-water material or mesh (W4, W6, W10).
- Decontamination / drainage basin (W5, W6).
- Telescope (W9).
- Balustrade / stone railing for the pool rooms (W6, W10) — none exists; omitted for now.
- Sink / wash basin (W8, W11).
- Laundry clothes-line / drying rack with hanging cloth (W11).
- Oven / stone hearth-oven (W12) — approximated with counters.
- Wall banner / tapestry that hangs correctly (flat authored prefabs lie flat under auto-orient) (W14).
- Large vertical rune-ring / portal disc (W15).
- Apothecary balance-scales (W16).
- Blue medical / caduceus floor-rune circle (W19).
- Tiered/stepped stone dais + chain-hung floating crystal (W20).
- Caduceus / medical wall banners (W18) — flat prefabs lie flat under auto-orient.
- Angel / saint / figure statues (W21/W22/W23/W25) — Knight_Armor stands in.
- Stone obelisk / monolith (W22).
- Chapel altar + chandeliers + grand staircase (W25, W21) — omitted/stood-in.
- **Trees, hedges, garden planters** (W26/W28/W30) — biggest gap; gardens are currently just flower clusters + fountains.
- **Bell** (W2, W29).
- **Gravestones / obelisks** (W22/W32).
- **Orbiting-ring / halo VFX** for hero crystals (W28/W31/W32 etc.).
- (Floor-1 art is dressed with stand-ins; this list is the art-pass shopping list for W1–W32.)

## Asset Notes / Gotchas
- **Crystal_02** has near-cubic mesh bounds (~2 cm all axes); uniform height-scaling turns it into a fat glowing blob. Use **Azure_Crystal_01** for slender hero crystals instead.
- **Toolbench_03** — the folder exists under `Models/` but has **no `.prefab`** (model file only). Use Toolbench_01/02.
- **Flat-authored props** (Curtain_01/02, Flag_01/02, Tapestry_01, and even Book_Shelves/Alchemist_Shelf which are authored lying down) have their smallest extent on Y. The auto-orient placer stands the shelves fine, but genuinely flat sheets (curtains/flags/tapestries) get laid flat — avoid them or place with a fixed upright rotation.
- **Column_01 (`Prefabs/Column_01`)** renders **magenta** (broken/missing material) in the blockout scene AND auto-orient lays it horizontal (cylinder → densest face is the side). Do NOT use it for obelisks/pillars; use Azure_Crystal or another asset. (Corner_Pillar_01 renders fine.)
- **Gameplay OneWayGate markers** render as **purple/magenta** cylinders/arrows at room edges — that's intentional gameplay coloring, not a broken material. Leave them.
- **Crystal_03 — RESOLVED 2026-07-15**: user imported `Models/Crystal_03/` (Meshy azure spire + full texture set incl. emission) and placed it s=250 at W28's center; the Floor_W28_Chapel template instance now sits at the W28 socket. ⚠ Two leftovers: `Crystal_03.mat` has **_EMISSION OFF** (emission texture exists — same gotcha as Azure_Crystal_01), and the template **prefab** still contains the dead "Crystal_03 (Missing Prefab)" node — clean/replace on approval.
- **MeshCollider standard (2026-07-15)**: Table_01, Chair_01, Stool_01, Chest_01, Barrels_01, Pottery_Trio_01 prefabs now carry non-convex MeshColliders (prefab-level physics for cover/bump props). New prop prefabs should follow suit.
- **Corner_Pillar_01.mat** darkened by user (_BaseColor 0.94 → 0.48) to match the tile-wall value range.

## Deprecated Assets
- None recorded.

> Before generating any new asset, check this file first. Never create a duplicate of something already listed under Produced.
