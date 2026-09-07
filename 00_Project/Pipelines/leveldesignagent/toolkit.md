# Toolkit — shared analysis code (Unity MCP `execute_code`, C# 6 / codedom)

All snippets verified against `DungeonA_Floor01_Blockout.unity` (2026-07-13). Rules: no `using` directives, fully-qualified types, `return` a string. When mutating the scene afterward, `UnityEditor.SceneManagement.EditorSceneManager.MarkSceneDirty(UnityEngine.SceneManagement.SceneManager.GetActiveScene())` + `SaveScene`. **Analysis snippets below are read-only** except where noted.

## HTTP hub call pattern (when stdio MCP is dead)

```bash
SID=$(curl -s -D - -o /dev/null -X POST http://127.0.0.1:8080/mcp \
  -H 'Content-Type: application/json' -H 'Accept: application/json, text/event-stream' \
  -d '{"jsonrpc":"2.0","id":1,"method":"initialize","params":{"protocolVersion":"2025-03-26","capabilities":{},"clientInfo":{"name":"cc","version":"1.0"}}}' \
  | grep -i 'mcp-session-id' | tr -d '\r' | awk '{print $2}')
curl -s -X POST http://127.0.0.1:8080/mcp -H 'Content-Type: application/json' \
  -H 'Accept: application/json, text/event-stream' -H "mcp-session-id: $SID" \
  -d '{"jsonrpc":"2.0","method":"notifications/initialized"}' > /dev/null
# then tools/call with {"name":"execute_code","arguments":{"action":"execute","code":"...","compiler":"codedom"}}
# responses are SSE "data:" lines; result JSON at result.content[0].text → data.result
```

## 1. Room boundary extractor (walls bounds + floor Y + doorways)

Doorway detection is a **sphere-cast sweep** (radius 0.5 ≈ character half-width) from the `Rooms/<name>` marker, at 3 probe heights (raised door thresholds — stairs up to corridors — are closed at eye height but open higher), with `Physics.queriesHitBackfaces = true` (blockout MeshColliders are one-sided; thin rays also leak through wall body seams — the sphere probe does not). A direction is a doorway when the first `_Blockout/*` hit lands >2 m beyond the room's wall-bounds edge; runs are merged and filtered to physical width ≥1.6 m.

Caveats (verified): interior Cover pillars split a wide nave into multiple reported lanes (W16 hub reports ~10 openings for 6 approaches); octagon corner bearings can over-report near diagonal neighbors. Treat output as candidate openings and cross-check against the plan doc's corridor list. `openAtH=1.0` ⇒ walk-in door; `openAtH≥3` ⇒ raised threshold (stairs) or high opening.

```csharp
string[] roomNames = new string[] { "W16_GrandInfirmary_Hub" }; // any Rooms/ name(s)
var sb = new System.Text.StringBuilder();
bool prevBF = UnityEngine.Physics.queriesHitBackfaces;
UnityEngine.Physics.queriesHitBackfaces = true;
float[] probeHeights = new float[] { 1.0f, 3.0f, 5.0f };
foreach (string roomName in roomNames) {
  var group = UnityEngine.GameObject.Find("_Blockout/Walls/" + roomName);
  var rends = group.GetComponentsInChildren<UnityEngine.Renderer>();
  var b = rends[0].bounds;
  foreach (var r in rends) b.Encapsulate(r.bounds);
  var marker = UnityEngine.GameObject.Find("Rooms/" + roomName);
  UnityEngine.Vector3 c = marker.transform.position;
  float floorY = -9999f;
  var floors = UnityEngine.GameObject.Find("_Blockout/Floors");
  foreach (var fr in floors.GetComponentsInChildren<UnityEngine.Renderer>()) {
    var fb = fr.bounds;
    if (c.x >= fb.min.x && c.x <= fb.max.x && c.z >= fb.min.z && c.z <= fb.max.z && fb.max.y <= c.y + 1.5f && fb.max.y > floorY) floorY = fb.max.y;
  }
  int N = 360; float maxDist = 120f; float probeR = 0.5f;
  float[] dist = new float[N]; float[] edge = new float[N]; bool[] open = new bool[N]; float[] openH = new float[N];
  for (int i = 0; i < N; i++) {
    float ang = i * UnityEngine.Mathf.PI * 2f / N;
    UnityEngine.Vector3 dir = new UnityEngine.Vector3(UnityEngine.Mathf.Sin(ang), 0f, UnityEngine.Mathf.Cos(ang));
    float bestAll = 0f; float bestH = -1f;
    foreach (float ph in probeHeights) {
      UnityEngine.Vector3 origin = new UnityEngine.Vector3(c.x, floorY + ph, c.z);
      var hits = UnityEngine.Physics.SphereCastAll(origin, probeR, dir, maxDist);
      float best = maxDist;
      foreach (var h in hits) {
        string path = ""; var tt = h.collider.transform; while (tt != null) { path = tt.name + "/" + path; tt = tt.parent; }
        if (!path.StartsWith("_Blockout/")) continue;   // props/players/markers don't count
        if (h.distance < best) best = h.distance;
      }
      if (best > bestAll) { bestAll = best; bestH = ph; }
    }
    UnityEngine.Vector3 o2 = new UnityEngine.Vector3(c.x, floorY, c.z);
    float tx = dir.x > 0.0001f ? (b.max.x - o2.x) / dir.x : (dir.x < -0.0001f ? (b.min.x - o2.x) / dir.x : 9999f);
    float tz = dir.z > 0.0001f ? (b.max.z - o2.z) / dir.z : (dir.z < -0.0001f ? (b.min.z - o2.z) / dir.z : 9999f);
    edge[i] = UnityEngine.Mathf.Min(tx, tz);
    dist[i] = bestAll; open[i] = bestAll > edge[i] + 2.0f; openH[i] = bestH;
  }
  for (int i = 0; i < N; i++) if (!open[i] && open[(i + N - 1) % N] && open[(i + 1) % N]) open[i] = true;
  sb.AppendLine("ROOM " + roomName + " outer=" + b.min.ToString("F1") + ".." + b.max.ToString("F1") + " floorY=" + floorY.ToString("F2"));
  int start0 = -1; for (int i = 0; i < N; i++) if (!open[i]) { start0 = i; break; }
  if (start0 < 0) { sb.AppendLine("  ALL OPEN?"); continue; }
  int runStart = -1;
  for (int k = 1; k <= N; k++) {
    int i = (start0 + k) % N;
    if (open[i] && runStart < 0) runStart = k;
    if ((!open[i] || k == N) && runStart >= 0) {
      int runEnd = k - 1; int span = runEnd - runStart + 1;
      int mid = (start0 + (runStart + runEnd) / 2) % N;
      float widthM = span * UnityEngine.Mathf.Deg2Rad * edge[mid];
      if (widthM >= 1.6f) {
        float ang = mid * UnityEngine.Mathf.PI * 2f / N;
        UnityEngine.Vector3 dir = new UnityEngine.Vector3(UnityEngine.Mathf.Sin(ang), 0f, UnityEngine.Mathf.Cos(ang));
        UnityEngine.Vector3 p = new UnityEngine.Vector3(c.x, floorY, c.z) + dir * edge[mid];
        sb.AppendLine("  DOOR bearing=" + mid + " width~" + widthM.ToString("F1") + "m pos=(" + p.x.ToString("F1") + ", " + p.z.ToString("F1") + ") openAtH=" + openH[mid].ToString("F1"));
      }
      runStart = -1;
    }
  }
}
UnityEngine.Physics.queriesHitBackfaces = prevBF;
return sb.ToString();
```

Sample verified output — `W25_SurgeryTheatre floorY=-7.67`: 5 doors (N,S,E×2,W) matching the plan's corridors; `W16` hub: naves N/S 6 m + side doors (lanes split by hub pillars).

## 2. Free-floor scanner (where can a prop go)

Grid-sample the interior; a cell is FREE if no renderer bounds (props in `Props_Dressing/<room>` or `_Blockout/Cover`) overlap it and it's ≥1 m inside the wall bounds. Run after the boundary extractor for `floorY` and outer bounds.

```csharp
string roomName = "W24_Store"; float floorY = -5.67f; // from extractor
var wg = UnityEngine.GameObject.Find("_Blockout/Walls/" + roomName);
var wr = wg.GetComponentsInChildren<UnityEngine.Renderer>();
var b = wr[0].bounds; foreach (var r in wr) b.Encapsulate(r.bounds);
var occupied = new System.Collections.Generic.List<UnityEngine.Bounds>();
var pdRoot = UnityEngine.GameObject.Find("Props_Dressing/" + roomName);
if (pdRoot != null) foreach (var r in pdRoot.GetComponentsInChildren<UnityEngine.Renderer>()) occupied.Add(r.bounds);
var cov = UnityEngine.GameObject.Find("_Blockout/Cover");
if (cov != null) foreach (var r in cov.GetComponentsInChildren<UnityEngine.Renderer>()) occupied.Add(r.bounds);
var sb = new System.Text.StringBuilder(); float cell = 1.0f; int free = 0, total = 0;
for (float x = b.min.x + 1f; x < b.max.x - 1f; x += cell) {
  for (float z = b.min.z + 1f; z < b.max.z - 1f; z += cell) {
    total++;
    var probe = new UnityEngine.Bounds(new UnityEngine.Vector3(x, floorY + 1f, z), new UnityEngine.Vector3(cell, 2f, cell));
    bool blocked = false;
    foreach (var ob in occupied) { if (ob.Intersects(probe)) { blocked = true; break; } }
    if (!blocked) free++;
  }
}
return roomName + ": " + free + "/" + total + " cells free (" + (100f * free / total).ToString("F0") + "%)";
// extend: collect free-cell coords into a list to propose placement spots
```

## 3. Player-camera POV renderer (verified)

Two steps: move `Main Camera` to the POV with `execute_code`, then `manage_camera` screenshot. **Record and restore the camera's original transform.** Eye height = `floorY + 1.75` (character 0.91 m + third-person camera sits above/behind; 1.75 above floor reproduces gameplay framing well — verified shot reads correctly).

```csharp
// step 1 — position (fill in from extractor output: doorway pos, room center)
var cam = UnityEngine.GameObject.Find("Main Camera");
var oldPos = cam.transform.position; var oldRot = cam.transform.eulerAngles;
cam.transform.position = new UnityEngine.Vector3(-8f, -1.67f + 1.75f, 76f);      // doorway, eye height
cam.transform.LookAt(new UnityEngine.Vector3(-8f, -0.5f, 62.8f));                 // room center / hero
return "old=" + oldPos.ToString("F2") + "|" + oldRot.ToString("F2");               // keep to restore!
```

```json
// step 2 — tools/call manage_camera
{"action":"screenshot","camera":"Main Camera","screenshot_file_name":"POV_<room>_<door>","include_image":true,"max_resolution":800}
```

Saves to `Assets/Screenshots/<name>.png` and returns inline base64. Read the image, judge against the art-polisher checklist, then restore the camera (step 1 in reverse). For an overview shot use `capture_source:"scene_view"` after `manage_scene {action:"scene_view_frame", target:"<room path>"}`.

## 4. Scene snapshot / diff (teaching loop, verified)

Snapshot one room's props as JSON (run via execute_code, save the returned string to `snapshots/<room>_<label>.json`). **Also capture per-prop `prefab` source (`PrefabUtility.GetCorrespondingObjectFromSource` + `AssetDatabase.GetAssetPath`), child `Light` components (color/intensity/range/shadows), and `ReflectionProbe`s (mode/size/boxProjection)** — lighting is part of the placement language (see `snapshots/W16_GrandInfirmary_Hub_teach_after.json` for the full format). And snapshot `_Blockout/Walls/<room>` too when the user may remodel walls (session 1 lesson: they did, and the before-walls state wasn't captured).

Basic transform-only version:

```csharp
var sb = new System.Text.StringBuilder();
var room = UnityEngine.GameObject.Find("Props_Dressing/W24_Store"); // or Gameplay/<group>
sb.Append("[");
bool first = true;
foreach (UnityEngine.Transform p in room.transform) {
  if (!first) sb.Append(",");
  first = false;
  sb.Append("{\"name\":\"" + p.name + "\",\"pos\":[" + p.position.x.ToString("F4") + "," + p.position.y.ToString("F4") + "," + p.position.z.ToString("F4") + "],\"rot\":[" + p.eulerAngles.x.ToString("F2") + "," + p.eulerAngles.y.ToString("F2") + "," + p.eulerAngles.z.ToString("F2") + "],\"scale\":[" + p.localScale.x.ToString("F4") + "," + p.localScale.y.ToString("F4") + "," + p.localScale.z.ToString("F4") + "]}");
}
sb.Append("]");
return sb.ToString();
```

Diff locally (python): match entries by `name` (fall back to index for renames); report **added / deleted / moved (Δpos) / rotated (Δyaw) / rescaled**. Then enrich each change with derived context for rule extraction:

- `d_center` = XZ distance to room center; `bearing_from_center`
- `d_wall` = distance to nearest wall-bounds edge; which side
- `d_door` = distance to nearest doorway (extractor) and offset from its axis
- `yaw_rel_center` / `yaw_rel_door` = facing relative to hero/door axis
- sibling spacing: distance to nearest same-prefab sibling; symmetry partner check (mirrored across a door axis within 0.3 m tolerance)

These derived numbers — not raw coordinates — are what become LEARNED.md rules.

## 5. Auto-orient + ground (project placement standard — mutating)

The standard: flattest face down, Y-yaw only, base on floor. Practical low-poly approximation — try the 6 axis-aligned orientations, keep the one with the largest footprint-to-height ratio, then drop to floor:

```csharp
var t = UnityEngine.GameObject.Find("Props_Dressing/W24_Store/Barrels_01").transform;
float floorY = -5.67f; // from extractor
UnityEngine.Vector3[] rots = new UnityEngine.Vector3[] {
  new UnityEngine.Vector3(0,0,0), new UnityEngine.Vector3(90,0,0), new UnityEngine.Vector3(-90,0,0),
  new UnityEngine.Vector3(0,0,90), new UnityEngine.Vector3(0,0,-90), new UnityEngine.Vector3(180,0,0) };
float bestScore = -1f; UnityEngine.Vector3 bestRot = UnityEngine.Vector3.zero;
foreach (var e in rots) {
  t.eulerAngles = e;
  var rr = t.GetComponentsInChildren<UnityEngine.Renderer>();
  var bb = rr[0].bounds; foreach (var r in rr) bb.Encapsulate(r.bounds);
  float score = (bb.size.x * bb.size.z) / UnityEngine.Mathf.Max(bb.size.y, 0.01f);
  if (score > bestScore) { bestScore = score; bestRot = e; }
}
t.eulerAngles = bestRot;                       // then add desired Y-yaw: t.Rotate(0, yaw, 0, UnityEngine.Space.World);
var rends2 = t.GetComponentsInChildren<UnityEngine.Renderer>();
var b2 = rends2[0].bounds; foreach (var r in rends2) b2.Encapsulate(r.bounds);
t.position = t.position + new UnityEngine.Vector3(0f, floorY - b2.min.y, 0f);   // ground
UnityEditor.SceneManagement.EditorSceneManager.MarkSceneDirty(UnityEngine.SceneManagement.SceneManager.GetActiveScene());
return t.name + " oriented " + bestRot.ToString() + " grounded at " + t.position.ToString("F2");
```

⚠ Fails on deliberately-flat props (curtains/flags/tapestries — they'll lie down); skip those. Known bad prefabs: `Crystal_02`, `Prefabs/Column_01`.

## 6. Light-probe validation (probes not inside objects + height-band coverage)

Checks every probe in a room's LightProbeGroup: flags probes embedded in geometry (`Physics.CheckSphere` r=0.15 against colliders — ignore trigger-only) and reports the height-band histogram so you can confirm floor/eye/mid/gallery/ceiling coverage (see art-polisher §Lighting).

```csharp
var room = UnityEngine.GameObject.Find("Props_Dressing/W16_GrandInfirmary_Hub");
var lpg = room.GetComponentInChildren<UnityEngine.LightProbeGroup>();
var sb = new System.Text.StringBuilder();
int inside = 0;
var bands = new System.Collections.Generic.Dictionary<int,int>();
foreach (var lp in lpg.probePositions) {
  UnityEngine.Vector3 w = lpg.transform.TransformPoint(lp);
  var cols = UnityEngine.Physics.OverlapSphere(w, 0.15f);
  bool bad = false;
  foreach (var c in cols) { if (!c.isTrigger) { bad = true; break; } }
  if (bad) { inside++; if (inside <= 15) sb.AppendLine("INSIDE GEOMETRY: " + w.ToString("F2")); }
  int band = UnityEngine.Mathf.FloorToInt(w.y + 5.67f); // height above W16 floor; adjust floorY per room
  if (!bands.ContainsKey(band)) bands[band] = 0;
  bands[band]++;
}
sb.AppendLine("total=" + lpg.probePositions.Length + " insideGeometry=" + inside);
foreach (var kv in bands) sb.AppendLine("  band " + kv.Key + "m: " + kv.Value + " probes");
return sb.ToString();
```

## 7. Elevation-transition scan (blockout validation)

Cluster all floor tiles (`_Blockout/Floors` + corridor floors) by top-Y; for each pair of XZ-adjacent tiles with ΔY > 0.5 m, require a stairs renderer bounds (`Stone_Stairs_*`) overlapping the shared edge; report unbridged pairs. DungeonA baseline: 54 tiles, 44 stairs, **0 unbridged**.
