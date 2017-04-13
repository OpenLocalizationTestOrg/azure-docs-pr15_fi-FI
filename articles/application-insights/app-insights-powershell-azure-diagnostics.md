<properties
    pageTitle="PowerShellin määritys sovelluksen tiedot Azure-tietokannassa | Microsoft Azure"
    description="Automatisoida määrittäminen Azure diagnostiikka putkien sovelluksen havainnollistamisen."
    services="application-insights"
    documentationCenter=".net"
    authors="sbtron"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="11/17/2015"
    ms.author="awills"/>

# <a name="using-powershell-to-set-up-application-insights-for-an-azure-web-app"></a>Hakemuksen tiedot Azure web-sovelluksen määrittäminen PowerShellin avulla

[Microsoft Azure](https://azure.com) voidaan [määrittää lähettämään Azure diagnostiikka](app-insights-azure-diagnostics.md) [Visual Studio sovelluksen](app-insights-overview.md)havainnollistamisen. Diagnostiikkaa liittyvät Azure Pilvipalveluiden ja Azure VMs. Telemetriatietojen käyttämällä sovelluksen havainnollistamisen SDK-paketissa sovelluksen kautta lähettämiesi yhdessä. Osana automatisointi uudet resurssit luominen Azure-tietokannassa voit määrittää diagnostiikka PowerShellin avulla.

## <a name="azure-template"></a>Azure-malli

Jos luot Azure Resurssienhallinta-mallin avulla resurssien web-sovellus on Azure, voit määrittää sovelluksen tietoja lisäämällä tämän resurssit-solmu:

    {
      resources: [
        /* Create Application Insights resource */
        {
          "apiVersion": "2015-05-01",
          "type": "microsoft.insights/components",
          "name": "nameOfAIAppResource",
          "location": "centralus",
          "kind": "web",
          "properties": { "ApplicationId": "nameOfAIAppResource" },
          "dependsOn": [
            "[concat('Microsoft.Web/sites/', myWebAppName)]"
          ]
        }
       ]
     } 

* `nameOfAIAppResource`-Sovelluksen tiedot-resurssin nimi
* `myWebAppName`-web-sovelluksen tunnus


## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>Ota diagnostiikan tunniste osana käyttöönotto pilvipalveluun

`New-AzureDeployment` Cmdlet-komento on parametrin `ExtensionConfiguration`, joka kestää matriisin diagnostiikka-määrityksiä. Nämä voidaan luoda avulla `New-AzureServiceDiagnosticsExtensionConfig` cmdlet-komento. Esimerkki:

```ps

    $service_package = "CloudService.cspkg"
    $service_config = "ServiceConfiguration.Cloud.cscfg"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $primary_storagekey = (Get-AzureStorageKey `
     -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext `
       -StorageAccountName $diagnostics_storagename `
       -StorageAccountKey $primary_storagekey

    $webrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WebRole" -Storage_context $storageContext `
      -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WorkerRole" `
      -StorageContext $storage_context `
      -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    New-AzureDeployment `
      -ServiceName $service_name `
      -Slot Production `
      -Package $service_package `
      -Configuration $service_config `
      -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)

``` 

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>Ota käyttöön vianmäärityksen tunniste aiemmin pilvipalvelussa

Aiemmin luodun palvelun käyttämällä `Set-AzureServiceDiagnosticsExtension`.

```ps
 
    $service_name = "MyService"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"
    $primary_storagekey = (Get-AzureStorageKey `
         -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext `
        -StorageAccountName $diagnostics_storagename `
        -StorageAccountKey $primary_storagekey

    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $webrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WebRole" 
    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $workerrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WorkerRole"
```

## <a name="get-current-diagnostics-extension-configuration"></a>Hae diagnostiikka-tunniste nykyiset määritykset

```ps

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```


## <a name="remove-diagnostics-extension"></a>Poista diagnostiikka-tunniste

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

Jos ottanut diagnostiikka-tunniste on käytössä `Set-AzureServiceDiagnosticsExtension` tai `New-AzureServiceDiagnosticsExtensionConfig` ilman roolin parametri, voit poistaa tunnisteen käyttäminen `Remove-AzureServiceDiagnosticsExtension` ilman rooli-parametrin. Jos rooli-parametri on käytetty, kun otat käyttöön tunnisteen sitten se on myös poistettaessa tunniste.

Voit poistaa diagnostiikka-tunniste yksittäisiä rooleille:

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```


## <a name="see-also"></a>Katso myös

* [Valvoa Azure Cloud Services-sovellusten kanssa hakemuksen tiedot](app-insights-cloudservices.md)
* [Lähetä Azure vianmääritys sovelluksen havainnollistamisen](app-insights-azure-diagnostics.md)
* [Automaattinen ilmoitusten määrittäminen](app-insights-powershell-alerts.md)

