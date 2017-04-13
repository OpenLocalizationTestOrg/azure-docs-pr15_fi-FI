<properties
    pageTitle="Azure seuranta REST API ongelmatilanteita | Microsoft Azure"
    description="Tietoa todennetaan pyynnöt ja Azure seuranta REST API-Liittymän avulla."
    authors="mcollier, rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="mcollier"/>

# <a name="azure-monitoring-rest-api-walkthrough"></a>Azure REST API ongelmatilanteita seuranta
Tässä artikkelissa kerrotaan, miten voit suorittaa todennusta, jotta koodisi käyttää [Microsoft Azure näytön REST API-viittaus](https://msdn.microsoft.com/library/azure/dn931943.aspx).         

Azure näyttö-Ohjelmointirajapinnan on voi hakea ohjelmallisesti käytettävissä oletusarvon metrisillä määritelmät (metrijärjestelmän, kuten suorittimen ajan, pyynnöt jne tyyppi), rakeisuuden ja metrisillä arvot. Kun hakea, tiedot voidaan tallentaa eri tietosäilö esimerkiksi Azure SQL-tietokanta, DocumentDB tai Azure tietojen järvi. Sieltä muita analyysi voidaan suorittaa tarpeen mukaan.

Lisäksi käsitteleminen eri metrisillä arvopisteiden kuin tässä artikkelissa esitellään näytön Ohjelmointirajapinnan mahdollistaa sellaisten ilmoitusten sääntöjä, Näytä tapahtumalokit ja paljon muuta. Katso täysi luettelo käytettävissä olevat toiminnot [Microsoft Azure näytön REST API-viittaus](https://msdn.microsoft.com/library/azure/dn931943.aspx).

## <a name="authenticating-azure-monitor-requests"></a>Todennustapa Azure näytön pyynnöt

Ensimmäiseksi on tarkistamiseen pyynnön.

Kaikki tehtävät kohdistaa Azure näytön Ohjelmointirajapinnan käyttäminen Azure resurssien hallinnan todennus-malli. Tämän vuoksi kaikki palvelupyynnöt on voi todentaa Azure Active Directory (Azure AD). Yksi vaihtoehto tarkistamiseen asiakassovellus on luominen Azure AD palvelun lyhennys ja Nouda todennus (JWT)-tunnuksen. Seuraava esimerkki komentosarja osoittaa luominen Azure AD-palvelun pääasiallista PowerShellin kautta. Saat tarkempia hallintapaketteihin lue ohjeet käyttämisestä [Luominen pääasiallista resursseihin palvelu PowerShellin Azure](../resource-group-authenticate-service-principal.md#authenticate-service-principal-with-password—powershell). On myös mahdollista [luoda service-tarpeen Azure portaalin kautta](../resource-group-create-service-principal-portal.md).

```PowerShell
$subscriptionId = "{azure-subscription-id}"
$resourceGroupName = "{resource-group-name}"
$location = "centralus"

# Authenticate to a specific Azure subscription.
Login-AzureRmAccount -SubscriptionId $subscriptionId

# Password for the service principal
$pwd = "{service-principal-password}"

# Create a new Azure AD application
$azureAdApplication = New-AzureRmADApplication `
                        -DisplayName "My Azure Monitor" `
                        -HomePage "https://localhost/azure-monitor" `
                        -IdentifierUris "https://localhost/azure-monitor" `
                        -Password $pwd

# Create a new service principal associated with the designated application
New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

# Assign Reader role to the newly created service principal
New-AzureRmRoleAssignment -RoleDefinitionName Reader `
                          -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

```

Kyselyn Azure näytön Ohjelmointirajapinnan asiakassovellus käytettävä aiemmin luotuja palvelun lyhennys tarkistamiseen. Seuraavassa esimerkissä PowerShell-komentosarjaa näkyy yksi vaihtoehto, saat JWT todennus suojaustunnuksen [Active Directory käyttöoikeuksien kirjaston](../active-directory/active-directory-authentication-libraries.md) (ADAL) avulla. JWT tunnuksen välitetään HTTP-todennus-parametrin pyyntöjä osana Azure näytön REST-Ohjelmointirajapinnalla.

```PowerShell
$azureAdApplication = Get-AzureRmADApplication -IdentifierUri "https://localhost/azure-monitor"

$subscription = Get-AzureRmSubscription -SubscriptionId $subscriptionId

$clientId = $azureAdApplication.ApplicationId.Guid
$tenantId = $subscription.TenantId
$authUrl = "https://login.windows.net/${tenantId}"

$AuthContext = [Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext]$authUrl
$cred = New-Object -TypeName Microsoft.IdentityModel.Clients.ActiveDirectory.ClientCredential -ArgumentList ($clientId, $pwd)
 
$result = $AuthContext.AcquireToken("https://management.core.windows.net/", $cred)

# Build an array of HTTP header values 
$authHeader = @{
'Content-Type'='application/json'
'Accept'='application/json'
'Authorization'=$result.CreateAuthorizationHeader()
}
```

Käyttöoikeuden määritys vaihe on valmis, kun kyselyt voidaan kohdistaa sitten Azure näytön REST-Ohjelmointirajapinnalla. On hyötyä kyselyt:

1. Luettele resurssin metrisillä määritelmät

2. Noutaa metrisillä arvot


## <a name="retrieve-metric-definitions"></a>Noutaa metrisillä määritelmät
>[AZURE.NOTE] Määritä hakemiseen metrisillä määritelmät Azure näytön REST-Ohjelmointirajapinnan käyttäminen "2016-03-01" API-versio.

```PowerShell
$apiVersion = "2016-03-01"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metricDefinitions?api-version=${apiVersion}"

Invoke-RestMethod -Uri $request `
                  -Headers $authHeader `
                  -Method Get `
                  -Verbose
```
Logiikan Azure-sovelluksen metrisillä määritelmät näkyisivät samalla tavalla kuin seuraavassa näyttökuvassa on esitetty:

![ALT "Metrisillä määritelmä vastauksen JSON näkymä".](./media/monitoring-rest-api-walkthrough/available_metric_definitions_logic_app_json_response_clean.png)

Lisätietoja on ohjeissa [luettelon Azure näytön REST API resurssin metrisillä määritykset](https://msdn.microsoft.com/library/azure/mt743621.aspx) .

## <a name="retrieve-metric-values"></a>Noutaa metrisillä arvot
Kun käytettävissä metrisillä määritelmät tiedetään, on sitten voi hakea liittyviä metrisillä arvoja. Käytä metrijärjestelmä nimi "arvoa' (ei" localizedValue") suodatus pyynnöt (esimerkiksi 'CpuTime' ja 'pyytää metrisillä arvopisteiden noutaa). Jos suodattimia ei ole määritetty, palautetaan oletusarvo-arvo.

>[AZURE.NOTE] Määritä Azure näytön REST-Ohjelmointirajapinnan käyttäminen metrisillä arvon hakemiseen "2016-06-01" API-versio.

**Menetelmä**: hankkiminen

**Pyydä URI**: https://management.azure.com/subscriptions/_{Tilaustunnus}_/resourceGroups/_{resurssi-ryhmän nimi}_/providers/_{resurssi-palvelu-nimitilan}_/_{resurssityyppi}_/_{resurssin nimi}_/providers/microsoft.insights/metrics?$filter=_{suodattimen}_ja api-version =_{apiVersion}_

Esimerkiksi hakemiseen RunsSucceeded metrisillä arvopisteiden tietyn aikavälin ja aika-syyt on 1 tunti pyynnön olisi seuraavasti:

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'RunsSucceeded') and aggregationType eq 'Total' and startTime eq 2016-09-23 and endTime eq 2016-09-24 and timeGrain eq duration'PT1H'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

Tulos näkyy näyttökuva seuraavan esimerkin:

![ALT "JSON vastaus näkyy keskimääräisen vastausajan kustannusarvo"](./media/monitoring-rest-api-walkthrough/available_metrics_logic_app_json_response.png)

Hakemiseen useita tietojen tai kooste pisteiden lisääminen metrisillä määritelmä nimet ja kooste-tyypit Suodata-tarkastelu seuraavan esimerkin mukaisesti:

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'ActionsCompleted' or name.value eq 'RunsSucceeded') and (aggregationType eq 'Total' or aggregationType eq 'Average') and startTime eq 2016-09-23T13:30:00Z and endTime eq 2016-09-23T14:30:00Z and timeGrain eq duration'PT1M'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

### <a name="use-armclient"></a>Käytä ARMClient
Käyttämällä PowerShell (katso yllä)-vaihtoehto on käyttää [ARMClient](https://github.com/projectkudu/ARMClient) Windows-tietokoneessa. ARMClient käsittelee Azure AD-todennus (ja tuloksena JWT-tunnuksen) automaattisesti. Seuraavissa vaiheissa kuvataan ARMClient käyttö metrisillä tietojen hakeminen:

1. Asenna [Chocolatey](https://chocolatey.org/) ja [ARMClient](https://github.com/projectkudu/ARMClient).

2. Kirjoita _armclient.exe kirjautuminen_pääte-ikkunassa. Tämä kehottaa sinua Azure kirjautuminen.

3. Kirjoita _armclient GET [your_resource_id]/providers/microsoft.insights/metricdefinitions?api-version=2016-03-01_

4. Kirjoita _armclient GET [your_resource_id]/providers/microsoft.insights/metrics?api-version=2016-06-01_


![ALT "Käyttäminen ARMClient toimimaan Azure seuranta REST API"](./media/monitoring-rest-api-walkthrough/armclient_metricdefinitions.png)


## <a name="retrieve-the-resource-id"></a>Resurssitunnus hakeminen
REST-Ohjelmointirajapinnan käyttäminen todella avulla saat lisätietoja saatavilla metrisillä määritykset, rakeisuuden sekä liittyvät arvot. Nämä tiedot on hyötyä, kun [Azure kirjaston](https://msdn.microsoft.com/library/azure/mt417623.aspx)käytöstä.

Yllä olevaa koodia resurssin Tunnusten käyttöasetusten on haluamasi Azure resurssin koko polku. Esimerkiksi kysely Azure-online-Resurssitunnus on seuraava:

*/subscriptions/{Subscription-ID}/resourceGroups/{Resource-group-name}/Providers/Microsoft.Web/Sites/{Site-name}/*

Seuraava luettelo sisältää resurssin tunnus muodot eri Azure resurssien muutamia esimerkkejä:

* **IoT keskittimeen** - /subscriptions/_{Tilaustunnus}_/resourceGroups/_{resurssi-ryhmän nimi}_/providers/Microsoft.Devices/IotHubs/_{iot-toiminnossa-name}_

* **Joustavasti SQL-ryhmän** - /subscriptions/_{Tilaustunnus}_/resourceGroups/_{resurssi-ryhmän nimi}_/providers/Microsoft.Sql/servers/_{db resurssivarantoon}_/elasticpools/_{sql-varannon nimi}_

* **SQL-tietokanta (Vipuventtiili v12)** – /subscriptions/_{Tilaustunnus}_/resourceGroups/_{resurssi-ryhmän nimi}_/providers/Microsoft.Sql/servers/_{server name}_/databases/_{tietokannan nimi}_

* **Palvelun Bus** - /subscriptions/_{Tilaustunnus}_/resourceGroups/_{resurssi-ryhmän nimi}_/providers/Microsoft.ServiceBus/_{nimitilan}_/_{nimi servicebus}_

* **AM asteikko joukot** - /subscriptions/_{Tilaustunnus}_/resourceGroups/_{resurssi-ryhmän nimi}_/providers/Microsoft.Compute/virtualMachineScaleSets/_{nimi AM}_

* **VMs** - /subscriptions/_{Tilaustunnus}_/resourceGroups/_{resurssi-ryhmän nimi}_/providers/Microsoft.Compute/virtualMachines/_{nimi AM}_

* **Tapahtuman keskittimet** - /subscriptions/_{Tilaustunnus}_/resourceGroups/_{resurssi-ryhmän nimi}_/providers/Microsoft.EventHub/namespaces/_{nimitilan eventhub}_


Käytettävissä on useita vaihtoehtoja, haetaan Resurssitunnus, mukaan lukien Azure resurssin Resurssienhallinnassa, tarkastelemalla haluamasi Azure-portaalissa ja PowerShell tai Azure-CLI kautta.

### <a name="azure-resource-explorer"></a>Azure resurssin Explorer
Voit etsiä haluamasi resurssin Resurssitunnus-yksi hyödyllisiä vaihtoehto on [Azure resurssin Explorer](https://resources.azure.com) -työkalun avulla. Siirry haluamasi resurssi ja katso näkyvissä, kuten seuraavassa näyttökuvan tunnus:

![ALT "Azure resurssin Explorer"](./media/monitoring-rest-api-walkthrough/azure_resource_explorer.png)

### <a name="azure-portal"></a>Azure portal
Resurssitunnus saadaan myös Azure-portaalista. Voit tehdä sen siirry haluamasi resurssi ja valitse sitten Ominaisuudet. Resurssitunnus näkyy ominaisuudet-sivu, joka seuraavassa näyttökuvassa näkyy:

![ALT-Resurssitunnus Azure-portaalissa ominaisuudet-sivu näkyy"](./media/monitoring-rest-api-walkthrough/resourceid_azure_portal.png)

### <a name="azure-powershell"></a>Azure PowerShell
Resurssitunnus voi hakea Azure PowerShell cmdlet-komennot. Voit hankkia Azure Web App-sovelluksessa Resurssitunnus-Suorita Get-AzureRmWebApp cmdlet, kuten seuraavassa näyttökuvassa on esitetty:

![ALT-Resurssitunnus PowerShellin kautta saadaan"](./media\monitoring-rest-api-walkthrough\resourceid_powershell.png)

### <a name="azure-cli"></a>Azure CLI
Hakemiseen käyttämällä Azure-CLI Resurssitunnus "azure Web App-sovelluksen Näytä"-komennon suorittaminen, joka määrittää "--json" asetus, kuten seuraavassa näyttökuvassa:

![ALT-Resurssitunnus PowerShellin kautta saadaan"](./media\monitoring-rest-api-walkthrough\resourceid_azurecli.png)

## <a name="retrieve-activity-log-data"></a>Noutaa toimintojen lokitiedot
Se on myös voi hakea Lisää kiinnostavat tiedot Azure resursseihin liittyviä käsitteleminen metrisillä määritelmät ja liittyvät arvot. Esimerkkinä on mahdollista kyselyn [toimintojen](https://msdn.microsoft.com/library/azure/dn931934.aspx) lokitiedot. Seuraavassa esimerkissä kyselyn toimintojen lokitiedot tietyn päivämäärävälin sisällä Azure näytön muiden Ohjelmointirajapinnan käyttäminen Azure-tilauksen:

```PowerShell
$apiVersion = "2014-04-01"
$filter = "eventTimestamp ge '2016-09-23' and eventTimestamp le '2016-09-24'and eventChannels eq 'Admin, Operation'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/providers/microsoft.insights/eventtypes/management/values?api-version=${apiVersion}&`$filter=${filter}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

## <a name="next-steps"></a>Seuraavat vaiheet
* Tarkista [Yleiskatsaus seuranta](monitoring-overview.md).
* Näytä [Tuetut arvot Azure näyttöä](monitoring-supported-metrics.md).
* Tutustu [Microsoft Azuren näytön REST API-viittaus](https://msdn.microsoft.com/library/azure/dn931943.aspx).
* Tarkista [Azure kirjaston](https://msdn.microsoft.com/library/azure/mt417623.aspx).
