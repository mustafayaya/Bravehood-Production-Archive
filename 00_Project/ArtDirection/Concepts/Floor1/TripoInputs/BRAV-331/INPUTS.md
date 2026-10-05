# INPUTS BRAV-331: Laundry and kitchen kit

Tripo input images, copied byte for byte from the archive branch `codex/brav-352-floor1-revisions-review-2026-10-03` (PR #8 / #7 source branches), renamed to `BRAV-331_<Part>[_<State>]_45.png`. SHA256 of each copy was checked equal to the source file hash, to the pack's own `GENERATION_MANIFEST.json` / `REFERENCE_MANIFEST.json`, and for the 10-02 files to the local mirror `output/floor1-review-audit/`. No newer version exists in `output/`.

| file | part | original name | source (archive branch : path) | SHA256 | approval basis | notes |
|---|---|---|---|---|---|---|
| `BRAV-331_WashTub_45.png` | WashTub | `BRAV-331_WashTub_45_DRAFT_v001.png` | `codex/brav-352-floor1-revisions-review-2026-10-03` : `00_Project/ArtDirection/Concepts/Floor1/BRAV-331/UserApproved_2026-10-02/BRAV-331_WashTub_45_DRAFT_v001.png` | `e5fab3f4149a078b98ac317915ed58c5bd54a168bf5635f1e92afb5e52b1ccc8` | user 2026-10-02 (kept by the 10-03 revision comments) | Ø 1.0 x 0.6, empty. Hoops: plain flat bands, no hex bolt heads (Never list: rivets) |
| `BRAV-331_WashPaddle_45.png` | WashPaddle | `BRAV-331_WashPaddle_45_DRAFT_v001.png` | `codex/brav-352-floor1-revisions-review-2026-10-03` : `00_Project/ArtDirection/Concepts/Floor1/BRAV-331/UserApproved_2026-10-02/BRAV-331_WashPaddle_45_DRAFT_v001.png` | `7831ed1dcd1d06a18b9e5ed6406aef7d843db4f5903108b55dbd1b4cdafffbb6` | user 2026-10-02 (kept by the 10-03 revision comments) | ~1.2 long; separate static mesh (the sheet says baked in the tub, the image is separate) |
| `BRAV-331_DryingRack_45.png` | DryingRack | `BRAV-331_DryingRack_45_DRAFT_v001.png` | `codex/brav-352-floor1-revisions-review-2026-10-03` : `00_Project/ArtDirection/Concepts/Floor1/BRAV-331/UserApproved_2026-10-02/BRAV-331_DryingRack_45_DRAFT_v001.png` | `97dae05c870812d3df4c991d3f1a8c8ea8ecb9ad6020a5b06110346eb9135cd2` | user 2026-10-02 (kept by the 10-03 revision comments) | 2.0 x 0.6 x 1.8 |
| `BRAV-331_HangingSheet_45.png` | HangingSheet | `BRAV-331_HangingSheet_45_REVISION_DRAFT_v002.png` | `codex/brav-352-floor1-revisions-review-2026-10-03` : `00_Project/ArtDirection/Concepts/Floor1/RevisionReview_2026-10-03/BRAV-331/BRAV-331_HangingSheet_45_REVISION_DRAFT_v002.png` | `600da8ff1f793e2f67496ca86e8b458464d9bc75d6a04b00098e558330bd329e` | 10-03 revision pack; delegated visual approval 2026-10-04 (comment 10871) | 1536x1024. 1.8 wide, 0.7 drop each side; no rail drawn |
| `BRAV-331_BreadTallyBoard_45.png` | BreadTallyBoard | `BRAV-331_BreadTallyBoard_45_REVISION_DRAFT_v001.png` | `codex/brav-352-floor1-revisions-review-2026-10-03` : `00_Project/ArtDirection/Concepts/Floor1/RevisionReview_2026-10-03/BRAV-331/BRAV-331_BreadTallyBoard_45_REVISION_DRAFT_v001.png` | `3afae89aeaca76803451f4a19e174f31662356223898dbc0d4e367d7c98d0be9` | 10-03 revision pack; delegated visual approval 2026-10-04 (comment 10871) | 1.0 x 0.10 x 0.8; notched sticks + two netted loaves; no writing |
| `BRAV-331_OakSettle_45.png` | OakSettle (Settle) | `BRAV-331_OakSettle_45_DRAFT_v001.png` | `codex/brav-352-floor1-revisions-review-2026-10-03` : `00_Project/ArtDirection/Concepts/Floor1/BRAV-331/UserApproved_2026-10-02/BRAV-331_OakSettle_45_DRAFT_v001.png` | `2cec013a122141faf529b636cbac4de3f373e3a8ae8058c25f2d7a0c50954957` | user 2026-10-02 (kept by the 10-03 revision comments) | 1.8 x 0.6 x 1.4, seat 0.45 |

**Pack split:** tub, paddle, rack, settle from the 2026-10-02 pack; hanging sheet and tally board from the 2026-10-03 pack. All are final.

## NO IMAGE for
- Tub with the paddle in it (sheet: 'paddle baked in'): no image; the paddle is modelled separately.
- Bloodied/clean sheet variants: material swap on the same mesh, no image needed.

## Superseded / not inputs: do NOT use
- `BRAV-331_LinenSheet_45_DRAFT_v001.png` (10-02): replaced by HangingSheet v002.
- `BRAV-331_BreadTallyBoard_45_DRAFT_v001.png` (10-02, bare pegs): replaced by the 10-03 board.
- `BRAV-331_HangingSheet_45_REVISION_DRAFT_v001.png` (10-03): rejected (too narrow and tall).
- `Approval_DRAFT_v001.png` (board); all `PROMPT_*`, `MEASURED_ENVELOPES.svg`, `REVIEW.html`, `CHANGE_LIST.md`, manifests: records, not inputs.
