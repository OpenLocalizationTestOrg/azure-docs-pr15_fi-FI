<properties 
   pageTitle="Määritä sivusto sivusto yhteydet perinteinen käyttöönoton mallin pakotettu tunneloinnin | Microsoft Azure"
   description="Voit ohjata tai "pakottaa" kaikki sidottujen Internet-liikenne takaisin paikalliseen sijaintiin."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/10/2016"
   ms.author="cherylmc" />

# <a name="configure-forced-tunneling-using-the-classic-deployment-model"></a>Määritä pakotettu tunneling perinteinen käyttöönotto-malli

> [AZURE.SELECTOR]
- [PowerShell – perinteinen](vpn-gateway-about-forced-tunneling.md)
- [PowerShell - resurssien hallinta](vpn-gateway-forced-tunneling-rm.md)

Pakotetun tunneling avulla voit uudelleenohjata tai "voimassa" kaikki sidottujen Internet-liikenne takaisin paikalliseen sijaintisi kautta sivusto sivusto VPN-tunneli tarkastus ja valvonta. Tämä on useimmissa yrityksen IT kriittinen vaatimus käytännöt. 

Ilman pakotettu tunneling Internet sidottujen tietoliikenteen oman VMs Azure-tietokannassa aina käy läpi Azure verkkoinfrastruktuuria suoraan-Internet-asetuksen avulla voit tutkia tai valvonta liikenne ilman. Internet-luvattomasti voi aiheuttaa mahdollisesti tietojen paljastaminen tai muun tyyppisiä suojauksen ilmeisesti.

Tässä artikkelissa edetään ohjatusti määrittäminen pakotettu tunneling virtual verkkojen luotu perinteinen käyttöönoton mallia. 

**Tietoja malleista Azure käyttöönotto**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

**Käyttöönotto-malleja ja pakotettu tunneloinnissa Työkalut**

Pakotetun tunneloinnin yhteyden voi määrittää perinteinen käyttöönoton mallia ja resurssien hallinnan käyttöönottomalli. Katso lisätietoja seuraavassa taulukossa. Päivitämme tässä taulukossa, kun uusia artikkeleita, uudet käyttöönoton mallit ja muita työkaluja palvelupakettiisi määritysten. Artikkelin ollessa käytettävissä on linkki suoraan taulukosta.

[AZURE.INCLUDE [vpn-gateway-forcedtunnel](../../includes/vpn-gateway-table-forcedtunnel-include.md)] 


## <a name="requirements-and-considerations"></a>Vaatimukset ja huomioon otettavia seikkoja

Pakotetun tunneling Azure-tietokannassa on määritetty VPN käyttäjän määrittämät tiet (UDR) kautta. Azure VPN-yhdyskäytävän oletusarvon reitin ilmaistaan uudelleenohjausta liikenne paikalliseen-sivustoon. Seuraavassa osassa on luettelo nykyinen rajoitus reititys taulukon ja tiet Virtual Azure-verkossa:


-  Kunkin VPN-aliverkon on valmis, järjestelmän reititys taulukko. Järjestelmä reititys-taulukko on tiet seuraavasta kolmesta ryhmästä:

    - **Paikallisen VNet reitittää:** Suoraan kohteeseen VMs virtual samassa verkostossa
    
    - **-Paikallisen tiet:** Azure VPN Gateway
    
    - **Oletusarvon reitin:** Yhteys Internetiin. Yksityisen IP-osoitteiden ei koske edellisen kaksi tiet pakettien poistetaan.


-  Käyttäjän määrittämät tiet versiossa oletusarvo-reitin lisääminen reititys taulukon luominen ja liitä reititys taulukon VNet-subnet(s) käyttöön kyseiset aliverkosta pakotettu tunneling.

- Sinun on määritettävä "oletussivusto" kesken paikallisen-paikalliset sivustot virtual verkkoyhteyttä.

- Pakotetun tunneling on oltava yhdistettynä VNet, jossa on dynaaminen reititys VPN yhdyskäytävän (ei staattinen yhdyskäytävä).
 
- Pakotettu tunneling ExpressRoute ei ole määritetty tämän järjestelmän kautta, mutta sen sijaan on käytössä, mainonta oletusarvon reitin ExpressRoute erityisen peering istunnot kautta. Katso lisätietoja [ExpressRoute ohjeissa](https://azure.microsoft.com/documentation/services/expressroute/) .



## <a name="configuration-overview"></a>Yleistä

Seuraavassa esimerkissä aliverkon ei ole pakotettu edusta tunneloidun. Voit hyväksyä ja vastata Internetistä suoraan jatkaa edusta-aliverkon työtaakkaa. Keskellä tason ja Taustajärjestelmä aliverkosta pakotetaan Tunneloitujen. Kaikki lähtevät yhteydet näiden kahden aliverkon Internet on pakotettu tai ohjataan takaisin paikalliseen-sivuston kautta jokin S2S VPN-tunneleita.

Näin voit rajoittaa ja tarkista Internet-yhteyttä, että näennäiskoneiden- tai cloud Services-palvelujen Azure-samalla, kun käyttöön oman monitasoisten arkkitehtuuri pakollinen. Voit myös käyttää pakotettu tunneling koko virtual verkkoihin Jos ei ole Internetiin yhteydessä oleva työmääriä virtual lopettaminen.


![Pakotettu Tunneling](./media/vpn-gateway-about-forced-tunneling/forced-tunnel.png)



## <a name="before-you-begin"></a>Ennen aloittamista

Tarkista, pidä seuraavat asiat ennen aloittamista määritys.

- Azure tilaus. Jos sinulla ei vielä ole Azure tilaus, voit ottaa [MSDN tilaajan edut](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) tai sisäänkirjautumisen määrittäminen [ilmainen tili](https://azure.microsoft.com/pricing/free-trial/).

- Määritetty virtual verkkoon. 

- Azure PowerShellin cmdlet-komennot uusimman version. Katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md) lisätietoja asentaminen PowerShellin cmdlet-komennot.


## <a name="configure-forced-tunneling"></a>Määritä pakotettu tunneling

Seuraavien ohjeiden avulla voit määrittää pakotettu tunneloinnin virtual verkkoon. Määritysvaiheet vastaavat VNet verkon määritys-tiedosto.



    <VirtualNetworkSite name="MultiTier-VNet" Location="North Europe">
     <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="DefaultSiteHQ">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch1">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch2">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch3">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
        </Gateway>
      </VirtualNetworkSite>
    </VirtualNetworkSite>

Tässä esimerkissä virtual verkon "MultiTier VNet" on kolme aliverkosta: *Frontend*, *Midtier*ja *Taustajärjestelmä* aliverkosta neljä cross paikallisen yhteydet: *DefaultSiteHQ*ja kolme *haaroja*. 

Ohjeita Määritä *DefaultSiteHQ* oletusarvon sivuston yhteys pakotettu tunneling ja määrittää Midtier ja Taustajärjestelmä aliverkosta käyttämään pakotettu tunneling.


1. Luo reititys taulukko. Seuraavan cmdlet-komennon avulla voit luoda reititystaulukon.

        New-AzureRouteTable –Name "MyRouteTable" –Label "Routing Table for Forced Tunneling" –Location "North Europe"

2. Lisää oletusarvon reitti reititys taulukkoon. 

    Seuraava esimerkki lisää oletusarvon reitin reititys vaiheessa 1 luomasi taulukon. Huomaa, että vain reitin tuettu on "0.0.0.0/0", "VPNGateway" NextHop kohde etuliite.
 
        Get-AzureRouteTable -Name "MyRouteTable" | Set-AzureRoute –RouteTable "MyRouteTable" –RouteName "DefaultRoute" –AddressPrefix "0.0.0.0/0" –NextHopType VPNGateway

3. Liitä aliverkosta reititys taulukon. 

    Kun reititys taulukko luodaan ja reitin lisätään, käyttämällä seuraavassa esimerkissä lisääminen tai liittää VNet aliverkon reititystaulukon. Esimerkki Lisää reititystaulukon "MyRouteTable" VNet MultiTier-VNet aliverkosta Midtier ja Taustajärjestelmä.

        Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Midtier" -RouteTableName "MyRouteTable"

        Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Backend" -RouteTableName "MyRouteTable"

4. Määrittää oletussivusto varten pakotettu tunneling. 

    Edellisessä vaiheessa cmdlet-komentosarjamallit luonut reititys taulukon ja siihen kaksi VNet aliverkosta reititystaulukon. Jäljellä oleva vaihe on valita usean sivuston yhteydet virtual verkoston kesken paikallisen sivuston oletussivuston tai tunnelin.

        $DefaultSite = @("DefaultSiteHQ")
        Set-AzureVNetGatewayDefaultSite –VNetName "MultiTier-VNet" –DefaultSite "DefaultSiteHQ"

## <a name="additional-powershell-cmdlets"></a>Lisätietoja PowerShellin cmdlet-komennot

### <a name="to-delete-a-route-table"></a>Jos haluat poistaa reititystaulukon

    Remove-AzureRouteTable -Name <routeTableName>

### <a name="to-list-a-route-table"></a>Jos haluat reitti-kenttä

    Get-AzureRouteTable [-Name <routeTableName> [-DetailLevel <detailLevel>]]

### <a name="to-delete-a-route-from-a-route-table"></a>Poistaa reittiä reitti-taulukosta

    Remove-AzureRouteTable –Name <routeTableName>

### <a name="to-remove-a-route-from-a-subnet"></a>Jos haluat poistaa reitin aliverkon

    Remove-AzureSubnetRouteTable –VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>

### <a name="to-list-the-route-table-associated-with-a-subnet"></a>Luettelon reitin aliverkon liittyvä taulukko
    
    Get-AzureSubnetRouteTable -VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>

### <a name="to-remove-a-default-site-from-a-vnet-vpn-gateway"></a>Jos haluat poistaa VNet VPN-yhdyskäytävän oletussivusto

    Remove-AzureVnetGatewayDefaultSite -VNetName <virtualNetworkName>






