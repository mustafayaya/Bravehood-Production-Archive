# Floor 1 Tripo input package (2026-10-05)

The final approved 45° reference images for the Floor 1 assets whose designs were approved on 2026-10-02 to 2026-10-04,
one folder per Jira ticket, **the one place Ömer takes Tripo inputs from**. Built from the 2026-10-02 pack (archive PR #7),
the 2026-10-03 revision pack (PR #8) and the newest approved files that existed only as Jira attachments or in Codex's
local `output/` folder. Every file is a byte-for-byte copy of its source (SHA256 in each `INPUTS.md`), renamed to
`BRAV-<n>_<Part>_45.png`.

Each folder has `INPUTS.md`: file, part, original name, source, SHA256, approval basis, notes; the parts that have **no
image** (build in Blender or ask for one); and the superseded files **not** to use. Do not take images from the PR packs
instead: they still list superseded versions.

Rules that apply to every ticket: `AI/TRIPO_SETTINGS.md` (Smart Mesh, triangle, FBX; segment only traps and moving
parts), `AI/TRIPO_REFERENCE_GENERATION.md`, `AI/ART_DIRECTION.md`. Numbers in the asset sheets (bravehood-docs
`sheets/`) beat the drawn proportions of the images: normalise in Blender.

| Ticket | Images | Status |
|---|---|---|
| BRAV-145 | 3 | Status: READY (scale the stump to 0.15 m per the sheet; add roots to the cluster) |
| BRAV-146 | 6 | Status: READY, parts Tally_Lantern, Windlass_Frame, Windlass_Drum, Counterweight first; HOLD: Gate_Portal and Gate_Leaf (drawn doorway about 4.3-4.6 x 6.8 m vs layout 6.9 x 5.0: Mustafa chooses the opening; antechamber width/ceili |
| BRAV-147 | 4 | Status: READY, parts NamedJar and (Blender variants) Spoiled/Decoy first; HOLD: ShelfRack (image has 4 boards at the wrong heights and no marked slot: fix in Blender or redraw) |
| BRAV-148 | 11 | Status: READY, parts E6, E3, E2, E5 first; HOLD: E1 facade and E4 bell (Blender edits of the Meshy models Gate_Obsidian_01 / Signal_Bell_01, E1 leaf has no image) and the E3 frame height (<= 5.8 under the 6.0 ceiling) and "4.6 = o |
| BRAV-150 | 6 | Status: READY |
| BRAV-151 | 5 | Status: READY, parts Quickened, Ironhide, Plinth, RadialFloor first; HOLD: Keen (image is a one-sided slanted slab, not a symmetric blade: decide redraw or symmetric tip in Blender before generating) |
| BRAV-153 | 4 | Status: READY, parts Cart, Lever, Drum first; HOLD: HingeBracket (cog-like quadrant must become a coarse ratchet) and LeverPost/StopPin/tail quadrant (no image: Blender build) |
| BRAV-154 | 4 | Status: READY, parts Floor_Tile, Wall_Crystal_Growth, Hanging_Crystal, Crystal_Bed first (bed frame from BRAV-158); HOLD: dome and oculus (BRAV-329 part 05, v004 not approved) and the hanging-crystal rod length |
| BRAV-155 | 4 | Status: READY, TallyLantern, TallyBar, LandscapeFrame, SquareFrame; HOLD: board-lamp swan-neck brackets (no image: Blender build) and the portal mount (needs the BRAV-146 antechamber) |
| BRAV-156 | 6 | Status: READY, parts WallLantern, OilLamp, Pricket, CandleCrown first; HOLD: InteractionRim (drawn near top-down, model disc + dome by the numbers or ask for a redraw) and glazing hex colours (lead to supply). |
| BRAV-157 | 9 | Status: READY, parts DonorPanel, DonorPlaque, LedgerDesk, GreatLedger, HighChair, PortraitFrame, ChildShoe first; HOLD: BenefactorDesk sealed bill (not drawn, add in Blender) and ledger desk position (sheet conflict, level lane). |
| BRAV-158 | 6 | Status: READY, WardBedFrame + WardMattress first; HOLD: bed pair not in the 2026-10-03/04 approval (user-approved 2026-10-02 only, Mustafa to confirm), straps / stool set / curtain screen / flowers / shawl / bottles have no approv |
| BRAV-330 | 7 | Status: READY (jar triangle cap: proposal below, Mustafa to confirm; L jar on floor/tables only until the shelf is measured) |
| BRAV-331 | 6 | Status: READY (confirm the loaves and hoop-bolt calls under Open items; they do not block starting) |
| BRAV-332 | 4 | Status: READY, parts AltarBlock / AltarCloth / Candlestick / PredellaStep first; HOLD: Statue_Caregiver master and Shrine LOD (input lives in the BRAV-150 package; image not yet in the archive, shrine fit and budget decision pendi |
| BRAV-333 | 6 | Status: READY |
| BRAV-334 | 11 | Status: READY, all parts except PitCorner first (PitCorner HOLD: the approved v003 image is an L with ~2 m arms, not a 1x1x0.5 module, confirm the shape before Tripo; PitBoard is a Blender box, no image) |
| BRAV-335 | 4 | Status: READY (BellTower: model the belfry empty, the v003 image shows the bell; LaundryChimney: build to 26 m / 5x5, the image is squat) |
| BRAV-336 | 6 | Status: READY, six reference surfaces first (lime plaster, fresco band, brick, roof tile, plank floor, floor tile); HOLD: gravel-earth, turf, stone flags, maiolica, `Stone_01` repaint, decal atlas have no reference, and the ward-d |
