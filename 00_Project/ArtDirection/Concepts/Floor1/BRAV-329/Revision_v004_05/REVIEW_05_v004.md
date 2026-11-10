# BRAV-329 part 05 — coffered dome multi-view references v004

2026-10-02. Pending Mustafa approval. These are 2D Tripo references, not a model delivery.

## Geometry held across all four views

- Dome diameter 27.6 m; rise 6.9 m; shell 1.0 m; open oculus diameter 4.6 m.
- Neutral grey faceted stone, continuous springing ring, no brass or coloured trim.
- The oculus is a real opening: white background is visible through it; no lid, boss, glass or crossing ribs.
- Regular layout chosen for the reference set: **12 radial sectors and 4 concentric coffer rows**.

## Views

- `BRAV-329_CofferedDome_Under45_v004.png` — below the dome at 45 degrees. The concave underside, all four coffer rows, inner rings, springing ring and open oculus are visible in one frame.
- `BRAV-329_CofferedDome_UpAxis_v004.png` — centred straight up the vertical axis. The 12-sector radial symmetry, four coffer rows and true open oculus are explicit.
- `BRAV-329_CofferedDome_Side_v004.png` — straight side elevation with a horizontal springing line. It fixes the low 6.9:27.6 profile and shows the open crown and shell edges without room context.
- Existing approved view, unchanged: `../Revision_v002/Tripo/BRAV-329_CofferedDome_45.png` — exterior at 45 degrees from above.

## Cross-view check

Checked the four images side by side against Jira comment 10666 and the v002 source. They describe one low dome shell: the same radial rib family, four coffer rows, continuous base ring, neutral-grey material and unobstructed oculus. `Under45` and `UpAxis` expose the player-facing interior that the exterior-only Tripo attempt had invented; `Side` fixes the silhouette and rise. Native outputs are retained without upscaling.

ImageGen prompts used the approved v002 dome as the geometry identity and `BOARD_Environment.png` as the style reference. The generated views remain `productionReady:false` until Mustafa approves all four views in Jira.
