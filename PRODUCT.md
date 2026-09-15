# Product

<!-- impeccable:product-schema 1 -->

## Platform

web

## Users

Acht Freunde auf dem Junggesellenabschied von Aron in Stuttgart. Zwei Rollen am
selben Link:

- **Der Organisator** (Admin per Passwort) trägt ein, was passiert: Schlücke
  nach jeder Bar, Bar-Status, gewürfeltes Getränk, Teilnehmer. Er hält das Handy
  in einer Hand, ein Getränk in der anderen, steht in einer lauten, dunklen Bar
  und wird im Lauf des Abends betrunkener.
- **Die anderen sieben** öffnen denselben Link und schauen nur: Wo stehen wir,
  was wird gerade getrunken, wer führt. Sie tippen nichts ein.

Beide sind auf dem Handy, oft über die Verknüpfung auf dem Home-Bildschirm, in
wechselndem Mobilfunknetz.

## Product Purpose

Die App führt durch eine Bar-Tour: pro Bar wird ein Getränk ausgewürfelt, alle
trinken dasselbe, danach wird für jeden eingetragen, wie viele Schlücke er
gebraucht hat. **Wenige Schlücke sind gut** – wer am Ende die wenigsten
gebraucht hat, gewinnt. Erfolg heißt: Der Abend läuft weiter, ohne dass jemand
auf das Handy starrt, und am Ende steht eine Rangliste, über die man streiten
kann.

## Positioning

Der Wettbewerb ist umgedreht: Nicht wer am meisten trinkt gewinnt, sondern wer
am wenigsten Schlücke braucht. Aussetzen ist erlaubt und kostet einen Malus
(Ø der Bar + Aufschlag), statt jemanden aus der Wertung zu werfen.

## Operating Context

- Feste Tour durch neun Stuttgarter Bars, in Reihenfolge abgearbeitet:
  pending → aktiv → erledigt.
- Pro Bar dreht der Organisator ein Glücksrad aus der Getränkeliste. Das
  Ergebnis wird bestätigt („Passt") oder abgelehnt („Gibt's hier nicht"), dann
  ist das Getränk für diese Bar gesperrt und es wird neu gedreht.
- Nach der Bar werden die Schlücke pro Person eingetragen, „ausgesetzt" ist ein
  eigener Zustand.
- Alles läuft live über eine gemeinsame Firebase-Datenbank; jede Änderung landet
  im Protokoll und ist einzeln zurücknehmbar, Gelöschtes im Papierkorb.
- Startdaten (Teilnehmer, Bars, Getränke) stehen im Code und gleichen sich beim
  Öffnen selbst in die laufende Datenbank ab.

## Capabilities and Constraints

- Vier Bereiche: Rangliste, Tour, Rad, Admin (hinter Passwort).
- Eine einzige `index.html` ohne Build-Schritt, ausgeliefert über GitHub Pages;
  Daten in der Firebase Realtime Database, Demo-Modus über `?demo=1`.
- Wer den Link hat, sieht alles; das Passwort schützt nur das Schreiben.
- Muss im Dunkeln, im Lärm und bei wackligem Netz bedienbar bleiben; die
  Eingabe der Schlücke ist die einzige Aktion, die wirklich schnell gehen muss.
- Fachbegriffe, die bleiben: Schlücke, Bar, Tour, Rad, ausgesetzt, Malus.

## Brand Commitments

- Name: **Aron JGA**. Sprache durchgehend Deutsch.
- Ein Foto von Aron liegt im Repo (`assets/aron.jpg`) und wird als Hero gezeigt.
- Ton: trocken und knapp. Der Witz liegt in der Gestaltung und in kleinen
  Details, nicht in Sprüchen (vom Nutzer im Interview festgelegt).
- Der Nutzer erlaubt eine nachgeladene Web-Schrift und das Neubauen einzelner
  Ansichten; Funktionen, Abläufe und Fachbegriffe bleiben erhalten.
- **Stehende Design-Präferenz:** Der Nutzer hat im Richtungsentscheid bewusst
  den Kategorie-Standard gewählt („moderne Party-App"), nicht eine eigene Welt.
  Künftige Arbeit führt diesen Standard sauber weiter, ohne ironische
  Extrawurst. Messlatte fürs Handwerk (mangels Vorgabe selbst gesetzt): Strava
  und Spotify.

## Evidence on Hand

Die neun Bars mit Adressen, die Getränkeliste und die acht Teilnehmernamen
stehen als Startdaten im Code. Ein Foto von Aron. Sonst nichts – keine
weiteren Bilder, kein Logo, keine Marke.
