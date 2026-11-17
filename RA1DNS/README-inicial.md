# Servei DNS

## Introducció

En aquesta documentació trobarem informació sobre la configuració dels serveis DNS de l’empresa Hospital d’Inca.
Disseny dels serveis DNS.
Els dos noms de domini de l’empresa son:

hospitalbinissalem.com
quirofans.com 
urgencies.com

El servei DNS està gestionat per dos servidors:
* [Server1](Server1/README.md)
* [Server2](Server2/README.md)

## Definició de les zones
### Requeriments zona hospitalbinissalem.com


|Nom de domini | Tipus de registres | Valor |
|-------------|-----------|-------|
|hospitalbinissalem.com     | SOA | Master de domini: admin@hospitalbinissalem.com</br>DNS primari: ns01.hospitalbinissalem.com</br>Número de sèrie 20261117000001
|hospitalbinissalem.com     | NS  | ns01.hospitalbinissalem.com  |
|hospitalbinissalem.com     | NS  | ns02.hospitalbinissalem.com  |
|hospitalbinissalem.com     | MX  | mail1.hospitalbinissalem.com | 
|hospitalbinissalem.com     | MX  | mail2.hospitalbinissalem.com |
|www.hospitalbinissalem.com |CNAME| web.hospitalbinissalem.com   |
|ftp.hospitalbinissalem.com |CNAME| file.hospitalbinissalem.com  |
|estacions.hospitalbinissalem.com     | NS  | ns01.hospitalbinissalem.com  |
|estacions.hospitalbinissalem.com     | NS  | ns02.hospitalbinissalem.com  |
| ns01.hospitalbinissalem.com         | A   | 192.168.158.45               |
| ns02.hospitalbinissalem.com         | A   | 192.168.158.46               |
| mail1.hospitalbinissalem.com        | A   | 192.168.158.47               |
| mail2.hospitalbinissalem.com        | A   | 192.168.158.48               |
| web.hospitalbinissalem.com          | A   | 192.168.158.49               |
| ftp.hospitalbinissalem.com          | A   | 192.168.158.50               |

### Requeriments zona estacions.hospitalbinissalem.com

|Nom de domini|Tipus de registres|Valor|
|-------------|-----------|-------|
|estacions.hospitalbinissalem.com | SOA | Master de domini: admin@hospitalbinissalem.com</br>DNS primari: ns01.hospitalbinissalem.com</br>Número de sèrie 20261117000001 |
|estacions.hospitalbinissalem.com     | NS  | ns01.hospitalbinissalem.com  |
|estacions.hospitalbinissalem.com     | NS  | ns02.hospitalbinissalem.com  |
|est01.estacions.hospitalbinissalem.com     | A  | 10.18.158.100  |
|est02.estacions.hospitalbinissalem.com     | A  | 10.18.158.101  |
|est03.estacions.hospitalbinissalem.com     | A  | 10.18.158.102  |

### Requeriments zona inversa per l’adreça 10.18.158.0/24 

|Nom de domini | Tipus de registres | Valor
|-------------|-----------|-------|
|158.18.10.in-addr.apra | SOA | Master de domini: admin@hospitalbinissalem.com</br>DNS primari: ns01.hospitalbinissalem.com</br>Número de sèrie 20261117000001 |
|158.18.10.in-addr.apra | NS  | ns01.hospitalbinissalem.com  |
|158.18.10.in-addr.apra | NS  | ns02.hospitalbinissalem.com  |
|100.158.18.10.in-addr.apra | PTR | est01.estacions.hospitalbinissalem.com |
|101.158.18.10.in-addr.apra | PTR | est02.estacions.hospitalbinissalem.com |
|102.158.18.10.in-addr.apra | PTR | est03.estacions.hospitalbinissalem.com |

Master de domini: admin@hospitalbinissalem.com</br>DNS primari: ns01.hospitalbinissalem.com</br>Número de sèrie 20261117000001 |### Requeriments zona quirofans.com
????completar

### Requeriments zona estacions.quirofans.com
????completar

