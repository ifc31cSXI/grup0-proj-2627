# Documentacio router mikrotik grup0

# Continguts

- [Documentacio router mikrotik grup0](#documentacio-router-mikrotik-grup0)
- [Continguts](#continguts)
  - [Introducció](#introducció)
  - [Configuració de les interfícies virtuals al proxmox](#configuració-de-les-interfícies-virtuals-al-proxmox)
  - [Configuració de les IPs](#configuració-de-les-ips)
  - [Configuració enrutament](#configuració-enrutament)
  - [Configuració NAT](#configuració-nat)
    - [OPCIO 1](#opcio-1)
      - [Definir les taules d’enrutament](#definir-les-taules-denrutament)
      - [Marca els paquets segons la xarxa](#marca-els-paquets-segons-la-xarxa)
      - [Configurar el NAT](#configurar-el-nat)
      - [Configurar taula d’encaminament](#configurar-taula-dencaminament)
    - [OPCIO 2](#opcio-2)
      - [Definir les taules d’enrutament](#definir-les-taules-denrutament-1)
      - [Marca els paquets segons la xarxa](#marca-els-paquets-segons-la-xarxa-1)
      - [Configurar el NAT](#configurar-el-nat-1)
      - [Configurar taula d’encaminament](#configurar-taula-dencaminament-1)
  - [References](#references)


## Introducció

Aquest és la configuració per al mikrotik virtualitzat que servirà per donar accés a les subxarxes de l’empresa.

mikrotik 7.16

usuari/contrasenya: admin/12345678

adreça IP: 192.168.1

## Configuració de les interfícies virtuals al proxmox

<img src="media/image3.png" style="width:5.07813in;height:2.46337in" />

## Configuració de les IPs
```
/ip address
add address=192.168.158.226/27 interface=ether1 network=192.168.158.224
add address=192.168.158.227/27 interface=ether2 network=192.168.158.224
add address=10.18.159.97/29 interface=ether3 network=10.18.159.96
add address=10.18.159.81/28 interface=ether4 network=10.18.159.80
add address=10.18.158.129/26 interface=ether5 network=10.18.158.128
add address=10.18.158.1/26 interface=ether6 network=10.18.158.0
add address=10.18.158.65/26 interface=ether7 network=10.18.158.64
add address=10.18.159.33/28 interface=ether8 network=10.18.159.32
add address=10.18.158.193/27 interface=ether9 network=10.18.158.192
add address=10.18.159.1/27 interface=ether10 network=10.18.159.0
add address=10.18.159.49/28 interface=ether11 network=10.18.159.48
```


## Configuració enrutament

```
add gateway=192.168.158.225
```

## Configuració NAT

Aquesta taula reflexa la configuració del NAT, d’aquesta forma només la Xarxa Administració surt per la interfície ether2 amb la IP 192.168.158.227.

| **Subxarxa**          | **Adreça de xarxa** | **interficie/NAT** | **Adreça IP/NAT** |
|-----------------------|---------------------|--------------------|-------------------|
| Gerencia              | 10.18.159.96/29     | ether1             | 192.168.158.226   |
| RRHH i administració  | 10.18.159.80/28     | ether1             | 192.168.158.226   |
| Urgències             | 10.18.158.128/26    | ether1             | 192.168.158.226   |
| Ambulatori            | 10.18.158.0/26      | ether1             | 192.168.158.226   |
| Quiròfans             | 10.18.158.64/26     | ether1             | 192.168.158.226   |
| Laboratori clínic     | 10.18.159.32/28     | ether1             | 192.168.158.226   |
| RX i ecografies       | 10.18.158.192/27    | ether1             | 192.168.158.226   |
| Intranet              | 10.18.159.0/27      | ether1             | 192.168.158.226   |
| Xarxa d’administració | 10.18.159.48/28     | ether2             | 192.168.158.227   |

Perquè l’encaminador faci el NAT així com s’ha definit, en primer lloc s’ha de definir per quines interfícies de xarxa ha de sortir cada subxarxa. Per això primer s’han d'identificar els paquets segons la xarxa d’origen, una vegada fet això s’han de configurar quina serà la taula d’encaminament per cada subxarxa i finalment crear una taula d’encaminament que indiqui quina serà la interfície de sortida per cada sub

### OPCIO 1

#### Definir les taules d’enrutament

```
/routing table
add disabled=no fib name=rNATether1
add disabled=no fib name=rNATether2
```

#### Marca els paquets segons la xarxa

Amb el mangle es marquen les conexions segons la xarxa per desprès indicar quina serà la taula d’encaminament segons la subxarxa

```
/ip firewall mangle
add action=mark-connection chain=prerouting connection-mark=no-mark \
dst-address=!10.18.0.0/16 new-connection-mark=XAdministracio passthrough=\
yes src-address=10.18.159.48/28
add action=mark-routing chain=prerouting connection-mark=XAdministracio \
new-routing-mark=rNATether2 passthrough=yes
add action=mark-connection chain=prerouting connection-mark=no-mark \
dst-address=!10.18.0.0/16 new-connection-mark=XGerencia passthrough=\
yes src-address=10.18.159.96/29
add action=mark-routing chain=prerouting connection-mark=XGerencia \
new-routing-mark=rNATether1 passthrough=yes
add action=mark-connection chain=prerouting connection-mark=no-mark \
dst-address=!10.18.0.0/16 new-connection-mark=XRRHH passthrough=\
yes src-address=10.18.159.80/28
add action=mark-routing chain=prerouting connection-mark=XRRHH \
new-routing-mark=rNATether1 passthrough=yes
add action=mark-connection chain=prerouting connection-mark=no-mark \
dst-address=!10.18.0.0/16 new-connection-mark=XUrgencies passthrough=\
yes src-address=10.18.158.128/26
add action=mark-routing chain=prerouting connection-mark=XUrgencies \
new-routing-mark=rNATether1 passthrough=yes
add action=mark-connection chain=prerouting connection-mark=no-mark \
dst-address=!10.18.0.0/16 new-connection-mark=XAmbulatori passthrough=\
yes src-address=10.18.158.0/26
add action=mark-routing chain=prerouting connection-mark=XAmbulatori \
new-routing-mark=rNATether1 passthrough=yes</p></td></tr></tbody></table>
```

#### Configurar el NAT

```
/ip firewall nat
add action=masquerade chain=srcnat out-interface=ether1
add action=masquerade chain=srcnat out-interface=ether2
```

#### Configurar taula d’encaminament

```
/ip route
add disabled=no distance=1 dst-address=0.0.0.0/0 gateway=\
192.168.158.225%ether1 pref-src="" routing-table=rNATether1 scope=30 \
suppress-hw-offload=no target-scope=10
add disabled=no distance=1 dst-address=0.0.0.0/0 gateway=\
192.168.158.225%ether2 pref-src="" routing-table=rNATether2 scope=30 \
suppress-hw-offload=no target-scope=10</p></td></tr></tbody></table>
``` 

### OPCIO 2

#### Definir les taules d’enrutament

```
/routing table
add disabled=no fib name=rXAdministracio
```

#### Marca els paquets segons la xarxa

Amb el mangle es marquen les conexions segons la xarxa per després indicar quina serà la taula d’encaminament segons la subxarxa

```
/ip/firewall/mangle
add action=mark-connection chain=prerouting connection-mark=no-mark \
dst-address=!10.18.0.0/16 new-connection-mark=XAdministracio passthrough=\
yes src-address=/28
add action=mark-routing chain=prerouting connection-mark=XAdministracio \
new-routing-mark=rXAdministracio passthrough=yes
```

#### Configurar el NAT

```
/ip firewall nat
add action=masquerade chain=srcnat out-interface=ether1
add action=masquerade chain=srcnat out-interface=ether2
```

#### Configurar taula d’encaminament

```
/ip route
add disabled=no distance=1 dst-address=0.0.0.0/0 gateway=\
192.168.158.225%ether1
pref-src="" routing-table=main scope=30 \
suppress-hw-offload=no target-scope=10
add disabled=no distance=1 dst-address=0.0.0.0/0 gateway=\
192.168.158.225%ether2 pref-src="" routing-table=rXAdministracio scope=30 \
suppress-hw-offload=no target-scope=10
add disabled=no dst-address=10.18.159.48/28 gateway=ether11 routing-table=\
rXAdministracio scope=10 suppress-hw-offload=no
```

## References

Firewall Marking

[<u>https://help.mikrotik.com/docs/display/ROS/Firewall+Marking</u>](https://help.mikrotik.com/docs/display/ROS/Firewall+Marking)
