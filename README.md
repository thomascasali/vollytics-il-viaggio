# Vollytics — Il viaggio

▶️ **Vedi la presentazione**: https://thomascasali.github.io/vollytics-il-viaggio/

Presentazione didattica interattiva su come nasce davvero un progetto di intelligenza
artificiale: dal video di una partita di beach volley a un sistema che riconosce giocatori,
palla, gesti e punteggio. Sei atti, oltre 10 simulatori interattivi, e la sceneggiatura fedele
a quello che è successo per davvero — errori compresi.

## A chi è rivolto

Studenti di quinta superiore, indirizzo informatico. Non serve nessun prerequisito di
intelligenza artificiale: solo curiosità per come si costruisce, passo dopo passo, un
progetto vero.

## Contenuti

| # | Titolo | Argomenti | Slide |
|---|--------|-----------|-------|
| 1 | Il punto di partenza | Il problema di un allenatore, cosa vede davvero un computer, il prototipo di tesi | 9 |
| 2 | Dal laboratorio al campo | Stadi come una catena di montaggio, il bug dell'OR, la prima web app, il primo feedback vero | 8 |
| 3 | Quando sembrava funzionare | Regole "a occhio", falsi positivi e negativi, precisione e richiamo, validazione vs test | 10 |
| 4 | Il gioco ha le sue regole | Il campo che non si vede, la proiezione in vista dall'alto, riconoscere i giocatori, la grammatica dello scambio | 11 |
| 5 | Cambiare punto di vista | Una telecamera mai vista, la fisica della parabola, il colore della palla, la distillazione circolare | 11 |
| 6 | Tocca a voi | Perché serve l'etichettatura umana, chi può insegnare alla macchina, quiz finale | 10 |
| A | Le parole dell'AI | Glossario visuale: dataset, object detection, precisione/richiamo/F1, generalizzazione | 9 |
| B | Dal prototipo al prodotto | I pezzi di un'app di AI, perché la GPU, permessi e dati, streaming o download | 7 |

## Come usarla in locale

1. Apri `index.html` nel browser (doppio click, oppure trascinalo nel browser)
2. Naviga tra gli atti cliccando sulle card della dashboard
3. Dentro un atto usa le frecce ← → (o Spazio) per andare avanti, ← per tornare indietro
4. Premi ESC per tornare all'indice, oppure un tasto da 1 a 9 per saltare direttamente a una slide
5. Nei simulatori con trascinamento funziona sia il mouse sia il touch

Non serve un server locale: essendo tutto CDN-based, funziona anche offline se il browser ha
già in cache gli script (altrimenti serve una connessione internet per caricare React/Babel/i
font da CDN).

## Tecnologie

- HTML5 + CSS3
- React 18 (via CDN, zero build)
- Babel standalone (transpiler JSX in-browser)
- Google Fonts (Montserrat, Inter)

Nessun bundler, nessun `npm install`: ogni file HTML è autonomo e si apre anche direttamente
da `file://`.

## Crediti

Fotogrammi dal video dimostrativo pubblicato su [vollytics.com](https://vollytics.com).

## Licenza

[Creative Commons Attribution 4.0 International (CC-BY-4.0)](LICENSE).
