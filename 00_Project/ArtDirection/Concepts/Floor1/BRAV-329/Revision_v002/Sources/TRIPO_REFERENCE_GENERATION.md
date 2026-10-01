# Tripo reference generation: the images we feed to Tripo

For whoever makes the reference images (Başak, or Codex with an image model) and whoever runs Tripo (Codex or a 3D artist).
Jira tickets link here: https://gitlab.com/gunesblbrn/bravehood/-/blob/development/AI/TRIPO_REFERENCE_GENERATION.md
Written 2026-09-29 for the Floor 1 models (Jira BRAV-141 and its sub-tasks, plus BRAV-122/123/125). It applies to
every future Tripo model. Tripo's own settings are in `AI/TRIPO_SETTINGS.md`. The era and style rules are in
`AI/ENVIRONMENT_BIBLE.md` §1 and Jira BRAV-141.

**Start from `AI/ART_DIRECTION.md`:** it holds the style references to attach to every generation and the
character and environment style blocks. The image model never sees this repo; the style reaches it only through the
prompt and the attached references.

## 1. The rule in one line
**One image per part, a 45° three-quarter view, the part alone on pure white, flat light, no text.** That image is
Tripo's input. Nothing else goes to Tripo.

**Sizes first:** no reference image is drawn before the part has an asset sheet (sizes, proportions, where it fits;
`ART_DIRECTION.md` §9). The parts plan below repeats the sheet's numbers.

## 2. Two separate deliverables per ticket
| | Approval sheet | Tripo reference |
|---|---|---|
| For | Mustafa, to judge the design | Tripo, to build the mesh |
| Shows | the object in its room, all states, the Knight for scale, the 20 m silhouette, the colour language, the era check | one part, nothing else |
| Views | front, side, three-quarter, in-room eye level | **one 45° view** |
| Light | the game's mood lighting, glows allowed | flat and neutral, nothing glows |
| Text | captions and labels allowed | none |

The approval sheet also carries the **parts plan**: an exploded diagram with numbered parts and their sizes in metres.
It lists exactly the Tripo images to make. Mustafa approves the sheet and the Tripo images before any generation.

## 3. Why a presentation sheet can't be a Tripo input
Tripo turns everything in the image into geometry and texture. The round-1 boss-door sheet (BRAV-146) failed on all of these:
- the views sat in the room, so the walls, banners, a man, floor rails and fog would all be modelled;
- the dramatic light, glows and bloom would be baked into the texture or become lumps of geometry;
- there was text on the object;
- the door, both machines, the lamps and the gallery were one blob, so nothing could move or be reused;
- parts were cropped at the image edge.

## 4. The 45° view
- **Camera:** 45° around the object from its front (between the front and the right side) and about 20–30° above
  its mid-height. Both the front face and one side face are clearly visible, and so is the top when it matters (a
  table, a lid, a plinth).
- **Lens:** long lens, close to orthographic. No wide-angle distortion, no converging verticals.
- **Framing:** the whole part in frame, centred, with about a 10 % margin on every side. Nothing cropped.
- **Orientation:** the part stands upright on its base as it would in the game (a wall piece as if mounted, a hanging
  lantern hanging straight down).
- **Characters:** the same 45° angle, camera at chest height, full body from head to feet, A-pose (arms about 45°
  down, feet under the shoulders), neutral face, empty hands.
- The hidden back is left to Tripo. If Tripo invents a wrong back on a part that is seen from all sides, the Tripo
  operator asks for one extra image from the opposite 45° (behind-left) for that part only.

## 5. Rules for every image
1. **One part, isolated.** No room, wall, floor, ground plane, sky, people, scale figure, hands, banners, text,
   labels, arrows or measurement lines.
2. **Background:** pure white `#FFFFFF`. No cast shadow, no contact shadow, no vignette, no gradient.
3. **Light:** flat, even, neutral white studio light. No coloured light, rim light, glow, bloom, emissive, light rays,
   smoke or fog. Glass, horn, crystal, candles and lantern panes are drawn **unlit**, as material. The game adds the light.
4. **State:** the rest state (sealed, empty socket, lantern unlit, trap armed). A part whose **shape** changes between
   states (an open leaf, a claimed stump, a cracked jar) gets its own image. A change of light only (lit/unlit) does not.
5. **Split:** anything that moves, rotates, lights up or repeats is its own part. Draw a repeated or mirrored part
   **once** (one lantern of three, one door leaf of two, the left windlass of a pair); Blender copies and mirrors it.
6. **Thin things:** chains, ropes, thin bars, wire and fine grilles are drawn chunky and solid, or left out and marked
   "built in Blender" on the parts plan. Tripo fails on thin, see-through detail.
7. **Look:** the Bravehood environment convention: chiselled, faceted low-poly shapes, chamfered edges, flat planes with
   visible facets, muted low-saturation hand-painted colour. It should sit next to the accepted Dungeon 3 Gallery Halls set
   (`SM_MGG_F1_*` in `Assets/ProductionAssets/Levels/Dungeon 3 Design — Floor 1 Gallery Halls 3D AI References/`).
8. **Proportions:** true to the sizes on the parts plan (the ticket and greybox sizes). Tripo keeps proportions, not scale;
   scale is set in Blender.
9. **Era:** a Renaissance plague house, c. 1450–1650 (bible §1). Nothing from the Never list: no electric or gas lamps,
   bulbs, enamel, riveted steel plate, portholes, industrial gears, gauges, rails, printed lettering, modern clothing.
10. **File:** PNG, **at least 2048 px on the long side, native (golden rule: ask for it in the request, check the real size, never upscale)**, square or 4:3.

## 6. Naming and delivery
- `<ticket>_<Part>_45.png`, e.g. `BRAV-146_GateLeaf_45.png`; a shape state adds the state: `BRAV-145_Crystal_Claimed_45.png`;
  the extra back view, only when asked: `<ticket>_<Part>_45back.png`.
- Upload to the art archive `Bravehood-Production-Archive/00_Project/ArtDirection/Concepts/Floor1/<ticket>/`: the
  approval sheet at the top, the Tripo images in `Tripo/`. Link both on the Jira ticket.
- Tripo: single-image input, one generation per image, settings per `AI/TRIPO_SETTINGS.md`. The Tripo task name repeats
  the image name.

## 7. Prompt template (AI image generation)
Fill in the brackets; keep the rest word for word. **Attach the environment style references from
`AI/ART_DIRECTION.md` §2 with every prompt**; for characters use that file's character block instead of this one.

```
[part name and what it is], [shape and size in metres], [materials], Renaissance plague hospital c. 1500,
a single isolated game asset, three-quarter view from 45 degrees, camera slightly above, long lens,
centred, the whole object visible with margin, pure white background, no shadow,
flat even neutral studio lighting, unlit glass and lanterns, stylised low-poly game model,
chiselled faceted planes, chamfered edges, muted low-saturation hand-painted colours
```
Avoid (negative prompt): `text, letters, signage, logo, banner, person, hands, room, wall, floor, ground, cast shadow,
fog, smoke, glow, bloom, light rays, coloured light, electric lamp, light bulb, rivets, steel plate, gears, pipes,
gauge, rails, cropped, multiple objects, collage, wide angle, perspective distortion`.

Example (BRAV-146, part 2):
```
A heavy oak gate leaf clad in hand-forged iron strapwork and nail heads, 2.4 m wide and 6 m tall with a pointed-arch top,
oak and black iron, Renaissance plague hospital c. 1500, a single isolated game asset, three-quarter view from 45 degrees,
camera slightly above, long lens, centred, the whole object visible with margin, pure white background, no shadow,
flat even neutral studio lighting, stylised low-poly game model, chiselled faceted planes, chamfered edges,
muted low-saturation hand-painted colours
```
Take sizes from the ticket and the greybox; the numbers above are an example only.

## 8. Check before handing an image to Tripo
- [ ] Passes the style check in `AI/ART_DIRECTION.md` §6 (matches the attached references)
- [ ] One part only, whole, centred, nothing cropped
- [ ] Pure white background, no shadow
- [ ] 45° three-quarter view, long lens, part upright
- [ ] Flat light; nothing glows; glass and flames unlit
- [ ] No text, people, room or floor
- [ ] Chains, ropes and thin bars chunky or marked "built in Blender"
- [ ] Matches the approved approval sheet and its parts plan
- [ ] Nothing from the era Never list
- [ ] Named `<ticket>_<Part>_45.png`, at least 2048 px

## 9. Floor 1 parts (starting point; Başak confirms on each parts plan)
| Ticket | Parts, one 45° image each |
|---|---|
| BRAV-142 Head Matron | body (A-pose, empty hands) · candle holder |
| BRAV-143 Censer Wretch | body (A-pose) · censer (chain built in Blender) |
| BRAV-145 Veil shard | crystal cluster · claimed stump · carried shard piece |
| BRAV-146 Boss gate | stone portal with jambs and tympanum · gate leaf (one) · portcullis grille · windlass unit with cog wheel (one side) · counterweight stone · socket niche · lantern with bracket (one) · gallery grille panel (one bay); chains built in Blender |
| BRAV-147 Specimen jars | named jar (blank tag area) · spoiled jar · shelf rack (one bay) |
| BRAV-148 Exits ×6 | per exit: the static frame and each moving part (gate leaf, chute lid, cellar door leaf, bell) · E3 the buyer's desk |
| BRAV-150 Hearth shrine | wall niche with the saint · oil lamp · offerings set · kneeling mat |
| BRAV-151 Veil stones | Quickened spire · Ironhide block · Keen blade · plinth (shared) |
| BRAV-152 Shutter | slab · jamb rail · crank with chain drum · counterweight and pulley |
| BRAV-153 Cart tipper | bier cart (upright) · hinge bracket · ratchet lever |
| BRAV-154 Arena | plain bed · crystal-grown bed · wall crystal growth · hanging crystal · floor tile module |
| BRAV-155 Wayfinding | plan board with frame (blank face) · tally bracket with three lanterns |
| BRAV-156 Light fixtures | each of the six fixtures |
| BRAV-157 Front of house | donor wall panel · one plaque · ledger desk · carved desk · chair · cabinet · portrait frame (blank) · cabinet of curiosities |
| BRAV-158 Wards and staff | made ward bed · bedside stool set · curtain screen · cot · mirror |
| BRAV-159 Back of house | anatomy table · sealed chest · stone slab · charnel niche wall module · ice-cellar cask · rope hoist platform · windlass · hoisted basket · chute hatch |
| BRAV-122 / 123 / 125 | the parts in each ticket's Structure section |
