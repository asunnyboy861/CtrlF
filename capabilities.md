# Capabilities Configuration

## Analysis
Based on operation guide analysis:
- "相机" / "camera" / "AVFoundation" → Camera access required
- "拍照" / "照片" / "截图保存" → Photo Library access required
- "购买" / "Pro" / "$4.99" / "IAP" → In-App Purchase required
- "搜索历史" / "SwiftData" → Local data persistence (no iCloud needed)
- "手电筒" / "flashlight" → AVFoundation torch control (no special capability)
- "语音" / "AVSpeechSynthesizer" → Audio playback (no special capability)

## Auto-Configured Capabilities
| Capability | Status | Method |
|------------|--------|--------|
| Camera (NSCameraUsageDescription) | ✅ Configured | Info.plist |
| Photo Library (NSPhotoLibraryAddUsageDescription) | ✅ Configured | Info.plist |
| In-App Purchase | ✅ Configured | Xcode capability |

## Manual Configuration Required
| Capability | Status | Steps |
|------------|--------|-------|
| None | N/A | N/A |

## No Configuration Needed
- iCloud: Not needed — all data stored locally via SwiftData
- Push Notifications: Not needed — no notification features
- HealthKit: Not needed — not a health app
- Location Services: Not needed — no location features
- Apple Watch: Not needed in MVP
- Background Modes: Not needed — no background processing
- Siri: Not needed in MVP
- Sign in with Apple: Not needed — no user accounts

## Verification
- Build succeeded after configuration: ⏳ Pending
- All entitlements correct: ⏳ Pending
