# Vollytics — Il viaggio — Presentazione didattica

## Stack Tecnologico
- HTML5 + CSS3 (CSS Grid, Flexbox, `clamp()`, media queries)
- React 18.3.1 via CDN (unpkg.com), versione fissata (mai `@18` senza patch: un
  aggiornamento a monte di unpkg non deve poter rompere una presentazione già pubblicata)
- Babel standalone `7.26.10` (versione fissata, compatibile col preset `classic` usato)
- SVG per diagrammi e simulatori
- Google Fonts: Montserrat (titoli), Inter (testo)
- NESSUN bundler, NESSUN npm, NESSUN framework aggiuntivo. Solo file HTML standalone, apribili anche da `file://`.

## GOTCHA Babel (obbligatorio in ogni file atto/appendice)
React UMD + Babel standalone, senza preset registrato esplicitamente, può fallire in silenzio
o usare il runtime `automatic` (che richiede un import di `react/jsx-runtime` non disponibile
via CDN in questo setup). Il fix, da ripetere identico in ogni file, con le versioni fissate
sopra:
```html
<script src="https://unpkg.com/react@18.3.1/umd/react.production.min.js"></script>
<script src="https://unpkg.com/react-dom@18.3.1/umd/react-dom.production.min.js"></script>
<script src="https://unpkg.com/@babel/standalone@7.26.10/babel.min.js"></script>
<script>Babel.registerPreset('classic', { presets: [[Babel.availablePresets.react, { runtime: 'classic' }]] });</script>
...
<script type="text/babel" data-presets="classic">
  // componenti React qui
</script>
```

## Architettura file

| # | File | Titolo | Accent | Icona | Slide |
|---|------|--------|--------|-------|-------|
| — | `index.html` | Vollytics — Il viaggio | — | 🏐 | dashboard |
| 1 | `atto1-il-punto-di-partenza.html` | Il punto di partenza | coral `#EF5540` | 🎯 | 8 |
| 2 | `atto2-dal-laboratorio-al-campo.html` | Dal laboratorio al campo | sole `#F6A93B` | 🧪 | 7 |
| 3 | `atto3-quando-sembrava-funzionare.html` | Quando sembrava funzionare | rosso `#e0563a` | 📉 | 8 |
| 4 | `atto4-il-gioco-ha-le-sue-regole.html` | Il gioco ha le sue regole | teal `#1FA9A0` | 📐 | ~11 |
| 5 | `atto5-cambiare-punto-di-vista.html` | Cambiare punto di vista | azzurro `#4e93b8` | 🔄 | ~11 |
| 6 | `atto6-tocca-a-voi.html` | Tocca a voi | sabbia `#E7C58C` | ✍️ | ~10 |
| A | `appendice-a-le-parole-dell-ai.html` | Le parole dell'AI | viola `#a879c4` | 📖 | ~9 |
| B | `appendice-b-dal-prototipo-al-prodotto.html` | Dal prototipo al prodotto | verde `#6fae5c` | 🏗️ | ~7 |

- `index.html`: dashboard con le 6 card degli atti più le 2 card delle appendici (HTML/CSS puro, nessun React)
- Un file HTML per atto/appendice, completamente standalone (include React, Babel, CSS inline)
- I componenti React sono definiti dentro un tag `<script type="text/babel" data-presets="classic">`
- `assets/frames/`: fotogrammi reali dal video demo pubblico di vollytics.com, più `manifest.json`
  coi box normalizzati dei gesti, la sequenza attorno a un contatto, la traccia palla di uno
  scambio e la griglia di pixel. **Solo fonte di immagini reali ammessa**; tutto il resto
  (partite A/B, schemi, simulatori) è illustrazione SVG.
- `_lavoro/` (non pubblicato, in `.gitignore`): sceneggiatura di lavoro, non è contenuto pubblico.
- Simulatori tenuti in Atto 1/2/3 (v2, BRIEF_V2.md): pixel a 3 livelli e disegna il
  rettangolo (Atto 1); catena di montaggio e bug dell'OR (Atto 2); precision/recall e
  validazione-test (Atto 3). Eliminati: "segna tu i tocchi", "il cursore magico" (nessun
  dato reale, solo illustrativi/giocattolo — vedi BRIEF_V2.md).

## Design System (brand Vollytics, tema scuro)

```js
const colors = {
  bg: '#0e1728', bgCard: '#16233c', bgLight: '#1E2E52',
  accent: '...', accentAlt: '...', accentSolid: '...', // per atto, vedi tabella sopra e sezione contrasto
  text: '#f3f5f9', textDim: '#c9d2e0', textMuted: '#93a0b6', border: '#2a3a5c',
  coral: '#EF5540', sun: '#F6A93B', teal: '#1FA9A0', sand: '#E7C58C', navy: '#1E2E52',
  success: '#1FA9A0', danger: '#EF5540', warn: '#F6A93B', info: '#4e93b8', purple: '#a879c4'
};
```

Font: Montserrat (titoli, peso 700/800) + Inter (testo) da Google Fonts, fallback `system-ui`.

### Contrasto e accessibilità (obbligatorio, KB `UI-CONTRAST-ACCESSIBILITY.md`)

- Tutto il testo body usa `text` / `textDim` / `textMuted` su sfondo scuro (`bg`, `bgCard`,
  `bgLight`): già verificato ≥ 4.5:1.
- **I colori accent (specialmente quelli medio-chiari come il coral o il teal) NON garantiscono
  4.5:1 con testo bianco sopra come sfondo pieno di un bottone.** Verificato per il coral
  `#EF5540`: testo bianco pieno dà solo ~3.5:1 (insufficiente per testo normale AA).
  Soluzione adottata: ogni atto definisce anche un **`accentSolid`**, una versione scurita
  di circa il 20-25% dell'accent (via un tool di conversione colore, non "a occhio"), usata
  SOLO come sfondo dei bottoni pieni con testo chiaro. Esempio verificato per l'Atto 1:
  `accent: '#EF5540'` → `accentSolid: '#b83a29'` (bianco su `accentSolid` ≈ 5.7:1, passa AA).
  Replica lo stesso procedimento per gli accent degli altri atti prima di usarli su un bottone.
- `accent` e `accentAlt` restano liberi per: testo su sfondo scuro, bordi, icone, gradient
  decorativi di sfondo — mai come sfondo pieno dietro testo chiaro senza passare da `accentSolid`.
- **Due strade per il testo dei bottoni pieni**, entrambe valide, scelte per file:
  1. `bestTextColor(bgHex)` (atto2, atto3): calcola numericamente se il testo bianco o
     `colors.bg` dà più contrasto sopra un dato sfondo, e lo usa automaticamente — utile
     quando l'accent è chiaro (es. il sole `#F6A93B`) e va abbinato a testo scuro invece che
     scurirlo. Definizione di riferimento (`relLuminance`/`contrastRatio`/`bestTextColor`)
     in cima al file, prima di `colors`. Verificato per l'Atto 2: `accentSolid` resta il sole
     non scurito (`#F6A93B`, testo bianco sopra darebbe solo ≈2.0:1); `bestTextColor` sceglie
     `colors.bg` (`#0e1728`) come testo, che dà ≈9.1:1 (ottimo AA).
  2. **Coppia fissa verificata** (atto1, atto4, atto5, atto6, appendice A, appendice B):
     quando l'accent è già scuro/saturo abbastanza da reggere testo bianco una volta
     scurito in `accentSolid`, si fissa `color: '#ffffff'` (o `colors.accentText` quando
     l'accent di base è chiaro, es. la sabbia dell'Atto 6) e si documenta il rapporto
     calcolato accanto alla definizione di `colors`, invece di richiamare la funzione a
     ogni bottone. Tabella dei rapporti verificati (bianco sopra `accentSolid`, salvo dove
     indicato):
     | File | accentSolid | Contrasto |
     |------|-------------|-----------|
     | Atto 1 (coral) | `#b83a29` | ≈5.71:1 |
     | Atto 3 (rosso) | `#A8412C` | ≈6.07:1 |
     | Atto 4 (teal) | `#177f78` | ≈4.83:1 |
     | Atto 5 (azzurro) | `#3e7693` | ≈4.98:1 |
     | Atto 6 (sabbia, testo scuro `accentText` invece di bianco) | `#E7C58C` | ≈10.9:1 |
     | Appendice A (viola) | `#7e5a93` | ≈5.57:1 |
     | Appendice B (verde) | `#4d7940` | ≈5.09:1 |

     A questi si aggiunge una coppia condivisa da tutti i file: `colors.bg` (testo scuro)
     sopra `colors.success` (`#1FA9A0`, il teal pieno non scurito) per i link "Atto N →" /
     "Torna all'indice" nella bottom-bar → ≈6.18:1, verificato.
- Target di tocco ≥ 44px (bottoni, controlli dei simulatori). Label dei bottoni mai troncate:
  `white-space` libero, padding orizzontale ≥ 16px.
- Stati disabled sempre visibili (sfondo `bgLight`, testo `textMuted`, mai solo opacity ridotta
  senza cambio di colore).
- Font-size sempre con `clamp()` (variabili `--fs-*` definite in `:root`), mai `px` fissi nei
  contenuti. Verificare la leggibilità anche a text-scale 1.3 prima del rilascio.
- SVG sempre con `viewBox`, mai dimensioni fisse in px.

## Componenti riutilizzabili (da ridefinire in ogni file atto)
- `Box({ type, title, children })`: callout `info` / `warn` / `danger` / `tip` / `success`
- `Glow({ children, color })`: card con bordo luminoso gradient
- `SolidButton({ onClick, disabled, bg, children })`: bottone pieno, sfondo `accentSolid` di
  default, testo bianco — è il bottone da usare ogni volta che il testo sta sopra un colore
  pieno (CTA dei simulatori, "Avanti")
- `GhostButton({ onClick, disabled, active, children })`: bottone contornato su sfondo scuro,
  per selettori di stato (velocità, toggle) dove non serve il contrasto di un bottone pieno

## Struttura slide

Principio (v2, BRIEF_V2.md): **ogni slide (tranne titolo/chiusura) mostra un artefatto
reale — video, fotogramma, schermata dell'app, traccia, grafico o simulatore — con **max
~60 parole di testo**. Niente slide fatte di sola frase in un box nel vuoto.

Ogni atto segue questa struttura:
1. Slide titolo (centrata, icona, nome atto, titolo, sottotitolo)
2. Slide concetti/narrazione (con box informativi e, dove serve, diagrammi SVG o fotogrammi reali)
3. Slide simulatore/i interattivo/i
4. Slide di chiusura atto: UNA sola slide con titolo forte + 3 punti di recap + box con la
   lezione dell'atto + collegamento all'atto successivo (non più frase+lezione separate)

L'Atto 1 (7 slide, 2 simulatori) è il modello di riferimento: stesso motore slide, stesso
top-bar/bottom-bar, stessi componenti `Box`/`Glow`/`SolidButton`/`GhostButton`, per tutti gli
atti successivi.

## Simulatori — pattern e gotcha

- **Simulatori con animazione fisica** (es. "Segna tu i tocchi"): `requestAnimationFrame` +
  `performance.now()`, mai `setInterval` per il movimento (jank a basso frame rate). Stato
  dell'animazione tenuto anche in un `useRef` parallelo allo `useState` quando la callback
  del frame deve leggere l'ultimo valore senza richiedere la funzione ad ogni render
  (stale closure altrimenti).
- **Simulatori con disegno a trascinamento** (es. "Disegna il rettangolo"): usare i **Pointer
  Events** (`onPointerDown/Move/Up` + `setPointerCapture`), non `onMouseDown`/`onTouchStart`
  separati — coprono mouse e touch con lo stesso codice. Contenitore con `touch-action: none`
  in CSS per impedire lo scroll della pagina mentre si trascina su mobile.
- **Flag globale `simActive`**: variabile modulare (non state React) impostata a `true` mentre
  un simulatore sta "catturando" la tastiera o il puntatore (spazio per un pulsante, drag in
  corso). L'handler `keydown` dell'`App` la controlla **prima** di gestire Spazio/Frecce/ESC/
  numeri, così i simulatori non fanno avanzare la slide per sbaglio. Ricordarsi di riportarla a
  `false` sia a fine interazione sia nel cleanup di `useEffect`.
- **Dati reali da `manifest.json`**: mai un `fetch('assets/frames/manifest.json')` nei file
  pubblicati — da `file://` fallisce (CORS). I valori necessari (box dei gesti, griglia pixel,
  traccia palla) vanno **copiati come costanti JS** nel file, con un commento che indica da
  quale campo del manifest vengono e, se serve una conversione (es. rimappare un box dal
  fotogramma intero al ritaglio zoom), la formula usata.
  Esempio di rimappatura (frame intero → ritaglio zoom), usata per il box della schiacciata
  nell'Atto 1: `locale = (assoluto - zoom_window_origine) / zoom_window_dimensione`, applicata
  separatamente su x e y.

## Navigazione (uguale in ogni atto)
- Deep-link `?slide=N` nell'URL: apre l'atto direttamente sulla slide N (utile per condividere
  un link a una slide precisa, e per fare screenshot automatici in fase di verifica)
- Freccia destra / Spazio: slide successiva
- Freccia sinistra: slide precedente
- ESC: torna a `index.html`
- Tasti 1-9: salto diretto alla slide N (se esiste)
- Swipe touch (soglia 60px) su mobile
- Tutto disabilitato mentre `simActive === true`
- Barra superiore fissa (`top-bar`): link indice, nome atto, contatore slide, progress bar
- Barra inferiore fissa (`bottom-bar`): bottoni indietro/avanti, link all'indice (solo sulla
  prima slide), link all'atto successivo (solo sull'ultima slide)

## Responsive Design
- Testo sempre con `clamp()`, mai `px` fissi per font-size dei contenuti
- SVG sempre con `viewBox`, mai dimensioni fisse in px
- Media query a `max-width: 768px` in ogni file: padding ridotto, font più piccoli, gallerie e
  griglie a 1 colonna dove serve
- Container principale: `maxWidth: '1200px'`, con padding laterale responsive
- Verificare sempre anche a 390px di larghezza (telefono), oltre che desktop e tablet, e a
  finestre larghe ma basse (LIM/notebook, es. 1280×800 e 1920×1080)
- **Fotogrammi reali sempre nel loro rapporto vero (16:9 per i frame `assets/frames/*.jpg`,
  1:1 per `pixel_crop.jpg`), mai forzati in un contenitore quadrato o con `objectFit: 'cover'`
  su un box dal rapporto diverso da quello del file.** Pattern corretto per un fotogramma che
  deve restare leggibile per intero (usato dai simulatori di disegno, dove le coordinate
  disegnate sono percentuali del fotogramma): contenitore `display: 'inline-block'` (si
  dimensiona esattamente quanto l'immagine renderizzata, mai una misura indipendente) +
  `<img>` con solo `maxWidth`/`maxHeight` (mai `width`+`height` fissi insieme, mai
  `aspectRatio` forzato a un valore diverso da quello reale del file) — il rapporto del file
  decide sempre la forma finale. Per una galleria di miniature dove un piccolo ritaglio va
  bene, `objectFit: 'cover'` su un box di altezza limitata (`maxHeight`) è accettabile: lì non
  ci sono coordinate sovrapposte da allineare.
- **Nessun contenuto deve mai finire nascosto sotto la bottom-bar.** Il motore usa un layout a
  colonna flessibile a tutta altezza (classe `.app-shell`: `height: 100vh; height: 100dvh;
  display: flex; flex-direction: column;`), con top-bar e bottom-bar come voci di flusso
  normale (`flex: 0 0 auto`, MAI `position: fixed`) e l'area della slide in mezzo
  (`flex: 1 1 auto; overflow-y: auto`) che scorre da sola quando il contenuto è più alto dello
  spazio disponibile. Così, qualunque altezza assumano le barre (es. i bottoni vanno a capo su
  schermi stretti), il contenuto non finisce mai coperto: nel peggiore dei casi compare uno
  scroll interno alla slide, mai un elemento invisibile dietro una barra. In aggiunta, per le
  slide con un fotogramma grande (dove lo scroll andrebbe evitato, specie su LIM), il media
  porta anche un `maxHeight` in `dvh` (es. `42dvh`) così si riduce da solo prima di dover
  scorrere.

## Vincoli di pubblicazione (non negoziabili)

- Il paper in preparazione (progetto attuale) resta **anonimo e senza numeri**: nessun nome
  di persona, nessuna università, nessun numero o metrica assoluta del progetto — solo forme
  qualitative o relative ("gli errori si dimezzano", "circa dieci volte meglio", "da
  inutilizzabile a utilizzabile").
- La **tesi di partenza** invece si può citare per nome, per intero, in una riga piccola
  (colore `textMuted`) sotto le figure che ne derivano. Testo esatto della citazione (costante
  `THESIS_CITATION` in ogni file che la usa):
  > Fonte: A. Zammarchi, *BallVisionAI: un'applicazione di Visione Artificiale per l'analisi
  > automatizzata di partite di Beach Volley*, tesi di laurea magistrale, Università di
  > Bologna – Campus di Cesena, A.A. 2023-24.
- Figure della tesi in `assets/tesi/` (`tesi_frame_annotato.jpg`, `tesi_torneo_parigi.jpg`,
  `tesi_torneo_tepic.jpg`, `tesi_torneo_uberlandia.jpg`, `tesi_player_desktop.jpg`,
  `tesi_court2d.jpg`, `tesi_traiettoria_battuta.jpg`): **nessuna necessità di sfocatura**, sono
  materiale della tesi ormai citata per nome — vanno sempre accompagnate dalla citazione sopra.
- Fotogrammi reali del progetto attuale: solo dal video demo già pubblico sulla home di
  vollytics.com, quelli in `assets/frames/` con il relativo `manifest.json`. Tutto il resto
  (partite A e B, schemi, simulatori) è illustrazione SVG. Le partite raccontate restano
  "partita A" (ripresa dal treppiede) e "partita B" (ripresa di lato); i fotogrammi demo
  illustrano concetti generali, non vanno attribuiti a nessuna delle due.
- Nessun codice sorgente, IP, indirizzo di server, path interno o credenziale.
- Durata del lavoro raccontato: un'estate (circa cinque settimane intense), sopra le
  fondamenta di una tesi.

## Link GitHub Pages
https://thomascasali.github.io/vollytics-il-viaggio/
