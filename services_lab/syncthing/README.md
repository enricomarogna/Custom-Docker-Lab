# SYNCTHING
**Syncthing** è una applicazione client/server di sincronizzazione file open-source e multipiattaforma, scritta in Go.

Gli apparati che eseguono il software sono in grado di condividere file senza la necessità di usufruire di un servizio di storage proprietario:

- Nessun dato viene mai memorizzato in luogo diverso dagli apparati stessi.
- Tutte le comunicazioni sono criptate tramite TLS.
- Ogni nodo viene identificato da un certificato crittografico. Solo i nodi esplicitamente autorizzati sono in grado di connettersi al cluster.

La configurazione può avvenire tramite browser ed è supportato il protocollo UPnP/NAT-PMP per la configurazione automatica dei router che lo supportano. Dalla versione 0.14.40 include il monitoraggio costante del filesystem, il che riduce la necessità di scansioni ripetute aumentando le prestazioni generali.
Sono disponibili [strumenti di terze parti](https://syncthing.net/downloads/) che, interfacciandosi al software, permettono di semplificarne ulteriormente l'utilizzo. 



---
Per maggiori specifiche visitare il repository ufficiale:
[https://github.com/linuxserver/docker-syncthing](https://github.com/linuxserver/docker-syncthing)