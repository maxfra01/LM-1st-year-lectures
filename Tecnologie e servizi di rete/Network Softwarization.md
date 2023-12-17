# Network Softwarization

Il più grande ambito di ricerca sulla reti al giorno d'oggi si basa sul **rivalutare gli approcci tradizionali** dell'architettura di rete:
- Cloud computing
- Big data
- Mobile Traffic
- IoT
Il problema principale riguarda i **pattern del traffico**.
L'architettura tradizionale è basato su TCP/IP, con il metodo client server, dove il server rimaneva la macchina più potente e il client no: ora non è più così, si cerca un approccio più distribuito.

4 Limitazioni dell'architettura tradizionale (dettata da ONF) sono:
- Architettura statica e complessa
- Policies inconsistenti
- Non scalabilità
- Dipendenze da un venditore

# Software-Defined Networking SDN

Un network device è partizionato fra **planes**: control, management e data plan.

![[Pasted image 20231129180610.png]]

Questo nuovo paradigma (SDN) separa il control plane dal forwarding plane,