# NGINX PROXY MANAGER

Fornisce un modo semplice per realizzare un reverse proxy con gestione automatica di attivazione e rinnovo SSL con Let's Encrypt.

### CONNESSIONE
- Espone i servizi web sulla rete
- SSL gratuito con Let's Encrypt
- Progettato pensando alla sicurezza
- Perfetto per le reti domestiche

### HOST PROXY
Esponi i servizi Web di rete privata e per connettersi ovunque.

### INTERFACCIA UTENTE
Basata su Tabler, l'interfaccia è piacevole, snella e pulita.

### SSL GRATUITO
Il supporto integrato di Let's Encrypt consente di proteggere i servizi Web senza alcun costo.
Il rinnovo dei certificati è completamente automatizzato

### DOCKER FTW
Creato come immagine Docker, Nginx Proxy Manager richiede solo un database.

### UTENTI MULTIPLI
Configurazione di ulteriori utenti per visualizzare o gestire i propri host.
Sono disponibili autorizzazioni di accesso completo.

---
## ROUTER
Aprire le porte del router sull'ip `192.xxx.x.254` assegnato a `NGINX Proxy Manager`:

| SERVIZI                 | PROTOCOLLO  | PORTA INTERNA | PORTA ESTERNA |
|:------------------------|:------------|:--------------|---------------|
| HTTP                    |TCP          | 80            |80             |
| HTTPS                   |TCP          | 443           |443            |
| ADGUARD                 |TCP          | 53            |53             |
| ADGUARD                 |UDP          | 53            |53             |
| ADGUARD DoT             |TCP          | 853           |853            |

---
## PRIMO ACCESSO

URL: `http://192.xxx.x.254:81`

Utente Admin di default
- Email:    `admin@example.com`
- Password: `changeme`


## SECRETS

La [documentazione ufficiale](https://nginxproxymanager.com/advanced-config/#docker-secrets) conferma la possibilità di utilizzare i *secrets*

"Questa immagine supporta l'uso dei *secrets* Docker per importare da file e impedire che nomi utente o password sensibili vengano passati o conservati in testo normale.

È possibile impostare qualsiasi variabile di ambiente da un file aggiungendo __FILE (*FILE* con doppio underscore) al nome della variabile di ambiente."



```bash
services:
  app:
    [...]
    environment:
      DB_MYSQL_HOST: "db"
      DB_MYSQL_PORT: 3306
      DB_MYSQL_USER: "npm"
      DB_MYSQL_PASSWORD__FILE: /run/secrets/MYSQL_PWD
      [...]
    volumes:
      [...]
    secrets:
      - MYSQL_PWD
    [..]
```

**N.B.**: Attualmente non sono riuscito a far funzionare correttamente i secrets per questa immagine quindi inserisco i dati tramite variabile nel fine **.env**


---
Per maggiori specifiche visitare il repository ufficiale:
[https://github.com/NginxProxyManager/nginx-proxy-manager](https://github.com/NginxProxyManager/nginx-proxy-manager)