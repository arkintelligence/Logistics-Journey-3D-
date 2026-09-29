# ARK — Journey experience: build notes

The prototype (`Logistics Journey.dc.html`) is structured so it ports cleanly to **React (web) · Node (API) · Flutter (iOS/Android)**.

## The one rule that makes it portable
Everything is driven by a single number: **`progress` 0 → 1**. The truck follows one spline route (`path.at(s)` → position + heading), so camera, container and route line all key off distance along it.
Scroll (web), autoplay, or a native gesture (mobile) only has to produce `progress`; every scene, camera move, label and status is a pure function of it.

| Range | Scene |
|---|---|
| 0.00–0.16 | Globe · network · dive to Singapore |
| 0.16–0.36 | Inland depot · RTG lifts container onto truck |
| 0.36–0.50 | Road · depot → port road → Gate 02 (stop, barrier lifts) → berth. Gets 4 extra screens of scroll (scroll→story warp, `_warp()`) |
| 0.50–0.65 | STS crane loads ship |
| 0.65–0.80 | Ship sails |
| 0.80–0.93 | Air freighter overflight |
| 0.93–1.00 | Outro |

## React (frontend)
- `src/scene/` — plain three.js (r128+) modules lifted from the prototype's logic class: `buildGlobe()`, `buildPort()`, `updateGlobe(p,t)`, `updatePort(p,t)`, `cameraShots`. No React inside — keeps it reusable.
- `<JourneyCanvas progress={p} />` — mounts the renderer in `useEffect`, disposes on unmount. (Optional later: port to `@react-three/fiber`; the update functions map 1:1 to `useFrame`.)
- `useScrollProgress()` — scroll → eased `progress` (same damping as prototype, `smoothing` ≈ 0.075).
- Overlay UI (header, chapter copy, rail, HUD, outro) stays as normal React components reading `progress`.
- Assets: `/public/assets/ark-logo.png`; swap-in GLB models later via `GLTFLoader` — each builder returns a `THREE.Group`, so a model can replace a procedural one without touching animation code.

## Photoreal model slot (already wired in the prototype)
Turn on the **useModelFiles** tweak and drop production models here — they replace the procedural ones automatically, animation untouched:
- `assets/models/truck.glb` — tractor + skeletal trailer, no container. Forward = +X, Y up. Auto-scaled to 16.4 m long; trailer deck ≈ 1.43 m.
- `assets/models/ship.glb` — hull + superstructure (containers are drawn separately). Forward = +X. Auto-scaled to 76 m; keel at y −6.
- `assets/models/plane.glb` — freighter, forward = +X. Auto-scaled to 40 m.
Wheel spin is lost on a swapped truck unless the wheels are named nodes (easy follow-up).

## Node (backend)
- `GET /api/shipments/:id` → `{ id, mode, status, eta, legs:[{mode, from, to, progress}] }` feeds the container tag + HUD (today hard-coded: "ARKU 402117", status strings in `_status()`).
- `GET /api/network` → ports + lanes for the globe (today the `PORTS` / `ARCS` constants).
- Same endpoints serve the Flutter app.

## Flutter (iOS / Android)
Flutter can't run three.js natively. Recommended, in order:
1. **WebView** (`webview_flutter`) hosting the React build of the journey page, with `progress` pushed in via a JS channel — identical visuals, one codebase.
2. **Pre-rendered video** of the same timeline, scrubbed by scroll (`video_player` seekTo) — lightest on low-end devices.
3. Native 3D (`flutter_scene` / Filament) — only if the app needs interactive 3D beyond this sequence.

## Performance budget kept in the prototype
Instanced containers (1 draw call per stack set), one shadow-casting light, bloom at half-float, DPR capped at 1.6, renderer shared by both scenes.
