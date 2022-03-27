# Docker Socket Proxy

Dare accesso al socket Docker potrebbe significare dare accesso root all'host, o anche a tutto lo swarm, ma alcuni servizi richiedono l'aggancio al socket per reagire agli eventi, ecc. L'uso di questo proxy consente di bloccare tutto ciò che si ritiene che quei servizi non dovrebbero fare.

Viene utilizzata l'immagine ufficiale di [HAProxy](http://www.haproxy.org/) basata su [Alpine](https://alpinelinux.org/), con un file di configurazione, che blocca l'accesso all'API del socket Docker in base alle variabili di ambiente impostate e restituisce uno stato `HTTP 403 Forbidden` per quelle richieste pericolose che non dovrebbero mai verificarsi.

## RACCOMANDAZIONI SULLA SICUREZZA

- Non esporre mai la porta di questo container a una rete pubblica ma solo per la rete docker dove risiedono solo il proxy stesso e i servizi che lo utilizzano.
- Revocare l'accesso a qualsiasi sezione API che si ritiene non necessaria al servizio.
- Questa immagine non include il supporto TLS ma solo un semplice proxy HTTP per il *socket Docker Unix host* (che non è protetto da TLS anche se l'host è configurato per la protezione TLS). Questo è in base alla progettazione perché bisognerebbe limitare l'accesso ad esso tramite il firewall integrato di Docker.


- [Leggere la documentazione](#VERSIONI-API-SUPPORTATE) per la versione API in utilizzo, per **capire cosa si sta facendo**.

---
## CONCEDERE O REVOCARE L'ACCESSO A DETERMINATE SEZIONE API

È possibile farlo tramite le variabili di ambiente che normalmente corrispondono al prefisso URL, ad esempio `AUTH` blocca l'accesso alle parti `/auth/*` delle API e così via.

I possibili valori delle variabili sono:

- `0` per **revocare** l'accesso.
- `1` per **concedere** l'accesso.

### ACCESSO CONCESSO PREDEFINITO

Queste sezioni API sono per lo più innocue e quasi necessarie per qualsiasi servizio che utilizza l'API, quindi sono concesse per impostazione predefinita.

- `EVENTS`
- `PING`
- `VERSION`

### ACCESSO REVOCATO PREDEFINITO

#### CRITICO PER LA SICUREZZA

Queste sezioni API sono considerate critiche per la sicurezza e quindi l'accesso viene revocato per impostazione predefinita. **Massima cautela quando si abilitano questi**:

- `AUTH`
- `SECRETS`
- `POST`: Quando disabilitato, sono consentite solo le operazioni `GET` e `HEAD`, il che significa che qualsiasi sezione dell'API è in sola lettura. Notare che questo ha effetto globale.
- `DELETE`: Abilta o disabilita tutte le operazioni `DELETE`

#### NON SEMPRE NECESSARIO

Probabilmente sarà neccesario concedere l'accesso ad alcune di queste sezioni dell'API, che possono esporre alcune informazioni di cui il servizio non ha bisogno.

| GET            | POST                  | DELETE              |
|:---------------|:----------------------|:--------------------|
| `BUILD`        | `ALLOW_RESTARTS`      | `NETWORKS_DELETE`   |  
| `COMMIT`       | `CONTAINERS_PRUNE`    | `CONTAINERS_DELETE` |    
| `CONFIGS`      | `CONTAINERS_CREATE`   | `IMAGES_DELETE`     |
| `CONTAINERS`   | `CONTAINERS_RESIZE`   | `VOLUMES_DELETE`    | 
| `DISTRIBUTION` | `CONTAINERS_START`    |                     |
| `EXEC`         | `CONTAINERS_UPDATE`   |                     |
| `IMAGES`       | `CONTAINERS_RENAME`   |                     |
| `INFO`         | `CONTAINERS_PAUSE`    |                     |
| `NETWORKS`     | `CONTAINERS_UNPAUSE`  |                     |
| `NODES`        | `CONTAINERS_ATTACH`   |                     |
| `PLUGINS`      | `CONTAINERS_WAIT`     |                     |
| `SERVICES`     | `CONTAINERS_EXEC`     |                     |
| `SESSION`      | `VOLUMES_CREATE`      |                     |
| `SWARM`        | `VOLUMES_PRUNE`       |                     |
| `SYSTEM`       | `NETWORKS_CREATE`     |                     |
| `TASKS`        | `NETWORKS_PRUNE`      |                     |
| `VOLUMES`      | `NETWORKS_CONNECT`    |                     |
|                | `NETWORKS_DISCONNECT` |                     |
|                | `IMAGES_CREATE`       |                     |
|                | `IMAGES_PRUNE`        |                     |

`ALLOW_RESTARTS` permette di utilizzare `kill`, `stop` e `restart` nei containers

## LOGGING

È possibile impostare il livello di registrazione o il livello di gravità dei messaggi da registrare con la variabile di ambiente `LOG_LEVEL`. Il valore predefinito è info. I valori possibili sono: `debug`, `info`, `notice`, `warning`, `err`, `crit`, `alert` e `emerg`.

## VERSIONI API SUPPORTATE

- [1.27](https://docs.docker.com/engine/api/v1.27/)
- [1.28](https://docs.docker.com/engine/api/v1.28/)
- [1.29](https://docs.docker.com/engine/api/v1.29/)
- [1.30](https://docs.docker.com/engine/api/v1.30/)
- [1.37](https://docs.docker.com/engine/api/v1.37/)