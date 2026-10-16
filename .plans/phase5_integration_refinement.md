# Phase 5: End-to-End Integration, Refinement & Testing

## Objective
Unify the Multi-Display Presentation, CameraX + MediaPipe Vision pipeline, and Filament 3D Engine into a cohesive, high-performance application with touch fallback controls, calibration settings, and comprehensive device testing.

---

## Tasks & Steps

### 5.1 End-to-End State Machine Integration
- [ ] Connect `GestureDetector` output streams to `HologramTransformState` in the Presentation/Fallback view.
- [ ] Implement dual-input prioritization:
  - Phone screen touch controls (drag to rotate, pinch to zoom) work simultaneously alongside hand gestures.
  - Active touch temporarily locks hand tracking to avoid conflict if both are used concurrently.

### 5.2 Performance & Memory Optimization
- [ ] Benchmark frame times:
  - Filament rendering target: 60 FPS on secondary display.
  - MediaPipe inference target: 30-60 FPS on background executor.
- [ ] Memory audit: Ensure texture and native C++ buffers in Filament and MediaPipe are recycled properly during rapid model switching.

### 5.3 On-Screen UI & Control Deck
- [ ] Jetpack Compose control deck on phone screen:
  - Model selector carousel (Earth, Droid, Heart, Custom file picker).
  - Shader styling buttons (Cyan Hologram, Neon Amber, Emerald, PBR Original).
  - Sensitivity sliders (Gesture rotation speed, pinch zoom factor, smoothing alpha).
  - Display status badge ("Connected: EPSON Projector 1080p" vs. "Standalone Preview Mode").
  - Camera switch toggle (Front ⟷ Rear).

### 5.4 Testing Matrix & Edge Case Handling
- [ ] Test rapid plugging / unplugging of external display (HDMI / USB-C / Simulated).
- [ ] Test app backgrounding / foregrounding (ensure CameraX and Filament pause/resume gracefully without memory leaks).
- [ ] Low-light hand tracking edge cases (display user warning if hand confidence is low).

---

## Verification Criteria
- Complete touchless end-to-end loop: Hand moves in front of phone camera -> 3D model rotates on the external projected wall in real time.
- Zero crashes on display hot-plugging.
- Memory usage remains steady under 250 MB with no leaks.
