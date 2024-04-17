# Auditoria amb el suite d'eines d'aircrack.
[Tutorial: How to crack WPA/WPA2](https://www.aircrack-ng.org/doku.php?id=cracking_wpa)

Anem a fer proves amb l'eina aircrack contra wifis WPA/WPA2 PSK. Bàsicament anem a aplicar tècniques de força bruta per endevinar la PSK d'una wifi d'aquest tipus. Per tant, és important en termes de seguretat que el PSK tingui força longitud i que no sigui una paraula de diccionari. En aquest procés és bàsic que poguem recollir els paquets que intervenen entre un client i un punt d'accés quan el client s'intenta afegir a la xarxa wifi.

La prova l'anem a fer amb un PSK curt o de diccionari (i que la paraula hi sigui en el diccionari).

Tal com diu l'article que he enllaçat abans, necessitem els següent:

* Necessitem que la targeta wifi on anem a fer l'atac és capaç d'injectar paquets.

Primer de tot vaig a veure si hi ha la suite d'eines d'`aircrack-ng` en els repositoris de Debian:

```bash
[10:57:53] juan ~ $ apt search aircrack-ng | head

WARNING: apt does not have a stable CLI interface. Use with caution in scripts.

Ordenant…
Buscar en tot el text…
aircrack-ng/focal 1:1.6-4 amd64
  wireless WEP/WPA cracking utilities

airgraph-ng/focal,focal 1:1.6-4 all
  Tool to graph txt files created by aircrack-ng

kismet-plugins/focal 2016.07.R1-1.1~build2 amd64
  wireless sniffer and monitor - plugins
```

La versió que tinc als repositoris és la 1.6.

## Pas 1. Configurar la targeta de xarxa en mode monitor
També hem de configurar la targeta en el canal on sigui el punt d'accés que volem auditar. Configurant la targeta en mode monitor aconseguirem desautenticar clients d'una xarxa wifi i capturar els posteriors paquets *handshake* d'autenticació. 

Configurem la targeta en mode monitor amb la comanda `airmon-ng`:

```bash
[11:28:02] juan ~ $ sudo airmon-ng

PHY     Interface       Driver          Chipset

phy0    wlp18s0         ath9k           Qualcomm Atheros AR9285 Wireless Network Adapter (PCI-Express) (rev 01)
null    wlxc025e915c972 r8188eu         TP-Link TL-WN722N v2
```

Obtinc una llista de les meves targetes de xarxa i llurs drivers. la segona targeta que es veu és una targeta wifi-usb i és la que anava a fer servir per fer les proves, però no he pogut posar-la en mode monitor. L'altra, sí.

## Pas 1.1. Injection test

Aquest test d'injecció té dues parts: en una primera part es fa un enviament de missatges broadcast de sol·licitud de sondeig (si algun punt d'accés respon, és que podem injectar paquets amb la nostra targeta); en una segona part, per cadascun dels punts d'accés llistats, es fa un test de qualitat de la connexió amb l'enviament de 30 paquets de sondeig. 

El test d'injecció em proporcionarà la següent informació:

* Si podem injectar paquets.
* Llistat de punts d'accés que tinc al voltant.
* Em diu la qualitat en quant a la connectivitat que puc tenir amb quests punts d'accés fent una prova d'enviament de 30 paquets a cadascun dels punts d'accés. A partir del % de paquets de resposta, obtinc la puntuació de la qualitat d'aquesta connectivitat.

## Pas 2. Iniciar aerodump-ng per a capturar els paquets handshake de l'autenticació
Important executar la comanda amb el canal que correspongui amb el canal del punt d'accés.

## Pas 3. Fer ús d'aireplay-ng per a desautenticar i provocar la reautenticació d'un client concret
Aquest pas és si el pas 2 no ha funcionat.

## Pas 4. Executar aircrack-ng per craquejar la PSK
todo