# NoSQL

"Nuovo" approccio per gestire un database dove **non sono presenti join**, **non c'è uno schema logico** ed è pensato per avere potenziale **scalabilità orizzontale**.
- Al posto delle tabelle ci sono delle strutture speciali come **document-based**, **coppie chiave-valore**, **DB basati sui grafi**, e columnar storage.
- Le query saranno dunque speciali e basati sulla struttura della memorizzazione dei dati.
- Non ci sono i join
- E' adatto per approcci gerarchici (come gestire JSON)
- Esempio: **MongoDB**

### Tipi di DB NoSQL

Le principali tipologie sono 4:

![[Pasted image 20231214102750.png]]

| Key-value                     | Column-family                                                   | Graph                                            | Document                                                                |
| ----------------------------- | --------------------------------------------------------------- | ------------------------------------------------ | ----------------------------------------------------------------------- |
| Approccio più semplice        | Le colonne rispecchiano gli attributi dei DB relazionei         | Basato su archi e nodi, teoria dei grafi         | I DB memorizzano i documenti                                            |
| Performance alte              | Anche in questo caso una chiave permette di accedere a una riga | Si usano per memorizzare informazioni sulle reti | I documenti sono esaustivi, quindi gli attributi e il valore coincidono |
| Facilmente scalabile e veloce |                                                                 | Perfetti per molte applicazioni reali            | I documenti hanno natura eterogenea                                                                        |
|                               |                                                                 |                                                  |MongoDB                                                                         |
 
### CouchDB

Un esempio di NoSQL è **Cluster Of Unreliable Commodity Hardware** (COUCH). E' un DB a tipo **document oriented** su cui è possibile eseguire query in una maniera **MapReduce**.
- Offre anche **replication**, per garantire la protezione dei dati, dato che usiamo hardware di commodity (economico).

# MongoDB

