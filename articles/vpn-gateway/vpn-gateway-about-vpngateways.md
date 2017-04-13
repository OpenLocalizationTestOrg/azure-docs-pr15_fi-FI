<properties 
   pageTitle="Tietoja VPN-yhdyskäytävän | Microsoft Azure"
   description="Lisätietoja Azure Virtual verkkoja VPN-yhdyskäytävän yhteydet."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="cherylmc" />

# <a name="about-vpn-gateway"></a>Tietoja VPN-yhdyskäytävän


Virtuaalinen yhdyskäytävien käytetään lähettämään verkkoliikennettä Azure virtual verkkojen ja paikallisen sijainnit sekä myös virtual verkostoja Azure (VNet VNet) välillä. Kun määrität VPN-yhdyskäytävän, luominen ja määrittäminen virtual yhdyskäytävien ja virtual verkkoyhteyden yhdyskäytävä.

Kun luot yhdyskäytävän VPN-resurssin Resurssienhallinta käyttöönoton mallissa määrittää useita asetuksia. Pakollinen asetus on "-GatewayType". Kahdenlaisia VPN yhdyskäytävän: Vpn- ja ExpressRoute. 

Kun verkkoliikennettä lähetetään oma yksityinen-yhteyden, voit käyttää yhdyskäytävän tyyppi "ExpressRoute". Tätä kutsutaan myös ExpressRoute-yhdyskäytävä. Kun verkkoliikennettä on lähetetty salattu julkisen yhteyden, käytä yhdyskäytävän tyyppi "Vpn". Tätä kutsutaan nimellä VPN-yhdyskäytävä. Sivuston sivusto-kohdan sivuston ja VNet VNet yhteydet kaikki käyttävät VPN-yhdyskäytävän.

Kunkin virtual verkon voi olla vain yksi VPN-yhdyskäytävän yhdyskäytävän tyyppiä kohden. Esimerkiksi voi olla yksi virtual yhdyskäytävien, joka käyttää - GatewayType ExpressRoute ja, joka käyttää - GatewayType VPN-yhteyttä. Tässä artikkelissa keskitytään ensisijaisesti VPN-yhdyskäytävän. Saat lisätietoja ExpressRoute [ExpressRoute teknisiä tietoja](../expressroute/expressroute-introduction.md).

## <a name="pricing"></a>Hinnat

[AZURE.INCLUDE [vpn-gateway-about-pricing-include](../../includes/vpn-gateway-about-pricing-include.md)] 


## <a name="gateway-skus"></a>Yhdyskäytävän tuotteissa

[AZURE.INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

Saat lisätietoja yhdyskäytävän tuotteissa [Yhdyskäytävän tuotteissa](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

### <a name="estimated-aggregate-throughput-by-sku"></a>Arvioitu kooste siirtonopeuden SKU mukaan

Seuraavassa taulukossa on yhdyskäytävän tyypit ja arvioitu kooste siirtonopeuden. Tässä taulukossa koskee Resurssienhallinta ja perinteinen käyttöönoton mallit.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggthroughput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)] 

## <a name="configuring-a-vpn-gateway"></a>VPN-yhdyskäytävän asetusten määrittäminen

Kun määrität VPN-yhdyskäytävän, ohjeet käytät määräytyvät käyttöönotto-mallin, jota käytit virtual verkoston luominen. Esimerkiksi jos olet luonut oman VNet perinteinen käyttöönotto-mallin, voit ohjeita ja ohjeet perinteinen käyttöönotto-mallin luominen ja VPN-yhdyskäytävän asetusten määrittäminen. Saat lisätietoja käyttöönoton mallien [tietoja Resurssienhallinta ja perinteinen käyttöönoton mallit](../resource-manager-deployment-model.md).

Yhdyskäytävän VPN-yhteyden on riippuvainen useita resursseja, joilla on tiettyjä asetuksia. Useimmat resursseista voi määrittää erikseen, vaikka ne on määritettävä joissakin tapauksissa tietyssä järjestyksessä. Voit aloittaa, luominen ja määrittäminen määritys-työkalulla, kuten Azure portaalin resursseja. Voit sitten myöhemmin päättää siirtyä muuta työkalua, kuten PowerShell, voit määrittää muita resursseja tai muokata aiemmin luotuja resursseja, kun käytettävissä. Ei voi tällä hetkellä määrittää jokaisen resurssikentät ja resurssiasetusta Azure-portaalissa. Ohjeita yhteyden kunkin topologian artikkeleista Määritä tarvittaessa tietty määritys-työkalu. Katso tietoja yksittäisten resurssien ja VPN-yhdyskäytävän asetusten [VPN yhdyskäytävän asetuksia](vpn-gateway-about-vpn-gateway-settings.md).

Seuraavissa osissa on taulukoita, joissa on luettelo:

- käytettävissä olevat käyttöönottomalli
- käytettävissä olevat määritystyökalut
- linkkejä, jotka vievät suoraan artikkeliin, jos se on käytettävissä

Avulla kaavioita ja kuvaukset valitsemalla yhteyden topologian tarpeiden. Kaaviot näyttää tärkeimpien perusaikataulun topologioissa, mutta ei luoda monimutkaisia kaavioita käyttämällä ohjeena määrityksiä.

## <a name="site-to-site-and-multi-site"></a>Sivusto ja usean sivuston

### <a name="site-to-site"></a>Sivusto sivusto

Sivusto (S2S) VPN-yhdyskäytävän yhteys ylittäneen yhteyden IP/IKE (IKEv1 tai IKEv2) VPN-tunnelin. Yhteys edellyttää VPN-laite sijaitsee paikallisen, joka on määritetty julkiseen IP-osoite ja takana tulkintatoiminnon ei tallennettu S2S yhteyksiä voi käyttää rajat paikallinen tai yhdistelmäympäristössä määrityksiä.   

![S2S yhteys] (./media/vpn-gateway-about-vpngateways/demos2s.png "sivuston sivusto")


### <a name="multi-site"></a>Usean sivuston

Voit luoda ja määrittää yhdyskäytävän VPN-yhteyden oman VNet ja useiden paikallisen verkostojen välillä. Kun käsittelet useita yhteyksiä, sinun on käytettävä RouteBased VPN-tyyppi (dynaaminen yhdyskäytävä perinteinen VNets). Koska VNet voi olla vain yksi VPN-yhdyskäytävän, kaikki yhteydet yhdyskäytävän kautta jakaa kaistanleveyttä. Tätä kutsutaan usein "usean sivuston" yhteyden.
 

![Usean sivuston yhteyden] (./media/vpn-gateway-about-vpngateways/demomulti.png "usean sivuston")

### <a name="deployment-models-and-methods-for-site-to-site-and-multi-site"></a>Käyttöönotto-malleja ja sivusto ja usean sivuston

[AZURE.INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)] 

## <a name="vnet-to-vnet"></a>VNet VNet

Yhteyden muodostaminen toiseen virtual virtual verkon (VNet VNet) on samanlainen kuin yhdistäminen VNet paikallisen sivuston sijainnissa. Yhteyksien molempien tietotyyppien käyttää VPN-yhdyskäytävän suojatun tunnelin IP/IKE. Voit myös yhdistää VNet VNet tietoliikenteen usean sivuston yhteyden määrityksiä. Voit vahvistaa verkkotopologioita, joka yhdistää paikallisen-yhteyksien väliset VPN-yhteyttä.

Voit yhdistää VNets voi olla:

- samassa tai eri alueilla
- saman tai toisen-tilauksessa 
- saman eri käyttöönotto-malleissa


![VNet VNet yhteyteen] (./media/vpn-gateway-about-vpngateways/demov2v.png "vnet vnet")

#### <a name="connections-between-deployment-models"></a>Käyttöönoton mallien väliset yhteydet

Azure on kaksi käyttöönoton mallit: perinteinen ja Resurssienhallinta. Jos olet käyttänyt Azure jonkin aikaa, sinulla on luultavasti Azure VMs ja esiintymän roolit perinteinen VNet käynnissä. Uudempaan VMs ja roolin esiintymät saattaa olla käynnissä VNet, joka on luotu resurssien hallinta. Voit luoda sallimaan resurssit yhden VNet viestintään resurssien toisessa VNets välinen yhteys.

#### <a name="vnet-peering"></a>VNet peering

Voit ehkä käyttää VNet peering voit luoda yhteyden, kunhan virtual verkon täyttää tietyt vaatimukset. VNet peering ei käytä VPN-yhdyskäytävä. Lisätietoja on artikkelissa [VNet peering](../virtual-network/virtual-network-peering-overview.md).


### <a name="deployment-models-and-methods-for-vnet-to-vnet"></a>Käyttöönotto-malleja ja VNet VNet menetelmät

[AZURE.INCLUDE [vpn-gateway-table-vnet-to-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 


## <a name="point-to-site"></a>Pisteen sivuston

Pisteen sivuston (P2S) VPN-yhdyskäytävä-yhteyden avulla voit luoda suojatun yhteyden virtual verkkoon yksittäisiä asiakastietokoneesta. P2S on VPN-yhteyden kautta SSTP (Secure Socket Tunneling Protocol). P2S yhteydet, eivät edellytä VPN-laitetta tai julkiselle IP-osoite, jos haluat tutustua. Aloittamalla se asiakastietokone muodostaa VPN-yhteyttä. Tämä ratkaisu on hyötyä, kun haluat muodostaa yhteyttä VNet remote sijainnista, kuten kotona tai konferenssiin, tai jos sinulla on vain muutama asiakkaat, jotka VNet yhdistämiseen. P2S yhteyksiä voidaan yhdessä S2S yhteydet saman VPN-yhdyskäytävän läpi kaikki molemmat yhteydet määritysten vaatimukset ovat yhteensopivia.


![Pisteen sivuston yhteyden] (./media/vpn-gateway-about-vpngateways/demop2s.png "pisteen sivuston")

### <a name="deployment-models-and-methods-for-point-to-site"></a>Käyttöönotto-malleja ja pisteen sivuston menetelmät

[AZURE.INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)] 


## <a name="expressroute"></a>ExpressRoute

[AZURE.INCLUDE [expressroute-intro](../../includes/expressroute-intro-include.md)]

ExpressRoute-yhteyden VPN-yhdyskäytävä on määritetty yhdyskäytävän sisältötyyppi 'ExpressRoute' "Vpn" sijaan. Saat lisätietoja ExpressRoute [ExpressRoute teknisiä tietoja](../expressroute/expressroute-introduction.md).


## <a name="site-to-site-and-expressroute-coexisting-connections"></a>Sivusto ja ExpressRoute samanaikaisesti olemassa olevien yhteydet

ExpressRoute on suora, erillinen yhteyden oman WAN (ei Internetin julkinen) Microsoft Services, mukaan lukien Azure. Sivusto VPN liikenteen Matkalaskut salattu julkisen Internetin välityksellä. Ei voi määrittäminen sivuston sivuston VPN- ja ExpressRoute yhteyksiä samassa virtual verkossa on monia etuja.

Voit määrittää sivuston sivuston VPN ExpressRoute kuin suojatun automaattisesti polku tai sivustoille, jotka eivät kuulu verkkoa, mutta yhdistetyissä kautta ExpressRoute sivusto sivusto VPN-yhteydet avulla. Tämä edellyttää kahta VPN-yhdyskäytävää saman virtual verkon ilmoitus sellaisen noudattamalla - GatewayType Vpn ja toinen - GatewayType ExpressRoute avulla.


![Coexist yhteys] (./media/vpn-gateway-about-vpngateways/demoer.png "expressroute site2site")


### <a name="deployment-models-and-methods-for-s2s-and-expressroute"></a>Käyttöönotto-malleja ja S2S ja ExpressRoute

[AZURE.INCLUDE [vpn-gateway-table-coexist](../../includes/vpn-gateway-table-coexist-include.md)] 


## <a name="next-steps"></a>Seuraavat vaiheet

Suunnittele VPN-yhdyskäytävä-määritys. Katso [VPN yhdyskäytävän suunnittelun ja rakenteen](vpn-gateway-plan-design.md) ja [yhdistämisestä Azure paikallisen verkon](../guidance/guidance-connecting-your-on-premises-network-to-azure.md).








 
