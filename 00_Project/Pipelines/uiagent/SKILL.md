---
name: uiagent
description: UIAgent — design, build, and extend Bravehood's game UI (HUD, menus, panels, bars, slots) through Unity MCP using the Game fantasy GUI kit. Use whenever the user asks about UI, HUD, health/stamina bars, boss bars, minimap, action bar, inventory panels, menus, fonts, or wants UI elements built or restyled. Knows the asset map, foldering conventions, the element library, and the souls-like reference layout.
---

# UIAgent — Bravehood game UI

Bravehood's UI target: **cinematic souls-like dungeon HUD** — dark translucent panels, COOL SILVER ornamental frames (house theme since 2026-07-25: Ethereal Silver for meters/map, Pastel Silver for slots + the Brawler PASTEL_SILVER TMP font), red health / green stamina as the only saturated colors, silver serif titles. Golden Beam was the original theme — user rejected it as 'too golden'; theme swap = folder-name substitution on sprite paths (all themes share identical canvases, so measured fill anchors carry over unchanged). Reference layout (user-approved mockup): top-left portrait + health/stamina + buffs · top-center boss bar · top-right minimap compass · left quest & party panels · right inventory/stat panels + keybind hints · bottom-center action bar (hex + square slots) with red/green bars beneath.

## Asset map (never duplicate assets — reference in place)

**⚠ Use the KIT sprites, not `Assets/Sprites/ExtractedUI/`** — the extracted sprites have fringed edges ("weird outlines", user-rejected). ExtractedUI remains only as an icon source (potion/skill/status icons) until kit equivalents are picked.

**Kit assembly rules (learned, HUD v2):**
- Health-meter naming is inverted: `ProgressBarImages/ProgressBarN.png` = ornate FRAME with dark tray; `ProgressBarBorders/BorderN.png` = outline only; `Fillers/FillerN.png` = glossy fill capsules shared across themes (1=green, 5=red). Frames pair by row number with the preview sheet (`Preview/HealthMeters.png`).
- **Z-order: Frame at sibling 0 (bottom), Ghost, then Fill on top** — frame trays are opaque and swallow fills placed beneath them.
- **Bars v4 = SLOT DNA (2026-07-25, user-approved consistency):** bars are built from the item slots' exact ingredients — NO health-meter kit frames at all (v2 ornate + v3 Border1 both rejected). Recipe per bar: `Cell` (sprite-null Image, UI_FrostedBlur, slot tint (0.58,0.62,0.7,1), inset 2) → `Ghost` + `Fill` flat (`Assets/Sprites/ExtractedUI/white_flat.png` — a pure 8×8 white square; NEVER the built-in UISprite: its rounded corners stretch into lens-shaped blobs under Filled mode, user-rejected. Filled/Horizontal, inset 5,4) → `Border` = **the slots' own `01-Slots Cube/03-Pastel Silver/Borders/Border2.png`**, SLICED (spriteBorder 72 uniform, `pixelsPerUnitMultiplier = 72/(0.4*height)`), slot tint (0.82,0.86,0.9,0.9). Sizes: health 400×26, stamina 330×20, boss 840×30. Colors: health (0.82,0.18,0.15), ghost (1,0.72,0.62,0.8); stamina (0.36,0.78,0.32), ghost (1,1,1,0.45). Portrait = 84 px diamond, identical slot recipe (45° + counter-rotated IconPivot, empty Icon awaiting character art). The unslotted Border2 stays Simple on the diamonds — importer border doesn't affect them. PIL tray-measuring method remains for fitting any future kit frame.
- **Custom fill shader v2 — LIQUID IN GLASS (unique-to-Bravehood, 2026-07-25):** `Assets/Shaders/BravehoodUIBarFill.shader` → `Bravehood/UI/BarFill`. Layers: cylindrical tube shading (_GlassCurve 0.45 — bright core, dark walls), broad top reflection (_GlassShine 0.22) + sharp specular line (_SpecStrength 0.45 @ _SpecPos 0.82), **procedural dust motes drifting very slowly inside the liquid** (two hash-grid layers w/ twinkle: _DustStrength 0.28, _DustDensityX/Y 34/3, _DustSize 0.10, _DustSpeed 0.018), slow diagonal sheen, living edge glow (`UIBar` mirrors fillAmount→_FillEdge on the per-instance material). Materials `UI_BarFill_{Health,Stamina,Boss}.mat` on Fill images only; ghosts flat. Full UI boilerplate (stencil/RectMask2D/alpha clip). v1 (plain gradient) rejected as not-AAA.
- Slots + MapGame: `Cells/itemsN.png` (dark bg) and `Borders/BorderN.png` share one canvas — overlay at the identical rect, cell under border. MapGame `Border1` = the N/S/E/W compass ring (minimap frame).
- Always preserve native aspect (get px dims via `sips`); never stretch frames.
- **Frosted blur** — full recipe in [modules/item-slots-and-blur.md](modules/item-slots-and-blur.md) (Recipe 2: renderer-feature attach code, UI_FrostedBlur.mat usage, mobile caveat).
- **TMP:** TMP Essential Resources are imported now (were missing — kit font materials showed `Hidden/InternalErrorShader`, text invisible; import via `TMPro.TMP_PackageResourceImporter.ImportResources(true,false,false)` in edit mode if a fresh clone regresses).

**`Assets/Game fantasy GUI kit/Game fantasy GUI kit/ResourceData/`** — the full kit:
- `Sprites/06-Health Meter/` — bar backgrounds + `Fillers/`, themed variants (03-Ethereal Silver ← house theme, 01-Golden Beam, 02-Pastel Silver)
- `Sprites/01-Slots Cube/`, `02-Slots Circle/` — slot frames in 10 color themes (Golden Beam = default)
- `Sprites/03-Slots Special/`, `05-Slots-Controls/` — special/control slot frames
- `Sprites/04-MapGame/` — minimap/compass frames (themed)
- `Sprites/08-Control Xbox PS PC/` — input glyphs: `pc/`, `playstation/`, `xbox/` (roll = Shift / Square / X)
- `Sprites/07-Game Buttons`, `10-Menu Elements`, `11-Modal Window`, `12-Tab Bar…` — menus/popups
- `Font/` — **`Brawler-Regular SDF   GOLDEN_BEAM.asset`** (TMP, gold serif — titles/boss names; note the triple space, load via `AssetDatabase.FindAssets("Brawler-Regular SDF t:TMP_FontAsset")` + path.Contains("GOLDEN")), Bona Nova family for body text.
- `Prefabs/Slots/`, `Scenes/` — kit demo content (reference only; don't instantiate directly).

## Foldering (strict)

- Scripts → `Assets/Core/Scripts/UI/` — namespace `Bravehood.UI`, PascalCase public fields, `[Header]`/`[Tooltip]` (house style).
- Prefabs → `Assets/Core/Prefabs/UI/` (e.g. `HUD_Player.prefab`).
- New extracted/purpose sprites → `Assets/Sprites/ExtractedUI/`.
- Never place project files inside the GUI-kit folder; never copy kit sprites out of it.

## Element library (`Assets/Core/Scripts/UI/`)

| Element | Script | Notes |
|---|---|---|
| Resource bar | `UIBar.cs` | Filled Image + smooth lerp + souls **ghost fill** (lingers 0.4 s after damage, then drains). `SetValue(cur,max)` / `SetInstant`. |
| Player HUD binder | `PlayerHUD.cs` | On the HUD canvas; auto-resolves Player; binds `Health.OnHealthChanged` + `Stamina.OnStaminaChanged` to bars. |
| Boss bar | `BossBarUI.cs` | CanvasGroup hidden by default; `Show(Health, name)` / `Hide()`; auto-hides on `OnDeath`. |
| Item slot | `ItemSlotUI.cs` | `SetItem(sprite,count)` / `SetCount` / `Clear`; frame may be rotated — icon+count live under a counter-rotated `IconPivot`. |
| Tooltip | `UITooltip.cs` + `UITooltipTrigger.cs` | Singleton frosted panel follows mouse (Input System `Mouse.current`), clamps to canvas, auto-sizes via layout+fitter (Cell/Border children need `LayoutElement.ignoreLayout`). Put a trigger on any raycastable graphic. |
| Main menu hub | `MainMenuController.cs` | PLAY landing screen brain: tabs, back-stack philosophy, focus keeping, PlayerPrefs menu memory, contextual prompts. F9/F10/F11 preview the run-complete / death / dismiss states. |
| Matchmaking CTA | `MatchmakingButtonUI.cs` | ENTER DUNGEON transforms in place: Idle -> SEARCHING (timer + sweep + Cancel) -> DUNGEON FOUND. `SimulatedSearchSeconds` stands in for netcode. |
| Dungeon/floor card | `DungeonSelectorUI.cs` | Floor cycling with wrap, cross-faded key art, threat + recommended gear, diamond page dots. |
| Segmented choice | `MenuSegmentedControl.cs` | PvPvE\|PvE, SOLO\|DUO\|TRIO. Selected segment marked by fill AND a gold diamond - never colour alone. |
| Controller footer | `MenuPromptBar.cs` + `MenuInputGlyphs.cs` | Contextual, <=5 prompts, whole glyph vocabulary swaps on device change. Never prints "A / Cross". |
| Focus dressing | `MenuFocusable.cs` | Border brightens, cell lifts, 1.5% scale, 100 ms. Explicit `Navigation.Mode.Explicit` graph wired by the builder. |
| Run report | `RunReportOverlay.cs` | Return-from-run and death states; takes over the left column instead of being its own screen. |
| Confirm modal | `MenuModal.cs` | Destructive decisions only; defaults focus to the safe option; raises `StateChanged` so the footer re-prompts. |
| Quest / season | `QuestEntryUI.cs`, `SeasonTrackUI.cs` | Animated post-run progress (~1.2 s, non-blocking). |
| Loadout strip | `LoadoutStripUI.cs` | Five real `EquipmentSlot`s; gear tier = average item tier; empty-critical-slot warning, never a blocking dialog. |
| Character stage | `CharacterStageUI.cs` | Right-stick / drag yaw with spring-back and idle sway on the real player rig. |
| Weapon icons | `WeaponIconCatalog.cs` (`Bravehood.Items`) | 34 authored icons in `Assets/Sprites/Items/Weapons/` as `{Class}_T{n}_{Name}.png` (Knight/Assassin/Barbarian/Mage × T1-4) + `_manifest.txt` holding the artist's exact display names. Catalog asset `Assets/Core/Data/Items/WeaponIconCatalog.asset`, rebuilt by **Bravehood ▸ UI ▸ Rebuild Weapon Icon Catalog** (also enforces Sprite/no-mip/max-256 import). Tier maps straight onto rarity (T1 Common → T4 Legendary). `EquipmentItem` now has an `icon` field. ⚠ This art fills only ~46%×86% of its own frame — draw it with `WeaponIconInset` (0.02), NOT the 0.14 default, or it reads tiny. |
| Inventory slot | `InventorySlotUI.cs` | Icon+count+rarity border+selection glow+optional ghost pictogram. `SetItem(sprite,count,ItemRarity)` / `SetSelected` / `Clear`; hover & gamepad select light the glow. `ItemRarity.{Common,Fine,Rare,Legendary}` → muted palette via `InventorySlotUI.RarityColor()` (silver / steel blue / violet / ember — never full-sat, gameplay red/green stay loudest). |

**Element library demo (2026-08-25):** `Assets/Scenes/UI_ElementLibrary.unity`, rebuilt from scratch by menu **Bravehood ▸ UI ▸ Build UI Library Demo** (`Assets/Core/Scripts/Editor/UILibraryBuilder.cs` — edit that, not the scene). 17 prefabs in `Assets/Core/Prefabs/UI/Library/`: UI_Panel, UI_Button_{Primary,Danger}, UI_Toggle, UI_Slider, UI_Dropdown, UI_InputField, UI_TabBar (now takes any tab count), UI_ScrollView, UI_Tooltip, plus the inventory set — UI_ItemSlot, UI_EquipSlot (ghost pictogram), UI_InventoryGrid (6-col GridLayoutGroup scroll), UI_ItemCard (rarity name + stat rows w/ compare deltas + flavor + Equip/Drop), UI_ContextMenu, UI_WeightBar, UI_CurrencyChip (kit Pastel Silver `icon.png` sigil). **Inventory sheet:** `Assets/Scenes/UI_InventoryLibrary.unity` via **Bravehood ▸ UI ▸ Build Inventory Library Demo** (`UILibraryBuilder.Inventory.cs`, same partial class) — driven by the real weapon catalog when it exists (grid, rarity ramp, paper-doll, item card), falling back to ExtractedUI placeholders. **Armory sheet:** `Assets/Scenes/UI_ArmoryLibrary.unity` via **Build Armory Icon Preview** (`UILibraryBuilder.Armory.cs`) — class×tier matrix of every authored icon + item card + loadout strip. `MakeItemSlot` takes a trailing `iconInset` (default 0.14 full-bleed, 0.02 for weapon art). Equipment ghost pictograms are generated PNGs `ghost_{helm,torso,legs,sword,shield,ring}.png` in ExtractedUI (PIL, filled silhouettes, drawn at GhostTint alpha 0.20). All slot-DNA: frosted Cell + sliced Border2 (`pixelsPerUnitMultiplier = 72/thickness`; ~10 px buttons/inputs, 16 px panels, 6 px small controls). Panel divider = generated `Assets/Sprites/ExtractedUI/divider_soft.png` (512×8: gaussian core σ0.7 @0.55 + halo σ2.0 @0.12; horizontally: tiny 1.5% ease-in, full to 25%, then long smoothstep dissolve to 0 at the right — LEFT-ANCHORED because the UI reads left→right, user-directed) drawn w−48 × 7, tint (0.85,0.89,0.94,0.55) — a gentle luminous whisper (user-approved). Rejected on the way: ornate kit `line.png` (arrow finials, busy), plain 2 px hairline (raw pixel), big/bright symmetric glow ("so big"). Dropdown arrow = kit `10-Menu Elements/02-Pastel Silver/arrow.png` rotated −90°. Toggles/handles = rotated-45° diamond cells. Learned traps: kit sprites outside slot/meter folders import as Default — builder's `LoadSprite` converts importer to Sprite type; `pc/Butt.png` has a baked "W" (not blank — use Mouse1/shift/space/Esc glyphs); BonaNova-Italic has no em-dash glyph (use ":"); Unity Toggle drives its check via canvasRenderer alpha — never SetActive(false) the check graphic; fresh `RectTransform`s default sizeDelta (100,100) — zero it on stretch-anchored scroll/grid content or it overhangs the viewport; when queueing prefabs, save children BEFORE any ancestor panel ("Can't save part of a Prefab instance"); **an `Image` with a null sprite still draws a plain filled rect** — never leave one enabled as a placeholder (it put a grey square in every empty inventory slot), create the object only when it has a sprite; **the scene builders refuse to run in play mode** (`BlockedByPlayMode`) — before that guard existed a play-mode run saved the open scene then threw half-way, leaving a stale scene that looked freshly rebuilt.

**QuickSlots (DS cross, bottom-left) — full recipe in [modules/item-slots-and-blur.md](modules/item-slots-and-blur.md)** (Recipe 1: diamond geometry, flat frosted styling, ItemSlotUI wiring, slot semantics).

Live prefab: **`Assets/Core/Prefabs/UI/HUD_Player.prefab`** (all Ethereal/Pastel Silver) — Canvas (ScreenSpaceOverlay, ScaleWithScreenSize 1920×1080, match 0.5) → `TopLeft` (laurel PortraitFrame 120×107, HealthBar 400×43.8 [ProgressBar5+Filler5], StaminaBar 330×29.7 [ProgressBar2+Filler1]; structure = Frame(bottom) → Ghost → Fill with measured anchors) · `BossBar` 840×42 top-center [ProgressBar3+Filler5], silver Brawler name label · `QuickSlots` DS diamond cross bottom-left · `Minimap` compass ring 200×197 top-right. Instance lives in the DungeonA scene.

Bar canon: fills from kit `Fillers/` — health/boss `Filler5` (red), stamina `Filler1` (green); ghost = same filler tinted pale (health ghost `(1,0.75,0.65,0.75)`). Text tint `(0.88,0.92,0.96)`, dark outline.

## Modules (built one by one — route by request)

| Request smells like | Load |
|---|---|
| Quick item slots / diamond slots / assigning items to HUD slots · frosted/blurred panel backgrounds | [modules/item-slots-and-blur.md](modules/item-slots-and-blur.md) |

(More modules will be added as elements are approved: bars, boss bar, minimap, menus…)

## Main menu (2026-08-25)

`Assets/Scenes/MainMenu.unity`, rebuilt from scratch by **Bravehood > UI > Build Main Menu** (builder partials `UILibraryBuilder.MainMenu.cs` / `.MainMenuUI` / `.MainMenuPanels` / `.MainMenuRight` / `.MainMenuParts` - edit the builder, not the scene). Canvas banked as `Assets/Core/Prefabs/UI/MainMenu_Canvas.prefab`; runtime scripts in `Assets/Core/Scripts/UI/MainMenu/`.

**Shape:** PLAY is the landing screen (never a lobby in front of it). Three columns around a live character - left = run setup (dungeon/floor card, mode, team size, ENTER DUNGEON, party), centre = 3D character + quick loadout strip + gear tier, right = quest log / season / news, plus a persistent top nav and a contextual controller footer. Switching tabs only rearranges panels: character, nav and footer never unload.

**Layout grid (1920x1080 ref):** `SafeArea` inset 48 x 30 (action-safe); nav 68 tall; content 92..946; columns 445 wide at x 0 and x 1379; footer 56. CanvasScaler matches on **height** so ultrawide widens the character stage rather than stretching the panels.

**Stage:** `_Stage` holds a 30-degree camera at z 4.25 (a ~1.6 m character fills ~72% of frame height), the real `Player.prefab` stripped to Transform/Animator/renderers, four point lights (key/rim/rim2/fill/bounce), a floor light-pool quad and a defocused dungeon plate.

**Menu art:** eight RenderTexture captures of the real DungeonA floor, graded in PIL (green cast tamed, split-toned, vignetted), in `Assets/Sprites/UI/MainMenu/` - six floor cards, a stage backdrop and a news banner, plus generated `icon_*.png` pictograms and `scrim_h/v` + `stage_pool` gradients.

**Colour discipline:** warm gold is spent ONLY on ENTER DUNGEON, the selected-segment diamond, the active tab marker and the next season reward. Everything else stays house silver.

**Traps learned here:**
- **Private fields do not survive the play-mode boundary.** The builder seeds `QuestEntryUI`/`SeasonTrackUI`/`LoadoutStripUI` in edit mode; plain private fields reset to 0 and printed "2 / 0" and "920 / 0 XP". They are `[SerializeField, HideInInspector]` now, with an `Awake` that re-syncs from the fill amount.
- **The gameplay Animator needs its whole rig to stay in Idle.** With `CharacterAnimator` and the ground check stripped, `Knight_Controller` fell through to `Knight_InAir` and T-posed. The menu mannequin gets a one-state `Assets/Core/Animations/MenuIdle.controller` instead.
- **Stripping components needs repeated passes** - `DestroyImmediate` refuses while a dependent lives (StatusEffects->Health, BodyPartHurtbox->BoxCollider), so loop until a pass removes nothing.
- **Set `pivot` before `localEulerAngles`.** `TopLeft` leaves the pivot in the corner, so a counter-rotated label (the season level numeral) spins out of its diamond.
- **Capturing dungeon art needs the culling driven manually.** `DungeonCullingSystem.CullFromPoint(pos)` before each `Camera.Render()`; without it every shot but the first renders skybox through missing geometry. `ForceShow()` on every `RoomCullingCell` afterwards restores the scene. MCP `manage_camera screenshot` is capped at game-view size - render to a `RenderTexture` for anything larger.
- **The kit menu chevron (`10-Menu Elements/.../arrow.png`) already points RIGHT** - rotate 180 for the previous arrow, 0 for next.
- TMP auto-size (min 20 / max 29, wrapping off) on the CTA label keeps "SEARCHING FOR DUNGEON" on one line in a button sized for "ENTER DUNGEON".

**Deliberately not built:** VENDORS tab (spec's recommended hierarchy is 6 tabs; the `MenuTab` enum already has it), authored armour icons, real matchmaking/scene load, menu SFX clips.

## Build workflow (Unity MCP)

1. `execute_code` (codedom, **C#6**: no local funcs — `System.Func` lambdas; fully-qualify) builds hierarchies; `SaveAsPrefabAssetAndConnect` to persist; `MarkSceneDirty` + save scene.
2. New sprites: verify `TextureImporter.textureType == Sprite` before use.
3. **Verify visually + functionally**: enter play (`Application.runInBackground = true` — editor is usually unfocused during MCP work), drive real gameplay (damage via `Health.TakeDamage`, stamina via `Stamina.TryConsume`, roll via `SetInputs`), then `manage_camera` `screenshot` `include_image=true` (ScreenCapture path captures overlay canvases; specifying a camera does NOT). Read `UIBar.Fill.fillAmount` numerically as ground truth.
4. Debug overlays that also draw on screen: DungeonCulling HUD (top-left line) and `DamageDebugGUI` (right side) — dev-only, not part of the HUD.
5. After UI work: update `AI/TASKS.md`; keep this skill's element table current when adding elements.

## Not built yet (reference regions still open)

Minimap/compass (kit `04-MapGame`), quest tracker panel, party/enemy list, inventory & stat panels, buff/status icon row under the player bars (`status_icon_frame` + status icons), pause screen (the main menu now exists - see below) — though panels/buttons/tabs/tooltips/keybind chips now exist as library prefabs to compose them from.
