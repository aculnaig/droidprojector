# Context: Hardware & Multi-Display Subsystem

## 1. External Display Protocols & Android Subsystem

Android natively manages secondary displays via `DisplayManager`. When a phone is connected to an external display, the system registers a new `Display` instance with the `DISPLAY_CATEGORY_PRESENTATION` flag.

### Connection Hardware Profiles
1. **USB-C DisplayPort Alternate Mode (Direct Cable)**:
   - Modern flagship phones (e.g. Samsung Galaxy, Google Pixel 8+, OnePlus, Sony Xperia) output hardware DisplayPort over Type-C.
   - Low latency (< 16ms), high resolution (1080p / 4K @ 60Hz), reliable power delivery.
2. **USB-C to HDMI Hub / Adapter**:
   - Converts DP alt mode to standard HDMI input for portable LED projectors, pico-projectors, or monitors.
3. **Wireless Display (Miracast / Google Cast)**:
   - Wi-Fi direct connection to smart projectors or dongles.
   - Slightly higher latency (50-100ms), but zero physical cables.

---

## 2. The `android.app.Presentation` Architecture

A `Presentation` is a specialized `Dialog` whose window is rendered on a secondary `Display`.

```kotlin
class HologramPresentation(
    context: Context,
    display: Display,
    private val onSurfaceReady: (SurfaceView) -> Unit
) : Presentation(context, display) {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.presentation_hologram)
        
        // Ensure the presentation window runs full screen with no decor
        window?.apply {
            setLayout(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.MATCH_PARENT)
            setBackgroundDrawable(ColorDrawable(Color.BLACK))
            addFlags(WindowManager.LayoutParams.FLAG_FULLSCREEN)
            addFlags(WindowManager.LayoutParams.FLAG_KEEP_SCREEN_ON)
        }
        
        val surfaceView = findViewById<SurfaceView>(R.id.hologram_surface)
        onSurfaceReady(surfaceView)
    }
}
```

---

## 3. Fallback Display Strategy

To guarantee development speed and accessibility on devices without an immediate external display attached:
- When `DisplayManager.getDisplays(DISPLAY_CATEGORY_PRESENTATION)` is empty:
  - Phone UI dynamically renders a split preview or Picture-in-Picture (PiP) containing the Filament 3D Surface.
- When an external display is plugged in:
  - Phone UI immediately switches the 3D surface to the external `Presentation` window, and the phone screen transforms into a dedicated control and camera tracking dashboard.
- Android's native `Simulate secondary displays` developer setting is fully supported for testing on emulators and physical devices.
