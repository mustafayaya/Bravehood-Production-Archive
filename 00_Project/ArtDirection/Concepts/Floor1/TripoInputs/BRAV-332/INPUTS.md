# INPUTS BRAV-332: Chapel altar and shared caregiver statue

Tripo input images, copied byte for byte from the archive branch `codex/brav-352-floor1-revisions-review-2026-10-03` (PR #8 / #7 source branches), renamed to `BRAV-332_<Part>[_<State>]_45.png`. SHA256 of each copy was checked equal to the source file hash, to the pack's own `GENERATION_MANIFEST.json` / `REFERENCE_MANIFEST.json`, and for the 10-02 files to the local mirror `output/floor1-review-audit/`. No newer version exists in `output/`.

| file | part | original name | source (archive branch : path) | SHA256 | approval basis | notes |
|---|---|---|---|---|---|---|
| `BRAV-332_AltarBlock_45.png` | AltarBlock | `BRAV-332_AltarBlock_45_REVISION_DRAFT_v001.png` | `codex/brav-352-floor1-revisions-review-2026-10-03` : `00_Project/ArtDirection/Concepts/Floor1/RevisionReview_2026-10-03/BRAV-332/BRAV-332_AltarBlock_45_REVISION_DRAFT_v001.png` | `077b050d720170868d3ea07d13fa9a671653a4f9ec2f62bf31d50e5b1e442b77` | 10-03 revision pack; delegated visual approval 2026-10-04 (comment 10872) | 2.0 x 0.9 x 1.0; drawn ~15% tall, follow the numbers; stone top, no cross |
| `BRAV-332_AltarCloth_45.png` | AltarCloth | `BRAV-332_AltarCloth_45_REVISION_DRAFT_v001.png` | `codex/brav-352-floor1-revisions-review-2026-10-03` : `00_Project/ArtDirection/Concepts/Floor1/RevisionReview_2026-10-03/BRAV-332/BRAV-332_AltarCloth_45_REVISION_DRAFT_v001.png` | `8a3300943e7baa4fdb0c8d7df0accf5aacf50314c57f21352f727890593eabbb` | 10-03 revision pack; delegated visual approval 2026-10-04 (comment 10872) | top 2.1 x 1.0, front drop 0.6, sides 0.3 |
| `BRAV-332_Candlestick_45.png` | Candlestick | `BRAV-332_Candlestick_45_REVISION_DRAFT_v001.png` | `codex/brav-352-floor1-revisions-review-2026-10-03` : `00_Project/ArtDirection/Concepts/Floor1/RevisionReview_2026-10-03/BRAV-332/BRAV-332_Candlestick_45_REVISION_DRAFT_v001.png` | `56511ca4b603206723a76a6bae933198f854dec10ac958ceb9b3a92e264ce8bb` | 10-03 revision pack; delegated visual approval 2026-10-04 (comment 10872) | Ø 0.18 x 0.5; unlit candle, no flame |
| `BRAV-332_PredellaStep_45.png` | PredellaStep | `BRAV-332_PredellaStep_45_REVISION_DRAFT_v002.png` | `codex/brav-352-floor1-revisions-review-2026-10-03` : `00_Project/ArtDirection/Concepts/Floor1/RevisionReview_2026-10-03/BRAV-332/BRAV-332_PredellaStep_45_REVISION_DRAFT_v002.png` | `569bb8bbe6fbbe95224b9f54e18a4f69144fcb9e793ca1d57f428d4b3a35cf07` | 10-03 revision pack; delegated visual approval 2026-10-04 (comment 10872) | 1536x1024. 3.0 x 1.8 x 0.15 |

**Statue master:** the shared anonymous-caregiver master input (`BRAV-150_332_AnonymousCaregiver_45_DELEGATED_APPROVED_v001.png`, Jira attachment 11975 on BRAV-332 = 11974 on BRAV-150, local `output/art-review/SharedCaregiver/`) is packaged **only in the BRAV-150 package** (one owner, one model). BRAV-332 needs from it: the 2.10 m master (figure 2.00 + base 0.10, W/D <= 0.54) for the Chapel on the 0.8 x 0.8 x 0.7 plinth, and a Shrine LOD (~1.5k tris) used at uniform scale 0.357143.

## NO IMAGE for
- **Statue master** (anonymous hospital caregiver, 2.10 m, shared): NOT in this package. The single input lives in the BRAV-150 package (one owner). See below.
- Chapel plinth 0.8 x 0.8 x 0.7: plain block, no image (Blender box, no Tripo).
- Lit-candle state: not a shape change (flame is `Candlelight_01`).

## Superseded / not inputs: do NOT use
- `BRAV-332_PredellaStep_45_REVISION_DRAFT_v001.png` (10-03): rejected (too thick, ornamental).
- `Approval_DRAFT_v002.png` (10-02 overview board with a room render): not a single part.
- The raw monk GLBs (`Meshy_AI_Hooded_Monk_in_Prayer...`, `Meshy_AI_Monk_with_Open_Bible...`): reuse rejected.
- All `PROMPT_*`, `MEASURED_ENVELOPES.svg`, `REVIEW.html`, `CHANGE_LIST.md`, manifests: records, not inputs.
