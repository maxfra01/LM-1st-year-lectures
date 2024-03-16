# Virtualizzazione

Spesso non è necessario che l'intero programma sia in [[Memoria principale]]: è possibile che sezioni del programma non servano o non vengano mai chiamate.
L'idea è quindi caricare solamente quello che serve: si parla di **partial loading**.
Così facendo ho più spazio in memoria, posso caricare più processi, posso velocizzare il caricamento dei processi e ho un maggior parallelismo.

La **memoria virtuale** è una separazione più netta fra la memoria logica e fisica. Grazie a questo meccanismo abbiamo diversi benefici:
- Solamente la parte di programma necessaria è caricata in memoria
- Lo spazio di indirizzamento logico può essere più grande di quello fisico
- Permette la condivisione dello spazio di indirizzamento
- Aumenta l'efficienza della creazione dei processi
- Posso aumentare il numero di programmi in esecuzione in parallelo
- Meno I/O da fare per caricare o swappare i processi

Lo **spazio di indirizzamento virtuale** è una vista logica degli indirizzi in memoria.

La memoria virtuale può essere implementata in due modi:
- **Demand paging**
- Demand segmentation

Lo scenario che consideriamo è il seguente:

![[Pasted image 20240315103443.png]]

Nella memoria logica abbiamo diverse convenzioni usate: il codice, e le variabili globali sono negli **indirizzi bassi** (a partire da zero), al di sopra di questa struttura abbiamo lo **stack** e lo **heap** (tipicamente lo stack cresce verso il basso, mentre lo heap cresce verso l'alto).
Lo spazio in mezzo è utile per l'uso di librerie condivise.

# Demand paging

Teoricamente si può portare tutto il programma in memoria al **load time**. In realtà posso decidere di fare entrare **in memoria principale solamente le pagine che mi servono** (ad esempio all'inizio posso portare solamente la prima pagina del main): abbiamo ridotto l'I/O necessario, la risposta in partenza è più rapida e si minimizza l'uso della memoria.
Se ho bisogno di una pagina ho due scenari (una volta ottenuto il riferimento alla pagina):
- Se il riferimento non è valido faccio abort del programma
- Se non si trova in memoria la carico.

Come posso **determinare le pagine** di cui ho bisogno?
Ho bisogno di inserire nel meccanismo di esecuzione una nuova funzionalità della MMU che permetta di portare in memoria le pagine se necessarie.
Ciò che mi permette di fare questa nuova operazione è il **validity bit**:
- Nel caso 1 significa che la pagina si trova in memoria
- Nel caso 0 significa che la pagina non si trova in memoria
Nel caso 0 devo distinguere due casistiche: la pagina può davvero non trovarsi in memoria (va dunque caricata) oppure non è proprio valida. In ogni caso scateno il **page fault**.

Gestire un **page fault** è costoso, dunque va **ridotto** il più possibile.

Lo scenario di un page fault è il seguente:
1. Si genera il page fault
2. Il sistema operativo cerca in un'altra tabella e decide
	- Se la reference è invalida, allora si fa abort
	- Oppure si deduce che la pagina non è in memoria
3. Il sistema operativo cerca un frame libero
4. Si fa lo swap tra il disco e il frame
5. Si setta il validation bit a 1
6. Si riparte dall'istruzione che ha causato il page fault

![[Pasted image 20240315105825.png]]

Altri possibili scenari:
- **Pure demand paging**: si parte con nessuna pagina caricata in memoria, poi ad ogni necessità si genera un page fault e si portano le pagine in memoria
- **Multiple page faults**: un'istruzione può accedere a pagine multiple per completare un'istruzione quindi si genererebbero multipli page faults (il problema è mitigato dalla località dei riferimenti)
- **Swap space**: è una memoria secondaria che viene in aiuto nella procedura di swapping

### Free frame list

La maggior parte dei sistemi operativi mantiene una **lista di frame liberi**: è un pool di frame che soddisfa la richiesta (non è un problema di fit, perciò prendo il primo frame in lista).
Ogni frame libero (appunto perchè libero) contiene il puntatore al prossimo blocco libero: basta sapere dove parte la lista e si possono scorrere tutti i frame liberi.

Talvolta sono necessari dei frame azzerati: si utilizza una tecnica chiamata **zero-fill-on-demand**.

Quando un sistema fa il boot, metto tutto lo spazio disponibile nella free frame list.

### Page fault dettagliato

1. Si scatena la trap
2. Si salvano i registri utente e lo stato del processo in esecuzione
3. Si determina che la trap è un page fault
4. Controlla che l'accesso alla pagina invalida sia legale, e determina la posizione della pagina su disco
5. Fa un issue per una read da disco al frame libero (I/O)
6. Mentre si aspetta il trasferimento la CPU viene concessa ad altri processi
7. Un interrupt informa che la copia è completata (I/O terminato)
8. Salva i registri e lo stato per gli altri utenti
9. Determina che l'input era per I/O da disco
10. Si corregge la entry in memoria 
11. Si attende la CPU
12. Si riprende il processo 

Per valutare le **performance** del demand paging definiamo il **Page fault rating**: $0\leq p \leq 1$
$$
\text{EAT} = (1-p) \cdot \text{memory access} + p\cdot (\text{page fault overhead + swap page out + swap page in})
$$
