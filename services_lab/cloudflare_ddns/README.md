# CLOUDFLARE DDNS
**Cloudflare DDNS** permette di utilizzare i servizi DNS di Cloudflare come un servizio DDNS.

### CREAZIONE DEL TOKEN API 
Al link [https://dash.cloudflare.com/profile/api-tokens](https://dash.cloudflare.com/profile/api-tokens)  e seguire i seguenti passi:
1. Cliccare su `Crea Token` > `Crea token personalizzato`
2. Assegnare un nome al token, ad esempio `Cloudflare-ddns`
3. Assegnare al token i seguenti permessi:
    - Zona > Impostazioni zone > Leggi
    - Zona > Zona > Leggi
    - Zona > DNS > Modifica
4. Impostare `Risorse zona`:
    - Includi > Tutte le zone
5. Comfermare la creazione e assegnare il token alla variabile del container o meglio salvarlo come secret.

---
Per maggiori specifiche visitare il repository ufficiale:
[https://github.com/oznu/docker-cloudflare-ddns](https://github.com/oznu/docker-cloudflare-ddns)