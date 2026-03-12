# Vevradion

En enkel webbspelare för Sveriges Radios alla kanaler. Passar både som nödfallsalternativ när SR:s egna plattformar (sverigesradio.se, appen) inte fungerar — och som ett renare, enklare sätt att lyssna i vardagen.

**Live:** [sr.liot.se](https://sr.liot.se)

---

## Bakgrund

En dag låg Sveriges Radios webbplats och app nere. De underliggande ljudströmmarna fungerade däremot utmärkt — SR distribuerar dem via en separat infrastruktur som inte påverkas av problem i den publika sajten. Vevradion byggdes samma dag för att ge direkt tillgång till strömmarna utan att gå via SR:s egna plattformar.

---

## Kanaler

Totalt 36 kanaler i tre grupper:

**Rikskanaler** — P1, P2, P3, P6

**Övriga kanaler** — Ekot direkt, P3 Din gata, P4 Plus, P4 Digital, Knattekanalen, Sameradion, SR Finska

**Lokala P4-kanaler** — Alla 25 regioner: Blekinge, Dalarna, Gotland, Gävleborg, Göteborg, Halland, Jämtland, Jönköping, Kalmar, Kristianstad, Kronoberg, Malmöhus, Norrbotten, Sjuhärad, Skaraborg, Stockholm, Sörmland, Uppland, Värmland, Väst, Västerbotten, Västernorrland, Västmanland, Örebro, Östergötland.

---

## Hur strömmarna fungerar

Alla strömmar hämtas direkt från SR:s publika streamingservrar via URL-mönstret:

```
https://live1.sr.se/{kanal-id}-{format}-{bithastighet}
```

Exempel: `https://live1.sr.se/p1-aac-128`

Fyra kvalitetsnivåer stöds:

| Kvalitet | Format | Användning |
|---|---|---|
| 32 kbps | HE-AAC-V2 | Mobildata, låg förbrukning |
| 96 kbps | MP3 | Bred kompatibilitet |
| 128 kbps | AAC-LC | Standard, bra balans |
| 320 kbps | AAC-LC | Högsta kvalitet |

P2 har dessutom en unik FLAC-ström för förlustfri kvalitet: `live1.sr.se/p2-flac`.

### Inget innehåll hostas här

Vevradion varken lagrar eller vidaredistrbuerar något ljudinnehåll. Det närmaste analogin är att bädda in en YouTube-video: när du klickar på en kanal skickar din webbläsare en förfrågan direkt till SR:s streamingservrar på `live1.sr.se` — Vevradion är aldrig inblandad i den kommunikationen. Allt som vår server levererar är HTML, CSS och JavaScript. Ljudet kommer alltid från SR.

Mer precist: webbläsarens `<audio>`-element öppnar en HTTP-anslutning mot SR:s server och läser en kontinuerlig dataström. SR:s server bestämmer vad som sänds och kan när som helst avbryta eller byta innehåll. Det skiljer sig från en nedladdning (där en fil kopieras) — här är det ett flöde i realtid som aldrig passerar våra servrar.

Uppspelningen sker via webbläsarens inbyggda `<audio>`-element. Ingen proxy, ingen mellanhand — webbläsaren ansluter direkt till SR:s streamingservrar. Ingen inloggning, inga cookies, ingen tracking.

---

## Projektstruktur

Projektet är medvetet enkelt:

```
sr-radio/
├── index.html          # Hela applikationen — HTML, CSS och JS i en fil
├── om.html             # Om-sida med bakgrund och syfte
├── installera.html     # Guide för installation på hemskärm
├── changelog.html      # Ändringslogg, uppdateras automatiskt
├── apple-touch-icon.png
├── favicon.svg
├── favicon.png
├── .github/
│   └── workflows/
│       └── changelog.yml   # GitHub Actions: uppdaterar changelog vid push till main
└── README.md
```

Det finns inget byggsteg, inga paket och inga externa beroenden utöver Google Fonts. Allt lever i statiska HTML-filer som kan hostas varsomhelst.

---

## index.html — applikationens hjärta

Hela spelaren ryms i en enda fil. Strukturen inuti den:

### CSS-variabler och tema

Alla färger definieras som CSS-variabler i `:root` — `--bg`, `--surface`, `--accent` och så vidare. Det gör det enkelt att justera temat på ett ställe.

### Kanaldata (JavaScript)

Kanalerna definieras som tre arrays direkt i skriptet:

```js
const NATIONAL = [{ id: 'p1', name: 'P1' }, ...]
const SPECIAL  = [{ id: 'ekotdirekt', name: 'Ekot direkt' }, ...]
const LOCAL    = [{ id: 'p4sth', name: 'P4 Stockholm' }, ...]
```

`id` matchar exakt den del av SR:s stream-URL som identifierar kanalen.

### Flerspråksstöd (i18n)

Ett `LANGS`-objekt innehåller översättningar för alla 10 språk SR sänder på. Varje språk har ett `s`-objekt med gränssnittstext, en `dir`-egenskap (`ltr`/`rtl`) och ett valfritt `channelId` för att markera vilken kanal som sänder på det språket.

```js
const LANGS = {
  sv: { name: 'Svenska', dir: 'ltr', s: { wordmark: 'Vevradion', idle: 'Välj en kanal', ... } },
  ar: { name: 'العربية', dir: 'rtl', s: { ... } },
  ...
}
```

Funktionen `setLang(code)` uppdaterar alla textelement i DOM:en, sätter `document.documentElement.lang` och `dir` (för RTL-stöd), och visar eventuell AI-översättningsbanner.

### Uppspelning

Spelaren använder ett enda `<audio>`-element. Funktionen `streamUrl(ch, q)` bygger rätt URL utifrån vald kanal och kvalitet. `setState(state)` synkroniserar UI — statusdot, VU-meter och play/paus-ikon — med ljudets faktiska tillstånd via `audio`-eventsen `playing`, `pause`, `waiting`, `stalled` och `error`.

### Mobilläge

På skärmar upp till 720 px döljs desktop-kontrollen och ersätts av:

- En **fast bottom player** med kanalnamn, mini-VU-meter och play/paus-knapp
- En **fliknavigering** (Riks / Lokala / Övrigt) som visar en sektion i taget
- En **inställningspanel** (`position: fixed`) som öppnas uppåt ovanför spelaren

### Tillgänglighet (WCAG 2.1 AA)

- Skip-link för tangentbordsnavigering
- Korrekt rubrikhierarki (`h1`, `h2`)
- `aria-live` på statusetiketten
- `aria-pressed` på kanal- och kvalitetsknappar
- `aria-current` på aktiv mobilflik
- `aria-label` på alla ikontryck
- `prefers-reduced-motion` stänger av animationer
- Synliga fokusindikatorer via `:focus-visible`

---

## Hosting och deployment

Primär domän: **[sr.liot.se](https://sr.liot.se)** — pekar mot Netlify via CNAME.

Repo: `github.com/elliotwahl/sr-radio`, branch `master` → GitHub Pages (spegelsajt).

Utveckling sker i branchen `beta`. När en feature är klar mergas den till `master`, varpå sajten automatiskt driftsätts. En GitHub Actions-workflow körs vid varje push till `master` och uppdaterar `changelog.html` med de commits som ingick i releasen.

---

## Byggt med AI

Hela projektet är utvecklat med **[Claude Code](https://claude.ai/claude-code)** — Anthropics AI-verktyg för mjukvaruutveckling. Kod, design och innehåll är genererade och itererade fram genom konversation med Claude, utan att någon rad skrivits manuellt.

---

## Licens

Inofficiellt projekt, inte affilierat med Sveriges Radio. Strömmar tillhör Sveriges Radio AB.
