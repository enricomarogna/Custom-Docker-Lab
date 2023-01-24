# JELLYFIN
**Jellyfin** è la miglior alternativa a Plex, ricco di plugin e completamente gratuito.

## ACCELERAZIONE HARDWARE
Fornire i permessi sul NAS
```bash
chown -R root:video /dev/dri
chmod -R g+rw /dev/dri
```
Nota: si consiglia l'inserimento nel `Task Scheduler` del NAS in modo che ad ogni riavvio della macchina vengano riassegnati i giusti permessi.
Nelle impostazioni del backend di Jallyfin è necessario configurari la transcodifica.



---
Per maggiori specifiche visitare il repository ufficiale:
[https://docs.linuxserver.io/images/docker-jellyfin](https://docs.linuxserver.io/images/docker-jellyfin)