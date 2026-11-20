# CRM Effekten Parkett

Das CRM des Corporate Relations Teams im Effekten Parkett (Börsenverein der HfWU). Hier liegen alle Unternehmen und Ansprechpartner. Das CRM zeigt auf einen Blick, wer wichtig ist, und erinnert an jedes Nachfassen.

## So hängt alles zusammen

| Teil | Aufgabe | Kosten |
|---|---|---|
| **GitHub Pages** | Zeigt die Seite an, also den Code des CRM | kostenlos |
| **Firebase** (von Google) | Speichert die Daten in Frankfurt und regelt die Anmeldung | kostenlos (Tarif Spark, ohne Zahlungsdaten) |

Im GitHub-Repository liegt **nur der Code, nie die Daten**. Kontakte, Unternehmen und Anfragen liegen ausschließlich in Firebase. Lesen und schreiben darf dort nur, wer sich angemeldet, seine E-Mail bestätigt hat und auf der Freigabeliste steht.

## Was liegt wo

| Datei | Inhalt |
|---|---|
| `index.html` | Das ganze CRM |
| `firebase-config.js` | Die Verbindung zu eurem Firebase-Projekt |
| `firestore.rules` | Die Zugriffsregeln. Werden in Firebase eingefügt |
| `.nojekyll` | Leere Hilfsdatei für GitHub Pages |
| `README.md` | Diese Anleitung |

---

## Einrichtung (einmalig, etwa 30 Minuten)

### Schritt 1: Firebase-Projekt anlegen

1. Öffne [console.firebase.google.com](https://console.firebase.google.com) und melde dich mit dem Google-Konto an, dem das CRM gehören soll.
2. Lege ein neues Projekt an. Name zum Beispiel `ep-crm`.
3. Google Analytics brauchst du nicht. Schalte es aus.
4. Der kostenlose Tarif **Spark** ist automatisch aktiv. Hinterlege keine Zahlungsdaten. Dann kann auch nie eine Rechnung kommen.

### Schritt 2: Anmeldung einschalten

1. Links im Menü: **Authentication** > **Jetzt starten**.
2. Reiter **Anmeldemethode**: **E-Mail/Passwort** aktivieren. Nur den ersten Schalter, nicht den Link ohne Passwort. Speichern.
3. Reiter **Einstellungen** > **Autorisierte Domains** > **Domain hinzufügen**: `DEIN-GITHUB-NAME.github.io`

### Schritt 3: Datenbank anlegen

1. Links im Menü: **Firestore Database** > **Datenbank erstellen**. Falls gefragt wird, die Standard-Edition nehmen.
2. Standort: **europe-west3 (Frankfurt)**. Der Standort lässt sich später nicht mehr ändern.
3. **Im Produktionsmodus starten**.
4. Reiter **Regeln**: Den gesamten Inhalt durch den Inhalt der Datei `firestore.rules` ersetzen und **Veröffentlichen**.

### Schritt 4: Dich selbst freischalten

Die erste Person muss einmal von Hand in die Freigabeliste. Alle weiteren schaltest du danach direkt im CRM frei.

In **Firestore Database** > Reiter **Daten** > **Sammlung starten**:

| Feld | Eintrag |
|---|---|
| Sammlungs-ID | `zugang` |
| Dokument-ID | deine E-Mail-Adresse, **alles klein geschrieben** |
| Feld | `rolle` |
| Typ | string |
| Wert | `verwalter` |

### Schritt 5: Web-App anmelden und Verbindungsdaten eintragen

1. Zahnrad oben links > **Projekteinstellungen** > Reiter **Allgemein** > unten bei **Meine Apps** auf das Symbol `</>` klicken.
2. Name: `CRM`. **Firebase Hosting nicht anhaken.** App registrieren.
3. Es erscheint ein Block `const firebaseConfig = { ... }`. Übertrage die Werte in die Datei `firebase-config.js`.

Die Werte sind kein Geheimnis. Sie stehen bei jeder Firebase-Web-App sichtbar im Quelltext. Geschützt werden die Daten durch die Zugriffsregeln aus Schritt 3.

### Schritt 6: Auf GitHub hochladen

1. Auf [github.com](https://github.com) ein neues Repository anlegen (**New repository**). Name zum Beispiel `ep-crm`. Sichtbarkeit **Public**, denn GitHub Pages ist bei kostenlosen Konten nur für öffentliche Repositorys gratis. Öffentlich ist dabei nur der Code.
2. Im neuen Repository auf **uploading an existing file** klicken.
3. Diese Dateien hineinziehen: `index.html`, `firebase-config.js`, `firestore.rules`, `README.md`, `.nojekyll`. Auf dem Mac zeigst du versteckte Dateien wie `.nojekyll` im Finder mit **Cmd + Shift + Punkt** an.
4. **Commit changes** klicken.
5. **Settings** > **Pages** > bei **Source** „Deploy from a branch“ wählen, Branch `main`, Ordner `/ (root)` > **Save**.
6. Nach ein bis zwei Minuten läuft das CRM unter `https://DEIN-GITHUB-NAME.github.io/ep-crm/`.

### Schritt 7: Konto erstellen und Daten umziehen

1. Im **alten CRM auf claude.ai**: Team & Daten > **Sicherung speichern**.
2. Das **neue CRM** öffnen > **Neu hier? Konto erstellen** mit der Adresse aus Schritt 4.
3. Die Bestätigungsmail öffnen und auf den Link klicken. Sie kommt von `noreply@…firebaseapp.com` und landet manchmal im Spam.
4. Im CRM auf **Ich habe bestätigt** tippen.
5. Team & Daten > **Sicherung einspielen** > die Datei aus Punkt 1 wählen.
6. Unter **Zugang zum CRM** die anderen Teammitglieder freischalten.

---

## Im Alltag

**Neue Person ins Team holen**
1. Team & Daten > Zugang zum CRM > **Adresse freischalten**.
2. Der Person den Link zum CRM schicken.
3. Sie tippt auf „Neu hier? Konto erstellen“, nimmt genau diese Adresse und bestätigt ihre E-Mail.

**Jemand verlässt das Team**
1. Kontakte übertragen (Team & Daten > Kontakte übertragen).
2. Unter Zugang zum CRM auf **Entfernen**. Die Person kommt danach sofort nicht mehr rein.

**Rollen**
- **Mitglied:** Kann alles im CRM sehen und bearbeiten.
- **Verwaltung:** Kann zusätzlich Leute freischalten und entfernen. Am besten haben immer mindestens zwei Leute diese Rolle.

**Übergabe des ganzen CRM**
- Firebase: Zahnrad > **Nutzer und Berechtigungen** > weitere Person als **Inhaber** hinzufügen. Danach kann die alte Person sich entfernen.
- GitHub: Im Repository **Settings** > **Collaborators** eine weitere Person einladen, oder das Repository über **Settings** > **Transfer** übergeben.
- Die Webadresse des Firebase-Projekts bleibt dabei gleich. Die GitHub-Adresse ändert sich nur, wenn das Repository den Besitzer wechselt.

**Sicherung**
Zum Ende jedes Semesters unter Team & Daten eine Sicherung speichern.

## Datenschutz

- **Sicherungen und Excel-Dateien nie ins Repository hochladen.** Sie enthalten Kontaktdaten, und das Repository ist öffentlich.
- Die Daten liegen in einem Rechenzentrum in Frankfurt.
- Die Datenschutzregeln für das Team stehen im CRM unter Team & Daten.

## Grenzen des kostenlosen Tarifs

1 GB Speicher, 50.000 Lesezugriffe und 20.000 Speichervorgänge pro Tag. Bei einigen hundert Kontakten kommt ihr da nicht in die Nähe. Wird eine Grenze doch erreicht, hält Firebase bis zum nächsten Tag an. Eine Rechnung kann ohne hinterlegte Zahlungsdaten nicht entstehen.

## Änderungen am CRM

Das CRM ist eine einzige Datei: `index.html`. Für eine neue Version wird nur diese Datei im Repository ersetzt (**Add file** > **Upload files**). Die Daten in Firebase bleiben dabei unberührt.
