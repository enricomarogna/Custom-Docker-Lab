# FLAME

**Flame** è uno *startpage*, ispirato a [SUI](https://github.com/CorneliousJD/SUI-Startpage), facile da configurare e utlizzare. La configurazione degli hub è consentita direttamente dagli editor integrati senza dover modificare alcun file. 


## FUNZIONALITÀ

- 📝 Crea, aggiorna, elimina le applicazioni e i segnalibri direttamente dall'app utilizzando gli editor GUI integrati
- 📌 Aggiungi gli elementi preferiti alla schermata iniziale per un accesso facile e veloce
- 🔍 Barra di ricerca integrata con filtro locale, 11 provider di ricerca web e possibilità di aggiungerne di propri
- 🔑 Sistema di autenticazione per proteggere le impostazioni, app e segnalibri
- 🔨 Decine di opzioni per personalizzare l'interfaccia di Flame in base alle proprie esigenze, incluso il supporto per CSS personalizzati, 15 temi colore integrati e generatore di temi personalizzato
- ☀️ Widget meteo con temperatura attuale, copertura nuvolosa e stato meteo animato
- 🐳 Integrazione Docker per selezionare e aggiungere automaticamente app in base alle loro etichette

---
## CONFIGURAZIONE DEL MODULO METEO

Ottenere la chiave API da  [Weather API](https://www.weatherapi.com/pricing.aspx).

    Il piano gratuito consente 1 milione di chiamate al mese. Flame effettua meno di 3.000 chiamate API al mese.

Ottenere lat/long per la propria posizione tramite [latlong.net](latlong.net).
Inseriti e salvati i dati, il widget meteo si aggiornerà e sarà visibile nella home page. 

---
## INTEGRAZIONE DOCKER

Per utilizzare l'integrazione Docker, ogni container deve avere le seguenti etichette:

```yml
labels:
  - flame.type=application # "app" funziona ugualmente
  - flame.name=Nome del container
  - flame.url=https://dominio.com
  - flame.icon=nome_icona # facoltativo, l'impostazione predefinita è "docker"
```
L'opzione ***Usa API Docker*** in `Impostazioni -> Docker` deve essere abilitata affinché funzioni.

---
## SOCKET PROXY
Abilitare l'utilizzo del *socket_proxy* da `Impostazioni -> Docker` e nel campo `Docker host` inserire `socket_proxy:2375`.

---
Per maggiori specifiche visitare il repository ufficiale:
[https://github.com/pawelmalak/flame](https://github.com/pawelmalak/flame)