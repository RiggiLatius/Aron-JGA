# Aron JGA – Bar-Crawl-Tracker

Eine einzige Datei (`index.html`). Kein Server, kein Build, keine npm-Installation,
keine Kosten. Geteilter Live-Stand über **Firebase Realtime Database** (Spark/Free),
gehostet über **GitHub Pages**.

**Spielregel:** An jeder Bar dreht ein Glücksrad das Getränk. Danach trägt der Admin
ein, wie viele Schlücke jeder gebraucht hat. **Wer am Ende die wenigsten Schlücke
hat, gewinnt.** Alle anderen verfolgen den Stand über denselben Link, read-only.

---

## 1. Einrichten

Die Firebase-Config des Projekts **`aron-jga`** ist in `index.html` schon
eingetragen. Es fehlt nur noch die Datenbank selbst und das Veröffentlichen.

### Realtime Database anlegen (der einzige offene Schritt)
1. [console.firebase.google.com](https://console.firebase.google.com) → Projekt
   **aron-jga** → links **Build → Realtime Database** → **Datenbank erstellen**.
2. Standort: **`europe-west1`** ist für Deutschland am schnellsten – die Region ist
   aber egal, die App probiert `europe-west1`, `us-central1` und
   `asia-southeast1` der Reihe nach durch und merkt sich die, die antwortet.
3. Bei der Frage nach den Regeln: **Im Testmodus starten**.
4. Tab **Regeln** kontrollieren – es muss offen sein:
   ```json
   {
     "rules": {
       ".read": true,
       ".write": true
     }
   }
   ```
   → **Veröffentlichen**. (Der Testmodus läuft nach 30 Tagen ab, deshalb lieber
   direkt diese Regeln setzen. Sicherheitshinweis siehe unten.)

Beim ersten Öffnen legt die App die neun Bars, die Getränkeliste und die
Einstellungen selbst an. Ist die Datenbank noch nicht erstellt oder sperren die
Regeln, sagt die App das im Klartext auf dem Bildschirm – kein weißer Screen.
Unter *Admin → Werkzeuge* steht, mit welcher Adresse sie gerade verbunden ist.

### Veröffentlichen
5. Repo auf GitHub → **Settings → Pages → Build and deployment → Source:
   „Deploy from a branch"**, Branch `main` (oder der Branch mit diesem Stand),
   Ordner `/ (root)` → **Save**.
6. Nach ein bis zwei Minuten liegt die App unter
   `https://<dein-name>.github.io/Aron-JGA/` – **diesen Link in die
   WhatsApp-Gruppe**.
7. Auf dem Handy: Link öffnen → Teilen-Menü → **„Zum Home-Bildschirm"**. Dann
   startet die App wie eine normale App ohne Browserleiste.

Das Projekt hat auch eine verknüpfte Firebase-Hosting-Site (`aron-jga`). Wer
mag, kann statt GitHub Pages `firebase deploy` benutzen – nötig ist es nicht.

### Ausprobieren ohne den echten Abend anzufassen
`?demo=1` an den Link hängen (`…/index.html?demo=1`): dann läuft alles nur im
Browser dieses Geräts, ohne die Datenbank. Praktisch zum Rumspielen vorher.

### Config ändern
Falls das Firebase-Projekt mal wechselt: der Block `FIREBASE_CONFIG` steht ganz
oben in `index.html`, klar markiert. `databaseURL` darf leer bleiben.

---

## 2. Das Foto von Aron

Das Foto liegt als **`assets/aron.jpg`** im Repo und wird ohne weiteres Zutun
angezeigt: klein und rund in der Kopfzeile, groß als Banner über der Rangliste.

Austauschen oder überschreiben geht auf drei Wegen, alle **ohne** Code-Eingriff:

* **Datei ersetzen:** neues `assets/aron.jpg` ins Repo. Die Endung ist egal – die
  App probiert `assets/aron.jpg`, `.jpeg`, `.png`, `.webp` und `.JPG` der Reihe
  nach und nimmt die erste Datei, die lädt.
* **Vom Handy hochladen:** Admin-Tab → *Einstellungen & Getränke* →
  **📷 Foto hochladen**. Das Bild wird auf max. 900 px verkleinert, in der DB
  gespeichert und erscheint sofort auf **allen** Geräten. Sticht die Repo-Datei aus.
* **Bild-URL** in das Feld *Bild* eintragen.

Lädt keins davon, zeigt die Rangliste einen schlichten Banner – nichts bricht.

---

## 3. Bedienung

Vier Tabs unten:

| Tab | Was |
|---|---|
| **Rangliste** | Standard-Ansicht für alle. Aufsteigend nach Gesamtschlücken, Platz 1–3 hervorgehoben, dazu Getränke-Anzahl, Ø und pro erledigter Bar der Bar-Sieger. |
| **Tour** | Alle Bars mit Status, Adresse als Google-Maps-Link, aktuelles Getränk groß. Antippen springt direkt in die Nachbearbeitung dieser Bar. |
| **Rad** | Glücksrad aus der Getränkeliste, ~3 s Animation. Danach **Passt** oder **Gibt's hier nicht** (sperrt das Getränk für diese Bar, neuer Dreh). Darunter „Getränk manuell wählen" als Fallback. |
| **Admin** | Nach Passwort. Alles Weitere. |

**Teilnehmer:** In `SEED_PARTICIPANTS` oben in `index.html` steht die Startliste
(aktuell nur `Dome`). Wer dort dazukommt, ist nach dem nächsten Öffnen der App
auch in der laufenden DB – der Abgleich läuft von allein (siehe unten). Oder du
legst Teilnehmer direkt im Admin-Tab an.

**Aussehen:** Dunkle Oberfläche mit einer warmen Signalfarbe, Schriften
Bricolage Grotesque und Archivo über Google Fonts, gezeichnete SVG-Icons statt
Emoji. Die Regeln dazu stehen in `DESIGN.md`, das Produktwissen in `PRODUCT.md`
(beides über die Impeccable-Skill angelegt). Ohne Netz fällt die App auf die
System-Schrift zurück und bleibt bedienbar.

**Passwort:** Standard `$$$`, steht in `settings.password` und ist im
Admin-Tab änderbar (`DEFAULT_PW` oben in `index.html` greift nur bei einer noch
leeren Datenbank). Auf dem Login-Screen steht es absichtlich **nicht** – sonst
könnte es jeder in der Gruppe ablesen. Nach der Eingabe wird es im `localStorage` des Geräts
gemerkt – man muss es also nur einmal pro Handy eintippen. Wer das Passwort nicht
hat, sieht alles, kann aber nichts ändern (Drehen darf jeder, bestätigen nur der
Admin).

**Schlücke:** Im Admin-Tab für **jede** Bar (nicht nur die aktive) – −/+ Stepper,
Direkteingabe, Minimum 1, Checkbox „ausgesetzt", Wert löschbar. „Ausgesetzt"
zählt als **Ø dieser Bar + Malus** (Standard 3, einstellbar). Später
dazugekommene Teilnehmer haben bei früheren Bars einfach keinen Wert und zählen
dort nicht mit.

---

## 4. Nichts geht verloren

* **Soft-Delete:** Teilnehmer, Bars, Getränke und Ergebnisse bekommen beim Löschen
  nur ein `deleted: true` (+ Zeitstempel) und werden ausgeblendet. Der Datensatz
  bleibt in der DB.
* **Papierkorb** (Admin-Tab): listet alles Gelöschte mit den damaligen Werten und
  einem **Wiederherstellen**-Button pro Eintrag. Gelöschte Teilnehmer zählen nicht
  in der Rangliste, ihre Schluck-Werte kommen beim Wiederherstellen komplett zurück.
* **Änderungsprotokoll** unter `/aron-jga/log`, append-only: pro Änderung ein
  Eintrag mit Zeitstempel, Art, betroffenem Pfad, **altem und neuem Wert**.
  Wer ein Feld antippt und den Wert unverändert lässt, erzeugt keinen Eintrag –
  gleiche Werte werden gar nicht geschrieben. Im
  Admin-Tab als Liste („21:14 – Schlücke Tobi @ Mos Eisley: 6 → 4"), neueste oben,
  jeder Eintrag mit **↩︎ = auf alten Wert zurücksetzen**. State-Änderung und
  Log-Eintrag gehen immer in **einem** `update()` mit Multi-Path-Keys raus – es gibt
  also keine Änderung ohne Protokolleintrag.
* **Undo** für die letzte Änderung (ein Button).
* **Backup kopieren / einfügen:** kompletter State als JSON in die Zwischenablage
  und zurück.
* **Roh-JSON-Editor** als Notausgang, mit Prüfung vor dem Speichern.
* **Startdaten-Abgleich:** Änderungen an `SEED_PARTICIPANTS`, `SEED_LOCATIONS`
  und `SEED_DRINKS` landen **beim nächsten Öffnen der App von allein** in der
  laufenden DB – auch wenn dort längst Schlücke drinstehen. Push, Seite neu
  laden, fertig. Automatisch laufen nur die unstrittigen Fälle:
  * **neue Einträge** anlegen,
  * **geänderte Namen und Adressen** nachziehen, sofern der Wert in der DB seit
    dem letzten Abgleich nicht von Hand geändert wurde.

  Alles andere wartet unter **Admin → Werkzeuge → „Startdaten abgleichen"** auf
  eine Entscheidung; der Knopf zeigt, wie viel ansteht:
  * **Hier von Hand geändert** – im Code steht etwas anderes als in der DB, aber
    den DB-Wert hat jemand im Admin gesetzt. Bleibt, bis du den Haken setzt;
    beide Werte stehen nebeneinander (`jetzt → laut Startdaten`).
  * **Nicht mehr in den Startdaten** – Eintrag kam aus dem Code, steht dort
    nicht mehr. Haken heißt ab in den Papierkorb, standardmäßig ohne Haken.

  Nie angefasst werden Schlücke, Ergebnisse, Bar-Status, gewürfeltes Getränk und
  die Reihenfolge; die pflegst du im Admin. Ein leeres Feld im Code heißt „keine
  Vorgabe" und überschreibt nichts. Was du nur in der DB angelegt hast, taucht im
  Abgleich gar nicht auf. Jeder Abgleich ist **ein** Log-Eintrag – ein ↩︎ nimmt
  ihn komplett zurück. Mit `SEED_AUTO = false` oben in `index.html` passiert gar
  nichts mehr ohne Knopfdruck.

**Wie die Zuordnung hält:** Jeder Startdaten-Eintrag hat einen festen `key`
(z.B. `moseisley`), der angelegte Datensatz merkt ihn sich im Feld `seed` und in
`seedSnap`, was zuletzt aus dem Code kam. Aus dem Dreieck Code / `seedSnap` / DB
fällt heraus, wer sich bewegt hat: weicht die DB vom Merkzettel ab, war es
jemand im Admin – dann fasst der Abgleich den Wert nicht an. Umbenennen im Code
heißt deshalb „umbenennen", nicht „neu anlegen"; für einen neuen Eintrag einfach
eine Zeile mit neuem `key` ergänzen.
* **„Alles zurücksetzen"** (doppelte Bestätigung) leert `participants`,
  `locations`, `results` und `drinks`, **lässt `/aron-jga/log` unangetastet**.
  Der Abend ist danach über das Protokoll noch komplett nachlesbar und über ↩︎
  pro Eintrag rekonstruierbar (auch der Reset selbst ist ein Eintrag – ein Klick
  auf sein ↩︎ holt alles zurück). Nur der separate, extra bestätigte Button
  **„Auch Verlauf endgültig löschen"** leert das Log.
* Das Log ist auf **500 Einträge** begrenzt, älteste rollen raus – weit jenseits
  dessen, was ein Abend produziert.

---

## 5. Datenmodell

```js
// /aron-jga/data
{
  settings: {
    eventName: "Aron JGA",
    password: "$$$",
    skipMalus: 3,           // Aufschlag auf den Bar-Durchschnitt bei "ausgesetzt"
    noRepeatDrinks: false,  // Getränke bar-übergreifend nicht wiederholen
    heroImage: ""           // Bild-URL oder data:-URI, leer = assets/aron.jpg
  },
  participants: { p1: { name: "Robin", deleted: false } },
  locations: {
    l1: { name: "küblerGo 24/7", address: "Rotebühlstraße 69, 70178 Stuttgart",
          order: 1, status: "pending", drink: null, rejected: [], deleted: false }
  },
  results: { "l1_p1": { sips: 4, skipped: false, deleted: false } },
  drinks: [{ name: "Bier 0,3", deleted: false }]
}

// /aron-jga/log  (append-only)
{ "-Nx1": { ts: 1757500000000, type: "sips",
            label: "Schlücke Tobi @ Mos Eisley",
            path: "results/l3_p2/sips", from: 6, to: 4,
            changes: [{ path: "results/l3_p2/sips", from: 6, to: 4 }] } }
```

`status` ist `pending` (offen) | `active` (aktiv) | `done` (abgeschlossen) und im
Admin-Tab in **beide** Richtungen frei schaltbar. Beim ersten Start legt die App
die neun Start-Bars und die Getränkeliste selbst an (auch das steht als Eintrag
im Log).

Technisch: Vanilla JS, inline CSS, Firebase-SDK per ESM-Import vom CDN,
`onValue` für Live-Updates (kein Polling), `update()` fürs Schreiben,
`push()` für die Log-Keys. Jede Änderung geht als **ein** `update()` mit
Multi-Path-Keys raus, das den State-Pfad und den Log-Eintrag zusammen enthält.

---

## 6. Sicherheitshinweis

Die DB-Regeln sind offen (`".read": true, ".write": true`). Das heißt:

* **Jeder, der den Link (bzw. die `databaseURL`) kennt, kann alle Daten lesen und
  theoretisch auch schreiben.** Das Passwort in der App ist reiner
  **Komfortschutz** gegen versehentliche Änderungen in der Gruppe – keine
  Sicherheitsmaßnahme. Es steht im Klartext in der Datenbank und ist im
  Browser auslesbar.
* Für einen Junggesellenabschied ist das völlig ok. Es gehören aber **keine
  sensiblen Daten** in diese DB.
* Der API-Key im Config-Block ist kein Geheimnis (er identifiziert nur das
  Projekt) – der Schutz kommt normalerweise über die DB-Regeln, die wir hier
  bewusst offen lassen.
* Nach dem Abend am besten die Realtime Database im Firebase-Projekt löschen
  oder die Regeln auf `false` setzen.
* Der Link ist der Zugang: wer ihn hat, sieht alles. Also nur in die Gruppe.
