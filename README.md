# Setup Garage — F1 Manager 24 companion

A single-file web app for working out car setups in F1 Manager 24 — pick a track,
adjust the five setup sliders, record what your driver actually reports back after
a run, and it calculates the closest setup consistent with everything you've told it.

## What it is (and isn't)

- **One self-contained file.** `index.html` has all the HTML, CSS and JavaScript
  inline. No build step, no bundler, no `npm install`, no framework.
- **No backend, no Node.** Every calculation runs in the browser. Nothing is sent
  to a server, ever.
- **No database.** All data (sliders, driver names, test history) is saved in the
  browser's own local storage, scoped to whichever device/browser opens it. There's
  no sync between devices — it's intentionally local-first.

## Running it

**Locally:** just open `index.html` in any modern browser (double-click it, or
drag it into a browser window). That's it — nothing to install.

**Hosted for others to use:** since it's a static file with no backend, any static
host works. Two free options that need zero server maintenance:

- **GitHub Pages** — in the repo settings, enable Pages and point it at the branch
  containing `index.html`. GitHub gives you a permanent URL for free.
- **Cloudflare Pages** — connect the repo, no build command needed (it's already
  built), output directory is the repo root. Also free, also permanent.

Either gives a stable public link that doesn't depend on any one machine being
switched on.

## How the calculator works

Five setup sliders (Front Wing, Rear Wing, Anti-Roll, Tyre Camber, Toe-Out) each
map to five car-balance readings (Oversteer, Braking Stability, Cornering, Traction,
Straights) through a fixed weighted formula. When you record a test, you're telling
the app how far off each balance reading was (Optimal/Great/Good/Bad-High/Bad-Low)
for that specific slider combination.

Hitting **Calculate** brute-forces every valid slider combination (~1 million,
runs in well under a second) and keeps only the ones consistent with every test
you've recorded so far, ranked by closeness to your current setup. Record more
tests across a session and it narrows in further each time.

## Known limitations

- Per-track "starting point" presets are a community-sourced list, not guaranteed
  optimal — they're just a sensible place to start before recording your own tests.
- The slider ranges assume the default fitted car parts. If a specific part changes
  a slider's real min/max, that's not currently configurable in the UI.
- Saved data is per-browser. Clearing browser storage (or switching devices)
  loses saved setups/history — there's no cloud backup.

## License

No license file is attached — add whichever you prefer (MIT is a reasonable
default for something like this) before making the repo public.

## Turning it into an Android APK

Once it's live on GitHub Pages (or Cloudflare Pages), you can package it as a
real, installable `.apk` using **PWABuilder** (pwabuilder.com) — free, no coding,
no Android Studio.

1. Go to https://www.pwabuilder.com
2. Paste in your GitHub Pages URL and click "Start".
3. It'll detect `manifest.json` automatically — that's where the app's name,
   colors and icons come from (edit that file in the repo to rename it or point
   at different icon files).
4. Click "Package for stores" → Android. You can upload your own logo image
   right there in the wizard if you don't want the placeholder checkered-flag
   icon included in this repo (`icon-192.png` / `icon-512.png`) — any square
   image works, PWABuilder resizes it for you.
5. Download the generated `.apk`. Transfer it to your phone (email, USB, cloud
   drive — whatever's easiest) and tap it to install. Android will warn about
   "installing from unknown sources" the first time — that's normal for an APK
   that isn't from the Play Store, not a sign anything's wrong.
