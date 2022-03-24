
# CUSTOM DOCKER LAB

## Hardware:
- **Host**: `Synology NAS DS720+ - DSM6 - 10GB RAM - 4TB Raid SHR`
- **Modem/Router**: `Fritz!Box 7590 - OS 07.39-94931 BETA - Wireguard VPN`
- **Switch**: `Zyxel 8Port Desktop Gigabit Ethernet`

---
## PANORAMICA

#### SERVICES
- [Linkding](/services_lab/linkding/) - Gestore segnalibri minimale, veloce e leggero.

---
## RETE
### CREAZIONE **MACVLAN**
La rete MACVLAN ospiterà **Nginx Proxy Manager** e containers e viene creata in questo modo:

```bash
sudo docker network create -d macvlan -o parent=eth0 --subnet=192.168.1.0/24 --gateway=192.168.1.1 --ip-range=192.168.1.100/24 npm_proxy
```
N.B.: Sarà neccessario aprire le porte `80` e `443` del router sull'ip del container di **Nginx Proxy Manager**.

### CREAZIONE RETE **BRIDGE**
Questa servirà alla comunicazione con l'host:
- Subnet: `192.168.200.0/24`
- Ip range: `192.168.200.2/32`
- Gataway: `192.168.200.1`

Aprire sul firewall l'ip `192.168.200.2` con sottomaschera `255.255.255.255`

---
## STRUTTURA

### LABORATORIO
```
root
    |-- README.md
    |
    |-- .gitignore
    |
    |-- profile_1
    |   |-- container_folder/
    |   |   |__ .env
    |   |   |__ README.md
    |   |   |__ docker-compose.yml
    |   |   |__ conf_folder/
    |   |   |__ secrets_folder/
    |   |       |__ secret_name.txt
    |   |
    |   |
    |   |-- container_folder/
    |       |__ .env
    |       |__ README.md
    |       |__ docker-compose.yml
    |       |__ conf_folder/
    |       |__ secrets_folder/
    |           |__ secret_name.txt
    |
    |-- profile_2
        |-- ...
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

# SECRETS
secrets:
  container_secret_name:
  file: ./secrets_folder/secret_name.txt

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
    secrets:
      - secret_name
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