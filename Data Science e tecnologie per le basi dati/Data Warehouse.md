# Data Warehouse, Introduzione

Sono basi dati usate per il supporto alle decisioni, e sono mantenute separate dalle basi dati operative dell'azienda.
I dati contenuti all'interno di un data warehouse sono **orientati ai soggetti di interesse**, **consistenti** e **durevoli nel tempo** e sono d'aiuto per le decisioni aziendali.

I dati sono tenuti in basi separati per diversi motivi: il primo riguarda le **prestazioni**, infatti interrogare un data warehouse potrebbe compromettere le transazioni sulla base dati operativa.
Inoltre esistono diversi mezzi di accesso a livello fisico.
Il secondo motivo è legato alla gestione dei dati: **storico**, **consolidamento dei dati** e **qualità dei dati**.

### Rappresentazione dei dati

I dati in una warehouse sono rappresentati come un **iper cubo** ad n dimensioni: i valori numerici (detti **misure**) sono individuati dall'intersezione delle n dimensioni:

![[Pasted image 20231005115906.png]]

Le tre dimensioni **descrittive** dell'esempio rappresentano ad esempio le vendite di un supermercato.

Una rappresentazione alternativa è quella **a stella**: le 3 dimensioni del cubo diventano **entità del modello ER**, le relazioni legano le entità alle misure (centro stella):

![[Pasted image 20231005120305.png]]

### Architetture per data warehouse

Trattiamo separatamente l'elaborazione OLTP e elaborazione OLAP, quindi dobbiamo necessariamente avere architetture su due livelli (quelle a un livello sono complessi e non presi in considerazione nel corso).

![[Pasted image 20231005121240.png]]

I dati in arrivo dalle sorgenti sono estratti tramite strumenti **ETL** (extraction transform load) e portati nella **data warehouse**:
- E: si acquisiscono i dati dalla sorgente
- T: prima di Puliscono i dati (correttezza e consistenza), poi si convertono nel formato del warehouse (integrazione)
- C: Infine si propagano le informazioni nel warehouse

Sopra al data warehouse sono presenti gli **OLAP server** che servono per rispondere meglio alle interrogazioni di analisi. Sono di 3 tipi:
- ROLAP (Relational OLAP): sono DBMS relazioni estesi con operazioni efficienti per i dati, SQL esteso e potenziato.
- MOLAP (Multidimensional OLAP): in questo tipo di tecnologie i dati sono rappresentati in forma matriciale, perciò sparsi, infatti sono presenti tecniche di compressione e decompressione. Queste soluzioni sono spesso proprietarie.
- HOLAP (Hybrid OLAP) costituiscono soluzioni ibride.

I **Data marts** sono porzioni che poi crescono in maniera omogena per generare il warehouse (sono 'pezzi' che compongono il warehouse): normalmente l'approccio è bottom up, cioè prima si progettano i marts e poi crescendo si passa la warehouse.
Normalmente i marts possono essere:
- alimentati dal data warehouse primario
- alimentati direttamente dalle sorgenti

Gli **strumenti di analisi** prendono i dati e analizzandoli li servono al cliente finale.

I **Metadati** sono dati sui dati, e descrivono tutti gli oggetti e le strutture che fanno parte della base dati.
Servono per trasformazione e caricamento (da dove provengono i dati... ), per la gestione dei dati (formato dei dati ecc...) e per la gestione delle query (codice SQL, CPU usage ecc...)

Concettualmente distinguiamo **due livelli: livello delle sorgenti e livello della warehouse**.
Nelle architetture a 3 livello è presente una **staging area** fra i due livelli di prima: funziona da buffer su cui svolgiamo la fase di ET, prima di essere caricati nel warehouse con gli strumenti L.


# Progettazione di Data warehouse

Quando si comincia un progetto le aspettative degli utenti sono alte, e ci sono una serie di problematiche come la qualità dei dati OLTP di partenza (dati inaffidabili) e la gestione 'politica' del progetto (collaborazione con i 'detentori' delle informazioni) 

### Analisi dei requisiti

Questa fase raccoglie i dati che servono per realizzare le esigenze delle aziende, si individuano dei **vincoli**.
Il data mart che si andrà a progettare è strategico per una determinata azienda, e sarà alimentato da poche (affidabili) sorgenti.

```ad-note
title: Riconciliazione
La riconcilazione dei dati da parte di diverse fonti è un processo che richiede tempo e molta fatica

```

I **requisiti applicativi** descrivono gli eventi di interesse (ovvero i fatti) che interessano all'azienda (gli scontrini per un supermercato).
Questi fatti sono caratterizzate da dimensione descrittive.
Altro importante parametro è il **carico di lavoro**: per determinare quest'ultimo si ricorre a report aziendali o a specifiche domande in linguaggio naturale (quanti scontrini si fanno al mese).

I **requisiti strutturali** dipendono da:
- **Periodicità** dell'alimentazione, se voglio inserire nuovi dati giornalmente, mensilmente ecc...
- **Spazio disponibile** per dati e strutture accessorie
- **Tipologia di struttura**, ovvero il numero di livelli necessari, se i data mart sono indipendenti o no...
- **Pianificazione del deployment**: come si deciderà di avviare il data mart, e inoltre bisogna considerare un periodo di formazione per i nuovi utenti.

### Progettazione concettuale DFM

Si utilizza un modello detto **Dimensional Fact Model**, che per uno specifico fatto definisci **schemi di fatto** che modellano dimensioni gerarchie e misure.
E' un modello grafico e può essere anche usato come documentazione di progetto utile a priori e posteriori.
Il modello si rifà alla rappresentazione dei dati tramite iper cubo, con fatti, dimensioni e misure.

Il **Fatto** che prendiamo in esempio è la vendita di un certo prodotto, che definiamo graficamente come:

Le **dimensioni** aggiuntive sono **data e negozio**.
Ogni fatto è descritto da misure numeriche indicate all'interno del fatto.

![[Pasted image 20231006203829.png]]

Le **Gerarchie** rappresentano una relazione di generalizzazione tra un sottoinsieme di attributi di una dimensione: nella pratica è una relazione funzionale 1:n.

![[Pasted image 20231006204016.png]]

Prendendo in esempio la dimensione NEGOZIO notiamo che un negozio si trova in una sola città (il viceversa è 1:n), data una città c'è una sola regione e così via.
Questi attributi sono necessari per confrontare tramite group by e generare report significativi (esempio, confronto vendite Piemonte e Lombardia).

Per la gerarchia del tempo, oltre alla scomposizione della data possiamo usare un attributo **vacanza** oppure **evento speciale** per verificare se (ad esempio) le abitudini dei consumatori sono influenzate da giorni particolari (come natale...).

Altri componenti del modello DFM sono riportati nell'immagine di sotto:

![[Pasted image 20231006204850.png]]

Il primo componente è l'**arco di opzionalità** (0,1) come nel caso del prodotto dietetico o no.
Gli **attributi descrittivi** non servono per raggruppare ma solo a fornire maggiori dettagli (ad esempio il numero di telefono).

Un concetto importante è al **convergenza** delle gerarchie: ad esempio dei punti vendite sono legate a una certa area geografica (spesso non coincidente con la regione); questa caratteristica geografica nell'esempio è il distretto di vendita che converge poi nello stato.

La **non additività** pone un limite alle interrogazioni che posso fare al modello.
L'**aggregazione** è il processo di calcolo del valore di misure a granularità meno fine di quella presente nello schema di fatto originale.
Le misure possono essere **additive, non additive e non aggregabili**.

### Misure

**Misure di flusso**: misure cumulative su un periodo di tempo, sono aggregabili con tutti gli operatori standard.
Ad esempio la quantità di prodotti venduti.

**Misure di livello**: misurano quantità in specifici istanti (snapshot), e non sono additive lungo la dimensione tempo.
Ad esempio il saldo del conto corrente

**Misure unitarie**: valutate in specifici istanti di tempo ed espressi in termini relativi, inoltre non sono additive lunga nessuna dimensione.
Ad esempio il prezzo di vendita di un prodotto.

### Operatori

**Operatori distributivi**: è sempre possibile il calcolo di aggregati da dati a livello di dettaglio maggiore.
Esempio: sum, min, max

**Algebrici**: il calcolo di aggregati da dati a livello di dettaglio maggiore è possibile in presenza di misure aggiuntive di supporto (ad esempio avg richiede count)

**Olistici**: Non è possibile il calcolo degli aggregati da dati a livello di gerarchia superiore. (moda, mediana)

### Altri costrutti 

![[Pasted image 20231006212107.png]]

La soluzione sotto non va bene, meglio unire le gerarchie e definire i **ruoli** delle due dimensioni.