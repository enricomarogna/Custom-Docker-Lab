# ADGUARD

AdGuard Home è un software a livello di rete per il blocco di pubblicità e monitoraggio. Copre tutti i dispositivi domestici e senza la necessità di dover installare software lato client.

Funziona come un server DNS che reindirizza i domini di tracciamento a un "buco nero", impedendo così ai dispositivi di connettersi a quei server.

Si rimanda alla [guida completa](https://github.com/AdguardTeam/AdGuardHome/wiki/Getting-Started).

### CERTIFICATI
Tramite il binding del volume relativo alla variabile `CERT_PATH` vengono resi disponibili nel container i certificati generati automaticamente da **Nginx Proxy Manager** tramite Cloudflare.
Generalmente la cartella si trova dentro `nginx-pm/conf/letsencrypt/archive/`.
In questo modo è sufficiente inserire il percorso del certificato nelle *Impostazioni di Critografia*, ad esempio:
```
/opt/adguardhome/conf/ssl/fullchain.pem
```
e
```
/opt/adguardhome/conf/ssl/privkey.pem
```

### ACCESSO AL CONTAINER
Per accedere al container:
```bash
docker exec -it adguard /bin/sh
```


---
Per maggiori specifiche visitare il repository ufficiale:
[https://github.com/AdguardTeam/AdGuardHome](https://github.com/AdguardTeam/AdGuardHome)