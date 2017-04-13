<properties
   pageTitle="Luo virtuaalisia verkko sivusto sivusto VPN-yhteyttä käyttämällä Azure Resurssienhallinta ja PowerShell | Microsoft Azure"
   description="Tässä artikkelissa esitellään luominen VNet käyttämällä resurssien hallinnan käyttöönottomalli ja yhteyden muodostaminen paikalliseen paikalliseen verkkoon yhdyskäytävän S2S VPN-yhteyden avulla."
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

# <a name="create-a-vnet-with-a-site-to-site-connection-using-powershell"></a>Luo VNet PowerShell-sivusto sivusto-yhteys

> [AZURE.SELECTOR]
- [Resurssien hallinta - portaalissa Azure](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Resurssien hallinta - PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
- [Perinteinen - perinteinen Portal](vpn-gateway-site-to-site-create.md)

Tässä artikkelissa käydään läpi luominen virtual verkko- ja sivusto sivusto VPN yhdyskäytävän yhteys Azure Resurssienhallinta käyttöönoton mallin paikalliseen verkkoon. Sivuston sivuston yhteydet voi käyttää rajat paikallinen tai yhdistelmäympäristössä määrityksiä.

![Sivusto kaavio] (./media/vpn-gateway-create-site-to-site-rm-powershell/s2srmps.png "sivuston sivusto") 


### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Käyttöönotto-malleja ja viestintämenetelmien sivuston sivuston yhteydet

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Seuraavassa taulukossa on tällä hetkellä käytettävissä käyttöönoton mallit ja viestintämenetelmien sivusto määrityksiä. Kun määritysvaiheet artikkeli on käytettävissä, on linkki suoraan tästä taulukosta. 

[AZURE.INCLUDE [site-to-site table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Lisämäärityksiä

Jos haluat yhdistää VNets yhteen, mutta yhteys paikalliseen sijaintiin ei luodaan, katso [VNet VNet yhteyden määrittäminen](vpn-gateway-vnet-vnet-rm-ps.md). Jos haluat lisätä sivuston sivuston yhteyden VNet, jossa on jo yhteyden, on [Lisää VNet aiemmin VPN-yhdyskäytävän yhteys S2S yhteyden](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md).

## <a name="before-you-begin"></a>Ennen aloittamista

Tarkista, pidä seuraavat asiat ennen aloittamista määritys.

- Yhteensopivat VPN-laite ja henkilölle, joka ei voi määrittää sen. Lisätietoja on artikkelissa [VPN-laitteiden tietoja](vpn-gateway-about-vpn-devices.md). Vaikka et olisikaan tuttuja määrittäminen VPN-laite- tai tunne alueiden sijaitsevat IP-osoite paikallisen oman verkon määritysten, sinulla on oltava henkilön kanssa, jolla voit antaa näiden tietojen järjestämiseen.

- Ulkoisesti aukeaman julkiseen IP-osoite VPN-laitteeseesi. Tämä IP-osoite ei löydy, takana tulkintatoiminnon
    
- Azure tilaus. Jos sinulla ei vielä ole Azure tilaus, voit ottaa [MSDN tilaajan edut](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) tai sisäänkirjautumisen määrittäminen [ilmainen tili](https://azure.microsoft.com/pricing/free-trial/).
    
- Azure Resurssienhallinta PowerShellin cmdlet-komennot uusimman version. Katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md) lisätietoja asentaminen PowerShellin cmdlet-komennot.


## <a name="Login"></a>1. tilauksen yhdistäminen 

Varmista, että voit siirtyä PowerShell-tilassa voit resurssien hallinnan Cmdlet-komentoja. Lisätietoja on artikkelissa [Windows PowerShellin resurssien hallinta](../powershell-azure-resource-manager.md).

Avaa PowerShell-konsolin ja muodostaa yhteyden tiliisi. Seuraavassa esimerkissä avulla voit tarkastella:

    Login-AzureRmAccount

Tarkista tilaukset-tilin.

    Get-AzureRmSubscription 

Määritä tilaus, jota haluat käyttää.

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

## <a name="VNet"></a>2. virtual verkon ja yhdyskäytävän aliverkon luominen

Esimerkeissä käytetään yhdyskäytävän aliverkon, /28. Vaikka ei voi luoda yhdyskäytävän aliverkon /29 mahdollisimman pieni, on suositeltavaa, että luot suurempi aliverkon, joka sisältää useita osoitteita valitsemalla vähintään /28 tai /27. Tämä sallii tarpeeksi osoitteiden sopimaan mahdollista lisämäärityksiä, haluat ehkä myöhemmin.

Jos sinulla on jo virtual verkossa, jossa on yhdyskäytävän aliverkon/29 tai suurempi, voit siirtyä suoraan [lisääminen paikalliseen verkkoon kirjauduttaessa-yhdyskäytävä](#localnet).


[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]  

### <a name="to-create-a-virtual-network-and-a-gateway-subnet"></a>Luo virtuaalisia verkko- ja yhdyskäytävän aliverkon

Seuraavassa esimerkissä avulla voit luoda virtuaalisen verkko- ja yhdyskäytävän aliverkon. Korvaa arvot itse. 

Luo ensin resurssiryhmä:
    
    New-AzureRmResourceGroup -Name testrg -Location 'West US'

Seuraavaksi luodaan virtual verkossa. Varmista, että määrität osoite välilyöntejä ole päällekkäin jonkin osoite välilyöntejä, joihin sinulla on paikallisen verkossa.

Seuraava esimerkki luo *testvnet* ja kahden aliverkon, eli *GatewaySubnet* virtual verkon ja toinen nimeltään *Subnet1*. On tärkeää yhden aliverkon nimeltä erityisesti *GatewaySubnet*luomiseen. Jos nimeät sen jotain muuta, että yhteysmäärityksen epäonnistuu. 

Määritä muuttujat.

    $subnet1 = New-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.0.0/28
    $subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name 'Subnet1' -AddressPrefix '10.0.1.0/28'

Luo VNet.

    New-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg `
    -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $subnet1, $subnet2

### <a name="gatewaysubnet"></a>Voit lisätä yhdyskäytävän aliverkon olet jo luonut virtual verkkoon

Tämä vaihe on pakollinen vain, jos haluat lisätä yhdyskäytävän aliverkon VNet aiemmin luomasi.

Voit luoda yhdyskäytävän aliverkon käyttämällä seuraavassa esimerkissä. Muista yhdyskäytävän aliverkon 'GatewaySubnet' nimi. Jos nimeät sen jotain muuta, voit luoda aliverkon, mutta Azure ei käsitellä sitä yhdyskäytävän aliverkon.

Määritä muuttujat.

    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName testrg -Name testvnet

Voit luoda yhdyskäytävän aliverkon.

    Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/28 -VirtualNetwork $vnet

Määritä asetukset. 

    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

## 3. <a name="localnet"> </a>lisääminen paikalliseen verkkoon kirjauduttaessa yhdyskäytävän

Virtual verkon paikalliseen verkkoon kirjauduttaessa yhdyskäytävän tarkoittaa yleensä paikallisen sijainti. Voit tarkastella sivuston nimi, jolla Azure voivat siihen viitataan ja määritä myös paikalliseen verkkoon kirjauduttaessa yhdyskäytävän osoite tilaa etuliite. 

Azure käyttää IP-osoitteiden etuliitettä määrittämäsi tunnistavan liikenne lähettää paikallisen sijainti. Tämä tarkoittaa, että sinun on määritettävä jokaisen osoitteen etuliitettä, jonka haluat yhdistetä lähiverkossa-yhdyskäytävä. Voit helposti päivittää seuraavia etuliitteitä paikallisen verkon muuttuessa. 

PowerShell-esimerkkejä käytettäessä Huomautus seuraavasti:
    
- *GatewayIPAddress* on paikallisen VPN-laitteen IP-osoite. VPN-laite ei löydy, takana tulkintatoiminnon 
- *AddressPrefix* on paikallisen osoitetilaa.

Voit lisätä yksittäisen osoitteen etuliite paikalliseen verkkoon kirjauduttaessa Gatewaylle seuraavasti:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'

Voit lisätä useita osoitteiden etuliitteiden paikalliseen verkkoon kirjauduttaessa Gatewaylle seuraavasti:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix @('10.0.0.0/24','20.0.0.0/24')

### <a name="to-modify-ip-address-prefixes-for-your-local-network-gateway"></a>Jos haluat muokata IP-osoitteiden etuliitteiden paikalliseen verkkoon kirjauduttaessa Gateway

Lähiverkossa-yhdyskäytävän etuliitteiden muuttua joskus. Voit muokata IP-osoitteiden etuliitteiden vaiheita määräytyvät sen mukaan, onko luomasi yhdyskäytävän VPN-yhteyden. Tämän artikkelin kohdassa [Muokkaa IP-osoitteiden etuliitteiden paikalliseen verkkoon kirjauduttaessa yhdyskäytävän](#modify) .


## <a name="PublicIP"></a>4. pyytää VPN-yhdyskäytävän julkiseen IP-osoite

Pyydä julkinen IP-osoite, jonka haluat kohdentaa Azure VNet VPN-yhdyskäytävän. Tämä ei ole sama IP-osoite, joka on määritetty VPN-laite. sen sijaan, että se on määritetty Azure VPN-yhdyskäytävän itse. Et voi määrittää IP-osoite, jota haluat käyttää. Se on kohdistettu dynaamisesti käyttämäsi yhdyskäytävän. IP-osoitteen käyttäminen määritettäessä paikallisen VPN-laite muodostaa yhdyskäytävän.

Tällä hetkellä Resurssienhallinta käyttöönoton mallille Azure VPN-yhdyskäytävän tukee vain julkiseen IP-osoitteiden dynaamisen kohdistus-menetelmällä. Tämä ei kuitenkaan tarkoita IP-osoite muuttuu. Ainoastaan Azure VPN yhdyskäytävän IP-osoitteen muuttumisen on kun yhdyskäytävän poistetaan ja luodaan uudelleen. Yhdyskäytävän julkiseen IP-osoite ei muuta kokoa, palauttaminen tai muut sisäiset ylläpito/päivitykset Azure VPN-yhdyskäytävän yli.

Käytä PowerShell seuraavassa esimerkissä:

    $gwpip= New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg -Location 'West US' -AllocationMethod Dynamic

## <a name="GatewayIPConfig"></a>5. yhdyskäytävän IP-osoitteiden määrityksen luominen

Yhdyskäytävän määrittäminen määrittää aliverkon ja käyttämään julkiseen IP-osoite. Seuraavassa esimerkissä avulla voit luoda yhdyskäytävän määritykset.

    $vnet = Get-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
    $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name gwipconfig1 -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id 

## <a name="CreateGateway"></a>6. VPN-Gatewayn luominen

Tässä vaiheessa voit luoda VPN-yhdyskäytävä. Yhdyskäytävän luominen voi kestää kauan suorittamiseen. Vähintään usein 45 minuuttia. 

Käytä seuraavia arvoja:

- *-GatewayType* sivusto sivusto määrittämisessä on *VPN-yhteyttä*. Yhdyskäytävän tyyppi on aina määrityksistä, jotka ovat käyttöönoton. Esimerkiksi yhdyskäytävän määrityksiä voi vaatia - GatewayType ExpressRoute. 

- *-VpnType* voi olla *RouteBased* (jota kutsutaan aineistoa dynaaminen yhdyskäytäviä) tai *PolicyBased* (jota kutsutaan aineistoa staattinen yhdyskäytäviä). Saat lisätietoja VPN yhdyskäytävän tyypeistä [VPN yhdyskäytävät](vpn-gateway-about-vpngateways.md#vpntype).
- *-GatewaySku* voi olla *Basic*, *Normaali*tai *korkean*.   

        New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
        -Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
        -VpnType RouteBased -GatewaySku Standard

## <a name="ConfigureVPNDevice"></a>7. VPN-laitteen määrittäminen

Tässä vaiheessa sinun on VPN-yhdyskäytävän julkiseen IP-osoitteen määrittämisestä paikallisen VPN-laite. Käsittele tiettyä kokoonpanotietoja laitteen valmistajalta. Voit viitata lisätietoja [VPN-laitteet](vpn-gateway-about-vpn-devices.md) .

Voit hakea VPN-yhdyskäytävän julkiseen IP-osoitteen avulla seuraavassa esimerkissä:

    Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg

## <a name="CreateConnection"></a>8. VPN-yhteyden luominen

Seuraavaksi luodaan sivusto sivusto VPN-yhteyden VPN-yhdyskäytävän ja VPN-laitteen välillä. Varmista, arvojen korvaaminen omalla. Jaetun avaimen on vastattava VPN-laitteen määrittäminen käytettävä arvo. Huomaa, `-ConnectionType` sivusto sivusto on *IP*. 

Määritä muuttujat.

    $gateway1 = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
    $local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg

Yhteyden muodostaminen.

    New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
    -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
    -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

Jälkeen lyhyt, kun yhteys muodostetaan. 

## <a name="toverify"></a>Tarkista VPN-yhteyden

Jakautuvat muutamalla eri tavalla, voit tarkistaa VPN-yhteyden.

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## <a name="modify"></a>Jos haluat muokata IP-osoitteiden etuliitteiden paikalliseen verkkoon kirjauduttaessa yhdyskäytävän

Jos haluat muuttaa paikalliseen verkkoon kirjauduttaessa Gatewayn etuliitteiden, käytä seuraavien ohjeiden mukaisesti. Kaksi joukkoa ohjeet toimitetaan. Voit valita ohjeita määräytyvät sen mukaan, jonka olet jo luonut yhdyskäytävän yhteys. 

[AZURE.INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="modifygwipaddress"></a>Jos haluat muokata paikallisen yhdyskäytävien yhdyskäytävän IP-osoite

[AZURE.INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Seuraavat vaiheet

- Voit lisätä näennäiskoneiden virtual verkkoihin. Lisätietoja on kohdassa [Create Virtual Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md) ohjeita.

- Saat lisätietoja erityisen [Erityisen yleiskatsaus](vpn-gateway-bgp-overview.md) ja [erityisen määrittämisestä](vpn-gateway-bgp-resource-manager-ps.md).

