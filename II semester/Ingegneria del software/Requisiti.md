# Requirement engineering

Si intende la **definizione delle proprietà del prodotto** prima di cominciarne lo sviluppo: è importante sottolineare che nel dialogo con un cliente,  i requisiti non sono quasi mai chiari né tantomeno realizzabili.

Le proprietà di un software si distinguono in funzionali e non funzionali.

![[Pasted image 20240308225547.png]]

La fase iniziale dei requisiti è una discussione informale con un cliente che descrive ciò di cui ha bisogno: l'obiettivo è quello di collezionare requisiti che siano **completi e consistenti**:
- Per completi intendiamo esaustivi
- Per consistenti intendiamo che non ci siano contraddizioni

E' utopistico pensare di generare tali requisiti per motivi come: omissioni, incorrettezze,  ambiguità, troppi dettagli e ridondanze.

Definiamo ora alcune tecniche per la **formalizzazione** dei requisiti:
### Stakeholders

Uno stakeholder è un individuo, un gruppo o un'organizzazione che **ha interesse o influenza su un progetto**, un'azienda o un'organizzazione. Gli stakeholder possono includere dipendenti, clienti, fornitori, investitori, autorità di regolamentazione.

Elencare gli stakeholders è fondamentale per capire i vari punti di vista nello sviluppo del progetto.

### Stories e personas

1. **User Stories (Storie degli Utenti):** Le storie degli utenti sono brevi descrizioni delle funzionalità del software, scritte dal punto di vista degli utenti finali. Solitamente seguono uno schema semplice: "Come\[tipo di utente\], voglio \[funzionalità\] per \[motivo\]". Ad esempio, "Come utente registrato, voglio poter modificare il mio profilo per mantenere le informazioni aggiornate". Le storie degli utenti servono a comunicare in modo chiaro e conciso le esigenze degli utenti al team di sviluppo e forniscono un modo efficace per suddividere il lavoro in compiti gestibili e focalizzati sull'esperienza utente.
    
2. **Personas:** Le personas sono archetipi o rappresentazioni fittizie di utenti reali. Sono create attraverso la raccolta di dati demografici, comportamentali e psicografici degli utenti target. Le personas aiutano il team di sviluppo a comprendere meglio le esigenze, i desideri e le motivazioni degli utenti finali, consentendo loro di progettare e sviluppare un software che soddisfi efficacemente le esigenze di un'ampia varietà di utenti. Ad esempio, potrebbe esserci una persona chiamata "Alice" che è una professionista occupata alla ricerca di un'applicazione mobile che semplifichi la gestione delle sue attività quotidiane. Comprendere le necessità di Alice aiuta il team a concentrarsi sulle funzionalità e sull'esperienza utente che sono più importanti per lei.

### Context diagram

Un diagramma di **contesto** (context diagram) è un tipo di diagramma che rappresenta visivamente l'ambiente esterno di un sistema software e le interazioni principali che il sistema ha con esso. Questo diagramma è spesso utilizzato durante le fasi iniziali del processo di progettazione del software per fornire una visione ad alto livello del sistema e del suo contesto.

Le entità fuori dall'applicazione sono detti **attori**.
Le **interfacce di comunicazione** sono frecce o  linee tra il sistema principale e le entità esterne indicano le interfacce di comunicazione o i flussi di dati tra di essi.

### Requisiti funzionali

Occorre **separare** e **nominare** tutti i requisiti funzionali, inoltre è buona norma l'uso di numerazione gerarchica: questo è utile in fase di sviluppo e testing.

La ISO/IEC 25010 si basa sui seguenti sette attributi di qualità del software:

1. **Funzionalità (Functionality):** Capacità del software di fornire funzionalità che soddisfino i requisiti specificati e gli obiettivi dell'utente.
2. **Affidabilità (Reliability):** Capacità del software di eseguire le sue funzioni richieste in modo affidabile, garantendo la precisione e la consistenza dei risultati.
3. **Usabilità (Usability):** Facilità con cui gli utenti possono utilizzare il software per raggiungere i propri obiettivi, includendo aspetti come l'intuitività dell'interfaccia utente e l'efficienza nelle operazioni. 
4. **Efficienza (Efficiency):** Capacità del software di utilizzare in modo efficiente le risorse disponibili, come la memoria, la CPU e la larghezza di banda di rete, durante l'esecuzione delle operazioni.
5. **Manutenibilità (Maintainability):** Facilità con cui il software può essere modificato, corretto o adattato in risposta a cambiamenti nei requisiti o nell'ambiente operativo.
6. **Portabilità (Portability):** Facilità con cui il software può essere trasferito da un ambiente di sviluppo o di esecuzione all'altro, senza necessità di modifiche sostanziali.
7. **Efficienza (Efficiency):** Capacità del software di eseguire correttamente le funzioni richieste nel tempo previsto e con risorse accettabili.

Inoltre definiamo anche:

1. **Sicurezza (Security):** Si riferisce alla capacità del software di proteggere i dati, i sistemi e le risorse da accessi non autorizzati, modifiche non autorizzate e altri attacchi informatici. Questo include la protezione dei dati sensibili, l'autenticazione degli utenti, il controllo degli accessi e la gestione delle vulnerabilità.
2. **Sicurezza operativa (Safety):** Si riferisce alla capacità del software di garantire che le operazioni siano eseguite in modo sicuro e senza rischi per le persone, le proprietà o l'ambiente circostante. Questo è particolarmente importante nei sistemi critici per la sicurezza, come i sistemi di controllo industriale e i sistemi medici.
3. **Dependability:** Si riferisce alla capacità del software di essere affidabile, disponibile, resiliente e mantenibile nel tempo. Include anche la capacità di ripristinare rapidamente il funzionamento normale dopo un guasto o un'interruzione.

### Requisiti non funzionali

Spesso sono più stringenti dei requisiti funzionali, e riguardano requisiti aggiuntivi (efficienza, tempo di risposta), organizzativi ed esterni.

Devono essere **misurabili**, altrimenti sono del tutto inutili: ad esempio un requisito del tipo "un sistema deve essere facile da usare e minimizzare gli errori" è inutile. Una variante sensata potrebbe essere "dopo 2 ore di training il numero di errori non supera i 2 per giorno".
Di fatto ciò che **non è testabile non è utile**.

Nel caso di **conflitti tra requisiti RF**, gli stakeholders chiave devono prendere una decisione (ad esempio efficienza vs sicurezza): questa è una decisione aziendale perciò i programmatori non possono scegliere.

E' importante definire anche i **requisiti di dominio** (in che ambito è posto il prodotto in uso) e definire un **glossario** per favorire la corretta comunicazione: in particolare per il glossario si può usare il linguaggio **UML**:

![[Pasted image 20240309125127.png]]

# Scenario e Use cases

Uno **scenario** è un esempio di interazione con il software in sviluppo: è composto da una serie di precisi passi (eventi). In questo contesto è necessario considerare il **tempo**.
Un **use case** è un insieme di scenari comuni che hanno un obiettivo simile in comune:

![[Pasted image 20240309125610.png]]

### Relazioni

Definiamo in genere le relazioni fra un attore e un use case:

![[Pasted image 20240309130138.png]]
![[Pasted image 20240309130149.png]]
![[Pasted image 20240309130207.png]]

### Design di un sistema

E' necessario considerare tutti gli elementi di un diagramma di contesto. In particolare produrremo un modello UML di una certa entità del sistema.

# Requirement doc: struttura

1 Overall description
2 Stakeholders
3 Context diagram and interfaces
4 Requirements
	• Functional
	• Non functional
	• Domain
5 Use case diagram
6 Scenarios
7 Glossary
8 System design

L'ordine può differire a seconda dell'importanza dei vari paragrafi, inoltre esistono numerose tipologie di metodi per sviluppare il documento.