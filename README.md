# VKT Form - Form Snapshot & Auto-Fill

English | [中文](languages/README_zh.md) | [Español](languages/README_es.md) | [Deutsch](languages/README_de.md) | [日本語](languages/README_ja.md) | [Français](languages/README_fr.md)

A browser extension that saves web form snapshots and auto-fills them later. All data stored locally, no cloud upload.

## Features

- **One-click form collection** — Scan and save all form fields on any page
- **Framework-aware auto-fill** — Writes through the native value setter and the browser's own editing pipeline, so Vue / React / Angular controlled components actually update their state
- **Deep field detection** — Shadow DOM, iframes and rich-text editors included; the inactive steps of a multi-step form are captured too
- **Read-back verification** — Every field is verified after writing; failures are reported instead of silently ignored
- **Field calibration** — Bind a field to an element on the page once and it fills forever after
- **Fully local** — All data stored in `chrome.storage.local`, never uploaded
- **Export/Import** — JSON backup and restore (Premium)
- **6 languages** — English, 中文, 日本語, Deutsch, Español, Français; auto-detected from the browser
- **Free tier** — 5 snapshots, unlimited fills; Premium removes the snapshot cap and adds JSON backup & restore

## How It Works

1. Visit any page with forms, click **Collect** to scan and save
2. Return to the page later, click **Fill** to auto-fill all fields
3. Manage snapshots in the side panel list (fill a specific one, update, calibrate, delete)

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
5. **Debugger mode** — off by default; it drives the browser's debugger to emit trusted input events for components that reject everything else. Chrome does not allow this permission to be requested at runtime, so it is granted at install — but it is not used until you turn the mode on, and it disconnects as soon as the fill finishes (the browser shows a debugging banner while it is attached).

Fields that are still missing after the first pass are retried for a few seconds, so forms that render late or appear conditionally get filled too.

## Field Calibration

Heuristics cover most pages; for the rest there is calibration. Open a snapshot's 🎯 panel, choose a field, click **Bind**, then click that input on the page. The binding is stored inside the snapshot and always takes priority — whatever the site changes afterwards.

## URL Normalization

Snapshots are indexed by normalized URL:
- Query strings and fragments removed
- Domain lowercased
- Trailing slashes normalized
- Optional: keep hash route for SPA apps (toggle in Settings)

## Build

```bash
npm install
npm run build
```

Output: `publish/vkt-form-v{version}.zip`

## Free vs Premium

| | Free | Premium |
|---|:---:|:---:|
| Snapshots | 5 max | Unlimited |
| Fills | Unlimited | Unlimited |
| Export / Import JSON | — | ✅ |
| Priority support | — | ✅ |

## License

Free version: 5 snapshots with unlimited fills. Premium key unlocks unlimited snapshots, JSON export/import, and priority support.
