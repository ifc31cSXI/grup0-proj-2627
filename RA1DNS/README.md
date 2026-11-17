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
|hospitalbinissalem.com     | SOA | Màster de domini: admin@hospitalbinissalem.com</br>DNS primari: ns01.hospitalbinissalem.com</br>Número de sèrie 20261117000001</br>TTL d'1 minut</br>... |
|hospitalbinissalem.com     | NS  | ns01.hospitalbinissalem.com |
|hospitalbinissalem.com     | NS  | ns02.hospitalbinissalem.com |
|hospitalbinissalem.com     | MX  | mail1.hospitalbinissalem.com |
|hospitalbinissalem.com     | MX  | mail2.hospitalbinissalem.com |
|www.hospitalbinissalem.com |CNAME| web.hospitalbinissalem.com |
|ftp.hospitalbinissalem.com |CNAME| file.hospitalbinissalem.com |
|ns01.hospitalbinissalem.com| A   | 192.168.158.225 |
|ns02.hospitalbinissalem.com| A   | 192.168.158.226 |
|mail1.hospitalbinissalem.com|A   | 192.168.158.227 |
|mail2.hospitalbinissalem.com|A   | 192.168.158.228 |
|web.hospitalbinissalem.com | A   | 192.168.158.229 |
|file.hospitalbinissalem.com| A   | 192.168.158.230 |
|estacions.hospitalbinissalem.com| NS | ns01.hospitalbinissalem.com |
|estacions.hospitalbinissalem.com| NS | ns02.hospitalbinissalem.com |




### Requeriments zona estacions.hospitalbinissalem.com

|Nom de domini|Tipus de registres|Valor|
|-------------|-----------|-------|
|estacions.hospitalbinissalem.com | SOA | Màster de domini: admin@hospitalbinissalem.com</br>DNS primari: ns01.hospitalbinissalem.com</br>Número de sèrie 20261117000001</br>TTL d'1 minut</br>... |
</br>número de sèrie 2008052001 |
|estacions.hospitalbinissalem.com      | NS  | ns01.hospitalbinissalem.com |
|estacions.hospitalbinissalem.com      | NS  | ns02.hospitalbinissalem.com |
|pc01.estacions.hospitalbinissalem.com | A   | 10.18.158.101 |
|pc02.estacions.hospitalbinissalem.com | A   | 10.18.158.102 |
|pc03.estacions.hospitalbinissalem.com | A   | 10.18.158.103 |
|pc04.estacions.hospitalbinissalem.com | A   | 10.18.158.104 |
|pc05.estacions.hospitalbinissalem.com | A   | 10.18.158.105 |

### Requeriments zona inversa per l’adreça 10.18.158.0/24 

|Nom de domini | Tipus de registres | Valor
|-------------|-----------|-------|
|158.18.10.in-addr.apra     | SOA | Màster de domini: admin@hospitalbinissalem.com</br>DNS primari: ns01.hospitalbinissalem.com</br>Número de sèrie 20261117000001</br>TTL d'1 minut</br>... |
|158.18.10.in-addr.apra     | NS  | ns01.hospitalbinissalem.com |
|158.18.10.in-addr.apra     | NS  | ns02.hospitalbinissalem.com |
|101.158.18.10.in-addr.apra | PTR | pc01.estacions.hospitalbinissalem.com |
|102.158.18.10.in-addr.apra | PTR | pc02.estacions.hospitalbinissalem.com |
|103.158.18.10.in-addr.apra | PTR | pc03.estacions.hospitalbinissalem.com |
|104.158.18.10.in-addr.apra | PTR | pc04.estacions.hospitalbinissalem.com |
|105.158.18.10.in-addr.apra | PTR | pc05.estacions.hospitalbinissalem.com |

### Requeriments zona quirofans.com
????completar

### Requeriments zona estacions.quirofans.com
????completar


