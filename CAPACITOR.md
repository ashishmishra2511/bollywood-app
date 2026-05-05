# Wrapping as a Native App with Capacitor

Capacitor turns this PWA into a real native iOS / Android / desktop app. It gives you access to native APIs (haptics, share, in-app purchase, push notifications, etc.) which is especially useful for App Store submissions.

> **You can skip this entirely** if PWABuilder's output works for you. This guide is for going deeper.

---

## Prerequisites

- **Node.js 18+** ([nodejs.org](https://nodejs.org/))
- **Android Studio** (for Android builds) — free
- **Xcode 15+** + **Mac** (for iOS builds) — Mac required, Xcode is free
- **Apple Developer Program** ($99/yr) for App Store submissions
- **Google Play Console** ($25 one-time) for Play Store submissions

---

## Setup (one-time)

```bash
# In a new folder somewhere
mkdir bollywood-native && cd bollywood-native
npm init -y
npm install @capacitor/core @capacitor/cli @capacitor/ios @capacitor/android

# Initialize Capacitor with your app metadata
npx cap init "Bollywood" "com.ashishmishra.bollywood" --web-dir=www
```

This creates a `capacitor.config.json` and a `www/` folder.

### Copy the PWA into Capacitor

```bash
# Copy the app contents into www/
cp -r /path/to/bollywood-app/* www/
```

### Configure capacitor.config.json

Replace the generated file with this:

```json
{
  "appId": "com.ashishmishra.bollywood",
  "appName": "Bollywood",
  "webDir": "www",
  "bundledWebRuntime": false,
  "ios": {
    "contentInset": "always",
    "scheme": "Bollywood",
    "backgroundColor": "#fff5e1"
  },
  "android": {
    "backgroundColor": "#fff5e1",
    "allowMixedContent": false
  },
  "plugins": {
    "SplashScreen": {
      "launchShowDuration": 1000,
      "launchAutoHide": true,
      "backgroundColor": "#fff5e1",
      "androidScaleType": "CENTER_CROP",
      "showSpinner": false
    }
  }
}
```

### Add native plugins (optional but recommended)

```bash
npm install @capacitor/haptics @capacitor/status-bar @capacitor/splash-screen @capacitor/app
npx cap sync
```

These give you proper native haptic feedback, status-bar styling, and a splash screen — crucial for getting through Apple's "is this just a website?" review.

### Wire up native APIs in `www/index.html`

Add this near the bottom of the `<script>` block (it's a graceful enhancement — falls back to web APIs if Capacitor isn't present):

```javascript
// Capacitor native enhancements
if (window.Capacitor) {
  import('@capacitor/haptics').then(({ Haptics, ImpactStyle }) => {
    window.nativeVibrate = (style = 'light') => {
      Haptics.impact({ style: style === 'heavy' ? ImpactStyle.Heavy : ImpactStyle.Light });
    };
  });

  import('@capacitor/status-bar').then(({ StatusBar, Style }) => {
    StatusBar.setBackgroundColor({ color: '#4a0e2a' });
    StatusBar.setStyle({ style: Style.Dark });
  });
}
```

---

## Add native platforms

```bash
npx cap add ios
npx cap add android
npx cap sync
```

This creates `ios/` and `android/` folders with full native projects.

---

## Building for Android

```bash
npx cap open android
```

Android Studio opens. From there:

1. Wait for Gradle sync to complete
2. **Build → Generate Signed Bundle / APK → Android App Bundle (.aab)**
3. Create or select a signing key (back this up forever — you can't update without it)
4. Build → output is in `android/app/build/outputs/bundle/release/app-release.aab`

Upload to [Play Console](https://play.google.com/console).

---

## Building for iOS

```bash
npx cap open ios
```

Xcode opens. From there:

1. Select **Bollywood** in the navigator → **Signing & Capabilities**
2. Set your **Team** (your Apple Developer account)
3. Update **Bundle Identifier** if needed
4. Add app icons by dragging the contents of `bollywood-app/icons/` into the project's **Assets.xcassets → AppIcon**
5. **Product → Archive**
6. **Window → Organizer → Distribute App → App Store Connect**

Apple's review process for App Store submissions is real. They'll reject "just a website" wrappers — having Capacitor with at least Haptics + StatusBar configured satisfies the "uses native iOS features" requirement.

Upload to [App Store Connect](https://appstoreconnect.apple.com/).

---

## Updating after launch

When you change the game itself (`www/` contents):

```bash
# 1. Update files in www/
cp -r /path/to/updated-bollywood-app/* www/

# 2. Sync to native projects
npx cap sync

# 3. Rebuild & resubmit
npx cap open ios     # or android
# … archive / generate bundle / upload …
```

For minor content updates that don't change manifest properties, you can actually skip resubmission entirely — the web view fetches the latest from your hosted PWA URL automatically (set `server.url` in `capacitor.config.json`).

---

## Troubleshooting

**"App is rejected as it's just a website"** — Add at least one Capacitor native plugin (Haptics is enough). Make sure SplashScreen is configured. Add at least one native gesture or notification.

**"Service worker not working in Capacitor"** — Capacitor uses native WebView; service workers don't run there. Your `service-worker.js` is only needed for the web/PWA version. Capacitor caches assets natively.

**"Icons look wrong in iOS"** — iOS requires icons at very specific sizes. Use [appicon.co](https://appicon.co/) to generate the iOS asset catalog from `icons/icon-1024.png`.

**Android build fails with "SDK location not found"** — install Android Studio first, then in Studio: **Tools → SDK Manager** → install Android 13+ SDK.

---

## Why Capacitor instead of just PWABuilder?

| Feature | PWABuilder Output | Capacitor |
|---|---|---|
| Easy to set up | ✅ Drag-and-drop | ⚠️ Some CLI |
| Auto-updates from your URL | ✅ Yes | ✅ Yes |
| Native iOS APIs (haptics, share) | ❌ Limited | ✅ Full |
| Apple App Store acceptance rate | ⚠️ Mixed | ✅ Higher |
| Push notifications | ❌ | ✅ |
| In-app purchases | ❌ | ✅ |
| Long-term flexibility | ⚠️ | ✅ |

If you only want Play Store + Microsoft Store + web installs, **PWABuilder is enough**. For Apple App Store, **Capacitor gives you fewer rejection headaches**.

---

Made with ♥ by Ashish Mishra
