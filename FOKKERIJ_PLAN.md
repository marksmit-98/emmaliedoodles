# Fokkerij-platform — Bouwplan

Uitbreiding van de bestaande nestplanning op `nestplanning.emmaliedoodles.com` naar een compleet fokkerij-platform.

---

## Architectuur

| Onderdeel | Keuze |
|---|---|
| Hosting | GitHub Pages (bestaand) |
| Data | Firebase Firestore (free tier) |
| Auth | Firebase Auth + Google Sign-In (`emmaliedoodles@gmail.com`) |
| Frontend | Vanilla JS, geen build-stap (consistent met bestaande code) |

Firebase project eigenaar: `mark.smit1998@gmail.com`  
App logins: `mark.smit1998@gmail.com` en `doodledelux@gmail.com`

---

## Bestanden

```
nestplanning/
├── index.html          ← bestaand, krijgt alleen een nav-link naar fokkerij
├── fokkerij.html       ← nieuw — het volledige fokkerij-platform
├── sw.js               ← bestaand, kleine update (cache-lijst)
└── ...rest ongewijzigd
```

---

## Data model (Firestore)

```
dogs/{id}
  name, birthDate, pedigree, status, fosterFamily, color, notes

heatCycles/{id}
  dogId, startDate, mated (bool), notes

nests/{id}
  dogId, matingDate, expectedBirthDate, nestName, notes
```

**Statuswaarden honden:** Actief / In opbouw / Gepensioneerd

---

## Berekeningen (client-side)

- **Gemiddeld interval** — AVERAGEIF over alle historische intervallen per hond
- **Fallback** (< 2 loopsheden) — EDATE(laatste, 6) = 6 kalendermaanden vooruit
- **Verwacht venster** — `[laatste + gemiddeld - 14, laatste + gemiddeld + 28]`
- **Interval weergave** — `X d (Y mnd Z d)`

---

## UI — 4 tabs in fokkerij.html

| Tab | Inhoud |
|---|---|
| Teefjes | Overzicht honden, toevoegen / bewerken / archiveren |
| Loopsheden | Per hond: datums + gedekt Ja/Nee, interval zichtbaar |
| Fokplanning | Tabel: laatste loopsheid, verwacht venster, gemiddeld interval |
| Kalender | 24-maanden matrix + maandkalender-view |

**Kleurcodering kalender:**
- Rood — venster valt in die maand + laatste loopsheid *niet* gedekt
- Geel — venster valt in die maand + laatste loopsheid *wel* gedekt
- Grijs achtergrond — huidige maand

**1-klik koppeling** — rode cel → "Dek registreren" → opent nestplanning met `geboortedatum = dekkingsdatum + 63 dagen`

---

## Bouwvolgorde

- [ ] **Fase 1** — Firebase setup (zie hieronder) + lege shell `fokkerij.html`
- [ ] **Fase 2** — Teefjes tab (CRUD)
- [ ] **Fase 3** — Loopsheden tab (CRUD + intervalberekening)
- [ ] **Fase 4** — Fokplanning tab (berekende tabel)
- [ ] **Fase 5** — Kalender tab (24-maanden matrix + maandview)
- [ ] **Fase 6** — 1-klik koppeling naar nestplanning

---

## Firebase setup (nog te doen)

Eenmalig, ~5 minuten:

1. Ga naar [console.firebase.google.com](https://console.firebase.google.com)
2. **Add project** → naam: `emmaliedoodles` → Google Analytics: uit
3. **Authentication → Get started** → Google → Enable → support email: `mark.smit1998@gmail.com` → Save
4. **Build → Firestore Database → Create database** → Production mode → regio: `eur3 (europe-west)`
5. **Project settings → </>** → app nickname: `fokkerij` → Register → kopieer de `firebaseConfig`
6. **Firestore → Rules** → vervang door:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.auth != null
        && request.auth.token.email in [
             'mark.smit1998@gmail.com',
             'doodledelux@gmail.com'
           ];
    }
  }
}
```

7. Klik **Publish**
8. Plak de `firebaseConfig` in de chat → dan begint het bouwen

---

## Vervolg

Open deze repo opnieuw in Claude Code, verwijs naar dit bestand, en plak de Firebase config. Dan wordt direct begonnen met fase 1.
