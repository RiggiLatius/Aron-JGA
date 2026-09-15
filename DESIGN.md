---
name: Aron JGA
description: Dunkle Bar-Tour-App fürs Handy – Rangliste, Tour, Glücksrad
colors:
  bg: "#0b0c11"
  surface: "#151822"
  accent: "#ff8a2b"
  gold: "#ffc93c"
  ok: "#35d6a0"
  bad: "#ff5c7a"
---

# Design System: Aron JGA

## Overview

**Creative North Star: „Die Anzeigetafel in der dunklen Bar"**

Die App wird nachts benutzt, in lauten Räumen, von Leuten mit einem Getränk in
der anderen Hand. Alles ist dunkel, weil der Raum dunkel ist – kein
Stil-Entscheid, sondern der Nutzungsort. Darauf liegt genau eine warme
Signalfarbe, die zeigt, was gerade dran ist: die aktive Bar, der Drehen-Knopf,
der laufende Tab. Zahlen sind das eigentliche Inhaltsmaterial und werden
entsprechend groß und in Tabellenziffern gesetzt.

Die Richtung ist bewusst der Kategorie-Standard, vom Nutzer gewählt und ohne
ironische Extrawurst ausgeführt. Messlatte für das Handwerk sind Strava
(Rangliste als Herzstück, große Zahlen, klare Abzeichen) und Spotify (dunkle
Flächen, großzügige Typo, ruhige Bewegung). Der Ton bleibt trocken: keine
Sprüche in der Oberfläche, der Charakter steckt in Typografie, Abständen und
Details.

**Key Characteristics:**
- Dunkel aus dem Nutzungsort heraus, eine warme Akzentfarbe als einziges Signal
- Zahlen in Tabellenziffern, groß gesetzt, rechtsbündig vergleichbar
- Gezeichnete Strich-Icons, keine Emoji
- Ein inszenierter Moment: die Ansicht steigt beim Tabwechsel auf

## Colors

Tiefes Blauschwarz als Raum, warmes Orange als einziges Signal, Gold nur für die
Führung.

### Primary
- **Bernstein** (`--ac` #ff8a2b): aktive Bar, Haupt-Knopf, laufender Tab,
  Getränkename. Sonst nichts – die Seltenheit macht die Wirkung.
- **Bernstein hell** (`--ac-h` #ffa45c): Hover und Fließtext-Akzente auf dunkel.

### Secondary
- **Gold** (#ffc93c): ausschließlich die Führung in der Rangliste und
  Bar-Sieger. Nie als Dekor.

### Tertiary
- **Grün** (#35d6a0) für abgeschlossen, **Rot** (#ff5c7a) für Löschen und
  Fehler. Beide nur in Status und Rückmeldung.

### Neutral
- **Raum** (#0b0c11) Seitengrund, **Fläche** (#151822) Karten,
  **Fläche 2/3** (#1d2130 / #272c3c) Eingaben und Knöpfe.
- **Text** (#f4f6fa), **Text 2** (#a9b2c2) für Zweitzeilen,
  **Text 3** (#727c8e) für Einheiten und Zeitstempel.
- **Linie** (#262b38 / #333a4c).

### Named Rules
**Die Ein-Signal-Regel.** Bernstein markiert pro Ansicht genau eine Sache: was
als Nächstes dran ist. Zwei bernsteinfarbene Flächen nebeneinander heißt, eine
davon ist falsch.

**Keine Glut.** Farbige Schatten ohne Versatz sind verboten. Höhe entsteht über
neutrale Schatten mit Versatz und Weichzeichnung, Ringe über echte Ränder.

## Typography

**Display Font:** Bricolage Grotesque (Fallback Archivo, System-Grotesk)
**Body Font:** Archivo (Fallback System-Grotesk)

**Character:** Bricolage bringt die leicht eigenwillige, moderne Note für Namen
und Zahlen; Archivo hält den Rest sachlich und hat echte Tabellenziffern, damit
Schluck-Zahlen untereinander vergleichbar stehen. Bewusst nicht Inter – zu viele
Oberflächen sehen damit gleich aus.

### Hierarchy
- **Display** (Bricolage 800, clamp 26–38px, 1.0, `-.035em`): Hero-Titel,
  Name des Führenden.
- **Zahl** (Bricolage 800, clamp 46–62px, `-.05em`): Schlücke des Führenden.
- **Headline** (Bricolage 800, 22px, `-.025em`): Überschriften in Karten.
- **Title** (Archivo 650–700, 16.5–17px, `-.015em`): Bar- und Teilnehmernamen.
- **Body** (Archivo 450, 16px, 1.45): Fließtext, Hinweise auf max. 62ch.
- **Label** (Archivo 650, 11–12px, `.08em`, Versalien): Einheiten, Status,
  Augenbrauen-Zeilen.

## Icons

Eigene Strich-Icons als `<symbol>`-Sprite oben in der Seite, 24er-Raster,
Strichstärke 1.75 (klein: 2), runde Enden, `currentColor`. Emoji sind als Icons
verboten; im Fließtext haben sie ohnehin nichts verloren.

## Motion

Ein inszenierter Moment: Beim Tabwechsel steigen die Blöcke der Ansicht
gestaffelt auf (12px, leichter Weichzeichner, `cubic-bezier(.16,1,.3,1)`,
Versatz 40ms). Nicht bei jedem Datenstand – sonst zappelt die Seite, während
jemand Schlücke einträgt. Knöpfe sinken beim Drücken auf 0.975. Der
Live-Punkt im Kopf pulsiert. `prefers-reduced-motion` schaltet alles ab.

## Browser-Oberflächen

Textauswahl in Bernstein auf dunkel, Cursor in Bernstein, Scrollbalken schmal in
Flächenfarbe, Fokusring 2px Bernstein mit 2px Abstand, `color-scheme: dark` für
native Bedienelemente, `accent-color` für Häkchen.

## Layout

Eine Spalte, maximal 660px, 16px Seitenrand, Inhalt zentriert. Runde Ecken in
vier Stufen (10/14/20/28). Untere Tableiste mit `env(safe-area-inset-bottom)`,
Kopfzeile klebt oben mit Weichzeichner. Die Tour ist ein senkrechter Zeitstrahl
mit Punkten: gefüllt = erledigt, bernstein = aktiv, hohl = offen.
