<properties
   pageTitle="Luoda ja muokata ExpressRoute piiri Resurssienhallinta ja PowerShellin avulla | Microsoft Azure"
   description="Tässä artikkelissa kerrotaan, miten voit luoda, valmistella, tarkista, päivittää, poistaa ja deprovision ExpressRoute piiri."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="ganesr"/>


# <a name="create-and-modify-an-expressroute-circuit"></a>Luoda ja muokata ExpressRoute piiri

> [AZURE.SELECTOR]
[Azure Portal - Resurssienhallinta](expressroute-howto-circuit-portal-resource-manager.md)
[PowerShell - Resurssienhallinta](expressroute-howto-circuit-arm.md)
[PowerShell – perinteinen](expressroute-howto-circuit-classic.md)


Tässä artikkelissa kerrotaan, miten voit luoda Azure ExpressRoute piiri käyttämällä Windows PowerShellin cmdlet-komennot ja Azure resurssien hallinnan käyttöönottomalli. Tässä artikkelissa myös noudattamalla voit virtapiirin tilan tarkistaminen, Päivitä tai poista ja deprovision sen.

**Tietoja malleista Azure käyttöönotto**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-you-begin"></a>Ennen aloittamista


- Hanki uusin versio PowerShellin Azure-moduuleja (vähintään versiota 1.0). Vaiheittaiset ohjeet siitä, miten voit määrittää tietokoneen käyttämään PowerShell-moduuleja [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md)noudata.

- Tarkista [edellytykset](expressroute-prerequisites.md) ja [Työnkulut](expressroute-workflows.md) , ennen kuin aloitat määritys.

## <a name="create-and-provision-an-expressroute-circuit"></a>Luoda ja valmistella ExpressRoute piiri

### <a name="1-sign-in-to-your-azure-account-and-select-your-subscription"></a>1. Kirjaudu Azure-tili ja valitse tilauksen

Aloita kokoonpanosi Kirjaudu Azure-tili. Lisätietoja PowerShell-artikkelissa [Windows PowerShellin resurssien hallinta](../powershell-azure-resource-manager.md). Seuraavien esimerkkien avulla voit tarkastella:

    Login-AzureRmAccount

Tarkista tilaukset tilin:

    Get-AzureRmSubscription

Valitse tilaus, jonka haluat luoda ExpressRoute-piiri varten:

    Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2. tuetut tarjoajien, sijainnit ja kaistanleveyksien luettelon noutaminen

Ennen kuin luot ExpressRoute piiri, sinun on tuetut yhdistämispalvelua tarjoajat, sijainnit ja kaistanleveyden asetukset.

PowerShell-cmdlet-komennon `Get-AzureRmExpressRouteServiceProvider` palauttaa tämän tietoja, joita käytetään myöhemmin vaiheet:

    Get-AzureRmExpressRouteServiceProvider

Tarkista, onko yhteys-palveluntarjoajan luettelossa. Huomioi seuraavat tiedot sillä tarvitset sitä myöhemmin piirin luodessasi:

- Nimi

- PeeringLocations

- BandwidthsOffered

Voit nyt luoda ExpressRoute piiri.   

### <a name="3-create-an-expressroute-circuit"></a>3. ExpressRoute piiri luominen

Jos sinulla ei vielä ole resurssiryhmä, sinun on luotava yksi, ennen kuin luot ExpressRoute piiri. Voit tehdä suorittamalla seuraavan komennon:


    New-AzureRmResourceGroup -Name "ExpressRouteResourceGroup" -Location "West US"


Seuraavassa esimerkissä esitetään, kuinka voit luoda 200 Mbps ExpressRoute piiri kautta Equinix piin laakso. Jos käytössäsi on jokin muu palveluntarjoaja ja muita asetuksia, korvaa tiedot, kun teet puhelinnumeroon. Seuraavassa on esimerkki-pyyntö, uusi palvelu-näppäimen:

    New-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup" -Location "West US" -SkuTier Standard -SkuFamily MeteredData -ServiceProviderName "Equinix" -PeeringLocation "Silicon Valley" -BandwidthInMbps 200

Varmista, että Määritä oikea SKU taso ja SKU perheen:

- TUOTE taso määrittää, onko käytössä ExpressRoute standardiksi tai ExpressRoute premium-lisäosa. Voit määrittää *Vakio* vakio tuote- tai *Premium* Hanki premium-lisäosa.

- Tuoteperheen määräytyy sen mukaan, Laskutus. Voit määrittää *Metereddata* laskutettavan tietoliikennesopimus ja *Unlimiteddata* rajoittamaton tietoliikennesopimus. Huomaa, että voit muuttaa laskutuksen tyyppi *Metereddata* *Unlimiteddata*, mutta ei voi muuttaa tyyppi *Unlimiteddata* *Metereddata*.


>[AZURE.IMPORTANT] ExpressRoute piiri laskutetaan-palvelun avainta annetaan hetken. Varmista tehtävän suorittamiseen, kun yhteys-palvelu on valmis valmistelu virtapiirin.

Vastaus sisältää palvelun-näppäintä. Saat kaikki parametrit on kuvaukset suorittamalla seuraavasti:


    get-help New-AzureRmExpressRouteCircuit -detailed


### <a name="4-list-all-expressroute-circuits"></a>4. Luettele kaikki ExpressRoute piirit

Saat kaikki ExpressRoute piirit, jonka loit luettelo-Suorita `Get-AzureRmExpressRouteCircuit` komento:


    Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

Vastauksen näyttää samalla tavalla kuin seuraavassa esimerkissä:


    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState          : Enabled
    ServiceProviderProvisioningState  : NotProvisioned
    ServiceProviderNotes              :
    ServiceProviderProperties         : {
                                          "ServiceProviderName": "Equinix",
                                          "PeeringLocation": "Silicon Valley",
                                          "BandwidthInMbps": 200
                                        }
    ServiceKey                        : **************************************
    Peerings                          : []

Voit hakea tiedot milloin tahansa `Get-AzureRmExpressRouteCircuit` cmdlet-komento. Puhelun ilman parametreja näyttää kaikki virtapiirit. Palvelun avainta listataan *ServiceKey* -kentässä:


    Get-AzureRmExpressRouteCircuit


Vastauksen näyttää samalla tavalla kuin seuraavassa esimerkissä:


    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : NotProvisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                       }
    ServiceKey                       : **************************************
    Peerings                         : []


Saat kaikki parametrit on kuvaukset suorittamalla seuraavasti:


    get-help Get-AzureRmExpressRouteCircuit -detailed

### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5. lähettäminen palvelun avainta palveluntarjoajan connectivity valmisteluun

*ServiceProviderProvisioningState* tietoja nykyisen tilan valmistelu tarjoaja reunassa. Tila on tilaa Microsoft-puolella. Lisätietoja piiri valmistelu hyötyä [Työnkulut](expressroute-workflows.md#expressroute-circuit-provisioning-states) on artikkelissa.

Kun luot uuden ExpressRoute piiri, virtapiirin on seuraavassa vaiheessa:


    ServiceProviderProvisioningState : NotProvisioned
    CircuitProvisioningState         : Enabled



Virtapiirin muuttuvat seuraavat tilaan, kun yhteys-palvelun käyttöön puolestasi:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

Voit käyttää ExpressRoute piiri sen on oltava seuraavat tilassa:

    ServiceProviderProvisioningState : Provisioned
    CircuitProvisioningState         : Enabled

### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6. Tarkista säännöllisesti tila ja piiri avaimen tila

Tarkistuksen tila ja piiri avaimen tila ilmoittaa, kun palveluntarjoajan on ottanut käyttöön oman piiri. Kun virtapiirin on määritetty, *ServiceProviderProvisioningState* näkyy *Provisioned*, kuten seuraavassa esimerkissä:


    Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"


Vastauksen näyttää samalla tavalla kuin seuraavassa esimerkissä:


    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                    }
    ServiceKey                       : **************************************
    Peerings                         : []

### <a name="7-create-your-routing-configuration"></a>7. reititys konfiguroinnin luominen

Vaiheittaiset ohjeet on artikkelissa [ExpressRoute piiri reitityksen kokoonpano](expressroute-howto-routing-arm.md) luomaan ja muokkaamaan piiri peerings.


>[AZURE.IMPORTANT] Nämä ohjeet koskevat vain piirit, jotka on luotu, jotka tarjoavat kerroksen 2 yritystietojen yhdistämispalvelut palveluntarjoajia. Jos käytät palveluntarjoaja, jossa voit valita hallittuja layer 3 services (yleensä IP VPN, kuten MPLS), yhteys-palveluntarjoajan määrittää ja hallita reititys puolestasi.

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8. virtual verkon linkittäminen ExpressRoute piiri

Linkki seuraavaksi virtual verkon ExpressRoute piiri. Käytä [linkittäminen virtual verkkojen ExpressRoute piirit](expressroute-howto-linkvnet-arm.md) artikkelin käsiteltäessä resurssien hallinnan käyttöönottomalli.

## <a name="getting-the-status-of-an-expressroute-circuit"></a>Käytön ExpressRoute piiri tila

Voit hakea tiedot milloin tahansa `Get-AzureRmExpressRouteCircuit` cmdlet-komento. Puhelun ilman parametreja näyttää kaikki virtapiirit.

    Get-AzureRmExpressRouteCircuit


Vastaus on samankaltainen kuin seuraavassa esimerkissä:


    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                       }
    ServiceKey                       : **************************************
    Peerings                         : []


Voit hankkia tiedot tietyn ExpressRoute piiri välittämällä resurssin nimen ja piiri nimen parametrina puhelun:


    Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"


Vastauksen näyttää samalla tavalla kuin seuraavassa esimerkissä:


    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                       }
    ServiceKey                       : **************************************
    Peerings                         : []


Saat kaikki parametrit on kuvaukset suorittamalla seuraavasti:

    get-help get-azurededicatedcircuit -detailed


## <a name="modify"></a>ExpressRoute piiri muokkaaminen

Voit muuttaa tiettyjä ominaisuuksia ExpressRoute piiri ilman vaikuttavat yhteys.

Voit tehdä seuraavia toimintoja ei ole käyttökatkot:

- Ota käyttöön tai poistaa käytöstä ExpressRoute piiri ExpressRoute premium apuohjelman.
- Suurentaa ExpressRoute piiri kaistanleveyden. Huomaa, että päivitysprosessin piirin kaistanleveyden ei tueta.
- Muuttaa niin, että suunnitelma rajoittamaton tietojen käytön mukaan laskutettavat tiedoista. Huomaa, että muuttaminen niin, että suunnitelma rajoittamaton tietojen käytön mukaan laskutettavat tietoja ei tueta.
-  Voit ottaa käyttöön ja poistaa käytöstä *Salli perinteinen toimintoja*.

Lisätietoja rajat ja rajoitukset viitata [ExpressRoute usein kysytyt kysymykset](expressroute-faqs.md).

### <a name="to-enable-the-expressroute-premium-add-on"></a>Jos haluat ottaa käyttöön ExpressRoute premium-lisäosa

Voit ottaa käyttöön aiemmin piiri ExpressRoute premium-lisäosan avulla seuraavat PowerShell-koodikatkelman:

    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Tier = "Premium"
    $ckt.sku.Name = "Premium_MeteredData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


Virtapiirin on nyt käytössä ExpressRoute premium lisäosa ominaisuudet. Huomaa, että olemme alkaa Laskutus premium lisäosa-ominaisuutta, kun komento on suoritettu onnistuneesti.

### <a name="to-disable-the-expressroute-premium-add-on"></a>Jos haluat poistaa käytöstä ExpressRoute premium-lisäosa

>[AZURE.IMPORTANT] Tämä toiminto saattaa epäonnistua, jos käytät resurssit, jotka ovat suurempia kuin mikä on sallittu vakio piiri.

Ota seuraavat seikat huomioon:

- Ennen kuin alentaa premium-standardia, sinun on varmistettava virtual verkkoja, jotka on linkitetty virtapiirin määrä on alle 10. Jos et tee näin, pyyntöä päivitys epäonnistuu ja Microsoft laskuttaa sinua premium tahdissa.

- Kaikki virtual verkot geopoliittisten muilla alueilla on Poista linkki. Jos et tee näin, pyyntöä päivitys epäonnistuu ja Microsoft laskuttaa sinua premium tahdissa.

- Reititystaulukon on oltava alle 4 000 tiet, yksityinen peering. Jos reititys-taulukkokoko on suurempi kuin 4 000 tiet, erityisen istunnon pudottaa ja ei reenabled, kunnes ilmoitettua etuliitteiden määrä siirtyy alle 4 000.

Voit poistaa aiemmin virtapiirin ExpressRoute premium lisäosa käytöstä seuraavan PowerShell cmdlet-komennon avulla:


    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Tier = "Standard"
    $ckt.sku.Name = "Standard_MeteredData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### <a name="to-update-the-expressroute-circuit-bandwidth"></a>Jos haluat päivittää ExpressRoute piiri kaistanleveyden

Tarkista tuettujen kaistanleveyden palveluntarjoajan asetukset- [ExpressRoute usein kysytyt kysymykset](expressroute-faqs.md). Voit valita minkä tahansa koko on suurempi kuin aiemmin piiri kokoa.

>[AZURE.IMPORTANT] Ei voi pienentää ExpressRoute-piiri ilman häiriöt kaistanleveyden. Päivitysprosessin kaistanleveyden edellyttää deprovision ExpressRoute piiri ja reprovision uuden ExpressRoute piiri.

Kun päätät minkä kokoinen, käytä seuraavaa komentoa oman piiri koon muuttaminen:


    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.ServiceProviderProperties.BandwidthInMbps = 1000

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


Oman piiri koon Microsoft-puolella. Valitse Ota yhteyttä yhteyden palveluntarjoajan päivittämään käyttömahdollisuudet kiertäminen vastaamaan tämän muutoksen. Kun olet tehnyt tämän ilmoituksen, emme aloittaa laskutus voit Päivitä-vaihtoehto.


### <a name="to-move-the-sku-from-metered-to-unlimited"></a>Siirrä-tuote käytön mukaan laskutettavat, rajoittamaton

Voit muuttaa ExpressRoute piiri SKU käyttämällä seuraavia PowerShell-koodikatkelman:

    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Family = "UnlimitedData"
    $ckt.sku.Name = "Premium_UnlimitedData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

### <a name="to-control-access-to-the-classic-and-resource-manager-environments"></a>Voit määrittää access perinteinen ja Resurssienhallinta-Ympäristöt  

Tarkista [siirtää ExpressRoute piirit Resurssienhallinta käyttöönoton mallin classic-](expressroute-howto-move-arm.md)ohjeita.  


## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Valmistelun poistaminen ja ExpressRoute piiri poistaminen

Ota seuraavat seikat huomioon:

- Kaikki virtual verkkojen ExpressRoute piiri on Poista linkki. Jos tämä epäonnistuu, tarkista, jos virtual verkkoja on linkitetty virtapiirin.

- Jos ExpressRoute piiri palveluntarjoajan valmistelu tila on **Provisioning** tai **Provisioned** on työskenneltävä palveluntarjoajalta deprovision kiertäminen piiri. Olemme säilyvät Varaa resurssit ja laskuttaa, kunnes palveluntarjoaja valmistelun poistaminen virtapiirin on valmis, ja ilmoittaa us.

- Jos palveluntarjoaja on käyttömahdollisuus purettu piiri (palveluntarjoajan valmistelu tilan on määritetty **valmisteltu ole**) voit poistaa virtapiirin. Tämä estää virtapiirin Laskutus

Voit poistaa ExpressRoute piiri suorittamalla seuraavan komennon:

    Remove-AzureRmExpressRouteCircuit -ResourceGroupName "ExpressRouteResourceGroup" -Name "ExpressRouteARMCircuit"



## <a name="next-steps"></a>Seuraavat vaiheet

Kun olet luonut oman piiri, varmista, että teet jonkin seuraavista:

- [Luoda ja muokata reititys ExpressRoute piiri](expressroute-howto-routing-arm.md)
- [Linkki virtual verkon ExpressRoute piiri](expressroute-howto-linkvnet-arm.md)
