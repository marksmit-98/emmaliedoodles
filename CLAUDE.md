# CLAUDE.md — Emmaliedoodles (fokkerij-tools)

## Bedrijf & doel
- Emmaliedoodles is de Australian Labradoodle-fokkerij van Emma en Mark.
- Deze repo bevat de interne fokkerij-tools, niet de publieke website.
- `emmaliedoodles.com` draait op WordPress en staat niet in deze repo.
- Zustermerk: DoodleDeLux (trimsalon van Emma), repo `marksmit-98/trimsalon-agenda`.

## Context bijhouden
- Leer je iets blijvends (regel, voorkeur, beslissing, account-detail)? Werk dan **dit bestand** bij in plaats van alleen lokale memory, zodat de context met de repo meereist.
- Houd het kort; verwijder wat niet meer klopt.

## Repo & techniek
- GitHub: `marksmit-98/emmaliedoodles`; lokale map: `fokplanning`.
- Vanilla JavaScript, geen build-stap.
- PWA met `manifest.json` en `sw.js`. Werk bij nieuwe bestanden de cache-lijst in `sw.js` bij.
- Data: Firebase Firestore; authenticatie: Firebase Auth met Google Sign-In.
- `index.html`: Fokprogramma — teefjes, loopsheden (met aparte dekdatum), fokplanning en kalender als 24-maandenmatrix; jaarlijkse ECVO-oogtest; castratie-/sterilisatiedeadline met Google Calendar-reminder (max 28 dagen vooraf, Google-limiet).
- `nestplanning.html`: Nestplanner met Google Calendar-koppeling via GSI.
- `drachtplanner.html`: Drachtplanner.
- `FOKKERIJ_PLAN.md`: oorspronkelijk bouwplan; deels verouderd, de beschreven fases zijn gebouwd.

## Accounts (niet-voor-de-hand-liggend)
- Push als `marksmit-98`. Commit-auteur: `Mark Smit <mark.smit1998@live.nl>`, niet het CloudNation-adres.
- Eigenaar Firebase-project: `mark.smit1998@gmail.com`.
- `ALLOWED` staat in zowel `index.html` als `nestplanning.html`:
  `mark.smit1998@gmail.com`, `doodledelux@icloud.com`, `emmaliedoodles@gmail.com`.
- Bij toegangswijzigingen: pas beide `ALLOWED`-lijsten én de Firestore rules aan.

## Huisregels/stijl
- Commit-berichten: Nederlands, imperatief; bijvoorbeeld `Voeg X toe` of `Fix Y`.
- Website-copy (`emmaliedoodles.com`): geen emoji's als 🎉 of 🚀; hooguit één hartje (💚 of 🤎) per tekstblok.
- Alle teefjes zijn eigen honden, ook als ze in een gastgezin wonen. Contrasteer nooit één hond als "onze eigen hond" met de andere; beschrijf waar ze wonen.
- Mollie woont bij Emma en Mark; Aaf, Pip, Lily en andere teefjes wonen in gastgezinnen.

## Domeinregels
- **Bepalend:** NL Besluit houders van dieren, art. 3.4 lid 3: maximaal één nest per 12 aaneengesloten maanden (strenger dan ALAEU's 10 maanden).
- Geboortedatum = dekdatum + 63 dagen.
- ALAEU (2026-02): minimaal 16 maanden bij dekking; geen nest na de 8e verjaardag; maximaal 5 nesten per teef; maximaal 3 pups per nest voor de fok.
- WALA v2.1: niet fokken na de 7e verjaardag; maximaal 2 pups per nest als fokhond verkocht.
- Eigen fokdoelen (vastgesteld 2026-11-01):
  - Circa 10 nesten per jaar, met buffer zodat gastgezinhonden een jaar kunnen overslaan.
  - Maximaal 4 nesten per teef; soms liever 3.
  - Eerste dekking vanaf 16 maanden, vaak liever rond 2 jaar.
  - In 2027 uit elk nest één fokpup houden, geplaatst in een gastgezin.
  - Uit Mollies nest liefst een reu als dekreu; uit de overige nesten teefjes.
  - De 12-maandenregel begrenst 2027 op circa 7–8 nesten.

## Deploy
- GitHub Pages, CNAME `fokplanning.emmaliedoodles.com`. Push naar main = live.

## Bekende valkuilen
- Verwar deze interne app niet met de WordPress-website.
- Gebruik `FOKKERIJ_PLAN.md` niet als actuele lijst van nog te bouwen fases.
- Wijzig toegang nooit in maar één HTML-bestand of alleen in de Firestore rules.
- Plan niet op basis van ALAEU's 10 maanden: de wettelijke 12 maanden zijn bepalend.
- `artifacts/`, `Fokprogramma_kalender.*` en `desktop.ini` zijn lokale bestanden (staan in `.gitignore`); niet committen.
