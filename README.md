
# CUSTOM DOCKER LAB

## Hardware:
- **Host**: `Synology NAS DS720+ - DSM6 - 10GB RAM - 1TB CACHE SSD RAID1 - 4TB Raid SHR`
- **Modem/Router**: `Fritz!Box 7590 - OS 07.39-94931 BETA - Wireguard VPN`
- **Switch**: `Zyxel 8Port Desktop Gigabit Ethernet`

---
## PANORAMICA

| SERVIZI                 | DESCRIZIONE                                          |
|:------------------------|:-----------------------------------------------------|
| [Diun][]                | Notificatore aggiornamento immagini                  |
| [Flame][]               | Startpage leggero con editor integrato               |
| [Glances][]             | Strumento di monitoraggio multipiattaforma           |
| [Linkding][]            | Gestore segnalibri minimale, veloce e leggero        |
| [Socket Proxy][]        | Connessione proxy al docker socket                   |


---
## RETE
### CREAZIONE **MACVLAN**
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

### CREAZIONE RETE **BRIDGE**
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
    |   |__ container_folder/
    |   |   |__ .env
    |   |   |__ README.md
    |   |   |__ docker-compose.yml
    |   |   |__ conf_folder/
    |   |   |   |__ .gitignore
    |   |   |__ secrets_folder/
    |   |       |__ secret_name.txt
    |   |
    |   |
    |   |__ container_folder/
    |       |__ .env
    |       |__ README.md
    |       |__ docker-compose.yml
    |       |__ conf_folder/
    |   |   |   |__ .gitignore
    |       |__ secrets_folder/
    |           |__ secret_name.txt
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
Per la creazione del *secret* è necessario, a partire dalla root del container, assegnare i proprietari e i permessi:
- Assegnazione dell'utente e del gruppo proprietario della *secrets_folder* e dei suoi files `sudo chown -R root:root secrets_folder/`
- Permessi della *secrets_folder* e dei suoi files `sudo chmod -R 600 secrets_folder/`
- Creazione del file che contiente il segreto `nano secret_name.txt`
- Salvare `^O` e uscire `^X`
- Creazione del segreto `docker secret create secret_name secret_name.txt`

Dichiararne l'utilizzo in *docker-compose.yml*:
```yml
version: "3.7"

# NETWORKS
networks:
  [...]

# SECRETS
secrets:
  container_secret_name:
  file: ./secrets_folder/secret_name.txt

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
    volumes:
      - VARIABILE_FILE=/run/secrets/container_secret_name
    secrets:
      - container_secret_name
    [...]
```

**N.B.:** alcune immagini con consentono l'uitilizzo dei secret e sarà quindi necessario optare per l'inserimento della password/token come variabile dentro *.env*



[Diun]:           /monitoring_lab/diun/
[Flame]:          /monitoring_lab/flame/
[Glances]:        /monitoring_lab/glances/
[Linkding]:       /services_lab/linkding/
[Socket Proxy]:   /services_lab/socket_proxy/