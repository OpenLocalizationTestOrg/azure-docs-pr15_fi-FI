<properties
    pageTitle="Siirtää, resurssien hallinta PowerShellin avulla | Microsoft Azure"
    description="Tässä artikkelissa käydään läpi ympäristö tuettu siirtämistä IaaS resurssit-perinteinen Azure resurssien hallinta PowerShellin Azure komennoilla"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="cynthn"/>

# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-powershell"></a>Siirtää IaaS resurssien perinteinen Azure resurssien hallinnan Azure PowerShell-toiminnolla

Seuraavia ohjeita noudattamalla voit PowerShellin Azure-komentojen avulla voit siirtää infrastruktuurin service (IaaS) resursseina perinteinen käyttöönoton mallista Azure resurssien hallinnan käyttöönottomalli. 

Jos haluat, voit myös siirtää resurssien [Azure rivin komentoliittymä (Azure CLI)](virtual-machines-linux-cli-migration-classic-resource-manager.md)avulla.

- Taustatietoa tuetut siirron tilanteita, joissa on [IaaS resurssit perinteinen Azure resurssien hallinnan ympäristö tuettu siirto](virtual-machines-windows-migration-classic-resource-manager.md). 
- Yksityiskohtaiset ohjeet ja vaiheittainen siirto on artikkelissa [tekninen perinpohjaisesti käsittelevään artikkeliin-ympäristö tuettu siirto-perinteinen Azure resurssien hallinta](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md).

## <a name="step-1-plan-for-migration"></a>Vaihe 1: Siirron suunnitteleminen

Seuraavassa on joitakin parhaita käytäntöjä, joilla on suositeltavaa, kun arvioit siirretään IaaS resurssit perinteinen resurssien hallinta:

- Lue [tuetut ja ei-tuetut ominaisuudet ja määritykset](virtual-machines-windows-migration-classic-resource-manager.md). Jos sinulla ei ole tuettu määrityksiä tai ominaisuuksia käyttäviä näennäiskoneiden, on suositeltavaa odottaa ilmoitetaan kokoonpano-ominaisuus-tukea. Myös jos sen tarpeisiisi, toiminto poistaa tai siirtää pois, määritys käyttöön siirron.
-   Jos on automatisoitu komentosarjoja tähän tarkoitukseen infrastruktuuri ja sovellusten käyttöönotto tänään, yritä luoda käyttämällä komentosarjoja siirron samalla testi-asetukset. Vaihtoehtoisesti voit määrittää otoksen ympäristöissä mukaan Azure-portaalissa.

>[AZURE.IMPORTANT]VPN yhdyskäytävien (sivuston-,-sivusto, Azure ExpressRoute-sovelluksen yhdyskäytävän, pisteen sivustoon) ei tueta tällä hetkellä siirtymisen klassinen resurssien hallinnan varten. Siirtää yhdyskäytävän perinteinen virtual verkosta, poistaa yhdyskäytävän ennen kuin suoritat Vahvista-toimintoa, voit siirtää verkon (Voit suorittaa valmisteleminen vaihe poistamatta yhdyskäytävän). Kun olet suorittanut siirron, yhdistää yhdyskäytävän Azure Resurssienhallinta.

## <a name="step-2-install-the-latest-version-of-azure-powershell"></a>Vaihe 2: Asenna uusin versio PowerShellin Azure

Asenna PowerShellin Azure tärkeimmät vaihtoehdot ovat: [PowerShell-valikoiman](https://www.powershellgallery.com/profiles/azure-sdk/) tai [WWW-ympäristö asennusohjelma (WebPI)](http://aka.ms/webpi-azps). WebPI vastaanottaa kuukausittain päivitykset. PowerShell-valikoiman vastaanottaa päivityksiä jatkuvasti. Tässä artikkelissa perustuu PowerShellin Azure versio 2.1.0.

Asennusohjeet Katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md).

<br>
## <a name="step-3-set-your-subscription-and-sign-up-for-migration"></a>Vaihe 3: Määritä tilauksen ja siirron rekisteröityminen

Käynnistä PowerShell-kehotteessa. Siirron, sinun on sekä perinteinen ympäristön määrittäminen ja Resurssienhallinta.

Kirjaudu tiliisi Resurssienhallinta-malli.

```powershell
    Login-AzureRmAccount
```

Hae käytettävissä tilaukset käyttämällä seuraava komento:

```powershell
    Get-AzureRMSubscription | Sort SubscriptionName | Select SubscriptionName
```

Määritä Azure tilauksen nykyisessä istunnossa. Tässä esimerkissä tilauksen oletusnimen **Oma Azure**-tilaukseen. Korvaa omalla Esimerkki tilauksen nimi. 

```powershell
    Select-AzureRmSubscription –SubscriptionName "My Azure Subscription"
```

>[AZURE.NOTE] Rekisteröinti on erikseen vaihe, mutta se on tehtävä vain kerran, ennen kuin yrität siirron. Ilman rekisteröinti, näet seuraavan virhesanoman: 

>   *BadRequest: Tilaus ei ole rekisteröity siirtoa varten.* 

Rekisteröi siirron resurssi-palvelussa käyttämällä seuraava komento:

```powershell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Odota viisi minuuttia rekisteröinnin loppuun. Voit tarkistaa hyväksynnän tila käyttämällä seuraava komento:

```powershell
    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Varmista, että RegistrationState on `Registered` ennen kuin jatkat. 

Kirjaudu nyt perinteinen mallin-tiliisi.

```powershell
    Add-AzureAccount
```

Hae käytettävissä tilaukset käyttämällä seuraava komento:

```powershell
    Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
```

Määritä Azure tilauksen nykyisessä istunnossa. Tässä esimerkissä oletusarvo-tilauksen **Oma Azure**-tilaukseen. Korvaa omalla Esimerkki tilauksen nimi. 

```powershell
    Select-AzureSubscription –SubscriptionName "My Azure Subscription"
```

<br>

## <a name="step-4-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-the-azure-region-of-your-current-deployment-or-vnet"></a>Vaihe 4: Varmista, että sinulla on tarpeeksi Azure Resurssienhallinta virtuaalikoneen sydämiä Azure alueen nykyisen käyttöönottoa tai VNET

Voit tarkistaa käytössäsi Azure Resurssienhallinta sydämiä määrä PowerShell-komentoa. Lisätietoja core kiintiön on artikkelissa [rajoitukset ja Azure Resurssienhallinta](../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager). 

Tässä esimerkissä tarkistaa käytettävyyden **Länsi USA** alueen. Esimerkki alueen nimi korvaaminen omalla. 

```powershell
Get-AzureRmVMUsage -Location "West US"
```

## <a name="step-5-run-commands-to-migrate-your-iaas-resources"></a>Vaihe 5: Suorita komennot, joilla IaaS resurssien siirtäminen

>[AZURE.NOTE] Kaikki tässä kuvatut toiminnot ovat idempotent. Jos sinulla on ongelmia kuin ominaisuus jota ei tueta tai määritysvirhe, on suositeltavaa valmisteleminen, yritä uudelleen Keskeytä tai Vahvista toiminto. Ympäristön yrittää toiminnon uudelleen.

### <a name="migrate-virtual-machines-in-a-cloud-service-not-in-a-virtual-network"></a>Siirtää näennäiskoneiden pilvipalvelussa (eivät sisälly virtual verkon)

Hae pilvipalveluihin luettelo käyttämällä seuraava komento ja valitse sitten pilvipalvelussa, jonka haluat siirtää. VMs pilvipalvelussa ovatko virtual verkossa tai jos ne on verkossa tai työntekijä roolit-komento palauttaa virhesanoman.

```powershell
    Get-AzureService | ft Servicename
```

Hae pilvipalvelussa käyttöönoton nimi. Tässä esimerkissä palvelunimi on **Oma palvelu**. Korvaa palvelun esimerkkinimi palvelun omaa nimeäsi. 

```powershell
    $serviceName = "My Service"
    $deployment = Get-AzureDeployment -ServiceName $serviceName
    $deploymentName = $deployment.DeploymentName
```

Valmistele näennäiskoneiden pilvipalvelussa siirtoa varten. Käytettävissä on kaksi vaihtoehtoista valittavana.

* **Vaihtoehto 1. Siirtää VMs ympäristö luodaan virtual verkkoon**

    Tarkista ensin, jos pilvipalvelussa, seuraavat komennot avulla voit siirtää:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    $validate.ValidationMessages
    ```

    Edeltävä komento näyttää kaikki varoitukset ja virheet, jotka estävät siirron. Jos vahvistus on tarkistettu, valitse **Valmistele** vaiheeseen siirtäminen:

    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    ```

* **Vaihtoehto 2. Siirtää aiemmin virtual verkon resurssien hallinnan käyttöönottomalli**

    Tässä esimerkissä resurssin ryhmänimi **myResourceGroup**, **myVirtualNetwork** VPN nimi ja **mySubNet**aliverkon nimi. Esimerkissä nimien tilalle oman resurssien nimet.

    ```powershell
    $existingVnetRGName = "myResourceGroup"
    $vnetName = "myVirtualNetwork"
    $subnetName = "mySubNet"
    ```

    Tarkista ensin, jos voit siirtää pilvipalvelussa, käyttämällä seuraavaa komentoa:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName -VirtualNetworkName $vnetName -SubnetName $subnetName
    $validate.ValidationMessages
    ```

    Edeltävä komento näyttää kaikki varoitukset ja virheet, jotka estävät siirron. Jos vahvistus on tarkistettu, voit jatka vaiheesta seuraavat valmisteleminen:

    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName -DeploymentName $deploymentName `
        -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName `
        -VirtualNetworkName $vnetName -SubnetName $subnetName
    ```
    

Kun valmistelu on luotu joko edellisen asetukset, kyselyn VMs siirron tila. Varmista, että ne ovat `Prepared` tila.

Tässä esimerkissä määrittää AM nimeksi **myVM**. Esimerkkinimi tilalle AM omaa nimeäsi.

    ```powershell
    $vmName = "myVM"
    $vm = Get-AzureVM -ServiceName $serviceName -Name $vmName
    $vm.VM.MigrationState
    ```

Tarkista määritykset valmiita resurssien PowerShell tai Azure portaalin. Jos et ole valmis siirtoa varten ja haluat palata vanha tila, kirjoita seuraava komento:

```powershell
    Move-AzureService -Abort -ServiceName $serviceName -DeploymentName $deploymentName
```

Jos valmiita määritys näyttää hyvältä, voit siirtää eteenpäin ja vahvista resurssit käyttämällä seuraava komento:

```powershell
    Move-AzureService -Commit -ServiceName $serviceName -DeploymentName $deploymentName
```

### <a name="migrate-virtual-machines-in-a-virtual-network"></a>Siirtää näennäiskoneiden virtual verkossa

Siirtää näennäiskoneiden virtual verkossa, voit siirtää verkkoon. Näennäiskoneiden Siirry automaattisesti liittyvään verkostoon. Valitse virtual verkkoon, jonka haluat siirtää. 

Tässä esimerkissä määrittää **myVnet**VPN-nimi. Korvaa VPN esimerkkinimi omalla. 

```powershell
    $vnetName = "myVnet"
```

>[AZURE.NOTE] Jos virtual verkko on verkossa tai työntekijä roolit tai VMs ei tueta määritykset, näkyviin tulee vahvistus-virhesanoma.

Tarkista ensin, jos voit siirtää virtual verkon käyttämällä seuraavaa komentoa:

```powershell
    Move-AzureVirtualNetwork -Validate -VirtualNetworkName $vnetName
```

Edeltävä komento näyttää kaikki varoitukset ja virheet, jotka estävät siirron. Jos vahvistus on tarkistettu, voit jatka vaiheesta seuraavat valmisteleminen:

```powershell
    Move-AzureVirtualNetwork -Prepare -VirtualNetworkName $vnetName
```

Tarkista määritykset valmiita näennäiskoneiden PowerShellin Azure tai Azure portaalin. Jos et ole valmis siirtoa varten ja haluat palata vanha tila, kirjoita seuraava komento:

```powershell
    Move-AzureVirtualNetwork -Abort -VirtualNetworkName $vnetName
```

Jos valmiita määritys näyttää hyvältä, voit siirtää eteenpäin ja vahvista resurssit käyttämällä seuraava komento:

```powershell
    Move-AzureVirtualNetwork -Commit -VirtualNetworkName $vnetName
```

### <a name="migrate-a-storage-account"></a>Siirrä tallennustilan-tili

Kun olet siirtänyt näennäiskoneiden, on suositeltavaa siirtää tallennustilan tilit.

Tallennustilan tileille siirtoon valmistautuminen seuraavat-komennon avulla. Tässä esimerkissä tallennustilan tilin nimi on **myStorageAccount**. Korvaa oman tallennustilan tilin nimi, esimerkiksi nimi. 

```powershell
    $storageAccountName = "myStorageAccount"
    Move-AzureStorageAccount -Prepare -StorageAccountName $storageAccountName
```

Tarkista valmiita tallennustilan tilin määritys PowerShellin Azure tai Azure portaalin. Jos et ole valmis siirtoa varten ja haluat palata vanha tila, kirjoita seuraava komento:

```powershell
    Move-AzureStorageAccount -Abort -StorageAccountName $storageAccountName
```

Jos valmiita määritys näyttää hyvältä, voit siirtää eteenpäin ja vahvista resurssit käyttämällä seuraava komento:

```powershell
    Move-AzureStorageAccount -Commit -StorageAccountName $storageAccountName
```

## <a name="next-steps"></a>Seuraavat vaiheet

- Katso lisätietoja siirron [ympäristö tuettu siirto IaaS resurssit perinteinen Azure resurssien hallinta](virtual-machines-windows-migration-classic-resource-manager.md).
- Muita verkkoresursseja siirtämisestä, Resurssienhallinta Azure PowerShellin avulla on samalla tavalla [Siirrä AzureNetworkSecurityGroup](https://msdn.microsoft.com/library/mt786729.aspx), [Siirrä AzureReservedIP](https://msdn.microsoft.com/library/mt786752.aspx)ja [Siirrä AzureRouteTable](https://msdn.microsoft.com/library/mt786718.aspx).
- Avaa lähde-komentosarjojen avulla voit siirtää Azure resurssien perinteinen resurssien hallinta-kohdassa [yhteisön työkaluja Azure Resurssienhallinta siirron siirto](virtual-machines-windows-migration-scripts.md)
