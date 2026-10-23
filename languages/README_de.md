# VKT Form — Formular-Snapshot & Auto-Fill

[English](../README.md) | [中文](README_zh.md) | [Español](README_es.md) | Deutsch | [日本語](README_ja.md) | [Français](README_fr.md)

Eine Browser-Erweiterung, die Web-Formular-Snapshots speichert und sie später automatisch ausfüllt. Daten bleiben standardmäßig lokal; Premium kann Snapshots optional über Ihr Chrome-/Edge-Konto zwischen Geräten synchronisieren.

## Installation

| Store | Link |
|---|---|
| Chrome Web Store | [In Chrome installieren](https://chromewebstore.google.com/detail/blghafafgccoglgpfmjmjnmlgdnmdonj) |
| Microsoft Edge Add-ons | [In Edge installieren](https://microsoftedge.microsoft.com/addons/detail/aejbpfaihkopjcpddgodddpoffpfoifd) |

> Installieren Sie nur über die offiziellen Stores oben – meiden Sie Download-Seiten Dritter.


## Funktionen

- **Ein-Klick-Formular-Erfassung** — Scannt und speichert alle Formularfelder auf jeder beliebigen Seite
- **Tiefe Felderkennung** — Inklusive Shadow DOM, iframes und Rich-Text-Editoren sowie der inaktiven Schritte eines mehrstufigen Formulars
- **Framework-bewusstes Auto-Fill** — Schreibt über den nativen Setter und die echte Bearbeitungspipeline des Browsers, sodass kontrollierte Komponenten von Vue / React / Angular ihren Zustand tatsächlich aktualisieren
- **Rücklese-Prüfung** — Jedes Feld wird nach dem Schreiben zurückgelesen; Fehlschläge werden gemeldet statt stillschweigend ignoriert
- **Feld-Kalibrierung** — Verknüpfen Sie ein Feld einmal mit einem Element auf der Seite, danach wird es immer ausgefüllt
- **Standardmäßig lokal** — Snapshots liegen in `chrome.storage.local`; nichts wird hochgeladen, solange Sie die Premium-Synchronisierung nicht aktivieren
- **Unbegrenzte Snapshots und Füllungen** — Keine Beschränkung in der kostenlosen Version
- **Export/Import** — JSON-Backup und Wiederherstellung, kostenlos für alle
- **Geräteübergreifende Synchronisierung (Premium)** — Optional über Ihr Chrome-/Edge-Konto; kein eigener Sync-Server

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

Gespeichert wird ein Snapshot unter dem Namen, den Sie vergeben; jeder Snapshot hält die normalisierte URL der Seite, und über sie wird eine Seite ihren Snapshots zugeordnet — dieselbe Seite kann also mehrere haben:
- Query-Strings und Fragmente werden entfernt
- Domain wird kleingeschrieben
- Abschließende Schrägstriche werden normalisiert
- Optional: Hash-Route für SPA-Apps beibehalten (Schalter in Einstellungen)

## Geräteübergreifende Synchronisierung (Premium)

Premium-Snapshots können über Ihr Chrome-/Edge-Konto zwischen Geräten synchronisiert werden, mithilfe des browserinternen `chrome.storage.sync`. Es gibt keinen Sync-Server von uns: Die Daten landen im Kontospeicher Ihres Browserherstellers – dort, wo auch Lesezeichen und Einstellungen liegen.

- Aktivieren Sie die Funktion unter **Einstellungen → Geräteübergreifende Synchronisierung**. Sie müssen in Chrome oder Edge angemeldet sein.
- Lokale Snapshots bleiben unbegrenzt und bearbeitbar, auch wenn die Synchronisierung aus ist.
- Das Kontingent legt der Browser fest (8 KB pro Element, insgesamt ca. 100 KB), daher werden große Snapshots in Blöcke aufgeteilt. Ein Snapshot, der nicht passt, bleibt lokal gespeichert, und das Panel informiert Sie.
- Löschungen werden ebenfalls synchronisiert: Ein bei aktiver Synchronisierung gelöschter Snapshot wird auch auf Ihren anderen Geräten entfernt.
- Sensible Werte werden im Klartext gespeichert und beim Synchronisieren wie jedes andere Feld übertragen.
- Das Ausschalten der Synchronisierung stoppt weitere Abgleiche, löscht aber keine Kopien, die bereits im Browserkonto liegen.

## Kostenlos vs. Premium

| | Kostenlos | Premium |
|---|:---:|:---:|
| Snapshots | Unbegrenzt | Unbegrenzt |
| Füllungen | Unbegrenzt | Unbegrenzt |
| Export / Import JSON | ✅ | ✅ |
| Geräteübergreifende Synchronisierung über Chrome-/Edge-Konto | — | ✅ |
| Priority Support | — | ✅ |

## Lizenz

Kostenlose Version: unbegrenzte lokale Snapshots, Füllungen, Kalibrierung sowie JSON-Export/-Import. Premium schaltet die geräteübergreifende Synchronisierung über Ihr Browserkonto und Priority Support frei.
