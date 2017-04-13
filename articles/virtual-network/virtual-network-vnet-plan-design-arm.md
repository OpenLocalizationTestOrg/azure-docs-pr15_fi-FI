<properties
   pageTitle="Azure Virtual Network (VNet) suunnitelma ja rakenne-opas | Microsoft Azure"
   description="Lue, miten voit suunnitella virtual verkot Azure eristystaso, yhteys ja sijainnin tarpeen mukaan."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/08/2016"
   ms.author="jdial" />

# <a name="plan-and-design-azure-virtual-networks"></a>Azure Virtual verkkojen suunnittelemisesta

Luominen VNet kokeilla on tarpeeksi helppoa, mutta on, otetaan käyttöön useita VNets ajan kuluessa tukemaan tuotannon organisaation tarpeisiin. Jotkin suunnittelun ja rakenteen osaat VNets käyttöönotto ja muodostaa tehokkaammin tarvittavat resurssit. Jos et ole aiemmin käyttänyt VNets, on suositeltavaa, [Lue lisätietoja VNets](virtual-networks-overview.md) ja [ottamisesta käyttöön](virtual-networks-create-vnet-arm-pportal.md) jokin ennen kuin jatkat. 

## <a name="plan"></a>Suunnitteleminen

Perusteellisesti ymmärtämisen Azure tilaukset, alueet ja verkkoresursseja on ehdottoman mukaisesti. Voit käyttää huomioon otettavia seikkoja luettelo pohjana. Kun tiedät, että nämä seikat, voit määrittää vaatimukset verkon suunnitteluun.

### <a name="considerations"></a>Huomioon otettavia seikkoja

Ennen kuin vastaaminen suunnittelun kysymyksiä alla, toimi seuraavasti:

- Kaikki Azure luominen koostuu vähintään yksi resurssit. Virtual machine (AM) on resurssi, AM käyttämä verkkosovittimen liittymän (NIC) on resurssi, NIC käyttämä julkiseen IP-osoite on resurssin, Verkkokortti on yhdistetty VNet on resurssi.
- Voit luoda [Azure alue](https://azure.microsoft.com/regions/#services) ja tilauksen resursseja. Ja resurssien voidaan yhdistää vain VNet, joka on saman alue ja ne ovat tilauksen. 
- Voit muodostaa VNets toisiinsa Azure [VPN-yhdyskäytävän](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)avulla. Voit myös muodostaa VNets eri alueilla ja tilaukset tällä tavalla.
- Voit muodostaa VNets paikalliseen verkkoon käyttämällä jotakin [yhteysasetukset](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site) Azure-tietokannassa. 
- Eri resursseja voidaan ryhmitellä yhteen [resurssiryhmien](../azure-resource-manager/resource-group-overview.md#resource-groups)hallinta yksikkönä resurssi on helpompaa. Resurssiryhmä voi olla useita alueelta resursseja, koska resurssit kuuluvat samaan tilaukseen.

### <a name="define-requirements"></a>Määritä vaatimukset

Käytä alla kysymyksiä lähtökohtana Azure verkon suunnitteluun.  

1. Mitä Azure sijainnit Haluatko käyttää isäntään VNets?
2. Tarvitsetko antamaan välisen Azure paikoista?
3. Tarvitsetko antamaan Azure VNet(s) ja paikallisen datacenter(s) välisen?
4. Kuinka monta infrastruktuurin kuin Service (IaaS)-VMs cloud services roolit ja sovellusten WWW-ratkaisun tarvitaan?
5. Tarvitseeko eristää liikenne ryhmiä VMs (eli edusta-WWW-palvelimien ja uudelleen tietokanta-palvelimet) perusteella?
6. On hyvä ohjaamiseen liikenteen suunnan virtual laitteiden avulla?
7. Käyttäjät käsittelevät eri Azure resurssien käyttöoikeudet?

### <a name="understand-vnet-and-subnet-properties"></a>Tietoja VNet ja aliverkon ominaisuudet

VNet ja aliverkosta resurssien avulla määrittää suojauksen reunaa varten työmääriä Azure käynnissä. VNet jakauma osoite välilyöntejä, lasketaan CIDR lohkot perusteella. 

>[AZURE.NOTE] Verkoston järjestelmänvalvojat ovat tuttuja CIDR merkintätapaa. Jos et ole tottunut CIDR, [Katso lisätietoja sen](http://whatismyipaddress.com/cidr).

VNets sisältää seuraavat ominaisuudet.

|Ominaisuus|Kuvaus|Rajoitukset|
|---|---|---|
|**Nimi**|VNet nimi|Merkkijono, enimmillään 80. Voi olla kirjaimia, numeroita, alaviiva, jaksot tai väliviivoja. On alettava kirjaimella tai numero. Lopussa on oltava kirjain, numero tai alaviivoja. Voi sisältää isot ja pienet kirjaimet.|  
|**sijainti**|Azure sijainti (tunnetaan myös nimellä alue).|On oltava jokin kelvollinen Azure sijainneista.|
|**addressSpace**|Osoitteiden etuliitteiden, jotka muodostavat CIDR merkintätavan VNet kokoelma.|Käytettävä matriisin kelvollinen CIDR osoitelohkot, mukaan lukien julkiseen IP-osoitealueet.|
|**aliverkosta**|Muotoiluja sisältäviä aliverkosta, jotka muodostavat VNet|Katso alla oleva aliverkon ominaisuudet-taulukko.||
|**dhcpOptions**|Objekti, joka sisältää yksittäisen pakollinen-ominaisuudeksi nimeltä **dnsServers**.||
|**dnsServers**|DNS-palvelimet VNet käyttämä taulukko. Jos palvelinta ei ole määritetty, käytetään Azure sisäinen nimenselvitys.|Matriisin enintään 10 DNS-palvelimilta IP-osoite on oltava.| 

Aliverkon on Aliobjektin resurssi, VNet ja auttaa Määritä CIDR estäminen käyttämällä IP-osoitteiden etuliitteiden välilyönnit osoitteen osia. NIC voit aliverkosta lisätään ja yhdistetty VMs, tarjoamalla connectivity eri toiminnoista.

Aliverkosta sisältää seuraavat ominaisuudet. 

|Ominaisuus|Kuvaus|Rajoitukset|
|---|---|---|
|**Nimi**|Aliverkon nimi|Merkkijono, enintään 80. Voi olla kirjaimia, numeroita, alaviiva, jaksot tai väliviivoja. On alettava kirjaimella tai numero. Lopussa on oltava kirjain, numero tai alaviivoja. Voi sisältää isot ja pienet kirjaimet.|
|**sijainti**|Azure sijainti (tunnetaan myös nimellä alue).|On oltava jokin kelvollinen Azure sijainneista.|
|**addressPrefix**|Yksittäisen osoitteen etuliitteen, jotka muodostavat CIDR merkintätavan aliverkon|Käytettävä yhtenä CIDR solualueena, joka kuuluu jonkin VNet osoite välilyöntejä.|
|**networkSecurityGroup**|Käytetty aliverkon NSG|Katso [NSGs](resource-groups-networking.md#Network-Security-Group)|
|**routeTable**|Reititystaulukon käytetty aliverkon|Katso [UDR](resource-groups-networking.md#Route-table)|
|**ipConfigurations**|IP-määritysten objektien NIC aliverkon yhteydessä käyttämien kokoelma|Katso [IP-määritys](../resource-groups-networking.md#IP-configurations)|

### <a name="name-resolution"></a>Nimenselvitys

Oman VNet käyttää oletusarvon mukaan [antamalla Azure nimenselvitys.](virtual-networks-name-resolution-for-vms-and-role-instances.md#Azure-provided-name-resolution) Voit ratkaista sisällä VNet ja julkinen Internet-nimet. Kuitenkin Jos muodostat yhteyden oman VNets paikallisen tietojen-keskukset, tarvitset antamaan [DNS-palvelimeen](virtual-networks-name-resolution-for-vms-and-role-instances.md#Name-resolution-using-your-own-DNS-server) nimien verkon välillä.  

### <a name="limits"></a>Rajoitukset

Tarkista voit varmistaa, että rakenteeseen ei ristiriidassa mitään rajoituksia [Azure rajoittaa](../azure-subscription-service-limits.md#networking-limits) -artikkelin verkko rajoitukset. Tietyt rajoitukset voidaan lisätä avaamalla tuki lippu.

### <a name="role-based-access-control-rbac"></a>Roolipohjainen käyttöoikeuksien valvonta (RBAC)

[Azure RBAC](../active-directory/role-based-access-built-in-roles.md) avulla voit määrittää käyttöoikeuksia eri käyttäjillä voi olla eri resursseja Azure-tietokannassa. Näin voit eroteltava työskentelyn apuna työryhmän niiden tarpeiden perusteella. 

Virtuaalinen verkot on kyse, **Verkon osallistujan** rooli käyttäjillä on Azure Resurssienhallinta virtual verkkoresursseja täydet oikeudet. Vastaavasti, **Perinteinen verkon osallistujan** rooli käyttäjillä on täydet oikeudet perinteinen virtual verkkoresursseihin.

>[AZURE.NOTE] Voit myös [luoda omia rooleja](../active-directory/role-based-access-control-configure.md) erottaa järjestelmänvalvojan tarpeitasi.

## <a name="design"></a>Rakenne

Kun tiedät vastauksia kysymyksiin [suunnittelu](#Plan) -osassa, tarkista seuraavat ennen kuin määrität oman VNets.

### <a name="number-of-subscriptions-and-vnets"></a>Tilaukset-ja VNets

Ota huomioon useita VNets luodaan seuraavissa tilanteissa:

- **VMs, joka on sijoitettava eri Azure sijainteihin**. VNets Azure ovat alueellisen. Niitä ei voi olla sijainnit. Tämän vuoksi tarvitset vähintään yksi VNet kunkin Azure tallennuspaikkaa host VMs.
- **Toiminnoista, jotka on oltava kokonaan erillään toisistaan**. Voit luoda erilliset VNets, jotka käyttävät samaa IP address välilyöntejä, vaikka erottaa toisistaan eri toiminnoista. 

Ota huomioon, näet yllä rajat tilauskohtaisten alueittain. Tämä tarkoittaa sitä, voit käyttää useita tilauksia niin, että voit ylläpitää Azure resurssien rajoitus. Voit käyttää sivuston sivuston VPN tai ExpressRoute-piiri, muodostaa VNets eri tilaukset.

### <a name="subscription-and-vnet-design-patterns"></a>Tilauksen ja VNet rakenne kuviot

Seuraavassa taulukossa kerrotaan joitakin yleisiä rakenne-kuvioiden määrittäminen tilaukset ja VNets avulla.

|Skenaario|Kaavio|Asiantuntijoille|Kuvakkeet|
|---|---|---|---|
|Yhden tilauksen, kaksi VNets sovelluksen kohden|![Yhden tilauksen](./media/virtual-network-vnet-plan-design-arm/figure1.png)|Voit hallita vain yksi tilaus.|Enimmäismäärä Azure alueittain VNets. Tämän jälkeen on useita tilauksia. Lue lisätietoja [Azure rajoittaa](../azure-subscription-service-limits.md#networking-limits) -artikkelin.|
|Yksi tilaus kohden kaksi VNets kohti app-sovelluksessa|![Yhden tilauksen](./media/virtual-network-vnet-plan-design-arm/figure2.png)|Käyttää vain kaksi VNets tilauskohtaisten.|Näyttävyyttä kun on liian monta sovellusten hallinta.|
|Yksi tilaus business yksikön kaksi VNets sovelluksen kohden.|![Yhden tilauksen](./media/virtual-network-vnet-plan-design-arm/figure3.png)|Saldo tilausten määrä ja VNets välillä.|Enimmäismäärä business (tilausta) kohti VNets. Lue lisätietoja [Azure rajoittaa](../azure-subscription-service-limits.md#networking-limits) -artikkelin.|
|Yksi tilaus business yksikön kaksi VNets ryhmittäin sovelluksia.|![Yhden tilauksen](./media/virtual-network-vnet-plan-design-arm/figure4.png)|Saldo tilausten määrä ja VNets välillä.|Sovellukset on ilmoiteta aliverkosta ja NSGs avulla.|


### <a name="number-of-subnets"></a>Aliverkosta määrä

Ota huomioon useita aliverkosta-VNet seuraavissa tilanteissa:

- **Ei ole tarpeeksi yksityisten IP-osoitteiden kaikki NIC-aliverkon**. Jos aliverkon-osoitetilaa ei sisällä riittävästi IP-osoitteiden aliverkon NIC määrä, haluat luoda useita aliverkosta. Muista, että Azure varaa 5 yksityisten IP-osoitteiden kunkin aliverkosta ei voi käyttää: osoitetilaa (aliverkon osoite ja moni) ensimmäisen ja viimeisen osoitteet ja 3 osoitteet käytettävän sisäisesti (DHCP ja tarkoituksiin DNS). 
- **Suojaus**. Voit käyttää aliverkosta VMs ryhmät erottaa toisistaan, työmääriä, joka on usean kerroksen rakenne ja käyttää näitä aliverkosta eri [verkon käyttöoikeusryhmät (NSGs)](virtual-networks-nsg.md#subnets) .
- **Hybrid yhteys**. Voit käyttää VPN-yhdyskäytäviä ja ExpressRoute piirit [muodostaa](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site) yhteyttä VNets keskenään ja paikallisen-tietojen center(s). VPN yhdyskäytävien ja ExpressRoute piirit vaativat aliverkon itse luoda.
- **Virtual laitteita**. Voit käyttää virtual laitteeseen, kuten palomuurin, WAN accelerator tai VPN-yhdyskäytävän Azure-VNet. Kun teet näin, on sellaisten laitteiden [reitin liikenteen](virtual-networks-udr-overview.md) ja erottaa ne oman aliverkon.

### <a name="subnet-and-nsg-design-patterns"></a>Aliverkon ja NSG rakenne kuviot

Seuraavassa taulukossa kerrotaan joitakin yleisiä rakenne-kuvioiden määrittäminen käyttämällä aliverkosta.

|Skenaario|Kaavio|Asiantuntijoille|Kuvakkeet|
|---|---|---|---|
|Yksittäisen aliverkon NSGs kohti sovelluskerroksen app kohden|![Yksittäisen aliverkon](./media/virtual-network-vnet-plan-design-arm/figure5.png)|Vain yksi aliverkon hallintaan.|Useita NSGs tarvittavat kunkin sovelluksen eristää.|
|Yhden aliverkon kohti NSGs kohti sovelluskerroksen-sovelluksessa|![Aliverkon app kohden](./media/virtual-network-vnet-plan-design-arm/figure6.png)|Vähemmän NSGs hallintaan.|Voit hallita useita aliverkosta.|
|Yhden aliverkon sovelluskerroksen-sovelluksen kohti NSGs kohden.|![Aliverkon / kerros](./media/virtual-network-vnet-plan-design-arm/figure7.png)|Saldo aliverkosta määrä ja NSGs välillä.|Enimmäismäärä tilauskohtaisten NSGs. Lue lisätietoja [Azure rajoittaa](../azure-subscription-service-limits.md#networking-limits) -artikkelin.|
|Yhden aliverkon kohti sovelluskerroksen kohti NSGs aliverkon kohden-sovelluksessa|![Aliverkon / kerros app kohden](./media/virtual-network-vnet-plan-design-arm/figure8.png)|Mahdollisesti vähemmän NSGs.|Voit hallita useita aliverkosta.|

## <a name="sample-design"></a>Esimerkki rakenne

Voit havainnollistaa tämän artikkelin tiedot sovelluksen tarvittaessa seuraava tilanne.

Voit käyttää yritys, joka on 2 Pohjois-Amerikassa tietojen keskikohdan mukaan ja kaksi keskikohdan mukaan Europe. 6 eri asiakkaan aukeaman sovellukset, ylläpitämä 2 eri tunnistaa liiketoimintayksiköitä, jonka haluat siirtää Azure vetäjänä. Tavallinen arkkitehtuuri sovellukset ovat seuraavat:

- App1, App2, App3 ja App4 ovat verkkosovellusten isännöimät Ubuntu Linux-palvelimiin. Kunkin sovelluksen muodostaa yhteyden eri sovelluspalvelin isännöivä RESTful services Linux-palvelimiin. RESTful palveluiden yhdistäminen uudelleen MySQL-tietokantaan.
- App5 ja App6 ovat verkkosovellusten isännöimät Windows Server 2012 R2: n Windows-palvelimiin. Kunkin sovelluksen muodostaa yhteyden uudelleen SQL Server-tietokantaan.
- Kaikki sovellukset isännöidään tällä hetkellä johonkin yrityksen tiedot-keskukset Pohjois-Amerikassa.
- Paikallisen tietojen keskikohdan mukaan käyttämällä 10.0.0.0/8 osoitetilaa varten.

Sinun täytyy suunnitella VPN-ratkaisun, joka täyttää seuraavat vaatimukset:

- Kukin business yksikkö ei vaikuta resurssien käyttöä koskevia muita liiketoimintayksiköitä.
- Sinun pitäisi pienentää VNets ja aliverkosta helpottavat hallintaa.
- Yritystietojen yksittäisen pitäisi olla yksittäinen testi/kehitys VNet käytetään kaikissa sovelluksissa.
- Kunkin sovelluksen isännöinnistä 2 kohti Manner (Pohjois-Amerikka ja Europe) eri Azure tietojen keskikohdan mukaan.
- Kunkin sovelluksen eristetään kokonaan toisistaan.
- Kunkin sovelluksen voivat käyttää asiakkaat http:n Internetin välityksellä.
- Salatun tunnelin paikallisen tietojen keskukset yhdistetään käyttäjät voivat käyttää kunkin sovelluksen.
- Paikallisen tietojen keskikohdan mukaan-yhteyden käytettävä aiemmin VPN-laitteet.
- Yrityksen verkko-ryhmä on täydet oikeudet VNet-määritys.
- Kehittäjät business kunkin yksikön vain pitäisi VMs käyttöön aiemmin aliverkosta.
- Kaikki sovellukset siirretään sellaisina kuin ne ovat Azure (hissin-ja-vaihto).
- Tietokantojen paikkoihin olisi replikoida kerran päivässä Azure muihin sijainteihin.
- Kunkin sovelluksen käytettävä 5 edusta-WWW-palvelimien 2 sovelluksen servers (tarvittaessa) ja 2 tietokantapalvelimiin.

### <a name="plan"></a>Suunnitteleminen

Käynnistä rakenteen suunnitteleminen vastaamalla [Määritä vaatimukset](#Define-requirements) -osassa kysymys alla kuvatulla tavalla.

1. Mitä Azure sijainnit Haluatko käyttää isäntään VNets?

    Pohjois-Amerikassa 2 sijainnit ja Euroopan 2 sijainnit. Kannattaa valita perustuvat aiemmin paikalliseen tietojen keskikohdan mukaan fyysinen sijainti. Näin fyysinen sijainneista Azure-yhteys on parempi viive.

2. Tarvitsetko antamaan välisen Azure paikoista?

    Kyllä. Koska tietokannat on replikoida kaikista sijainneista.

3. Tarvitsetko antamaan välisen Azure VNet(s) ja että paikallisten tietojen center(s)?

    Kyllä. Koska käyttäjät yhteydessä paikallisen tietojen keskukset on voi käyttää sovellukset salatun tunnelin kautta.
 
4. Kuinka monta IaaS VMs tarvitaan ratkaisuun?

    200 IaaS VMs. App1, App2 ja App3 vaativat 5 WWW-palvelimien kunkin 2 sovellusten kunkin ja palvelinten 2 tietokantapalvelimien kunkin. Tämä on 9 IaaS VMs kohti sovelluksen tai 36 IaaS VMs yhteensä. App5 ja App6 vaativat 5 WWW-palvelimien ja 2 tietokannan palvelinten kunkin. Tämä on 7 IaaS VMs kohti sovelluksen tai 14 IaaS VMs yhteensä. Tämän vuoksi 50 IaaS VMs on kaikissa sovelluksissa Azure alueittain. Koska haluamme 4 alueet, ole 200 IaaS VMs.

    Sinun on myös antaa DNS-palvelimet kunkin VNet tai paikallisen tietojen-keskukset ratkaisemaan nimeä Azure-IaaS VMs ja paikallisen verkon välillä. 

5. Tarvitseeko eristää liikenne ryhmiä VMs (eli edusta-WWW-palvelimien ja uudelleen tietokanta-palvelimet) perusteella?

    Kyllä. Kunkin sovelluksen on oltava kokonaan toisistaan erillään ja kerroksia sovellus tulisi olla eristetty. 

6. On hyvä ohjaamiseen liikenteen suunnan virtual laitteiden avulla?

    Ei. Virtuaalinen laitteiden voidaan säätää tarkemmin liikenteen suunnan, mukaan lukien tarkempia tietoja tasossa kirjaaminen. 

7. Käyttäjät käsittelevät eri Azure resurssien käyttöoikeudet?

    Kyllä. Verkko-ryhmän on virtual verkkoasetusten täydet oikeudet, kun kehittäjät vain pitäisi niiden VMs käyttöön olemassa aliverkosta. 

### <a name="design"></a>Rakenne

Noudata määrittäminen tilaukset, VNets tai aliverkosta NSGs rakenne. Käsiteltävät aiheet NSGs tähän, mutta olisi Lue lisää [NSGs](virtual-networks-nsg.md) asentamiseksi rakenteeseen.

**Tilaukset-ja VNets**

Tilaukset- ja VNets liittyvät seuraavat vaatimukset:

- Kukin business yksikkö ei vaikuta resurssien käyttöä koskevia muita liiketoimintayksiköitä.
- Olisi pienentää VNets ja aliverkosta määrä.
- Yritystietojen yksittäisen pitäisi olla yksittäinen testi/kehitys VNet käytetään kaikissa sovelluksissa.
- Kunkin sovelluksen isännöinnistä 2 kohti Manner (Pohjois-Amerikka ja Europe) eri Azure tietojen keskikohdan mukaan.

Perusteella näitä vaatimuksia on tilauksen kunkin business yksikön. Sen mukaan, kulutus resurssien business yksiköstä Laske kohti rajoitukset muiden liiketoimintayksiköitä varten. Ja jälkeen, kun haluat pienentää VNets määrä, kannattaa harkita **yksi tilaus business yksikön kaksi VNets ryhmittäin sovellusten** kuvion tarkastelu alapuolella.

![Yhden tilauksen](./media/virtual-network-vnet-plan-design-arm/figure9.png)

Haluat myös määrittää kunkin VNet osoitetilaa varten. Sillä tarvitset paikalliset tiedot väliset yhteydet keskittää ja Azure alueiden Azure VNets käytettäviä osoitetilaa ei voi olla paikallisen verkoston kanssa ja käyttää kunkin VNet osoitetilaa eivät pitäisi olla aiemmin VNets muiden kanssa. Alla olevassa taulukossa osoite-välilyöntejä-sovelluksella täyttävät nämä vaatimukset.  

|**Tilauksen**|**VNet**|**Azure-alue**|**Osoitetilaa varten**|
|---|---|---|---|
|BU1|ProdBU1US1|Länsi USA|172.16.0.0/16|
|BU1|ProdBU1US2|Yhdysvaltojen Itä|172.17.0.0/16|
|BU1|ProdBU1EU1|Pohjois-Eurooppa|172.18.0.0/16|
|BU1|ProdBU1EU2|Länsi Europe|172.19.0.0/16|
|BU1|TestDevBU1|Länsi USA|172.20.0.0/16|
|BU2|TestDevBU2|Länsi USA|172.21.0.0/16|
|BU2|ProdBU2US1|Länsi USA|172.22.0.0/16|
|BU2|ProdBU2US2|Yhdysvaltojen Itä|172.23.0.0/16|
|BU2|ProdBU2EU1|Pohjois-Eurooppa|172.24.0.0/16|
|BU2|ProdBU2EU2|Länsi Europe|172.25.0.0/16|

**Aliverkosta ja NSGs määrä**

Aliverkosta ja NSGs liittyvät seuraavat vaatimukset:

- Olisi pienentää VNets ja aliverkosta määrä.
- Kunkin sovelluksen eristetään kokonaan toisistaan.
- Kunkin sovelluksen voivat käyttää asiakkaat http:n Internetin välityksellä.
- Salatun tunnelin paikallisen tietojen keskukset yhdistetään käyttäjät voivat käyttää kunkin sovelluksen.
- Paikallisen tietojen keskikohdan mukaan-yhteyden käytettävä aiemmin VPN-laitteet.
- Tietokantojen paikkoihin olisi replikoida kerran päivässä Azure muihin sijainteihin.

Perusteella näitä vaatimuksia, voi käyttää yhdessä aliverkon sovelluskerroksen kohden ja NSGs avulla suodatin liikenne sovellusta kohden. Sen mukaan, sinun on vain 3 aliverkosta kunkin VNet (edusta, sovelluskerroksen ja tietojen layer)- ja yksi NSG kohti aliverkon kohden. Tässä tapauksessa kannattaa harkita **yhden aliverkon kohti sovelluskerroksen, NSGs kohti app** rakenne kaavan. Alla olevassa kuvassa edustava **ProdBU1US1** VNet rakenne kuvion käyttö.

![Yhden aliverkon / kerros, yksi NSG kohti sovelluksen / kerros](./media/virtual-network-vnet-plan-design-arm/figure11.png)

Kuitenkin myös haluat luoda välillä VNets ja paikallisen tietojen-keskukset VPN-yhteys ylimääräisiä aliverkon. Ja määritä kunkin aliverkon osoitetilaa. Alla olevassa kuvassa näkyy esimerkki ratkaista **ProdBU1US1** VNet. Tässä skenaariossa replikoida kunkin VNet varten. Jokainen väri edustaa toiseen sovellukseen.

![Esimerkki VNet](./media/virtual-network-vnet-plan-design-arm/figure10.png)

**Käyttöoikeuksien hallinta**

Voit käyttää ohjausobjektin liittyvät seuraavat vaatimukset:

- Yrityksen verkko-ryhmä on täydet oikeudet VNet-määritys.
- Kehittäjät business kunkin yksikön vain pitäisi VMs käyttöön aiemmin aliverkosta.

Perusteella näitä vaatimuksia, voit lisätä käyttäjiä verkko kuulumisia valmiin **Verkon osallistujan** rooli, valitse jokaisen tilauksen; ja luo mukautettu rooli jokaisen tilauksen hänelle oikeudet VMs lisääminen aiemmin aliverkosta sovellusten-kehittäjille.

## <a name="next-steps"></a>Seuraavat vaiheet

- [Ota virtual verkon](virtual-networks-create-vnet-arm-template-click.md) perusteella tilanne.
- Tietoja [kuormituksen tasaus](../load-balancer/load-balancer-overview.md) IaaS VMs ja [hallita useita Azure alueiden päälle reitityksen](../traffic-manager/traffic-manager-overview.md).
- Lue lisää [NSGs ja kuinka voit suunnitella](virtual-networks-nsg.md) NSG-ratkaisun.
- Lisätietoja oman [paikallisen - ja VNet yhteysasetukset](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site).
