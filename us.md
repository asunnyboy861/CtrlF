# CtrlF - iOS Development Guide

## Executive Summary

**CtrlF** (subtitle: *Find Text in Any Book*) is a real-time camera-based text search application for iOS that enables users to find specific keywords and phrases in physical books, textbooks, manuals, and any printed material — essentially bringing the Ctrl+F experience to the real world.

### Product Vision
Transform how 50M+ students, researchers, and professionals interact with physical books by providing instant, real-time text search through their iPhone camera. No photo capture required — just point, type, and find.

### Key Differentiators
- **Brand = Function**: "CtrlF" is the world's most recognized keyboard shortcut, making the app instantly understandable
- **Real-time Search**: Live camera feed with instant text matching and highlight overlay — no capture-then-search workflow
- **Multi-word Phrase Search**: Unlike SearchCam (single word only), CtrlF supports full phrase matching
- **One-time Purchase**: $4.99 Pro upgrade vs. competitors' subscription models ($5.99/mo for I Spy Text)
- **100% On-device**: All OCR processing via Apple Vision framework — zero network requests, zero data upload
- **Search History**: Persistent search history with SwiftData — no competitor offers this
- **Flashlight Control**: Built-in torch toggle for low-light environments
- **iOS 17+ Compatibility**: Broader device support vs. I Spy Text's iOS 18+ requirement

### Target Market
- **Primary**: US college/university students (20M+)
- **Secondary**: Researchers, professionals, book readers
- **Tertiary**: Anyone who needs to find text in printed materials

## Competitive Analysis

| App | Rating | Price | Strengths | Weaknesses | Our Advantage |
|-----|--------|-------|-----------|------------|---------------|
| **SearchCam** | 4.5/5 (206) | Free + $3 ad removal | First-mover, real-time search, simple UI | Single-word only, no updates since 2021, outdated UI, no phrase search | Multi-word phrases, active updates, search history, modern SwiftUI |
| **I Spy Text** | New | Free + $5.99/mo subscription | Multi-word search, 25+ languages, audio feedback | iOS 18+ only, expensive subscription, no track record | iOS 17+, one-time purchase, search history, flashlight control |
| **Prizmo Go** | 4.4/5 (278) | Free + IAP | Excellent OCR, VoiceOver support, curved text handling | Text extraction focused, not search-oriented, complex UI | Search-first design, simpler UX, real-time highlight overlay |
| **Google Lens** | 4.5/5 | Free | Powerful, multi-purpose | Requires internet, not book-focused, privacy concerns | 100% offline, book-optimized, instant highlight overlay |
| **ABBYY FineReader** | 4.0/5 | Free + IAP | 183 languages, AI classification | Document scanner, not real-time search | Real-time search, simpler workflow, no document management overhead |

## Apple Design Guidelines Compliance

- **Camera Usage**: NSCameraUsageDescription required — clear, user-facing justification provided
- **Photo Library**: NSPhotoLibraryAddUsageDescription for screenshot save feature
- **Privacy**: All OCR processing 100% on-device via Vision framework — no data leaves the device
- **Human Interface Guidelines**:
  - Ultra-thin material backgrounds for search bar and toolbar
  - SF Symbols for all icons
  - Dynamic Type support for accessibility
  - VoiceOver labels on all interactive elements
  - Dark mode as default (camera app context)
- **App Store Review Guidelines**:
  - No health/medical claims — utility/productivity category
  - No user data collection — privacy-first design
  - IAP follows guidelines for one-time non-consumable purchase
  - Restore Purchases button included in Settings

## Technical Architecture

- **Language**: Swift 5.9+
- **Framework**: SwiftUI (primary), UIKit (camera preview via UIViewRepresentable)
- **Camera**: AVFoundation (AVCaptureSession, AVCaptureVideoDataOutput)
- **OCR**: Vision framework (VNRecognizeTextRequest)
- **Data Persistence**: SwiftData (search history) + UserDefaults (settings)
- **Rendering**: CoreGraphics + SwiftUI Canvas (highlight overlay)
- **Audio**: AVFoundation (AVSpeechSynthesizer for voice feedback)
- **Monetization**: StoreKit 2 (one-time non-consumable IAP)
- **Minimum iOS**: 17.0

## Module Structure

```
CtrlF/
├── CtrlFApp.swift
├── Views/
│   ├── ContentView.swift
│   ├── CameraPreviewView.swift
│   ├── HighlightOverlayView.swift
│   ├── SearchBarView.swift
│   ├── BottomToolBarView.swift
│   ├── SearchHistoryView.swift
│   └── SettingsView.swift
├── ViewModels/
│   └── SearchViewModel.swift
├── Services/
│   ├── CameraManager.swift
│   ├── TextRecognitionEngine.swift
│   ├── FlashlightController.swift
│   ├── SearchHistoryManager.swift
│   └── StoreManager.swift
├── Models/
│   ├── MatchedTextItem.swift
│   ├── RecognizedTextItem.swift
│   └── SearchHistoryItem.swift
├── Utilities/
│   └── Constants.swift
└── Assets.xcassets/
```

## Implementation Flow

### Step 1: Project Setup
- Create new SwiftUI iOS App project in Xcode
- Set Bundle ID: com.zzoutuo.CtrlF
- Set minimum deployment: iOS 17.0
- Add NSCameraUsageDescription and NSPhotoLibraryAddUsageDescription to Info.plist
- Add SwiftData capability

### Step 2: Camera Layer
- Implement CameraManager with AVCaptureSession
- Configure back camera (builtInWideAngleCamera)
- Set session preset to .photo for high-quality OCR
- Implement AVCaptureVideoDataOutputSampleBufferDelegate
- Create CameraPreviewView (UIViewRepresentable with AVCaptureVideoPreviewLayer)
- Implement frame callback via onFrameCaptured closure

### Step 3: OCR Recognition Layer
- Implement TextRecognitionEngine with VNRecognizeTextRequest
- Configure recognition level .accurate for best results
- Set recognition languages to ["en-US", "en-GB"] (expandable)
- Enable language correction (usesLanguageCorrection = true)
- Implement frame skip interval (every 2nd frame) for performance
- Implement case-insensitive substring matching for multi-word phrases
- Return MatchedTextItem array with boundingBox and confidence

### Step 4: Highlight Rendering Layer
- Implement HighlightOverlayView using SwiftUI Canvas
- Convert Vision coordinate system (bottom-left origin) to SwiftUI (top-left origin)
- Draw yellow semi-transparent fill (0.35 opacity) with yellow stroke (2pt)
- Apply rounded corners (4pt radius) to highlight rectangles
- Set allowsHitTesting(false) for pass-through interaction

### Step 5: UI Assembly
- Build ContentView with ZStack: black background + camera preview + highlight overlay + controls
- Implement search bar with .ultraThinMaterial background
- Add match count badge (yellow capsule)
- Add flashlight toggle button
- Implement bottom toolbar: Capture, History, Settings
- Wire up onChange handlers for search text and matched items

### Step 6: Search History
- Define SearchHistoryItem as SwiftData @Model
- Implement SearchHistoryManager with max 50 items
- Create SearchHistoryView with grouped by date layout
- Add tap-to-research functionality

### Step 7: Settings & IAP
- Implement SettingsView with sections: Search, Camera, Pro, About
- Add OCR language selection
- Add highlight color customization
- Add flashlight default toggle
- Implement StoreKit 2 one-time non-consumable purchase
- Add Restore Purchases button
- Add Privacy Policy and Terms links

### Step 8: Polish & Optimization
- Add animation for highlight appearance/disappearance
- Implement low-light detection and flashlight suggestion
- Add haptic feedback on match found
- Optimize frame processing frequency
- Test on multiple device sizes

## UI/UX Design Specifications

- **Primary Color**: System Yellow (#FFD60A) — highlight matched text
- **Background**: Pure Black (#000000) — camera preview
- **Material**: .ultraThinMaterial — search bar and toolbar
- **Typography**: SF Pro, Dynamic Type
- **Corner Radius**: 12pt (search bar), 16pt (bottom toolbar), 4pt (highlight boxes)
- **Spacing**: 16pt horizontal margins, 8pt vertical spacing
- **Icons**: SF Symbols 5
- **Animations**: Highlight fade-in 0.2s, search bar expand 0.3s spring
- **Layout**: Single-hand optimized — search bar at top, toolbar at bottom

## Code Generation Rules

- One feature per module, high cohesion, low coupling
- MVVM architecture with ObservableObject
- Semantic naming, clear file structure
- Never add comments in code unless asked
- Apple native first: SwiftUI, Vision, AVFoundation, SwiftData
- All OCR processing on .global(qos: .userInteractive) queue
- UI updates on .main queue
- Use weak self in closures to prevent retain cycles
- Frame skip interval to control CPU usage
- 100% on-device processing — zero network requests

## Build & Deployment Checklist

- [ ] Xcode project created with correct Bundle ID
- [ ] Info.plist includes camera and photo library usage descriptions
- [ ] SwiftData model compiled
- [ ] StoreKit 2 IAP product configured in App Store Connect
- [ ] App icon generated and added
- [ ] Privacy Policy page hosted
- [ ] Terms of Use page hosted
- [ ] Support page hosted
- [ ] App Store metadata prepared (description, keywords, screenshots)
- [ ] TestFlight beta testing completed
- [ ] App Store review submitted
