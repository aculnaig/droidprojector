# Phase 3: 3D Filament Engine & Holographic Shaders

## Objective
Integrate Google Filament / Sceneview to render glTF/GLB models with pure black backgrounds, implement holographic visual effects (rim glow, scanlines), and support loading bundled as well as user-selected 3D models.

---

## Tasks & Steps

### 3.1 Sceneview & Filament Core Setup
- [ ] Initialize Filament `Engine`, `Renderer`, `Scene`, `Camera`, and `View`.
- [ ] Set clear color to `(0.0f, 0.0f, 0.0f, 1.0f)` (absolute pitch black).
- [ ] Configure lighting: Subtle directional key light + ambient fill to simulate holographic projection illumination.

### 3.2 glTF / GLB Asset Pipeline
- [ ] Bundle 3-4 open-source sample assets into `app/src/main/assets/models/`:
  - `earth_globe.glb`
  - `sci_fi_droid.glb`
  - `human_heart.glb`
- [ ] Implement asynchronous model loader (`ModelLoader`):
  - Read from `assets/` or local `content://` URIs via Android SAF (`ActivityResultContracts.OpenDocument`).
  - Automatically center and normalize model bounding box so all models fit uniformly in the holographic volume.

### 3.3 Holographic Material & Shader Effects
- [ ] Implement holographic material properties:
  - Custom rim-lighting shader / Fresnel glow on silhouette edges.
  - Animated horizontal scanline overlay.
  - Holographic tint selector (Cyan, Amber, Emerald, Original PBR).

### 3.4 Spatial Transformation Controller
- [ ] Expose transformation API:
  - `rotate(deltaYaw: Float, deltaPitch: Float, deltaRoll: Float)`
  - `scale(scaleMultiplier: Float)`
  - `translate(dx: Float, dy: Float, dz: Float)`
  - `resetTransform()`

---

## Verification Criteria
- 3D models load smoothly at 60 FPS.
- Black background emits zero light when tested with a connected display/projector.
- Holographic shader toggle switches between standard PBR and glowing holographic look.
