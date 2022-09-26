# PROTONVPN
**ProtonVPN** crea un container per la connessione VPN di Proton VPN. [Qui la documentazione](https://tprasadtp.github.io/protonvpn-docker/#/README).
Il container client che vuole sfruttare la rete generata da ProtonVPN non dovrà configurare le porte e dovrà avere `network_mode` configurato su `container:protonvpn`.
La porta di accesso al container client verrà esposta su ProtonVPN tramite `expose`. Ad esempio se il container client è esposto sulla porta `8080`, sarà raggiungibile su `protonvpn:8080`


---
Per maggiori specifiche visitare il repository ufficiale:
[https://github.com/tprasadtp/protonvpn-docker](https://github.com/tprasadtp/protonvpn-docker)