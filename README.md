# Aron JGA – Bar-Crawl-Tracker

Eine einzige Datei (`index.html`). Kein Server, kein Build, keine npm-Installation,
keine Kosten. Geteilter Live-Stand über **Firebase Realtime Database** (Spark/Free),
gehostet über **GitHub Pages**.

**Spielregel:** An jeder Bar dreht ein Glücksrad das Getränk. Danach trägt der Admin
ein, wie viele Schlücke jeder gebraucht hat. **Wer am Ende die wenigsten Schlücke
hat, gewinnt.** Alle anderen verfolgen den Stand über denselben Link, read-only.

---

## 1. Einrichten (einmalig, ~5 Minuten)

### Firebase-Projekt anlegen
1. [console.firebase.google.com](https://console.firebase.google.com) → **Projekt hinzufügen**
   (Name z. B. `aron-jga`). Google Analytics kann man abwählen.
2. Links im Menü **Build → Realtime Database** → **Datenbank erstellen**.
   Standort: `europe-west1` (Belgien) ist für Deutschland am schnellsten.
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

### Config kopieren und einsetzen
5. Zahnrad oben links → **Projekteinstellungen** → unten **Meine Apps** →
   **Web-App hinzufügen** (Icon `</>`), Name egal, Hosting **nicht** nötig.
6. Firebase zeigt einen Block `const firebaseConfig = { … }`. Diese Werte in
   `index.html` ganz oben in den markierten Block eintragen:

   ```js
   const FIREBASE_CONFIG = {
     apiKey:            "AIza…",
     authDomain:        "aron-jga.firebaseapp.com",
     databaseURL:       "https://aron-jga-default-rtdb.europe-west1.firebasedatabase.app",
     projectId:         "aron-jga",
     storageBucket:     "aron-jga.appspot.com",
     messagingSenderId: "123456789012",
     appId:             "1:123456789012:web:abc123"
   };
   ```

   Wichtig ist vor allem **`databaseURL`**. Fehlt sie im Snippet, steht sie in der
   Realtime Database oben über der Datenansicht. Solange dort noch `HIER_…` steht,
   läuft die App im **Demo-Modus**: alles funktioniert, aber die Daten liegen nur
   im Browser des jeweiligen Geräts und werden nicht geteilt.

### Veröffentlichen
7. `index.html` (und diese `README.md`) in ein GitHub-Repo pushen.
8. Im Repo: **Settings → Pages → Build and deployment → Source: „Deploy from a
   branch"**, Branch `main`, Ordner `/ (root)` → **Save**.
9. Nach ein bis zwei Minuten liegt die App unter
   `https://<dein-name>.github.io/<repo>/` – **diesen Link in die WhatsApp-Gruppe**.
10. Auf dem Handy: Link öffnen → Teilen-Menü → **„Zum Home-Bildschirm"**. Dann
    startet die App wie eine normale App ohne Browserleiste.

---

## 2. Das Foto von Aron einsetzen

Es gibt zwei Wege, beide brauchen **keinen** Code-Eingriff:

* **Vom Handy hochladen (empfohlen):** Admin-Tab → *Einstellungen & Getränke* →
  **📷 Foto hochladen**. Das Bild wird auf max. 900 px verkleinert und in der DB
  gespeichert – es erscheint sofort auf **allen** Geräten (Kopfzeile + Rangliste).
* **Als Datei im Repo:** Foto als `assets/aron.jpg` ins Repo legen. Die App nimmt
  diesen Pfad automatisch, wenn im Feld *Bild* nichts anderes steht.
* Alternativ eine beliebige Bild-URL in das Feld *Bild* eintragen.

Ist kein Bild vorhanden, zeigt die Rangliste einen schlichten Banner – nichts
bricht.

---

## 3. Bedienung

Vier Tabs unten:

| Tab | Was |
|---|---|
| **Rangliste** | Standard-Ansicht für alle. Aufsteigend nach Gesamtschlücken, Platz 1–3 hervorgehoben, dazu Getränke-Anzahl, Ø und pro erledigter Bar der Bar-Sieger. |
| **Tour** | Alle Bars mit Status, Adresse als Google-Maps-Link, aktuelles Getränk groß. Antippen springt direkt in die Nachbearbeitung dieser Bar. |
| **Rad** | Glücksrad aus der Getränkeliste, ~3 s Animation. Danach **Passt** oder **Gibt's hier nicht** (sperrt das Getränk für diese Bar, neuer Dreh). Darunter „Getränk manuell wählen" als Fallback. |
| **Admin** | Nach Passwort. Alles Weitere. |

**Passwort:** Standard `aron2026`, steht in `settings.password` und ist im
Admin-Tab änderbar. Nach der Eingabe wird es im `localStorage` des Geräts
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
  Eintrag mit Zeitstempel, Art, betroffenem Pfad, **altem und neuem Wert**. Im
  Admin-Tab als Liste („21:14 – Schlücke Tobi @ Mos Eisley: 6 → 4"), neueste oben,
  jeder Eintrag mit **↩︎ = auf alten Wert zurücksetzen**. State-Änderung und
  Log-Eintrag gehen immer in **einem** `update()` mit Multi-Path-Keys raus – es gibt
  also keine Änderung ohne Protokolleintrag.
* **Undo** für die letzte Änderung (ein Button).
* **Backup kopieren / einfügen:** kompletter State als JSON in die Zwischenablage
  und zurück.
* **Roh-JSON-Editor** als Notausgang, mit Prüfung vor dem Speichern.
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
    password: "aron2026",
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
`push()` für die Log-Keys.

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
