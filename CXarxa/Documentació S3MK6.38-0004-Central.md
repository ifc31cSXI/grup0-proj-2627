# Documentació Switch Mikrotik Central

## Continguts

- [Documentació Switch Mikrotik Central](#documentació-switch-mikrotik-central)
  - [Continguts](#continguts)
  - [Introducció](#introducció)
- [Gestió de les VLANS](#gestió-de-les-vlans)
  - [Taula de configuració VLANS pel switch](#taula-de-configuració-vlans-pel-switch)
  - [Definició de les interficies VLANs](#definició-de-les-interficies-vlans)
  - [Definició dels ports](#definició-dels-ports)
- [Gestió d'adreces IPs](#gestió-dadreces-ips)
- [Gestió del servei d'enrutament](#gestió-del-servei-denrutament)


## Introducció

Aquesta es la configuració utilitzada al switch MikroTik CRS326-24-2S+ [<u>https://mikrotik.com/product/crs326\_24g\_2s\_in</u>](https://mikrotik.com/product/crs326_24g_2s_in)

Les funcions d'aquest switch seran:

-   Gestió de les VLANS
-   Gestió del servei d'enrutament.

# Gestió de les VLANS

A l'hora de configurar el switch m'he basat amb els següents exemples. [<u>https://wiki.mikrotik.com/wiki/Manu al:Interface/Bridge\#Bridge\_VLAN\_Filtering</u>](https://wiki.mikrotik.com/wiki/Manual:Interface/Bridge#Bridge_VLAN_Filtering)

La gestió de les VLANS es farà segons el port del switch, ja que d'aquesta forma es simplifica l'administració del switch.

## Taula de configuració VLANS pel switch

| **Departament/Subxarxa** | **Adreça Xarxa**   | **VLANs** | **Ports Switch**                   |
|--------------------------|--------------------|-----------|------------------------------------|
| Gerencia                 | 10.18.159.96/29    | 3596      | 1T, 2T, 3T, 4T, 5T,6T,7T,8T,9T,10T |
| RRHH i administració     | 10.18.159.80/28    | 3580      | 1T, 2T, 3T, 4T, 5T,6T,7T,8T,9T,10T |
| Urgències                | 10.18.158.128/26   | 3528      | 1T, 2T, 3T, 4T, 5T,6T,7T,8T,9T,10T |
| Ambulatori               | 10.18.158.0/26     | 3500      | 1T, 2T, 3T, 4T, 5T,6T,7T,8T,9T,10T |
| Quiròfans                | 10.18.158.64/26    | 3564      | 1T, 2T, 3T, 4T, 5T,6T,7T,8T,9T,10T |
| Laboratori clínic        | 10.18.159.32/28    | 3532      | 1T, 2T, 3T, 4T, 5T,6T,7T,8T,9T,10T |
| RX i ecografies          | 10.18.158.192/27   | 3592      | 1T, 2T, 3T, 4T, 5T,6T,7T,8T,9T,10T |
| Intranet                 | 10.18.159.0/27     | 3501      | 1T, 2T, 3T, 4T, 5T,6T,7T,8T,9T,10T |
| Xarxa d’administració    | 10.18.159.48/28    | 3548      | 1T, 2T, 3T, 4T, 5T,6T,7T,8T,9T,10T |
| DMZ                      | 192.168.158.224/27 | 3499      | 1T, 2T, 3T, 4T, 5T,6T,7T,8T,9T,10T |

## Definició de les interficies VLANs

Aquestes instruccions permeten definir les vlans.

``` 
/interface vlan
add interface=bridge name=v3596GerenciaG0 vlan-id=3596
add interface=bridge name=v3580RRHHG0 vlan-id=3580
add interface=bridge name=v3528UrgenciesG0 vlan-id=3528
add interface=bridge name=v3500AmbulatoriG0 vlan-id=3500
add interface=bridge name=v3564QuirofansG0 vlan-id=3564
add interface=bridge name=v3532LaboratoriG0 vlan-id=3532
add interface=bridge name=v3592RXG0 vlan-id=3592
add interface=bridge name=v3501IntranetG0 vlan-id=3501
add interface=bridge name=v3548AdmG0 vlan-id=3548
add interface=bridge name=v3499DMZG0 vlan-id=3499
```

## Definició dels ports

Es defineix el port que seran TAGGED i UTAGGED i quines VLANs seran etiquetades per cada un dels ports. Aleshores s'indica quines són els ports que seran troncals o TAGGED en concret són: ether1, ehter2, ether3, ether4 …

```
/interface bridge vlan
add bridge=bridge tagged=bridge,ether1,ether2,ether3,ether4,ether5,ether6,ether7,ether9,ether10 vlan-ids=3596
add bridge=bridge tagged=bridge,ether1,ether2,ether3,ether4,ether5,ether6,ether7,ether9,ether10 vlan-ids=3580
add bridge=bridge tagged=bridge,ether1,ether2,ether3,ether4,ether5,ether6,ether7,ether9,ether10 vlan-ids=3528
add bridge=bridge tagged=bridge,ether1,ether2,ether3,ether4,ether5,ether6,ether7,ether9,ether10 vlan-ids=3500
add bridge=bridge tagged=bridge,ether1,ether2,ether3,ether4,ether5,ether6,ether7,ether9,ether10 vlan-ids=3564
add bridge=bridge tagged=bridge,ether1,ether2,ether3,ether4,ether5,ether6,ether7,ether9,ether10 vlan-ids=3532
add bridge=bridge tagged=bridge,ether1,ether2,ether3,ether4,ether5,ether6,ether7,ether9,ether10 vlan-ids=3592
add bridge=bridge tagged=bridge,ether1,ether2,ether3,ether4,ether5,ether6,ether7,ether9,ether10 vlan-ids=3501
add bridge=bridge tagged=bridge,ether1,ether2,ether3,ether4,ether5,ether6,ether7,ether9,ether10 vlan-ids=3548
add bridge=bridge tagged=bridge,ether1,ether2,ether3,ether4,ether5,ether6,ether7,ether9,ether10 vlan-ids=3499
```

# Gestió d'adreces IPs

Per assignar les adreces a les interfícies i a les VLANs [<u>http://wiki.mikrotik.com/wiki/Manual:Interface/VLAN</u>](http://wiki.mikrotik.com/wiki/Manual:Interface/VLAN).

```
/ip address
add address=192.168.158.225/27 interface=ether1 network=192.168.158.224
```
# Gestió del servei d'enrutament

```
/ip route
add distance=1 gateway=192.168.250.1
add distance=1 dst-address=192.168.128.0/24 gateway=192.168.2.28
add distance=1 dst-address=192.168.129.0/24 gateway=192.168.2.29
add distance=1 dst-address=192.168.130.0/24 gateway=192.168.2.30
add distance=1 dst-address=192.168.131.0/24 gateway=192.168.2.31
add distance=1 dst-address=192.168.132.0/24 gateway=192.168.2.32
```

