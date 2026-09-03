# Changelog — Guarda in alto

Planetario interattivo del Gruppo Astrofili Casalese "Giovanni Celoria", Casale Monferrato.

---

## v5.0

Versione pubblica che raccoglie tutto il lavoro svolto a partire dalla 3.0. Per chi usa l'app è un aggiornamento solo: il riquadro delle novità nel menù elenca perciò le implementazioni principali dell'intero ciclo, non le singole voci di sottoversione, che non direbbero nulla di utile a chi apre l'app.

**Le novità principali dalla 3.0**

- **Realtà aumentata**: il cielo calcolato sovrapposto all'immagine della fotocamera
- **Calibrazione della bussola** su un astro di posizione nota o a mano, che corregge anche il puntamento fuori dalla realtà aumentata e recupera la declinazione magnetica, mai considerata prima
- **Le tue ottiche**: si registrano binocolo e telescopio e l'app calcola e disegna il campo che inquadrano davvero, con la possibilità di restringere la vista fino a quel campo
- **Cinque percorsi lunghi** per imparare a leggere il cielo di ogni stagione, oltre ai dodici brevi
- **Riconoscimento delle costellazioni** dentro i percorsi: non più solo trovare una stella, ma vedere la figura di cui fa parte
- **Memoria dei percorsi**: quali sono stati completati e da dove riprendere
- **Curiosità verificate** su ogni stella, pianeta e costellazione del quiz
- **Oggetti del profondo cielo in forma e dimensione reali**
- **Menù che si adatta al livello**: chi comincia trova 6 gruppi invece di 15
- **Dimensione del testo regolabile**, utile al buio durante le serate

---

**Pulizia del codice prima del rilascio**

Scansione completa, ripetuta fino a esito senza anomalie.

**Un bug funzionale che nessuno aveva notato.** Il markup aveva `id="noteLuogo"` mentre il codice cercava `luogoNota`: la nota accanto a «Dove e quando osservi» era quindi congelata sul testo statico e non seguiva più il cambio di località. Introdotto accorpando i gruppi nella 4.1, e invisibile perché il codice è protetto e non produceva errori.

**Riepiloghi ripristinati.** Sempre dall'accorpamento della 4.1 erano sparite le note di riepilogo dei gruppi fusi. Ora «Dove e quando osservi» mostra luogo e ora insieme, e «Il cielo che vedi» ha di nuovo la sua nota — era rimasto l'unico gruppo del menù a non averne una. La qualità del cielo non è stata aggiunta alla nota del luogo di proposito: allungherebbe la riga fino a mandare il titolo a capo, cioè il difetto corretto nella 4.8.2.

**Codice non più pertinente, rimosso:**

- `skyCtxTest()`, funzione di collaudo mai chiamata
- Il blocco che copiava il logo in `#gruppoLogo`, elemento non più esistente. Il logo del Gruppo è incorporato direttamente nel markup e si visualizza correttamente, quindi non andava reinserito: andava solo ripulito il riferimento morto
- L'elemento `.tooltip-bubble`, interfaccia mai implementata e per giunta priva di senso su smartphone, dove non esiste un puntatore da passare sopra gli elementi
- Sei regole CSS orfane: `.hamburger-btn`, `.help-note`, `.rt-ok` (rimasta dalla 4.6, quando lo stato «in cielo adesso» è stato tolto), `.nv-v` e `.nv-d` di una vecchia resa del changelog, `.chisiamo-separatore` della struttura precedente alla 4.7

**Verifica finale:** zero riferimenti a elementi inesistenti, zero identificatori orfani, zero funzioni mai chiamate, zero regole CSS morte, zero duplicati, zero residui di sviluppo.

---

## v4.8.2

La dimensione del testo copre ora anche il menù e i suoi sottotesti: titoli dei gruppi, nomi e descrizioni delle opzioni, note, strumenti registrati, avviso di novità.

In verifica è emerso un bug distinto, non legato all'estensione in sé: `.group-title` aveva `flex: 0 0 auto` **senza** `min-width: 0`. Nel modello flessibile questo impedisce a un elemento di restringersi sotto la sua larghezza intrinseca — lo stesso bug già risolto altrove con `.opt-text{min-width:0}`, qui sfuggito perché il titolo di un gruppo non aveva mai avuto bisogno di restringersi finché il testo era piccolo. A testo ingrandito, un titolo lungo affiancato a una nota lunga («Dove e quando osservi» + «Casale Monferrato») faceva traboccare il pulsante invece di andare a capo.

Verificato il non-traboccamento su ogni nodo dell'intero menù aperto, non a campione, a tutte e tre le scale, comprese le liste generate dinamicamente.

---

## v4.8.1

Due difetti nel comando «Dimensione del testo», trovati subito dopo la consegna della 4.8: i pulsanti sembravano non fare nulla.

La causa non era il meccanismo — click e stato funzionavano già correttamente — ma l'assenza di qualunque riscontro visibile nella stessa schermata: la scala tocca le schede di lettura, non il menù, quindi cliccando non cambiava nulla sotto gli occhi. Aggiunta un'anteprima dal vivo subito sotto i pulsanti, che si ingrandisce insieme al resto.

Scrivendo quell'anteprima è emerso un secondo difetto di natura diversa: due escape in stile JavaScript lasciate dentro testo HTML statico, dove non vengono interpretate e comparivano lettera per lettera. Corretto scrivendo il carattere accentato vero e usando l'entità HTML per l'apostrofo. Controllato l'intero file per la stessa classe di errore fuori da `<script>`: nessun'altra occorrenza.

---

## v4.8

**Dimensione del testo regolabile**

Tre passi — normale, grande, molto grande — sulle schede da leggere. Serve alle serate pubbliche, dove il telefono passa di mano al buio, spesso a persone anziane, e il filtro rosso della modalità notturna abbassa ulteriormente il contrasto.

**Scala solo le superfici di lettura, non tutta l'interfaccia**

Il file aveva **141 dichiarazioni `font-size` in px e nessuna in `rem`**: non esisteva alcuna scala tipografica, ogni dimensione era fissa e indipendente. Scalarle tutte avrebbe ingrandito anche etichette della barra strumenti, badge e note da 9px pensate per stare in spazi fissi, facendo traboccare i contenitori e raddoppiando la lunghezza del menù appena compattato nella v4.6.

Sono state quindi convertite **33 dichiarazioni mirate** a `calc(Npx * var(--txt-scala))`: riquadro dell'oggetto selezionato, schede dei percorsi e del quiz, eventi, lezioni, diario, elenchi. Cioè dove si *legge*, non dove si *comanda*.

Tre passi discreti invece di un cursore continuo, per una ragione pratica: tre valori si possono verificare tutti e tre, un cursore continuo no.

**Non è legata alla modalità notturna**

Chi ha bisogno di caratteri grandi ne ha bisogno sempre, non solo al buio. L'impostazione sta in «Il cielo che vedi», sezione *Come è disegnato*, ed è visibile già al livello Principiante: è accessibilità, non un'opzione avanzata.

**I nomi disegnati sul cielo restano invariati**

Ingrandire anche quelli renderebbe la mappa illeggibile per sovrapposizione delle etichette, che è il contrario dell'obiettivo.

**Difetto trovato in verifica e corretto**

A 1,4× la barra informativa traboccava: la data in carattere monospazio occupava 114 px in una cella da 106. Le celle vanno ora **a capo** invece di comprimersi. Escludere la barra dalla scala sarebbe stata la scorciatoia, ma avrebbe lasciato piccolo proprio un testo che si deve leggere.

Verificate tutte e tre le scale su ogni superficie toccata, non a campione, controllando sia il traboccamento interno sia l'uscita dai bordi dello schermo a 390 px.

---

## v4.7

**Le costellazioni evidenziate ora seguono le linee del catalogo**

Correzione di un errore introdotto nella v4.5: le figure usate dai passi di riconoscimento erano state **scritte a mano** invece di essere ricavate da `LINES`, il database che l'app usa già per disegnare le costellazioni in cielo.

Il risultato erano due tracciati diversi sovrapposti. Il Drago, l'Aquila, il Sagittario ed Ercole mostravano l'evidenziazione su un percorso e le linee vere su un altro. Cefeo, che nel percorso circumpolare non aveva alcun passo di riconoscimento, non veniva evidenziato affatto.

**La correzione, e perché è strutturale e non un ritocco**

`FIG` è ora costruito a runtime da `LINES`. Un segmento appartiene alla figura se almeno una delle due stelle è della costellazione: così restano incluse le linee di confine con una stella condivisa, come Elnath fra Toro e Auriga, che appartiene legittimamente a entrambe le figure.

La differenza non è di accuratezza ma di natura: derivando il dato dalla fonte, l'incoerenza fra evidenziazione e disegno diventa **impossibile per costruzione**, non solo improbabile. Ricopiare a mano un dato che l'app possiede già era esattamente l'errore.

**Cefeo**

Aggiunto il passo di riconoscimento che mancava, con la sua figura a casetta col tetto a punta rivolto verso la Polare.

**Due etichette adeguate**

La figura del Sagittario è ora l'intera costellazione e non la sola Teiera; quella di Pegaso l'intera costellazione e non il solo Quadrato. I testi che le accompagnano sono stati adeguati di conseguenza, mantenendo il riferimento all'asterismo come parte più riconoscibile.

**Verifica a tappeto**

Controllati tutti e 17 i percorsi: 44 passi di riconoscimento, ogni singolo segmento verificato presente in `LINES`, nessuna figura vuota, nessuna costellazione citata nei testi senza il proprio passo di riconoscimento.

---

## v4.6

**L'elenco dei percorsi riorganizzato**

Con 17 voci l'elenco era arrivato a 1646 px: **2,4 schermate di scorrimento** per la sola lista, per giunta annidata due livelli dentro il menù.

**Il difetto non era solo la lunghezza**

L'elenco mescolava due intenti diversi. I percorsi lunghi si scelgono in base a quanto si vuole imparare; i brevi in base a cosa c'è in cielo stasera. Messi in fila, con sei intestazioni stagionali in mezzo, nessuno dei due criteri funzionava — e ogni percorso aggiunto in futuro avrebbe allungato la stessa lista invece di entrare in una delle due famiglie.

Ora sono due sottogruppi apribili, **«Imparare a leggere il cielo» (5)** e **«Percorsi brevi» (12)**. La fisarmonica chiude i gruppi fratelli, quindi non possono mai essere aperti insieme: **0,5 e 1,2 schermate invece di 2,4**.

**Voci compattate**

Da 88 a 61 px per voce: nome e stato di avanzamento stanno ora su una riga sola invece che su due.

**Lo stato di visibilità solo quando dice qualcosa**

Compariva su tutte e 17 le voci, ma in gennaio 12 dicevano «in cielo adesso»: quella riga non informava, occupava spazio. Ora compare **solo quando l'oggetto non è comodamente osservabile** — sotto l'orizzonte, o troppo basso. In gennaio scende da 17 voci a 4.

**Nota di riepilogo**

Accanto al titolo di ogni sottogruppo compare quanti percorsi contiene e, se ne hai completati, quanti — «1 di 5 fatti».

---

## v4.5

**I percorsi guidati diventano un percorso di apprendimento**

Da 12 a 17 percorsi, ma soprattutto da elenco piatto a struttura che accompagna.

**Il limite del motore, e perché contava**

La figura di riferimento era **una sola per percorso**, fissa dall'inizio alla fine. Si poteva quindi insegnare a *trovare* una stella, ma non a riconoscere la costellazione di cui fa parte: il percorso su Arturo arrivava ad Arturo e Spica come punti luminosi, senza mai nominare Boote e citando la Vergine solo di sfuggita. Si imparava a trovare due puntini, non a leggere quella zona di cielo.

Ora la figura è un dato del **singolo passo** (nuovo tipo `figura`), e le figure già incontrate restano disegnate attenuate, come impalcatura da cui si è arrivati.

Una conseguenza da non sbagliare: `suFigura()` ora controlla **tutte** le figure attive, non solo quella del percorso. Altrimenti un tratto che corre lungo il lato di una costellazione appena insegnata sarebbe stato tratteggiato pur essendo visibile in cielo, invertendo la convenzione grafica su cui si regge tutto il sistema — linea piena dove il cielo la mostra davvero, tratteggiata dove l'occhio deve immaginarla.

**43 passi di riconoscimento**

Aggiunti sia ai 12 percorsi brevi sia ai 5 nuovi. Le figure delle 23 costellazioni stanno in un blocco unico riusabile, non ripetute dentro ogni percorso: una correzione vale ovunque, e non restano versioni divergenti della stessa costellazione in punti diversi del file.

**Cinque percorsi lunghi: uno circumpolare e quattro stagionali**

Non insegnano un trucco, insegnano a leggere una porzione di cielo. Il cielo che non tramonta mai; leggere il cielo d'inverno, di primavera, d'estate, d'autunno.

Sono **scritti a sé e non composti dai brevi**, ed è una scelta deliberata: i brevi hanno aperture (`ancora`) e chiusure (`meta`) autonome, scritte per stare in piedi da sole. Incatenarli avrebbe prodotto cinque «cerca il Grande Carro» rivolti a chi lo sta guardando da dieci minuti, e cinque «hai finito» prima della fine. Il collegamento fra le due famiglie passa invece dai rimandi.

**Memoria dei progressi**

Il quiz teneva punteggio e storico, il diario registrava le osservazioni, i percorsi non registravano nulla: si facevano e spariva tutto. Ora l'avanzamento si salva a ogni passo, l'elenco mostra cosa è già completato e da dove si riprende, e il progresso **non regredisce** rivedendo un passo precedente. Serve soprattutto ai percorsi lunghi, che durano più di una serata.

**Rimandi fra le sezioni**

In fondo a ogni percorso compaiono i collegamenti ai percorsi correlati, all'esercizio e alle lezioni. Erano tre isole senza alcun ponte: chi finiva un percorso non aveva alcun invito a mettere alla prova quello che aveva appena imparato.

**Menù: «Chi siamo» torna diviso in sezioni**

Con l'accorpamento della v4.1 era diventato un unico blocco da 16,8 KB con tre soli titoletti piatti, il più lungo del menù. E i titoli di sezione (10px, monospace, attenuati) erano **più piccoli del testo che intestavano**: quello stile era nato come etichetta sopra un gruppo di interruttori, non come intestazione di contenuto. Tornano quattro sottogruppi apribili, con lo stesso stile già usato dentro «Impara il cielo».

**Verifiche**

Ogni chiave di ogni figura e di ogni passo è controllata contro il catalogo da uno script dedicato: una chiave sbagliata non produce errori, **spezza la linea in silenzio**, che è peggio. Ne è emersa una (`etaper`, che in catalogo si chiama `miram`). Verificati anche tutti i 115 passi per centro di puntamento calcolabile e testo presente, e la resa grafica per campione con screenshot reali su ogni tipo di passo.

---

## v4.4

**Curiosità completate: copertura piena su ogni oggetto del quiz**

Secondo e ultimo lotto: le restanti 64 stelle candidate (magnitudine da 1,3 a 2,6) e le 23 costellazioni ammissibili. Sommato al primo lotto della v4.3, **ora ogni oggetto che il quiz può effettivamente proporre ha una curiosità collegata** — verificato eseguendo `quizCandidati()` su quattro date diverse dell'anno e su una sessione di quiz reale di più domande consecutive, non solo controllando che le voci esistano nella tabella.

**Le costellazioni, come deciso, raccontano il loro mito**

Perché Orione e lo Scorpione non sono mai visibili insieme in cielo. Chi è davvero il centauro del Sagittario. Perché Cassiopea, nel mito punita per la sua vanità, ruota a testa in giù per metà dell'anno. Ventitré aneddoti, uno per costellazione, coerenti con le illustrazioni mitologiche che l'app disegna già.

**Un difetto minore corretto per strada**

Verificando la copertura è emerso che `CON_NAMES` non aveva la traduzione italiana di due costellazioni, Lupo (`Lup`) e Poppa (`Pup`): comparivano nel quiz e nel resto dell'app con la sola sigla a tre lettere invece del nome. Non è stato introdotto da questo lavoro, ma era lì da correggere ed è stato sistemato insieme.

**Fatti verificati anche in questo lotto**

Fra i più delicati: l'improvviso aumento di luminosità di Dschubba nel 2000, dovuto all'espulsione di un anello di gas dalla stella; il fatto che Adhara, circa 4,7 milioni di anni fa, sia stata probabilmente la stella più brillante mai apparsa nel cielo notturno terrestre; la rotazione di Regolo, così veloce da deformarla del 30% rispetto a una sfera perfetta.

**Riepilogo fuori dall'app**

Consegnato un file separato con tutte le 108 curiosità (78 stelle, 7 pianeti, la Luna, 23 costellazioni) leggibili in un colpo solo, per la revisione.

---

## v4.3

**Curiosità nel quiz «Mettiti alla prova»**

Un fatto per oggetto, mostrato nella scheda di esito **dopo** aver risposto, non prima: l'indizio già esistente serve ad aiutare a trovare il bersaglio, la curiosità serve a lasciare qualcosa da ricordare anche dopo aver chiuso l'app. Sono due scopi opposti, e restano visivamente distinti — colore ciano e icona ✨ per la curiosità, contro il dorato dell'indizio — apposta per non confonderli.

**Perché a lotti, e non tutto insieme**

I candidati reali del quiz sono circa 90: 77 stelle nominate sotto la soglia di magnitudine 2,6, le costellazioni che superano il filtro di compattezza, 7 pianeti, la Luna. Scriverli e verificarli tutti in una sessione sola avrebbe significato lavorare stanchi verso la fine, con il rischio concreto di infilare un fatto sbagliato fra i tanti giusti. La tabella `CURIOSITA` è separata dalla logica di `quizCandidati()` apposta per poter essere ampliata a pezzi: un oggetto senza voce non mostra nulla, esattamente come già faceva `indizio` quando non serviva.

**Primo lotto: le 14 stelle più luminose, i 7 pianeti, la Luna**

Sirio, Arturo, Vega, Capella, Rigel, Procione, Achernar, Betelgeuse, Altair, Aldebaran, Antares, Spica, Polluce, Fomalhaut — tutte con magnitudine 1,2 o più luminosa, quindi anche le più frequenti nelle prime domande del quiz. Ogni fatto è stato verificato prima di essere scritto, non ricavato a memoria: la regola vale anche per il testo divulgativo, non solo per le coordinate astronomiche.

La scelta fra fatto fisico e aneddoto storico è stata fatta caso per caso, secondo cosa rendeva di più su quell'oggetto specifico — per esempio la storia della scoperta della compagna nana bianca di Sirio, o il significato del nome di Antares, «rivale di Marte».

**Le costellazioni restano vuote in questo lotto**

La decisione presa è di legarle al mito, non alla forma o alla posizione, per restare coerenti con le illustrazioni mitologiche che l'app ha già. Meritano la stessa cura del resto e sono rimandate a un lotto successivo.

---

## v4.2

**Onboarding e avviso di novità, rimasti indietro rispetto alle modifiche recenti**

Il passo finale della presentazione iniziale elencava ancora «Cosa vedo stasera, Impara il cielo, Cerca»: nessuna parola sulla realtà aumentata, nessun nome aggiornato dei gruppi dopo l'accorpamento della 4.1. Riscritto per rispecchiare il menù vero, con la realtà aumentata nominata esplicitamente e marcata beta.

**Nuovo passo: «Le tue ottiche» non è la difficoltà osservativa**

I due concetti si somigliano nel nome e arrivano in sequenza nell'onboarding — prima si sceglie con che cosa si osserva (occhio nudo/binocolo/telescopio, che decide solo quali oggetti mostrare), subito dopo esisterebbe la tentazione di confonderla con «Le tue ottiche» (le focali dello strumento vero, registrate per calcolare il campo). Aggiunto un passo breve e dedicato a distinguerli, finché l'utente li ha ancora entrambi in mente.

**Avviso di novità al riavvio**

Le voci «Novità» e «Tutte le versioni» esistevano già, ma erano passive: comparivano solo se l'utente andava a cercarle. Chi riapriva l'app dopo settimane non aveva modo di sapere che qualcosa era cambiato.

Ora si confronta la versione vista l'ultima volta con quella corrente. Se sono diverse, compaiono un pallino sul pulsante Controlli e, aprendo il menù, una riga di riepilogo con le voci vere di quella versione e un pulsante «Ho capito» che lo chiude. Nessun overlay imposto: l'onboarding è invasivo di proposito, e solo alla prima apertura; questo resta un segnale che si può ignorare.

**Caso limite corretto in verifica**

Chi aveva già superato l'onboarding ma non aveva mai avuto una «versione vista» salvata — cioè chiunque avesse installato l'app prima che questo avviso esistesse — veniva escluso dal controllo, perché il confronto con un valore assente non scattava. Non aveva visto la versione corrente più di chiunque altro: va trattato come «ancora da vedere», non come esente. Corretto prima della consegna.

---

## v4.1

**Il menù si adatta al livello scelto**

Il menù era cresciuto a 15 gruppi e 33 righe di opzioni: un principiante che cercava «fammi vedere qualcosa» doveva scorrere accanto a «Griglia equatoriale» e «Campo apparente dell'oculare». Ora **chi comincia trova 6 gruppi e 8 righe; l'intero menù gli entra in una schermata senza scorrere**. A livello Intermedio le righe diventano 18, ad Avanzato 24.

Non ci sono tre menù diversi da tenere allineati: c'è un solo menù, con il livello minimo dichiarato su ogni gruppo, sezione e riga.

**Filtrare per gruppo non bastava.** «Come appare il cielo» aveva nove interruttori, di cui a un principiante ne servono tre: o gli si negavano i nomi delle stelle, o gli si dava anche «Tieni acceso lo schermo». Il filtro agisce quindi anche sulle singole righe.

**Separato il livello di interfaccia da quello del cielo**

Era una trappola pronta a scattare: ogni interruttore impostava `state.level = null` («da qui in poi è una scelta personale»). Se il menù avesse letto quel valore, sarebbe cambiato forma sotto le dita dell'utente appena toccava qualcosa. Ora `state.livello` governa il menù e non si azzera; `state.level` continua a governare il cielo e ad azzerarsi alla prima personalizzazione.

**Accorpamenti: da 22 titoli a 7 gruppi**

| Prima | Ora |
|---|---|
| Dove ti trovi + Quando + Il tuo cielo | **Dove e quando osservi** |
| Cosa posso osservare + Tipi di oggetto + Come appare il cielo + Righe di riferimento + Legenda | **Il cielo che vedi** |
| Astrofili Celoria + Giovanni Celoria + Come si usa + Novità | **Chi siamo** |

«Il cielo che vedi» ha due sezioni interne — *Cosa compare* e *Come è disegnato* — invece di un elenco unico: fondere davvero i due gruppi avrebbe prodotto 17 righe di fila al livello Avanzato, cioè esattamente il problema che si voleva risolvere.

**Rinomine**

- **«Il tuo strumento» → «Le tue ottiche»**: termine più preciso e plurale coerente con più strumenti registrati. È accettabile perché il gruppo vive da Intermedio in su; a un principiante sarebbe stato gergo, come «campo» nella 4.0
- **Livello «Tutto» → «Avanzato»**: descrive chi sei, come gli altri due, invece di quanto vedi. È cambiata la sola etichetta, non la chiave `tutto` salvata in `localStorage`: chi l'aveva scelta se la sarebbe vista azzerare all'aggiornamento

**Via d'uscita esplicita**

In fondo al menù ridotto compare l'avviso che esistono altre impostazioni e come raggiungerle. Chi ha visto una voce e non la ritrova pensa che l'app sia rotta, e nascondere senza dirlo è peggio che non nascondere.

**Calibrazione obbligata all'apertura della realtà aumentata**

Alla prima attivazione la calibrazione non è più un invito accanto a un'etichetta, ma il primo passo. Un cielo sovrapposto e sfalsato di quindici gradi è peggio di nessun cielo proprio per un principiante: chi è esperto si accorge che quella non è Vega, chi comincia si fida e impara una cosa sbagliata con la sicurezza che gli dà la sovrapposizione. Chi ha già calibrato non se la ritrova davanti.

**Difetto trovato in verifica**

La prima marcatura delle righe usava una espressione regolare non delimitata correttamente e assegnava il livello a righe sbagliate: «Stelle doppie» era finita fra le voci visibili ai principianti. Rifatta con parsing bilanciato dei blocchi anziché con regex.

---

## v4.0

**Realtà aumentata (beta): il cielo sopra l'immagine della fotocamera**

Si attiva dal menù "Realtà aumentata", segnalato come **beta** sia lì sia dentro la vista, perché chi la usa in piazza il menù non ce l'ha davanti.

**Tre limiti dichiarati, non nascosti**

- **Di notte la fotocamera non riprende le stelle.** In video l'esposizione è di pochi centesimi di secondo: passano la Luna e i pianeti più brillanti, nient'altro. La realtà aumentata non serve quindi a identificare le stelle nell'immagine — a quello serve già la bussola — ma a legare il cielo calcolato al paesaggio reale: campanili, alberi, profilo delle colline
- **Il campo inquadrato dalla fotocamera non è ricavabile.** Nessuna API del browser lo espone, e varia da circa 65° a oltre 100° secondo il modello e l'obiettivo. Si parte da un valore tipico e lo si corregge a mano, una volta per dispositivo
- **La bussola sbaglia di diversi gradi.** Su fondo nero non si nota; con il paesaggio vero dietro si nota subito. Per questo lo stato della calibrazione è sempre in vista e non è un'opzione nascosta

**Calibrazione della bussola: il guadagno vero**

L'app non aveva **mai** corretto la declinazione magnetica: il sensore restituisce il Nord magnetico e il valore veniva usato come Nord vero. A questo si somma l'errore del magnetometro, che qualunque massa metallica vicina peggiora di parecchi gradi.

Invece di stimare la declinazione da un modello geomagnetico — che andrebbe aggiornato e verificato — si misura l'errore complessivo una volta sola: si punta un astro di posizione nota (Luna, Venere, Giove, Marte, Saturno: gli unici che la fotocamera veda davvero) e lo scarto fra posizione calcolata e direzione letta contiene già entrambi i contributi. Quando non c'è alcun astro utile sopra l'orizzonte resta la regolazione a mano, sempre disponibile.

La correzione agisce dentro `direzionePuntata()`, quindi **migliora anche la bussola normale e i percorsi guidati**, non solo la realtà aumentata.

**Errore di segno trovato in verifica**

`alpha` cresce in senso opposto all'azimut: sommare l'offset raddoppiava l'errore invece di annullarlo, e la calibrazione avrebbe peggiorato il puntamento anziché correggerlo. Corretto in sottrazione e verificata la convergenza: 14° di errore iniziale portati esattamente a zero.

**Comportamento della vista in realtà aumentata**

La vista non si anima, non si trascina e il pizzico non cambia il campo: deve restare agganciata a dove punta il telefono, altrimenti il cielo disegnato scivola via dall'immagine della fotocamera. Anche lo sfondo dipinto e la sagoma inventata del terreno vengono spenti: l'orizzonte vero è già nell'immagine, e sovrapporgliene uno finto ne mostrerebbe due diversi.

**Percorsi guidati in realtà aumentata**

Funzionano indicando la direzione con freccia ed evidenziazione, mentre a girarsi è l'osservatore — che è poi il gesto che un percorso guidato deve insegnare. `showRouteStep` chiamava `animateViewTo` per conto proprio, senza passare da `goToTarget`, e andava corretta separatamente.

**Gestione della fotocamera**

Lo stream viene rilasciato esplicitamente all'uscita, al passaggio in secondo piano e alla chiusura della pagina: senza `stop()` su ogni traccia il sensore resta acceso, scalda e consuma per tutta la serata. Se la fotocamera manca o il permesso è negato, la funzione resta spenta con un messaggio chiaro, senza schermate rotte.

**Rinominate le due righe del menù strumenti**

"Campo" da solo era gergo, incomprensibile a un neofita. Ora **"Cerchio del campo inquadrato"** e **"Simulazione del campo inquadrato"**, con l'avvertenza esplicita che si riproduce l'ampiezza dell'inquadratura e non l'aspetto all'oculare: dal vivo gli oggetti deboli sono molto più tenui e senza contorni netti.

**Da verificare sul campo**

La funzione è stata provata solo su browser da scrivania con fotocamera simulata. Restano da riscontrare su dispositivi reali: il comportamento di `getUserMedia` su iPhone in modalità installata da schermata Home, e la precisione effettiva della bussola dopo la calibrazione.

---

## v3.2

**Gli strumenti dell'osservatore: il campo che il binocolo o il telescopio inquadra davvero**

Si registrano i propri strumenti dal menù "Il tuo strumento" e si richiamano con un tocco. L'app calcola il campo reale, ne disegna il cerchio nel cielo e, se richiesto, restringe la vista fino a quel campo per mostrare gli oggetti alla scala a cui l'oculare li mostra.

**Come si ricava il campo, e perché non dall'apertura**

L'apertura di uno strumento — i 50 mm di un «10x50» — non compare in nessuna formula del campo inquadrato: determina luminosità e potere risolutivo, non l'ampiezza di ciò che si vede. Il campo dipende da due soli valori:

    campo reale = campo apparente dell'oculare / ingrandimento

Vale identico per binocolo e telescopio; cambia solo da dove arriva l'ingrandimento. Nel binocolo è il primo numero della sigla. Nel telescopio è il rapporto fra la focale del telescopio e quella dell'oculare.

- L'apertura resta un dato **facoltativo**, usato solo per la pupilla d'uscita (apertura / ingrandimento), che dice se lo strumento è sfruttabile a occhio adattato al buio
- Il campo apparente dell'oculare è l'unico valore che spesso l'osservatore non conosce. Quando manca si usano **50°**, tipici di un oculare semplice, ma il risultato viene marcato come **stima** sia nel menù sia sull'etichetta a schermo. Un valore assunto non deve mai sembrare misurato: sarebbe la stessa scorciatoia che ricavare il campo dall'apertura, solo meglio nascosta

**Geometria del cerchio**

Il raggio sullo schermo usa la formula esatta della proiezione gnomonica per un cono centrato sull'asse ottico, `focal·tan(campo/2)`. Non è un'approssimazione: è lo stesso principio già usato da `projectFp` per tutta la vista, quindi resta corretto a qualunque ampiezza di campo. Un'approssimazione lineare (pixel per grado moltiplicati per l'ampiezza) sbaglierebbe di circa il 5% già a 45°, dove la tangente cresce più che linearmente.

Lo zoom inscrive il cerchio nel **lato corto** dello schermo, non in altezza: il campo indicato è quello verticale, e su un telefono in verticale un cerchio inscritto in altezza uscirebbe dai due lati.

**Due difetti emersi provandolo a forte ingrandimento**

- `pxPerDegAt` misurava i pixel per grado proiettando un punto di sonda a **un grado** di distanza. Con la vista strumento il campo scende sotto il grado, quel punto cade fuori dall'inquadratura, la proiezione lo scarta e la misura fallisce — proprio dove la resa in scala reale serve di più. Il passo di sonda ora segue il campo inquadrato
- Il limite al raggio degli oggetti era fisso a 420 px. A forte ingrandimento M31 veniva tagliata a quel valore e sembrava entrare comodamente in un oculare che invece non la contiene: esattamente l'informazione opposta a quella che lo strumento deve dare. Il limite ora segue la dimensione della vista, così l'oggetto deborda quando deve

`FOV_MIN` **non** è stato abbassato per consentire lo zoom profondo, benché fosse la strada più breve: compare nel denominatore di `skyStarLimit()`, e cambiarlo avrebbe spostato la magnitudine limite a ogni livello di zoom anche per chi non usa alcuno strumento. La vista strumento usa un proprio minimo separato.

---

## v3.1

**Selezione al tocco proporzionata alla dimensione dell'oggetto**

Con gli oggetti disegnati in scala reale dalla 3.0, la selezione al tocco confrontava ancora soltanto la distanza dal dito entro un raggio fisso. Un oggetto esteso catturava così qualsiasi tocco al suo interno: M31, che a piena scala può superare i 200 px, rendeva irraggiungibili M32 e M110 che le stanno dentro — con effetti anche sul diario delle osservazioni, dove finiva annotato l'oggetto sbagliato.

- L'area di tocco di ogni oggetto è ora il suo raggio reale a schermo, con un minimo di 20 px perché i simboli piccoli restino raggiungibili da un dito
- Fra oggetti sovrapposti vince il **più piccolo**, cioè il più specifico. Le stelle, che non hanno un raggio proprio, vincono sempre su un oggetto esteso che le contenga
- A parità di dimensione vince il più vicino al dito

---

## v3.0

**Gli oggetti del profondo cielo prendono forma e dimensione vere**

Fino alla 2.9 ogni oggetto era un simbolo di dimensione fissa: M31 (178′) e M57 (1,3′) venivano disegnati identici. Ora il simbolo cresce fino alla dimensione angolare reale.

- Importati da OpenNGC anche **MinAx** (asse minore) e **PosAng** (angolo di posizione), oltre al MajAx già presente. M31 risulta così un ovale di 178′×70′ inclinato di 35°, non un cerchio
- Dove MinAx manca, l'oggetto è trattato come circolare. Non è un'approssimazione di comodo: il campo manca soprattutto per ammassi globulari e nebulose planetarie, che tondi lo sono davvero
- La scala reale si attiva **solo sopra la dimensione del segnaposto minimo**. Disegnare tutto in scala renderebbe M57 largo 0,23 px al campo di 70°, M27 1,2 px e M13 2,9 px: sparirebbero invece di informare. Sotto la soglia resta il simbolo fisso, sopra si passa alla scala vera — e ingrandendo sempre più oggetti la superano, che è esattamente quando il realismo serve
- I pixel per grado sono **misurati** proiettando un secondo punto a un grado di distanza, non ricavati dal campo inquadrato: la proiezione gnomonica dilata ai bordi e quella a disco no, quindi una costante sarebbe sbagliata fuori centro

**Aspetto secondo il tipo di oggetto
**

- **Galassie**: alone sfumato e contorno ellittico orientato secondo PosAng; accenno dei bracci a spirale solo quando l'oggetto è abbastanza grande e non è visto di taglio, dove i bracci non si distinguerebbero comunque
- **Ammassi globulari**: nucleo denso che sfuma verso l'esterno, con granulosità disposta in modo deterministico così che la figura non cambi a ogni fotogramma
- **Nebulose planetarie**: anello con il centro vuoto, che è il loro tratto distintivo
- **Resti di supernova**: contorno tratteggiato doppio
- **Nebulose diffuse**: macchia dal bordo indefinito
- **Ammassi aperti**: il solo contorno tratteggiato. Una prima stesura aggiungeva stelle disegnate dentro il cerchio, ma le stelle di un ammasso aperto sono già quelle vere del catalogo, disegnate al loro posto: i punti finti le raddoppiavano con oggetti inventati indistinguibili da quelli reali

**Pulizia**

Il blocco che disegnava gli oggetti di Messier duplicava per intero il corpo di `drawDeepSymbol` invece di richiamarla, per cui ogni modifica andava fatta due volte. Ora entrambi i cataloghi passano dalla stessa funzione.

**Nota sulle immagini fotografiche**

È stata valutata e scartata l'ipotesi di usare fotografie reali come fa Stellarium. Le illustrazioni delle costellazioni che l'app già usa sono sotto Free Art License, ma le texture delle nebulose seguono un regime diverso: nei credits di Stellarium le immagini degli oggetti Messier risultano concesse per cortesia dal Grasslands Observatory, e alcune texture sono utilizzabili solo con attribuzione per singola immagine e/o per scopi non commerciali. Non è una licenza trasferibile. Restano percorribili in futuro i survey di pubblico dominio (NASA/ESA/ESO) previa verifica singola, oppure le immagini prodotte dal Gruppo stesso con l'ASIAIR.

---

## v2.9

**Dimensioni angolari degli oggetti Messier**

I 265 oggetti NGC/IC avevano già il campo `size`; i 110 oggetti di Messier **non ne avevano nessuno**. Ora ce l'hanno tutti e 110.

- Valori da **OpenNGC** (campo MajAx), la stessa fonte già usata per i NGC/IC: i due cataloghi restano così coerenti fra loro invece di mescolare misure prese da isofote diverse
- 106 oggetti ricavati direttamente da OpenNGC. Gli altri quattro non erano ricavabili e sono stati verificati singolarmente su fonti esterne anziché stimati o omessi:
  - **M45** (Pleiadi) — 110′. Non ha designazione NGC (hè Melotte 22), quindi è assente da OpenNGC. Valore confermato da fonti multiple indipendenti
  - **M40** (Winnecke 4) — 0,8′. È una doppia ottica, non un oggetto esteso: il valore è la separazione fra le due componenti, 52,8″ misurati da Hipparcos nel 1991. La scheda lo etichetta "Separazione delle due stelle", non "Dimensione apparente"
  - **M73** — 2,8′. Asterismo di quattro stelle; presente in OpenNGC ma privo di MajAx
  - **M102** — 6,3′, assumendo l'identificazione con NGC 5866, che è quella già implicita nelle coordinate usate dall'app (RA 15,082 / Dec +55,763). L'identificazione di M102 è storicamente controversa
- Verifica a campione contro valori noti: M31 177,8′, M42 90′, M13 16,5′, M57 1,3′, M27 6,7′ — tutti coerenti

**Come viene mostrata**

- La riga compare ora anche per gli oggetti di Messier, non solo per i NGC/IC
- Sotto i 10′ viene mostrato il decimale: arrotondando all'intero, M57 (1,3′) e M76 (1,1′) risultavano entrambi "1′" e la differenza spariva
- Aggiunto un paragone con la Luna piena (31′), che è l'unico metro di misura angolare che chiunque ha già in mente. Le soglie sono frazioni vere di 31′: in una prima stesura M13, a 16,5′, veniva descritto come "circa un quarto della Luna piena" quando in realtà ne è poco più della metà

**Nota tecnica emersa durante il lavoro**

L'identificativo interno degli oggetti Messier è costruito come `'M' + m.id`, dove `m.id` vale già "M40": il risultato è "MM40". L'anomalia **non è stata corretta**, perché `selKey()` usa quell'identificativo come chiave del diario delle osservazioni: cambiarlo farebbe sparire senza avviso le osservazioni già salvate dagli utenti, che risiedono in localStorage e non hanno logica di migrazione. Il controllo su M40 usa quindi un flag esplicito impostato a monte.

---

## v2.8

**Denominazione dell'associazione**

Uniformata in tutta l'app. Il nome è **Gruppo Astrofili Casalese "Giovanni Celoria"**; dove lo spazio è stretto sono ammesse le forme **G.A.C.** e **Astrofili Celoria**. Non sono ammesse forme troncate ("il Gruppo", "Gruppo Astrofili", "Astrofili" da solo).

Corretto in:
- Blocco donazione: il corpo del testo porta ora il nome completo; il bottone usa "Sostieni gli Astrofili Celoria", perché il nome per esteso mandava il bottone su tre righe a 390 px
- Titolo della sezione nel menù: da "Gruppo Astrofili Casalese" (privo di «Giovanni Celoria») ad "Astrofili Celoria"
- Voce "Diventa socio": "come associarsi agli Astrofili Celoria"
- Voce "Sito degli Astrofili Celoria" nell'elenco dei collegamenti
- Due voci del changelog interno all'app
- Tre commenti nel codice

Lasciata invariata la frase nella sezione su Giovanni Celoria ("è per questo che il Gruppo Astrofili Casalese porta il suo nome"): scrivere lì il nome per esteso renderebbe la frase circolare, dato che sta spiegando proprio l'origine di quel nome.

Restano legittime le occorrenze in cui "Gruppo Astrofili Casalese" e «Giovanni Celoria» sono separati da un'andata a capo (`<br>`) all'interno dello stesso nome, la stringa URL-encoded del manifest e il nome della funzione `buildGruppo()`.

---

## v2.7

**Eventi in arrivo**
- Le congiunzioni fra **Luna e pianeti** erano annunciate dal commento nel codice ma non venivano mai calcolate: il ciclo girava solo sui pianeti fra loro. Ora la Luna è inclusa, con soglia più larga (6°) perché la si riconosce a occhio nudo anche a distanza
- Aggiunti i **passaggi della Luna accanto alle stelle zodiacali di prima grandezza** (Aldebaran, Regolo, Spica, Antares): ricorrono ogni mese e non richiedono strumenti. Le coordinate sono quelle del catalogo HYG già usato per disegnare il cielo, non ricopiate a parte
- Misurato su 24 date sparse nell'anno, l'insieme delle due aggiunte porta la media da **1,6 a 3,3 eventi nei sette giorni** e azzera le settimane completamente vuote (prima erano 3 su 24)
- **Rimosso l'elenco "Più avanti"** con gli eventi oltre i sette giorni. Era possibile solo dopo le aggiunte sopra: con i dati precedenti la sezione sarebbe rimasta vuota o quasi in metà delle date
- Verifica: la congiunzione Luna-Giove dell'8 settembre 2026 calcolata dall'app dà 0,8°, contro i 50′ (0,83°) di In-The-Sky.org, che usa l'effemeride JPL DE430

**Come si usa**
- Tolto il riferimento al sito in https dalla voce sulla bussola. L'avviso resta a runtime, dove compare solo se i sensori sono davvero bloccati e quindi serve
- Tolta la nota finale sul calcolo delle posizioni di Sole, Luna e pianeti

**Storico versioni**
- La scheda "Tutte le versioni" mostra ora la versione in uso e le tre precedenti, con una riga che segnala l'esistenza delle più vecchie. Lo storico integrale resta nel commento in cima al file e in questo changelog

**Sostegno al Gruppo**
- La donazione non è più una voce indistinta nell'elenco dei collegamenti, ma un blocco a sé con una ragione esplicita e un invito all'azione, nel colore istituzionale del Gruppo (#db8036) anziché nell'oro dell'interfaccia, così da leggersi come una richiesta del Gruppo e non come un comando dell'app. Contrasto verificato sopra 4,5:1 anche in modalità luce rossa

---

## v2.6

**Correzioni**
- Il tasto "Salta" in "Mettiti alla prova" non aveva alcun gestore di click collegato: non succedeva nulla toccandolo. Ora passa alla domanda successiva senza contarla come tentativo
- Rimosso il testo fisso "Dentro la tolleranza: consideralo centrato", che compariva a ogni risposta esatta in aggiunta all'esito già mostrato, risultando ridondante
- `--text-dim` (usato per note, didascalie e testo secondario in tutta l'app) era sotto la soglia WCAG AA per il contrasto su testo normale (4,43:1 contro sfondo scuro, fino a 3,88:1 sui pannelli semitrasparenti; anche peggio con il filtro della modalità luce rossa attivo). Schiarito da `#6b7690` a `#76829f`, ora sopra 4,5:1 in entrambe le modalità
- I gruppi del menù non chiudevano i fratelli già aperti allo stesso livello: aprendone diversi in sequenza lo scorrimento si allungava progressivamente. Ora l'apertura di un gruppo chiude gli altri gruppi aperti allo stesso livello (vale sia per le famiglie principali sia per le sottosezioni dentro "Impara il cielo")

---

## v2.5

**Identità visiva**
- Icona propria dell'app (collina del Monferrato con stella scintilla), usata come favicon, icona di installazione (iOS e Android) e marchio nel menù
- Logo a colori del Gruppo Astrofili Casalese inserito nella sua sezione dedicata
- Nuova schermata di caricamento, con tempi ricalibrati perché ogni scritta resti effettivamente leggibile: nella stesura originale il sottotitolo spariva nello stesso istante in cui finiva di comparire, e i puntini di caricamento non arrivavano mai a essere visti
- Aggiunto il manifest per l'installazione come app (icone 192/512 px, colori, orientamento)
- Scritta "GUARDA IN ALTO" allineata per colore e carattere in ogni punto in cui compare

---

## v2.4

**Menù**
- Riordino completo pensato per chi apre l'app la prima volta
- Nuovo ordine: **Livello** (in cima, prima decisione) → **Impara il cielo** (con dentro come sottosezioni: Percorsi guidati, Mettiti alla prova, Capire il cielo, Eventi in arrivo, Il tuo diario, Cos'è quel puntino) → **Contesto** (Dove ti trovi, Quando, Il tuo cielo) → **Aspetto** (Cosa posso osservare, Come appare il cielo, Righe di riferimento, Tipi di oggetto) → **Chi siamo** (Gruppo, Celoria, Come si usa, Novità), quest'ultimo con trattamento visivo più leggero
- "Eventi in arrivo" e "Il tuo diario" spostati dentro "Impara il cielo": prima erano gruppi separati, ora l'apprendimento è un blocco unico coerente

---

## v2.3

**Cataloghi**
- Aggiunti **Urano e Nettuno**, con gli stessi elementi orbitali già usati per gli altri cinque pianeti; posizioni verificate contro un'effemeride esterna con scarto sotto 0,02°
- Compaiono solo dal livello binocolo/telescopio in su, coerentemente con la loro reale osservabilità (Urano magnitudine 5,8, Nettuno 7,9)
- Consigli osservativi dedicati nel riuadro informativo

**Menù**
- Le due legende (colori dei pianeti, simboli del cielo profondo) riunite in un'unica sottosezione a tendina dentro "Cosa posso osservare"

---

## v2.2

- Istruzioni per installare l'app sul telefono, integrate nella presentazione iniziale, con testo diverso a seconda della piattaforma rilevata (iPhone/Safari, iPhone/altro browser, Android/Chrome, Android/altro)
- Legenda dei simboli del cielo profondo, disegnata dalla stessa funzione usata per il cielo vero, così da non poter mai disallinearsi da ciò che si vede davvero

---

## v2.1

- Sezione "Novità" alleggerita: solo i titoli di ciò che è cambiato nella versione corrente, senza descrizione estesa
- Aggiunto un pulsante "Tutte le versioni" che apre lo storico completo del changelog

---

## v2.0 — v1.6 · Le figure mitologiche

Una serie di correzioni successive sullo stesso sottosistema, ognuna delle quali ha isolato un difetto distinto:

- **v1.6** — Corretta la trama a triangoli visibile sulle figure: il ritaglio dei singoli triangoli era dilatato in proporzione alla loro dimensione, e ai bordi scopriva porzioni di disegno esterne al reticolo. Portato a un margine fisso di 0,6 pixel. Introdotta una tela di composizione separata per le figure. Via Lattea resa graduale in base alla qualità del cielo scelta, invece che accesa o spenta
- **v1.8** — I nodi del reticolo delle figure erano identificati in una cache senza tener conto della finezza del reticolo stesso (diversa fra vista in prima persona e mappa): passando da una vista all'altra, la cache restituiva punti di cielo sbagliati anche di 18 gradi, spezzando le figure con tagli netti. Risolto calcolando la precessione dei nodi al momento, senza cache
- **v1.9** — Componendo tutte le figure sulla stessa tela, l'ultima disegnata cancellava quelle già disegnate lungo il bordo del proprio reticolo: le costellazioni vicine tra loro (Orsa Maggiore/Drago, Ercole/Serpente) apparivano tagliate da quadrilateri netti. Risolto facendo comporre e riversare ogni figura singolarmente, in fusione additiva, così dove si sovrappongono si sommano invece di sostituirsi
- **v2.0** — Stesso principio applicato al taglio lungo l'orizzonte: i nodi del reticolo sotto l'orizzonte venivano scartati, e ogni cella che ne toccava anche uno spariva per intero, producendo un bordo a gradini. Risolto lasciando proseguire il reticolo sottoterra e ritagliando il risultato lungo la linea vera dell'orizzonte

---

## v1.7

- Bussola: eliminata la doppia sorgente di dati che causava letture in conflitto; aggiunto un filtro sul rumore dei sensori
- Diario delle osservazioni: aggiunta una copia di sicurezza esportabile/importabile
- Numero di versione mostrato a schermo, nel menù

---

## v1.5

- Corretto il puntamento della bussola: il calcolo si basava sul solo angolo di inclinazione, che invertiva il segno superata la verticale e mandava la vista sottoterra alzando il telefono verso lo zenit. Riscritto usando la matrice di rotazione completa del dispositivo

---

## v1.4

- Diario delle osservazioni (prima introduzione)
- Sezioni dedicate a **Giovanni Celoria** e al **Gruppo Astrofili Casalese**, con link a sito, adesione, donazioni e social
- Aggiunti i luoghi fissi **Treville** e **Moleto**
- L'app viene rinominata **"Guarda in alto"**

---

## v1.3

- "Eventi in arrivo" (prima introduzione)
- Il glossario diventa **"Capire il cielo"**, con contenuti ampliati e organizzati per argomento
- Avvio guidato al primo utilizzo (presentazione interattiva)

---

## v1.2

- "Qualità del cielo" (Città / Periferia / Campagna / Cielo buio), che filtra cosa mostrare in base a quanto è realmente osservabile
- "Cosa vedo stasera": scaletta automatica degli oggetti migliori del momento
- "Mettiti alla prova": esercizio di riconoscimento con punteggio
- Riconoscitore animato di stelle, pianeti, satelliti e aerei

---

## v1.1

- Percorsi guidati di stella in stella
- Funzione di ricerca
- Riquadro informativo con orari di sorgere/culminazione/tramonto e consigli osservativi

---

## v1.0

- Prima versione pubblica
- Vista in prima persona e mappa planisferica
- Cataloghi stellari di base
- Figure mitologiche delle costellazioni
