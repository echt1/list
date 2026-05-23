# 🎬 Meine Liste – Setup Guide

Deine persönliche Watchlist / Bucketlist als GitHub Page.

---

## 🚀 Schritt-für-Schritt Einrichtung

### 1. Firebase Projekt erstellen

1. Geh auf [console.firebase.google.com](https://console.firebase.google.com)
2. Klick **"Projekt hinzufügen"**
3. Namen wählen (z.B. `meine-liste`) → Durchklicken
4. Im Projekt: Links auf das **`</>`** Icon klicken (Web-App hinzufügen)
5. App-Namen eingeben → **"Firebase Hosting"** kannst du abwählen (wir nutzen GitHub Pages)
6. Den `firebaseConfig`-Block kopieren – den brauchst du gleich

### 2. Firestore Datenbank einrichten

1. Links im Menü: **Firestore Database** → "Datenbank erstellen"
2. **Produktionsmodus** wählen → Region: `europe-west3` (Frankfurt)
3. Nach dem Erstellen: Tab **"Regeln"** klicken und folgendes einfügen:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    // Einträge: Jeder kann lesen, nur Owner kann schreiben
    match /entries/{doc} {
      allow read: if true;
      allow write: if request.auth != null && request.auth.token.email == "DEINE@EMAIL.DE";
    }

    // Vorschläge: Jeder kann schreiben (anonym), nur Owner kann lesen/aktualisieren
    match /suggestions/{doc} {
      allow create: if true;
      allow read, update: if request.auth != null && request.auth.token.email == "DEINE@EMAIL.DE";
    }
  }
}
```

> ⚠️ Ersetze `DEINE@EMAIL.DE` mit deiner echten E-Mail (Google-Account)!

4. **"Veröffentlichen"** klicken

### 3. Google Auth aktivieren

1. Links: **Authentication** → "Loslegen"
2. Tab **"Sign-in-Methode"**
3. **Google** aktivieren → deine E-Mail als Support-E-Mail eintragen → Speichern
4. Tab **"Einstellungen"** → **"Autorisierte Domains"** → Deinen GitHub-Pages-Domain eintragen:
   `DEIN-USERNAME.github.io`

### 4. index.html konfigurieren

Öffne `index.html` und ersetze den `FIREBASE_CONFIG`-Block ganz oben:

```javascript
const FIREBASE_CONFIG = {
  apiKey:            "AIza...",          // aus Firebase Console
  authDomain:        "meine-liste.firebaseapp.com",
  projectId:         "meine-liste",
  storageBucket:     "meine-liste.appspot.com",
  messagingSenderId: "12345...",
  appId:             "1:12345...:web:abc..."
};
const OWNER_EMAIL = "deine@email.de";   // dein Google-Account
```

### 5. GitHub Repository + GitHub Pages

1. Neues Repo auf [github.com](https://github.com) erstellen (z.B. `meine-liste`)
2. `index.html` und `README.md` hochladen
3. Repo **Settings** → **Pages** → Source: **"Deploy from a branch"** → Branch: `main` → Ordner: `/ (root)`
4. Nach ~1 Minute ist die Seite live unter:
   `https://DEIN-USERNAME.github.io/meine-liste`

---

## 🎮 Benutzung

| Wer | Was |
|-----|-----|
| Alle | Einträge ansehen, filtern, suchen |
| Alle | Vorschläge einreichen (kein Login nötig) |
| Owner (du) | Login → Einträge hinzufügen/bearbeiten/löschen |
| Owner (du) | Status toggeln (Geplant ↔ Erledigt) |
| Owner (du) | Vorschläge von Freunden annehmen oder ablehnen |

---

## 📬 Vorschläge teilen

Schick Freunden einfach den Link zur Seite. Der **"+ Vorschlag"** Button oben rechts ist für alle sichtbar – kein Account nötig. Du siehst dann ein Banner wenn neue Vorschläge reinkommen.

---

## 🔧 Anpassen

- **Seitenname ändern:** In `index.html` suche nach `Meine Liste` und `meine liste`
- **Weitere Kategorien:** Die Kategorien sind momentan Film/Serie/Spiel/Buch. Für mehr: im `catEmoji`- und `catLabel`-Objekt ergänzen und neue Pills + Cat-Options hinzufügen.
- **Farbe ändern:** Die Akzentfarbe ist lila (`#a855f7`). Suche nach `--accent` und tausche sie aus.
