# SYS.PPL Workout Tracker

Personal PPL (Push/Pull/Legs) workout logger. Offline-capable PWA hosted on GitHub Pages, sideload-friendly for Android.

**Live URL (once deployed):** `https://steveparakal.github.io/ppl-tracker/`

---

## Files in this repo

| File | Purpose |
|------|---------|
| `index.html` | The whole app — HTML + CSS + JS in one file |
| `manifest.json` | PWA config — enables "Install app" / "Add to Home screen" |
| `sw.js` | Service worker — offline support after first load |
| `icon.svg` | Vector app icon |
| `icon-192.png`, `icon-512.png` | Raster icons for Android home screen |
| `README.md` | This file |

---

## Deployment (one-time, ~10 min)

### 1. Create the GitHub repo
- Go to https://github.com/new
- Repository name: **`ppl-tracker`** (exactly this — the URL depends on it)
- Public (required for free GitHub Pages)
- **Do NOT** check "Add a README file", "Add .gitignore", or "Choose a license" — leave all boxes unchecked
- Click **Create repository**

### 2. Upload the files
On the empty repo page:
- Click **"uploading an existing file"** (link in the middle of the page)
- Drag all 6 files (everything in this folder) into the drop zone
- Commit message: `initial deploy` (or anything)
- Click **Commit changes**

### 3. Enable GitHub Pages
- In your repo, go to **Settings** (top nav)
- Left sidebar: **Pages**
- Under **Source**, select **Deploy from a branch**
- Branch: **`main`**, Folder: **`/ (root)`**
- Click **Save**
- Wait ~30–60 seconds. Refresh the Pages settings page. You'll see:
  > Your site is live at https://steveparakal.github.io/ppl-tracker/

### 4. Install on your phone
- Open the URL in **Chrome on Android** (not Firefox — File System Access API needs Chrome)
- Chrome menu (⋮) → **Install app** or **Add to Home screen**
- Icon appears on your home screen — tap it to launch as standalone (no browser chrome)

### 5. Link your backup file
- Open the app from the home-screen icon
- Tap **ENTER**
- Bottom nav → **db**
- Scroll to `// AUTO_BACKUP` section → **SET BACKUP FILE**
- Pick a location — I'd suggest `/Documents/sys_ppl_backup.json` or `/Download/`
- Confirm — done. Every session save auto-writes to this file.

---

## Updating the app later

Any time you (or I) change the code:
1. Bump `CACHE_NAME` in `sw.js` (e.g. `v1` → `v2`) so the service worker picks up new assets
2. Upload changed files via the repo's **Add file → Upload files**
3. Wait ~30s for Pages to redeploy
4. Refresh the app on your phone — the SW will fetch the new version

---

## Backup strategy

- **Auto-backup** writes to your chosen file after every session save (debounced 2s)
- **Manual export** available anytime via DB → `EXPORT_JSON`
- **Restore** via DB → `IMPORT_JSON` (two-tap confirmation)
- **Highly recommended:** once a month, copy the backup file to Google Drive or email it to yourself. Belt and suspenders.

Things that can wipe browser storage (and why auto-backup matters):
- Clearing Chrome data with "Cookies and site data" checked
- Chrome Settings → Site settings → your app URL → Clear & reset
- Renaming the repo (changes URL = changes origin = fresh storage)
- Adding a custom domain (same problem)

---

## Troubleshooting

**"Add to Home screen" option doesn't appear**
- Make sure you're on Chrome, not another browser
- Reload the page once — the service worker needs a first successful load to register
- Check DevTools → Application → Manifest for validation errors

**AUTO_BACKUP shows "NOT SUPPORTED"**
- You're on a browser without File System Access API (Firefox, iOS Safari, older Chrome)
- Use manual `EXPORT_JSON` regularly instead

**AUTO_BACKUP shows "RECONNECT NEEDED"**
- Browser dropped file permission. Tap **RECONNECT** in the DB screen to re-authorize.

**App won't work offline**
- Load it online once — the service worker caches everything on first visit
- Check Chrome's DevTools → Application → Service Workers to confirm SW is `activated`

**Icon looks pixelated on home screen**
- Different Android launchers scale icons differently. The 512px PNG should look sharp on most.
- If it looks bad, we can regenerate at higher resolutions.

**"I lost all my data"**
- If you have your backup file: DB → `IMPORT_JSON` → pick the file → confirm.
- If you don't: unfortunately, no recovery. This is why the backup file matters.

---

## Tech notes

- Pure HTML/CSS/JS — no build step, no dependencies to install
- Storage: `localStorage` for data, `IndexedDB` for the File System Access handle
- Charts: Chart.js 4.4.0 via CDN (cached by service worker)
- Icons: Tabler Icons via CDN (cached by service worker)
- Fonts: Google Fonts (Share Tech Mono, Oswald, Bungee) — cached

---

## Aesthetic credits

DedSec / Watch Dogs 2 zine aesthetic — halftones, torn paper, spray-paint, glitch. All original code and SVG artwork; no Ubisoft assets used.
