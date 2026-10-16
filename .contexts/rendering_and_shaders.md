# Context: 3D Graphics Engine & Holographic Shaders

## 1. Engine: Google Filament & Sceneview

Google Filament is an open-source, real-time physically based rendering (PBR) engine written in modern C++ with first-class Kotlin bindings for Android.

### Architectural Fit
- Ultra-lightweight footprint compared to heavyweight game engines.
- Native Android `SurfaceView` and `TextureView` support, making it seamless to render into secondary `Presentation` windows.
- First-class `.gltf` and `.glb` binary asset parsing via `gltfio`.

---

## 2. Optical Projection Physics (The Black Background Principle)

Projectors (DLP, 3LCD, Laser) work by casting light onto a surface. They cannot project "black light".
- **Color `#000000`** = Mirrors tilt away / LCD pixels block light = 0 lumens emitted.
- By setting the background color of the 3D scene to completely pure black (`0x00000000` or `#000000`), only the 3D model emits light onto the projection surface.
- In a dimly lit room or on a semi-reflective acrylic surface, this optical reality creates the true illusion of a floating 3D hologram in empty space.

---

## 3. Holographic Shading Techniques

To enhance the visual "hologram" aesthetic beyond standard solid geometry:

1. **Fresnel / Rim Glow**:
   - Accentuates outer silhouettes:
     $$I_{rim} = \left(1.0 - \max(\vec{N} \cdot \vec{V}, 0.0)\right)^{power}$$
   - Makes the edges of the model glow brightly in cyan, green, or amber.

2. **Animated Scanlines**:
   - Sinusoidal intensity modulation across world or screen Y coordinate:
     $$I_{scan} = \sin(y \cdot frequency + time \cdot speed) \cdot amplitude$$

3. **Additive / Semi-transparent Blending**:
   - Optional mode where internal geometry is subtly visible through translucent glowing outer layers.

---

## 4. Bundled Asset Specifications

To allow immediate out-of-the-box demonstration without requiring user downloads:
- **`earth_globe.glb`**: Textured Earth sphere with spinning animation.
- **`sci_fi_droid.glb`**: Futuristic mechanical droid with articulated hover animation.
- **`human_heart.glb`**: Anatomical beating 3D heart showing volumetric structure.
- **Custom Loader**: Storage Access Framework (`ACTION_OPEN_DOCUMENT`) allows users to pick and load any standard `.glb` file directly from local storage or cloud drive.
