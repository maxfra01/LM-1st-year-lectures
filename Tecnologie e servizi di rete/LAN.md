# Repeater

Si tratta di un dispositivo che opera a livello 1: si occupa semplicemente di ripetere **bit che riceve e di propagarli**.
Inoltre è in grado di passare da un mezzo fisico all'altro (da rame a fibra).
Si utilizza per connettere delle reti caratterizzate **dallo stesso indirizzo MAC**, oppure un'altra importante applicazione è quella di ripristinare un segnale degradato su lunghe distanze.

![[Pasted image 20231113134237.png]]

In altre parole, un repeater è in grado di far comunicare due domini fisici differenti: purtroppo però **unisce anche i domini di collisione**.
Quando un repeater ha più di 2 porte di parla di **Hub**.

# Bridge/Switch

E' un dispositivo che lavora a livello 2 che serve per connettere diversi protocolli di livello 2 (ad esempio si passa da ethernet a fast ethernet).
Inoltre posso passare da un tipo di trama a un'altra.

Il funzionamento di un bridge si divide in 3 fasi:
1. Si memorizza il pacchetto (è un nodo **store and forward**)
2. Si modifica opportunamente la trama per cambiare il protocollo (ad esempio da Ethernet a Token Ring)
3. Si inoltra il pacchetto

I Bridge sono in grado di **dividere i domini di collisione**, andando inoltre a lasciar intatto il broadcast che continuerà a funzionare correttamente. 

![[Pasted image 20231113140651.png]]

Un'altra che posso fare per aumentare la velocità di comunicazione è usare due link per permettere la trasmissione e la ricezione simultaneo (in sostanza il **full duplex**): quest'ultimo è implementabile solo nel caso di un nodo che possa temporaneamente memorizzare il pacchetto e non inoltrarlo subito.
Così facendo, la banda raddoppia a livello teorica (in realtà gli hosts saturano il downlink, mentre i server l'uplink) e inoltre si eliminano le collisioni (il CSMA/CD non è più necessario).

Nel caso di un bridge multi-porta, si parla di **switch**.

### Bridge trasparenti

All'interno di una LAN vengono definiti **switch trasparenti**. In particolare per trasparenza si intende che:
- Lo switch deve essere plug and play e non deve richiedere riconfigurazioni negli hosts.
- Le performance del sistema possono essere diverse, ma le funzionalità devono rimanere le stesse

Quindi gli end systems (ovvero gli hosts) opereranno nella stessa maniera di sempre, ovvero usando gli stessi protocolli, stessi formati, stesse trame.

Ogni interfaccia degli switch ha un indirizzo MAC, ma non viene mai usato se non per messaggi generati dagli switch stessi per motivi di configurazione.

### Forwarding process

Le regole sono semplicemente due:
- Unicast: lo switch è in grado di fare forwarding **solo sulla porta dove si trova la destinazione**.
- Multicast o Broadcast: si esegue il **flooding**, ovvero si inoltra su tutte le porte tranne quella sorgente (Spesso non sempre in modo simultaneo).

Per implementare queste regole servono 3 componenti:
- Una tabella locale di forwarding ([[#Filtering Database]])
- Stazioni che imparano autonomamente ([[#Backward learning]])
- Individuazione dei loop (**Spanning tree algorithm**)

### Filtering Database

Si tratta di una tabella con le posizioni di ogni MAC address presente nella rete locale.
Per ogni entry si tiene traccia del MAC, della porta tramite cui raggiungerlo e l'**ageing time**.

![[Pasted image 20231113145819.png]]

Questa tabella può essere configurata **dinamicamente** tramite gli algoritmi di backward learning, oppure **staticamente** (a mano).

### Backward learning

L'idea di base è quella di **investigare sui frame ricevuti** dallo switch: se ricevo un pacchetto con sorgente H1 dalla porta P1, allora significa che H1 è raggiungibile dalla porta P1. L'indirizzo destinazione viene ignorato da questo algoritmo.

Questo principio funziona anche in presenza di switch multipli nella stessa LAN:

![[Pasted image 20231113150342.png]]

E' possibile che gli host si spostino all'interno della rete, e se quest'ultimi non generano traffico è difficile che le tabelle si aggiornino.
Nel caso si generino dei pacchetti broadcast, allora sono sempre ricevuti correttamente in quanto raggiungono tutta la rete e quindi si mantengono le reti aggiornate.
Nel caso si unicast spesso alcune porzioni della rete non sono aggiornate e quindi è possibile che i pacchetti si perdano.

```ad-danger
title: MAC Flooding attack
Se si generano tantissimi pacchetti con informazioni casuali e MAC sorgente randomici è possibile **riempire la forwarding table** di uno switch. Così facendo lo switch sarà costretto a fare flooding dei nuovi pacchetti che riceve, e sarà possibile (da parte dell'attaccante) di intercettare il traffico sparso sulla rete (Oltre che di rallentarla).

```

### Spanning tree algorithm

Consideriamo che l'host H1 mandi un pacchetto in broadcast su una rete in cui sono presenti più switch: si genera la seguente situazione.

![[Pasted image 20231113151320.png]]

Questo perchè alcuni switch possono avere delle forwarding table **inconsistenti**. La soluzione consiste nell'evitare che nella rete fisica si creino dei loop:
- Posso creare fisicamente delle reti senza loop
- Posso avere un **algoritmo che disabilita temporaneamente i loop** (meglio)

L'algoritmo è definito dallo standard 802.1D: si identificano i nodi e la **rete diventa un MST che ha un percorso unico fra qualsiasi sorgente e la relativa destinazione**.

# Router

Sono dispositivi al livello 3 che connettono diverse reti e servono per interfacciarsi a internet.
Non sono dispositivi trasparenti: **separano i domini di broadcast**.

![[Pasted image 20231113152018.png]]

# L2 vs L3

Consideriamo le caratteristiche viste finora e notiamo che i pro del livello 2 sono superiori a quelli del livello 3: di base funziona tutto con qualsiasi protocollo di rete e ovunque io mi trovi nella rete.

Quindi **scegliamo di usare L2**: ora il problema diventa scegliere se usare una singola LAN o più LANs connesse in qualche modo.
Per ragioni di sicurezza e performance la strada più conveniente è quella di usare LAN multiple.
Per la sicurezza si pensi al MAC flooding attack, mentre per le performance si pensi al traffico broadcast di una singola rete LAN con tantissimi dispositivi.

# VLAN

