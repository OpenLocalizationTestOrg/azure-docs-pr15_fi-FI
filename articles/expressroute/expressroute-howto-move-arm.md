<properties
   pageTitle="Siirrä ExpressRoute piirit klassinen resurssien hallinnan | Microsoft Azure"
   description="Tällä sivulla kerrotaan, kuinka perinteinen piiri siirretään resurssien hallinnan käyttöönottomalli."
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


# <a name="move-expressroute-circuits-from-the-classic-to-the-resource-manager-deployment-model"></a>Siirtää ExpressRoute piirit klassinen resurssien hallinnan käyttöönottomalli

## <a name="configuration-prerequisites"></a>Määritysten edellytykset

- Tarvitset PowerShellin Azure-moduulit uusimman version (vähintään versiota 1.0).
- Varmista, että olet tarkistanut [edellytykset](expressroute-prerequisites.md), [Reititys vaatimukset](expressroute-routing.md)ja [Työnkulut](expressroute-workflows.md) ennen kuin aloitat määritys.
- Ennen edeltävän tarkemmin, tarkista tiedot, jotka tarjotaan [ExpressRoute-piiri-perinteinen resurssien hallinnan siirtäminen](expressroute-move.md). Varmista, että sinulla on täysin ymmärtää rajat ja rajoitukset, mikä on mahdollista.
- Jos haluat Azure ExpressRoute piiri Siirry Azure resurssien hallinnan käyttöönottomalli perinteinen käyttöönotto-mallin, sinulla on oltava piiri täysin määritetty ja toiminnallisia perinteinen käyttöönotto-mallin.
- Varmista, että resurssiryhmä, joka on luotu resurssien hallinnan käyttöönottomalli.

## <a name="move-the-expressroute-circuit-to-the-resource-manager-deployment-model"></a>Siirrä ExpressRoute piiri resurssien hallinnan käyttöönottomalli

Sinun on siirrettävä ExpressRoute piiri resurssien hallinnan käyttöönottomalli, niin, että voit käyttää sitä klassinen ja resurssien hallinnan käyttöönotto-mallit. Voit tehdä tämän suorittamalla PowerShell-komennoilla.

### <a name="step-1-gather-circuit-details-from-the-classic-deployment-model"></a>Vaihe 1: Kerääminen piiri tiedot perinteinen käyttöönottomalli

Haluat kerätä tietoja ExpressRoute piiri ensin.

Kirjaudu Azure perinteinen ympäristön ja kerääminen service-näppäintä. Voit käyttää seuraavia PowerShell-koodikatkelman tarvitsemasi tiedot:

    # Sign in to your Azure account
    Add-AzureAccount

    # Select the appropriate Azure subscription
    Select-AzureSubscription "<Enter Subscription Name here>"

    # Import the PowerShell modules for Azure and ExpressRoute
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

    # Get the service keys of all your ExpressRoute circuits
    Get-AzureDedicatedCircuit

Kopioi **palvelun avainta** virtapiirin, jonka haluat siirtää resurssien hallinnan käyttöönottomalli.

### <a name="step-2-sign-in-to-the-resource-manager-environment-and-create-a-new-resource-group"></a>Vaihe 2: Kirjaudu Resurssienhallinta-ympäristön ja luo uusi resurssiryhmä

Voit luoda uusi resurssiryhmä käyttämällä seuraavia koodikatkelman:

    # Sign in to your Azure Resource Manager environment
    Login-AzureRmAccount

    # Select the appropriate Azure subscription
    Get-AzureRmSubscription -SubscriptionName "<Enter Subscription Name here>" | Select-AzureRmSubscription

    #Create a new resource group if you don't already have one
    New-AzureRmResourceGroup -Name "DemoRG" -Location "West US"

Voit käyttää aiemmin resurssiryhmä myös, jos sinulla on jo jokin.

### <a name="step-3-move-the-expressroute-circuit-to-the-resource-manager-deployment-model"></a>Vaihe 3: Siirrä ExpressRoute piiri resurssien hallinnan käyttöönottomalli

Olet nyt valmiina siirtyy ExpressRoute piiri klassinen resurssien hallinnan käyttöönotto-malliin. Tarkista ennen kuin jatkat [siirtäminen ExpressRoute-piiri Resurssienhallinta käyttöönoton mallin classic-](expressroute-move.md) kohdasta tiedot.

Voit tehdä tämän suorittamalla seuraavat koodikatkelman:

    Move-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "DemoRG" -Location "West US" -ServiceKey "<Service-key>"

>[AZURE.NOTE] Kun siirto on valmis, uusi nimi, joka näkyy edellisen cmdlet-komento käytetään resurssin sähköpostiosoite. Virtapiirin olennaisesti nimetään.

## <a name="enable-an-expressroute-circuit-for-both-deployment-models"></a>Ota käyttöön ExpressRoute-piiri sekä käyttöönotto-malleissa

Sinun on siirrettävä ExpressRoute piiri resurssien hallinnan käyttöönottomalli ennen käyttöönottoa mallin käytön hallinnasta.

Suorita seuraavat cmdlet-komennolla sekä käyttöönotto-mallien käyttäminen:

    # Get details of the ExpressRoute circuit
    $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"

    #Set "Allow Classic Operations" to TRUE
    $ckt.AllowClassicOperations = $true

    # Update circuit
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

Kun tämä toiminto on suoritettu, voi tarkastella virtapiirin perinteinen käyttöönotto-mallissa.

Suorita ExpressRoute piiri tietoja seuraavasti:

    get-azurededicatedcircuit

Sinun on voitava on lueteltu service-näppäintä. Voit nyt hallita vakio perinteinen käyttöönoton malli-komentoja, joilla perinteinen VNets ja standard ARM-komentojen käyttäminen ARM VNETs ExpressRoute piiri linkkejä. Seuraavissa artikkeleissa opastaa hallinnasta ExpressRoute piiri linkit:

- [Linkki virtual verkon ExpressRoute-piiri Resurssienhallinta käyttöönotto-mallissa](expressroute-howto-linkvnet-arm.md)
- [Linkki virtual verkon ExpressRoute-piiri perinteinen käyttöönotto-mallissa](expressroute-howto-linkvnet-classic.md)


## <a name="disable-the-expressroute-circuit-to-the-classic-deployment-model"></a>ExpressRoute piiri perinteinen käyttöönoton mallin poistaminen käytöstä

Suorita seuraavat cmdlet-komento perinteinen käyttöönoton mallin käyttöoikeuksien poistaminen:

    # Get details of the ExpressRoute circuit
    $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"

    #Set "Allow Classic Operations" to FALSE
    $ckt.AllowClassicOperations = $false

    # Update circuit
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

Kun tämä toiminto on suoritettu, sinun ei voi tarkastella virtapiirin perinteinen käyttöönotto-mallissa.

## <a name="next-steps"></a>Seuraavat vaiheet

Kun olet luonut oman piiri, varmista, että teet jonkin seuraavista:

- [Luoda ja muokata reititys ExpressRoute piiri](expressroute-howto-routing-arm.md)
- [Linkki virtual verkon ExpressRoute piiri](expressroute-howto-linkvnet-arm.md)
