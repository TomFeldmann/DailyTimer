## Daily Timer
Eine kleine, statische Daily-Seite für Teams: zufällige Reihenfolge, Timer, Redeobjekt, und einfache Verwaltung der Teilnehmenden direkt im Browser.

Die Seite besteht nur aus HTML, CSS und JavaScript. Es ist kein Build-Step, kein Backend und keine Datenbank nötig.

### Features
- Großer Countdown-Timer für das Daily
- Zufälliger Reihenfolge aller aktiven Teilnehmenden
- Namen direkt über die Seite hinzufügen oder entfernen
- Teilnehmende per Checkbox aktivieren oder deaktivieren
- Namen werden lokal im Browser gespeichert
- Pause, Reset, Skip und Weiter
- Manuelles Starten einer bestimmten Person über den Play-Button
- Bereits erledigte Personen werden markiert und kommen in derselben Runde nicht erneut dran
- Zufälliges Redeobjekt pro Daily-Start aus objects.json
- Dunkles Design
- Blaues und Gelbes Design

### Dateien
.
├── index.html
└── objects.json

index.html enthält die komplette Daily-Seite.
objects.json enthält die möglichen Redeobjekte. Die Datei muss im selben Verzeichnis wie index.html liegen.

### Nutzung
1. Repo klonen / downloaden
2. index.html und objects.json im selben Ordner ablegen
3. Die Seite über den Webserver öffnen (funktioniert auch lokal)

### Namen Speichern
Die Namen werden im localStorage im Browser gespeichert.
Das bedeutet:
- Die Namen bleiben nach dem Neuladen erhalten.
- Die Namen sind nur auf diesem Gerät und in diesem Browser gespeichert.
- Es werden keine Namen an einen Server geschickt
- Wenn Browserdaten gelöscht werden, können die Namen verschwinden

### Redeobjekte konfigurieren
Die Redeobjekte stehen in objects.json

Beispiel:
```
[ 
    { "artikel": "Den", "name": "Stab" }, 
    { "artikel": "Die", "name": "Tastatur" }, 
    { "artikel": "Das", "name": "Kabel" }
]
```
Daraus wird auf der Seite zum Beispiel: 

Den Rede-Stab hat:<br>Max

oder:

Das Rede-Kabel hat:<br>Anna

Bei jedem Start über GO wird ein Redeobject zufällig ausgewählt. Das Objekt bleibt für die laufende Runde gleich.

### Timerdauer ändern
Die Dauer ist in index.html im JavaScript-Bereich festgelegt:
```
const DURATION_SECONDS = 3 * 60;
```
Beispiele:
```
const DURATION_SECONDS = 4 * 60; // 4 Minuten
const DURATION_SECONDS = 5 * 60; // 5 Minuten
const DURATION_SECONDS = 90;     // 90 Sekunden
```
Die Warnzeit ist dort ebenfalls definiert:
```
const WARNING_SECONDS = 30;
```
Ab diesem Wert wechselt der Timer in die Warnfarbe.

### Bedienung
#### Go
Startet eine neue Daily-Runde.<br>Dabei werden alle aktivierten Teilnehmenden zufällig gemischt. Deaktivierte Teilnehmende werden nicht berücksichtigt.
#### Pause
Pausiert oder setzt den aktuellen Timer fort.
#### Skip
Überspringt die aktuelle Person sofort. Die Person wird als erledigt markiert und kommt in dieser Runde nicht erneut dran.
#### Weiter
Erscheint nach Ablauf des Timers. Erst wenn Weiter geklcikt wird, startet die nächste Person.
#### Reset 
Setzt die aktuelle Runde zurück.
#### Play Button (neben einem Namen)
Starte diese Person direkt.<br>Wenn gerade eine andere Person läuft, wird diese aktuelle Person alsa erledigt markiert und die ausgewählte Person startet sofort.

### Daten und Datenschutz
Diese Daily-Seite läuft komplett im Browser.
- Keine Anmeldung
- Kein Tracking
- Keine Datenbank
- Keine Server-Speicherung von Namen
- Keine externen Abhängigkeiten

### Anpassungen
Farben, Schriftgrößen und Layout können direkt im ```<style>```-Bereich von index.html angepasst werden.

Wichtige Farbvariablen stehen oben in CSS:

```
:root {
    --green: #58a6ff;
    --red: #f2cc60;
    --button: #1f6feb;
    --danger: #d29922;
}
```
Die Variablennamen ```--green``` und ```--red``` sind die beiden Highlightfarben, grün für Positiv und Rot für Negativ bedingte Ereignisse, zum Beispiel ist der Timmer ```--green```, aber wenn die Warnzeit anfängt wird er ```--red```.

### Lizenz
Dieses Projekt kann frei genutzt und angepasst werden.