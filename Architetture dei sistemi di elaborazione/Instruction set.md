# Instruction set Architecture

L'**ISA** è il modo in cui il sistema è visto da un compilatore o da un programmatore.
L'architettura è scelta in modo da bilanciare vari parametri come complessità, performance, potenza ecc...

I processori sono classificati in base alla loro **memoria interna**:
- **Stack**
- **Accumulatore**
- **Registri**, suddivisi in register-memory e register-register
Attualmente i processori sono **general-purpose register** in quanto i registri sono molto più rapidi delle memorie e per il compilatore sono molto semplici da usare.

![[Pasted image 20231003104547.png]]

Un'altra classificazione per i processori riguarda il **numero degli operandi della ALU**: normalmente il numero degli operandi varia da 0 a 3.

![[Pasted image 20231003110032.png]]

### Memory addressing
Prima distinzione su **Little Endian** e **Big Endian**, nel primo caso (come scriviamo) il LSB è a destra, viceversa nel big endian il LSB è quello più a sinistra.
Una seconda distinzione si basa sull'accesso **allineato o meno** della memoria: l'accesso allineato consente di leggere solamente a multipli del parallelismo della memoria; nel caso disallineato questo non è vero e i processori che consentono questo sono molto più complessi.

| Modo di indirizzamento      | Esempio di codice      | Commento                                                                              |
| --------------------------- | ---------------------- | ------------------------------------------------------------------------------------- |
| Register mode               | Add R4,R3              |                                                                                       |
| Immediate mode              | Add R4, #3             |                                                                                       |
| Displacement mode           | Add R4, 100(R1)        | Tramite questa notazione saltiamo di 100 posizioni in memoria rispetto al registro R1 |
| Indirect mode               | Add R4, (R1)           | Simile al precedente ma usando i puntatori                                            |
| Indexed mode                | Add R3, (R1+R2)        | La somma di R1 ed R2 da la posizione di memoria alla quale vogliamo accedere          |
| Absolute mode               | Add R3, (1001)         | Indica esplicitamente la posizione di memoria                                         |
| Memory indirect mode        | Add, R1, @(R3)         | Equivalente a \*(\*R3)                                                                |
| (post) Autoincremented mode | Add R1, (R2)+          | A seguito dell'addizione incremento il registro R2, utile per scandire vettori        |
| Autodecrement mode          | Add R1, -(R2)          | Analogo al precedente                                                                 |
| Scaled mode                 | Add R1, 100(R2) \[R3\] | Non usati                                                                                      |

Come scegliere il miglior modo di indirizzamento?


