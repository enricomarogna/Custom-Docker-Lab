# WALLABAG

[Wallabag](https://www.wallabag.it/) è un gestore di bookmarks e preferiti simile a **Pocket*, ma gratuito, open-source e seflhostable. Cattura i testi delle pagine web in modo da poterle ri-leggere in un secondo momento, è possibile inserire appunti e archiviare articoli una volta terminata la lettura.
Wallabag ha [una serie di app](https://www.wallabag.it/en/applications) per smartphone e browser ed è dotata di API, oltre ad essere supportato da molti aggregatori di feed o lettori RSS.

## SPECIFICHE

#### UTENZE
Utenze di default:
- username: `wallabag`
- password: `wallabag`

#### VARIABILI D'AMBIENTE
Questo container utilizza SQLite come database, è possibile utilizzare MySQL soltanto nel caso venga creata una istanza dedicata di Mariadb, vedi ([#9](https://github.com/wallabag/docker/issues/9#issue-156828385)) e ([#121](https://github.com/wallabag/docker/issues/121#issue-356721722)).



---
Per maggiori specifiche visitare il repository ufficiale:
[https://github.com/wallabag/docker](https://github.com/wallabag/docker)