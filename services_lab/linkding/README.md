# LINKDING

**Linkding** è un semplice servizio per gestire i segnalibri. È progettato per essere minimale, veloce e facile da configurare.

### PANORAMICA DELLE FUNZIONALITÀ:

- Tag per l'organizzazione dei segnalibri
- Ricerca per testo o tag
- Modifica in blocco
- Archivio segnalibri
- Modalità scura
- Crea automaticamente istantanee di siti Web con segnalibri nell'archivio Web
- Fornisce automaticamente titoli e descrizioni di siti Web con segnalibri
- Importa ed esporta segnalibri in formato HTML Netscape
- Estensioni per Firefox e Chrome e un bookmarklet che dovrebbe funzionare nella maggior parte dei browser
- API REST per lo sviluppo di app di terze parti
- Pannello di amministrazione per il self-service dell'utente e l'accesso ai dati grezzi
- Facile da configurare tramite Docker, **utilizza SQLite come database**

Demo: https://demo.linkding.link/ (configurato con registrazione aperta)

### CONFIGURAZIONE UTENTE
È necessario creare un utente in modo da poter accedere all'applicazione. **Sostituire le credenziali** nel comando seguente prima di eseguirlo:

```bash
docker-compose exec linkding python manage.py createsuperuser --username=mario --email=mario@dominio.com
```

Il comando chiederà una password sicura. Una volta completato il processo, è possibile utilizzare l'applicazione accedendo all'interfaccia utente con le credenziali create. 

---
Per maggiori specifiche visitare il repository ufficiale:
[https://github.com/sissbruecker/linkding](https://github.com/sissbruecker/linkding)