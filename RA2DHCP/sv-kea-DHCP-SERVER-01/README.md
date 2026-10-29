# sv-kea-DHCP-SERVER-01 servei DHCP

## Continguts

- [sv-kea-DHCP-SERVER-01 servei DHCP](#sv-kea-dhcp-server-01-servei-dhcp)
  - [Continguts](#continguts)
  - [Introducció](#introducció)
  - [Capacitat i característiques físiques del maquinari](#capacitat-i-característiques-físiques-del-maquinari)
  - [Definició de requisits](#definició-de-requisits)
  - [Guia d’instal·lació](#guia-dinstallació)
    - [Comprovació de sintaxi (Recomanat)](#comprovació-de-sintaxi-recomanat)
  - [Guia de configuració del servei DHCP per a la subxarxa de *Gestió d'infraestructura*. Passa 2](#guia-de-configuració-del-servei-dhcp-per-a-la-subxarxa-de-gestió-dinfraestructura-passa-2)
  - [Manteniments necessaris](#manteniments-necessaris)
  - [Manuals d’utilització del sistema](#manuals-dutilització-del-sistema)
  - [Sistema de còpies de seguretat](#sistema-de-còpies-de-seguretat)
  - [Responsables](#responsables)
  - [Referències](#referències)
  - [Tasques](#tasques)

## Introducció

Aquest contenidor es desplegarà a partir d'una plantilla d'ubuntu 26.04 al qual s'instal·laran els serveis de DHCP utilitzant el paquet de isc-kea.

El contenidor lxc *sv-kea-DHCP-SERVER-01* s'implementarà a la subxarxa de *Gestió d'infraestructura* 10.18.159.64/27 en concret se li assignaran els següents paràmetres:

Adreça IP: 10.18.159.66/27
Gateway: 10.18.159.65
DNS: 8.8.8.8, 8.8.4.4

Al contenidor lxc *sv-kea-DHCP-SERVER-01* hi haurà la configuració del servei DHCP per a les tres subxarxes *Gestió d'infraestructura*, *Quiròfans* i *Ambulatori*. 

En concret la subxarxa *Gestió d'infraestructura* serà gestionada directament a través de la interfície de xarxa que tindrà connectada directament. En canvi les subxarxes *Quiròfans* i *Ambulatori* les assignacions de configuració de xarxa les faran a través d'un agent DHCP relé.

## Capacitat i característiques físiques del maquinari

Capacitat

## Definició de requisits

Definició

## Guia d’instal·lació

Primer de tot s'ha d'actualitzar el sistema i instal·lar els paquets **isc-kea** necessaris:
```
apt update
apt upgrade
apt install kea
```

### Comprovació de sintaxi (Recomanat)
Executa la següent comanda al terminal indicant la ruta del fitxer de configuració:

```
sudo kea-dhcp4 -t /etc/kea/kea-dhcp4.conf
```

Si la configuració és correcta, la comanda finalitzarà sense mostrar cap error greu (o indicarà que la configuració s'ha validat amb èxit).

Si hi ha un error de sintaxi (per exemple, una coma o un clau JSON que falta), el terminal mostrarà la línia exacta i el motiu de la fallada.
Dels serveis i paquets que s'han instal·lat amb **isc-kea** només utilitzarem els que s'encarreguen del servei DHCP IPv4.

Al final podem comprovar i reiniciar el servei amb les següents instruccions:
```
# Reiniciar el servei DHCP IPv4
systemctl restart kea-dhcp4-server
# Comprovar l'estat del servei DHCP IPv4
systemctl status kea-dhcp4-server
# Veure els logs del servei
journalctl -xeu kea-dhcp4-server
# Veure els logs del servei de forma interactiva
journalctl -xfu isc-dhcp-server 
```

Una vegada comprovat que la instal·lació és correcte es pot procedir a fer la configuració per cada una de les subxarxes

## Guia de configuració del servei DHCP per a la subxarxa de *Gestió d'infraestructura*. Passa 2

A l'hora de configurar l'assignació de paràmetres de xarxa de forma automàtica per la subxarxa de *Gestió d'infraestructura* s'han de tenir en compte els següents paràmetres:
* *adreça xarxa*: 10.18.159.64/28
* *Porta d'enllaç*: 10.18.159.65
* *Adreça difusió*: 10.18.159.95
* *Rang d'adreces IPs fixes*: 10.18.159.65 - 10.18.159.69 (Temps de vida 15 dies)
* *Rang d'adreces IPs automàtiques*: 10.18.159.70 - 10.18.159.79 (Temps de vida infinit)
* *Rang d'adreces IPs dinàmiques*: 10.18.159.80 - 10.18.159.93 (Temps de vida 5 hores)
* *Adreces IPs servidors DNS*: 8.8.8.8, 8.8.4.4

A partir d'aquests paràmetres hem de configurar el servei DHCP.

Conceptes a tenir en compte respecte els paràmetres de configuració del servei DHCP.

1. **Dynamic Allocation (`10.18.159.80 - 10.18.159.93`):** Els equips que rebin una IP d'aquest rang obtindran un *lease* temporal de 5 hores (18000 s). Hauran de demanar la renovació del temps quan passi la meitat del període. 
2. **Automatic Allocation (`10.18.159.70 - 10.18.159.79`):** Els equips que rebin una IP d'aquest rang tindran un `valid-lifetime` establert a `4294967295` (Valor hexadecimal 0xFFFFFFFF`0` infinit). El servidor registrarà l'adreça com a permanent per a aquest client i no caducarà mai. Els equips als quals s'els assignarà una adreça IP d'aquest rang seran equips que necessitin una IP fixa i que tinguin unes característiques que el propi servei DHCP pugui comprovar, com per exemple punt d'acces wifi d'una marca concreta, sensors o aprells d'IoT que estiguin conectats a la xarxa.
3. **Manual Allocation (`10.18.159.65 - 10.18.159.69`):** Per assignar una IP a un equip d'aquest ran s'haurà d'assignar a través de la seva adreça MAC. Aquesta serà assigna explícitament per l'administrador.

Per aquest motiu al fitxer kea-dhcp4.conf s'ha definit tres categories a `client-classes`: *EQUIPS-AUTOMATICS*, *EQUIPS-DINAMICS* i *EQUIPS-RESERVES*

```
        "client-classes": [
            {
                // Classifiquem com a "EQUIPS-AUTOMATICS" els que els dos primer bytes de la mac siguin igual bc:25
                "name": "EQUIPS-AUTOMATICS",
                "test": "substring(pkt4.mac, 0, 2) == 0xbc25",
                "valid-lifetime": 4294967295  // Temps infinit (Automatic Allocation)
            },        
            {
                "name": "EQUIPS-DINAMICS",
                "test":  "not (substring(pkt4.mac, 0, 2) == 0xbc25)"
            },
            {
                "name": "EQUIPS-RESERVES",
                "valid-lifetime": 2592000
            }
        ]
```

En aquest cas concret la selecció funciona d'aquesta forma:

1. **Si connecta un dispositiu amb la MAC `BC:125:22:33:44:55`:**
* El servidor li assigna la classe `EQUIPS-AUTOMATICS`.
* Rep una assignació **automàtica** amb `valid-lifetime: 4294967295` (permanent i sense caducitat).

2. **Si connecta un dispositiu qualsevol (p. ex. un telèfon o un portàtil de visita):**
* No compleix la regla `EQUIPS-AUTOMATICS`.
* Rep una assignació **dinàmica** amb la caducitat definida per defecte en el nostre cas és de 5 hores.

3. **EQUIPS-RESERVES**
* En el cas que el dispositiu pertanyi a `EQUIPS-RESERVES` se li assignarà una caducitat de 15 dies `valid-lifetime": 2592000`

A part de l'adreça MAC (`pkt4.mac`), el servei **Kea DHCP** permet avaluar la MAC del client per poder distingir el tipus de MAC.

Una vegada estan classificats els dispostius de xarxa al servei DHCP es pot definir per una part l'assignació de les adreces IPs:
```
            "pools": [
                {
                    // POOL 1: DYNAMIC ALLOCATION (Per a tothom que NO sigui de la classe de servidors/corporatius)
                    "pool": "10.18.159.80-10.18.159.93",
                    "client-classes": [
                        "EQUIPS-DINAMICS"
                    ]
                },
                {
                    // POOL 2: AUTOMATIC ALLOCATION (Només per a membres de la classe "EQUIPS-AUTOMATICS")
                    "pool": "10.18.159.70-10.18.159.79",
                    "client-classes": [
                        "EQUIPS-AUTOMATICS"
                    ]   
                }
            ],
            "reservations": [
                    {
                        "hw-address": "00:11:22:33:44:55",
                        "ip-address": "10.18.159.67",
                        "hostname": "servidor-fitxers",
                        "client-classes": [
                            "EQUIPS-RESERVES"
                        ]
                    }
            ]
```
En el cas del conjunt d'adreces assignades dinàmicament o automàticament s'han definit dos pools de dades:
1. **Dynamic Allocation (`10.18.159.80 - 10.18.159.93`)** ens asseguram que els dispositius del pool pertanyen a la classe de clients `EQUIPS-DINAMICS` i se li assignen els paràmetres d'aquesta classe.
n. **Automatic Allocation (`10.18.159.70 - 10.18.159.79`)** ens asseguram que els dispositius del pool pertanyen a la classe de clients `EQUIPS-AUTOMATICS` i se li assignen els paràmetres d'aquesta classe.
3. **Manual Allocation (`10.18.159.65 - 10.18.159.69`):** en aquest cas la gestió la fa el mateix administrador això vol dir, que l'adminstrador s'ha d'encarregar d'associar una MAC a una adreça IP, amb aquesta relació s'associen els clients a `EQUIPS-RESERVES` d'aquesta forma se li assignen els paràmetres d'aquesta classe.

Ara només queda assignar els paràmetres convecionals de xarxa d'aquest forma:
```
            "option-data": [{
                    "name": "routers",
                    "data": "10.18.159.65"
                },
                {
                    "name": "domain-name-servers",
                    "data": "8.8.8.8, 8.8.4.4"
                },
                {
                    "name": "domain-name",
                    "data": "hospitalbinissalem.com"
                }
            ],
```

Es pot veure com queda finalment fitxer de configuració [kea-dhcp4.conf](etc/kea/kea-dhcp4.conf)

## Manteniments necessaris

Manteniment

## Manuals d’utilització del sistema

Manuals

## Sistema de còpies de seguretat

Copies seguretat

## Responsables

Responsables

## Referències

Referències

## Tasques

En aquest apartat s’anotarà una breu descripció de la tasca feta, qui la feta i el temps que s’ha invertit en dur-la terme.

| Tasca | Descripció | Qui l’ha feta | Data | Duració |
|---|---|---|---|---|
| | | | | |
| | | | | |
| | | | | |
| | | | | |