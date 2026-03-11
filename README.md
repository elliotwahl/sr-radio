# SR Live

En enkel webbspelare för Sveriges Radios alla kanaler. Passar både som nödfallsalternativ när SR:s egna plattformar (sverigesradio.se, appen) inte fungerar — och som ett renare, enklare sätt att lyssna i vardagen.

## Syfte

SR:s webbplats och appar kan drabbas av driftstörningar, men de underliggande ljudströmmarna (som distribueras via separat infrastruktur) är ofta fortsatt tillgängliga. Den här sidan ger direkt tillgång till alla strömmar utan att gå via SR:s sajt. Men den fungerar lika bra när allt är igång — för den som föredrar en avskalad lyssnarupplevelse utan onödiga distraktioner.

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

## Byggt med AI

Hela projektet är utvecklat med **[Claude Code](https://claude.ai/claude-code)** — Anthropics AI-verktyg för mjukvaruutveckling. Kod, design och innehåll är genererade och itererade fram genom konversation med Claude, utan att någon rad skrivits manuellt.

## Licens

Inofficiellt projekt, inte affilierat med Sveriges Radio. Strömmar tillhör Sveriges Radio AB.
