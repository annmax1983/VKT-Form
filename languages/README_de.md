# VKT Form — Formular-Snapshot & Auto-Fill

[English](../README.md) | [中文](README_zh.md) | [Español](README_es.md) | Deutsch | [日本語](README_ja.md) | [Français](README_fr.md)

Eine Browser-Erweiterung, die Web-Formular-Snapshots speichert und sie später automatisch ausfüllt. Alle Daten lokal gespeichert, kein Cloud-Upload.

## Funktionen

- **Ein-Klick-Formular-Erfassung** — Scannt und speichert alle Formularfelder auf jeder beliebigen Seite
- **Tiefe Felderkennung** — Inklusive Shadow DOM, iframes und Rich-Text-Editoren sowie der inaktiven Schritte eines mehrstufigen Formulars
- **Framework-bewusstes Auto-Fill** — Schreibt über den nativen Setter und die echte Bearbeitungspipeline des Browsers, sodass kontrollierte Komponenten von Vue / React / Angular ihren Zustand tatsächlich aktualisieren
- **Rücklese-Prüfung** — Jedes Feld wird nach dem Schreiben zurückgelesen; Fehlschläge werden gemeldet statt stillschweigend ignoriert
- **Feld-Kalibrierung** — Verknüpfen Sie ein Feld einmal mit einem Element auf der Seite, danach wird es immer ausgefüllt
- **Vollständig lokal** — Alle Daten in `chrome.storage.local` gespeichert, nie hochgeladen
- **Export/Import** — JSON-Backup und Wiederherstellung (Premium)
- **Kostenlose Stufe** — 5 Snapshots; Premium hebt alle Beschränkungen auf

## So funktioniert es

1. Besuche eine beliebige Seite mit Formularen, klicke auf **Erfassen** zum Scannen und Speichern
2. Kehre später zur Seite zurück, klicke auf **Füllen**, um alle Felder automatisch auszufüllen
3. Verwalte Snapshots im Seitenpanel (bestimmten füllen, aktualisieren, kalibrieren, löschen)

## Feld-Matching

Jedes gespeicherte Feld wird gegen alle Felder der Seite bewertet; gewinnt der beste Kandidat über der Konfidenzschwelle. Alles darunter wird als „nicht gefunden" gemeldet, statt in das falsche Feld geschrieben zu werden.

Signale, ungefähr nach Gewicht:

- Attribute `name`, `id` und `autocomplete`
- Label-Text, `aria-label`, umschließendes `<label>`, Placeholder, umgebender Text
- Semantisches Token (`username`, `phone`, `email`, `address`, …) auf Chinesisch und Englisch
- Struktureller Pfad und Zeilen-/Spaltenposition in wiederkehrenden Containern (Tabellenzeilen)
- DOM-Reihenfolge, als letzter Ausweg

Da das Matching auf Deskriptoren statt auf Positionen basiert, funktioniert ein Snapshot weiterhin, wenn die Seite Feldnamen ändert, das Formular umsortiert oder es unter einer anderen URL ausliefert.

## Füllen

Jedes Feld wird mit eskalierenden Strategien geschrieben und nach jedem Versuch zurückgelesen:

1. **Nativer Setter + Events** — schreibt über den Setter von `HTMLInputElement.prototype` und löst `beforeinput` / `input` / `change` aus. Der Weg über das Prototyp ist der Grund, warum Reacts Änderungsverfolgung überhaupt anspringt.
2. **Commit-Auslöser** — `blur` / `focusout` für Komponenten, die nur beim Verlassen des Feldes speichern.
3. **Echte Bearbeitungspipeline** — `document.execCommand('insertText')` nach Fokus und Auswahl; die erzeugten Events sind von echter Tastatureingabe nicht zu unterscheiden. So werden auch Rich-Text-Editoren gefüllt.
4. **Komponenten-Adapter** — für div-basierte Selects (Element Plus, Ant Design, Arco, Naive UI, Vant, …) öffnet er die Liste wie ein Benutzer und klickt die Option mit dem gespeicherten Wert an.
5. **Debugger-Modus** — standardmäßig aus; er nutzt den Browser-Debugger, um vertrauenswürdige Eingabe-Events für Komponenten zu erzeugen, die alles andere ablehnen. Chrome erlaubt diese Berechtigung nicht zur Laufzeit, daher wird sie bei der Installation erteilt – verwendet wird sie erst, wenn Sie den Modus aktivieren, und danach sofort getrennt (währenddessen zeigt der Browser ein Debug-Banner).

Felder, die danach noch fehlen, werden einige Sekunden lang erneut versucht – so werden auch spät gerenderte oder bedingt eingeblendete Formulare gefüllt.

## Feld-Kalibrierung

Heuristiken decken die meisten Seiten ab; für den Rest gibt es die Kalibrierung. Öffnen Sie das 🎯-Panel eines Snapshots, wählen Sie ein Feld, klicken Sie auf **Verknüpfen** und dann auf das entsprechende Feld auf der Seite. Die Verknüpfung wird im Snapshot gespeichert und hat immer Vorrang – egal, was die Seite danach ändert.

## URL-Normalisierung

Snapshots werden nach normalisierter URL gespeichert:
- Query-Strings und Fragmente werden entfernt
- Domain wird kleingeschrieben
- Abschließende Schrägstriche werden normalisiert
- Optional: Hash-Route für SPA-Apps beibehalten (Schalter in Einstellungen)

## Build

```bash
npm install
npm run build
```

Output: `publish/vkt-form-v{version}.zip`

## Kostenlos vs. Premium

| | Kostenlos | Premium |
|---|:---:|:---:|
| Snapshots | max. 5 | Unbegrenzt |
| Füllungen | Unbegrenzt | Unbegrenzt |
| Export / Import JSON | — | ✅ |
| Priority Support | — | ✅ |

## Lizenz

Kostenlose Version: 5 Snapshots. Premium-Schlüssel schaltet unbegrenzte Nutzung, JSON-Export/-Import und Priority Support frei.
