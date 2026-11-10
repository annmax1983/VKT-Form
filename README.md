# VKT Form - Form Snapshot & Auto-Fill

English | [中文](languages/README_zh.md) | [Español](languages/README_es.md) | [Deutsch](languages/README_de.md) | [日本語](languages/README_ja.md) | [Français](languages/README_fr.md)

A local-first browser extension that saves web form snapshots and auto-fills them later. Data stays in your browser; Pro can optionally sync snapshots between your devices through your browser account (Chrome, Edge or Firefox Sync). Runs on Chrome, Edge and Firefox.

## Highlights

- **Works on modern forms** — native setters, the browser's editing pipeline and component adapters keep React, Vue and Angular state in sync.
- **Goes deep** — Shadow DOM, iframes, rich-text editors and the hidden steps of multi-step forms are all covered.
- **Tells the truth** — every write is read back and verified; unmatched fields are reported, never filled into the wrong box.
- **Handles the long tail** — one-time field calibration for pages that heuristics cannot solve.
- **Local-first and unlimited** — no snapshot or fill cap; JSON export/import is free for everyone.
- **Optional sync** — Pro can keep devices in step through your browser account (Chrome / Edge / Firefox Sync), with no sync server of ours.

## Installation

| Store | Link |
|---|---|
| Chrome Web Store | [Install for Chrome](https://chromewebstore.google.com/detail/blghafafgccoglgpfmjmjnmlgdnmdonj) |
| Microsoft Edge Add-ons | [Install for Edge](https://microsoftedge.microsoft.com/addons/detail/aejbpfaihkopjcpddgodddpoffpfoifd) |
| Firefox Add-ons (AMO) | [Install for Firefox](https://addons.mozilla.org/firefox/addon/vkt-form-fill/) |

> Install only from the official stores above — avoid third-party download sites.

## Supported Browsers

| Browser | Status | Notes |
|---------|--------|-------|
| Google Chrome | ✅ Fully supported | Side panel UI, Manifest V3 |
| Microsoft Edge | ✅ Fully supported | Side panel UI, Manifest V3 |
| Mozilla Firefox | ✅ Fully supported | Firefox sidebar UI, Manifest V3, Firefox 140+ (desktop) |

> **Firefox differences:**
> - The panel opens in Firefox's **Sidebar** (View → Sidebar → VKT Form) instead of Chromium's side panel.
> - Firefox has no debugger API for extensions, so **debugger mode** is not offered there; the other four filling strategies — native setter + events, commit triggers, the real editing pipeline and component adapters — work exactly as on Chrome and Edge.
> - Firefox asks for `storage`, `activeTab`, `scripting` and `<all_urls>`. The `sidePanel` and `debugger` permissions are Chrome / Edge only.

## Features

- **One-click form collection** — Scan and save the form fields on any page (password fields only if you opt in)
- **Framework-aware auto-fill** — Writes through the native value setter and the browser's own editing pipeline, so Vue / React / Angular controlled components actually update their state
- **Deep field detection** — Shadow DOM, iframes and rich-text editors included; the inactive steps of a multi-step form are captured too
- **Read-back verification** — Every field is verified after writing; failures are reported instead of silently ignored
- **Field calibration** — Bind a field to an element on the page once and it fills forever after
- **Local-first** — Snapshots live in the extension's local storage (`storage.local`); nothing is uploaded unless you turn on Pro cross-device sync
- **Unlimited snapshots and fills** — No free-tier cap
- **Export/Import** — JSON backup and restore, free for everyone
- **Cross-device sync (Pro)** — Optional sync through your browser account (Chrome / Edge / Firefox Sync); no sync server of ours
- **6 languages** — English, 中文, 日本語, Deutsch, Español, Français; auto-detected from the browser

## Usage

1. **Collect** — open the panel on a page with a form (Chrome / Edge: side panel from the toolbar icon; Firefox: sidebar via **View → Sidebar → VKT Form**) and click **Collect**.
2. **Fill** — click **Fill** to match the current page, or **Fill this** on a saved snapshot.
3. **Calibrate if needed** — open the snapshot's 🎯 panel, click **Bind**, then click the field on the page. The binding is saved with the snapshot and always takes priority.
4. **Back up** — use the export/import icons to save or restore all snapshots as JSON.
5. **Sync (Pro)** — open **Settings → Cross-device sync**, sign in to your browser account (Chrome, Edge or Firefox), and turn it on. Use **Sync now** for an immediate refresh.

## Field Matching

Every stored field is scored against every field on the page, and the best candidate above a confidence threshold wins. Anything below the threshold is reported as a miss rather than written into the wrong box.

Signals, roughly in order of weight:

- `name`, `id` and `autocomplete` attributes
- Label text, `aria-label`, wrapping `<label>`, placeholder, nearby text
- Semantic token (`username`, `phone`, `email`, `address`, …) in Chinese and English
- Structural path, and row/column position inside repeating containers such as table rows
- DOM order, as a last resort

Because matching is descriptor-based rather than position-based, a snapshot still fills after the site renames its fields, reorders the form, or serves it from a different URL.

## Filling

Each field is written with escalating strategies, and the value is read back after every attempt:

1. **Native setter + events** — writes through `HTMLInputElement.prototype`'s setter and dispatches `beforeinput` / `input` / `change`. Going through the prototype is what makes React's change tracking fire at all.
2. **Commit triggers** — `blur` / `focusout` for widgets that only save on blur.
3. **Real editing pipeline** — `document.execCommand('insertText')` after focusing and selecting, which produces events indistinguishable from typing. Also how rich-text editors are filled.
4. **Component adapter** — for div-based selects (Element Plus, Ant Design, Arco, Naive UI, Vant, …) it opens the dropdown like a user would and clicks the option carrying the stored value.
5. **Debugger mode** — Chrome / Edge only, off by default; it drives the browser's debugger to emit trusted input events for components that reject everything else. Chrome does not allow this permission to be requested at runtime, so it is granted at install — but it is not used until you turn the mode on, and it disconnects as soon as the fill finishes (the browser shows a debugging banner while it is attached). Firefox has no extension debugger API, so that strategy is not offered in the Firefox build.

Fields that are still missing after the first pass are retried for a few seconds, so forms that render late or appear conditionally get filled too.

## Field Calibration

Heuristics cover most pages; for the rest there is calibration. Open a snapshot's 🎯 panel, choose a field, click **Bind**, then click that input on the page. The binding is stored inside the snapshot and always takes priority — whatever the site changes afterwards.

## URL Normalization

Snapshots are keyed by the name you give them, and each one stores the page's normalized URL — that URL is what ties a page back to its snapshots, so a single page can hold more than one:
- Query strings and fragments removed
- Domain lowercased
- Trailing slashes normalized
- Optional: keep hash route for SPA apps (toggle in Settings)

## Cross-device Sync (Pro)

Pro snapshots can sync between devices through your browser account, using the browser's built-in sync storage (`storage.sync`). There is no sync server of ours: the data goes to your browser vendor's account sync (Chrome Sync, Edge Sync or Firefox Sync), the same place bookmarks and settings go.

- Turn it on in **Settings → Cross-device sync**. You need to be signed in to your browser account (Chrome, Edge or Firefox).
- Local snapshots stay unlimited and editable with sync off.
- The browser sets the quota (8 KB per item, ~100 KB total), so large snapshots are split into chunks. A snapshot that does not fit stays saved locally and the panel tells you.
- Deletions sync too: a snapshot deleted while sync is on is removed from your other devices.
- Password fields are not collected unless you turn on **Settings → Include password fields**. Everything that is collected is stored in plain text and synced like any other field.
- Turning sync off stops further syncing but does not erase copies already in your browser account.

## Free vs Pro

| | Free | Pro |
|---|:---:|:---:|
| Snapshots | Unlimited | Unlimited |
| Fills | Unlimited | Unlimited |
| Export / Import JSON | ✅ | ✅ |
| Cross-device sync via your browser account (Chrome / Edge / Firefox) | — | ✅ |
| Priority support | — | ✅ |

---

## Source Code Notice

> ⚠️ **This repository is for project showcase only — the full source code is not published here.** It contains the user documentation, release notes and support information for the extension.

> 📥 **Download and install only from the official browser stores:** Chrome Web Store · Microsoft Edge Add-ons · Firefox Add-ons (AMO). No offline installation package is distributed from this repository.

---

## License

This software is proprietary and closed-source. Without prior written authorization from the author, decompiling, reverse-engineering, cracking, modifying, repackaging, redistributing or reselling the program (in whole or in part) is strictly prohibited.

Free version: unlimited local snapshots, fills, calibration, and JSON export/import. Pro unlocks cross-device sync through your browser account and priority support.

---

## Links

- **Official website:** https://annmax1983.com/
- **This extension:** https://annmax1983.com/extensions/vkt-form
- **Privacy policy:** https://annmax1983.com/privacy
- **Contact:** support@annmax1983.com
- **☕ Support this project (Ko-fi):** https://ko-fi.com/annmax?ref=vkt-form
