## Daily Timer

Die von mir gehostete Version findest du hier: [daily.tomfe.de](https://daily.tomfe.de)

Eine kleine, statische Daily-Seite für Teams: zufällige Reihenfolge, Timer, Redeobjekt, Vorbereitungs-Countdown und einfache Verwaltung der Teilnehmenden direkt im Browser.

Die Seite besteht nur aus HTML, CSS und JavaScript. Es ist kein Build-Step, kein Backend und keine Datenbank nötig.

---

### Features

- Großer Countdown-Timer für das Daily
- Zufällige Reihenfolge aller anwesenden Teilnehmenden
- 7-Sekunden-Vorbereitungs-Countdown vor jeder Person
- Anzeige, wer als Nächstes dran ist: `Mach dich bereit: X`
- Namen direkt über die Seite hinzufügen oder entfernen
- Teilnehmende per Checkbox auf anwesend oder abwesend setzen
- Anwesenheit kann auch während laufender Timer und während des Prep-Timers geändert werden
- Namen werden lokal im Browser gespeichert
- Pause, Reset, Skip und Weiter
- Skip funktioniert auch während des Prep-Timers
- Manuelles Starten einer bestimmten Person über den Play-Button
- Play-Button funktioniert auch während des Prep-Timers
- Wenn per Play zu einer anderen Person gewechselt wird, wird die aktuelle Person als erledigt markiert
- Bereits erledigte Personen werden markiert und kommen in derselben Runde nicht erneut dran
- Zufälliges Redeobjekt pro Daily-Start aus `objects.json`
- Dunkles Design
- Farbenblindfreundliches Design: Blau statt Grün, Gelb statt Rot
- Flüßiges Ablauf-Blinken: Der Timer wechselt nach Ablauf zwischen Gelb und Hellgrau, statt komplett zu verschwinden

---

### Dateien

```text
.
├── index.html
└── objects.json
```

`index.html` enthält die komplette Daily-Seite.

`objects.json` enthält die möglichen Redeobjekte. Die Datei muss im selben Verzeichnis wie `index.html` liegen.

---

### Nutzung

1. Repo klonen oder downloaden
2. `index.html` und `objects.json` im selben Ordner ablegen
3. Die Seite über den Webserver öffnen

Hinweis: Die Seite sollte über einen Webserver geöffnet werden, damit `objects.json` zuverlässig geladen werden kann. Lokal per Doppelklick kann das Laden der JSON-Datei je nach Browser blockiert werden. In diesem Fall nutzt die Seite die eingebauten Fallback-Objekte.

---

### Namen speichern

Die Namen werden im `localStorage` im Browser gespeichert.

Das bedeutet:

- Die Namen bleiben nach dem Neuladen erhalten.
- Die Namen sind nur auf diesem Gerät und in diesem Browser gespeichert.
- Es werden keine Namen an einen Server geschickt.
- Wenn Browserdaten gelöscht werden, können die Namen verschwinden.

---

### Redeobjekte konfigurieren

Die Redeobjekte stehen in `objects.json`.

Beispiel:

```json
[
  { "artikel": "Den", "name": "Stab" },
  { "artikel": "Die", "name": "Tastatur" },
  { "artikel": "Das", "name": "Kabel" }
]
```

Daraus wird auf der Seite zum Beispiel:

```text
Den Rede-Stab hat:
Max
```

oder:

```text
Das Rede-Kabel hat:
Anna
```

Bei jedem Start über **Go** wird ein Redeobjekt zufällig ausgewählt. Das Objekt bleibt für die laufende Runde gleich.

---

### Timerdauer ändern

Die Dauer ist in `index.html` im JavaScript-Bereich festgelegt:

```js
const DURATION_SECONDS = 3 * 60;
```

Beispiele:

```js
const DURATION_SECONDS = 4 * 60; // 4 Minuten
const DURATION_SECONDS = 5 * 60; // 5 Minuten
const DURATION_SECONDS = 90;     // 90 Sekunden
```

Die Vorbereitungszeit vor jeder Person ist ebenfalls dort definiert:

```js
const PREP_SECONDS = 7;
```

Die Warnzeit ist dort ebenfalls definiert:

```js
const WARNING_SECONDS = 30;
```

Ab diesem Wert wechselt der Timer in die Warnfarbe.

---

### Bedienung

#### Go

Startet eine neue Daily-Runde.

Dabei werden alle anwesenden Teilnehmenden zufällig gemischt. Abwesende Teilnehmende werden nicht berücksichtigt.

Vor der ersten Person startet zuerst der 7-Sekunden-Prep-Timer.

#### Prep-Timer

Vor jeder Person läuft ein kurzer Vorbereitungs-Countdown.

Während dieser Zeit bleibt oben weiterhin der Redegegenstand sichtbar, zum Beispiel:

```text
Den Rede-Stab hat:
Max
```

Darunter steht, wer danach dran ist:

```text
Mach dich bereit: Anna
```

Wenn niemand mehr danach kommt, steht dort:

```text
Niemand mehr danach
```

#### Pause

Pausiert oder setzt den aktuellen Rede-Timer fort.

Der Prep-Timer wird nicht pausiert.

#### Skip

Überspringt die aktuelle Person sofort.

Die Person wird als erledigt markiert und kommt in dieser Runde nicht erneut dran.

Skip funktioniert sowohl während der Redezeit als auch während des Prep-Timers.

#### Weiter

Erscheint nach Ablauf des Rede-Timers.

Erst wenn **Weiter** geklickt wird, startet der Prep-Timer für die nächste Person.

#### Reset

Setzt die aktuelle Runde zurück.

Alle erledigt-Markierungen der laufenden Runde werden entfernt.

#### Play-Button neben einem Namen

Startet diese Person direkt.

Wenn gerade eine andere Person läuft oder sich im Prep-Timer befindet, wird diese aktuelle Person als erledigt markiert und die ausgewählte Person startet mit eigenem Prep-Timer.

Die manuell gestartete Person wird aus der restlichen Zufallsreihenfolge entfernt, damit sie in derselben Runde nicht erneut dran kommt.

#### Anwesenheits-Checkbox

Über die Checkbox kann eine Person auf anwesend oder abwesend gesetzt werden.

Das funktioniert auch während einer laufenden Runde, während der Redezeit, während des Prep-Timers und nach Ablauf des Timers.

- Wird eine Person auf abwesend gesetzt, wird sie aus der restlichen Runde entfernt.
- Wird die aktuelle Person auf abwesend gesetzt, wird sie übersprungen und die nächste Person vorbereitet.
- Wird eine Person während einer laufenden Runde wieder auf anwesend gesetzt, wird sie zufällig in die restliche Runde eingefügt, sofern sie noch nicht erledigt war.

---

### Ablauf einer Runde

1. **Go** klicken
2. Redeobjekt wird zufällig aus `objects.json` geladen
3. Anwesende Teilnehmende werden zufällig gemischt
4. 7-Sekunden-Prep-Timer für die erste Person
5. Rede-Timer läuft
6. Nach Ablauf blinkt die Zeit gelb/hellgrau
7. **Weiter** klicken
8. Prep-Timer für die nächste Person startet
9. Wiederholen, bis niemand mehr übrig ist

---

### Daten und Datenschutz

Diese Daily-Seite läuft komplett im Browser.

- Keine Anmeldung
- Kein Tracking
- Keine Datenbank
- Keine Server-Speicherung von Namen
- Keine externen Abhängigkeiten
- Namen werden nur lokal im Browser gespeichert

---

### Anpassungen

Farben, Schriftgrößen und Layout können direkt im `<style>`-Bereich von `index.html` angepasst werden.

Wichtige Farbvariablen stehen oben im CSS:

```css
:root {
  --green: #58a6ff;
  --red: #f2cc60;
  --button: #1f6feb;
  --danger: #d29922;
}
```

Hinweis: Die Variablennamen `--green` und `--red` stammen noch aus einer älteren Version. Inhaltlich sind sie inzwischen farbenblindfreundlich belegt:

- `--green` ist die blaue Hauptfarbe.
- `--red` ist die gelbe Warnfarbe.

Der Timer ist im Normalzustand blau. Ab der Warnzeit wird er gelb.

Nach Ablauf blinkt der Timer nicht durch komplettes Verschwinden, sondern wechselt zwischen Gelb und Hellgrau. Das ist ruhiger und performanter.

---

### Lizenz

Dieses Projekt kann frei genutzt und angepasst werden.
