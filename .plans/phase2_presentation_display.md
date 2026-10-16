# Phase 2: Multi-Display Presentation & Dual-Mode Fallback

## Objective
Implement Android's `Presentation` API to output fullscreen 3D visuals to secondary displays (USB-C DisplayPort, HDMI, or Wireless Miracast) while managing a seamless fallback preview when no external display is attached.

---

## Tasks & Steps

### 2.1 Display Listener & Discovery Service
- [ ] Create `DisplayManagerService`:
  - Register `DisplayManager.DisplayListener` to detect dynamic connection and disconnection of external displays.
  - Query `displayManager.getDisplays(DisplayManager.DISPLAY_CATEGORY_PRESENTATION)`.
  - Expose a reactive `StateFlow<DisplayTarget>` to the UI and Presentation layer.

### 2.2 `HologramPresentation` Implementation
- [ ] Subclass `android.app.Presentation`:
  - Configure `FLAG_FULLSCREEN` and `FLAG_KEEP_SCREEN_ON`.
  - Set window background to `#000000` (pure black).
  - Embed a dedicated `SurfaceView` or Filament `SceneView` container.
  - Bind lifecycle to external display events (handle unexpected unplug gracefully without app crash).

### 2.3 Dual-Mode UI Fallback
- [ ] Implement auto-switching logic:
  - **Connected State**: External display renders the full-screen 3D hologram; phone screen becomes an interactive control deck showing the camera tracker and touch controls.
  - **Disconnected State**: Phone screen renders a split-screen or floating preview container housing the 3D hologram directly alongside the camera view.

---

## Verification Criteria
- Enable Developer Options: `Simulate secondary displays > 1080p`.
- Verify that `HologramPresentation` automatically pops up on the simulated display showing a black background and test graphics.
- Disconnect simulated display; verify app cleanly falls back to rendering on the phone screen without throwing `BadTokenException` or crashing.
