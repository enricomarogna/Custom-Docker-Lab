# REDIS
**Redis** è un DBMS NoSQL di tipo “key/value storage”. Si basa infatti su una struttura a dizionario: ogni valore immagazzinato è abbinato ad una chiave univoca che ne permette il recupero.

Questo progetto si è guadagnato un posto di spicco tra i database NoSQL grazie ad alcuni vantaggi di rilievo:

- estrema velocità: Redis conserva i dati in memoria RAM, salvandoli in maniera persistente solo in un secondo momento. Ciò permette di ottenere ottime prestazioni in scrittura e lettura;
- dispone di una grande varietà di tipi di dato. Quindi nonostante l'architettura dell'archivio sia basata su una struttura a dizionario, i valori possono assumere varie forme: liste, dizionari stessi, e molto altro. Da questo punto di vista, Redis può essere visto come un gestore di strutture dati persistenti;
- tutte le operazioni sono atomiche, quindi in caso di accessi concorrenti da parte di più client, i dati forniti risulteranno sempre aggiornati;
- possibilità di implementare configurazioni multi-nodo, cluster e replication.

---
### PERSISTENZA DEL DATABASE
Poiché si tratta di un contenitore non root, i file e le directory montati devono disporre delle autorizzazioni appropriate per l'`UID` **1001**:

```bash
sudo chown 1001 ./conf/data
```


---
Per maggiori specifiche visitare il repository ufficiale:
[https://github.com/bitnami/bitnami-docker-redis](https://github.com/bitnami/bitnami-docker-redis)