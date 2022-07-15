# AUTHELIA
**Authelia** è un server di autenticazione e autorizzazione open source che fornisce autenticazione a due fattori e Single Sign-On (SSO) per le applicazioni tramite un portale web. Agisce in concerto a reverse-proxy come nginx, Traefik o HAProxy per far sapere loro se le richieste devono essere consentite o reindirizzate al portale di Authelia per l'autenticazione. Authelia supporto l'utilizzo di **YubiKey**.

### CONFIGURAZIONE SERVER
Il container si aspetta di trovare, dentro la cartella `config`, i files `configuration.yml` e `users_database.yml` altrimenti li andrà a generare in autonomia. Rinominare `configuration.example.yml` e `users_database.example.yml` per utilizzare i flies d'esempio.
- `configuration.yml` contiene tutti i paremetri configurazione e saranno da configurare i seguenti dati dati minimi:
    - **7 Configurazione TOTP**
    - **11 Access Control Configuration**
    - **12 Session Provider Configuration**
    - **14 Storage Provider Configuration**
    - **15 Notification Provider**
- `users_database.yml` contiene i dati di autenticazione degli utenti

### CONFIGURAZIONE NGINX PROXY MANAGER
I parametri di configurazione vannop incollati allinterno dell'host in *Nginx Proxy Manager* `Advanced --> Custom Nginx Configuration`, per l'host di Authelia utilizzare il contenuto di `templates/autheli.example.conf` mentre per tutti gli host da proteggere il contenuto di `templates/protect_domain.example.conf` avendo cura di modificare i parametri come da indicazioni presenti all'interno del file.

### HASHING CRITTOGRAFICO
È un processo unidirezionale per proteggere la password in testo in chiaro creando una stringa di bit di dimensione fissa, chiamata hash, utilizzando la funzione di hash crittografica.
Per la password dell'utente, da inserire in *users_database.yml*, può essere generato tramite [argon2.online](https://argon2.online/) in questo modo:
- Plain Text Input:     inserire la propria password
- Salt:                 cliccare N volte sull'icona a forma di ingraggio
- Parallelism Factor:   1
- Memory Cost:          16
- Iterations:           2
- Hash Length:          16
- Argon:                Argon2id
Il parametro da utilizzare, dopo aver cliccato su **GENERATE HASH** è **Output in Encoded Form**, sarà simile a 
```yml
$argon2id$v=19$m=16,t=2,p=1$dGlqV0dqUXJwT3BrWjk0dw$KO2hUPXGc5gZqyZZacQ+lw
```

Maggiori [info](https://www.authelia.com/docs/configuration/authentication/file.html#passwords)

---
### LOGO E FAVICON
Per personalizzare, nella schermata di login, il logo e la favicon caricare all'interno della cartella `/conf/config/asset/` i file `logo.png` e `favicon.ico`.


### FONTI
[Smarthomebeginner](https://www.smarthomebeginner.com/docker-authelia-tutorial/)

---
Per maggiori specifiche visitare il repository ufficiale:
[https://github.com/authelia/authelia](https://github.com/authelia/authelia)