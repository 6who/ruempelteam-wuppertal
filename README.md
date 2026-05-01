# Rümpelteam Wuppertal — Webseite

Statische Webseite (HTML + Tailwind via CDN, kein Build-Schritt) für ein Entrümpelungs- und Haushaltsauflösungs-Team aus Wuppertal.

---

## 1. Lokal anschauen

Die Webseite ist eine reine statische Datei und kann auf zwei Wegen geöffnet werden:

**Option A — direkt im Browser (am einfachsten):**
Doppelklick auf `index.html`. Funktioniert für die meisten Tests sofort.

**Option B — kleiner lokaler Server (empfohlen, damit relative Links sauber arbeiten):**

```bash
# im Projekt-Ordner ausführen
python -m http.server 8000
```

Dann im Browser öffnen: `http://localhost:8000`.

---

## 2. Vor dem Live-Gang erledigen

Diese Punkte **müssen** noch ausgefüllt werden, bevor die Webseite öffentlich erreichbar ist:

### Logo

- Logo-Datei (sauber, ohne Flyer-Hintergrund) als `assets/logo.png` ablegen.
- Empfohlene Größe: mindestens 600 × 600 px, transparenter Hintergrund.
- Wird im Header, Hero, Footer, Impressum und Datenschutz automatisch geladen.

### Formspree-Setup (Kontaktformular)

1. Account anlegen auf [formspree.io](https://formspree.io) (kostenloser Plan reicht für ~50 Anfragen / Monat).
2. Neues Form erstellen, als Empfänger-Adresse `ruempelteam-wuppertal@gmx.de` eintragen.
3. Formspree gibt eine Form-ID aus (z. B. `xyzabcde`) und damit eine Endpoint-URL: `https://formspree.io/f/xyzabcde`.
4. In `index.html` nach `REPLACE_WITH_FORMSPREE_ID` suchen und die ID dort einsetzen:
   ```html
   <form action="https://formspree.io/f/xyzabcde" method="POST">
   ```
5. Die GMX-Adresse einmalig per Bestätigungs-Mail verifizieren — sonst gehen keine Mails durch.

> **Hinweis:** Solange `REPLACE_WITH_FORMSPREE_ID` nicht ersetzt ist, fällt das Formular automatisch auf einen `mailto:`-Fallback zurück. Es funktioniert also auch vor dem Setup, öffnet dann aber das Mailprogramm des Kunden.

### Impressum (`impressum.html`)

Alle mit `TODO:` markierten Felder ausfüllen:

- Inhaber-/Firmenname und Rechtsform
- Vollständige Anschrift
- Umsatzsteuer-ID (oder Hinweis „Kleinunternehmer §19 UStG")
- Verantwortlich nach §55 RStV (i. d. R. identisch mit dem Inhaber)

### Datenschutzerklärung (`datenschutz.html`)

Alle mit `TODO:` markierten Felder ausfüllen:

- Verantwortlicher (Name, Anschrift)
- Stand-Datum
- Hosting-Anbieter (sobald gewählt)

Der Hinweis zur Auftragsverarbeitung durch Formspree (USA) ist bereits enthalten — er ist DSGVO-relevant, weil Daten in die USA übertragen werden.

> Für eine rechtssichere Erklärung empfiehlt sich eine kurze Prüfung durch einen Anwalt oder die Nutzung eines DSGVO-Generators (z. B. e-Recht24, eRecht-Datenschutz).

---

## 3. Hosting-Optionen

Die Seite ist eine reine statische Webseite und läuft auf jedem Hoster:

| Anbieter | Aufwand | Kosten |
|---|---|---|
| **Netlify Drop** | Drag & Drop des Projekt-Ordners auf [netlify.com/drop](https://app.netlify.com/drop) | gratis |
| **GitHub Pages** | Repository erstellen, Pages aktivieren, Domain verknüpfen | gratis |
| **Vercel** | `vercel deploy` im Ordner | gratis (Hobby) |
| **Klassischer Webhoster** | Dateien per FTP hochladen | je nach Anbieter |

Für eine eigene Domain (z. B. `ruempelteam-wuppertal.de`) bei jedem dieser Anbieter einfach in den Domain-Einstellungen verknüpfen.

---

## 4. Pflege & Anpassung

**Inhalte ändern:** Alles ist direkt in den HTML-Dateien als Klartext. Suchen + ersetzen reicht.

**Farben anpassen:** In `index.html` im `<style>`-Block oben:

```css
:root {
  --bg:   #0a0a0a;   /* Hintergrund */
  --neon: #84FF1A;   /* Akzent-Grün */
}
```

Die gleiche Farbe ist im `tailwind.config`-Block oben definiert (`colors.neon`). Beide Stellen anpassen, wenn die Farbe geändert wird.

**Neue Leistung hinzufügen:** In `index.html` nach `<!-- LEISTUNGEN -->` suchen und ein bestehendes `<article class="svc-card …">` duplizieren.

**Schriften ändern:** `<link href="https://fonts.googleapis.com/...">` im Head und die `fontFamily`-Block in der Tailwind-Config zeitgleich anpassen.

---

## 5. Performance-Hinweis

Tailwind wird derzeit über CDN nachgeladen — sehr bequem, aber jeder Besucher lädt ~30 KB extra. Für maximale Performance kann später ein lokaler Tailwind-Build aufgesetzt werden:

```bash
npm install -D tailwindcss
npx tailwindcss -i ./input.css -o ./assets/tailwind.css --minify
```

Dann im HTML den CDN-Script-Tag durch `<link rel="stylesheet" href="assets/tailwind.css">` ersetzen. Optional, nicht zwingend.

---

## 6. Verifikations-Checkliste

Vor dem Live-Gang im Browser einmal durchklicken:

- [ ] Logo ist im Hero und Footer sichtbar
- [ ] Hero-CTA „Kostenloses Angebot anfordern" springt zum Kontakt-Formular
- [ ] Beide Telefonnummern sind klickbar (Mobile: ruft an)
- [ ] E-Mail-Link öffnet Mailprogramm
- [ ] Instagram- und TikTok-Links öffnen die richtigen Profile
- [ ] Formular: Pflichtfelder validieren, Datenschutz-Checkbox blockiert ohne Häkchen
- [ ] Formspree-ID eingesetzt ODER mailto-Fallback funktioniert
- [ ] Impressum: alle TODO-Platzhalter ersetzt
- [ ] Datenschutz: alle TODO-Platzhalter ersetzt
- [ ] Mobile-Ansicht: Hamburger-Menü funktioniert, kein horizontales Scrollen
- [ ] Lighthouse (Chrome DevTools): Performance / Accessibility ≥ 90

---

## 7. Datei-Übersicht

```
webseite/
├── index.html          ← Hauptseite (alle Sections)
├── impressum.html      ← Pflicht §5 TMG
├── datenschutz.html    ← Pflicht DSGVO
├── README.md           ← diese Datei
└── assets/
    ├── logo.png        ← vom Inhaber abzulegen
    └── favicon.svg     ← Browser-Tab-Icon
```
