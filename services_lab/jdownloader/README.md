# JDOWNLOADER
**Jdownloader** è un potente gestore di download

### ACCESSO ALLA WEBGUI
Il container di JDownloader è settato per incanalare il flusso di dati tramite la VPN messa a disposizione dal contaner di `NORDVPN` o `PROTONVPN`. Per questo motivo, dopo aver aperto la porta di JDownloader, la `5800`, sul container di NORDVPN e PROTONVPN, la WebGui sarà accessibile su `https://nordvpn:5800` oppure `https://protonvpn:5800` (non contemporaneamente, in questa versione è impostato per l'uso di ProtonVPN).

### CHECK IP
Per verificare l'ip con cui si scaricano i files, scaricare `http://ipcheck0.jdownloader.org/`.
Verrà scaricato un file che conterrà l'ip.

---
Per maggiori specifiche visitare il repository ufficiale:
[https://github.com/jlesage/docker-jdownloader-2](https://github.com/jlesage/docker-jdownloader-2)