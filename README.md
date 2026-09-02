# Guarda in alto 🌌

Planetario interattivo da smartphone — vista in prima persona del cielo notturno e planisfero, senza installazione, senza server.

`[screenshot da inserire]`

## Cos'è

**Guarda in alto** è un planetario interattivo pensato per essere usato con lo smartphone puntato verso il cielo, sviluppato dal **Gruppo Astrofili Casalese "Giovanni Celoria"** di Casale Monferrato per l'uso durante serate osservative pubbliche e attività di divulgazione nelle scuole.

L'app ha due modalità:
- **Vista in prima persona** — proiezione realistica del cielo così come appare puntando il telefono, con navigazione touch e giroscopio
- **Planisfero** — vista dall'alto dell'intera volta celeste, utile per orientarsi e pianificare l'osservazione

Non richiede installazione: è un'unica pagina web che funziona direttamente dal browser del telefono.

## Funzionalità

- Vista cielo in prima persona con proiezione gnomonica e planisfero
- Navigazione touch e tramite giroscopio, zoom, modalità notturna a luce rossa
- Motore astronomico completo (precessione, nutazione, parallasse, aberrazione, rifrazione atmosferica), verificato contro le effemeridi NASA
- Catalogo di 2.492 stelle, 110 oggetti Messier, 265 oggetti NGC/IC, 17 stelle doppie, 20 asterismi, 85 illustrazioni mitologiche delle costellazioni
- 12 percorsi guidati di sky-hopping ed esercizi di riconoscimento degli oggetti celesti
- 23 lezioni tematiche ("Capire il cielo")
- Suggerimento "Cosa osservare stasera"
- **Eventi dei prossimi sette giorni**, calcolati e non elencati a memoria: fasi lunari, passaggi della Luna accanto a pianeti e alle stelle zodiacali brillanti, congiunzioni planetarie, opposizioni, massime elongazioni, equinozi e solstizi. Solo gli sciami meteorici hanno date fisse, perché dipendono da quando la Terra attraversa la scia di polvere lasciata da una cometa
- Diario osservativo personale con backup e ripristino
- Posizioni preimpostate per Casale Monferrato, Treville e Moleto (Barchiuso)

## Come si usa

L'app è online su **[cielo.astrofiliceloria.it](https://cielo.astrofiliceloria.it)** — basta aprire il link dal browser dello smartphone.

Note:
- Richiede connessione HTTPS per l'accesso al giroscopio (necessario per la vista in prima persona)
- Funziona anche in modalità planisfero senza giroscopio, su qualsiasi dispositivo
- I dati del diario osservativo sono salvati localmente sul dispositivo (localStorage): cambiando telefono o browser vanno esportati manualmente dalla sezione diario

## Stack tecnico

- File HTML singolo e autonomo (~880 KB), nessuna dipendenza esterna, nessun server richiesto
- Compatibile con qualsiasi browser mobile moderno

## Changelog

Le versioni e le modifiche sono documentate in [`CHANGELOG.md`](./CHANGELOG.md).

## Fonti dati e crediti

- **HYG Database v4.1** (Hipparcos/Yale, catalogo stellare) — CC BY-SA — [astronexus/HYG-Database](https://github.com/astronexus/HYG-Database)
- **OpenNGC** (catalogo oggetti del profondo cielo NGC/IC) — CC BY-SA 4.0 — [mattiaverga/OpenNGC](https://github.com/mattiaverga/OpenNGC)
- **Stellarium** (linee delle costellazioni e illustrazioni mitologiche) — Free Art License — [Stellarium GitHub](https://github.com/Stellarium/stellarium)

## Licenza

Il codice di questa applicazione è di proprietà esclusiva del **Gruppo Astrofili Casalese "Giovanni Celoria"**. Tutti i diritti riservati: non è concesso l'uso, la copia, la modifica o la ridistribuzione senza autorizzazione esplicita degli Astrofili Celoria.

I dati astronomici integrati mantengono le rispettive licenze originali, indicate sopra.

## Il Gruppo Astrofili Casalese "Giovanni Celoria"

Attivi dal 1974 a Casale Monferrato, gli Astrofili Celoria organizzano serate osservative pubbliche e attività di divulgazione astronomica per le scuole del territorio del Monferrato.

- Sito ufficiale: [www.astrofiliceloria.it](https://www.astrofiliceloria.it)
- `[link social da inserire]`
- Sostieni gli Astrofili Celoria: [donazione PayPal](https://www.paypal.com/donate/?hosted_button_id=MEKA65JC7K3UQ)
