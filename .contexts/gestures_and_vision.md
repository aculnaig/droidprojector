# Context: Computer Vision & Touchless Gesture Pipeline

## 1. Vision Engine: Google MediaPipe Tasks Vision

MediaPipe's `HandLandmarker` provides high-fidelity, real-time 3D hand tracking directly on mobile devices using GPU/NNAPI acceleration.

### Dependency Configuration
```kotlin
implementation("com.google.mediapipe:tasks-vision:0.10.14")
```

The model used is `hand_landmarker.task`, bundled into `app/src/main/assets/`.

---

## 2. Landmark Geometry & Gesture Definitions

MediaPipe tracks 21 3D landmarks (x, y, z normalized in `[0.0, 1.0]`):

```
       8   12  16  20    (Tips: Index, Middle, Ring, Pinky)
       │   │   │   │
       7   11  15  19
       │   │   │   │
   4   6   10  14  18
   │   │   │   │   │
   3   5───9───13──17    (Knuckles: MCPs)
   │    \     /    /
   2     \   /    /
   │      \ /    /
   1       0             (Wrist)
```

### Recognized Gestures & Spatial Mappings

1. **Pinch-to-Zoom (Scale)**:
   - Metric: Euclidean distance $d$ between Thumb Tip (`#4`) and Index Tip (`#8`).
   - If $d < d_{threshold}$, pinch is active. As distance changes relative to initial pinch, update 3D model scale:
     $$Scale_{t} = Scale_{t-1} \times \left(1.0 + \Delta d \cdot k_{scale}\right)$$

2. **Palm Drag (Rotation)**:
   - Metric: Centroid of Palm $C = \frac{Wrist_0 + IndexMCP_5 + PinkyMCP_{17}}{3}$.
   - Movement $\Delta C_x \rightarrow$ Yaw rotation (Y-axis).
   - Movement $\Delta C_y \rightarrow$ Pitch rotation (X-axis).

3. **Open Hand Hover (Idle / Tracking indicator)**:
   - Visual indicator on the phone screen highlights hand tracking status.

---

## 3. Coordinate Normalization & Filtering

- **Camera Facing Adjustment**: Front camera frames are mirrored; coordinates must be flipped along the X axis ($x_{adjusted} = 1.0 - x_{raw}$) so physical hand movements match visual expectation.
- **Low-Pass / EMA Smoothing**:
  $$\vec{P}_{smooth}(t) = \alpha \vec{P}_{raw}(t) + (1 - \alpha) \vec{P}_{smooth}(t-1)$$
  A smoothing factor $\alpha \approx 0.35$ eliminates sensor noise and trembling while preserving responsive feel.
