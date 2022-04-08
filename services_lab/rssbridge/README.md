# RSS BRIDGE

RSS-Bridge è un progetto PHP in grado di generare feed RSS e Atom per i siti web che ne sono sprovvisti. Può essere utilizzato su server web o come applicazione autonoma in modalità CLI.

Importante: RSS-Bridge non è un lettore di feed o un aggregatore di feed, ma uno strumento per generare feed che vengono consumati dai lettori di feed e dagli aggregatori di feed.


### FORMATI OUTPUT

RSS-Bridge è in grado di produrre diversi formati di output:

- Atom : feed Atom, da utilizzare nei lettori di feed
- Html: pagina HTML semplice
- Json: JSON, per il consumo da parte di altre applicazioni
- Mrss : feed MRSS, da utilizzare nei lettori di feed
- Plaintext: testo non elaborato, utilizzabile da altre applicazioni

È possibile estendere RSS-Bridge con il formato desiderato, utilizzando l'API Format.

### WHITELIST

Rinominare il file `whitelist.example.txt` in `whitelist.txt` e inserire la lista dei servizi desiderati.
Ad esempio:

```bash
FacebookBridge
WikipediaBridge
TwitterBridge
```

oppure

```bash
Facebook
Wikipedia
Twitter
```

### CONFIGURAZIONE PERSONALIZZATA

Rinominando `config.ini.example.php` in `config.ini.php` è possibile personalizzare alcuni parametri:

- **System**: questa sezione specifica i parametri specifici del sistema
- **Cache**: questa sezione riguarda il comportamento di memorizzazione nella cache di RSS-Bridge
- **Proxy**: questa sezione può essere utilizzata per specificare un server proxy per RSS-Bridge da utilizzare per il recupero dei contenuti
- **Authentication**: questa sezione definisce i parametri per richiedere l'autenticazione per utilizzare RSS-Bridge
- **Admin**: questa sezione specifica i parametri relativi all'amministratore della istanza di RSS-Bridge

Si rimanda alla [documentazione ufficiale](https://github.com/RSS-Bridge/rss-bridge/wiki/Custom-Configuration) maggiori specifiche e dettagli.


---
Per maggiori specifiche visitare il repository ufficiale:
[https://github.com/RSS-Bridge/rss-bridge](https://github.com/RSS-Bridge/rss-bridge)