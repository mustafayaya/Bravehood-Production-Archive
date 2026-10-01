# BRAV-329 module sheet v001: sizes for the Floor 1 architecture kit

**2026-10-01 · for Başak and Codex · status: PROPOSED, needs Mustafa's check on the `P` rows.** Give this sheet to whoever
redraws or re-generates a BRAV-329 part. The picture is `BRAV-329_ModuleSheet_v001.png` (every part to scale next to the
1.59 m Knight). Style comes from `AI/ART_DIRECTION.md` and the approved designs; this sheet only fixes **sizes and
proportions**.

How each number is marked: **F** = fixed by the greybox layout (`Floor1_Full_Layout.json`), **D** = derived from F by the
2.3 m grid, **P** = a proposal made here, change it if you disagree.

## 1. The grid and the fixed facts
| Fact | Value | |
|---|---|---|
| Layout grid G | **2.3 m** (2 m × the 1.15 layout scale); every room width and length is a multiple of it | F |
| **Module pitch** | **4.6 m = 2G** | D |
| Knight (scale figure) | 1.59 m; world scale is locked | F |
| Wall thickness | 1.0 m (all 391 walls) | F |
| Doors | 4.5 m high; widths 3.5 (16 of 75), 4.0, 4.5, 4.6; a few 5.0–7.0 tall gates | F |
| Passage widths | 4.0 (service passages), 4.6 (walkways), 6.9 (spines); **the ticket's "3.0 m clear" is wrong, 4.0 is the narrowest** | F |
| Room heights | 5.4 · 5.5 · 6.0 (crypt, passages) · 7.0 (service) · 8.0 (wards) · 9.5 · 11.92 (Gothic core) · 12.0 (Perfected Ward) | F |
| Parapets / rails | 1.2 m high × 0.4 m thick (32 parapets); gallery rail 1.1 × 0.3 | F |
| Stair flight | rise 2.473 m over a 6.5 m run, 8 risers of 0.309 m; widths 3.5 / 4.0 / 5.8 | F |
| Clearance rules | ≥ 3.5 m under any ceiling over walkable floor; ≥ 2.5 m clear fighting lane | F |

## 2. The 13 parts
Sizes in metres (W across, D depth, H height). "Springing" is where the curve starts above the floor.

| # | Part | Size | Rule that sets it | |
|---|---|---|---|---|
| 01 | **Rib-vault bay** | plan 4.6 × 4.6; shell rise **2.6** | springing = room height − 2.6 (5.4 in an 8 m ward, 9.3 in an 11.92 m hall); pointed arches, 4 corner springers 0.9 wide | D / P |
| 02 | **Timber truss bay** | span **13.8**, bay depth 4.6, rise **4.5** | wall plate = room height − 4.5 (3.5 in an 8 m ward, 5.0 in 9.5, 7.4 in 11.92); open, no tie beam in the lane | D / P |
| 03 | **Barrel-vault bay** | span 4.6, length 4.6, rise **2.3** (semicircle) | springing = room height − 2.3 (3.7 in the 6.0 crypt and passages, over the 3.5 minimum) | D / P |
| 04 | **Coffered ceiling panel** | 4.6 × 4.6, depth 0.45, 2 × 2 coffers | the Study is 13.8 × 13.8 = 3 × 3 panels, underside at 4.98 (room 5.43) | D |
| 05 | **Coffered dome** | Ø **27.6**, rise **6.9**, shell 1.0; **open oculus Ø 4.6** | Perfected Ward is 50.6 × 46.0 × 12.0; springing at 5.1; centred on the arena; the rest of the ceiling stays flat. **Footprint to be agreed with BRAV-154** | P |
| 06 | **Pointed doorway arch** | opening **4.6 × 4.5**, overall 6.2 × 5.3 × 1.0 | doors are 4.5 high; springing at 2.5; stretchable to the 3.5 opening (width only, ≤ 25 %) | F / P |
| 07 | **Pointed arcade bay** | pitch 4.6, clear span 3.9, depth 1.0; springing 3.7, crown 5.8 | repeats between piers; the sheet draws two to show the repeat | D / P |
| 08 | **Round arcade arch** | pitch 4.6, clear span 3.9, depth 1.0; springing 3.7, crown 5.65 | the loggia is 4.6 deep (the ward-front veranda), arch pitch 4.6, columns outside fighting lanes | D / P |
| 09 | **Renaissance column** | height **3.7** = base 0.45 + shaft 2.75 + capital 0.5; shaft 0.7, base 1.1, capital 1.0 | its top is the springing of 07 / 08 | D / P |
| 10 | **Lancet window** | overall 2.3 × 5.0 × 1.0; glass 1.15 × 4.0, sill 1.5 | sits in a 1.0 m wall; sill above the Knight's chest | D / P |
| 11 | **Balustrade run** | 2.3 long × 0.4 deep × **1.2** high, 4 balusters | replaces the 1.2 m parapets; **no built-in posts** | F |
| 12 | **Balustrade end post** | 0.5 × 0.5 × **1.4** | one part, shared by runs and stair | D / P |
| 13 | **Great stair flight** | width **5.8**, run **6.5**, rise **2.473** (8 × 0.309 riser, 0.8125 tread) | the layout's own flight; two stacked flights make the double height; no built-in rails | F |

## 3. How they fit the rooms
At a 4.6 m pitch the vaults and trusses tile the greybox exactly:
| Room | Size × height | Bays |
|---|---|---|
| Intake | 27.6 × 23.0 × 11.92 | 6 × 5 rib-vault |
| Hub | 32.2 × 32.2 × 11.92 | 7 × 7 rib-vault |
| Chapel | 23.0 × 18.4 × 11.92 | 5 × 4 rib-vault |
| Isolation Gate Hall | 55.2 × 18.4 × 11.92 | 12 × 4 rib-vault |
| Ward 1 (W and E) | 46.0 × 13.8 × 8.0 | 10 × 3 rib-vault |
| Ward 2 | 41.4 × 13.8 × 8.0 / 9.5 / 11.92 | 9 trusses (or rib-vault if Gothic) |
| Ward 3 | 32.2–41.4 × 13.8 × 8.0 | 7 or 9 trusses |
| Benefactor's Study | 13.8 × 13.8 × 5.43 | 3 × 3 coffered panels |
| Cold Storage, Morgue (crypt) | 18.4 × 18.4, 36.8 × 25.3, h 6.0 | barrel-vault, springing 3.7 |

Not a multiple of 4.6: the **Isolation wards (57.5 long)** and the **Spine passages (43.7)** leave a 2.3 m half-bay; the
service passages (4.0 wide) and the Morgue Tunnel (14.9) need the barrel vault stretched about 13 %. Allow a **half-bay
filler** and a Blender stretch of up to ±15 % on these; nothing else needs a new part.

## 4. Gaps found while measuring (decisions needed)
1. **There is no Gothic pier.** The rib-vault bay needs something to stand on: a pier or wall respond, 5.4 m tall in the
   8 m wards and 9.3 m tall in the 11.92 m halls. The 13 parts have only the Renaissance column (3.7 m). **Proposal:** add
   part 14, a plain square Gothic pier with a capital (0.9 × 0.9, heights by stretch), plus the conditional pilaster.
2. **Dome footprint.** A 27.6 m dome covers about a third of the arena; the rest is flat at 12.0 m. If you want the dome to
   cover the whole 50.6 × 46.0 room it becomes an elliptical dome and the numbers change (BRAV-154).
3. **The 2.5 m lane and the loggia columns:** the loggia is 4.6 m deep with columns on a 4.6 m pitch; keep the columns on
   the outer line, the lane stays behind them.

## 5. Check of the approved designs against this sheet
Measured from the images' outlines (the 45° view shortens widths, so ±20 % is normal):
| Part | Image H/W | Expected from the sheet | Result |
|---|---|---|---|
| 06 Pointed doorway arch | 1.54 | ≈ 1.0 | **too tall and narrow, redraw** |
| 09 Renaissance column | 2.98 | ≈ 2.4 | slimmer than the sheet by about 24 %: borderline, redraw with the sheet |
| 10 Lancet window | 2.15 | ≈ 2.15 | OK |
| 08 Round arcade arch | 0.62 | ≈ 0.61 | OK |
| 07 Pointed arcade bay | 1.58 | ≈ 1.5 | OK (but it still reads as a copy of 06: it needs separate piers with capitals) |
| 11 Balustrade run | 0.73 | ≈ 0.63 | OK |
| 12 End post | 1.80 | ≈ 2.0 | OK |

The ceilings (01–05) and the stair (13) can't be judged from a bounding box: Başak checks them by eye against §2.
