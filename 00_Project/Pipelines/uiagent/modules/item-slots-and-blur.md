# Module: Quick Item Slots (DS diamonds) + Frosted Blur

The first two proven UI recipes. Each is complete and replayable in any scene via Unity MCP
`execute_code` (codedom C#6 — `System.Func` lambdas, fully-qualified names). Screenshot-verify
after building (see SKILL.md workflow).

---

## Recipe 1 — DS quick item slots (bottom-left diamond cross)

**Look:** flat/modern (user-approved 2026-07-25): translucent frosted diamonds, hairline silver
border, upright icons. NOT the textured cell + ornate border (rejected as heavy/busy).

**Hierarchy** (inside the HUD canvas):

```
QuickSlots (RectTransform, anchor/pivot bottom-left, pos (44,40), size 240×240)
└─ Slot_Up / Slot_Left / Slot_Right / Slot_Down       ← DS cross order
   each: 70×70, pivot center, localEulerAngles z=45   ← the diamond = rotated square
   ├─ Cell    (stretch-fill, offset ±3)  Image: sprite NULL → material UI_FrostedBlur,
   │          color (0.58,0.62,0.7,1) = tint over blur. Inset 3 px hides the square's
   │          sharp corners under the border's rounded hairline.
   ├─ IconPivot (center, 70×70, localEulerAngles z=−45) ← counter-rotation keeps content upright
   │  ├─ Icon  (center, 42×42) Image disabled until an item is set, preserveAspect
   │  └─ Count (anchor bottom-right, pos (6,−14), 46×26) TMP, Brawler PASTEL_SILVER,
   │           size 20, BottomRight, color (0.88,0.92,0.96)
   └─ Border  (stretch-fill) Image: kit `01-Slots Cube/03-Pastel Silver/Borders/Border2.png`
              (thinnest border in the kit — hairline + two tiny swirls), tint (0.82,0.86,0.9,0.9)
```

**Cross positions** (anchored, within the 240×240 root): Up (120,196) · Left (44,120) ·
Right (196,120) · Down (120,44).

**Component:** add `Bravehood.UI.ItemSlotUI` to each slot root; wire `Icon` + `CountLabel`.
API: `SetItem(Sprite, int)` / `SetCount(int)` / `Clear()`. Count label shows only when >1.
Gameplay assignment system does not exist yet — these are the UI endpoints.

**Slot semantics (souls):** Up = spell, Left = weapon L, Right = weapon R, Down = consumable.

**Why Border2:** flattest option. Border1 (edge-midpoint ornaments → diamond-tip crowns) is the
ornate alternative if a heavier look is ever wanted; Slots Special is rune-tech (off-theme);
Slots Circle rings are reserved for spell/magic slots.

---

## Recipe 2 — Frosted blur for UI panels (Unified Universal Blur)

**Package:** `com.unify.unified-universal-blur` (git, in manifest.json). Two parts must exist:

1. **Renderer feature** — `Unified.UniversalBlur.Runtime.UniversalBlurFeature` on the URP
   renderer asset. Currently attached to `Assets/Settings/PC_Renderer.asset` ONLY
   (⚠ `Mobile_Renderer.asset` not wired — mobile falls back to the plain tint; wiring it costs
   real GPU on mobile, decide deliberately). Attach programmatically:

   ```
   var f = ScriptableObject.CreateInstance<Unified.UniversalBlur.Runtime.UniversalBlurFeature>();
   f.name = "UniversalBlurFeature";
   AssetDatabase.AddObjectToAsset(f, rendererData); AssetDatabase.SaveAssets();
   TryGetGUIDAndLocalFileIdentifier(f, out guid, out localId);
   // SerializedObject on rendererData: append f to m_RendererFeatures
   // AND append localId to m_RendererFeatureMap, Apply + SaveAssets.
   ```

   Feature tunables: `Intensity` (0–1, default 1), `Iterations`, `Downsample`, `Scale`, `Offset`.

2. **UI material** — `Assets/Core/Materials/UI/UI_FrostedBlur.mat`, shader
   `Unify/UI/Tinted Blur`. Reusable for ANY panel: assign to a UI `Image` (sprite can be null);
   the Image's `color` becomes the tint multiplied over the blurred scene behind it.
   QuickSlots cells use (0.58,0.62,0.7,1); darker tints → darker glass, alpha stays 1.

**Verify:** blur renders only through the URP camera path in play mode — screenshot via
`manage_camera` (no `camera` param) and check a panel over a lit area shows smeared light,
not sharp geometry. If panels show plain grey/black: the renderer feature is missing/disabled
on the ACTIVE quality level's renderer.

**Intended reuse:** pause menu, inventory, tooltips, modal windows — one material assignment.
