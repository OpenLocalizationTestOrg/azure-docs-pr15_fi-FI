<properties 
   pageTitle="VPN-yhdyskäytävän suunnittelun ja rakenteen | Microsoft Azure"
   description="Tietoja VPN-yhdyskäytävän suunnittelun ja rakenteen paikallisen-, hybrid ja VNet VNet yhteydet"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management,azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="cherylmc"/>

# <a name="planning-and-design-for-vpn-gateway"></a>Suunnittelun ja rakenteen VPN-yhdyskäytävän

Suunnitteluun paikallisen- ja VNet VNet määritykset voi olla yksinkertaisen tai monimutkaisen verkko tarpeidesi. Tässä artikkelissa käydään läpi basic suunnittelun ja rakenteen huomioon otettavia seikkoja.

## <a name="planning"></a>Suunnittelu


### <a name="compare"></a>Paikallisen-yhteyden asetukset

Jos haluat ottaa yhteyttä paikalliseen sivustojen suojatusti virtual verkkoon, sinulla on voit tehdä tämän kolmella eri tavalla: sivuston sivusto-kohdan sivuston ja ExpressRoute. Vertaa eri rajat paikallisen yhteydet, jotka ovat käytettävissä. Voit valita vaihtoehdon voi riippua eri huomioon otettavia seikkoja, kuten:


- Millaisia siirtonopeuden ratkaisu edellyttävät?
- Haluatko julkisen Internetin kautta suojatun VPN-yhteyden tai yksityinen-yhteyden kautta?
- Sinulla on julkinen IP-osoite käytettävissä?
- Aiotko käyttää VPN laitteen? Jos näin on, on yhteensopiva?
- Muodostetaan todella tietokoneiden tai Haluatko pysyvä yhteys sivustossasi?
- Millaista VPN-yhdyskäytävän tarvitaan ratkaisu kannattaa luoda?
- Mitä yhdyskäytävän SKU kannattaa käyttää?


Seuraavan taulukon avulla voit päättää ratkaisu kannattaa yhteys.


[AZURE.INCLUDE [vpn-gateway-cross-premises](../../includes/vpn-gateway-cross-premises-include.md)]



### <a name="gwrequire"></a>Yhdyskäytävän vaatimukset VPN-tyyppi ja SKU

[AZURE.INCLUDE [vpn-gateway-gwsku](../../includes/vpn-gateway-gwsku-include.md)]

Saat lisätietoja yhdyskäytävän tuotteissa [VPN-yhdyskäytävän asetuksia](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

#### <a name="aggregate-throughput-by-sku-and-vpn-type"></a>Kooste siirtonopeuden tuote- ja VPN-tyypin mukaan

Seuraavassa taulukossa on yhdyskäytävän tyypit ja arvioitu kooste siirtonopeuden. Arvioitu kooste siirtonopeuden ehkä suunnittelemasi vaikuttavien korjaustermi.
Hinnat siirtymiseen yhdyskäytävän tuotteissa. Katso tietoja hinnat [VPN-yhdyskäytävän hinnat](https://azure.microsoft.com/pricing/details/vpn-gateway/). Tässä taulukossa koskee Resurssienhallinta ja perinteinen käyttöönoton mallit.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggtput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)] 

#### <a name="supported-configurations-by-sku-and-vpn-type"></a>Tuetut käyttömahdollisuudet tuote- ja VPN-tyypin mukaan

[AZURE.INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)] 

### <a name="wf"></a>Työnkulun

Seuraavassa luettelossa kuvataan yhteisen työnkulun cloud yhteyksiä varten:

1.  Suunnittele ja connectivity topologian suunnitteleminen ja luettelon kaikkien verkkojen yhdistettävää osoite-välilyöntejä.
2.  Azure virtual verkoston luominen. 
3.  Luo VPN-yhdyskäytävän virtual verkossa.
4.  Luo ja määritä yhteydet paikallisen verkkojen tai muiden virtual käyttäminen (tarvittaessa).
5.  Luo ja määritä pisteen sivuston yhteyden Azure-VPN-yhdyskäytävän, (tarvittaessa).
 

## <a name="design"></a>Rakenne

### <a name="topologies"></a>Yhteyden topologioissa

Aloita tarkistamalla [VPN yhdyskäytävä](vpn-gateway-about-vpngateways.md) on artikkelissa kaaviot. Artikkelissa basic kaaviot, kunkin topologian (Resurssienhallinta tai perinteinen) käyttöönoton mallit ja mitkä käyttöönotto-Työkalut voit ottaa käyttöön kokoonpanosi.   

### <a name="designbasics"></a>Suunnittelun perusteet

Seuraavissa osissa käsitellään VPN yhdyskäytävän perusteet. Ota huomioon myös [Verkko services rajoitukset](../articles/azure-subscription-service-limits.md#networking-limits).


#### <a name="subnets"></a>Tietoja aliverkosta

Kun luot yhteyksiä, ota huomioon aliverkon alueita. Ei voi olla päällekkäiset aliverkon osoitealueet. Päällekkäisten aliverkon on, kun yksi VPN tai paikallisen sijainti sisältää saman osoitetilan, joka sisältää toiseen sijaintiin. Tällöin tarvitset oman verkon insinöörien paikallisen paikallisen-verkoissa, alueen, voit käyttää Azure IP-osoite, carve osoitteiden tilaa/aliverkosta. Tarvitset osoitetilaa, jota ei ole käytetty paikallisen paikalliseen verkkoon. 

Välttäminen päällekkäiset aliverkosta on myös tärkeää, kun käsittelet VNet VNet yhteydet. Jos yhteyttä aliverkosta päällekkäin ja IP-osoite on lähettäminen- ja kohde VNets, VNet VNet yhteydet epäonnistuu. Azure ei voi reitittää tiedot muiden VNet, koska lähettämisen VNet kuuluu kohdeosoite. 

VPN yhdyskäytävien edellyttävät tietyn aliverkon kutsutaan yhdyskäytävän aliverkon. Kaikki yhdyskäytävän aliverkosta nimen on oltava GatewaySubnet toimii oikein. Varmista, että ei, jos haluat nimetä yhdyskäytävän aliverkon eri nimellä ja ei käyttöönottaminen VMs tai muu yhdyskäytävän aliverkon. Katso [yhdyskäytävän aliverkosta](vpn-gateway-about-vpn-gateway-settings.md#gwsub).

#### <a name="local"></a>Tietoja paikallisen verkon yhdyskäytävät

Paikallinen yhdyskäytävien viittaa yleensä paikallisen sijainti. Perinteinen käyttöönotto-mallin paikalliseen verkkoon kirjauduttaessa yhdyskäytävän kutsutaan paikallisen verkko-sivusto. Kun määrität paikallisen yhdyskäytävien, voit antaa sille nimen, määrittää paikallisen VPN-laite julkiseen IP-osoite ja määrittää paikallisen sijainnin olevien osoitteiden etuliitteiden. Azure tarkastelee kohde osoitteiden etuliitteiden verkkoliikenteelle, kuulee määrityksistä, jotka on määritetty paikalliseen verkkoon kirjauduttaessa yhdyskäytävän ja reitittää paketit vastaavasti. Voit muokata näitä osoite etuliitteitä tarpeen mukaan. Lisätietoja on artikkelissa [paikallisen verkon yhdyskäytävien](vpn-gateway-about-vpn-gateway-settings.md#lng).


#### <a name="gwtype"></a>Tietoja yhdyskäytävän tyypit

Valitsemalla oman topologian tyyppi on oikea yhdyskäytävä on tärkeää. Jos valitset tyyppi on väärä, että yhdyskäytävä ei toimi oikein. Yhdyskäytävän tyyppi määrittää, miten yhdyskäytävän itse yhdistää ja on pakollinen määritys resurssien hallinnan käyttöönottomalli.

Yhdyskäytävän ovat:

- VPN
- ExpressRoute

#### <a name="connectiontype"></a>Tietoja yhteystyypit

Kunkin määritys edellyttää tietyn tietoyhteyden. Yhteyden ovat:

- IP
- Vnet2Vnet
- ExpressRoute
- VPNClient


#### <a name="vpntype"></a>Tietoja VPN-tyypit

Kunkin määritys edellyttää VPN-tietyntyyppinen. Jos liittää kaksi määritykset, kuten sivuston sivuston yhteyden ja saman VNet pisteen sivuston yhteyden luominen sinun on käytettävä VPN-tyyppi, jotka täyttävät molemmat yhteyden vaatimukset.

[AZURE.INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)] 

Seuraavissa taulukoissa VPN-tyyppi, kun se yhdistää kunkin yhteysmäärityksen. Varmista, että yhdyskäytävän VPN-tyyppi vastaa määrityksistä, jotka haluat luoda. 


[AZURE.INCLUDE [vpn-gateway-table-vpntype](../../includes/vpn-gateway-table-vpntype-include.md)] 

### <a name="devices"></a>Sivusto yhteyksien VPN-laitteet

Käyttöönoton mallia, vaikka sivusto sivusto-yhteyden määrittäminen edellyttää seuraavia kohteita:

- VPN-laitteella, joka on yhteensopiva Azure VPN yhdyskäytävät
- Julkisen IPv4-IP-osoite, jota ei ole on NAT

Sinun täytyy kokemus VPN-laitteen määrittäminen, tai olet jonkun, joka määrittää laitteen puolestasi. Lisätietoja VPN-laitteet tarkastella [tietoja VPN-laitteita](vpn-gateway-about-vpn-devices.md). VPN laitteet-artikkelissa on tietoja vahvistettu laitteita, laitteita, jotka on vahvistettu vaatimukset ja linkkejä laitteen määritysten asiakirjoihin, jos se on käytettävissä.

### <a name="forcedtunnel"></a>Harkitse pakotettua tunnelin reititys

Useimmat määritykset voit määrittää pakotettu tunneling. Pakotetun tunneling avulla voit uudelleenohjata tai "voimassa" kaikki sidottujen Internet-liikenne takaisin paikalliseen sijaintisi kautta sivusto sivusto VPN-tunneli tarkastus ja valvonta. Tämä on useimmissa yrityksen IT kriittinen vaatimus käytännöt. 

Ilman pakotettu tunneling Internet sidottujen tietoliikenteen oman VMs Azure-tietokannassa aina käy läpi Azure verkkoinfrastruktuuria suoraan-Internet-asetuksen avulla voit tutkia tai valvonta liikenne ilman. Internet-luvattomasti voi aiheuttaa mahdollisesti tietojen paljastaminen tai muun tyyppisiä suojauksen ilmeisesti.

**Pakotettu tunneloinnin kaavio**

![Pakotettu Tunneling yhteys] (./media/vpn-gateway-plan-design/forced-tunnel.png "Pakotettu tunneling")

Pakotetun tunneloinnin yhteyden voi määrittää sekä käyttöönotto-malleissa ja käyttämällä eri työkaluja. Katso lisätietoja seuraavassa taulukossa. Päivitämme tässä taulukossa, kun uusia artikkeleita, uudet käyttöönoton mallit ja muita työkaluja palvelupakettiisi määritysten. Artikkelin ollessa käytettävissä on linkki suoraan taulukosta.

[AZURE.INCLUDE [vpn-gateway-table-forcedtunnel](../../includes/vpn-gateway-table-forcedtunnel-include.md)] 



## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja apuna suunnittelemasi [VPN yhdyskäytävän usein kysytyt kysymykset](vpn-gateway-vpn-faq.md) ja [VPN yhdyskäytävä](vpn-gateway-about-vpngateways.md) -artikkeleissa.

Saat lisätietoja tiettyä yhdyskäytävän asetusten [VPN yhdyskäytävän asetusten tietoja](vpn-gateway-about-vpn-gateway-settings.md).




