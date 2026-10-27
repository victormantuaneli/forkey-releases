# ForKey for iOS

**ForKey** is a professional acoustic piano tuning application for iOS, engineered with physical string inharmonicity models, custom Railsback stretch tuning curve calculation, and a multi-ring stroboscopic visualization.

---

## Architecture & Features

### 1. Acoustic Inharmonicity Physics ($B$-Factor)
* **Real-time Fletcher String Model**: Fits $f_n = n f_1 \sqrt{1 + B n^2}$ to spectral peaks detected via Apple's **Accelerate (vDSP)** real-to-complex FFT.
* **Global Model**: Solves and smooths the inharmonicity curve across all 88 keys from sparse note samples.

### 2. Railsback Custom Stretch Solver
* **Physical Interval Optimization**: Optimizes partial alignments (2:1 & 4:2 octaves, 3:1 twelfths, 3:2 fifths) derived from weighted interval matrices.
* **Temperaments**: Built-in support for Equal Temperament, Werckmeister III, Vallotti, and Pythagorean temperaments.
* **Pitch Raise Overpull**: Dynamic bridge/soundboard settling compensation.

### 3. Visuals & Interface
* **Concentric Strobe Wheel**: 4 rotating dashed rings representing partials 1, 2, 3, and 4. Rotates clockwise when sharp, counter-clockwise when flat, and freezes stationary when in tune.
* **Central Cents Needle**: Adaptive spring-damped needle indicating deviation from -50 to +50 cents.
* **Interactive 88-Key Piano Keyboard**: Visualizes key measurement status, active note tracking, and manual lock.

---

## Project Structure

```
ForKey/
├── App/
│   ├── ForKeyApp.swift           // SwiftUI Application lifecycle & microphone permission
│   └── Info.plist                // iOS bundle configuration
├── Audio/
│   └── AudioInputManager.swift   // AVAudioEngine input tap (Measurement mode)
├── DSP/
│   ├── AccelerateFFT.swift       // vDSP real-to-complex DFT with windowing
│   ├── PeakFinder.swift          // Parabolic peak interpolation (sub-bin precision)
│   ├── BandpassFilter.swift      // 6th-order IIR biquad bandpass filter
│   ├── ZeroCrossingDetector.swift// High-speed phase and zero-crossing detection
│   └── CombinedPitchSmoother.swift// Adaptive dampening & needle smoothing
├── Engine/
│   ├── PianoPhysics.swift        // Fletcher inharmonicity & acoustic math
│   ├── InharmonicityEstimator.swift // Linear regression solver for B & outlier rejection
│   ├── GlobalInharmonicityModel.swift // 88-key physical interpolation
│   ├── RailsbackCurveSolver.swift // Interval-weighted custom tuning curve solver
│   ├── PitchRaiseCalculator.swift // Soundboard downbearing overpull compensation
│   └── TuningCoordinator.swift   // Central controller linking DSP to UI
├── Models/
│   ├── PianoNote.swift           // 88-key model
│   ├── TuningStyle.swift         // Pure octave, pure 12th, pure 5th, low stretch
│   └── Temperament.swift         // Historic diatonic temperament offsets
├── Views/
│   ├── Wheel/
│   │   └── TuningWheelView.swift // 4-ring strobe visualizer & center dial
│   ├── Main/
│   │   ├── MainTuningView.swift  // Primary dashboard
│   │   ├── NoteInfoBannerView.swift // Note name, Hz, cents, and B
│   │   └── ModeSwitchSelector.swift // Auto vs Lock mode toggle
│   ├── Keyboard/
│   │   └── PianoKeyboardView.swift // 88-key interactive keyboard
│   └── Settings/
│       └── TuningSettingsSheet.swift // Styles, temperaments, A4 calibration
├── Resources/                    // Bundled tuning styles & temperaments
├── .github/workflows/
│   └── build-ipa.yml            // Cloud CI to compile .ipa on macOS
└── ForKey.xcodeproj/             // Xcode project file
```

---

## How to Compile the `.ipa` from Windows (GitHub Actions)

1. Initialize git and push to a GitHub repository:
   ```bash
   cd C:\Users\Felipe\Documents\ForKey
   git init
   git add .
   git commit -m "Initial commit of ForKey iOS"
   git branch -M main
   git remote add origin https://github.com/YOUR_USERNAME/ForKey.git
   git push -u origin main
   ```
2. Navigate to your repository on **GitHub** and click on the **Actions** tab.
3. The workflow **"Build ForKey iOS IPA"** will run on a cloud macOS runner (`macos-14`).
4. Once completed (approx. 2 minutes), download the artifact **`ForKey-iOS-IPA`**, which contains **`ForKey.ipa`**!

---

## Sideloading & Automatic Updates via SideStore

### 📲 Add to SideStore / AltStore (Recommended)

ForKey provides an official community repository so you can install and receive **one-tap updates** directly inside SideStore or AltStore without manually downloading IPAs!

1. **One-Click Add**:
   - [Add Source to SideStore](sidestore://source?url=https://raw.githubusercontent.com/victormantuaneli/forkey-releases/main/apps.json)
   - [Add Source to AltStore](altstore://source?url=https://raw.githubusercontent.com/victormantuaneli/forkey-releases/main/apps.json)

2. **Manual Add**:
   - In **SideStore** or **AltStore**, tap the **Sources** tab.
   - Tap **+** in the top right corner.
   - Paste the repository source URL:
     ```
     https://raw.githubusercontent.com/victormantuaneli/forkey-releases/main/apps.json
     ```
   - Tap **Add**. ForKey will now appear in your **Browse** list and alert you whenever new updates are released!

---

## Other Sideloading Options

* **Sideloadly**: Connect your iPhone to Windows via USB, drag `ForKey.ipa` from the [Latest Release](https://github.com/victormantuaneli/forkey/releases/latest), enter your Apple ID, and click Start.
* **TrollStore**: For supported iOS versions (iOS 14.0–17.0 on eligible devices), open the `.ipa` in TrollStore for permanent signing without 7-day refreshing.

