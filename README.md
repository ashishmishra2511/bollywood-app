# Bollywood — Movie Guessing Game

A two-player Bollywood-themed movie guessing game. Player 1 picks a movie title, Player 2 guesses letters one at a time. Each wrong letter strikes one off the **BOLLYWOOD** banner. Score = remaining letters × 10. Pass-and-play, infinite rounds, runs anywhere.

**Created by:** Ashish Mishra
**Contact:** [ashishmishra2511@gmail.com](mailto:ashishmishra2511@gmail.com)
**Version:** 1.0.0
**License:** MIT

---

## What's in this folder

```
bollywood-app/
├── index.html              The game (Progressive Web App)
├── manifest.webmanifest    PWA metadata (name, icons, theme)
├── service-worker.js       Offline cache & background runtime
├── icons/                  All app icons (16px → 1024px, plus maskable + Apple)
├── README.md               This file
├── DEPLOYMENT.md           How to host & publish to app stores
├── CAPACITOR.md            How to wrap as native iOS/Android apps
└── LICENSE                 MIT license text
```

---

## Quick test (local)

To play it straight away on your computer:

```bash
# From inside the bollywood-app folder
python3 -m http.server 8080
# then visit http://localhost:8080 in your browser
```

> ⚠️ Don't open `index.html` directly with `file://` — service workers and the install button only work over `http://` or `https://`. The simple Python server above is fine for testing.

---

## Install on your devices

The app is a **Progressive Web App (PWA)**. Once you host it on any HTTPS URL (free options below), you can install it on every major platform — no app stores needed for personal sharing.

| Platform | How to install |
|---|---|
| **Android (Chrome)** | Open the URL → tap the **"Install app"** banner, or **⋮ menu → Install app** |
| **iPhone / iPad (Safari)** | Open the URL → tap the **Share** button → **Add to Home Screen** |
| **macOS / Windows / Linux (Chrome / Edge)** | Open the URL → click the **install icon** in the address bar (the ⊕ symbol), or **⋮ menu → Install Bollywood…** |
| **Samsung Internet** | Open the URL → menu → **Add page to → Home screen** |

Once installed it gets its own icon, runs full-screen, and works offline.

Inside the app, tap the **(i)** button in the top-right corner to see install options + creator info.

---

## Hosting (free options)

You need to put this folder online over HTTPS. Three good free hosts:

### Option 1 — Netlify (easiest)
1. Go to [app.netlify.com/drop](https://app.netlify.com/drop)
2. Drag the entire `bollywood-app` folder onto the page
3. Done — you get a URL like `https://your-app.netlify.app`

### Option 2 — Vercel
1. Sign up at [vercel.com](https://vercel.com)
2. Push this folder to a GitHub repo
3. Import the repo on Vercel — it auto-deploys

### Option 3 — GitHub Pages
1. Push this folder to a GitHub repository called `bollywood-app`
2. **Settings → Pages → Branch: main, folder: /(root)**
3. Your URL will be `https://YOUR_USERNAME.github.io/bollywood-app/`

---

## Want to publish to Play Store / App Store?

See [`DEPLOYMENT.md`](./DEPLOYMENT.md) for full step-by-step guides covering:

- **Google Play Store** (via PWABuilder or Bubblewrap → APK/AAB)
- **Apple App Store** (via PWABuilder or Capacitor → IPA)
- **Microsoft Store** (via PWABuilder → MSIX)

If you'd rather wrap as a native app first, see [`CAPACITOR.md`](./CAPACITOR.md).

---

## How the game works

1. **Welcome** — both players enter their name, one at a time
2. **Director's chair** — current director types a movie title (kept secret with a Show/Hide toggle)
3. **Pass the device** — to the guesser
4. **Game** — guesser types letters & digits on their keyboard. Vowels are revealed automatically. Each wrong letter strikes one from the BOLLYWOOD banner with an animation + sound
5. **Round end** — points awarded based on remaining BOLLYWOOD letters (× 10)
6. **Roles swap** — repeat as long as you want
7. **New Game** anytime to reset scores and start over

---

## Tech notes

- Single-file HTML — no build step, no framework, no bundler
- Pure CSS animations + Web Audio API for sound effects (no audio files)
- Uses `navigator.vibrate` for haptic feedback on mobile
- Works fully offline once installed (service worker caches everything)
- All icons embedded as inline SVG; only the favicons are PNG
- ~70KB compressed total

---

## License

MIT — see [`LICENSE`](./LICENSE) for full text. Free to fork, modify, or redistribute with attribution.
