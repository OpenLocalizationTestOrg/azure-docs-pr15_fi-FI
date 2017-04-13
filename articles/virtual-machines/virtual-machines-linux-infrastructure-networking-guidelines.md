<properties
    pageTitle="Verkon infrastruktuuri ohjeet | Microsoft Azure"
    description="Lisätietoja avaimen suunnittelu ja käyttöönotto Profiilikuva Azure infrastruktuuripalvelut virtual verkon käyttöönotto."
    documentationCenter=""
    services="virtual-machines-linux"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="networking-infrastructure-guidelines"></a>Verkko-infrastruktuurin ohjeet

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

Tässä artikkelissa keskitytään tietoja tarvittavat suunnittelun vaiheet sisällä Azure virtual verkko ja yhteydet aiemmin paikallinen ympäristöjen välillä.


## <a name="implementation-guidelines-for-virtual-networks"></a>Toteutuksen jälkeisen virtual verkkojen ohjeet

Päätökset:

- Minkä tyyppisiä VPN haluat isännöidä IT työmäärää tai infrastruktuurin (vain pilvipalveluita tai paikallisen-)?
- Paikallisen-verkoissa virtual paljonko osoitetilaa on hyvä isännöimiseen aliverkosta ja VMs nyt ja tulevaisuudessa suositeltavaa kerralla laajentamista?
- Oletko keskitetystä virtual verkkojen tai luoda yksittäisiä virtual verkostoja kunkin resurssiryhmän?

Tehtävät:

- Määritä osoitetilaa virtual verkkojen luodaan.
- Määritä, mitkä aliverkosta ja osoitetilaa varten kunkin.
- Paikallisen-virtual verkkojen määrittää paikalliseen verkkoon kirjauduttaessa osoite välilyöntejä paikallisen sijaintien vaikuttavat virtual verkon VMs saavuttamiseksi.
- Työn verkko ryhmän varmistamiseksi tarvittavat reititys on määritetty kun luominen rajat paikallisen virtual verkkojen paikallisen.
- Luo virtuaalisia verkkoon nimeämiskäytännön mukaisesti.


## <a name="virtual-networks"></a>Virtuaalinen verkot

Virtuaalinen verkot ovat tukevia näennäiskoneiden (VMs) välisten yhteyksien. Voit määrittää aliverkosta, mukautetut IP-osoite, DNS-asetukset, suodattamalla security ja kuormituksen tasaus, fyysinen verkkojen kanssa. [VPN-yhdyskäytävän](../vpn-gateway/vpn-gateway-about-vpngateways.md) tai [Express reitin piiri](../expressroute/expressroute-introduction.md)avulla voit muodostaa Azure virtual verkkojen paikallisen verkkoihin. Voit lukea lisää [virtual verkkojen](../virtual-network/virtual-networks-overview.md)ja niiden osien.

Resurssiryhmiä käyttämällä on sivuston rakenteen virtual verkko-osia. VMs muodostaa virtual verkkoihin omia resurssiryhmä ulkopuolella. Yhteisen rakenne lähestymistavan olisi keskitettyä ryhmiin, jotka sisältävät oman core verkkoinfrastruktuuria, joka voi hallita yleisiä työryhmän luomiseen. VMs ja otettu käyttöön erillinen resurssin ryhmiin sovellukset. Tämän menetelmän avulla, joka sisältää niiden VMs avaamatta käyttöoikeuksien työnkulkuun leveämpi virtual verkko resursseja resurssiryhmän sovelluksen omistajat käyttöoikeus.

## <a name="site-connectivity"></a>Sivuston yhteys

### <a name="cloud-only-virtual-networks"></a>Vain pilvipalveluita virtual verkot
Jos paikalliset käyttäjät ja tietokoneet eivät edellytä jatkuvaa yhteys VMs Azure virtual verkossa, VPN-mallissa on suoraan eteenpäin:

![Vain pilvipalveluita virtual Perusverkkokaavio](./media/virtual-machines-common-infrastructure-service-guidelines/vnet01.png)

Tämä vaihtoehto on yleensä Internetiin yhteydessä oleva toiminnoista, kuten Internet-pohjaisten verkkopalvelin. Voit hallita näitä VMs SSH tai pisteen sivuston VPN-yhteydet.

Koska ne yhdisty paikalliseen verkkoon, vain Azure virtual verkkojen käyttää jääviä yksityinen IP-osoitetilaa. Osoitetilaa varten voi olla samaa yksityinen tilaa, joka on käytössä paikallisen.


### <a name="cross-premises-virtual-networks"></a>Paikallisen-virtual verkot
Jos paikalliset käyttäjät ja tietokoneet edellyttävät jatkuvaa yhteys VMs Azure virtual verkossa, luoda paikallisen-virtual verkkoon. Yhdistä virtual verkon ExpressRoute tai sivuston sivuston VPN-yhteyden paikalliseen verkkoon.

![Paikallisen-virtual verkkokaavio](./media/virtual-machines-common-infrastructure-service-guidelines/vnet02.png)

Tässä määrityksessä Azure virtual verkko on käytettävä pilvipohjainen tunniste on paikalliseen verkkoon.

Koska ne yhteyden paikalliseen verkkoon, rajat paikallisen virtual verkot on käytettävä käytössä organisaatiossa, joka on yksilöivä osoitetilan osa. Samalla tavalla kuin yrityksen käyttäjien määritetään tietyt IP-osoitteiden Azure tulee toiseen paikkaan, kun laajennat verkon ulos.

Sallimaan paketteja kulkemaan paikallisen-virtual verkosta paikalliseen verkkoon sinun on määritettävä asiaa paikallisen osoitteiden etuliitteiden joukon osana virtual verkon lähiverkossa-määritys. Virtual verkoston osoitetilan ja asiaa paikallisen sijainnit joukko, mukaan voi olla useita osoitteiden etuliitteiden paikalliseen verkkoon kirjauduttaessa.

Voit muuntaa vain pilvipalveluita virtual verkon paikallisen-virtual verkkoon, mutta todennäköisesti edellyttää uudelleen IP, VPN-osoitetilaa ja Azure resurssit. Tämän vuoksi Mieti Jos virtual verkko on yhdistetty paikalliseen verkkoon, kun määrität IP-osoitteiden.

## <a name="subnets"></a>Aliverkosta
Aliverkosta avulla voit järjestää toisiinsa liittyvien resurssit joko loogisesti (esimerkiksi yhden aliverkon VMs liittyvät samassa sovelluksessa), tai fyysisesti (esimerkiksi yhden aliverkon resurssiryhmä kohden). Voit käyttää myös aliverkon eristystaso tekniikoita suojauksen.

Paikallisen-virtual verkkojen kannattaa suunnitella aliverkosta samoja sääntöjä, jota käytetään paikallisen resurssien kanssa. **Azure aina käyttää kunkin aliverkon-osoitetilan kolme ensimmäistä IP-osoitteita**. Määrittää, kuinka aliverkon tarvittavat osoitteet, Aloita kappalemäärän VMs, jotka on nyt. Arvioi tulevien kasvun ja aliverkon koon määrittäminen seuraavan taulukon avulla.

Tarvitaan VMs määrä | Host luvun tarvitaan | Aliverkon koosta
--- | --- | ---
1 – 3 | 3 | / 29
4 – 11     | 4 | / 28
12 – 27 | 5 | / 27
28 – 59 | 6 | / 26
60 – 123 | 7 | / 25

> [AZURE.NOTE] Tavallinen paikallisen aliverkosta Host ()-isäntäosoitteet aliverkon kanssa n host bittien enimmäismäärä on 2<sup>n</sup> – 2. Azure aliverkon Host ()-isäntäosoitteet aliverkon kanssa n host bittien enimmäismäärä on 2<sup>n</sup> – 5 (2 ja 3 osoitteille, joita Azure käyttää kunkin aliverkon).

Jos aliverkon koko, joka on liian pientä, on uudelleen IP- ja ota uudelleen VMs-aliverkon.


## <a name="network-security-groups"></a>Verkon käyttöoikeusryhmät
Voit käyttää suodatus säännöt liikenteestä, joka menee virtual verkkojen kautta käyttämällä verkon käyttöoikeusryhmät. Voit luoda hajautetun suodatus säännöt virtual verkko ympäristön hallinta saapuva ja lähtevä liikenne, lähde- ja IP-alueita, sallittu portit jne. Verkon käyttöoikeusryhmät voi suojata aliverkosta virtual verkossa tai suoraan annetun virtual Verkkokäyttöliittymän. On suositeltavaa ovat jotkin verkon käyttöoikeusryhmän suodattaminen liikenne virtual verkoissa. Voit lukea lisää [Verkko-käyttöoikeusryhmät](../virtual-network/virtual-networks-nsg.md)tietoja.


## <a name="additional-network-components"></a>Lisää verkko-osat
Paikalliseen fyysinen verkkoinfrastruktuuria, jossa Azure virtual verkko voi olla pelkästään aliverkosta ja IP-osoitteen määrittäminen. Kun suunnittelet sovelluksen infrastruktuuri, haluat ehkä joitakin näiden osien lisääminen:

- [Yhdyskäytävien VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md) - Azure virtual verkostojen yhdistäminen Azure virtual verkoista tai muodosta yhteys paikalliseen verkkoihin sivusto sivusto VPN-yhteyden kautta. Ota käyttöön erillinen, suojattuja yhteyksiä yhteydet Express reitin. Voit myös antaa käyttäjille suoran pisteen sivuston VPN-yhteydet.
- [Kuormituksen](../load-balancer/load-balancer-overview.md) – tarjoaa kuormituksen tasaamisen liikenteen sekä ulkoisista ja sisäisistä tietoliikenteen haluamallasi tavalla.
- [Sovelluksen Gateway](../application-gateway/application-gateway-introduction.md) - HTTP kuormituksen tasaamisen sovellustason, antamisesta joitakin muita verkkosovellusten sijaan käyttöönotto Azure kuormituksen.
- [Liikenteen hallinta](../traffic-manager/traffic-manager-overview.md) - DNS-pohjaisen liikenteen jakauman ohjaamaan loppukäyttäjien lähinnä käytettävissä oleva päätepiste, voit isännöidä sovelluksesi ulos Azure palvelinkeskusten eri alueilla.


## <a name="next-steps"></a>Seuraavat vaiheet

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 