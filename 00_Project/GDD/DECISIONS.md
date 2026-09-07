# DECISIONS

Chronological record of important design/technical decisions. Newest at top.

## 2026-07-13 — W26–W32 dressed; Floor 1 fully dressed
Dressed the last 7 rooms from 7 unlabeled refs (2 batches: W26–W30 = imgs1–5, W31–W32 = imgs6–7), user away → auto-picked the recommended **by-size/grandeur** mapping: img5 Sacred Tree→W28 (hero Chapel), img1 Garden→W30, img2 Bell→W29, img3 Garden→W26, img4 Records→W27, img7 Crystal Fountain Hall→W31, img6 Crystal Shrine→W32. Heavy stand-in use: gardens have **no tree/hedge asset** → Bouquet flower clusters; sacred tree & shrine crystals → Azure_Crystal; bell → Boiler_01; gravestones → low platforms + flower bowls; statues → Knight_Armor. Avoided Column_01 (known-broken). **All W1–W32 rooms are now prop-dressed** — remaining work is an art pass (real trees/bell/water/statues), NavMesh, lighting, spawners.

## 2026-07-13 — W21–W25 dressed to concept art (mapped by size)
Dressed W21–W25 from 5 unlabeled refs whose themes (chapel, study, memorial, fountain hall, library) matched none of the utilitarian scene group names (pharmacy, lab, isolation, store, surgery). Confirmed with the user: map **by size/grandeur** — img1 Chapel→W25 (biggest SurgeryTheatre), img4 Fountain Hall→W21, img5 Library→W23, img3 Memorial→W22, img2 Study→W24 (smallest). Dressed themes diverge from frozen group names (only Props_Dressing changed). Angel/saint statues stood in with Knight_Armor; obelisk/fountain/altar stood in with crystals/platforms/ritual table. **Gotcha found: `Prefabs/Column_01` renders magenta (broken material) and auto-orient lays it horizontal** — swapped the W22 obelisk to an Azure_Crystal monolith; logged in ASSETS.md. Also confirmed OneWayGate gameplay markers are intentionally purple.

## 2026-07-12 — W16–W20 dressed to concept art (user away, auto-mapped)
Dressed W16–W20 from 5 refs while the user was away, instructed to auto-pick the recommended mapping. Two images carried explicit labels — img1 "W19 Examination Hall", img3 "W16 Pharmacy" — so those were honored directly; the other three assigned by theme/size: img5 grand rotunda→W20 Reliquary (largest remaining), img2 ward→W17, img4 crystal ward→W18. Consequence: the **dressed themes now diverge from the frozen scene group names** (e.g. the 28×28 hub `W16_GrandInfirmary_Hub` is dressed as a grand pharmacy; `W19_Records` as an exam hall). Only `Props_Dressing` content changed; geometry/names untouched. Flagged W16↔W20 as swappable if the user prefers the rotunda in the hub. Verified the hub interior has no internal pillar grid (only perimeter walls/pillars), so it dressed freely.

## 2026-07-12 — W11–W15 dressed to concept art
Re-dressed W11–W15 from 5 reference images (again theme-ordered, not W-number). Confirmed mapping with the user: img5→W11 Laundry, img4→W12 Kitchen, img3→W13 Pantry, img2→**W14 Convalescent dressed as a dining/mess hall**, img1→**W15 Triage Hall dressed as an apothecary/healing hall**. Same auto-orient pass. Decided to use **volumetric props only** (no curtains/flags/tapestries) because those are authored flat and auto-orient lays them down — so drying lines (W11) and banners (W14) are represented by folded-fabric / omitted, logged as missing assets. Placeholders: wash basins (W11), pantry fountain (W13), rune-ring/healing crystal (W15).

## 2026-07-12 — W6–W10 dressed to concept art
Re-dressed W6–W10 from 5 reference images. The images were ordered by theme, not W-number; confirmed the mapping with the user (**match by purpose**): img1→W10 Baths, img2→W8 Armory, img3→W9 Observation Gallery, img4→W7 Guard Barracks, img5→W6 Waste Disposal. Same auto-orient + character-scale + grounding pass as W1–W5. Placeholders where no real asset exists (water/basin/pool, telescope→globe, wall crystal→azure). Gameplay markers (extraction/objective/ladder/balcony) left in place. Learned: **Crystal_02 scales to a blob** — use Azure_Crystal_01 for hero crystals.

## 2026-07-12 — Adopt AI/ documentation workflow
The `AI/` folder is the project's permanent memory. Read it first every session; documentation wins over chat history. Seeded from existing memory files + the blockout plan doc.

## 2026-07-11 — Prop auto-orient standard
Source models have **inconsistent native up-axes** (no single rotation fixes all — e.g. Bed_01 stood on end). Standard: detect the flattest mesh face, rest it on the floor, then apply only a Y-yaw. Guarantees level tops / flat bases. All room dressing must use this.

## 2026-07-11 — W1–W5 dressed to concept art
Rooms W1–W5 re-dressed from 5 reference concept images, auto-oriented and character-scaled. Bell/fountain/basin left as clearly-marked placeholders (no real asset yet).

## 2026-07-11 — Stairs at every elevation transition
`Stone_Stairs_01` (5-step) on short runs, `Stone_Stairs_02` (13-step) on long corridors. Run = gap + ~2 m (≈1 m overlap each end) so stairs always reach both floors — an earlier run-cap left visible voids and was removed.

## 2026-07-11 — Scene scaled ×0.45454 to the character
Whole scene uniformly scaled about origin to match PlayKit character (0.45454 → ~0.91 m), using `NewRoom_01 (7).prefab` as the reference ratio. Walls end up ~5.28 m / ~5.8× character height. Floor tiling auto-adjusted to keep ~2.885 m stone tiles.

## 2026-07-10 — Walls & floors FROZEN
After hand-tuning wall segmentation, height, the floor swap, and floor material/tiling, the geometry + shading is considered final. Do not move/resize/re-material walls or floors, or regenerate them from the room table, unless explicitly asked. (See memory `walls-floors-frozen`.)

## 2026-07-10 — Wall face orientation
Player-facing wall side had a defective material. All ~533 walls (and corner pillars) rotated 180° about Y so the clean face points at the player.

## 2026-07-10 — Uniform wall size per room
Within a single room all wall segments use one size (rooms may differ from each other), matching the owner's `W15_TriageHall` example. Leftover corner gaps closed with corner pillars.

## 2026-07-11 — Floor material = Stone_01 @ ~2.885 m tiling
Floors use per-size material instances derived from `Stone_01`, tiled to ~2.885 m stones (chosen over a flat 3×3). Kept as-is.

## 2026-07-06 — DungeonA is independent of Black Veil
`DungeonA_Floor01_Blockout` is a new blockout built from a top-down reference map, separate from the existing Black Veil sanatorium blockout (which was left untouched). Note: the plan doc header still carries the "Black Veil Sanatorium" title and room names — treat that as the room-naming source, not as a merge of the two blockouts.

## Environment / tooling
- UnityMCP `execute_code` is **C# 6 only** (CodeDom): no `using`, no local functions, fully-qualify `UnityEngine.*`.
- The Claude↔Unity MCP link can go stale mid-session; the only reliable fix is restarting the Claude Code session (restarting Unity alone does not help).
