# TRANSMISSION
Questo container contiene OpenVPN e Transmission con una configurazione in cui Transmission è in esecuzione solo quando OpenVPN ha un tunnel attivo. Ha il supporto integrato per molti provider VPN popolari per semplificare l'installazione.
Viene accompagnato da un secondo [container che funge da proxy](https://haugene.github.io/docker-transmission-openvpn/vpn-networking/) sulla porta 8080.

Il container è configurato per utilizzare NordVPN ma l'immagine supporta anche [altri provider](https://haugene.github.io/docker-transmission-openvpn/supported-providers/).


---
Per maggiori specifiche visitare il repository ufficiale:
[https://github.com/haugene/docker-transmission-openvpn](https://github.com/haugene/docker-transmission-openvpn)