# Pricing Configuration

## Monetization Model: Freemium (Free + One-Time IAP)

- **Download Price**: Free
- **IAP Type**: Non-Consumable (One-Time Purchase)
- **Subscription**: None
- **Rationale**: Student audience prefers one-time purchase over subscriptions. No ongoing API/server costs justify subscription pricing. $4.99 one-time is perceived as exceptional value vs. I Spy Text's $5.99/month subscription.

## Free Tier Features
- Real-time camera text search
- Photo library text search
- Multi-word phrase matching
- Yellow highlight overlay
- Flashlight toggle
- 5 free searches per day (shared across Camera & Photo modes, loss aversion trigger)

## Pro Upgrade Features (One-Time $4.99)
- Unlimited searches (no daily limit)
- Search history (SwiftData persistence)
- Screenshot capture and save
- Custom highlight colors
- Audio feedback (text-to-speech)
- Multi-language OCR (25+ languages)
- Low-light auto flashlight
- Priority future updates

## App Store Connect Pricing

### IAP Product
- **Reference Name**: CtrlF Pro Upgrade
- **Product ID**: `com.zzoutuo.CtrlF.pro`
- **Type**: Non-Consumable
- **Price**: $4.99 (Tier 5)
- **Display Name**: CtrlF Pro
- **Description**: Unlock unlimited searches and all Pro features

### App Price
- **Price Tier**: Free

## Launch Pricing Strategy
- First month: $4.99 (50% off from "original" $9.99)
- After launch period: $9.99 regular price
- Scarcity messaging: "Limited time offer"

## Policy Pages Required
- Support Page: ✅
- Privacy Policy: ✅
- Terms of Use: ✅ (Required for IAP apps)

## Apple IAP Compliance Checklist
- [x] Non-consumable product type selected
- [x] Restore Purchases functionality implemented
- [x] No dark patterns in paywall UI
- [x] Clear pricing displayed before purchase
- [x] No auto-renewal (one-time purchase)
