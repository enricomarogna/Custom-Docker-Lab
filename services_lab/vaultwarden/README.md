# VAULTWARDEN
**Vaultwarden** è la versione docker server, scritta in Rust, del popolare password manager [Bitwarden](https://bitwarden.com/).

---
### VARIABILI
Qui un [elenco di tutte le variabili](https://github.com/dani-garcia/vaultwarden/blob/main/.env.template) utilizzabile per la configurazione del container.

---
### DATABASE
La configurazione standard di Vaultwarden utilizza SQLite mentre questa configurazione prevede l'utilizzo di MySQL/MariaDB.
Creare preventivamente il database con le caratteristiche e [modalità indicate nella documentazione](https://github.com/dani-garcia/vaultwarden/wiki/Using-the-MariaDB-%28MySQL%29-Backend).

---
### YUBIKEY OTP
Per abilitare l'autenticazione mediante YUBIKEY seguire i passi operativi presenti sulla [docs ufficiale](https://github.com/dani-garcia/vaultwarden/wiki/Enabling-Yubikey-OTP-authentication).
Passare ID e SECRET_KEY tramite secret.

**Approfondimenti**: [link](https://github.com/dani-garcia/vaultwarden/wiki/Enabling-Yubikey-OTP-authentication)

---
### SOTTODIRECTORY
L'istanza viene "nascosta" di default, per aggiungere un ulteriore [livello di sicurezza](https://github.com/dani-garcia/vaultwarden/wiki/Hardening-Guide#hiding-under-a-subdir), sotto la directory **/vaultwarden**, quindi ad esempio https://vaultwarden.dominio.com/vaultwarden/. Il parametro è customizzabile, anche aggiungendo ulteriori sottodirectory, ad esempio **/first_path/second_path**.

È neccesario aggiungere il percorso nella configurazione di NGINX Proxy Manager:

da:
```nginx
location / {
    set $upstream_ ...
}
```
a: 
```nginx
location /vaultwarden/ {
    set $upstream_ ...
}
```

È **fondamentale** includere lo trailing slash finale `/`

**Approfondimenti**: [link](https://github.com/dani-garcia/vaultwarden/wiki/Using-an-alternate-base-dir)

---
### SMTP
Si consiglia la configurazione dei parametri SMTP per permettere autenticazione a due fattori, inviti e reset, in caso contrario commentare le variabili **SMTP_** nel file `docker-compose.yml`.

**Approfondimenti**: [link](https://github.com/dani-garcia/vaultwarden/wiki/SMTP-Configuration)

---
### SICUREZZA
Qui un [elenco delle misure di sicurezza](https://github.com/dani-garcia/vaultwarden/wiki/Hardening-Guide) applicabili consigliate.

---
### SECRETS
È necessario creare i seguenti secrets (oppure sostituirli con le variabili):
- `VAULTWARDEN_YUBICO_CLIENT_ID`
- `VAULTWARDEN_YUBICO_SECRET_KEY`
- `VAULTWARDEN_SMTP_PASSWORD`
- `VAULTWARDEN_ADMIN_TOKEN`

---
### PORTA
La porta di accesso al container è quella configurata nella variabile `ROCKET_PORT` (default `8080`).


---
Per maggiori specifiche visitare il repository ufficiale:
[https://github.com/dani-garcia/vaultwarden](https://github.com/dani-garcia/vaultwarden)