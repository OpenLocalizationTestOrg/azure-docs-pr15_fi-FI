<properties 
   pageTitle="VPN-yhdyskäytävän asetuksista VPN yhdyskäytävien | Microsoft Azure"
   description="Lisätietoja Azure Virtual Network VPN-yhdyskäytävän asetuksia."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="cherylmc" />

# <a name="about-vpn-gateway-settings"></a>VPN-yhdyskäytävän asetuksista

Yhdyskäytävän yhteys VPN-ratkaisua useiden resurssien määrittäminen perustuu voidaksesi lähettää verkkoliikennettä virtual verkkojen välillä ja paikallisen sijainnit. Kullekin resurssille sisältää määritettävät asetukset. Resurssien ja asetusten yhdistelmä määrittää yhteyden ulkoasusta.

Tässä artikkelissa osissa käsitellään, resurssien ja VPN-yhdyskäytävän **Resurssienhallinta** käyttöönoton mallin liittyviä asetuksia. Olet ehkä hyödyllistä voit tarkastella käytettävissä käyttömahdollisuudet yhteyden topologian kaavioiden avulla. Voit etsiä kunkin yhteyden ratkaisu [VPN yhdyskäytävä](vpn-gateway-about-vpngateways.md) on artikkelissa kuvaukset ja topologian kaavioita. 

## <a name="gwtype"></a>Yhdyskäytävän tyypit

Kunkin virtual verkon voi olla vain yksi kunkin VPN-yhdyskäytävä. Varmista luodessasi virtual yhdyskäytävien, että yhdyskäytävän tyyppi on oikea kokoonpanon.

-GatewayType käytettävissä olevat arvot: 

- VPN
- ExpressRoute

VPN-yhdyskäytävän edellyttää, `-GatewayType` *VPN-yhteyttä*.  

Esimerkki:

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
    -Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
    -VpnType RouteBased
 

## <a name="gwsku"></a>Yhdyskäytävän tuotteissa


[AZURE.INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

### <a name="configuring-the-gateway-sku"></a>Määrittää yhdyskäytävän SKU

**Yhdyskäytävän tuote, joka määrittää Azure-portaalissa**

Jos käytät Azure portaalin Resurssienhallinta virtual yhdyskäytävien luominen, voit valita yhdyskäytävän SKU avattavan luettelon avulla. Näyttöön tulee asetukset vastaavat yhdyskäytävän tyyppi ja VPN-tyyppi, joka valitaan.

Esimerkiksi jos valitset yhdyskäytävän tyyppi "VPN- ja VPN tyyppi 'käytäntöön perustuva', näet vain"Perustiedot"tuote koska se on käytettävissä PolicyBased VPN-yhteydet vain tuote. Jos valitset 'Reitti-pohjainen', voit valita Basic, Vakio ja korkean tuotteissa. 


**Yhdyskäytävän tuote, joka määrittää PowerShellin avulla**


Seuraavassa esimerkissä PowerShell määritetään `-GatewaySku` kuin *Vakio*.

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
    -Location 'West US' -IpConfigurations $gwipconfig -GatewaySku Standard `
    -GatewayType Vpn -VpnType RouteBased

**Yhdyskäytävän SKU muuttaminen**

Jos haluat päivittää oman yhdyskäytävän SKU tehokkaampia SKU (korkean Basic/Normaali) voit käyttää `Resize-AzureRmVirtualNetworkGateway` PowerShell cmdlet-komennon. Voit myös aikaisempaan tiedostomuotoon yhdyskäytävän SKU koon tätä cmdlet-komennolla.

Seuraavassa PowerShell-esimerkissä yhdyskäytävän tuote, jonka kokoa muutetaan korkean.

    $gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
    Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance

### <a name="estimated-aggregate-throughput-by-gateway-sku-and-type"></a>Arvioitu kooste siirtonopeuden mukaan yhdyskäytävän tuote- ja laji

Seuraavassa taulukossa on yhdyskäytävän tyypit ja arvioitu kooste siirtonopeuden. Tässä taulukossa koskee Resurssienhallinta ja perinteinen käyttöönoton mallit.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggthroughput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)] 


## <a name="connectiontype"></a>Yhteystyypit

Resurssien hallinnan käyttöönotto-mallissa kunkin määritys edellyttää tietyn virtual verkon yhdyskäytävän yhteyden tyyppi. Käytettävissä olevat Resurssienhallinta PowerShell arvot `-ConnectionType` ovat:

- IP
- Vnet2Vnet
- ExpressRoute
- VPNClient

PowerShellin seuraavassa esimerkissä on yhteyden S2S, joka edellyttää yhteystyyppi *IP*.

    New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
    -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
    -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'


## <a name="vpntype"></a>VPN-tyypit

Kun luot VPN yhdyskäytävän määrityksen VPN-yhdyskäytävän, VPN-tyyppi on määritettävä. VPN-tyyppi, joka on valittava, riippuu siitä, jonka haluat luoda yhteyden topologian. Esimerkiksi P2S yhteyden edellyttää RouteBased VPN-tyyppi. VPN-tyypin voi riippua myös laitteet, jotka aiot käyttää. S2S määrityksiä edellyttää VPN-laite. Jotkin VPN-laitteet tukevat vain tietyt VPN-tyyppi.

Voit valita VPN-tyyppi on täyttävät kaikki yhteyden ratkaisun haluat luoda. Esimerkiksi jos haluat luoda S2S VPN-yhdyskäytävän yhteys ja P2S VPN yhdyskäytävän yhteyden samassa virtual verkossa, vakiomittoja VPN-tyypin *RouteBased* koska P2S vaatii RouteBased VPN-tyyppi. Voit myös on varmistaa, että VPN-laitteesi tukevat RouteBased VPN-yhteyden. 

Kun VPN-yhdyskäytävä on luotu, et voi muuttaa VPN-tyyppi. Sinulla on VPN-yhdyskäytävän ja luoda uuden. Kahdenlaisia VPN:

[AZURE.INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]


Seuraavassa esimerkissä PowerShell määritetään `-VpnType` *RouteBased*nimellä. Varmista luodessasi yhdyskäytävän - VpnType on oikea kokoonpanon. 

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
    -Location 'West US' -IpConfigurations $gwipconfig `
    -GatewayType Vpn -VpnType RouteBased

##  <a name="requirements"></a>Yhdyskäytävän vaatimukset

[AZURE.INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)] 


## <a name="gwsub"></a>Yhdyskäytävän aliverkon

Voit määrittää VPN-yhdyskäytävä-sinun on luoda yhdyskäytävän aliverkon oman VNet. Yhdyskäytävän aliverkon nimen on oltava *GatewaySubnet* toimii oikein. Tämä nimi avulla tiedät, että tämä aliverkon käyttää yhdyskäytävän Azure.

Yhdyskäytävän aliverkon vähimmäiskoko määräytyy määrityksistä, jotka haluat luoda. Se on mahdollista luoda yhdyskäytävän aliverkon /29 mahdollisimman pieni, mutta suosittelemme, että voit luoda yhdyskäytävän aliverkon /28 tai suurempi (/ 28, /27, /26, jne.). 

Yhdyskäytävän suurentaminen luominen estää suorittamasta vastaan yhdyskäytävän kokorajoitukset. Esimerkiksi luomiisi VPN-yhdyskäytävän S2S yhteyden yhdyskäytävän aliverkon kokoisena-/29. Nyt voit määrittää S2S/ExpressRoute haluat olla määritykset. Kyseisen määritys edellyttää yhdyskäytävän aliverkon vähimmäiskoko /28. Luo kokoonpanosi joutua muokkaamaan yhdyskäytävän aliverkon sopimaan yhteys, joka on /28 vähimmäisvaatimus.

Seuraavassa Resurssienhallinta PowerShell-esimerkissä yhdyskäytävän aliverkon nimeltä GatewaySubnet. Voit tarkastella CIDR-merkintätapaa määrittää /27, joka mahdollistaa tarpeeksi useimmat käyttömahdollisuudet tällä hetkellä käytettävissä olevia IP-osoitteet.

    Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)] 


## <a name="lng"></a>Paikalliseen verkkoon kirjauduttaessa yhdyskäytävät

VPN-yhdyskäytävä-määritysten luotaessa paikalliseen verkkoon kirjauduttaessa yhdyskäytävän edustaa usein paikallisen sijainti. Perinteinen käyttöönotto-mallin paikalliseen verkkoon kirjauduttaessa yhdyskäytävä on kutsutaan paikallisen sivuston. 

Anna lähiverkon yhdyskäytävän nimi, paikallisen VPN-laite julkiseen IP-osoite ja määritä osoitteiden etuliitteiden, jotka sijaitsevat paikallisen sijainti. Azure tarkastelee kohde osoitteiden etuliitteiden verkkoliikenteelle, kuulee määrityksistä, jotka olet määrittänyt paikallisen yhdyskäytävien ja reitittää paketit vastaavasti. Voit myös määrittää paikallisen verkon IP-osoitteiden VNet VNet määritykset, käyttää yhdyskäytävän VPN-yhteyden.

PowerShellin seuraavassa esimerkissä luodaan uusi paikallinen yhdyskäytävien:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'

Joskus tarvitset paikallisen verkon yhdyskäytävän asetuksia. Esimerkiksi kun lisäät tai muutat osoite-arvo, tai jos VPN-laitteen IP-osoite muuttuu. Perinteinen VNet voit muuttaa näitä asetuksia perinteinen portaalissa paikallisen verkot-sivulla. Resurssin Manageria varten on artikkelissa [Muokkaa paikalliseen verkkoon kirjauduttaessa yhdyskäytävän asetusten PowerShellin avulla](vpn-gateway-modify-local-network-gateway.md).

## <a name="resources"></a>REST API ja PowerShell cmdlet-komennot

Tekniset lisäresursseja ja tietyn syntaksivaatimukset käytettäessä REST API ja PowerShellin cmdlet-komennot VPN-yhdyskäytävä-määrityksiä varten on artikkelissa seuraavilla sivuilla:

|**Perinteinen** | **Resurssien hallinta**|
|-----|----|
|[PowerShellin](https://msdn.microsoft.com/library/mt270335.aspx)|[PowerShellin](https://msdn.microsoft.com/library/mt163510.aspx)|
|[REST-OHJELMOINTIRAJAPINNALLA](https://msdn.microsoft.com/library/jj154113.aspx)|[REST-OHJELMOINTIRAJAPINNALLA](https://msdn.microsoft.com/library/mt163859.aspx)|


## <a name="next-steps"></a>Seuraavat vaiheet

Katso lisätietoja käytettävissä yhteyden määrityksiä [VPN yhdyskäytävän](vpn-gateway-about-vpngateways.md) . 







 
