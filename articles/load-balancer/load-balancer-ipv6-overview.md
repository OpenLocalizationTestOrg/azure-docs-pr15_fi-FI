<properties
    pageTitle="Yleistä Azure kuormituksen IPv6 | Microsoft Azure"
    description="Tietoja IPv6-tuki Azure ladata tasaustoiminto ja kuormituksen VMs."
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    keywords="IPv6: ta, azure kuormituksen, kahden pinon, julkiseen ip, alkuperäisen ipv6, mobile, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="overview-of-ipv6-for-azure-load-balancer"></a>Azure kuormituksen IPv6 yleiskatsaus

Internetiin yhteydessä oleva kuormituksen tasoitusmääritykset voidaan ottaa käyttöön IPv6-osoite. Lisäksi IPv4-yhteys näin seuraavia ominaisuuksia:

* Alkuperäisen lopusta loppuun IPv6-yhteyden julkinen Internet-asiakkaiden ja Azuren näennäiskoneiden (VMs) välillä kuormituksen kautta.
* Alkuperäisen lopusta loppuun IPv6 lähtevä väliset yhteydet VMs ja julkinen Internet IPv6 käyttävä asiakkaille.

Seuraavassa kuvassa on kuvattu IPv6-toimintojen Azure kuormituksen.

![Azure kuormituksen IPv6: N kanssa](./media/load-balancer-ipv6-overview/load-balancer-ipv6.png)

Kun käyttöön, IPv4 tai IPv6 käyttävä Internet-asiakasohjelman viestiä julkisen IPv4 tai IPv6-osoitteet (tai isäntänimet), Azure Internetiin yhteydessä oleva kuormituksen. Kuormituksen reitittää IPv6-pakettien käyttämällä NAT (NAT) VMs yksityinen IPv6-osoitteet. IPv6-Internet-asiakas ei saa yhteyttä suoraan VMs IPv6-osoite.

## <a name="features"></a>Ominaisuudet

Alkuperäisen IPv6-tuki käyttöön kautta Azure Resurssienhallinta VMs on:

1. IPv6-palveluiden kuormituksen IPv6-asiakkaiden Internetissä
2. Alkuperäisen IPv6- ja IPv4 päätepisteet VMs ("useita käyttöjärjestelmiä pinottu")
3. Saapuvan ja lähtevän käynnistämä alkuperäisen IPv6-yhteydet
4. Tuetut protokollia, kuten TCP, UDP ja HTTP (S) käyttöön palvelun arkkitehtuureihin monia

## <a name="benefits"></a>Edut

Tämä toiminto mahdollistaa seuraavat tärkeät edut:

* Täytä vaatii, että uusia sovelluksia voi käyttää vain IPv6-asiakkaille government-asetukset
* Ota käyttöön mobile ja Internet asioita (IOT) kehittäjien osoite kasvava mobile & IOT markkina-alueet (IPv4 + IPv6) Azuren näennäiskoneiden kahden pinottu avulla

## <a name="details-and-limitations"></a>Tiedot ja rajoitukset

Tiedot

* Azure DNS-palvelu sisältää IPv4 A ja IPv6 AAAA nimi-tietueita, ja vastaa kuormituksen molemmat tietueet. Asiakkaan valitsee osoite (IPv4 tai IPv6) kanssa kommunikoimiseen.
* Kun AM käynnistää julkisen Internet IPv6-yhteys laitteen yhteyden, AM lähde IPv6 address on verkko-osoitteen käännetty (NAT) kuormituksen julkisen IPv6-osoite.
* VMs Linux-käyttöjärjestelmää on määritetty vastaanottamaan kautta DHCP IPv6 IP-osoite. Monia Linux kuvat Azure-valikoiman jo määritetty tukemaan IPv6 ilman muutoksia. Lisätietoja on artikkelissa [Määrittäminen DHCPv6 Linux VMs varten](load-balancer-ipv6-for-linux.md)
* Jos valitset kunto näytteenottimen käytettäväksi kuormituksen tasauspalvelun, IPv4-näytteenottimen luominen ja käyttäminen IPv4- ja IPv6 päätepisteet. Jos yhteyttä AM huolto, IPv4- ja IPv6 päätepisteet otetaan kierto ulos.

Rajoitukset

* Et voi lisätä IPv6 kuormituksen Vastatilin säännöt Azure-portaalissa. Sääntöjen voi luoda vain mallin CLI, PowerShellin kautta.
* Et voi päivittää aiemmin VMs käyttää IPv6-osoitteet. Uusi VMs on otettava käyttöön.
* Yhden verkoston käyttöliittymä-kunkin AM voi määrittää yksittäisen IPv6-osoite.
* Julkinen IPv6-osoitteet ei voi määrittää AM. Hän voi määrittää vain kuormituksen tasauspalvelun.
* Et voi määrittää käänteinen DNS-haku, julkisen IPv6-osoitteet.
* IPv6-osoitteet VMs ei voi olla Azure-pilvipalvelussa jäsenet. Ne voidaan yhdistää Azure Virtual verkkoon (VNet) ja keskenään IPv4-osoitteet niiden päälle.
* Yksityinen IPv6-osoitteet voidaan ottaa käyttöön yksittäisiä VMs resurssiryhmä-, mutta ei voi ottaa käyttöön resurssin ryhmään asteikko joukot kautta.
* Azure VMs ei voi yhdistää muiden VMs toisiin Azure services ja paikallisen laitteiden IPv6 päälle. Ne yhteyttä Azure kuormituksen vain IPv6: ssa. Ne voivat kuitenkin viestiä näiden resurssien avulla IPv4.
* Verkko-käyttöoikeusryhmän (NSG) suojaus IPv4 tuetaan kahden pinon (IPv4 + IPv6) käyttöönotoissa. NSGs eivät vaikuta IPv6-päätepisteet.
* IPv6 AM päätepiste ei näkyvät yhteys Internetiin. Se on kuormituksen tasauspalvelun takana. Vain kuormituksen tasauspalvelun säännöissä portit voi käyttää IPv6: ssa.
* Muuttaminen IdleTimeout parametrin IPv6 on **tällä hetkellä tueta**. Oletusarvo on neljä minuuttia.

## <a name="next-steps"></a>Seuraavat vaiheet

Opi ottamaan kuormituksen IPv6: N kanssa.

* [Käytettävyys IPv6 alueittain](https://go.microsoft.com/fwlink/?linkid=828357)
* [Ottaa käyttöön kuormituksen IPv6: N kanssa mallin avulla](load-balancer-ipv6-internet-template.md)
* [Ottaa käyttöön kuormituksen IPv6: N kanssa Azure PowerShellin avulla](load-balancer-ipv6-internet-ps.md)
* [Kuormituksen IPv6: N kanssa käyttämällä Azure CLI käyttöönotto](load-balancer-ipv6-internet-cli.md)
