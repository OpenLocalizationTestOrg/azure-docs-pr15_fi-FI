<properties
    pageTitle="Analysoi käyttämällä lokiin Analytics Azure vianmäärityslokit | Microsoft Azure"
    description="Log Analytics lokeja voi lukea Azure services, joka kirjoittaa Azure blob storage JSON-muodossa, vianmäärityslokit."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>


# <a name="analyze-azure-diagnostic-logs-using-log-analytics"></a>Analysoi Azure vianmäärityslokit Log-analyysin avulla

Lokitiedoston Analytics voi kerätä lokit seuraavat Azure Services, joka kirjoittaa [vianmäärityslokit Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) -blob-säiliö JSON-muodossa:

+ Automaatio (ennakkoversio)
+ Avaimen säilö (ennakkoversio)
+ Sovelluksen Gateway (ennakkoversio)
+ Verkko-käyttöoikeusryhmän (ennakkoversio)

Seuraavissa osissa ohjatusti PowerShellin avulla:

+ Määritä lokin Analytics kerätä lokit kullekin resurssille tallennustila  
+ Ota loki Analytics-ratkaisun Azure-palveluun

Ennen kuin lokin Analytics voi kerätä tietoja näiden resurssien, Azure Diagnostiikka on otettava käyttöön. Käytä `Set-AzureRmDiagnosticSetting` cmdlet-komento, voit ottaa kirjaamisen käyttöön.

Lue lisätietoja siitä, miten voit ottaa käyttöön Diagnostiikan kirjaus on seuraavissa artikkeleissa:

+ [Avaimen säilö](../key-vault/key-vault-logging.md)
+ [Sovelluksen yhdyskäytävän](../application-gateway/application-gateway-diagnostics.md)
+ [Verkko-käyttöoikeusryhmän](../virtual-network/virtual-network-nsg-manage-log.md)

Tässä ohjeessa sisältää tiedot myös käyttöön:

+ Tietojen kerääminen vianmääritys
+ Tietojen kerääminen pysäyttäminen

## <a name="configure-log-analytics-to-collect-azure-diagnostic-logs"></a>Määritä lokin Analytics kerätä Azure vianmäärityslokit

Palvelun lokien kerääminen ja visualisointi lokit ratkaisun käyttöönotto suoritetaan käyttämällä PowerShell-komentosarjojen.

Alla olevassa esimerkissä mahdollistaa kaikkien tuettujen resurssien kirjautuminen

```
# update to be the storage account that logs will be written to. Storage account must be in the same region as the resource to monitor
# format is similar to "/subscriptions/ec11ca60-ab12-345e-678d-0ea07bbae25c/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount"
$storageAccountId = ""

$supportedResourceTypes = ("Microsoft.Automation/AutomationAccounts", "Microsoft.KeyVault/Vaults", "Microsoft.Network/NetworkSecurityGroups", "Microsoft.Network/ApplicationGateways")

# update location to match your storage account location
$resources = Get-AzureRmResource | where { $_.ResourceType -in $supportedResourceTypes -and $_.Location -eq "westus" }

foreach ($resource in $resources) {
    Set-AzureRmDiagnosticSetting -ResourceId $resource.ResourceId -StorageAccountId $storageAccountId -Enabled $true -RetentionEnabled $true -RetentionInDays 1
}
```


Jotkin resurssien on mahdollista tehdä edellisen määritykset Azure-portaalista [Azure yleiskatsaus vianmäärityslokit](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-diagnostic-logs)kuvattujen vaiheiden avulla.

## <a name="configure-log-analytics-to-collect-azure-diagnostic-logs"></a>Määritä lokin Analytics kerätä Azure vianmäärityslokit

On annettu PowerShellin komentosarja-moduulin, joka vie määrittäminen Log analyysin helpottamiseksi kaksi cmdlet-komennot:

1. `Add-AzureDiagnosticsToLogAnalyticsUI`kysyy syötteen ja ei pysty määrittämään yksinkertainen käyttömahdollisuudet
2. `Add-AzureDiagnosticsToLogAnalytics`Vie resurssien valvottavat syötteenä ja määrittää lokiin Analytics  

### <a name="pre-requisites"></a>Vaatimukset

1. Azure PowerShell 1.0.8-versiolla tai uudempaa versiota ja toiminnallisia havainnollistamisen cmdlet-komennot.
  - [Asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md)
  - Tarkista versiossasi cmdlet-komennot:`Import-Module AzureRM.OperationalInsights -MinimumVersion 1.0.8 `
2. Diagnostiikan kirjaus on määritetty Azure resurssille, joita haluat seurata. Käytä `Set-AzureRmDiagnosticSetting` tai [Käytä Log Analytics Azure tallennustilan tileistä tietojen keräämiseen](log-analytics-azure-storage.md) ottamisesta käyttöön diagnostiikka viitata.
3. [Lokitiedoston Analytics](https://portal.azure.com/#create/Microsoft.LogAnalyticsOMS) -työtila  
4. AzureDiagnosticsAndLogAnalytics PowerShell-moduuli
  - Lataa [AzureDiagnosticsAndLogAnalytics](https://www.powershellgallery.com/packages/AzureDiagnosticsAndLogAnalytics/) -moduulin PowerShell-valikoimasta

### <a name="option-1-run-the-interactive-configuration-scripts"></a>Vaihtoehto 1: Vuorovaikutteinen määritys-komentosarjojen suorittamisen

Avaa PowerShell- ja suorita:

```
# Connect to Azure
Login-AzureRmAccount
# If you have diagnostics logs being written to classic storage you will also need to run
Add-AzureAccount

# Import the module
Install-Module -Name AzureDiagnosticsAndLogAnalytics

# Run the UI configuration script
Add-AzureDiagnosticsToLogAnalyticsUI

```

Ovat näkyvissä käytettävissä olevat valinnat luettelo ja tee valintasi kehote valita.
Sinua pyydetään kunkin seuraavat asetukset:

+ Resurssityypit (Azure palvelut)-lokien kerääminen
+ Resurssin esiintymiä voi kerätä-lokit
+ Kirjaudu Analytics työtilan tietojen keräämiseen

Kun olet suorittanut tämän komentosarjan, pitäisi näkyä Log Analytics tietueiden noin 30 minuuttia sen jälkeen, kun uusi diagnostiikkatiedot kirjoitetaan tallennustilan. Jos tietueita ei ole käytettävissä, kun tällä hetkellä viitata vianmääritys alla olevasta osiosta.

### <a name="option-2-build-a-list-of-resources-and-pass-them-to-the-configuration-cmdlet"></a>Vaihtoehto 2: Muodostaa luettelon resurssien ja välittää niitä määritysten cmdlet-komento

Voit luoda luettelon resursseista, joissa on käytössä Azure diagnostiikka ja siirtää sitten resurssit määritysten cmdlet-komento.

Näet lisätietoja cmdlet suorittamalla `Get-Help Add-AzureDiagnosticsToLogAnalytics`.


Voit etsiä lisätietoja OMS [Log Analytics PowerShellin cmdlet-komennot](https://msdn.microsoft.com/library/mt188224.aspx)

>[AZURE.NOTE] Jos resurssi ja työtilan ovat eri Azure-tilauksissa, siirtyä niiden välillä käyttämällä tarvittaessa`Select-AzureRmSubscription -SubscriptionId <Subscription the resource is in>`

```
# Connect to Azure
Login-AzureRmAccount
# If you have diagnostics logs being written to classic storage you will also need to run
Add-AzureAccount

# Import the module
Install-Module -Name AzureDiagnosticsAndLogAnalytics

# Update the values below with the name of the resource that has Azure diagnostics enabled and the name of your Log Analytics workspace
$resourceName = "***example-resource***"
$workspaceName = "***log-analytics-workspace***"

# Find the resource to monitor:
$resource = Find-AzureRmResource -ResourceType "Microsoft.KeyVault/Vaults" -ResourceNameContains $resourceName

# Find the Log Analytics workspace to configure, for example:
$workspace = Find-AzureRmResource -ResourceType "Microsoft.OperationalInsights/workspaces" -ResourceNameContains $workspaceName

# Perform configuration
Add-AzureDiagnosticsToLogAnalytics $resource $workspace

# Enable the Log Analytics solution
Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -intelligencepackname KeyVault -Enabled $true

```
Kun olet suorittanut tämän komentosarjan, pitäisi näkyä Log Analytics tietueiden noin 30 minuuttia sen jälkeen, kun uusi diagnostiikkatiedot kirjoitetaan tallennustilan. Jos tietueita ei ole käytettävissä, kun tällä hetkellä viitata vianmääritys alla olevasta osiosta.  

>[AZURE.NOTE] Määritystä ei ole näkyvissä Azure-portaalissa. Voit tarkistaa määrittäminen `Get-AzureRmOperationalInsightsStorageInsight` cmdlet-komento.  


## <a name="stopping-log-analytics-from-collecting-azure-diagnostic-logs"></a>Pysäyttää Log Analytics-keräävän Azure vianmäärityslokit

Voit poistaa resurssin Log Analytics-määritys `Remove-AzureRmOperationalInsightsStorageInsight` cmdlet-komento.

## <a name="troubleshooting-configuration-for-azure-diagnostic-logs"></a>Vianmääritys Azure vianmäärityslokit määritys

*Ongelma*

Resurssi, joka on logging perinteinen tallennustilan määritettäessä, näyttöön tulee seuraava virhe:

```
Select-AzureSubscription : The subscription id 7691b0d1-e786-4757-857c-7360e61896c3 doesn't exist.

Parameter name: id
```

*Tarkkuus*

Kirjaudu sisään Ohjelmointirajapinnan perinteinen käyttöönotto-mallin`Add-AzureAccount`

## <a name="troubleshooting-data-collection-for-azure-diagnostic-logs"></a>Azure vianmäärityslokit keräystoiminto vianmääritys

Jos et näe tietojen Azure resurssin Log Analytics varten, voit käyttää seuraavien vianmääritysvaiheiden avulla:

+ Tarkista juoksutus tallennustilan tilin tiedot
+ Tarkista palvelun Log Analytics-ratkaisu on otettu käyttöön
+ Varmista, että loki Analytics määritetty tallennustilan lukeminen
+ Päivitä tallennustilan tilin avain

### <a name="verify-data-is-flowing-to-the-storage-account"></a>Tarkista tiedot on juoksutus tallennustilan-tilille

Voit tarkistaa, jos tiedot juoksutus tallennustilan tilille työkalua, joka sallii selaamisen Azure storage (esimerkiksi Visual Studio) tai PowerShell-toiminnolla.

Tallennustilan tilin resurssi on määritetty kirjautua seuraavat PowerShellin käyttäminen:

`Find-AzureRmResource -ResourceType "Microsoft.KeyVault/Vaults" | select ResourceId | Get-AzureRmDiagnosticSetting `

Azure vianmääritys käyttämän tallennustilan säilö on nimi, jossa muoto, jossa on:  

`insights-logs-<Category>`

Lisätietoja on lisätietoja tallennustilan tilin sisällön tarkasteleminen [Tarkista uusimmat tiedot säilö tallennustilan cmdlet-komentojen avulla](../storage/storage-powershell-guide-full.md) .

### <a name="verify-the-log-analytics-solution-for-the-service-is-enabled"></a>Tarkista palvelun Log Analytics-ratkaisu on otettu käyttöön

Jos käytät `Add-AzureDiagnosticsToLogAnalyticsUI`, oikea Log Analytics-ratkaisun otetaan automaattisesti käyttöön puolestasi.

Jos haluat tarkistaa, jos ratkaisu on käytössä, suorita seuraavat PowerShell:

`Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $logAnalyticsResourceGroup -WorkspaceName $logAnalyticsWorkspaceName`

Jos ratkaisu ei ole käytössä, voit ottaa sen seuraavat PowerShellin avulla:

`Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $logAnalyticsResourceGroup -WorkspaceName $logAnalyticsWorkspaceName -IntelligencePackName $solution -Enabled $true`

Voit etsiä nimen, voit ottaa käyttöön jokaisen resurssilaji-ratkaisun (muuttuja on käytettävissä, kun olet tuonut moduulin) seuraavat PowerShellin käyttäminen:

`$MonitorableResourcesToOMSSolutions`

### <a name="verify-that-log-analytics-is-configured-to-read-from-storage"></a>Varmista, että loki Analytics määritetty tallennustilan lukeminen

Jos lisäät Azure lisäresursseja, joudut Diagnostiikan kirjaus niiden ottaminen käyttöön ja määrittää lokiin Analytics niiden.
Voit tarkistaa, mitkä resurssit ja tallennustilaa tilit Log Analytics on määritetty palautteen kerääminen-lokit, käytä seuraavia PowerShell:

```
# Find the Workspace ResourceGroup and Name
$logAnalyticsWorkspace = Get-AzureRmOperationalInsightsWorkspace

#Get the configuration for all resources:
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $logAnalyticsWorkspace.ResourceGroupName -WorkspaceName $logAnalyticsWorkspace.Name

#Get the just the resources configured:
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $logAnalyticsWorkspace.ResourceGroupName -WorkspaceName $logAnalyticsWorkspace.Name | select Containers
```

### <a name="update-the-storage-key"></a>Päivitä tallennustilan avain

Jos muutat tallennustilan tilin näppäintä, loki Analytics-määritys on myös päivity uusi avain.
Uusi avain seuraavat PowerShellin avulla voit päivittää Log Analytics-määritys:

`Set-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $logAnalyticsWorkspace.ResourceGroupName -WorkspaceName $logAnalyticsWorkspace.Name –Name <Storage Insight Name> -StorageAccountKey $newKey `

Voit etsiä tallennustilan tietoja nimi `Get-AzureRmOperationalInsightsStorageInsight` cmdlet-komento, aiempien esimerkkien.

## <a name="next-steps"></a>Seuraavat vaiheet

- [Käytä Blob-objektien tallennustilaan IIS ja tapahtumien taulukkotallennus](log-analytics-azure-storage-iis-table.md) Lue lokit Azure services, kirjoita diagnostiikka taulukkotallennus tai kirjoittaa blob storage IIS-lokeja.
- [Ota käyttöön ratkaisuja](log-analytics-add-solutions.md) selventäminen tiedot.
- [Käytä hakukyselyt](log-analytics-log-searches.md) tietojen analysoimista varten.
