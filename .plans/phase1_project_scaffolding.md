# Phase 1: Project Scaffolding & Dependencies

## Objective
Establish the foundational Android project structure using modern Gradle (Kotlin DSL), Android Gradle Plugin 8.x, Jetpack Compose, CameraX, Filament/Sceneview, and MediaPipe dependencies.

---

## Tasks & Steps

### 1.1 Android Project Scaffolding
- [ ] Initialize standard Android app structure:
  - `app/build.gradle.kts`
  - `settings.gradle.kts`
  - `build.gradle.kts` (root)
  - `gradle/libs.versions.toml` (version catalog)
  - `app/src/main/AndroidManifest.xml`
- [ ] Configure `minSdk = 26`, `targetSdk = 35`, `compileSdk = 35`.
- [ ] Configure Java 17/21 toolchain with Kotlin 2.0+ and Jetpack Compose compiler.

### 1.2 Permissions & Manifest Declarations
- [ ] Request Camera permission: `<uses-permission android:name="android.permission.CAMERA" />`.
- [ ] Request feature: `<uses-feature android:name="android.hardware.camera" android:required="true" />`.
- [ ] Configure hardware acceleration: `android:hardwareAccelerated="true"`.

### 1.3 Version Catalog (`libs.versions.toml`) Dependencies
- [ ] AndroidX Core, Lifecycle, Activity Compose.
- [ ] Jetpack Compose Material 3 & Navigation.
- [ ] CameraX (`camera-core`, `camera-camera2`, `camera-lifecycle`, `camera-view`).
- [ ] Google MediaPipe Tasks Vision (`tasks-vision:0.10.14`).
- [ ] Sceneview / Google Filament (`io.github.sceneview:sceneview:2.2.1`).

### 1.4 Base Application & Activity Shell
- [ ] Create `MainActivity.kt` with runtime camera permission handling.
- [ ] Set up basic Jetpack Compose scaffold to verify build integrity.

---

## Verification Criteria
- `./gradlew assembleDebug` completes with zero errors.
- App launches and displays basic Compose UI while cleanly prompting for Camera permission.
