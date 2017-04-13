<properties
    pageTitle="Yleistä Azure resurssiryhmä projektin käyttöönoton komentosarja | Microsoft Azure"
    description="Tässä artikkelissa kuvataan Azure resurssiryhmä käyttöönotto-projektin PowerShell-komentosarjaa toiminta."
    services="visual-studio-online"
    documentationCenter="na"
    authors="tfitzmac"
    manager="timlt"
    editor="" />

 <tags
    ms.service="azure-resource-manager"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="07/26/2016"
    ms.author="tomfitz" />

# <a name="overview-of-the-azure-resource-group-project-deployment-script"></a>Azure resurssiryhmä projektin käyttöönoton komentosarja yleiskatsaus

Azure resurssiryhmä käyttöönoton projektit apua vaiheen ja ota käyttöön tiedostoja ja muita tietoja Azure. Kun luot Azure resurssien hallinnan käyttöönotto-projektin Visual Studiossa, kutsutaan **Käyttöönotto AzureResourceGroup.ps1** PowerShell-komentosarjaa on lisätty projektiin. Tämä artikkeli sisältää tiedot tämä komentosarja mitä ja suorittaa sen sisällä ja Visual Studio ulkopuolella.

## <a name="what-does-the-script-do"></a>Mitä komentosarja tarkoittaa?

Ota käyttöön AzureResourceGroup.ps1 komentosarja tekee kaksi asiaa, jotka ovat tärkeitä käyttöönoton työnkulun.

- Lataa tiedostot ja palvelutiedot tarvitaan mallin käyttöönottoa varten
- Mallin ottaminen käyttöön

Komentosarjan ensimmäisen osan Lataa tiedostot ja palvelutiedot käyttöönottoa varten ja viimeisen cmdlet-komento komentosarjan todella otetaan käyttöön malli. Esimerkiksi jos virtual machine varten on määritettävä komentosarjan kanssa, käyttöönoton komentosarja ensin suojatusti Lataa kokoonpanon komentosarjaa Azure-tallennustilan tilin. Tällöin käytettävissä Azure resurssien hallinnan määrittämiseen virtuaalikoneen valmistelun aikana.

Koska kaikki mallin ominaisuuksissa on on ylimääräisiä palvelutiedot, joka on ladattava, Vaihda parametrin nimeltä *uploadArtifacts* arvioidaan. Jos kaikki tiedot on ladattava, Määritä *uploadArtifacts* Vaihda komentosarja soitettaessa. Huomaa, että tärkeimmät mallitiedosto ja parametrit-tiedostoa ei tarvitse voi ladata. Vain muut tiedostot, kuten määritysten komentosarjoja sisäkkäinen käyttöönoton mallit ja sovelluksen tiedostoista on ladattava.

## <a name="detailed-script-description"></a>Komentosarjan yksityiskohtainen kuvaus

Seuraavassa on kuvaus käyttöönotto AzureResourceGroup.ps1 PowerShellin Azure komentosarjan Älä valitse osiot.

>[AZURE.NOTE] Tässä artikkelissa käyttöönotto AzureResourceGroup.ps1 komentosarja 1.0 versio.

1.  Määritellä tarvitsemia Azure resurssien hallinnan käyttöönotto-projektin parametrit. Jotkin parametrit on oletusarvo, jotka olivat valittuina, milloin projekti on luotu. Voit muuttaa oletusarvoja komentosarjan tai lisätä eri parametriarvot ennen komentosarjan suorittaminen.

    ```
    Param(
      [string] [Parameter(Mandatory=$true)] $ResourceGroupLocation,
      [string] $ResourceGroupName = 'AzureResourceGroup1',
      [switch] $UploadArtifacts,
      [string] $StorageAccountName,
      [string] $StorageAccountResourceGroupName,
      [string] $StorageContainerName = $ResourceGroupName.ToLowerInvariant() + '-stageartifacts',
      [string] $TemplateFile = '..\Templates\azuredeploy.json',
      [string] $TemplateParametersFile = '..\Templates\azuredeploy.parameters.json',
      [string] $ArtifactStagingDirectory = '..\bin\Debug\staging',
      [string] $AzCopyPath = '..\Tools\AzCopy.exe',
      [string] $DSCSourceFolder = '..\DSC'
    )
    ```

  	|Parametri|Kuvaus|
  	|---|---|
  	|$ResourceGroupLocation|Alueen tai tietojen Keskitä resurssiryhmän, kuten **Länsi US** tai **Itä-Aasian**sijainti.|
  	|$ResourceGroupName|Azure resurssiryhmän nimi.|
  	|$UploadArtifacts|Binaarinen arvo, joka ilmaisee, onko palvelutiedot on ladattava Azure järjestelmästä.|
  	|$StorageAccountName|Jossa oman palvelutiedot ladataan Azure-tallennustilan tilin nimi.|
  	|$StorageAccountResourceGroupName|Azure resurssiryhmän, joka sisältää tallennustilan tilin nimi.|
  	|$StorageContainerName|Käyttää ladataan palvelutiedot säilytykseen nimi.|
  	|$TemplateFile|Käyttöönotto-tiedoston polku (`<app name>.json`) Azure resurssiryhmä projektin.|
  	|$TemplateParametersFile|Parametrit-tiedoston polku (`<app name>.parameters.json`) Azure resurssiryhmä projektin.|
  	|$ArtifactStagingDirectory|Järjestelmään, jossa palvelutiedot paikallisesti ladataan, mukaan lukien PowerShell-komentosarjaa pääkansion polku. Tämä polku voi olla suoria tai komentosarjan sijainnin suhteessa.|
  	|$AzCopyPath|Jossa AzCopy.exe työkalu kopioi .zip-tiedostoja, kuten PowerShell-komentosarjaa pääkansion polku. Tämä polku voi olla suoria tai komentosarjan sijainnin suhteessa.|
  	|$DSCSourceFolder|DSC (toivottuja tilan määritys) lähde-kansioon, mukaan lukien PowerShell-komentosarjaa pääkansion polku. Tämä polku voi olla suoria tai komentosarjan sijainnin suhteessa. Katso [esittely Azure PowerShell DSC (toivottuja tilan määritys)-tunniste](http://blogs.msdn.com/b/powershell/archive/2014/08/07/introducing-the-azure-powershell-dsc-desired-state-configuration-extension.aspx), tarvittaessa lisätietoja.|

1.  Tarkista, onko palvelutiedot on ladattava Azure. Muussa tapauksessa siirry vaiheeseen 11. Muussa tapauksessa toimimalla seuraavasti.

1.  Muunna kaikki muuttujat suhteellinen liikeradan absoluuttiset polut. Esimerkiksi liikeradan muuttaminen, kuten `..\Tools\AzCopy.exe` , `C:\YourFolder\Tools\AzCopy.exe`. Lisäksi alusta muuttujat *ArtifactsLocationName* ja *ArtifactsLocationSasTokenName* Null. *ArtifactsLocation* ja *SaSToken* voidaan parametrit-malliin. Jos niiden arvojen ei null, kun lukeminen parametrit-tiedostossa, komentosarja luo niiden arvoja.

    Azure-Työkalut käyttää mallissa parametrin arvot *_artifactsLocation* ja *_artifactsLocationSasToken* palvelutiedot hallitsemiseen. Jos PowerShell-komentosarjaa löytää parametrien nimet, mutta parametriarvot ei anneta, komentosarja latauksia palvelutiedot ja palauttaa ne parametrien tarvittavat arvot. Sen jälkeen siirtää ne cmdlet-komennon kautta `@OptionsParameters`.

  	|Muuttujan|Kuvaus|
  	|---|---|
  	|ArtifactsLocationName|Jossa Azure palvelutiedot sijaitsevat polku.|
  	|ArtifactsLocationSasTokenName|SAS (jaettu Access allekirjoitus) tunnuksen nimi, jota käytetään komentosarja palvelun Bus todentamiseen. Lisätietoja on kohdassa [Jaettu palvelun Bus Access allekirjoituksen todentaminen](service-bus-shared-access-signature-authentication.md) .|

    ```
    if ($UploadArtifacts) {
    # Convert relative paths to absolute paths if needed
    $AzCopyPath = [System.IO.Path]::Combine($PSScriptRoot, $AzCopyPath)
    $ArtifactStagingDirectory = [System.IO.Path]::Combine($PSScriptRoot, $ArtifactStagingDirectory)
    $DSCSourceFolder = [System.IO.Path]::Combine($PSScriptRoot, $DSCSourceFolder)

    Set-Variable ArtifactsLocationName '_artifactsLocation' -Option ReadOnly
    Set-Variable ArtifactsLocationSasTokenName '_artifactsLocationSasToken' -Option ReadOnly

    $OptionalParameters.Add($ArtifactsLocationName, $null)
    $OptionalParameters.Add($ArtifactsLocationSasTokenName, $null)
    ```

1.  Tämän osan tarkistukset onko <app name>. parameters.json-tiedosto (jota kutsutaan "parametrit tiedosto") on ylemmän tason solmu nimeltä **Parametrit** (- `else` estä). Muussa tapauksessa on ei ole Ylemmän tason solmu. Jommankumman muotoilun ovat oikein.
    
    ```
    if ($JsonParameters -eq $null) {
            $JsonParameters = $JsonContent
        }
        else {
            $JsonParameters = $JsonContent.parameters
        }
    ```

1.  Käydä läpi JSON parametrien kokoelma. Jos parametriarvo on määritetty *_artifactsLocation* tai *_artifactsLocationSasToken*, Määritä muuttujan *$OptionalParameters* kyseiset arvot. Tämä estää komentosarjan korvaaminen vahingossa antaisit parametriarvot.

    ```
    $JsonParameters | Get-Member -Type NoteProperty | ForEach-Object {
        $ParameterValue = $JsonParameters | Select-Object -ExpandProperty $_.Name

        if ($_.Name -eq $ArtifactsLocationName -or $_.Name -eq $ArtifactsLocationSasTokenName) {
            $OptionalParameters[$_.Name] = $ParameterValue.value
        }
    }
    ```

1.  Hanki tallennustilan tilin avain ja konteksti tallennustilan tilin resurssin käyttää palvelutiedot käyttöönottoa varten.

    ```
    $StorageAccountKey = (Get-AzureRMStorageAccountKey -ResourceGroupName $StorageAccountResourceGroupName -Name $StorageAccountName).Key1

    $StorageAccountContext = (Get-AzureRmStorageAccount -ResourceGroupName $StorageAccountResourceGroupName -Name $StorageAccountName).Context
    ```

1.  Jos käytät PowerShell DSC virtual koneen DSC käyttöajan pidentämiseksi palvelutiedot olevan yksittäisen zip-tiedoston. Luo, .zip-arkistotiedostoja DSC kokoonpanoa varten. Toiminto, tarkista, onko $DSCSourceFolder. Jos on olemassa DSC määrityksen, poista se ja luo sitten uusi pakattu tiedosto nimeltä dsc.zip.

    ```
    # Create DSC configuration archive
    if (Test-Path $DSCSourceFolder) {
    Add-Type -Assembly System.IO.Compression.FileSystem
        $ArchiveFile = Join-Path $ArtifactStagingDirectory "dsc.zip"
        Remove-Item -Path $ArchiveFile -ErrorAction SilentlyContinue
        [System.IO.Compression.ZipFile]::CreateFromDirectory($DSCSourceFolder, $ArchiveFile)
    }
    ```

1.  Jos ei ole Azure palvelutiedot polku on annettu parametrit-tiedostossa, määrittää käyttämään palvelutiedot ladattaessa PowerShell-komentosarjaa polku. Voit tehdä tämän luoda polku yhdistelmän tallennustilan tilin päätepisteen polku sekä tallennustilan säilö nimi. Päivitä sitten parametrit-tiedoston uuteen polulla.

    ```
    # Generate the value for artifacts location if it is not provided in the parameter file
    $ArtifactsLocation = $OptionalParameters[$ArtifactsLocationName]
    if ($ArtifactsLocation -eq $null) {
        $ArtifactsLocation = $StorageAccountContext.BlobEndPoint + $StorageContainerName
        $OptionalParameters[$ArtifactsLocationName] = $ArtifactsLocation
    }
    ```

1.  Kopioi kaikki tiedostot paikalliseen tallennustilan avattavan polkuun online Azure-tallennustilan tilin **AzCopy** -apuohjelma (sisältyy Azure resurssiryhmä käyttöönoton projektin **Työkalut** -kansiossa) avulla. Jos tämä vaihe ei onnistu, Lopeta komentosarja, koska käyttöönoton ei todennäköisesti onnistu ilman tarvittavat tiedot.

    ```
    # Use AzCopy to copy files from the local storage drop path to the storage account container
    & $AzCopyPath """$ArtifactStagingDirectory""", $ArtifactsLocation, "/DestKey:$StorageAccountKey", "/S", "/Y", "/Z:$env:LocalAppData\Microsoft\Azure\AzCopy\$ResourceGroupName"
    if ($LASTEXITCODE -ne 0) { return }
    ```

1.  Jos SAS-tunnuksen palvelutiedot sijainti ei ole parametrit-tiedostossa, luo sellainen tilapäinen vain luku-online säilytykseen pääsy. Välittää sitten kyseisen SAS tunnuksen cmdline voin kuin "optionalParameter." Huomaa, että parametreja cmdline on kulkenut ohittavat arvojen parametrit-tiedosto.

    ```
    # Generate the value for artifacts location SAS token if it is not provided in the parameter file
    $ArtifactsLocationSasToken = $OptionalParameters[$ArtifactsLocationSasTokenName]
    if ($ArtifactsLocationSasToken -eq $null) {
       # Create a SAS token for the storage container - this gives temporary read-only access to the container
       $ArtifactsLocationSasToken = New-AzureStorageContainerSASToken -Container $StorageContainerName -Context $StorageAccountContext -Permission r -ExpiryTime (Get-Date).AddHours(4)
       $ArtifactsLocationSasToken = ConvertTo-SecureString $ArtifactsLocationSasToken -AsPlainText -Force
       $OptionalParameters[$ArtifactsLocationSasTokenName] = $ArtifactsLocationSasToken
    }
    ```

1.  Luo resurssiryhmän, jos sitä ei ole jo ole ja tarkista kelpoisuustarkistuksen virheet, jotka estävät käyttöönoton onnistumisen mallia ja parametrit-tiedosto.

    ```
    # Create or update the resource group using the specified template file and template parameters file
    New-AzureRMResourceGroup -Name $ResourceGroupName -Location $ResourceGroupLocation -Verbose -Force -ErrorAction Stop

    Test-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroupName -TemplateFile $TemplateFile -TemplateParameterFile $TemplateParametersFile @OptionalParameters -ErrorAction Stop
    ```

1. Käyttöönotto-malli. Tämä koodi luo aikaleima käyttöönottoa yksilöllinen nimi.

    ```
    New-AzureRMResourceGroupDeployment -Name ((Get-ChildItem $TemplateFile).BaseName + '-' + ((Get-Date).ToUniversalTime()).ToString('MMdd-HHmm')) `
        -ResourceGroupName $ResourceGroupName `
        -TemplateFile $TemplateFile `
        -TemplateParameterFile $TemplateParametersFile `
        @OptionalParameters `
        -Force -Verbose
    ```

## <a name="deploy-the-resource-group"></a>Resurssiryhmä käyttöönotto

### <a name="to-deploy-the-resource-group-in-visual-studio"></a>Ottaa käyttöön Visual Studiossa resurssiryhmä

1. Valitse pikavalikosta Azure resurssiryhmä projektin **käyttöönotto** > **Uudet käyttöönoton**.

    ![][0]

1. **Ota resurssiryhmä** -valintaikkunassa joko valita aiemmin resurssiryhmä avattavassa luettelossa ottaa käyttöön tai valitse ** &lt;Luo uusi... &gt;** Luo uusi resurssiryhmä.

    ![][1]

1. Pyydettäessä resurssiryhmän nimi ja sijainti Kirjoita **Luominen resurssiryhmä** -valintaikkunaan ja valitse sitten **Luo** -painiketta.

    ![][2]

1. Voit **Muokata parametrit** -valintaikkuna ja kirjoita sitten puuttuu parametriarvot **Muokkaa parametrit** -painiketta.

    ![][3]

    >[AZURE.NOTE] Jos tarvittavat parametrit on arvoja, tämä valintaikkuna avautuu automaattisesti, kun otat käyttöön.

    ![][4]

1. Kun olet valmis parametrin arvot, valitse **Tallenna** -painiketta ja valitse **Ota käyttöön** -painiketta.

    Käyttöönoton komentosarja (Ota käyttöön-AzureResourceGroup.ps1) suoritetaan ja mallin, minkä tahansa palvelutiedot sekä ottaa käyttöön Azure.

### <a name="to-deploy-the-resource-group-by-using-powershell"></a>Ottaa käyttöön resurssiryhmän PowerShell-toiminnolla

Jos haluat suorittaa komentosarjan käyttämättä Visual Studio käyttöönotto-komento ja Käyttöliittymä, komentosarja pikavalikon, valitse **Avaa PowerShell ise:**.

![][5]


## <a name="command-deployment-examples"></a>Komennon käyttöönoton esimerkkejä

### <a name="deploy-using-default-values"></a>Ottaa käyttöön käyttämällä oletusarvoja

Tässä esimerkissä näytetään käyttämällä parametrin oletusarvoja komentosarjan suorittaminen. (Koska sijainti-parametria ei ole oletusarvon, voit tarvittaessa tekemään.)

`.\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation eastus`

### <a name="deploy-overriding-the-default-values"></a>Ottaa käyttöön oletusarvot ohittaminen

Tässä esimerkissä näytetään ottamaan mallia ja parametrit tiedostoja, jotka poikkeavat oletusarvot komentosarjan suorittaminen.

```
.\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation eastus –TemplateFile ..\templates\AnotherTemplate.json –TemplateParametersFile ..\templates\AnotherTemplate.parameters.json
```

### <a name="deploy-using-uploadartifacts-for-staging"></a>Ottaa käyttöön UploadArtifacts käytön vaiheet

Tässä esimerkissä näytetään ladataan palvelutiedot release-kansiosta ja muun kuin oletusarvoisen mallit käyttöön komentosarjan suorittaminen.

```
.\Deploy-AzureResourceGroup.ps1 -StorageAccountName 'mystorage' -StorageAccountResourceGroupName 'Default-Storage-EastUS' -ResourceGroupName 'myResourceGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\windowsvirtualmachine.json' -TemplateParametersFile '..\templates\windowsvirtualmachine.parameters.json' -UploadArtifacts -ArtifactStagingDirectory ..\bin\release\staging
```

Tässä esimerkissä näytetään komentosarjan suorittaminen PowerShellin Azure tehtävässä Visual Studio Onlinessa.

```
$(Build.StagingDirectory)/AzureResourceGroup1/Scripts/Deploy-AzureResourceGroup.ps1 -StorageAccountName 'mystorage' -StorageAccountResourceGroupName 'Default-Storage-EastUS' -ResourceGroupName 'myResourceGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\windowsvirtualmachine.json' -TemplateParametersFile '..\templates\windowsvirtualmachine.parameters.json' -UploadArtifacts -ArtifactStagingDirectory $(Build.StagingDirectory)
```

## <a name="next-steps"></a>Seuraavat vaiheet
Lue lisää tietoja Azure Resurssienhallinta [Azure resurssin hallinnassa: yleiskatsaus](azure-resource-manager/resource-group-overview.md).

Katso Lisää esimerkkejä Azure resurssiryhmä projektien käsitteleminen [käyttöönotto ja Azure resurssien](https://github.com/Microsoft/HealthClinic.biz/wiki/Deploy-and-manage-Azure-resources) - [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Yhdistä [esittely](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). Katso Lisää quickstarts-HealthClinic.biz esittely, [Azure Developer Työkalut Quickstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).

[0]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy1c.png
[1]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy2bc.png
[2]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy3bc.png
[3]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy4bc.png
[4]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy5c.png
[5]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy6c.png
