# RSS BRIDGE

RSS-Bridge è un progetto PHP in grado di generare feed RSS e Atom per i siti web che ne sono sprovvisti. Può essere utilizzato su server web o come applicazione autonoma in modalità CLI.

Importante: RSS-Bridge non è un lettore di feed o un aggregatore di feed, ma uno strumento per generare feed che vengono consumati dai lettori di feed e dagli aggregatori di feed.


#### FORMATI OUTPUT

RSS-Bridge è in grado di produrre diversi formati di output:

- Atom : feed Atom, da utilizzare nei lettori di feed
- Html: pagina HTML semplice
- Json: JSON, per il consumo da parte di altre applicazioni
- Mrss : feed MRSS, da utilizzare nei lettori di feed
- Plaintext: testo non elaborato, utilizzabile da altre applicazioni

È possibile estendere RSS-Bridge con il formato desiderato, utilizzando l'API Format.

---
Per maggiori specifiche visitare il repository ufficiale:
[https://github.com/RSS-Bridge/rss-bridge](https://github.com/RSS-Bridge/rss-bridge)