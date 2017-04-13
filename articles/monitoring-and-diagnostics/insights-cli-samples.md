<properties
    pageTitle="Azure näytön CLI pikaopas objektit. | Microsoft Azure"
    description="Esimerkki CLI komennot Azure näytön ominaisuudet. Azure näytön on Microsoft Azure-palvelu, jonka avulla voit lähettää ilmoitukset, Soita web URL-osoitteet on määritetty telemetriatietojen tiedot ja Automaattinen skaalaus pilvipalveluihin, näennäiskoneiden ja verkkosovelluksissa arvojen perusteella."
    authors="kamathashwin"
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
    ms.date="09/08/2016"
    ms.author="ashwink"/>

# <a name="azure-monitor--cross-platform-cli-quick-start-samples"></a>Azure näytön Office kaikissa ympäristöissä CLI pika-aloitusopas-objektit

Tässä artikkelissa kerrotaan Esimerkki käyttöliittymä (CLI) komentojen avulla voit käyttää Azure näyttö-ominaisuuksia. Azure näytön avulla Automaattinen skaalaus pilvipalveluihin näennäiskoneiden ja Web Apps-sovellusten ja voit lähettää ilmoitukset tai Soita web URL-osoitteet on määritetty telemetriatietoja arvojen perusteella.

>[AZURE.NOTE] Azure valvonta on uusi nimi "Azure-tiedot" kutsuttiin 2016 25 syyskuuhun asti. Kuitenkin nimitilan ja näin alla olevia komentoja edelleen sisältää "tiedot".


## <a name="prerequisites"></a>Edellytykset

Jos et ole jo asentanut Azure-CLI-kohdassa [Azure-CLI asentaminen](../xplat-cli-install.md). Jos et tiedä Azure CLI, voit lukea lisää se käyttäminen [Azure-CLI Mac, Linux-ja Windows Azure resurssien hallinta](../xplat-cli-azure-resource-manager.md).


Asenna Windows-npm [Node.js sivuston](https://nodejs.org/). Kun olet tehnyt asennuksen käyttämällä CMD.exe ja Suorita järjestelmänvalvojana-oikeudet, suorittaa kansio, johon on asennettu npm seuraavasti:

```console
npm install azure-cli --global
```

Siirry seuraavaksi minkä tahansa kansion/sijainti ja kirjoita komentorivillä:

```console
azure help
```

## <a name="log-in-to-azure"></a>Azure kirjautuminen

Ensimmäinen vaihe on Azure-tiliisi kirjautuminen.

```console
azure login
```

Kun olet suorittanut tämän komennon, sinun on kirjauduttava kautta näytön ohjeita. Jälkeenpäin näet tili, TenantId ja oletusarvon tilauksen tunnus. Kaikki komennot toimivat oletusarvo-tilauksen yhteydessä.

Nykyisen tilauksen tiedot-luettelosta käyttäminen

```console
azure account show
```

Jos haluat muuttaa eri tilauksen käytön yhteydessä, käytä seuraavaa komentoa.

```console
azure account set "subscription ID or subscription name"
```

Voit käyttää Azure Resurssienhallinta ja Azure näytön komentoja, voit on oltava Azure Resurssienhallinta-tilassa.

```console
azure config mode arm
```

Voit tarkastella luetteloa kaikista tuettu Azure näytön komentoja, tee seuraavat.

```console
azure insights
```

## <a name="view-audit-logs-for-a-subscription"></a>Näytä valvontalokien tilaukseen

Jos haluat tarkastella valvontalokien luettelo, tee seuraavat.

```console
azure insights logs list [options]
```

Kokeile seuraavia Näytä kaikki käytettävissä olevat vaihtoehdot.

```console
azure insights logs list -help
```

Esimerkki luettelosta lokit resourceGroup mukaan

```console
azure insights logs list --resourceGroup "myrg1"
```

Esimerkki luettelosta lokit soittajan mukaan

```console
azure insights logs list --caller "myname@company.com"
```

Esimerkki luettelosta kirjaa soittajan resurssi-tyypin mukaan sisällä alkamis-ja end

```console
azure insights logs list --resourceProvider "Microsoft.Web" --caller "myname@company.com" --startTime 2016-03-08T00:00:00Z --endTime 2016-03-16T00:00:00Z
```

## <a name="work-with-alerts"></a>Ilmoitusten käyttäminen
Osan tietojen avulla voit käsitteleminen hälytykset.

### <a name="get-alert-rules-in-a-resource-group"></a>Saat ilmoituksen säännöt resurssiryhmä

```console
azure insights alerts rule list abhingrgtest123
azure insights alerts rule list abhingrgtest123 --ruleName andy0323
```

### <a name="create-a-metric-alert-rule"></a>Metrijärjestelmän ilmoitusten säännön luominen

```console
azure insights alerts actions email create --customEmails foo@microsoft.com
azure insights alerts actions webhook create https://someuri.com
azure insights alerts rule metric set andy0323 eastus abhingrgtest123 PT5M GreaterThan 2 /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Web-EastUS/providers/Microsoft.Web/serverfarms/Default1 BytesReceived Total
```

### <a name="create-a-log-alert-rule"></a>Lokitiedoston ilmoitusten säännön luominen

```console
azure insights alerts rule log set ruleName eastus resourceGroupName someOperationName
```

### <a name="create-webtest-alert-rule"></a>Webtest ilmoitusten säännön luominen

```console
azure insights alerts rule webtest set leowebtestr1-webtestr1 eastus Default-Web-WestUS PT5M 1 GSMT_AvRaw /subscriptions/b67f7fec-69fc-4974-9099-a26bd6ffeda3/resourcegroups/Default-Web-WestUS/providers/microsoft.insights/webtests/leowebtestr1-webtestr1
```

### <a name="delete-an-alert-rule"></a>Ilmoitusten säännön poistaminen

```console
azure insights alerts rule delete abhingrgtest123 andy0323
```

## <a name="log-profiles"></a>Log-profiileista
Tämän osan tietojen avulla voit käsitellä log-profiileista.

### <a name="get-a-log-profile"></a>Hae log-profiili

```console
azure insights logprofile list
azure insights logprofile get -n default
```


### <a name="add-a-log-profile-without-retention"></a>Lisää log profiili ilman säilytys

```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a>Poista log-profiili

```console
azure insights logprofile delete --name default
```

### <a name="add-a-log-profile-with-retention"></a>Lisää log-profiili, jolla säilytys

```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```

### <a name="add-a-log-profile-with-retention-and-eventhub"></a>Log-profiili, jolla säilytys- ja EventHub lisääminen

```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --serviceBusRuleId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/testshoeboxeastus/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```


## <a name="diagnostics"></a>Vianmääritys
Tämän osan tietojen avulla voit käyttää diagnostiikan asetuksia.

### <a name="get-a-diagnostic-setting"></a>Hae diagnostiikan asetus

```console
azure insights diagnostic get --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp
```

### <a name="disable-a-diagnostic-setting"></a>Diagnostiikan asetusten poistaminen käytöstä

```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled false
```

### <a name="enable-a-diagnostic-setting-without-retention"></a>Diagnostiikan asetus ilman säilytys

```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled true
```


## <a name="autoscale"></a>Automaattinen skaalaus
Tämän osan tietojen avulla voit käsitellä Automaattinen skaalaus-asetukset. Haluat muokata näitä esimerkkejä.

### <a name="get-autoscale-settings-for-a-resource-group"></a>Resurssiryhmä Automaattinen skaalaus-asetukset

```console
azure insights autoscale setting list montest2
```

### <a name="get-autoscale-settings-by-name-in-a-resource-group"></a>Saat Automaattinen skaalaus asetukset resurssiryhmä nimen mukaan

```console
azure insights autoscale setting list montest2 -n setting2
```


### <a name="set-auotoscale-settings"></a>Auotoscale asetusten määrittäminen

```console
azure insights autoscale setting set montest2 -n setting2 --settingSpec
```
