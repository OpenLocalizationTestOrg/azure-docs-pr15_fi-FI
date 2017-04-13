<properties 
    pageTitle="Azure etsinnän hallinta ja Powershell-komentosarjojen | Microsoft Azure | Isännöityjen pilvipalvelussa haku" 
    description="Hallitse Azure Search-palvelun kanssa PowerShell-komentosarjojen. Luo tai päivitä Azure-hakupalvelun ja hallita Azure haun järjestelmänvalvojan näppäimet" 
    services="search" 
    documentationCenter="" 
    authors="seansaleh" 
    manager="mblythe" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="search" 
    ms.devlang="na" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="powershell" 
    ms.date="08/15/2016" 
    ms.author="seasa"/>

# <a name="manage-your-azure-search-service-with-powershell"></a>Hallitse Azure Search-palvelun PowerShellin avulla
> [AZURE.SELECTOR]
- [Portal](search-manage.md)
- [PowerShellin](search-manage-powershell.md)
- [REST-OHJELMOINTIRAJAPINNALLA](search-get-started-management-api.md)

Tässä ohjeaiheessa kerrotaan PowerShell-komennoilla voit suorittaa useita hallintatehtäviä Azure Etsi-palveluita. Selkeät luominen search-palvelun ja skaalaus se hallinta sen API-näppäimiä.
Nämä komennot rinnakkainen käytettävissä [Azure haun hallinnan REST API](http://msdn.microsoft.com/library/dn832684.aspx)hallinta-asetukset.

## <a name="prerequisites"></a>Edellytykset
 
- Sinulla on oltava Azure PowerShell 1.0- tai suurempi. Katso ohjeet [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md).
- Sinun on kirjauduttava Azure tilaukseen powershellissä seuraavalla tavalla.

Ensin sinun täytyy Azure kirjautuessasi tällä komennolla:

    Login-AzureRmAccount

Määritä Microsoft Azure sisäänkirjautuminen-valintaikkunan sähköpostiosoite Azure-tili ja salasana.

Vaihtoehtoisesti voit [Kirjautuminen ei-vuorovaikutteisesti pääasiallista palvelun kanssa](../resource-group-authenticate-service-principal.md).

Jos sinulla on useita Azure tilauksia, sinun on Azure tilauksen määrittämiseen. Saat tilauksistasi nykyinen luettelo-komennon suorittaminen

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

Jos haluat määrittää tilauksen, suorita seuraava komento. Seuraavassa esimerkissä Tilauksen nimi on `ContosoSubscription`.

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

## <a name="commands-to-help-you-get-started"></a>Komentoja, jotka auttavat käytön aloittaminen

    $serviceName = "your-service-name-lowercase-with-dashes"
    $sku = "free" # or "basic" or "standard" for paid services
    $location = "West US"
    # You can get a list of potential locations with
    # (Get-AzureRmResourceProvider -ListAvailable | Where-Object {$_.ProviderNamespace -eq 'Microsoft.Search'}).Locations
    $resourceGroupName = "YourResourceGroup" 
    # If you don't already have this resource group, you can create it with 
    # New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Register the ARM provider idempotently. This must be done once per subscription
    Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Search" -Force

    # Create a new search service
    # This command will return once the service is fully created
    New-AzureRmResourceGroupDeployment `
        -ResourceGroupName $resourceGroupName `
        -TemplateUri "https://gallery.azure.com/artifact/20151001/Microsoft.Search.1.0.9/DeploymentTemplates/searchServiceDefaultTemplate.json" `
        -NameFromTemplate $serviceName `
        -Sku $sku `
        -Location $location `
        -PartitionCount 1 `
        -ReplicaCount 1
    
    # Get information about your new service and store it in $resource
    $resource = Get-AzureRmResource `
        -ResourceType "Microsoft.Search/searchServices" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19
    
    # View your resource
    $resource
    
    # Get the primary admin API key
    $primaryKey = (Invoke-AzureRmResourceAction `
        -Action listAdminKeys `
        -ResourceId $resource.ResourceId `
        -ApiVersion 2015-08-19).PrimaryKey

    # Regenerate the secondary admin API Key
    $secondaryKey = (Invoke-AzureRmResourceAction `
        -ResourceType "Microsoft.Search/searchServices/regenerateAdminKey" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19 `
        -Action secondary).SecondaryKey

    # Create a query key for read only access to your indexes
    $queryKeyDescription = "query-key-created-from-powershell"
    $queryKey = (Invoke-AzureRmResourceAction `
        -ResourceType "Microsoft.Search/searchServices/createQueryKey" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19 `
        -Action $queryKeyDescription).Key
    
    # View your query key
    $queryKey

    # Delete query key
    Remove-AzureRmResource `
        -ResourceType "Microsoft.Search/searchServices/deleteQueryKey/$($queryKey)" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19
        
    # Scale your service up
    # Note that this will only work if you made a non "free" service
    # This command will not return until the operation is finished
    # It can take 15 minutes or more to provision the additional resources
    $resource.Properties.ReplicaCount = 2
    $resource | Set-AzureRmResource
    
    # Delete your service
    # Deleting your service will delete all indexes and data in the service
    $resource | Remove-AzureRmResource
    
## <a name="next-steps"></a>Seuraavat vaiheet
    
Palvelussa on luotu, voi kestää seuraavat vaiheet: [indeksi](search-what-is-an-index.md)- [kyselyn indeksin](search-query-overview.md), ja lopuksi luoda ja hallita omia Etsi sovellus, joka käyttää Azure haku.

- [Luo Azure-hakuindeksin Azure-portaalissa](search-create-index-portal.md)

- [Kysely Etsi Explorerilla Azure-portaalissa Azure hakuindeksin](search-explorer.md)

- [Määritys indeksointitoiminto lataa tiedot muista palveluista](search-indexer-overview.md)

- [.NET Azure-haun käyttäminen](search-howto-dotnet-sdk.md)

- [Analysoi Azure haku-liikenne](search-traffic-analytics.md)
