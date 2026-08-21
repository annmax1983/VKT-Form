# vkt-form - Form Snapshot & Auto-Fill

English | [中文](languages/README_zh.md) | [Español](languages/README_es.md) | [Deutsch](languages/README_de.md) | [日本語](languages/README_ja.md) | [Français](languages/README_fr.md)

A browser extension that saves web form snapshots and auto-fills them later. All data stored locally, no cloud upload.

## Features

- **One-click form collection** — Scan and save all form fields on any page
- **Smart auto-fill** — Match by `name` attribute (primary) with DOM order fallback
- **Framework compatible** — Works with Vue, React, and other SPA frameworks
- **Fully local** — All data stored in `chrome.storage.local`, never uploaded
- **Export/Import** — JSON backup and restore
- **Free tier** — 5 snapshots, 20 fills/day; Premium removes all limits

## How It Works

1. Visit any page with forms, click **Collect** to scan and save
2. Return to the page later, click **Fill** to auto-fill all fields
3. Manage snapshots in the popup list (fill specific, update, delete)

## URL Normalization

Snapshots are keyed by normalized URL:
- Query strings and fragments removed
- Domain lowercased
- Trailing slashes normalized
- Optional: keep hash route for SPA apps (toggle in Settings)

## Field Matching Strategy

1. **Primary**: Match by `name` attribute (stable across page changes)
2. **Fallback**: Match by DOM order (`domIndex`) + `tagName` + `type` for fields without `name`

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
| Fills per day | 20 | Unlimited |
| Export / Import JSON | — | ✅ |
| Priority support | — | ✅ |

## License

Free version: 5 snapshots, 20 fills/day. Premium key unlocks unlimited usage, JSON export/import, and priority support.
