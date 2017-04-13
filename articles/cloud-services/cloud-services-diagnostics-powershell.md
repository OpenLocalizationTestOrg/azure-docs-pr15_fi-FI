<properties
    pageTitle="Ota käyttöön PowerShellin Azure pilvipalveluihin diagnostiikka | Microsoft Azure"
    description="Opi ottamaan diagnostiikkaa pilvipalveluihin PowerShellin avulla"
    services="cloud-services"
    documentationCenter=".net"
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>


# <a name="enable-diagnostics-in-azure-cloud-services-using-powershell"></a>Ota diagnostiikan Azure Cloud Services-palveluissa PowerShellin avulla

Voit kerätä diagnostiikan tietoja, kuten sovelluksen lokit suorituskyvyn laskuri pilvipalvelu, käyttämällä Azure diagnostiikka-tunniste jne. Tässä artikkelissa käsitellään Azure diagnostiikka-tunniste käyttöön pilvipalveluun PowerShellin avulla.  Katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md) varten tämän artikkelin tarvittavat yhteyden edellytykset ovat olemassa.

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>Ota diagnostiikan tunniste osana käyttöönotto pilvipalveluun

Tämän menetelmän hyvän jatkuva integroinnin tyyppi tilanteissa, joissa diagnostiikka-tunniste on käytössä osana käyttöönotto pilvipalvelussa. Kun luot uuden pilvipalvelussa käyttöympäristön välittäminen *ExtensionConfiguration* -parametrissa [Uusi AzureDeployment](https://msdn.microsoft.com/library/azure/mt589089.aspx) cmdlet-komennon avulla voidaan ottaa diagnostiikka-tunniste. *ExtensionConfiguration* -parametri on matriisin diagnostiikka-määrityksiä voidaan luoda [Uusi AzureServiceDiagnosticsExtensionConfig](https://msdn.microsoft.com/library/azure/mt589168.aspx) cmdlet-komennolla.

Seuraavassa esimerkissä esitetään, miten voit ottaa pilvipalvelussa WebRole ja WorkerRole diagnostiikka kukin ottavat eri diagnostiikka-määritys.

    $service_name = "MyService"
    $service_package = "CloudService.cspkg"
    $service_config = "ServiceConfiguration.Cloud.cscfg"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)

Jos Diagnostiikka-kokoonpanotiedosto määrittää StorageAccount elementti tallennustilan tilin nimellä, valitse uusi AzureServiceDiagnosticsExtensionConfig cmdlet-komento käyttää automaattisesti tallennustilan tilin. Tätä varten tallennustilan tilin on oltava sama otetaan käyttöön pilvipalvelussa-tilauksen.

Kohde-tulostus sisällytetään Azure SDK 2.6 alkaen tunniste määritysten tiedostot MSBuild luoma Julkaise-tallennustilan tilin nimi-diagnostiikka-määritysten merkkijono on määritetty palvelun määritystiedostossa (.cscfg) perusteella. Alla olevaa komentosarjaa näytetään, miten jäsentää tunniste määritysten tiedostot Julkaise kohde-tulostus ja diagnostiikka-tunniste kunkin roolin määrittäminen käyttöönotossa pilvipalvelussa.

    $service_name = "MyService"
    $service_package = "C:\build\output\CloudService.cspkg"
    $service_config = "C:\build\output\ServiceConfiguration.Cloud.cscfg"

    #Find the Extensions path based on service configuration file
    $extensionsSearchPath = Join-Path -Path (Split-Path -Parent $service_config) -ChildPath "Extensions"

    $diagnosticsExtensions = Get-ChildItem -Path $extensionsSearchPath -Filter "PaaSDiagnostics.*.PubConfig.xml"
    $diagnosticsConfigurations = @()
    foreach ($extPath in $diagnosticsExtensions)
    {
    #Find the RoleName based on file naming convention PaaSDiagnostics.<RoleName>.PubConfig.xml
    $roleName = ""
    $roles = $extPath -split ".",0,"simplematch"
    if ($roles -is [system.array] -and $roles.Length -gt 1)
        {
        $roleName = $roles[1]
        $x = 2
        while ($x -le $roles.Length)
            {
               if ($roles[$x] -ne "PubConfig")
                {
                    $roleName = $roleName + "." + $roles[$x]
                }
                else
                {
                    break
                }
                $x++
            }
        $fullExtPath = Join-Path -path $extensionsSearchPath -ChildPath $extPath
        $diagnosticsconfig = New-AzureServiceDiagnosticsExtensionConfig -Role $roleName -DiagnosticsConfigurationPath $fullExtPath
        $diagnosticsConfigurations += $diagnosticsconfig
        }
    }
    New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration $diagnosticsConfigurations

Visual Studio Online käyttää samaa menetelmää automaattinen pilvipalveluihin deploymnts diagnostiikka-tunniste. Katso täydellinen esimerkki [Julkaise AzureCloudDeployment.ps1](https://github.com/Microsoft/vso-agent-tasks/blob/master/Tasks/AzureCloudPowerShellDeployment/Publish-AzureCloudDeployment.ps1) .

Jos StorageAccount ei ole määritetty diagnostiikan määrityksissä, sinun on StorageAccountName-parametrissa siirtää-cmdlet-komennolla. Jos StorageAccountName-parametri on määritetty, valitse cmdlet käyttää aina tallennustilan tilin, joka on määritetty parametrin ja ei yksi, joka on määritetty diagnostiikka määritystiedostossa.

Jos Diagnostiikka-tallennustilan tilin on eri tilauksen perusteella, sinun on eksplisiittisesti välittää StorageAccountName ja StorageAccountKey Parameters-cmdlet-komennolla. StorageAccountKey-parametria ei tarvita, kun diagnostiikka tallennustilan-tili on saman tilauksen-cmdlet-komennolla voit kysely ja määritä avainarvon, kun otat käyttöön diagnostiikka-tunniste. Kuitenkin Jos Diagnostiikka-tallennustilan tilin on eri tilauksen, valitse cmdlet ei välttämättä voi hankkia avain automaattisesti eikä sinun tarvitse erikseen määrittää avaimen StorageAccountKey-parametrin kautta.

    $webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
    $workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key


## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>Ota käyttöön vianmäärityksen tunniste aiemmin pilvipalvelussa

Voit ottaa käyttöön tai päivittää diagnostiikka-pilvipalvelussa jo käynnissä [Määrittäminen AzureServiceDiagnosticsExtension](https://msdn.microsoft.com/library/azure/mt589140.aspx) cmdlet-komento.


    $service_name = "MyService"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    Set-AzureServiceDiagnosticsExtension -DiagnosticsConfiguration @($webrole_diagconfig,$workerrole_diagconfig) -ServiceName $service_name


## <a name="get-current-diagnostics-extension-configuration"></a>Hae diagnostiikka-tunniste nykyiset määritykset
Nykyinen diagnostiikka määrityskohde pilvipalveluun [Get-AzureServiceDiagnosticsExtension](https://msdn.microsoft.com/library/azure/mt589204.aspx) cmdlet-komennon avulla.

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"

## <a name="remove-diagnostics-extension"></a>Poista diagnostiikka-tunniste
Käytöstä diagnostiikka pilvipalveluun, voit käyttää [Poista AzureServiceDiagnosticsExtension](https://msdn.microsoft.com/library/azure/mt589183.aspx) cmdlet-komento.

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"

Jos otit *Määrittäminen AzureServiceDiagnosticsExtension* tai *Uusi AzureServiceDiagnosticsExtensionConfig* ilman *rooli* -parametrin diagnostiikka-tunniste voit poistaa tunnisteen, *Poista AzureServiceDiagnosticsExtension* käyttäminen ilman *rooli* -parametrin. Jos *rooli* -parametri on käytetty, kun otat käyttöön tunnisteen sitten se on myös poistettaessa tunniste.

Voit poistaa diagnostiikka-tunniste yksittäisiä rooleille:

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"


## <a name="next-steps"></a>Seuraavat vaiheet

- Katso lisäohjeita käyttämisestä Azure diagnostiikka- ja muita menetelmiä ongelmien [Ottaminen käyttöön diagnostiikka Azure Cloud Services-palveluissa ja näennäiskoneiden](cloud-services-dotnet-diagnostics.md).
- [Diagnostiikan määritysten rakenteen](https://msdn.microsoft.com/library/azure/dn782207.aspx) kerrotaan diagnostiikka-tunniste eri xml-määrityksiä asetukset.
- Lisätietoja diagnostiikka-tunniste käyttöön näennäiskoneiden on artikkelissa [luominen Windowsin Virtual tietokoneessa, jossa seuranta- ja diagnostiikka Azure Resurssienhallinta mallin avulla](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)  
