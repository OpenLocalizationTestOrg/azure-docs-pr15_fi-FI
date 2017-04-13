<properties 
   pageTitle="Määritä sivusto yhteyksien resurssien hallinnan käyttöönottomalli pakotettu tunneloinnin | Microsoft Azure"
   description="Voit ohjata tai "pakottaa" kaikki sidottujen Internet-liikenne takaisin paikalliseen sijaintiin."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/10/2016"
   ms.author="cherylmc" />

# <a name="configure-forced-tunneling-using-the-azure-resource-manager-deployment-model"></a>Määritä pakotettu tunneling käyttämällä Azure resurssien hallinnan käyttöönottomalli

> [AZURE.SELECTOR]
- [PowerShell – perinteinen](vpn-gateway-about-forced-tunneling.md)
- [PowerShell - resurssien hallinta](vpn-gateway-forced-tunneling-rm.md)

Pakotetun tunneling avulla voit uudelleenohjata tai "voimassa" kaikki sidottujen Internet-liikenne takaisin paikalliseen sijaintisi kautta sivusto sivusto VPN-tunneli tarkastus ja valvonta. Tämä on useimmissa yrityksen IT kriittinen vaatimus käytännöt.

Ilman pakotettu tunneling Internet sidottujen tietoliikenteen oman VMs Azure-tietokannassa aina käy läpi Azure verkkoinfrastruktuuria suoraan-Internet-asetuksen avulla voit tutkia tai valvonta liikenne ilman. Internet-luvattomasti voi aiheuttaa mahdollisesti tietojen paljastaminen tai muun tyyppisiä suojauksen ilmeisesti

Tässä artikkelissa käydään läpi määrittäminen pakotettu tunneling virtual verkkojen luotu resurssien hallinnan käyttöönottomalli.

**Tietoja malleista Azure käyttöönotto**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

**Käyttöönotto-malleja ja pakotettu tunneloinnissa Työkalut**

Pakotetun tunneloinnin yhteyden voi määrittää perinteinen käyttöönoton mallia ja resurssien hallinnan käyttöönottomalli. Katso lisätietoja seuraavassa taulukossa. Päivitämme tässä taulukossa, kun uusia artikkeleita, uudet käyttöönoton mallit ja muita työkaluja palvelupakettiisi määritysten. Artikkelin ollessa käytettävissä on linkki suoraan taulukosta.

[AZURE.INCLUDE [vpn-gateway-table-forced-tunneling](../../includes/vpn-gateway-table-forcedtunnel-include.md)] 


## <a name="about-forced-tunneling"></a>Pakotettu noin tunneling


Seuraavassa kaaviossa on kuvattu miten pakotettu tunneloinnin toimii. 

![Pakotettu Tunneling](./media/vpn-gateway-forced-tunneling-rm/forced-tunnel.png)

Yllä olevassa esimerkissä aliverkon ei ole pakotettu edusta tunneloidun. Voit hyväksyä ja vastata Internetistä suoraan jatkaa edusta-aliverkon työtaakkaa. Keskellä tason ja Taustajärjestelmä aliverkosta pakotetaan Tunneloitujen. Kaikki lähtevät yhteydet näiden kahden aliverkon Internet on pakotettu tai ohjataan takaisin paikalliseen-sivuston kautta jokin S2S VPN-tunneleita.

Näin voit rajoittaa ja tarkista Internet-yhteyttä, että näennäiskoneiden- tai cloud Services-palvelujen Azure-samalla, kun käyttöön oman monitasoisten arkkitehtuuri pakollinen. Voit myös käyttää pakotettu tunneling koko virtual verkkoihin Jos ei ole Internetiin yhteydessä oleva työmääriä virtual lopettaminen.

## <a name="requirements-and-considerations"></a>Vaatimukset ja huomioon otettavia seikkoja

Pakotetun tunneling Azure-tietokannassa on määritetty VPN käyttäjän määrittämät tiet kautta. Azure VPN-yhdyskäytävän oletusarvon reitin ilmaistaan uudelleenohjausta liikenne paikalliseen-sivustoon. Saat lisätietoja käyttäjän määrittämät reititys ja virtual verkkojen [käyttäjän määrittämät tiet ja IP-välityksen](../virtual-network/virtual-networks-udr-overview.md).

- Kunkin VPN-aliverkon on valmis, järjestelmän reititys taulukko. Järjestelmä reititys-taulukko on tiet seuraavasta kolmesta ryhmästä:

    - **Paikallisen VNet reitittää:** Suoraan kohteeseen VMs virtual samassa verkostossa
    
    - **Paikallisen tiet:** Azure VPN Gateway
    
    - **Oletusarvon reitin:** Yhteys Internetiin. Yksityisen IP-osoitteiden ei koske edellisen kaksi tiet pakettien poistetaan.

-  Tämä toiminto käyttää käyttäjän määrittämät tiet (UDR) reititys taulukon oletusarvon reititys ja liittää sitten reititys taulukon luominen VNet-subnet(s) käyttöön kyseiset aliverkosta pakotettu tunneling.

- Pakotetun tunneling on oltava yhdistettynä VNet, jossa on reitti-pohjainen VPN-yhdyskäytävän. Sinun on määritettävä "oletussivusto" kesken paikallisen-paikalliset sivustot virtual verkkoyhteyttä.

- Pakotettu tunneling ExpressRoute ei ole määritetty tämän järjestelmän kautta, mutta sen sijaan on käytössä, mainonta oletusarvon reitin ExpressRoute erityisen peering istunnot kautta. Katso lisätietoja [ExpressRoute ohjeissa](https://azure.microsoft.com/documentation/services/expressroute/) .

## <a name="configuration-overview"></a>Yleistä

Seuraavien ohjeiden avulla voit luoda resurssiryhmä ja VNet. Sitten VPN-yhdyskäytävän Luo ja määritä pakotettu tunneling. Tässä toimintosarjassa virtual verkon "MultiTier VNet" on 3 aliverkosta: *Frontend*, *Midtier*ja *taustatietokannan*, 4 rajat paikallisen yhteydet: *DefaultSiteHQ*ja 3 *haaroja*.

Prosessin vaiheiden määrittäminen *DefaultSiteHQ* oletusarvon sivuston yhteys pakotettu tunneling ja määrittää Midtier ja Taustajärjestelmä aliverkosta käyttämään pakotettu tunneling.

    
## <a name="before-you-begin"></a>Ennen aloittamista

Tarkista, pidä seuraavat asiat ennen aloittamista kokoonpanosi.

- Azure tilaus. Jos sinulla ei vielä ole Azure tilaus, voit ottaa [MSDN tilaajan edut](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) tai sisäänkirjautumisen määrittäminen [ilmainen tili](https://azure.microsoft.com/pricing/free-trial/).

- Tarvitset asenna uusin versio Azure Resurssienhallinta PowerShellin cmdlet-komennot (1.0 tai uudempi). Katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md) lisätietoja asentaminen PowerShellin cmdlet-komennot.


## <a name="configure-forced-tunneling"></a>Määritä pakotettu tunneling

1. PowerShell-konsolissa kirjautua Azure-tili. Tämä cmdlet-komento kysyy kirjautumisen tunnistetietoja Azure-tiliin. Jälkeen kirjautumisesta, se lataa-käyttäjätilin asetusten, jotta ne ovat käytettävissä PowerShellin Azure.

        Login-AzureRmAccount 

2. Saat luettelon käytettävissä Azure tilaukset.

        Get-AzureRmSubscription

2. Määritä tilaus, jota haluat käyttää. 

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
        
3. Luo resurssiryhmä.

        New-AzureRmResourceGroup -Name "ForcedTunneling" -Location "North Europe"

4. Luo virtuaalisia verkon ja määritä aliverkosta. 

        $s1 = New-AzureRmVirtualNetworkSubnetConfig -Name "Frontend" -AddressPrefix "10.1.0.0/24"
        $s2 = New-AzureRmVirtualNetworkSubnetConfig -Name "Midtier" -AddressPrefix "10.1.1.0/24"
        $s3 = New-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -AddressPrefix "10.1.2.0/24"
        $s4 = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix "10.1.200.0/28"
        $vnet = New-AzureRmVirtualNetwork -Name "MultiTier-VNet" -Location "North Europe" -ResourceGroupName "ForcedTunneling" -AddressPrefix "10.1.0.0/16" -Subnet $s1,$s2,$s3,$s4

5. Luo paikalliseen verkkoon kirjauduttaessa yhdyskäytävät.

        $lng1 = New-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.111" -AddressPrefix "192.168.1.0/24"
        $lng2 = New-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.112" -AddressPrefix "192.168.2.0/24"
        $lng3 = New-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.113" -AddressPrefix "192.168.3.0/24"
        $lng4 = New-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.114" -AddressPrefix "192.168.4.0/24"
        
6. Reititystaulukon ja reitin säännön luominen

        New-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" –Location "North Europe"
        $rt = Get-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" 
        Add-AzureRmRouteConfig -Name "DefaultRoute" -AddressPrefix "0.0.0.0/0" -NextHopType VirtualNetworkGateway -RouteTable $rt
        Set-AzureRmRouteTable -RouteTable $rt


7. Liitä Midtier ja Taustajärjestelmä aliverkosta reititystaulukon.

        $vnet = Get-AzureRmVirtualNetwork -Name "MultiTier-Vnet" -ResourceGroupName "ForcedTunneling"
        Set-AzureRmVirtualNetworkSubnetConfig -Name "MidTier" -VirtualNetwork $vnet -AddressPrefix "10.1.1.0/24" -RouteTable $rt
        Set-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -VirtualNetwork $vnet -AddressPrefix "10.1.2.0/24" -RouteTable $rt
        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

8. Luo yhdyskäytävä oletussivusto. Tämä vaihe kestää jonkin aikaa, viimeistele vähintään joskus 45 minuuttia, koska aiot luoda ja määrittää yhdyskäytävän.<br> `-GatewayDefaultSite` On cmdlet-parametri, joka mahdollistaa pakotettu reititys määrittämisen toimi, niin huolehtia määrittäminen tätä asetusta oikein. Tämä parametri on käytettävissä PowerShell 1.0 tai uudempi versio.

        $pip = New-AzureRmPublicIpAddress -Name "GatewayIP" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -AllocationMethod Dynamic
        $gwsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
        $ipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "gwIpConfig" -SubnetId $gwsubnet.Id -PublicIpAddressId $pip.Id
        New-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -IpConfigurations $ipconfig -GatewayType Vpn -VpnType RouteBased -GatewayDefaultSite $lng1 -EnableBgp $false

9. Muodostaa sivuston sivuston VPN-yhteydet.

        $gateway = Get-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling"
        $lng1 = Get-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" 
        $lng2 = Get-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" 
        $lng3 = Get-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" 
        $lng4 = Get-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" 

        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng1 -ConnectionType IPsec -SharedKey "preSharedKey"
        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng2 -ConnectionType IPsec -SharedKey "preSharedKey"
        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng3 -ConnectionType IPsec -SharedKey "preSharedKey"
        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection4" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng4 -ConnectionType IPsec -SharedKey "preSharedKey"

        Get-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling"
        



