<properties
   pageTitle="Käyttöönoton erittäin käytettävissä hybrid verkko-arkkitehtuuri | Microsoft Azure"
   description="Miten toteuttamisesta suojattu sivusto verkoston arkkitehtuuri, joka ulottuu Azure virtual verkon ja paikallisen verkon yhteydessä käyttämällä ExpressRoute VPN yhdyskäytävän automaattisesti."
   services="guidance,virtual-network,vpn-gateway,expressroute"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="implementing-a-highly-available-hybrid-network-architecture"></a>Käyttöönoton erittäin käytettävissä hybrid verkko-arkkitehtuuri

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Tässä artikkelissa kerrotaan parhaita käytäntöjä virtual verkkoihin Azure-yhteyttä paikalliseen verkkoon käyttämällä ExpressRoute, sivustossa sivuston näennäisen yksityisverkon (VPN) automaattisesti yhteydeksi. Liikenne jatkuu paikalliseen verkkoon ja Azure virtual verkon välillä (VNet) ExpressRoute-yhteyden kautta.  Jos näkyvissä on menettäminen ExpressRoute piirissä, liikenne reititetään IP VPN-tunnelin kautta.

> [AZURE.NOTE] Azure on kaksi eri käyttöönoton mallit: [Resurssienhallinta] [ resource-manager-overview] ja perinteinen. Tämä Sinikopio käyttää Resurssienhallinta, johon Microsoft suosittelee uuden käyttöönotoissa.

Tavallisesti tämä arkkitehtuuri laatikkomäärät ovat seuraavat:

- Hybrid sovellukset, jossa työmääriä jaetaan paikalliseen verkkoon ja Azure välillä.

- Sovellusten suurissa, kriittisten toiminnoista, jotka edellyttävät skaalattavuus hyvin aste.

- Suurissa varmuuskopiointi- ja laitokset tietoja, jotka on tallennettu Työmaan ulkopuolella.

- Käsittely Big datasta toiminnoista.

- Käytä Azure palauttaminen sivustossa.

Huomaa, että jos ExpressRoute piiri ei ole käytettävissä, VPN-reitin vain käsittelee yksityisiä peering yhteyksiä. Julkiset peering ja peering yhteydet Microsoft välittää Internetin välityksellä.

## <a name="architecture-diagram"></a>Arkkitehtuurikaavio

>[AZURE.NOTE] [Azure VPN Gateway-palveluun] [ azure-vpn-gateway] toteuttaa kahdentyyppisiä VPN yhdyskäytävien; VPN VPN yhdyskäytävien ja ExpressRoute VPN yhdyskäytävät. Tässä asiakirjassa *VPN-yhdyskäytävän* viittaa Azure-palveluun, kun lauseita *VPN VPN-yhdyskäytävän* ja *ExpressRoute VPN yhdyskäytävän* termi avulla voit viitata yhdyskäytävän VPN- ja ExpressRoute käyttöotot tarpeen mukaan.

Seuraavassa kaaviossa korostaa tärkeitä osia tämän arkkitehtuuri:

> Visio-tiedosto, joka sisältää arkkitehtuuri Tässä kaaviossa on ladattavissa [Microsoft download Centeristä][visio-download]. Tässä kaaviossa on verkossa"Hybrid - Kannattaa VPN-sivu.

![[0]][0]

- **Azure Virtual verkot (VNets).** Kunkin VNet sijaitsee yhden Azure alueen ja ylläpitää useita sovelluksen tasoa. Sovelluksen tasoa voidaan Segmentoitu aliverkosta käyttäminen kunkin VNet ja/tai verkon käyttöoikeusryhmät (NSGs).

- **Paikallisen yrityksen verkkoon.** Tämä on verkon tietokoneiden ja laitteiden, organisaation käytössä paikalliseen yksityisverkon kautta.

- **VPN-laitteen.** Tämä on laite- tai palvelu, joka sisältää ulkoisen yhteys paikalliseen verkkoon. VPN-laitteen ehkä laite, tai se voi olla esimerkiksi reititys- ja Remote Access Service (RRAS) Windows Server 2012-ohjelmiston ratkaista.

> [AZURE.NOTE] Luettelo tuetuista VPN-laitteiden ja yhdyskäytävän Azure VPN-yhteyden muodostamisesta valitun VPN-laitteiden määrittämisestä on artikkelissa [Azure tukemat VPN-laiteluettelosta]sopiva laite ohjeet[vpn-appliance].

- **VPN VPN-yhdyskäytävä.** VPN VPN-yhdyskäytävän mahdollistaa VNet muodostaa VPN-laitteen paikalliseen verkkoon. VPN VPN-yhdyskäytävän määritetään hyväksymään pyynnöt paikallisen verkosta vain VPN-laitteen kautta. Lisätietoja on artikkelissa [etäyhteyden paikallisen-verkon Microsoft Azure virtual verkkoon][connect-to-an-Azure-vnet].

- **ExpressRoute VPN-yhdyskäytävä.** ExpressRoute VPN-yhdyskäytävän mahdollistaa muodostaa yhteys paikalliseen verkoston kanssa käytettävät ExpressRoute virtapiirin VNet.

- **Yhdyskäytävän aliverkon.** VPN-yhdyskäytävät järjestetään saman aliverkon.

- **VPN-yhteyttä.** Yhteys on ominaisuudet, jotka määrittävät yhteystyyppi (IP) ja paikallisen VPN-laitteen jaettu avaimen salaamiseen liikenne.

- **ExpressRoute piiri.** Tämä on tason 2 tai layer 3 piiri toimittamaan connectivity-palvelun kanssa Azure liitokset paikallisen verkon reunan reitittimen kautta. Virtapiirin käyttää laitteisto-infrastruktuurin hallitsee yhteyden toimittaja.

- **N-taso cloud-sovellus.** Tämä on ylläpidettävä Azure-sovellus. Se voi olla useita tasoa kanssa useita aliverkosta Azure kuormituksen tasoitusmääritykset kautta. Kunkin aliverkon tietoliikenne sinulta voidaan veloittaa säännöt, jotka on määritetty käyttämällä [Azure verkon käyttöoikeusryhmät][azure-network-security-group](NSGs). Lisätietoja on ohjeaiheessa [käytön aloittaminen Microsoft Azure suojauksen][getting-started-with-azure-security].

## <a name="recommendations"></a>Suosituksia

Azure tarjoaa useita eri resursseja ja jotta hakemisto-arkkitehtuuri voidaan resurssityypit valmisteltu monella tavalla. On annettu Azure Resurssienhallinta-mallin asentaminen viittaus-arkkitehtuuri, joka noudattaa näitä suosituksia. Jos haluat luoda oman viittaus-arkkitehtuuri seuraavien suositusten ellei sinulla ole tietyn vaatimus suositus sovi.

### <a name="vnet-and-gatewaysubnet"></a>VNet ja GatewaySubnet

Luo ExpressRoute VPN-yhdyskäytävän ja VPN VPN-yhdyskäytävän saman VNet. Tämä tarkoittaa, että ne olisi jakaa saman niminen **GatewaySubnet** aliverkon

Jos VNet sisältää jo nimeltä **GatewaySubnet**aliverkon, varmista, se on /27 tai suurempi osoitetilaa. Jos aiemmin aliverkon on liian pientä, voit poistaa sen seuraavasti ja luoda uuden seuraava luettelomerkki esitetyllä tavalla:

```powershell
$vnet = Get-AzureRmVirtualNetworkGateway -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
Remove-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet
```

Jos VNet ei sisällä nimeltä **GatewaySubnet**aliverkon, valitse Luo uusi seuraavasti:

```powershell
$vnet = Get-AzureRmVirtualNetworkGateway -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.224/27"
$vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="vpn-and-expressroute-gateways"></a>VPN- ja ExpressRoute yhdyskäytävät

Tarkista, että organisaation täyttää [ExpressRoute valmistelevat vaatimukset] [ expressroute-prereq] Azure muodostamisesta.

Jos sinulla on jo VPN VPN-yhdyskäytävän Azure VNet, Poista sivusto, alla kuvatulla tavalla.

```powershell
Remove-AzureRmVirtualNetworkGateway -Name <yourgatewayname> -ResourceGroupName <yourresourcegroup>
```

Ohjeiden [soveltamisesta hybrid-verkoston arkkitehtuuri, ja Azure ExpressRoute] [ implementing-expressroute] ExpressRoute yhteyden.

Ohjeiden [soveltamisesta hybrid-verkoston arkkitehtuuri Azure ja paikallisen VPN] [ implementing-vpn] muodostaa yhteys VPN VPN-yhdyskäytävä.

Kun olet muodostanut VPN-yhdyskäytävän yhteydet, Testaa ympäristön seuraavalla tavalla:

1. Varmista, että voit muodostaa paikallisen verkosta Azure VNet.

2. Lopeta ExpressRoute yhteyden testaaminen palveluntarjoajalta.

3. Varmista, että voit edelleen yhdistät paikallisen verkosta oman Azure VNet VPN-yhdyskäytävän VPN-yhteyden avulla.

4. Voit reestabilish ExpressRoute connectivity palveluntarjoajalta.

## <a name="considerations"></a>Huomioon otettavia seikkoja

Katso ExpressRoute huomioon otettavia seikkoja [käyttöönoton Hybrid-verkoston arkkitehtuuri, ja Azure ExpressRoute] [ guidance-expressroute] ohjeita.

Katso sivuston sivuston VPN huomioon otettavia seikkoja [käyttöönoton Hybrid-verkoston arkkitehtuuri Azure ja paikallisen VPN] [ guidance-vpn] ohjeita.

Katso Yleiset Azure suojausasiat [Microsoftin pilvipalveluihin ja verkkosuojaus][best-practices-security].

## <a name="solution-deployment"></a>Ratkaisun käyttöönotto

Jos sinulla on jo määritetty sopivia verkko-laitteen aiemmin paikalliseen infrastruktuuri, voit ottaa viite-arkkitehtuuri toimimalla seuraavasti:

1. Napsauta alla olevaa painiketta hiiren kakkospainikkeella ja valitse joko "Avaa linkki uudessa välilehdessä" tai "Avaa linkki uudessa ikkunassa":  
[![Ottaa käyttöön Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-vpn-er%2Fazuredeploy.json)

2. Linkki Azure-portaalissa avaamaan Odota ja valitse seuraavasti: 
  - **Resurssiryhmä** nimi on jo määritetty parametri-tiedostossa, joten valitsemalla **Luo uusi** ja kirjoita `ra-hybrid-vpn-er-rg` tekstiruutuun.
  - Valitse alue-ruutuun **sijainti** avattavasta luettelosta.
  - Älä muokkaa **Mallin pääkansion Uri** tai **Parametrin pääkansion Uri** -tekstiruutuja.
  - Tarkista ehdot ja valitse sitten **voin sopivat edellä mainittujen ehdot** -valintaruutu.
  - Valitse **Osta** -painiketta.

3. Odota suorittamiseen käyttöönottoa varten.

4. Napsauta alla olevaa painiketta hiiren kakkospainikkeella ja valitse joko "Avaa linkki uudessa välilehdessä" tai "Avaa linkki uudessa ikkunassa":  
[![Ottaa käyttöön Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-vpn-er%2Fazuredeploy-expressRouteCircuit.json)

5. Odota linkki Azure-portaalissa, avaa sitten kirjoita sitten seuraavasti: 
  - Valitse **Käytä aiemmin** **resurssiryhmä** -kohta ja kirjoita `ra-hybrid-vpn-er-rg` tekstiruutuun.
  - Valitse alue-ruutuun **sijainti** avattavasta luettelosta.
  - Älä muokkaa **Mallin pääkansion Uri** tai **Parametrin pääkansion Uri** -tekstiruutuja.
  - Tarkista ehdot ja valitse sitten **voin sopivat edellä mainittujen ehdot** -valintaruutu.
  - Valitse **Osta** -painiketta.

<!-- links -->

[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[vpn-appliance]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md
[azure-vpn-gateway]: ../vpn-gateway/vpn-gateway-about-vpngateways.md
[connect-to-an-Azure-vnet]: https://technet.microsoft.com/library/dn786406.aspx
[azure-network-security-group]: ../virtual-network/virtual-networks-nsg.md
[getting-started-with-azure-security]: ./../security/azure-security-getting-started.md
[expressroute-prereq]: ../expressroute/expressroute-prerequisites.md
[implementing-expressroute]: ./guidance-hybrid-network-expressroute.md#implementing-this-architecture
[implementing-vpn]: ./guidance-hybrid-network-vpn.md#implementing-this-architecture
[guidance-expressroute]: ./guidance-hybrid-network-expressroute.md
[guidance-vpn]: ./guidance-hybrid-network-vpn.md
[best-practices-security]: ../best-practices-network-security.md
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/deploy-reference-architecture.sh
[vnet-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/virtualNetwork.parameters.json
[virtualnetworkgateway-vpn-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/virtualNetworkGateway-vpn.parameters.json
[virtualnetworkgateway-expressroute-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/virtualNetworkGateway-expressRoute.parameters.json
[er-circuit-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/expressRouteCircuit.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[naming conventions]: ./guidance-naming-conventions.md
[azure-cli]: https://azure.microsoft.com/documentation/articles/xplat-cli-install/
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[0]: ./media/blueprints/hybrid-network-expressroute-vpn-failover.png "Arkkitehtuuri käyttämällä ExpressRoute ja VPN-yhdyskäytävän helposti saatavilla hybrid verkko-arkkitehtuuri"
[ARM-Templates]: https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/
