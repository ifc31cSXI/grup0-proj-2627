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
|hospitalbinissalem.com |SOA |Master de domini: admin@hospitalbinissalem.com DNS primari: ns01.hospitalbinissalem.com número de sèrie 2008052001… |
|hospitalbinissalem.com | NS | ns01.hospitalbinissalem.com
|hospitalbinissalem.com | NS |ns02.hospitalbinissalem.com |



### Requeriments zona estacions.hospitalbinissalem.com

|Nom de domini|Tipus de registres|Valor|
|-------------|-----------|-------|
|estacions.hospitalbinissalem.com | SOA | Master de domini: admin@hospitalbinissalem.com DNS primari: ns01.hospitalbinissalem.com
número de sèrie 2008052001 |
|estacions.hospitalbinissalem.com | NS | ns01.hospitalbinissalem.com |


### Requeriments zona inversa per l’adreça 10.18.158.0/24 

|Nom de domini | Tipus de registres | Valor
|-------------|-----------|-------|
|158.18.10.in-addr.apra | SOA | Master de domini: admin@jservers.com DNS primari: ns01.hospitalbinissalem.com número de sèrie 2008052001 … |
|158..18.10.in-addr.apra | NS | ns01.hospitalbinissalem.com |


### Requeriments zona quirofans.com
????completar

### Requeriments zona estacions.quirofans.com
????completar


