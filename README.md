# Link Grabber — for Firefox & Chrome

> Extract, filter, and copy every link on any page in seconds. Works on both Firefox and Chrome.

Ever landed on a page packed with download links, media files, or references and thought *"I just want all of these URLs, right now"*? Link Grabber does exactly that. One click opens a popup with every link on the page, grouped by domain, ready to filter and copy — no fuss, no tracking, no cloud.

---

## Features at a glance

| Feature | Details |
|---|---|
| **Deep link scanning** | Captures anchors plus media/embed sources — `img`, `video`, `audio`, `iframe`, `script`, `link`, `source`, `track` |
| **Smart filtering** | Plain text, multi-term (`1080p+mp4`), or full regex (`/pattern/i`) |
| **Domain grouping** | Results grouped by hostname; click a domain to isolate it, click again to reset |
| **One-click copy** | Click any row to copy that URL, or hit **Copy All** to grab everything filtered |
| **Live highlighting** | Hover a link in the popup and it lights up orange on the actual page |
| **Per-site defaults** | Save a default search per site — the filter pre-fills automatically when you land there |
| **Theme sync** | Dark/light preference stored locally and shared across popup and settings page |

---

## Installation

### Firefox
1. Download the latest `.xpi` from the [Releases page](https://github.com/jt-ito/enhanced-link-grabber-for-chrome-and-firefox/releases).
2. Open Firefox → `about:addons` → click the gear icon → **Install Add-on From File** → pick the `.xpi`.

Or load temporarily for development:
1. Go to `about:debugging#/runtime/this-firefox`.
2. Click **Load Temporary Add-on** and select `manifest.json` from the Firefox folder.

### Chrome
1. Download the latest `link_grabber_chrome_vX.X.zip` from the [Releases page](https://github.com/jt-ito/enhanced-link-grabber-for-chrome-and-firefox/releases) and unzip it.
2. Go to `chrome://extensions/`, enable **Developer mode**, click **Load unpacked**, and select the unzipped folder.

---

## Using the extension

Click the toolbar icon to open the popup. From there:

**Filtering**
- Type anything to do a plain contains match — e.g. `mp4` shows only links with "mp4" in the URL.
- Use `+` to require multiple terms — e.g. `1080p+60fps` shows links containing both.
- Wrap in `/slashes/` for a regex — e.g. `/\.mkv$/i` matches `.mkv` links case-insensitively.
- Hit **×** to clear the filter instantly.

**Domain focus**
- Click any domain header to show only links from that domain.
- Click it again to go back to all results.

**Copying**
- Click any link row to copy that single URL to your clipboard.
- Click **Copy All** to copy every currently visible (filtered + domain-focused) link, one per line.

**Theme**
- Click **Toggle Dark** to switch between dark and light mode. Your preference is saved automatically.

**Settings**
- Click **Settings** to open the settings page.

---

## Per-site default searches

If you always look for the same thing on a particular site — say, `mp4` on a video archive, or `/episode/` on a media site — you can save that as a default:

1. Open **Settings** from the popup.
2. Enter the site (e.g. `example.com`) and your default filter (e.g. `mp4+1080p` or `/\.mkv$/`).
3. Click **Save / Update**.

Next time you open the popup on that site with an empty filter, it auto-fills for you. You can save as many site/filter pairs as you like and delete them any time. Everything is stored locally — nothing leaves your device.

---

## Privacy & data collection

This extension collects **nothing**. No analytics, no telemetry, no external requests of any kind.

- Declared in `manifest.json` under `browser_specific_settings.gecko.data_collection_permissions` as `required: ["none"]`.
- All stored data (default searches, theme preference) lives in `browser.storage.local` / `chrome.storage.local` on your own device.
- Permissions requested and why:

| Permission | Why |
|---|---|
| `activeTab` | Read the current page to collect links |
| `tabs` | Query the active tab URL for per-site defaults |
| `storage` | Save your settings and theme preference locally |
| `clipboardWrite` | Copy links to your clipboard |

---

## How it works (for the curious)

- **`js/contentscript.js`** — injected into every page. Listens for three messages: `getLinks` (returns all collected URLs), `highlight` (outlines a specific anchor in orange), and `clearHighlight` (removes the outline).
- **`js/popup.js`** — drives the popup UI. Loads your theme and per-site default on open, renders grouped/filtered links, handles copy actions, and forwards hover events to the content script.
- **`js/options.js`** — powers the settings page. Reads and writes per-site default search entries and the active theme to local storage.
- **`background.js`** — minimal listener; handles install logging. Firefox uses a persistent background script; Chrome uses a MV3 service worker.

---

## Project layout

```
Firefox extension (Manifest V2)
├─ manifest.json
├─ background.js
├─ popup.html
├─ options.html
├─ js/
│  ├─ contentscript.js
│  ├─ popup.js
│  └─ options.js
└─ icon_48/96/128.png

link_grabber_chrome_v1_3_3/   ← Chrome extension (Manifest V3)
├─ manifest.json
├─ background.js               ← service worker
├─ popup.html
├─ options.html
├─ js/
│  ├─ contentscript.js
│  ├─ popup.js
│  └─ options.js
└─ icon_48/96/128.png
```

---

## Development tips

- **Quick popup reload** — reopen the popup after editing JS/HTML; no full extension reload needed.
- **Content script changes** — refresh the target tab after saving `contentscript.js`.
- **Firefox auto-reload** — install `web-ext` globally (`npm install -g web-ext`) and run `web-ext run` from the Firefox folder for a dedicated profile with live reload.
- **Build for AMO** — `web-ext build` produces a ready-to-submit ZIP in `web-ext-artifacts/`.
- **Chrome packaging** — zip the contents of `link_grabber_chrome_v1_3_3/` and upload to the Chrome Web Store developer dashboard.

---

## License

MIT — see `LICENSE`.
