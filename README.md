# Sproei Rooster

Self-contained herbouw van de oorspronkelijke Azure Sproeiplanner, nu draaiend op statische GitHub Pages met state in een GitHub Gist (geen backend, geen database).

## Hoe het werkt

- Per dag wordt 1 zone besproeid (rouleert 1 → 2 → 3 → 4 → 1).
- De buur kiest zijn naam/huisnummer en drukt op **Sproei** zodra de sproeier aan staat.
- Daarna wordt het formulier voor die dag verborgen en zien anderen "Er is vandaag al gesproeid door X in zone Y."
- Alleen de instructie-rij van de **huidige** zone wordt getoond (kraanstand + foto).
- Onderaan staat het volledige overzicht (`SproeiDatum | Door Wie | Zone`).
- Pagina ververst zichzelf na 5 minuten inactiviteit.

## Eenmalige setup

### 1. Gist aanmaken

1. Ga naar https://gist.github.com
2. Nieuwe **secret gist** met bestandsnaam `sproeiplanner.json` en inhoud `{}`
3. Kopieer het **Gist ID** uit de URL (`https://gist.github.com/<user>/<GIST_ID>`)

### 2. Personal Access Token

1. https://github.com/settings/tokens
2. Maak een token met scope `gist` (classic) of fine-grained met gist read/write
3. Bewaar de token

### 3. Namen invullen

Pas in `index.html` de constante `NAMES` aan:

```js
const NAMES = [
  'van Beelen (26)',
  'Bolk-Bulthuis (30)',
  'Helvoirt en Klein (36)',
  // ...
];
```

### 4. Foto's plaatsen

Zet de volgende JPGs in `images/`:

- `kraan.jpg` — kraan met timer
- `zone1.jpg` t/m `zone4.jpg` — kraanstand per zone

De zone-rij gebruikt de foto van de **huidige** zone.

### 5. Deploy

Push naar `main`. Zet GitHub Pages aan (Settings → Pages → Source: GitHub Actions). De workflow in `.github/workflows/pages.yml` doet de rest.

### 6. Eerste keer gebruiken

Open de gepubliceerde URL → vul Token + Gist ID in (eenmalig, blijft in `localStorage` van het apparaat).

## Kraanstanden per zone

Uit de originele app:

| Zone | Stand        |
|------|--------------|
| 1    | — — — &#124; |
| 2    | &#124; &#124; — &#124; |
| 3    | &#124; — &#124; &#124; |
| 4    | &#124; — — — |

## Bestanden

- `index.html` — de app
- `style.css` — styling (rode message-banner, groene tabel-header, etc.)
- `images/` — foto's
- `.github/workflows/pages.yml` — GitHub Pages deploy

## Beveiliging

De PAT met `gist`-scope wordt lokaal per apparaat bewaard. Elke buur moet hem eenmalig invoeren. Voor minimaal risico: maak een aparte GitHub-account aan met alleen deze ene gist en deel die token.
