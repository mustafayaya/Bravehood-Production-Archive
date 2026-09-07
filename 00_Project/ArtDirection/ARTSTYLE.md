# ARTSTYLE

> Most numeric targets below are **not yet formally documented**. What is recorded here are the rules the current dungeon build actually follows. Fill in TBDs when the owner specifies them.

## Character Proportions (the measuring stick)
- **Current (2026-07-14):** judge sizes against the live `PlayKit/Player` — **~2.0 m tall at scale 1.0**. W16-final proportions: tile-wall panels 11.9 m ≈ 6× player, shelf tiers 3.63 m ≈ 1.8×, border cornice at 8–9 m, gate column tiers 4.15 m.
- Historical: `NewRoom_01 (7).prefab` at scale 0.45454 → ~0.91 m was the original reference (pre-teaching rooms were scaled to it).
- **Rule:** all environment and props are sized relative to the player character.

## Materials
- **NEW STANDARD (2026-07-14, W16):** big surfaces use the world-space tiling shader `MotoX/Environment/WorldSpaceTiling` (`Assets/Shaders/WorldSpaceEnvironment.shader`) — tiles in world space, one material fits any mesh size, **GPU instancing enabled**. Trio in `NewAssets/Materials/Stone_01/`: walls `Stone_01TileTest 1` (_TilesPerMeter 0.11, _BumpScale 1.57), floors `Stone_01TileTest` (0.40 → 2.5 m stones), ceilings `Ceiling` (0.21). Shared accents _SandColor (0.72,0.64,0.49), _PuddleColor (0.76,0.81,0.84).
- Legacy (pre-W16 rooms): floor instances derived from `Stone_01`, size-based tiling ~2.885 m stones (57 cached instances); wall material `Wall_01`, walls rotated 180° so the clean face points at the player. Migrate to the world-space materials as rooms get their art pass.

## Placement Rules (props)
- **Auto-orient:** every prop's flattest mesh face is detected and rested on the floor, then only a Y-yaw is applied. Guarantees a level top and flat base regardless of the model's inconsistent native up-axis.
- Props are grounded (base sits on floor Y) and scaled character-relative.

## Color Palette
TBD.

## Lighting
TBD.

## Polycount Targets
TBD.

## Texture Resolutions
TBD.

## Hero Asset Quality
TBD.

## UI Style
TBD.
