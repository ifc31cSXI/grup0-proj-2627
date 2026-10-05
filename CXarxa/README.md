# Adreçament Xarxa Grup 0

## Continguts

- [Adreçament Xarxa Grup 0](#adreçament-xarxa-grup-0)
  - [Continguts](#continguts)
  - [Introducció](#introducció)
  - [Adraces IP de la Xarxa Grup 0](#adraces-ip-de-la-xarxa-grup-0)
    - [Departament/Subxarxes](#departamentsubxarxes)
  - [Assignació adreces IPs grup 0](#assignació-adreces-ips-grup-0)
    - [rv-mk-716-grupX](#rv-mk-716-grupx)
  - [Documents](#documents)

## Introducció

Es detalla l'organització dels departaments de l'empresa Hospital Binissalem els quals ens servirà per definir les diferents subxarxes que definiran l'empresa ...

L'Hospital té el següent organigrama

- Gerència
- RRHH i administració
- Urgències
- Ambulatori
- Quiròfans
- Laboratori clínic
- Farmàcia
- RX i ecografies

## Adraces IP de la Xarxa Grup 0

Aquestes són les adreces IPs que es tindran en compte a l'hora de configurar la xarxa.

Les adreces de la xarxa DMZ son 192.168.158.224/27

Les adreces de la xarxa privada son 10.18.158.0/23

Es important a l’hora d'assignar les subxarxes tenir en compte que les VLANS del grup 0 es troben dins el rang: 3600-3640.

### Departament/Subxarxes

| Departament / Subxarxa | # hosts | Adreça Xarxa       | VLANs | Porta d'enllaç  | Adreça de difusió |
|---|---:|---|---:|---|---|
| Gerencia                |      5 |    10.18.159.96/29 | 3600  |    10.18.159.97 |   10.18.159.103 |
| RRHH i administració    |      7 |   10.18.159.104/29 | 3601  |   10.18.159.105 |   10.18.159.109 |
| Urgències               |     60 |   10.18.158.128/26 | 3602  |   10.18.158.129 |   10.18.158.191 |
| Ambulatori              |     60 |     10.18.158.0/26 | 3603  |     10.18.158.1 |    10.18.158.63 |
| Quiròfans               |     60 |    10.18.158.64/26 | 3604  |    10.18.158.65 |   10.18.158.127 |
| Laboratori clínic       |     20 |    10.18.159.32/27 | 3605  |    10.18.159.33 |    10.18.159.63 |
| RX i ecografies         |     30 |   10.18.158.192/27 | 3606  |   10.18.158.193 |   10.18.158.223 |
| Intranet                |     20 |     10.18.159.0/27 | 3607  |     10.18.159.1 |    10.18.159.31 |
| Xarxa d’administració   |     20 |    10.18.159.64/27 | 3608  |    10.18.159.65 |    10.18.159.95 |
| DMZ                     |     20 | 192.168.158.224/27 | 3609  | 192.168.158.225 | 192.168.158.255 |

## Assignació adreces IPs grup 0 

### rv-mk-716-grupX

Aquestes son les adreces IP del router mikrotik de la xarxa del grup X.

Aquestes son les adreces IP del router mikrotik de la xarxa del grup 0.

| **Interfície** | **IP/mascara**     | **VLANs** | **Subxarxa**          |
|----------------|--------------------|-----------|-----------------------|
| ether1         | 192.168.158.226/27 | 3609      | DMZ                   |
| ether2         | 192.168.158.227/27 | 3609      | DMZ                   |
| ether3         | 10.18.159.97/29    | 3600      | Gerencia              |
| ether4         | 10.18.159.105/28   | 3601      | RRHH i administració  |
| ether5         | 10.18.158.129/26   | 3602      | Urgències             |
| ether6         | 10.18.158.1/26     | 3603      | Ambulatori            |
| ether7         | 10.18.158.65/26    | 3604      | Quiròfans             |
| ether8         | 10.18.159.33/28    | 3605      | Laboratori clínic     |
| ether9         | 10.18.158.193/27   | 3606      | RX i ecografies       |
| ether10        | 10.18.159.1/27     | 3607      | Intranet              |
| ether11        | 10.18.159.65/27    | 3608      | Xarxa d’administració |

## Documents
Relació documents de la configuració de la xarxa del grup 0.
- [Documentació S3MK6.38-0004-Central](Documentació%20S3MK6.38-0004-Central.md)
- [Documentació rv-mk-716-grup0.md](Documentació%20rv-mk-716-grup0.md)
- [Documentació mv-lxc-dmz-grup0.md](Documentació%20mv-lxc-dmz-grup0.md)
- [Documentació mv-lxc-subxarxa-grup0.md](Documentació%20mv-lxc-subxarxa-grup0.md)