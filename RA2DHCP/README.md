# Servei DHCP

## Introducció

Les següents subxarxes tindran servei d'adreçament dinàmic amb DHCP pels servidors Linux i mikrotik respectivament, a excepció de la subxarxa de DelegacioRemota, que serà proveïda d'adreçament dinàmic amb un Relay DHCP.


**Xarxa d’administració / Gestió infraestructura:**
* *adreça xarxa*: 10.18.159.64/28
* *VLAN:* 3608
* *Porta d'enllaç*: 10.18.159.65
* *Adreça difusió*: 10.18.159.95
* *Rang d'adreces IPs fixes*: 10.18.159.65 - 10.18.159.69
* *Rang d'adreces IPs automàtiques*: 10.18.159.70 - 10.18.158.79
* *Rang d'adreces IPs dinàmiques*: 10.18.159.80 - 10.18.159.93
* *Servei DHCP*: Servidor Linux amb isc-kea
* *Adreça IP servidor DHCP*: 10.18.159.66
* *Adreces IPs servidors DNS*: 8.8.8.8, 8.8.4.4

**Quiròfans**
* *adreça xarxa*: 10.18.158.0/26
* *VLAN:* 3604
* *Porta d'enllaç*: 10.18.158.1
* *Adreça difusió*: 10.18.158.63
* *Rang d'adreces IPs fixes*: 10.18.158.1 - 10.18.158.19
* *Rang d'adreces IPs automàtiques*: 10.18.158.20 - 10.18.158.39
* *Rang d'adreces IPs dinàmiques*: 10.18.158.40 - 10.18.158.62
* *Servei DHCP*: Servidor Relay
* *Adreça IP servidor DHCP*: 10.18.159.66
* *Adreces IPs servidors DNS*: 8.8.8.8, 8.8.4.4

**Ambulatori (remota)**
* *adreça xarxa*: 10.18.158.64/26
* *VLAN:* 3603
* *Porta d'enllaç*: 10.18.158.65 
* *Adreça difusió*: 10.18.158.127
* *Rang d'adreces IPs fixes*: 10.18.158.65 - 10.18.158.79
* *Rang d'adreces IPs automàtiques*: 10.18.158.80 - 10.18.158.99
* *Rang d'adreces IPs dinàmiques*: 10.18.158.100 - 10.18.158.126
* *Servei DHCP*: Servidor Relay
* *Adreça IP servidor DHCP*: 10.18.159.66
* *Adreces IPs servidors DNS*: 8.8.8.8, 8.8.4.4


Els serveis de DHCP està implementat a partir de contenidors LXC aquests contenidors es desplegaran al clúster de proxmox. 

Els contenidors LXC es desplegaran a partir d'una imatge d'Ubuntu 26.04 i el servei que s’instal·larà seran el de isc-kea.

### Servidors
Els serveis DHCP estarà format per tres dockers:
* [sv-kea-DHCP-SERVER-01](sv-kea-DHCP-SERVER-01/README.md)
* sv-kea-DHCP-RELAY-01
* sv-kea-DHCP-RELAY-02




