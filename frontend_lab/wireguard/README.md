# WIREGUARD
WireGuard® è una VPN estremamente semplice ma veloce e moderna che utilizza una crittografia all'avanguardia. Mira a essere più veloce, più semplice, più snello, più utile di IPsec e notevolmente più performante di OpenVPN. WireGuard è progettato come una VPN generica per l'esecuzione su interfacce integrate e super computer, adatta a molte circostanze diverse.

## CLOUDFLARE
CNAME con proxy **disattivato**.

## ROUTER
Aprire la porta UDP (standard `51820`) del router sull'ip `192.xxx.x.254` assegnato a `NGINX Proxy Manager`.

## NGINX PROXY MANAGER
Impostare uno `stream`:
- Incoming Port: `51820`
- Forward Host: `192.xxx.x.224` (Ip assegnato a Wireguard)
- Forward Port: `51820`
- TCP Forwarding: `disabilitato`
- UDP Forwarding: `abilitato`



---
Per maggiori specifiche visitare il repository ufficiale:
[https://github.com/linuxserver/docker-wireguard](https://github.com/linuxserver/docker-wireguard)