# NORDVPN
**NORDVPN** è un provider VPN, questo container mette a disposizione una connessione privata tramite i propri [server distribuiti per il mondo](https://nordvpn.com/servers/).

### CONFIGURAZIONE DEL CONTAINER CLIENT
Supponendo di voler collegare **JDownloader**, la cui porta di acesso è '5800', al container di NORDVPN, è sufficiente aprire su quest'ultimo la porta '5800'. Sul container di JDownloader non dovranno essere configurati `ports` e `networks`. Al loro posto passare `network_mode: container:nordvpn`. Ad esempio:

```bash
services:
  jdownloader:
    container_name: JDownloader
    image: jlesage/jdownloader-2
    environment:
      - PUID=100
      - PGID=100
      - TZ=Europe/Rome
      - SECURE_CONNECTION=1
    volumes:
      - "./conf/config:/config:rw"
      - "./jdownloader:/output:rw"
    network_mode: container:nordvpn
    restart: unless-stopped
```

---
Per maggiori specifiche visitare il repository ufficiale:
[https://github.com/azinchen/nordvpn](https://github.com/azinchen/nordvpn)