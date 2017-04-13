<properties 
    pageTitle="Luo sovelluksen tiedot-resurssi PowerShell-komentosarja" 
    description="Hakemuksen tiedot resurssien luonnin automatisoida." 
    services="application-insights" 
    documentationCenter="windows"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="02/19/2016" 
    ms.author="awills"/>

#  <a name="powershell-script-to-create-an-application-insights-resource"></a>Luo sovelluksen tiedot-resurssi PowerShell-komentosarja

*Hakemuksen tiedot on esikatselu.*

Kun haluat seurata uusi sovellus - tai uuden version sovelluksen - ja [Visual Studio hakemuksen tiedot](https://azure.microsoft.com/services/application-insights/), voit määrittää Microsoft Azure uusi resurssi. Resurssi on, jossa telemetriatietojen tiedot sovelluksen analysoida ja näkyviin. 

Uusi resurssi luonnin automatisoida PowerShell-toiminnolla.

Esimerkiksi Jos kehität mobiililaitteen-sovelluksen, todennäköisesti, milloin tahansa olemassa olevan sovelluksen asiakkaiden käyttämien julkaistun useita versioita. Et halua saada telemetriatietojen tulokset sekoitetaan eri versioita. Näin saat muodosta prosessin Luo uuden resurssin jokaisen muodosta.

## <a name="script-to-create-an-application-insights-resource"></a>Komentosarjan, joka luo sovelluksen tiedot-resurssi

Katso asiaa cmdlet-Etkö:

* [Uusi AzureRmResource](https://msdn.microsoft.com/library/mt652510.aspx)
* [Uusi AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt678995.aspx)


*PowerShell-komentosarjaa*  

```PowerShell


###########################################
# Set Values
###########################################

# If running manually, uncomment before the first 
# execution to login to the Azure Portal:

# Add-AzureRmAccount

# Set the name of the Application Insights Resource

$appInsightsName = "TestApp"

# Set the application name used for the value of the Tag "AppInsightsApp" 
# - http://azure.microsoft.com/documentation/articles/azure-preview-portal-using-tags/
$applicationTagName = "MyApp"

# Set the name of the Resource Group to use.  
# Default is the application name.
$resourceGroupName = "MyAppResourceGroup"

###################################################
# Create the Resource and Output the name and iKey
###################################################

#Select the azure subscription
Select-AzureSubscription -SubscriptionName "MySubscription"

# Create the App Insights Resource

$resource = New-AzureRmResource `
  -ResourceName $appInsightsName `
  -ResourceGroupName $resourceGroupName `
  -Tag @{ Name = "AppInsightsApp"; Value = $applicationTagName} `
  -ResourceType "Microsoft.Insights/Components" `
  -Location "Central US" `
  -PropertyObject @{"Type"="ASP.NET"} `
  -Force

# Give owner access to the team

New-AzureRmRoleAssignment `
  -SignInName "myteam@fabrikam.com" `
  -RoleDefinitionName Owner `
  -Scope $resource.ResourceId 


#Display iKey
Write-Host "App Insights Name = " $resource.Name
Write-Host "IKey = " $resource.Properties.InstrumentationKey

```

## <a name="what-to-do-with-the-ikey"></a>Mitä tehdään iKey

Kullekin resurssille tunnistetaan sen instrumentation avaimen (iKey). IKey on tulos, resurssi luonnin komentosarjaa. Muodosta komentosarjan annettava sovelluksen tietoja: n SDK iKey upotettuna sovelluksen.

Voit tehdä iKey SDK kahdella tavalla:
  
* Valitse [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md): 
 * `<instrumentationkey>`*ikey*`</instrumentationkey>`
* Tai [alustus koodi](app-insights-api-custom-events-metrics.md): 
 * `Microsoft.ApplicationInsights.Extensibility.
    TelemetryConfiguration.Active.InstrumentationKey = "`*iKey*`";`



## <a name="see-also"></a>Katso myös

* [Hakemuksen tiedot ja WWW-testi resurssien luominen malleista](app-insights-powershell.md)
* [Määritä valvonta PowerShellin Azure-kansio](app-insights-powershell-azure-diagnostics.md) 
* [PowerShell-toiminnolla ilmoitusten määrittäminen](app-insights-powershell-alerts.md)

 