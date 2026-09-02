# Changelog — Guarda in alto

Planetario interattivo del Gruppo Astrofili Casalese "Giovanni Celoria", Casale Monferrato.

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
- Consigli osservativi dedicati nel riquadro informativo

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
