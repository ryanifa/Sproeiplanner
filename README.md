# Sproei Rooster

Self-contained webapp om met de buren een sproeischema bij te houden. State (geschiedenis + volgende zone) wordt gesynchroniseerd via een GitHub Gist, zodat alle gebruikers dezelfde data zien.

## Hoe het werkt

- Per dag wordt 1 zone besproeid (1 → 2 → 3 → 4 → 1 ...).
- De buur selecteert zijn/haar naam en drukt op **Sproei** zodra de sproeier aan staat.
- Daarna wordt de knop voor die dag verborgen en zien anderen "De sproeier is vandaag al aangezet."
- De volgende dag rouleert de zone +1 automatisch.
- Onderaan staat het overzicht van wie wanneer welke zone heeft besproeid.

## Eenmalige setup

### 1. Gist aanmaken

1. Ga naar https://gist.github.com
2. Maak een nieuwe **secret gist** met bestandsnaam `sproeiplanner.json` en inhoud `{}`
3. Kopieer het **Gist ID** uit de URL (bv. `https://gist.github.com/jouwnaam/<GIST_ID>`)

### 2. Personal Access Token

1. Ga naar https://github.com/settings/tokens
2. Maak een **Fine-grained** of **Classic** token aan met scope `gist`
3. Bewaar de token (begint met `ghp_` of `github_pat_`)

### 3. Namen instellen

Bewerk in `index.html` de constante `NAMES`:

```js
const NAMES = [
  'van Beelen (26)',
  'Bolk-Bulthuis (30)',
  'Helvoirt en Klein (36)',
  // ...
];
```

### 4. Foto's plaatsen

Zet de volgende bestanden in `images/`:

- `kraan.jpg` — foto van de kraan met de timer
- `zone1.jpg` — kraanstand voor zone 1
- `zone2.jpg` — kraanstand voor zone 2
- `zone3.jpg` — kraanstand voor zone 3
- `zone4.jpg` — kraanstand voor zone 4

Ontbrekende foto's tonen automatisch een placeholder.

### 5. Zone-stand notatie

In `index.html` de constante `ZONE_STANDEN` aanpassen aan jouw kraanstanden (welke kraan moet open per zone).

### 6. Deploy

Push naar `main` en zet GitHub Pages aan:

- Repo → **Settings** → **Pages** → Source: **Deploy from a branch** (`main` / root) of **GitHub Actions**.

Open de gepubliceerde URL, vul bij eerste gebruik je token + Gist ID in. Deze worden alleen in `localStorage` van het apparaat bewaard.

## Beveiliging

- De PAT met `gist`-scope wordt lokaal opgeslagen per apparaat. Iedere buur die de app gebruikt, moet zijn eigen token invoeren (of jouw token delen — in dat geval kunnen zij ook in andere gists van die account).
- Voor het laagste risico: maak een aparte GitHub account aan met alleen deze ene gist en deel die token.

## Bestanden

- `index.html` — de hele app
- `images/` — zone-foto's en kraanfoto
- `README.md` — dit bestand
