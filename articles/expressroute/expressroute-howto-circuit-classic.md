<properties
   pageTitle="Luoda ja muokata ExpressRoute piiri perinteinen käyttöönoton mallia ja PowerShellin avulla | Microsoft Azure"
   description="Tässä artikkelissa käydään läpi luominen ja valmisteleminen ExpressRoute piiri. Tässä artikkelissa myös kerrotaan, miten deprovision oman piiri ja Tarkista tila, Päivitä tai poista."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="ganesr;cherylmc"/>

# <a name="create-and-modify-an-expressroute-circuit"></a>Luoda ja muokata ExpressRoute piiri

> [AZURE.SELECTOR]
[Azure Portal - Resurssienhallinta](expressroute-howto-circuit-portal-resource-manager.md)
[PowerShell - Resurssienhallinta](expressroute-howto-circuit-arm.md)
[PowerShell – perinteinen](expressroute-howto-circuit-classic.md)

Tässä artikkelissa käydään läpi vaiheet ja luoda Azure ExpressRoute piiri PowerShellin cmdlet-komennot ja perinteinen käyttöönoton mallia. Tässä artikkelissa myös esitellään, miten deprovision ExpressRoute piiri ja Tarkista tila, Päivitä tai poista.

**Tietoja malleista Azure käyttöönotto**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]


## <a name="before-you-begin"></a>Ennen aloittamista

### <a name="1-review-the-prerequisites-and-workflow-articles"></a>1. Tarkista edellytykset ja työnkulun artikkelit

Varmista, että olet tarkistanut [edellytykset](expressroute-prerequisites.md) ja [Työnkulut](expressroute-workflows.md) ennen kuin aloitat määritys.  


### <a name="2-install-the-latest-versions-of-the-azure-powershell-modules"></a>2. Asenna uusimmat versiot PowerShellin Azure-moduulit 

Noudata [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md) vaiheittaisia ohjeita siitä, miten voit määrittää tietokoneen käyttämään PowerShellin Azure-moduuleja.

### <a name="3-log-in-to-your-azure-account-and-select-a-subscription"></a>3. kirjautuminen Azure-tili ja valitse tilaus

1. Suorita järjestelmänvalvojana Windows PowerShell-kehotteessa käyttämällä seuraavan cmdlet-komennon:

        Add-AzureAccount
2. Kirjaudu sisään näytön, joka tulee näkyviin kirjaudu tilillesi.

3. Saat luettelon käytettävissä tilauksistasi.

        Get-AzureSubscription
4. Valitse tilaus, jota haluat käyttää.
    
        Select-AzureSubscription -SubscriptionName "mysubscriptionname"

## <a name="create-and-provision-an-expressroute-circuit"></a>Luoda ja valmistella ExpressRoute piiri

### <a name="1-import-the-powershell-modules-for-expressroute"></a>1. tuominen ExpressRoute PowerShell-moduulit

 Jos et ole vielä tehnyt on tuotavien Azure ja ExpressRoute moduulit PowerShell-istunnon jotta voit aloittaa ExpressRoute Cmdlet-komentoja. Voit tuoda moduulit sijainnista, ne on asennettu paikalliseen tietokoneeseen. Sen mukaan, kun asensit moduulit menetelmä sijainti voi olla eri kuin seuraavassa esimerkissä. Muokkaa esimerkin tarvittaessa.  

    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2. tuetut tarjoajien, sijainnit ja kaistanleveyksien luettelon noutaminen

Ennen kuin luot ExpressRoute piiri, sinun on tuetut yhdistämispalvelua tarjoajat, sijainnit ja kaistanleveyden asetukset.

PowerShell-cmdlet-komennon `Get-AzureDedicatedCircuitServiceProvider` palauttaa tämän tietoja, joita käytetään myöhemmin vaiheet:

    Get-AzureDedicatedCircuitServiceProvider

Tarkista, onko yhteys-palveluntarjoajan luettelossa. Huomioi seuraavat tiedot sillä tarvitset sitä myöhemmin piirin luodessasi:

- Nimi

- PeeringLocations

- BandwidthsOffered

Voit nyt luoda ExpressRoute piiri.         

### <a name="3-create-an-expressroute-circuit"></a>3. ExpressRoute piiri luominen

Seuraavassa esimerkissä esitetään, kuinka voit luoda 200 Mbps ExpressRoute piiri kautta Equinix piin laakso. Jos käytössäsi on jokin muu palveluntarjoaja ja muita asetuksia, korvaa tiedot, kun teet puhelinnumeroon.

>[AZURE.IMPORTANT] ExpressRoute piiri laskutetaan-palvelun avainta annetaan hetken. Varmista tehtävän suorittamiseen, kun yhteys-palvelu on valmis valmistelu virtapiirin.


Seuraavassa on esimerkki-pyyntö, uusi palvelu-näppäimen:

    $Bandwidth = 200
    $CircuitName = "MyTestCircuit"
    $ServiceProvider = "Equinix"
    $Location = "Silicon Valley"

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Standard -BillingType MeteredData

Tai jos haluat luoda ExpressRoute piiri ja premium-apuohjelma, käytä seuraavassa esimerkissä. Lisätietoja on lisätietoja premium-lisäosan [ExpressRoute usein kysytyt kysymykset](expressroute-faqs.md) .

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Premium - BillingType MeteredData


Vastauksen sisältää palvelun-näppäintä. Saat kaikki parametrit on kuvaukset suorittamalla seuraavasti:

    get-help new-azurededicatedcircuit -detailed

### <a name="4-list-all-the-expressroute-circuits"></a>4. luettelon ExpressRoute piirit

Voit avata `Get-AzureDedicatedCircuit` kaikki ExpressRoute piirit, jonka loit luettelo-komennon:


    Get-AzureDedicatedCircuit

Vastaus on suurin piirtein seuraavanlaisen seuraavassa esimerkissä:

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

Voit hakea tiedot milloin tahansa `Get-AzureDedicatedCircuit` cmdlet-komento. Puhelun ilman parametreja näyttää kaikki virtapiirit. *ServiceKey* -kentässä näkyvät service-näppäintä.

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

Saat kaikki parametrit on kuvaukset suorittamalla seuraavasti:

    get-help get-azurededicatedcircuit -detailed

### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5. lähettäminen palvelun avainta palveluntarjoajan connectivity valmisteluun


*ServiceProviderProvisioningState* tietoja nykyisen tilan valmistelu tarjoaja reunassa. *Tila* on tilaa Microsoft-puolella. Lisätietoja piiri valmistelu hyötyä [Työnkulut](expressroute-workflows.md#expressroute-circuit-provisioning-states) on artikkelissa.

Kun luot uuden ExpressRoute piiri, virtapiirin on seuraavassa vaiheessa:

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


Virtapiirin siirtyvät seuraavat tilaan, kun yhteys-palvelun käyttöön puolestasi:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

Voit käyttää sitä seuraavia tilan on oltava ExpressRoute piiri:

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled


### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6. Tarkista säännöllisesti tila ja piiri avaimen tila

Voit tietää, kun palveluntarjoajan on ottanut käyttöön oman piiri. Kun virtapiirin on määritetty, *ServiceProviderProvisioningState* näkyy *Provisioned* , kuten seuraavassa esimerkissä:

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

### <a name="7-create-your-routing-configuration"></a>7. reititys konfiguroinnin luominen

Viitata [ExpressRoute piiri reitityksen kokoonpano (Luo ja Muokkaa piiri peerings)](expressroute-howto-routing-classic.md) vaiheittaiset ohjeet artikkelista.

>[AZURE.IMPORTANT] Nämä ohjeet koskevat vain piirit, jotka on luotu, jotka tarjoavat kerroksen 2 yritystietojen yhdistämispalvelut palveluntarjoajia. Jos käytät palveluntarjoaja, jossa voit valita hallittuja layer 3 services (yleensä IP VPN, kuten MPLS), yhteys-palveluntarjoajan määrittää ja hallita reititys puolestasi.

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8. virtual verkon linkittäminen ExpressRoute piiri

Linkki seuraavaksi virtual verkon ExpressRoute piiri. [Linkittäminen ExpressRoute piirit virtuaalinen verkkoihin](expressroute-howto-linkvnet-classic.md) viitata vaiheittaiset ohjeet. Jos tarvitset virtual verkoston luominen käyttämällä ExpressRoute perinteinen käyttöönotto-mallin, katso ohjeet [luominen ExpressRoute virtual verkon](expressroute-howto-vnet-portal-classic.md) .

## <a name="getting-the-status-of-an-expressroute-circuit"></a>Käytön ExpressRoute piiri tila

Voit hakea tiedot milloin tahansa `Get-AzureCircuit` cmdlet-komento. Puhelun ilman parametreja näyttää kaikki virtapiirit.

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

    Bandwidth                        : 1000
    CircuitName                      : MyAsiaCircuit
    Location                         : Singapore
    ServiceKey                       : #################################
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

Voit hankkia tiedot tietyn ExpressRoute piiri välittämällä palvelun avainta parametrina puhelun:

    Get-AzureDedicatedCircuit -ServiceKey "*********************************"

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled


Saat kaikki parametrit on kuvaukset suorittamalla seuraavasti:

    get-help get-azurededicatedcircuit -detailed

## <a name="modifying-an-expressroute-circuit"></a>ExpressRoute piiri muokkaaminen

Voit muuttaa tiettyjä ominaisuuksia ExpressRoute piiri ilman vaikuttavat yhteys.

Voit tehdä seuraavia toimintoja ei ole käyttökatkot:

- Ota käyttöön tai poistaa käytöstä ExpressRoute piiri ExpressRoute premium apuohjelman.
- Suurentaa ExpressRoute piiri kaistanleveyden. Huomaa, että päivitysprosessin piirin kaistanleveyden ei tueta.
- Muuttaa niin, että suunnitelma rajoittamaton tietojen käytön mukaan laskutettavat tiedoista. Huomaa, että muuttaminen niin, että suunnitelma rajoittamaton tietojen käytön mukaan laskutettavat tietoja ei tueta.
- Voit ottaa käyttöön ja poistaa käytöstä *Salli perinteinen toimintoja*.

Lisätietoja saat lisätietoja rajat ja rajoitukset [ExpressRoute usein kysytyt kysymykset](expressroute-faqs.md) .

### <a name="to-enable-the-expressroute-premium-add-on"></a>Jos haluat ottaa käyttöön ExpressRoute premium-lisäosa

Voit ottaa ExpressRoute premium-lisäosan käyttöön aiemmin piiri seuraavan PowerShell cmdlet-komennon avulla:

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Premium

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Premium
    Status                           : Enabled

Oman piiri on nyt käytössä ExpressRoute premium lisäosa ominaisuudet. Huomaa, että olemme alkaa Laskutus premium lisäosa-ominaisuutta, kun komento on suoritettu onnistuneesti.

### <a name="to-disable-the-expressroute-premium-add-on"></a>Jos haluat poistaa käytöstä ExpressRoute premium-lisäosa

>[AZURE.IMPORTANT] Tämä toiminto saattaa epäonnistua, jos käytät resurssit, jotka ovat suurempia kuin mikä on sallittu vakio piiri.

Ota seuraavat seikat huomioon:

- Varmista, että linkitetty virtapiirin virtual verkkojen määrä on alle 10 ennen premium alentaa vakio. Jos et, pyyntöä päivitys epäonnistuu ja voit laskuttaa premium-kurssit.

- Kaikki virtual verkot geopoliittisten muilla alueilla on Poista linkki. Jos et, pyyntöä päivitys epäonnistuu ja voit laskuttaa premium-kurssit.

- Reititystaulukon on oltava alle 4 000 tiet, yksityinen peering. Jos reititys-taulukkokoko on suurempi kuin 4 000 tiet, erityisen istunnon pudottaa ja ei reenabled, kunnes ilmoitettua etuliitteiden määrä siirtyy alle 4 000.

Voit poistaa käytöstä ExpressRoute premium-lisäosan aiemmin piiri seuraavan PowerShell cmdlet-komennon avulla:

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Standard

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled



### <a name="to-update-the-expressroute-circuit-bandwidth"></a>Jos haluat päivittää ExpressRoute piiri kaistanleveyden

Tarkista tuettujen kaistanleveyden asetusten palveluntarjoajan [ExpressRoute usein kysytyt kysymykset](expressroute-faqs.md) . Voit valita minkä kokoinen on suurempi kuin aiemmin piiri kokoa, kunhan fyysinen portti (jossa oman piiri luodaan) avulla.

>[AZURE.IMPORTANT] Ei voi pienentää ExpressRoute-piiri ilman häiriöt kaistanleveyden. Päivitysprosessin kaistanleveyden edellyttää, että deprovision ExpressRoute piiri ja reprovision uuden ExpressRoute piiri.

Kun olet päättänyt, mitä tarvitset kokoa, voit kokoa oman piiri seuraava komento:

    Set-AzureDedicatedCircuitProperties -ServiceKey ********************************* -Bandwidth 1000

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

Oman piiri on on koon Microsoft-puolella. Ota yhteyttä yhteyden palveluntarjoajan päivittämään käyttömahdollisuudet kiertäminen vastaamaan tämän muutoksen. Huomaa, että olemme alkaa Laskutus voit tästä lähtien Päivitä-asetus käyttöön.

Jos näet seuraavan virheen, kun piiri kaistanleveyden kasvattaminen, se tarkoittaa sitä siellä ei ole riittäviä kaistanleveyden jää porttiin, jossa on luotu aiemmin piiri. Sinun on Poista tätä piiri ja luo uusi piiri on haluamasi kokoinen. 

    Set-AzureDedicatedCircuitProperties : InvalidOperation : Insufficient bandwidth available to perform this circuit
    update operation
    At line:1 char:1
    + Set-AzureDedicatedCircuitProperties -ServiceKey ********************* ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Set-AzureDedicatedCircuitProperties], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.SetAzureDedicatedCircuitPropertiesCommand
    

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Valmistelun poistaminen ja ExpressRoute piiri poistaminen

Ota seuraavat seikat huomioon:

- On Poista kaikki virtual verkkojen ExpressRoute virtapiirin tätä toimintoa varten. Tarkista, onko virtual verkkoja, jotka on linkitetty virtapiirin Jos toiminto epäonnistuu.

- Jos ExpressRoute piiri palveluntarjoajan valmistelu tila on **Provisioning** tai **Provisioned** on työskenneltävä palveluntarjoajalta deprovision kiertäminen piiri. Olemme säilyvät Varaa resurssit ja laskuttaa, kunnes palveluntarjoaja valmistelun poistaminen virtapiirin on valmis, ja ilmoittaa us.

- Jos palveluntarjoaja on käyttömahdollisuus purettu piiri (palveluntarjoajan valmistelu tilan on määritetty **valmisteltu ole**) voit poistaa virtapiirin. Tämä estää virtapiirin laskutus.

Voit poistaa ExpressRoute piiri suorittamalla seuraavan komennon:

    Remove-AzureDedicatedCircuit -ServiceKey "*********************************"



## <a name="next-steps"></a>Seuraavat vaiheet

Kun olet luonut oman piiri, varmista, että teet jonkin seuraavista:

- [Luoda ja muokata reititys ExpressRoute piiri](expressroute-howto-routing-classic.md)
- [Linkki virtual verkon ExpressRoute piiri](expressroute-howto-linkvnet-classic.md)
