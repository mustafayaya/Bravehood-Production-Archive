# BRAV-329 architecture references — v001

2026-09-30. Design preparation following Mustafa's BRAV-329 comment 10640 and BRAV-317 comment 10641. Mustafa approved the design sheet and all individual designs in this chat, including column v002 and open-oculus dome v003, and requested saving and continuation. Canonical approved designs are in `ApprovedDesigns/`; `APPROVED_MANIFEST.json` records their sources and SHA-256 hashes. Dimensional validation and production resolution remain pending; no Tripo generation or Unity changes.

## Confirmed scope

| Part | Planned Tripo file | Location |
|---|---|---|
| 01 Gothic rib-vault bay | BRAV-329_RibVaultBay_45.png | Chapel, Admissions Hall, Ward 1 |
| 02 Open timber roof-truss bay | BRAV-329_TimberTrussBay_45.png | Long wards |
| 03 Barrel-vault bay | BRAV-329_BarrelVaultBay_45.png | Arcades, service passages, crypt |
| 04 Coffered ceiling panel | BRAV-329_CofferedPanel_45.png | Benefactor's Study |
| 05 Coffered dome | BRAV-329_CofferedDome_45.png | Perfected Ward; footprint requires BRAV-154 coordination |
| 06 Pointed doorway arch | BRAV-329_PointedDoorArch_45.png | Gothic core |
| 07 Pointed arcade bay | BRAV-329_PointedArcadeBay_45.png | Gothic core |
| 08 Round arcade arch | BRAV-329_RoundArcadeArch_45.png | Cloister/loggia; repeats with separate column 09 |
| 09 Renaissance column with capital/base | BRAV-329_RenaissanceColumn_45.png | Renaissance arcade |
| 10 Gothic lancet window | BRAV-329_LancetWindow_45.png | Gothic core; neutral dark unlit glass |
| 11 Straight balustrade run | BRAV-329_BalustradeRun_45.png | Ward 2 gallery, Great Hall stair, courtyard closures |
| 12 Balustrade end post | BRAV-329_BalustradeEndPost_45.png | Shared closure/stair component |
| 13 Great stair module | BRAV-329_GreatStair_45.png | Double-height Great Hall |

Pilaster is conditional: add only if the approved arcade assembly requires it. Repeated columns and posts are generated once and duplicated during assembly. Courtyard props belong to BRAV-334.

## Dimensional constraints and unresolved measurements

- Ward envelope: 41–46 m long, 13.8 m wide, 8–12 m high (ticket constraints, not a module grid).
- Service passages: 3.0 m clear.
- Minimum ceiling/springing over walkable floor: 3.5 m.
- Minimum clear fighting lane/loggia opening: 2.5 m; columns stay outside fighting lanes.
- Scale figure: existing Knight approximately 1.59 m. World scale remains locked.
- **Every individual module W/H/D, grid pitch, interface position, stair rise/run and dome footprint remains TBD.** The game repo and `Assets/Core/Data/LevelDesign/Floor1_Full_Layout.json` are absent from this workspace. Do not infer module dimensions from room envelopes or concept images.

## Design constraints

2026-09-30 owner revision in this chat: the Perfected Ward dome has a genuinely open circular apex oculus like the Pantheon; no solid apex boss or glass closure. The underside reference v003 preserves the coffer pattern and shows the aperture through the shell. Exact aperture diameter remains pending dimensional validation.

Stone and fresco plaster, no glazed wall tile including Apothecary. Floor maiolica/terracotta remains allowed under BRAV-336. Plain grey-blue architecture without gilding. Reference Gallery Halls meshes provide style only, not Floor 1 reuse. Perfected Ward dome expresses perfection through regular symmetric coffers, without adding decorative crystal.

Courtyards are visible, not walkable, closed by visible balustrades or low walls. No invisible wall substitution and no new courtyard lights; use neutral overcast outdoor light.

## Delivery and approval

### Current delivery scope (owner correction, 2026-09-30)

Mustafa explicitly requested saving and uploading the approved artwork only, with no Tripo work. This delivery is the approved design archive, not a mesh-production handoff. The canonical selections, approved sheet, manifest and gallery are published on a feature branch for review; no Tripo generation is part of this request. Historical draft files remain local. Resolution and greybox measurements describe the source files' limitations, not a request to regenerate or proceed to Tripo.

1. Review-sheet v001 is a design draft. It does not establish dimensions or pass greybox fit checks.
2. Obtain the layout JSON, module grid and BRAV-154 footprint, then produce the measured exploded plan and per-part references.
3. Every image generation attaches `BOARD_Environment.png` and uses ART_DIRECTION sections 4–5 verbatim with the appropriate approval-sheet adaptation.
4. Each final Tripo reference is one isolated part, 45 degrees, flat neutral light, white background, no shadow/text/room/scale figure, at least 2048 px on its long edge. Check style beside the board before delivery.
5. Mustafa approves both the measured sheet and individual Tripo images before any Tripo generation. This is required by `AI/TRIPO_REFERENCE_GENERATION.md` section 2.

No meshes, prefab edits, scene edits, commit or push are included in this preparation.

## Draft image QA

### Reference generation following sheet approval

The generated iteration history is saved in `ReferenceDrafts/` and approved selections in `ApprovedDesigns/`, outside the final `Tripo/` folder. The built-in image generator returned 1254x1254 PNGs despite the 2048x2048 request. The designs are approved, but do not meet the reference policy's 2048 px minimum. Do not upscale and claim native resolution compliance. CLI/API high-resolution generation requires user authorization for that fallback workflow.

Design approval is recorded; exact dimensions, greybox grid and dome footprint remain unresolved. Ceiling underside views are chosen for readability and must be reconciled with the final 45-degree camera requirement before delivery.

All 13 reference drafts were produced and visually reviewed. Review gallery: `REFERENCE_REVIEW_v001.html`. Before final image delivery: remove the rear infill from the open timber truss; remove integrated end posts from the straight balustrade and great stair (separate post asset already exists); eliminate brown trim that could read as brass on plain stone architecture. Other drafts show isolated complete parts, empty arch apertures and neutral dark unlit window glass. Every draft remains pending native-resolution compliance, dimensions and individual approval. No greybox fit, camera safety or mesh validation is claimed.

`BRAV-329_DesignReview_DRAFT_v001.png` is a visual direction draft, not the final approval sheet. Board comparison: broad facets and muted colours are present; no gilded fantasy trim or industrial elements. The room renders include finer masonry subdivision than the reference board; simplify in final part images. The doorway-arch vignette incorrectly includes a door leaf; remove that leaf from the isolated arch reference. The numbered catalogue is not a fully measured exploded assembly. Exact dimensions and image proportions have not been validated against the greybox. No image from this sheet may be cropped and used as a Tripo input.
