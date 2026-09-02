# Changelog — Guarda in alto

Planetario interattivo del Gruppo Astrofili Casalese "Giovanni Celoria", Casale Monferrato.

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
