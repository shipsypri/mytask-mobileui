# K-INDEV Logistics — My Tasks (PWA)

Mobile-friendly driver task app. Two screens:
1. **My Tasks** — trip card with delivery / pickup counts and quick actions.
2. **Preview** — full stop-by-stop timeline (addresses, ETA, TimeSlot) including EV charge break stops.

Built as a **Progressive Web App** so it installs to the Android home screen like a native app, and can be wrapped into a signed APK with zero extra code.

---

## File structure

```
.
├── index.html          # The app (both screens, vanilla JS — no build step)
├── manifest.json       # PWA manifest
├── sw.js               # Service worker (offline caching)
├── vercel.json         # Vercel routing + service-worker headers
├── icons/
│   ├── icon-192.png
│   ├── icon-512.png
│   └── icon-512-maskable.png
└── README.md
```

No dependencies, no build step. Just static files.

---

## Deploy to Vercel

### Option A — GitHub + Vercel (recommended)
1. Push this folder to a GitHub repo.
2. Go to [vercel.com](https://vercel.com) → **New Project** → import the repo.
3. Framework preset: **Other** (it's plain static HTML).
4. Click **Deploy**. You'll get a URL like `https://your-app.vercel.app`.

### Option B — Vercel CLI
```bash
npm i -g vercel
vercel
```
Follow the prompts; accept defaults.

---

## Install on Android (no APK needed)

Once it's live:
1. Open the Vercel URL in **Chrome** on Android.
2. Tap the **⋮ menu** → **Install app** (or **Add to Home Screen**).
3. The app icon lands on your home screen and opens fullscreen — no browser UI.

This is how most modern field-ops apps ship. It works offline thanks to the service worker.

---

## Generate a real signed APK

If you need an actual `.apk` / `.aab` file (e.g. to sideload or upload to Play Store):

1. Visit [**pwabuilder.com**](https://www.pwabuilder.com/).
2. Paste your live Vercel URL.
3. Click **Package for stores → Android**.
4. Download the generated APK. It's a Trusted Web Activity wrapper around this PWA — same UI, native installer.

You can sideload the APK directly (Android settings → allow install from unknown sources), or submit the `.aab` to Google Play.

---

## Editing the data

All trip data lives in the `STOPS` array near the bottom of `index.html`. Each entry:

```js
{ type:'stop', n:1, address:'…', eta:'10:15', slot:'09:00 - 21:00' }
```

- `type`: `'home'` (depot), `'stop'` (delivery), or `'ev'` (charge break — rendered in green and surfaced by "Show breaks only")
- `n`: sequence number shown on the left
- `address`, `eta`, `slot`: free text

The deliveries count on the home screen auto-updates from this array.

---

## License
Internal use.
