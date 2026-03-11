# SR Live

En enkel webbspelare för Sveriges Radios alla kanaler — byggd som ett nödfallsalternativ när SR:s egna digitala plattformar (sverigesradio.se, appen) inte fungerar.

## Syfte

SR:s webbplats och appar kan drabbas av driftstörningar, men de underliggande ljudströmmarna (som distribueras via separat infrastruktur) är ofta fortsatt tillgängliga. Den här sidan ger direkt tillgång till alla strömmar utan att gå via SR:s egen sajt.

## Kanaler

Alla rikskanaler (P1, P2, P3, P6), lokala P4-kanaler för alla 25 regioner samt övriga kanaler som Ekot direkt, P3 Din gata, P4 Plus, Knattekanalen, Sameradion och SR Finska.

## Hur det fungerar

Sidan hämtar ljud direkt från SR:s publika streamingservrar på `live1.sr.se`. Länkarna är offentligt dokumenterade av SR själva. Ingen inloggning, inga cookies, ingen tracking.

Fyra kvalitetsnivåer väljs i spelaren:
- **32 kbps** — HE-AAC-V2, lägst dataförbrukning
- **96 kbps** — MP3
- **128 kbps** — AAC-LC, bra balans (standard)
- **320 kbps** — AAC-LC, högst kvalitet

## Teknisk stack

En enda HTML-fil utan externa beroenden eller byggsteg. Primärt hostad på **[sr.liot.se](https://sr.liot.se)**, med spegling via GitHub Pages.

## Licens

Inofficiellt projekt. Strömmar tillhör Sveriges Radio.
