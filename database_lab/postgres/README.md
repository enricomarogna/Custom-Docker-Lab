# POSTGRES
**PostgreSQL** è un DBMS ad oggetti ed è una reale alternativa sia rispetto ad altri prodotti liberi come MySQL, Firebird SQL e MaxDB che a quelli a codice chiuso come Oracle, IBM Informix o DB2 ed offre caratteristiche uniche nel suo genere che lo pongono per alcuni aspetti all'avanguardia nel settore delle basi di dati. 

# DATABASE MULTIPLI
Per la creazione di più database è necessario indicare, nella variabile `POSTGRES_MULTIPLE_DATABASES`, i nomi dei database da creare, ad esempio `POSTGRES_MULTIPLE_DATABASES=db1,db2,db3`.
Questo avviene per mezzo dello script `/postgresql_multiple_database/dbs_creator.sh` ma **solo se la cartella `/data` non contiene già un database**.

Fonte: [https://github.com/mrts/docker-postgresql-multiple-databases](https://github.com/mrts/docker-postgresql-multiple-databases)



---
Per maggiori specifiche visitare il repository ufficiale:
[https://hub.docker.com/_/postgres/](https://hub.docker.com/_/postgres/)
