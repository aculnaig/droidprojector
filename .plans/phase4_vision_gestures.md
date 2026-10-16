# Phase 4: Vision & Gesture Tracking Pipeline

## Objective
Implement CameraX with front/back camera switching and integrate Google MediaPipe Tasks Vision (`HandLandmarker`) to translate real-time hand movements into 3D holographic manipulation.

---

## Tasks & Steps

### 4.1 CameraX Capture Pipeline
- [ ] Implement `CameraManager`:
  - Support toggle between `CameraSelector.DEFAULT_FRONT_CAMERA` and `DEFAULT_BACK_CAMERA`.
  - Configure `Preview` for on-phone visual feedback.
  - Configure `ImageAnalysis` with `STRATEGY_KEEP_ONLY_LATEST` to avoid processing backlog.
  - Convert `ImageProxy` to `MPImage` (MediaPipe image format).

### 4.2 MediaPipe Hand Landmarker Integration
- [ ] Bundle `hand_landmarker.task` in `app/src/main/assets/`.
- [ ] Initialize `HandLandmarker` in `RUNNING_MODE_LIVE_STREAM` with GPU acceleration.
- [ ] Extract 21 hand landmarks on background thread with timestamp tracking.

### 4.3 Gesture Recognition & State Machine
- [ ] Implement `GestureDetector`:
  - **Pinch**: Compute distance between landmark 4 (Thumb Tip) and 8 (Index Tip). When pinched, track distance deltas for scaling.
  - **Palm Drag**: Compute palm centroid from landmarks 0 (Wrist), 5 (Index MCP), 17 (Pinky MCP). Track $(\Delta x, \Delta y)$ for rotation.
  - **Swipe**: Detect rapid lateral displacement of hand centroid to cycle models.
- [ ] Coordinate conversion:
  - Mirror X-axis for front-facing camera.
  - Apply Exponential Moving Average (EMA) smoothing to filter out jitters.

### 4.4 Phone UI Landmark Overlay
- [ ] Create a lightweight Canvas overlay in Jetpack Compose showing detected hand skeleton and active gesture badges (e.g. "PINCH ZOOM", "ROTATING", "SWIPE").

---

## Verification Criteria
- Hand skeleton is detected and tracked smoothly over camera preview (> 30 FPS).
- Moving open hand rotates the 3D model in real time.
- Pinching index finger and thumb scales the model up and down with low latency and without stutter.
- Switching between front and back camera works instantly without breaking the tracking loop.
