# Leaf — offline-first PDF & comic PWA

A snappy, minimalistic PWA starter for reading PDFs and image-based comics. The interface is responsive and supports a library grid/list, collections, local file import, dark mode, page progress, bookmarks, swipe page turns, pinch zoom, keyboard navigation, and offline app-shell caching.

## Run locally

Service workers require `localhost` or HTTPS (not `file://`).

```bash
# Option A: Python
python -m http.server 8080

# Option B: Node
npx serve .
```

Open `http://localhost:8080` and install it from your browser's Install/Add to Home Screen option. For a production deployment, serve over HTTPS.

## What works in this starter

- **PDF reading:** PDF.js renders pages.
- **Image comics:** import JPG, PNG, WEBP, GIF, and other browser-supported image types as individual items.
- **Local-first library:** files are stored as Blobs in IndexedDB, so imported books stay available offline on that device.
- **Progress and bookmarks:** stored locally for each imported book.
- **Gestures:** swipe left/right to turn pages; pinch to zoom on touchscreens; Ctrl+wheel and keyboard controls on desktop.
- **Dark mode:** persisted locally.
- **PWA shell:** service worker caches the app shell for repeat/offline visits.

## Important limitations / next steps

1. **Cloud sync is not connected by default.** The “Cloud sync” button explains setup. Cross-device sync needs a backend and user sign-in. A production implementation should sync metadata/progress/bookmarks through Firebase Auth + Firestore and book files through Firebase Storage (or another private storage service). Never put service-account credentials in client code. Use Firebase web config only, restrict authorized domains, and configure Storage/Firestore security rules so each signed-in user can access only their own data.
2. **PDF.js is loaded from a CDN.** Open the app online once so the reader library loads. For dependable fully-offline PDF rendering, vendor the matching `pdf.min.mjs` and `pdf.worker.min.mjs` files into the app and update the imports in `app.js`. The app shell itself is cached by the service worker.
3. **Folder support is represented by collections/genres.** This keeps the interface lightweight. Rename/delete collection management can be added to suit your workflow.
4. **Large-file limits:** IndexedDB capacity varies by browser/device. Avoid clearing site data if you want to keep your local shelf.
5. **Page turning:** the current CSS transition is a lightweight book-like turn effect, not a physically accurate 3D paper simulation.

## Deploy

Upload the folder to any static host with HTTPS (for example, GitHub Pages, Netlify, or Cloudflare Pages). Ensure the host serves `app.js` as JavaScript and permits `.mjs` CDN modules. A root-relative deployment works best; for a subpath, preserve relative paths.

## Project layout

- `index.html` — app shell and screens
- `styles.css` — responsive UI, dark mode, page-turn animation
- `app.js` — IndexedDB, PDF.js rendering, gestures, bookmarks, collection/library behavior
- `sw.js` — service worker
- `manifest.webmanifest` — installable PWA metadata
- `icons/icon.svg` — app icon
