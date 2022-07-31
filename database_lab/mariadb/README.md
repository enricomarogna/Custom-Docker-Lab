# MARIADB
**Mariadb** è uno dei server di database più popolari. Realizzato dagli sviluppatori originali di MySQL.

### PERMESSI
Assegnare i permessi 644 al file `custom.cnf` per evitare l'errore *Warning: World-writable config file '/etc/mysql/conf.d/custom.cnf' is ignored* [#43](https://github.com/linuxserver/docker-mariadb/issues/43): 

```bash
chmod 644 conf/custom.cfn
```

---
Per maggiori specifiche visitare il repository ufficiale:
[https://github.com/linuxserver/docker-mariadb](https://github.com/linuxserver/docker-mariadb)
