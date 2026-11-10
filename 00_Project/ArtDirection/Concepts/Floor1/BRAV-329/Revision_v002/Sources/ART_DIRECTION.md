# ART DIRECTION: the pack every image and model generation starts from

For anyone who makes a concept, a reference image or a Tripo model: Codex, Claude, Başak, a 3D artist.
Written 2026-09-30, after BRAV-144 came back as a photoreal "Witcher" butcher: the image model had the ticket's idea
but never saw our style. **Image models cannot open links or read this repo.** They see only the prompt text and the
images attached to it. So the style has to travel inside every prompt, as text **and** as pictures. This file is the
single place both come from. The full rules behind it are in `AI/CHARACTERS.md` (characters), `AI/ENVIRONMENT_BIBLE.md`
(era, light, colour) and `AI/TRIPO_REFERENCE_GENERATION.md` (the 45° Tripo image); you do not need to read those to
generate, but do not contradict them.

## Golden rules
1. **Every 2D reference is made at Tripo resolution: at least 2048 px on the long side, native.** Ask for that size in the
   request, check the file's real pixel size when it comes back, and never upscale. If the tool returns less (the built-in
   image generator returned 1254 px for a 2048 request on BRAV-329), stop, say so on the ticket, and ask the owner for the
   high-resolution route; do not hand the image on as a reference.
2. **No sheet, no design:** sizes first (§9).

## 1. Before you generate (every time)
1. **Is the ticket open for generation?** A ticket marked **BLOCKED** or **DESIGN FIRST** gets no images. Stop and ask.
2. **Is there an asset sheet?** Every asset that is designed gets a **sheet with its sizes before the first image**
   (§9), and the sheet goes to Başak with the ticket. No sheet, no design. Images are drawn to the sheet's proportions.
3. **Pick the block:** character or creature → §3 Character; anything else (prop, architecture, trap, fixture) → §4
   Environment.
4. **Attach the style references** (§2). Every generation, no exceptions. Text alone loses to the model's photoreal
   default.
5. **Paste the block word for word**, add the ticket's subject in the brackets, add the negative prompt (§5).
6. **Check the result against the references side by side** (§6), and its pixel size (golden rule 1) before anyone else sees it. Off-style results are
   regenerated, not delivered.

## 2. Style references (attach these)
In `AI/art_direction/refs/` (GitLab:
https://gitlab.com/gunesblbrn/bravehood/-/tree/development/AI/art_direction/refs). These are renders of models already in
the game, so they are the look, not an idea of it.

| Use for | Attach | Shows |
|---|---|---|
| Characters, creatures, enemies | `BOARD_Character.png` (or `CHAR_01`, `CHAR_02`, `CHAR_03` separately) | the Knight: hand-placed planes, flat painted colour, the square planar nose and the face family, 45° body |
| Props, architecture, traps, fixtures | `BOARD_Environment.png` (or two or three of `ENV_01`–`ENV_06`) | the accepted Gallery Halls set (`SM_MGG_F1_*`): chiselled facets, chamfered edges, muted hand-painted colour, tarnished brass |

- A tool that takes one image: attach the board. A tool that takes several: attach 2–3 single images closest to the
  subject (a bench for furniture, the arch for architecture, the body and the face for a character).
- Use the image as a **style** reference (style / image-prompt weight), not as the subject: we want our look, not a
  second bench.
- Once a Floor 1 concept is approved, add it here (next number) so later tickets inherit it. Remove nothing without the
  owner.
- The renders are regenerated with `Tools/Art/fbx_style_render.py` (Gallery Halls meshes, 45°, flat light) and the
  Knight QA renders in `Tools/Rigging/knight_tiers/qa/`.

## 3. Character block (characters, creatures, enemies)
Copy word for word; fill the brackets from the ticket.
```
[who: role, body type, height in heads], [clothing and gear, materials], [the one silhouette idea from the ticket],
Renaissance plague hospital c. 1500, stylised low-poly game character in the style of the attached reference images,
authored hand-placed planes, visibly faceted flat-shaded surfaces, hand-painted flat colour with broad value shapes,
muted desaturated palette (forest green, oxblood, umber, charcoal, tarnished brass), stern face with a large square
planar nose, angular jaw, thick brows, small eyes, hair as sculpted masses, heavy cloth with few broad folds,
[view: approval sheet or the 45° Tripo view from TRIPO_REFERENCE_GENERATION §4], pure white background, flat even
neutral light
```
Rules that the block does not say but the result must meet (`CHARACTERS.md` §1):
- **Former humans stay human.** A Thrall, the Matron, a mini-boss keep the same face family and construction; what the
  Veil took is drawn as **regular, symmetric, crystalline** facets (Veil geometry), unlit in the reference image.
- Planes are irregular and slightly asymmetric (a dented plate, a crooked belt). Wear is a chipped plane, never a
  scratch texture.
- No realistic skin pores, stubble, strands, blood splatter, wet shine or grime. Horror comes from shape and posture,
  not gore detail.

## 4. Environment block (props, architecture, traps, fixtures)
The same line as `TRIPO_REFERENCE_GENERATION.md` §7, plus the reference anchor.
```
[part name and what it is], [shape and size in metres], [materials], Renaissance plague hospital c. 1500,
a single isolated game asset in the style of the attached reference images, three-quarter view from 45 degrees,
camera slightly above, long lens, centred, the whole object visible with margin, pure white background, no shadow,
flat even neutral studio lighting, unlit glass and lanterns, stylised low-poly game model, chiselled faceted planes,
chamfered edges, muted low-saturation hand-painted colours, tarnished brass accents
```
For an approval sheet (the object in its room, all states, the Knight for scale), keep the style half of the block
(everything from "stylised low-poly" on) and replace the white-background half with the room and its light from
`ENVIRONMENT_BIBLE.md` §2.

## 5. Negative prompt (both blocks)
```
photorealistic, photo, realistic, hyperrealistic, PBR, physically based, cinematic, film still, concept art painting,
subsurface skin, skin pores, stubble, individual hair strands, dirt overlay, grime, rust streaks, micro scratches,
blood splatter, gore, wet shine, depth of field, bokeh, film grain, dramatic rim light, volumetric light, fog, smoke,
glow, bloom, text, letters, logo, signage, watermark, frame, UI, modern clothing, electric lamp, light bulb, gas lamp,
riveted steel plate, gears, pipes, gauge, rails, firearms, cropped, multiple characters, collage
```
For the 45° Tripo view add the "Avoid" list of `TRIPO_REFERENCE_GENERATION.md` §7.

## 6. Style check (before you hand anything over)
Put the result next to the attached references. It passes only if every line is true:
- [ ] Would sit in the same game as the references: same facet size, same flat painted colour, same muted palette
- [ ] Not photoreal, not a painting, not PBR: no pores, grime, rust streaks, scratches, film look
- [ ] Characters: the face family (square planar nose, angular jaw, thick brows, small eyes), stern at rest
- [ ] Era: c. 1450–1650, nothing on the Never list (`ENVIRONMENT_BIBLE.md` §1)
- [ ] No text, logos or heraldry on the object
- [ ] At least 2048 px on the long side, native (golden rule 1)
- [ ] The ticket's own checklist (parts, sizes, 45° view) passes

Two failed tries in a row: stop, post both images on the ticket with what failed, and ask. Do not tune the prompt
toward the model's taste.

## 7. What went wrong before (so it is not repeated)
- **BRAV-144 (2026-09-30):** run while BLOCKED; no style text, no references → a photoreal leather-apron butcher with
  PBR grime. Fixed by §1.1, §2 and §5.
- **BRAV-146 round 1:** a presentation sheet sent to Tripo → see `TRIPO_REFERENCE_GENERATION.md` §3.
- **Characters from Tripo/Meshy directly:** dense generated meshes read as noise under flat shading
  (`CHARACTERS.md` §1.1). Tripo characters are blockout and reference only.

## 8. The header on every art ticket
Every Jira ticket that asks for an image or a model starts with this block, so that even a chat given only the ticket
gets the essentials. Keep it identical on all tickets; change it here first.
```
ART DIRECTION: read before generating (AI/ART_DIRECTION.md)
1. Status: [OPEN for generation | BLOCKED: design first, do not generate].
2. Attach the style references to every generation: characters → AI/art_direction/refs/BOARD_Character.png,
   everything else → AI/art_direction/refs/BOARD_Environment.png
   (https://gitlab.com/gunesblbrn/bravehood/-/tree/development/AI/art_direction/refs).
3. Paste the [character | environment] style block and the negative prompt from AI/ART_DIRECTION.md §3-5 word for word.
   Look in one line: stylised low-poly game [character | asset], hand-placed faceted planes, flat hand-painted muted
   colour, Renaissance plague hospital c. 1500, like the Knight and the Gallery Halls set. Never photoreal, PBR,
   grime, scratches or a painted concept.
4. Before delivering: style check side by side with the references (§6). Two misses: stop and post both.
```

## 9. The asset sheet (sizes first, then the design)
A design drawn without sizes gets approved by eye and then fails at the greybox: proportions are baked into the model
(Tripo keeps proportions, not scale), and modules must tile. So **every asset gets a sheet before anyone draws it, and
the sheet is given to Başak together with the ticket** (owner, 2026-10-01). The model sheet is
`sheets/BRAV-329_ModuleSheet_v001.md` plus its picture.

A sheet is one page (a table and a scaled picture) with:
1. **Where it goes:** the rooms, with their sizes and heights from the layout (`Floor1_Full_Layout.json`).
2. **Sizes in metres:** W × D × H for each part, with the Knight (1.59 m) drawn beside it to scale. Each number is marked
   **F** (fixed by the layout), **D** (derived), or **P** (proposed, needs the owner's check).
3. **Rules that set the sizes:** the grid (2.3 m, module pitch 4.6 m), door and passage widths, clearances (≥ 3.5 m under
   a ceiling, ≥ 2.5 m lane), the wall thickness (1.0 m).
4. **Fit:** how many modules fill each room, and what is left over (half bays, stretch limits).
5. **Gaps and open questions** found while measuring.
6. **A check of the finished design** against the sheet (outline proportions, ±20 % at 45°).
Props that are not modular (a lantern, a jar) still get the short version: size, where it stands, the scale figure.
Who writes it: the lane that owns the ticket, from the layout and the greybox, before the ticket is opened for design.
