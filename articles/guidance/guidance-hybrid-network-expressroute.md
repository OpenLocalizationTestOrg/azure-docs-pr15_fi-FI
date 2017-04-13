<properties
   pageTitle="Käyttöönoton Hybrid-verkoston arkkitehtuuri, ja Azure ExpressRoute | Viittaus arkkitehtuuri | Microsoft Azure"
   description="Miten suojattu sivusto verkko-arkkitehtuuri, joka ulottuu Azure virtual verkko- ja Azure ExpressRoute yhdistetään paikallisen-verkkoon."
   services=""
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="implementing-a-hybrid-network-architecture-with-azure-expressroute"></a>Hybrid-verkoston arkkitehtuuri, Azure ExpressRoute ja toteuttaminen

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Tässä artikkelissa kerrotaan parhaita käytäntöjä yhteyden paikalliseen verkkoon virtual verkkoihin Azure-ExpressRoute avulla. ExpressRoute yhteydet tehdään yksityinen erillinen yhteyden kolmannen osapuolen connectivity-palvelun kautta. Yksityinen yhteys ulottuu paikallisen verkon Azure tarjoavat access-Azure, yleisölle PaaS Services-palveluissa ja SaaS Office 365-palveluiden käyttämistä päätepisteistä oman IaaS-infrastruktuuria. Tämän asiakirjan ohjeessa on yhden Azure virtual verkoston (VNet) avulla, mitä kutsutaan yksityinen peering muodostaminen ExpressRoute avulla.

> [AZURE.NOTE] Azure on kaksi eri käyttöönoton mallit: [Resurssienhallinta] [ resource-manager-overview] ja perinteinen. Tämä Sinikopio käyttää Resurssienhallinta, johon Microsoft suosittelee uuden käyttöönotoissa.

Tavallisesti tämä arkkitehtuuri laatikkomäärät ovat seuraavat:

- Hybrid sovellukset, jossa työmääriä jaetaan paikalliseen verkkoon ja Azure välillä.

- Sovellusten suurissa, kriittisten toiminnoista, jotka edellyttävät skaalattavuus hyvin aste.

- Suurissa varmuuskopiointi- ja laitokset tietoja, jotka on tallennettu Työmaan ulkopuolella.

- Käsittely Big datasta toiminnoista.

- Käytä Azure palauttaminen sivustossa.

> [AZURE.NOTE] [Teknisiä tietoja ExpressRoute] [ expressroute-technical-overview] esitellään ExpressRoute.

## <a name="architecture-diagram"></a>Arkkitehtuurikaavio

Seuraavassa kaaviossa korostaa tärkeitä osia tämän arkkitehtuuri:

> Visio-tiedosto, joka sisältää arkkitehtuuri Tässä kaaviossa on ladattavissa [Microsoft download Centeristä][visio-download]. Tässä kaaviossa on "Hybrid verkko - ER"-sivulla.

![[0]][0]

- **Azure virtual verkot (VNets).** Kunkin VNet sijaitsee yhden Azure alueen ja ylläpitää useita sovelluksen tasoa. Sovelluksen tasoa voidaan Segmentoitu aliverkosta käyttäminen kunkin VNet ja/tai verkon käyttöoikeusryhmät (NSGs). 

- **Azure julkisen palvelut.** Nämä ovat Azure palvelut, joita voidaan käyttää hybrid-sovelluksesta. Nämä palvelut ovat käytettävissä myös julkisen Internetin kautta, mutta käyttää niitä ExpressRoute piiri kautta on pieni viive ja aiempaa helpommin ennakoitavia suorituskykyä, koska liikenne ei siirry Internetin kautta. Yhteyksien suoritetaan käyttämällä **julkisia peering**-osoitteet, jotka joko organisaation omaisuutta tai toimittamaan connectivity-palveluntarjoajan. 

- **Office 365-palveluihin.** Nämä ovat yleisesti saatavilla Office 365-sovelluksia ja palveluja Microsoftin. Yhteyksien suoritetaan käyttämällä **Microsoft peering**-osoitteet, jotka joko organisaation omaisuutta tai toimittamaan connectivity-palveluntarjoajan.

>[AZURE.NOTE] Voit myös muodostaa suoraan yhteyden Microsoft CRM Online – Microsoft peering.

- **Paikallisen yrityksen verkkoon.** Tämä on verkon tietokoneiden ja laitteiden, organisaation käytössä paikalliseen yksityisverkon kautta.

- **Paikallinen reunan reitittimen.** Nämä ovat reitittimen, jotka muodostavat yhteyden palveluntarjoaja hallitsee piiri paikalliseen verkkoon. Sen mukaan, miten yhteys on valmisteltu sinun on annettava näiden reitittimen käyttämä julkiseen IP-osoitteet. 

- **Microsoft edge reitittimen.** Nämä ovat käytettävissä aktiivinen aktiivinen-kokoonpanossa kahden reitittimen. Nämä reitittimen Ota yhteys palveluntarjoaja yhteyden niiden piirit käyttäjän palvelinkeskukseen. Sen mukaan, miten yhteys on valmisteltu sinun on annettava näiden reitittimen käyttämä julkiseen IP-osoitteet.

- **ExpressRoute piiri.** Tämä on tason 2 tai layer 3 piiri toimittamaan connectivity-palvelun kanssa Azure liitokset paikallisen verkon reunan reitittimen kautta. Virtapiirin käyttää laitteisto-infrastruktuurin hallitsee yhteyden toimittaja.

- **Yhdistämispalvelua tarjoajat.** Nämä ovat yrityksille, jotka tarjoavat yhteyden käyttämällä joko tason 2 tai oman palvelinkeskuksen ja Azure palvelinkeskuksen väliset yhteydet layer 3.

## <a name="recommendations"></a>Suosituksia

Azure tarjoaa useita eri resursseja ja jotta hakemisto-arkkitehtuuri voidaan resurssityypit valmisteltu monella tavalla. On annettu Azure Resurssienhallinta-mallin asentaminen viittaus-arkkitehtuuri, joka noudattaa näitä suosituksia. Jos haluat luoda oman viittaus-arkkitehtuuri seuraavien suositusten ellei sinulla ole tietyn vaatimus suositus sovi.

### <a name="connectivity-providers"></a>Yhdistämispalvelua tarjoajat

Valitse sopiva ExpressRoute yhteyden toimittaja sijaintisi. Yhteys-palveluntarjoajat käytettävissä käyttäjän sijainnissa luettelo saamiseksi seuraavan PowerShellin Azure-komennon avulla:

```powershell
Get-AzureRmExpressRouteServiceProvider
```

ExpressRoute yhdistämispalvelua tarjoajat muodostaa yhteyttä palvelinkeskuksen Microsoft seuraavilla tavoilla:

- **Yhteiskäyttö osoitteessa cloud exchange.** Jos sijaintisi yhtä tilojen cloud Exchangen kanssa, voit tilata virtual rajat-yhteyksien kautta rinnakkain kehittäjän Ethernet exchange Microsoft pilveen. Rinnakkain tarjoajat tarjota Layer 2 rajat-yhteydet tai hallittua Layer 3 rajat-kautta rinnakkain-tila-infrastruktuuria – Microsoft cloud...

- **Pisteestä pisteeseen Ethernet-yhteydet.** Voit muodostaa paikallisen-palvelinkeskusten/toimipisteitä pisteestä pisteeseen Ethernet linkkien kautta Microsoftin pilvipalveluihin. Pisteestä pisteeseen Ethernet tarjoajat tarjota Layer 2 yhteydet tai hallita sivuston ja Microsoft cloud välisiä yhteyksiä Layer 3.

- **Mikä tahansa-,-tahansa (IPVPN) verkot.** Voit integroida oman WAN Microsoft pilven kautta. IPVPN valmistajat (yleensä MPLS VPN) tarjoavat tahansa-,-tahansa toimipisteiden ja palvelinkeskusten väliset yhteydet. Microsoft cloud on yhdistetty toisiinsa, että voit muuttaa asiakirjan ulkoasua aivan kuten muita Tampereen toimistossa WAN. WAN valmistajat tarjoavat yleensä hallitun Layer 3-yhteys.

Saat lisätietoja yhdistämispalvelua tarjoajat [ExpressRoute piirit ja reititys toimialueiden][connectivity-providers].

### <a name="expressroute-circuit"></a>ExpressRoute piiri

Varmista, että organisaatiosi on täytetty [ExpressRoute valmistelevat vaatimukset] [ expressroute-prereqs] Azure muodostamisesta.

Jos et ole vielä tehnyt niin, Lisää nimeltä aliverkon `GatewaySubnet` Azure VNet, ja luo ExpressRoute VPN yhdyskäytävän Azure VPN Gateway-palveluun. Saat lisätietoja tämän prosessin [ExpressRoute työnkulut piiri valmistelu ja piiri Yhdysvaltojen][ExpressRoute-provisioning].

Luo ExpressRoute piiri seuraavasti:

1. Suorittaa seuraavan PowerShell-komennon:

    ```powershell
    New-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>> -Location <<location>> -SkuTier <<sku-tier>> -SkuFamily <<sku-family>> -ServiceProviderName <<service-provider-name>> -PeeringLocation <<peering-location>> -BandwidthInMbps <<bandwidth-in-mbps>>
    ```

2. Lähetä `ServiceKey` varten uuden piiri-palveluntarjoajaan.

3. Odota valmistelu virtapiirin palveluntarjoajasi. Voit tarkistaa piirin valmistelu tilan PowerShell-komennon avulla:

    ```powershell
    Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
    ```

`Provisioning state` -Kenttään `Service Provider` osa tulos muuttuu `NotProvisioned` , `Provisioned` kun virtapiirin on valmis.

>[AZURE.NOTE]Jos käytät layer 3 yhteyden-palveluntarjoajan pitäisi määrittäminen ja hallinta reititys Sinun on määritettävä käyttöön palveluntarjoajan toteuttamisesta haluamasi tiet tarvittavat tiedot.

Jos käytät tason 2 yhteyden, noudata seuraavia ohjeita:

1. Varaa kaksi/30 aliverkosta koostuu kunkin julkiseen IP-osoitteet, peering, jonka haluat ottaa käyttöön. Nämä/30 aliverkosta käytetään säätää virtapiirin käytettäviä reitittimen IP-osoitteet. Jos toteuttavat yksityinen, yleisön ja Microsoft peering, sinun on 6 /30 aliverkosta kelvollinen julkiseen IP-osoitteet.     

2. Määritä reititys ExpressRoute piiri. Sinun täytyy suorittaa komennon alapuolella mistäkin, peering haluat määrittää (yksityinen, yleisön ja Microsoftin).

    >[AZURE.NOTE]Katso [Luo ja Muokkaa reititys ExpressRoute piiri] [ configure-expressroute-routing] lisätietoja. Lisää peering reitittää liikenteen verkon PowerShell-komennoilla avulla:

    ```powershell
    Set-AzureRmExpressRouteCircuitPeeringConfig -Name <<peering-name>> -Circuit <<circuit-name>> -PeeringType <<peering-type>> -PeerASN <<peer-asn>> -PrimaryPeerAddressPrefix <<primary-peer-address-prefix>> -SecondaryPeerAddressPrefix <<secondary-peer-address-prefix>> -VlanId <<vlan-id>>

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit <<circuit-name>>
    ```

3. Varaa kelvollinen julkisten IP-osoitteiden julkisen NAT käyttäminen ja Microsoft peering toiseen resurssivarantoon. On suositeltavaa on eri resurssivarantoon kunkin peering varten. Määrittää yhteyden palveluntarjoajaan resurssivarantoon, jolloin ne määrittää erityisen ilmoitusten näistä alueista.

[Oman yksityisen VNet(s) pilveen linkittäminen ExpressRoute virtapiirin][link-vnet-to-expressroute]. Käytä PowerShell-komennot:

```
$circuit = Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
$gw = Get-AzureRmVirtualNetworkGateway -Name <<gateway-name>> -ResourceGroupName <<resource-group>>
New-AzureRmVirtualNetworkGatewayConnection -Name <<connection-name>> -ResourceGroupName <<resource-group>> -Location <<location> -VirtualNetworkGateway1 $gw -PeerId $circuit.Id -ConnectionType ExpressRoute
```

Huomaa seuraavat seikat:

- ExpressRoute käyttää reunan yhdyskäytävän Protocol (erityisen) reititystiedot verkon ja Azure välillä vaihtaminen.

- Voit yhdistää useita VNets sijaitsevat saman ExpressRoute piiri eri alueita, kunhan kaikki VNets ExpressRoute pystyvät saman geopoliittisten alueen sijaitsevat.

## <a name="scalability-considerations"></a>Skaalattavuus huomioon otettavia seikkoja

ExpressRoute piirit on suuren kaistanleveyden polku verkkojen välillä. Yleensä suuremman kaistanleveyden mitä suurempi kustannukset. Vaikka jotkin palveluntarjoajat Salli yhteyden kaistanleveyttä, varmista, että olet valinnut alkuperäisen kaistanleveyden, jota surpasses tarpeitasi ja mahdollistaa kasvu. Siltä varalta, että haluat suurentaa kaistanleveyden myöhemmin, voit jättää on kaksi vaihtoehtoa.

Suurentaa kaistanleveyden. Muista, että kaikki tarjoajan avulla voit tehdä, dynaamisesti. Ja vältä tarvitse tehdä tässä mahdollisimman hyvin. Mutta tarvittaessa tarkistettuaan palvelussa, toimi alla olevia komentoja.

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
$ckt.ServiceProviderProperties.BandwidthInMbps = <<bandwidth-in-mbps>>
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

Voit suurentaa ilman menettäminen kaistanleveyden. Päivitysprosessin kaistanleveyden aiheuttaa häiriöitä yhdistämispalvelun. Sinun on Poista virtapiirin ja luo uudet määritykset kanssa.

Jos käytät vakio-Versiota varten ExpressRoute ja tarve Premium päivittää tai muuttaa olet omaa hinnoittelua suunnitteleminen (laskutettavan tai rajoittamaton), suorittaa alla olevia komentoja. `Sku.Tier` Ominaisuus voi olla `Standard` tai `Premium`; `Sku.Name` ominaisuus voi olla `MeteredData` tai `UnlimitedData`.

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>

$ckt.Sku.Tier = "Premium"
$ckt.Sku.Family = "MeteredData"
$ckt.Sku.Name = "Premium_MeteredData"

 Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

>[AZURE.IMPORTANT] Varmista, että `Sku.Name` ominaisuuden vastineita `Sku.Tier` ja `Sku.Family`. Jos muutat perhe- ja taso, mutta ei nimeä, yhteys eivät ole käytettävissä.

Voit päivittää ilman häiriöt tuote, mutta rajoittamaton hinnoittelu suunnitelma käytön mukaan laskutettavat siirtyminen ei onnistu. Kun päivitysprosessin olevat versiot kaistanleveyden käyttöä on oltava vakio SKU oletusarvon rajoissa.

> [AZURE.NOTE] ExpressRoute on hinnoittelutiedot suunnitelmia asiakkaille, että tai rajoittamaton tietojen perusteella. Katso [ExpressRoute hinnat] [ expressroute-pricing] lisätietoja. Kulujen vaihdella sen mukaan, piiri kaistanleveyden. Kaistanleveyttä todennäköisesti vaihtelevat tarjoajan toimittaja. Käytä `Get-AzureRmExpressRouteServiceProvider` cmdlet-komento on käytettävissä alueellasi ja kaistanleveyksien, jotka tarjoavat tarjoajat.

Yksittäisen ExpressRoute piiri tukee useita peerings ja VNet linkkejä. Katso [ExpressRoute rajoitukset] [ expressroute-limits] lisätietoja.

Ylimääräisen kulun ExpressRoute Premium-lisäosa on:

- Enintään 10 000 tiet piiri kohden. Ilman ExpressRoute Premium lisäosa rajana on 4 000 tiet piiri kohden.

- Yleinen ottaminen käyttöön missä tahansa resurssien kaikki alueen sijasta vain saman Manner alueiden maailman ExpressRoute-piiri yhteys.

- VNet linkkien kohti piiri 10 suurempi rajoittamaan mukaan virtapiirin kaistanleveyden määrä kasvaa.

ExpressRoute piirit on suunniteltu sallimaan väliaikaiset verkko bursts ylöspäin kahdesti kaistanleveyden rajoitus, joka on hankittu varten ilman lisäkustannuksia. Tämä saavutetaan tarpeettomat linkkien avulla. Kuitenkin kaikki yhdistämispalvelua tarjoajat tukevat tätä ominaisuutta; Tarkista yhteys-palveluntarjoajan avulla toiminto ennen sen mukaan.

## <a name="availability-considerations"></a>Käytettävyys huomioon otettavia seikkoja

Voit määrittää suuren käytettävyyden Azure-yhteyden eri tavoin sen mukaan, käytät tarjoajan tyypin ja ExpressRoute piirit ja olet valmis, voit määrittää VPN yhdyskäytävän yhteyksien määrä. Pisteinä jäljempänä yhteenvedon käytettävyysasetukset:

- ExpressRoute ei tue reitittimen redundancy protokollia, kuten HSRP ja VRRP toteuttamisesta suuren käytettävyyden. Käyttää sen sijaan erityisen istuntojen kohden peering tarpeettomat kahdet. Helpottaa erittäin käytettävissä olevat yhteydet verkkoon Microsoft Azure valmistelee voit kaksi tarpeettomat portit aktiivinen aktiivinen-kokoonpanossa kahden reitittimen (Microsoft edge osana) kanssa.

- Jos käytät tason 2-yhteyttä, ota käyttöön tarpeettomat reitittimen paikallisen verkon aktiivinen aktiivinen-kokoonpanossa. Yhdistä jonka yhden reitittimen ja toissijainen virtapiiri toiseen. Tämän avulla voit helposti saatavilla yhteyden molemmissa päissä yhteyden. Tämä on tarpeen vaatiessa ExpressRoute SLA. Katso [Azure ExpressRoute SLA] [ sla-for-expressroute] lisätietoja.

Kaavion seuraavassa tarpeettomat paikallisen reitittimen kokoonpanon, joka on liitetty ensisijainen ja toissijainen virtapiirit. Kunkin piiri käsittelee liikenteen julkiseen peering ja yksityiset peering (kunkin peering on määritetty /30 kahdet välilyöntejä, verkko-osoitteissa edellisessä kohdassa kuvatulla tavalla).

![[1]][1]

Jos käytät layer 3-yhteys, tarkista, että se sisältää tarpeettomat erityisen istunnot, joka käsittelee käytettävyys puolestasi.

Virtuaalinen verkkojen voi yhdistää useita ExpressRoute piirit ja kunkin piirit toimittamaan voit eri palveluntarjoajia. Tämä strategia on muita suuren käytettävyyden ja tietojen palauttaminen ominaisuuksia.

Määritä sivusto sivusto VPN kuin automaattisesti polku ExpressRoute. Tämä koskee vain yksityinen peering. Azure- ja Office 365-palveluja varten Internet on polku, vain automaattisesti.

Erityisen istuntojen käytetään oletusarvon mukaan 60 sekuntia aikakatkaisun-arvosta. Kun istunto on aikakatkaistu 3 kertaa, reitin on merkitty ei ole käytettävissä ja kaikki liikenne uudelleenohjataan jäljellä reitittimeen. 180 sekunnin aikakatkaisu voi olla liian pitkä kriittisten sovellusten. Tässä tapauksessa voit muuttaa erityisen aikakatkaisun asetusten paikallisen reitittimen arvoltaan pienemmän vastineen.

## <a name="manageability-considerations"></a>Hallittavuuden huomioon otettavia seikkoja

Voit käyttää [Azure Connectivity työkalujen (AzureCT)] [ azurect] seurannassa paikallisen palvelinkeskukseen ja Azure väliset yhteydet.

## <a name="security-considerations"></a>Suojausasiat

Voit määrittää suojausasetukset Azure-yhteyden eri tavoin-suojauksen koskee ja vaatimustenmukaisuus tarpeiden mukaan. Kohdeosoite-kohdan alla on yhteenveto tietoturva-asetukset.

ExpressRoute toimii layer 3. Verkon suojaus-laite joka rajoittaa liikenne oikeutetut resurssien avulla voidaan estää sovelluskerroksen uhkien. Lisäksi ExpressRoute yhteyksien julkisen peering vain voidaan aloittaa paikallisen. Tällöin päivittämättömien-palvelun käyttäminen ja vaarantavat paikalliset tiedot julkisesta Internetistä.

Suurenna suojaus, lisää verkon suojauksen laitteiden paikalliseen verkkoon ja palvelun edge-reitittimen välillä. Tämä auttaa rajoita luvattoman liikenne sisään VNet:

![[2]][2]

Valvonta-tai yhteensopivuuden voi olla tarpeen kieltää tutustuminen komponenteilta käynnissä Internet VNet ja toteuttaa [Pakotettu tunneling][forced-tuneling]. Tässä tilanteessa Internet-liikenne uudelleenohjataan takaisin käynnissä paikallisen, jossa voi seurata välityspalvelimen kautta. Voit estää luvattomia liikenne juoksutus ulos ja suodattaa mahdollisesti haitallisen saapuvan liikenteen voi määrittää välityspalvelin.

![[3]][3]

Oletusarvon mukaan Azure VMs Näytä tarjoamiseksi hallintaa varten - RDP ja Remote Powershell for Windowsin VMs ja SSH pääsy kirjautuminen, kun käyttöön palvelun Azure-portaalissa Linux-pohjaiset VMs käyttämistä päätepisteistä. Suurenna suojaus, ei ota käyttöön oman VMs julkiseen IP-osoite ja NSGs avulla voit varmistaa, että nämä VMs eivät ole kaikkien käytettävissä. VMs vain pitäisi olla käytettävissä sisäinen IP-osoitetta. Nämä osoitteet voidaan luoda ottaminen käyttöön paikallisen DevOps henkilökunnan suorittamaan kaikki tarvittavat määritykset ja ylläpito ExpressRoute verkon kautta.

Jos on paljastaa hallinta päätepisteet VMs, ulkoiset verkkoon, käytä NSGs ja/tai käyttöoikeusluettelot rajoittaa porttien näkyvyyden whitelist IP-osoitteet tai verkkoja.

## <a name="troubleshooting-considerations"></a>Huomioon otettavia seikkoja vianmääritys

Jos aiemmin toimii ExpressRoute piiri epäonnistuu nyt muodostaa ei ole mitään määritysten muutokset paikallisia tai oman yksityisen VNet joudut ehkä connectivity tarjoajalta ja käyttää niitä voit korjata tämän ongelman. Voit käyttää myös seuraavat PowerShellin Azure-komennot suorittaa joitakin rajoitettu tarkistaminen ja selvittää, missä ongelmat saattavat ovat:

- Varmista, että virtapiirin on valmisteltu:

```powershell
Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
```

Tämä komento näet oman piiri useita ominaisuudet mukaan lukien `ProvisioningState`, `CircuitProvisioningState`, ja `ServiceProviderProvisioningState` alla kuvatulla tavalla.

```
ProvisioningState                : Succeeded
Sku                              : {
                                     "Name": "Standard_MeteredData",
                                     "Tier": "Standard",
                                     "Family": "MeteredData"
                                   }
CircuitProvisioningState         : Enabled
ServiceProviderProvisioningState : NotProvisioned
```

Jos `ProvisioningState` ei ole määritetty `Succeeded` sen jälkeen, kun yritit luoda uuden piiri, poista virtapiirin alla-komennon avulla ja luo uudelleen.

```powershell
Remove-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
```

Jos palveluntarjoajan oli jo valmisteltu piiri- ja `ProvisioningState` on määritetty `Failed`, tai `CircuitProvisioningState` ei ole `Enabled`, saat tarjoajalta saat lisäohjeita.

## <a name="solution-deployment"></a>Ratkaisun käyttöönotto

Jos sinulla on jo määritetty sopivia verkko-laitteen aiemmin paikalliseen infrastruktuuri, voit ottaa viite-arkkitehtuuri toimimalla seuraavasti:

Jos sinulla on jo määritetty VPN-laitteen aiemmin paikalliseen infrastruktuuri, voit ottaa viite-arkkitehtuuri toimimalla seuraavasti:

1. Napsauta alla olevaa painiketta hiiren kakkospainikkeella ja valitse joko "Avaa linkki uudessa välilehdessä" tai "Avaa linkki uudessa ikkunassa":  
[![Ottaa käyttöön Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-er%2Fazuredeploy.json)

2. Linkki Azure-portaalissa avaamaan Odota ja valitse seuraavasti: 
  - **Resurssiryhmä** nimi on jo määritetty parametri-tiedostossa, joten valitsemalla **Luo uusi** ja kirjoita `ra-hybrid-er-rg` tekstiruutuun.
  - Valitse alue-ruutuun **sijainti** avattavasta luettelosta.
  - Älä muokkaa **Mallin pääkansion Uri** tai **Parametrin pääkansion Uri** -tekstiruutuja.
  - Tarkista ehdot ja valitse sitten **voin sopivat edellä mainittujen ehdot** -valintaruutu.
  - Valitse **Osta** -painiketta.

3. Odota suorittamiseen käyttöönottoa varten.

4. Napsauta alla olevaa painiketta hiiren kakkospainikkeella ja valitse joko "Avaa linkki uudessa välilehdessä" tai "Avaa linkki uudessa ikkunassa":  
[![Ottaa käyttöön Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-er%2Fazuredeploy-expressRouteCircuit.json)

5. Linkki Azure-portaalissa avaamaan Odota ja valitse seuraavasti: 
  - Valitse **Käytä aiemmin** **resurssiryhmä** -kohta ja kirjoita `ra-hybrid-er-rg` tekstiruutuun.
  - Valitse alue-ruutuun **sijainti** avattavasta luettelosta.
  - Älä muokkaa **Mallin pääkansion Uri** tai **Parametrin pääkansion Uri** -tekstiruutuja.
  - Tarkista ehdot ja valitse sitten **voin sopivat edellä mainittujen ehdot** -valintaruutu.
  - Valitse **Osta** -painiketta.

6. Odota suorittamiseen käyttöönottoa varten.

## <a name="next-steps"></a>Seuraavat vaiheet

- Katso [käyttöönoton erittäin käytettävissä hybrid verkko-arkkitehtuuri] [ highly-available-network-architecture] tietoja lisääntyvien hybrid verkon käytettävyyttä perusteella ExpressRoute epäonnistumista VPN-yhteyden kautta.

<!-- links -->
[expressroute-technical-overview]: ../expressroute/expressroute-introduction.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[azure-powershell]: ../powershell-azure-resource-manager.md
[expressroute-prereqs]: ../expressroute/expressroute-prerequisites.md
[configure-expressroute-routing]: ../expressroute/expressroute-howto-routing-arm.md
[sla-for-expressroute]: https://azure.microsoft.com/support/legal/sla/expressroute/v1_0/
[link-vnet-to-expressroute]: ../expressroute/expressroute-howto-linkvnet-arm.md
[ExpressRoute-provisioning]: ../expressroute/expressroute-workflows.md
[expressroute-pricing]: https://azure.microsoft.com/pricing/details/expressroute/
[expressroute-limits]: ../azure-subscription-service-limits.md#networking-limits
[sample-script]: #sample-solution-script
[connectivity-providers]: ../expressroute/expressroute-introduction.md#how-can-i-connect-my-network-to-microsoft-using-expressroute
[azurect]: https://github.com/Azure/NetworkMonitoring/tree/master/AzureCT
[forced-tuneling]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[highly-available-network-architecture]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[arm-templates]: ../resource-group-authoring-templates.md
[naming-conventions]: ./guidance-naming-conventions.md
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/deploy-reference-architecture.sh
[vnet-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/parameters/virtualNetwork.parameters.json
[virtualnetworkgateway-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/parameters/virtualNetworkGateway.parameters.json
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[er-circuit-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/parameters/expressRouteCircuit.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[azure-cli]: https://azure.microsoft.com/documentation/articles/xplat-cli-install/
[0]: ./media/guidance-hybrid-network-expressroute/figure1.png "Hybrid verkoston arkkitehtuuri Azure ExpressRoute käyttäminen"
[1]: ./media/guidance-hybrid-network-expressroute/figure2.png "Tarpeettomien reitittimen käyttäminen ExpressRoute ensisijainen ja toissijainen piirit"
[2]: ./media/guidance-hybrid-network-expressroute/figure3.png "Suojauksen laitteiden lisääminen paikalliseen verkkoon"
[3]: ./media/guidance-hybrid-network-expressroute/figure4.png "Käyttämällä pakotettu tunneling valvottavien sidottujen Internet-liikenne"
[4]: ./media/guidance-hybrid-network-expressroute/figure5.png "ExpressRoute piiri ServiceKey etsiminen"
