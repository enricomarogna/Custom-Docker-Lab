
# CUSTOM DOCKER LAB

## Hardware:
- **Host**: `Synology NAS DS720+ - DSM6 - 10GB RAM - 1TB CACHE SSD RAID1 - 4TB Raid SHR`
- **Modem/Router**: `Fritz!Box 7590 - OS 07.39-94931 BETA - Wireguard VPN`
- **Switch**: `Zyxel 8Port Desktop Gigabit Ethernet`

---
## PANORAMICA

| SERVIZI                 | PROFILO   | DESCRIZIONE                                           |
|:------------------------|:----------|:------------------------------------------------------|
| [AdGuard][]             |Services   | Server DNS                                            |
| [Adminer][]             |Database   | Gestionale database                                   |
| [Authelia][]            |Frontend   | Server di autenticazione                              |
| [Baikal][]              |Services   | Server CalDAV e CarDAV                                |
| [Calibre][]             |Services   | Gestore e organizzatore di librerie per ebook         |
| [Cloudflare DDNS][]     |Services   | Server DDNS                                           |
| [Diun][]                |Monitoring | Notificatore aggiornamento immagini                   |
| [Flame][]               |Monitoring | Startpage leggero con editor integrato                |
| [Flaresolverr][]        |Monitoring | Server proxy per bypassare la protezzione Cloudflare  |
| [FreshRSS][]            |Services   | Server per la gestione dei feed rss                   |
| [Glances][]             |Monitoring | Strumento di monitoraggio multipiattaforma            |
| [Gotify][]              |Monitoring | Server per la gestione delle notifiche                |
| [Jackett][]             |Indexers   | Server indexer manager/proxy                          |
| [JDownloader][]         |Services   | Potente applicazione per la gestione dei download     |
| [Mariadb][]             |Database   | Server database MySQL                                 |
| [Monica][]              |Services   | Server CRM per contatti personali                     |
| [Navidrome][]           |Mediacenter| Server e streamer di raccolte musicali                |
| [Nginx Proxy Manager][] |Frontend   | Reverse Proxy con SSL                                 |
| [Organizr][]            |Frontend   | Potente statpage e hub                                |
| [Overseerr][]           |Indexers   | Ricerca e suggerimenti di contenuti multimediali      |
| [Lidarr][]              |Indexers   | Gestore e organizzatore di librerie musicali          |
| [Linkding][]            |Services   | Gestore segnalibri minimale, veloce e leggero         |
| [Portainer][]           |Frontend   | Amministratore container, immagini, volumi e altro    |
| [Postgres][]            |Database   | Server database PostgreSQL                            |
| [Plex][]                |Mediacenter| Server e client per lo stream di file multimediali    |
| [Prowlarr][]            |Indexers   | Server indexer manager/proxy                          |
| [Radarr][]              |Indexers   | Gestore e organizzatore di librerie per films         |
| [Readarr][]             |Indexers   | Gestore e organizzatore di librerie per ebook         |
| [Redis][]               |Database   | Server database NoSQL                                 |
| [Rotki][]               |Services   | Tracker di criptovalute                               |
| [RssBridge][]           |Services   | Generatore di feed RSS e Atom                         |
| [Sonarr][]              |Indexers   | Gestore e organizzatore di librerie per serie tv      |
| [Syncthing][]           |Services   | Client/server di sincronizzazione file                |
| [Transmission-OpenVPN][]|Services   | Torrent client                                        |
| [Ubooquity][]           |Mediacenter| Server di lettura/download di ebook della libreria    |
| [UptimeKuma][]          |Monitoring | Monitoraggio self-hosted come "Uptime Robot"          |
| [Vaultwarden][]         |Services   | Gestore di password                                   |
| [Wallabag][]            |Services   | Gestore di bookmarks e preferiti                      |
| [Wireguard][]           |Frontend   | Server VPN                                            |


---
## RETE
### 1. CREAZIONE **MACVLAN**
La rete MACVLAN ospiterà **Nginx Proxy Manager** e containers, verrà denominata **npm_proxy** e viene creata in questo modo:

Determinare l'interfaccia di rete attiva dell'host:
```bash
ifconfig
```
o meglio:
```bash
ip route get 1.1.1.1 | sed -nr 's/.*dev ([^\ ]+).*/\1/p'
```
Supponendo di ottenere `eth0`, che la subnet sia `192.168.1.0/24`, il gateway `192.168.1.1` e di voler chiamare la rete `npm_proxy`:

```bash
sudo docker network create -d macvlan -o parent=eth0 --subnet=192.168.1.0/24 --gateway=192.168.1.1 --ip-range=192.168.1.100/24 npm_proxy
```
N.B.: Sarà neccessario aprire le porte `80` e `443` del router sull'ip del container di **Nginx Proxy Manager**.

### 2. CREAZIONE RETE **BRIDGE**
Questa servirà alla comunicazione tra l'host e il container di **Nginx Proxy Manager**:
- Subnet: `192.168.200.0/24`
- Ip range: `192.168.200.2/32`
- Gataway: `192.168.200.1`

```bash
sudo docker network create --subnet=192.168.200.0/24 --gateway=192.168.200.1 --ip-range=192.168.200.2/32 npm_bridge
```

Aprire sul firewall l'ip `192.168.200.2` con sottomaschera `255.255.255.255`

---
## STRUTTURA

### LABORATORIO
```
root
    |__ README.md
    |
    |__ .gitignore
    |
    |__ profile_1
    |   |__ container_folder_1/
    |   |   |__ .env
    |   |   |__ README.md
    |   |   |__ docker-compose.yml
    |   |   |__ conf_folder/
    |   |   |   |__ .gitignore
    |   |   |__ secrets_folder/
    |   |       |__ SECRET_NAME.txt
    |   |       |__ .gitignore
    |   |
    |   |
    |   |__ container_folder_2/
    |       |__ ...
    |
    |-- profile_2
        |__ ...
```

### VARIABILI - *.env*

È possibile ricavare PUID e PGID dal terminale dell'host: `id $user`
```bash
PUID=1000
PGID=1000
TZ="Europe/Rome"            
CONTAINER_NAME=             # Nome da assegnare al container
IMG_LINK=                   # Link dell'immagine
IMG_VERSION=                # Versione dell'immagin         
IP=                         # Indirizzo ip univoco da assegnare al cont         
PORT_INT=                   # Porta interna del container
PORT_PUB=                   # Porta pubblica del container
PROFILE=                    # Profilo 
DOMAIN_NAME=                # Nome dominio, es. dominio.com
```

### CONTAINER - *docker-compose.yml*
```yml
---
version: "3.7"

# NETWORKS
networks:
  npm_proxy:
    external:
      name: npm_proxy

# SERVICES
services:
  cont_name:
    container_name: ${CONTAINER_NAME}
    image: ${IMG_LINK}:${IMG_VERSION:-latest}
    environment:
      - PUID=${PUID}
      - PGID=${PGID}
      - TZ=${TZ}
    volumes:
      - './conf_folder:/data'
    labels:
      - "diun.enable=true"
      # Flame
      - flame.type=app
      - flame.name=${CONTAINER_NAME}
      - flame.url=https://${CONTAINER_NAME}.${DOMAIN_NAME}
      - flame.icon=${CONTAINER_NAME}.svg
    ports:
      - target: ${PORT_INT}
        #published: ${PORT_PUB}
        protocol: tcp
    networks:
      npm_proxy:
        ipv4_address: ${IP}
    profiles:
      - ${PROFILE}
    restart: unless-stopped
```

---

## SECRETS
Per alcuni containers è necessario indicare dati sensibili, come *passwords* oppure dei *token* e riportali nel *docker-compose.yml* o nel file *.env* è una pratica sconsigliata per quanto riguarda la sicurezza poichè questi dati vengono riportati all'interno del container, pertanto un attaccante potrebbe entrarne in possesso.
Per questo sono stati sviluppati i *secrets*.

#### CREAZIONE DEL SECRET
Per la creazione del *secret* è necessario seguire i seguenti passi:

- Creazione del file che contiente il segreto 
  ```bash
  printf "PaSSw0rdSegReta" > ./secrets_folder/SECRET_NAME.txt
  ```
  *N.B.*: `printf` [non gestisce correttamente](https://www.ibm.com/docs/en/i/7.2?topic=functions-printf-print-formatted-characters) l'inserimento della password se questa contiene il carattere `%`.
  
- Creazione del segreto 
  ```bash
  docker secret create SECRET_NAME /secrets_folder/SECRET_NAME.txt
  ```
- Assegnazione dell'utente e del gruppo proprietario della *secrets_folder* e dei suoi files 
  ```bash
  sudo chown -R root:root secrets_folder/ && sudo chmod -R 600 secrets_folder/
  ```

Dichiararne l'utilizzo in *docker-compose.yml*:
```yml
version: "3.7"

# NETWORKS
networks:
  [...]

# SECRETS
secrets:
  SECRET_NAME:
    file: ./secrets_folder/SECRET_NAME.txt

# SERVICES
services:
  [...]
```
e utilizzarlo all'interno del container, aggiungendo al nome della variabile **_FILE**:

```yml
# SERVICES
services:
  cont_name:
    [...]
    environment:
      - VARIABILE_FILE=/run/secrets/SECRET_NAME
    secrets:
      - SECRET_NAME
    [...]
```

**N.B.:** alcune immagini con consentono l'utilizzo dei secret e sarà quindi necessario optare per l'inserimento della password/token come variabile dentro *.env*




[AdGuard]:                /services_lab/adguard/
[Adminer]:                /database_lab/adminer/
[Authelia]:               /frontend_lab/authelia/
[Baikal]:                 /services_lab/baikal/
[Calibre]:                /mediacenter_lab/calibre/
[Cloudflare DDNS]:        /services_lab/cloudflare_ddns/
[Diun]:                   /monitoring_lab/diun/
[Flame]:                  /monitoring_lab/flame/
[Flaresolverr]:           /indexers_lab/flaresolverr/
[FreshRSS]:               /services_lab/freshrss/
[Glances]:                /monitoring_lab/glances/
[Gotify]:                 /monitoring_lab/gotify/
[Jackett]:                /indexers_lab/jackett/
[JDownloader]:            /services_lab/jdownloader/
[Mariadb]:                /database_lab/mariadb/
[Monica]:                 /services_lab/monica/
[Navidrome]:              /mediacenter_lab/navidrome/
[Nginx Proxy Manager]:    /frontend_lab/nginx-pm/
[Organizr]:               /frontend_lab/organizr/
[Overseerr]:              /indexers_lab/overseerr/
[Lidarr]:                 /indexers_lab/lidarr/
[Linkding]:               /services_lab/linkding/
[Portainer]:              /frontend_lab/portainer/
[Postgres]:               /database_lab/postgres/
[Plex]:                   /mediacenter_lab/plex/
[Prowlarr]:               /indexers_lab/prowlarr/
[Readarr]:                /indexers_lab/readarr/
[Radarr]:                 /indexers_lab/radarr/
[Redis]:                  /database_lab/redis/
[Rotki]:                  /services_lab/rotki/
[RssBridge]:              /services_lab/rssbridge/
[Sonarr]:                 /indexers_lab/sonarr/
[Syncthing]:              /services_lab/syncthing/
[Transmission-OpenVPN]:   /services_lab/transmission/
[Ubooquity]:              /mediacenter_lab/ubooquity/
[UptimeKuma]:             /monitoring_lab/uptimekuma/
[Vaultwarden]:            /services_lab/vaultwarden/
[Wallabag]:               /services_lab/wallabag/
[Wireguard]:              /frontend_lab/wireguard/