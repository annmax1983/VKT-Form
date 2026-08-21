# vkt-form
[English](../README.md) | [中文](README_zh.md) | [Español](README_es.md) | Deutsch | [日本語](README_ja.md) | [Français](README_fr.md)

Browser-Formular-Snapshot-Erweiterung — Ein-Klick-Speicherung, Ein-Klick-Ausfüllung. Alle Daten lokal im Browser gespeichert.

> Chromium-basiert · Manifest V3 · Kein Tracking · Benutzergesteuerte Erfassung

---

## Warum vkt-form?

Jedes Mal dieselben Formulare auszufüllen ist mühsam. Mit vkt-form einmal speichern und jederzeit mit einem Klick ausfüllen.

| Vorteil | Details |
|---------|---------|
| 🔒 **Datenschutz** | Alle Daten lokal im Browser. Kein Server, kein Upload, kein Tracking. |
| ⚡ **Ein-Klick-Bedienung** | „Erfassen" zum Speichern, „Ausfüllen" zum Wiederherstellen. |
| 🧠 **Intelligente Zuordnung** | Name-Attribut priorisiert, DOM-Reihenfolge als Fallback, Vue/React-kompatibel. |
| 💾 **Datenverlust verhindern** | JSON-Export/Import-Backup. |
| 🆓 **Kostenlos nutzbar** | 5 Snapshots, 20 Ausfüllungen/Tag. |
| 🌍 **6 Sprachen** | Automatische Browser-Spracherkennung. |

---

## Funktionen

| Funktion | Beschreibung |
|----------|-------------|
| 📋 **Formularerfassung** | Button-gesteuert, scannt alle `input/select/textarea` Elemente der Seite. |
| ⚡ **Intelligentes Ausfüllen** | Priorität: `name`-Attribut, Fallback: `domIndex + tagName + type`. |
| 🔄 **Framework-kompatibel** | `input`, `change`, `click` Events, Vue/React-kompatibel. |
| 📊 **Kontingentverwaltung** | Kostenlos: 5 Snapshots, 20/Tag. Premium: unbegrenzt. |
| 🔑 **Lizenzschlüssel** | Eingabe im Einstellungsbereich für Premium-Aktivierung. |
| 📥📤 **Import/Export** | JSON-Backup und Wiederherstellung. |
| 🌐 **URL-Normalisierung** | Query/Fragment entfernt, Domain kleingeschrieben, Hash-Route optional (SPA). |
| 🌍 **Mehrsprachig** | English, 中文, 日本語, Español, Deutsch, Français. |

---

## Unterstützte Browser

| Browser | Status |
|---------|--------|
| Google Chrome | ✅ Vollständig |
| Microsoft Edge | ✅ Vollständig |
| Brave | ✅ Unterstützt |
| Opera | ✅ Unterstützt |
| Vivaldi | ✅ Unterstützt |
| Chromium-basierte Browser | ✅ Unterstützt (Manifest V3) |

---

## Installation

### Entwicklermodus

1. Erweiterungsseite öffnen:
   - **Chrome**: `chrome://extensions/`
   - **Edge**: `edge://extensions/`
2. **Entwicklermodus** aktivieren (oben rechts)
3. **Entpackte Erweiterung laden** → `vkt-form`-Ordner wählen
4. Symbol in der Toolbar erscheint

---

## Verwendung

### Formular-Snapshot speichern

1. Seite mit Formular besuchen
2. Symbol in der Toolbar klicken
3. **🔄 Erfassen** klicken
4. Formularfelder werden gescannt und gespeichert

### Automatisches Ausfüllen

**Methode A: Aktuelle Seite abgleichen**
1. Zur gespeicherten Seite zurückkehren
2. **⚡ Ausfüllen** klicken
3. Passender Snapshot wird automatisch ausgefüllt

**Methode B: Bestimmten Snapshot ausfüllen**
1. Gewünschten Eintrag in der Liste finden
2. **Diese füllen** klicken
3. Erzwungenes Ausfüllen mit diesem Snapshot

---

## Datenschutz

- ✅ **Kein Daten-Upload** — `chrome.storage.local` Speicherung
- ✅ **Manuell ausgelöst** — Kein automatisches Scannen
- ✅ **Kein Tracking** — Keine Telemetrie, keine Remote-Aufrufe
- ✅ **Minimale Berechtigungen** — Nur `storage` und `activeTab`

---

## Berechtigungen

| Berechtigung | Zweck |
|--------------|-------|
| `storage` | Lokale Speicherung von Snapshots und Einstellungen |
| `activeTab` | Zugriff auf aktuellen Tab nur bei Button-Klick |

---

---

## Hinweis zum Quellcode

> ⚠️ **Dieses Repository veröffentlicht keinen Quellcode.** Es enthält nur Nutzerdokumentation, Versionshinweise und Support-Ressourcen. Die Erweiterung wird ausschließlich über den Chrome Web Store vertrieben. Es werden keine Offline-Installationspakete oder Quellcodes für Endbenutzer bereitgestellt.


## ❤️ Unterstützung

Wenn vkt-form Ihnen hilft, unterstützen Sie uns gerne!

**[👉 vkt-form unterstützen](https://annmax1983.github.io/vkt-form/)**
