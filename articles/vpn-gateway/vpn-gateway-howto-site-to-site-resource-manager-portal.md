<properties
   pageTitle="Luo virtuaalisia verkko sivusto sivusto VPN-yhteyttä käyttämällä Azure Resurssienhallinta ja Azure-portaalin | Microsoft Azure"
   description="Voit luoda VNet mallin resurssien hallinnan käyttöönotto ja muodostaa yhteyttä paikalliseen paikalliset verkon yhdyskäytävän S2S VPN-yhteyden avulla."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="cherylmc"/>

# <a name="create-a-vnet-with-a-site-to-site-connection-using-the-azure-portal"></a>Luo VNet sivuston sivuston yhteyden Azure-portaalissa

> [AZURE.SELECTOR]
- [Resurssien hallinta - portaalissa Azure](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Resurssien hallinta - PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
- [Perinteinen - perinteinen Portal](vpn-gateway-site-to-site-create.md)


Tässä artikkelissa käydään läpi luominen virtual verkko- ja sivusto sivusto VPN yhdyskäytävän yhteys paikalliseen verkkoon Azure resurssien hallinnan käyttöönottomalli ja Azure-portaalin käyttäminen. Sivuston sivuston yhteydet voi käyttää rajat paikallinen tai yhdistelmäympäristössä määrityksiä.

![Kaavio](./media/vpn-gateway-howto-site-to-site-resource-manager-portal/s2srmportal.png)


### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Käyttöönotto-malleja ja viestintämenetelmien sivuston sivuston yhteydet

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Seuraavassa taulukossa on tällä hetkellä käytettävissä käyttöönoton mallit ja viestintämenetelmien sivusto määrityksiä. Kun määritysvaiheet artikkeli on käytettävissä, on linkki suoraan tästä taulukosta.

[AZURE.INCLUDE [site-to-site table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Lisämäärityksiä 

Jos haluat yhdistää VNets yhteen, mutta yhteys paikalliseen sijaintiin ei luodaan, katso [VNet VNet yhteyden määrittäminen](vpn-gateway-vnet-vnet-rm-ps.md). Jos haluat lisätä sivuston sivuston yhteyden VNet, jossa on jo yhteyden, on [Lisää VNet aiemmin VPN-yhdyskäytävän yhteys S2S yhteyden](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md).

## <a name="before-you-begin"></a>Ennen aloittamista

Tarkista, pidä seuraavat asiat ennen aloittamista kokoonpanosi:

- Yhteensopivat VPN-laite ja henkilölle, joka ei voi määrittää sen. Lisätietoja on artikkelissa [VPN-laitteiden tietoja](vpn-gateway-about-vpn-devices.md). Vaikka et olisikaan tuttuja määrittäminen VPN-laite- tai tunne alueiden sijaitsevat IP-osoite paikallisen oman verkon määritysten, sinulla on oltava henkilön kanssa, jolla voit antaa näiden tietojen järjestämiseen.

- Ulkoisesti aukeaman julkiseen IP-osoite VPN-laitteeseesi. Tämä IP-osoite ei löydy, takana tulkintatoiminnon
    
- Azure tilaus. Jos sinulla ei vielä ole Azure tilaus, voit ottaa [MSDN tilaajan edut](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) tai sisäänkirjautumisen määrittäminen [ilmainen tili](http://azure.microsoft.com/pricing/free-trial/).

### <a name="values"></a>Määritysten esimerkkiarvojen tämän Harjoitus


Käytä näitä vaiheita hioa, voit esimerkkiarvojen määritykset:

- **VNet nimi:** TestVNet1
- **Osoite tila:** 10.11.0.0/16 ja 10.12.0.0/16
- **Aliverkosta:**
    - FrontEnd: 10.11.0.0/24
    - Taustajärjestelmä: 10.12.0.0/24
    - GatewaySubnet: 10.12.255.0/27
- **Resurssiryhmä:** TestRG1
- **Sijainti:** Yhdysvaltojen Itä
- **DNS-palvelin:** 8.8.8.8
- **Yhdyskäytävän nimi:** VNet1GW
- **Julkiseen IP:** VNet1GWIP
- **VPN-tyyppi:** Reititys-pohjainen
- **Yhteyden tyyppi:** Sivusto (IP)
- **Yhdyskäytävän tyyppi:** VPN
- **Paikalliseen verkkoon kirjauduttaessa yhdyskäytävän nimi:** Sivusto2
- **Yhteyden nimi:** VNet1toSite2


## <a name="CreatVNet"></a>1. virtual verkoston luominen 

Jos sinulla on jo VNet, varmista, että asetukset ovat yhteensopivia VPN-yhdyskäytävän rakenne. Kiinnitä erityistä huomiota aliverkosta, jotka voivat olla muiden verkkojen. Jos sinulla on päällekkäisiä aliverkosta, yhteys ei toimi oikein. Jos yhteyttä VNet on määritetty oikein asetukset, voit aloittaa [Määritä DNS-palvelin](#dns) -osion ohjeita.

### <a name="to-create-a-virtual-network"></a>Virtuaalinen verkoston luominen

[AZURE.INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]  

## <a name="subnets"></a>2. Lisää muita osoitetilan ja aliverkosta

Voit lisätä muita osoitetilan ja aliverkosta oman VNet, kun se on luotu.

[AZURE.INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)] 

## <a name="dns"></a>3. Määritä DNS-palvelin

### <a name="to-specify-a-dns-server"></a>DNS-palvelin

[AZURE.INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="gatewaysubnet"></a>4. yhdyskäytävän aliverkon luominen

Ennen yhteyden muodostamista virtual verkon yhdyskäytävän, sinun on luoda yhdyskäytävän aliverkon, johon haluat muodostaa virtual verkossa. Jos mahdollista kannattaa luoda yhdyskäytävän aliverkon käyttämän CIDR joukon /28 tai /27 sopimaan tulevien lisämäärityksiä vaatimukset tarpeeksi IP-osoitteet.

Jos luot määritysten hioa nimellä, tarkista nämä [arvot](#values) yhdyskäytävän aliverkon luotaessa.

### <a name="to-create-a-gateway-subnet"></a>Voit luoda yhdyskäytävän aliverkon


[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="VNetGateway"></a>5. VPN Gatewayn luominen

Jos olet luomassa määritysten kuin hioa, voit viitata [otoksen määritysten arvot](#values).

### <a name="to-create-a-virtual-network-gateway"></a>Voit luoda virtuaalisen yhdyskäytävien

[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="LocalNetworkGateway"></a>6. paikalliseen verkkoon kirjauduttaessa Gatewayn luominen

Paikalliseen verkkoon kirjauduttaessa yhdyskäytävän viittaa paikallisen sijainti. Anna lähiverkon yhdyskäytävän nimi, jonka Azure viitata siihen. 

Jos olet luomassa määritysten kuin hioa, voit viitata [otoksen määritysten arvot](#values).

### <a name="to-create-a-local-network-gateway"></a>Voit luoda paikallisen yhdyskäytävien

[AZURE.INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]

## <a name="VPNDevice"></a>7. VPN-laitteen määrittäminen

[AZURE.INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

## <a name="CreateConnection"></a>8. sivusto sivusto VPN-yhteyden luominen

Luo sivusto VPN-yhteyden VPN-yhdyskäytävän ja VPN-laitteen välillä. Varmista, arvojen korvaaminen omalla. Jaetun avaimen on vastattava VPN-laitteen määrittäminen käytettävä arvo. 

Ennen aloittamista sisältö, varmista, että virtual yhdyskäytävien ja paikallisen verkon yhdyskäytävien luonut. Jos luot määritysten hioa nimellä, tarkista nämä [arvot](#values) yhteyden luomiseen.

### <a name="to-create-the-vpn-connection"></a>VPN-yhteyden luominen


[AZURE.INCLUDE [vpn-gateway-add-site-to-site-connection-rm-portal](../../includes/vpn-gateway-add-site-to-site-connection-rm-portal-include.md)]

## <a name="VerifyConnection"></a>9. Tarkista VPN-yhteys

Voit tarkistaa VPN-yhteyden portaalissa tai PowerShell-toiminnolla.

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## <a name="next-steps"></a>Seuraavat vaiheet

- Kun yhteys on valmis, voit lisätä näennäiskoneiden virtual verkkoihin. Katso näennäiskoneiden [oppimispolku](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) lisätietoja.

- Saat lisätietoja erityisen [Erityisen yleiskatsaus](vpn-gateway-bgp-overview.md) ja [erityisen määrittämisestä](vpn-gateway-bgp-resource-manager-ps.md).
