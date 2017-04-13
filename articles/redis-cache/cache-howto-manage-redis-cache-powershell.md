<properties
    pageTitle="Azure PowerShellin Azure Redis.txt välimuistin hallinnan | Microsoft Azure"
    description="Opettele hallintatehtäviin Azure Redis välimuistin Azure PowerShellin avulla."
    services="redis-cache"
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="manage-azure-redis-cache-with-azure-powershell"></a>Azure PowerShellin Azure Redis.txt välimuistin hallinnan

> [AZURE.SELECTOR]
- [PowerShellin](cache-howto-manage-redis-cache-powershell.md)
- [Azure CLI](cache-manage-cli.md)

Tässä ohjeaiheessa esitellään voit suorittaa yleisiä tehtäviä luot kuten, Päivitä ja skaalata Azure Redis välimuistin esiintymät, miten uudelleen pikanäppäinten ja miten voit tarkastella tietoja siitä, että tallentaa. Katso luettelo kaikista Azure Redis välimuistin PowerShellin cmdlet-komennot, [Azure Redis välimuistin cmdlet-komennot](https://msdn.microsoft.com/library/azure/mt634513.aspx).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)][perinteinen mallia](../resource-manager-deployment-model.md#classic-deployment-characteristics).

## <a name="prerequisites"></a>Edellytykset

Jos olet jo asentanut PowerShellin Azure, sinulla on oltava PowerShellin Azure versio 1.0.0 tai uudempi versio. Voit tarkistaa, jotka ovat asentaneet tällä komennolla PowerShellin Azure komentokehotteen PowerShellin Azure-version.

    Get-Module azure | format-table version

[AZURE.INCLUDE [powershell-preview](../../includes/powershell-preview-inline-include.md)]

Ensin sinun on kirjauduttava, Azure tällä komennolla.

    Login-AzureRmAccount

Määritä Azure-tili ja salasana sähköpostiosoite Microsoft Azure-kirjautuminen-valintaikkuna.

Seuraava, jos sinulla on useita tilauksia Azure, haluat määrittää Azure tilauksen. Saat tilauksistasi nykyinen luettelo-komennon suorittaminen

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

Jos haluat määrittää tilauksen, suorita seuraava komento. Seuraavassa esimerkissä Tilauksen nimi on `ContosoSubscription`.

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

Ennen kuin voit käyttää Windows PowerShellin Azure resurssien hallintaa, tarvitset seuraavat:

- Windows PowerShell-Version 3.0 tai 4.0. Voit etsiä Windows PowerShell-version kirjoittamalla:`$PSVersionTable` ja tarkista arvon `PSVersion` on 3.0 tai 4.0. Asenna yhteensopiva versio-kohdassa [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) - tai [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

Saat yksityiskohtaisia ohjeita cmdlet-komento, katso tässä opetusohjelmassa, hanki ohjeita cmdlet-komennon avulla

    Get-Help <cmdlet-name> -Detailed

Jos esimerkiksi haluat ohjeita `New-AzureRmRedisCache` cmdlet-tyyppi:

    Get-Help New-AzureRmRedisCache -Detailed

### <a name="how-to-connect-to-azure-government-cloud-or-azure-china-cloud"></a>Yhteyden muodostamisesta Azure Government Cloud tai Azure kiina pilvestä

Oletusarvoisesti Azure ympäristö on `AzureCloud`, joka edustaa yleistä Azure cloud-esiintymä. Voit muodostaa yhteyden eri esiintymän, `Add-AzureRmAccount` komennon kanssa `-Environment` tai -`EnvironmentName` komentorivivalitsin haluamasi ympäristön tai ympäristön nimi.

Voit tarkastella käytettävissä ympäristöissä luettelo suorittamalla `Get-AzureRmEnvironment` cmdlet-komento.

### <a name="to-connect-to-the-azure-government-cloud"></a>Muodostaa Azure Government pilveen

Muodosta yhteys Azure Government Cloud jollakin seuraavista komennoista.

    Add-AzureRMAccount -EnvironmentName AzureUSGovernment

tai

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureUSGovernment)

Voit luoda välimuistin Azure Government pilvipalvelussa käyttämällä jotakin seuraavista sijainneista.

-   USGov Virginia
-   USGov Iowa

Saat lisätietoja Azure Government Cloud [Microsoft Azure julkishallintoa](https://azure.microsoft.com/features/gov/) tai [Microsoft Azure Government Sovelluskehittäjän opas](../azure-government-developer-guide.md).

### <a name="to-connect-to-the-azure-china-cloud"></a>Muodostaa Azure Kiinan pilveen

Muodostaa Azure Kiinan pilveen jollakin seuraavista komennoista.

    Add-AzureRMAccount -EnvironmentName AzureChinaCloud

tai

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureChinaCloud)

Voit luoda välimuistin Azure Kiinan pilvipalvelussa käyttämällä jotakin seuraavista sijainneista.

-   Kiinan Itä
-   Kiinan Pohjois

Saat lisätietoja Azure Kiinan pilveen [Kiinassa 21vianetin ylläpitämä Azure AzureChinaCloud](http://www.windowsazure.cn/).

### <a name="properties-used-for-azure-redis-cache-powershell"></a>Azure Redis välimuistin PowerShell-ominaisuudet

Seuraavassa taulukossa on ominaisuudet ja kuvauksia tavallisimmat parametrien kun luomisen ja ylläpidon Azure PowerShellin Azure Redis välimuistin kopioita.

| Parametri          | Kuvaus                                                                                                                                                                                                        | Oletusarvo  |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| Nimi               | Välimuistin nimi                                                                                                                                                                                                  |          |
| Sijainti           | Välimuistin sijainti                                                                                                                                                                                              |          |
| ResourceGroupName  | Resurssin ryhmänimi, johon luot välimuisti                                                                                                                                                                   |          |
| Kokoa               | Välimuistin kokoa. Kelvollisia arvoja: P1, P2, P3, P4, C0, C1, C2, C3, C4, C5, C6, 250 Mt, vähintään 1 gt, 2,5 Gigatavua, 6 Gigatavua, 13 Gigatavua, 26 gt, 53 gt                                                                     | VÄHINTÄÄN 1 GT      |
| ShardCount         | Luo luotaessa premium-välimuistin asentaminen käytössä shards määrä. Kelvollisia arvoja: 1, 2, 3, 4, 5, 6, 7, 8, 9 ja 10                                                                                                      |          |
| TUOTE                | Määrittää välimuistin tuote. Kelvollisia arvoja: perustiedot, Vakio, Premium                                                                                                                                         | Vakio |
| RedisConfiguration | Määrittää Redis.txt asetukset. Lisätietoja asetuksista on artikkelissa [RedisConfiguration ominaisuudet](#redisconfiguration-properties) seuraavan taulukon. |          |
| EnableNonSslPort   | Ilmaisee, onko-SSL portti on käytössä.                                                                                                                                                                     | EPÄTOSI    |
| MaxMemoryPolicy    | Tämä parametri on vähennetty – Käytä RedisConfiguration sijaan.                                                                                                                                              |          |
| StaticIP           | Kun isännöinnin VNET-välimuistin määrittää välimuistin aliverkon yksilöllinen IP-osoite. Jos ei anneta, yksi valitaan aliverkosta puolestasi.                                                                                                                     |          |
| Aliverkon             | Kun isännöinnin VNET-välimuistin määrittää nimen, johon ottamaan välimuistin aliverkon.                                                                                                                  |          |
| VirtualNetwork     | Kun isännöinnin VNET-välimuistin määrittää VNET, jossa ottamaan välimuistin Resurssitunnus.                                                                                                             |          |
| KeyType            | Määrittää, mitkä pikanäppäin uudelleen, kun uudistamalla pikanäppäimet. Kelvollisia arvoja: ensisijaisen, toissijainen |  |                                                                                                                                                                                                              |          |


### <a name="redisconfiguration-properties"></a>RedisConfiguration ominaisuudet

| Ominaisuus                      | Kuvaus                                                                                                          | Hinnat tasoa       |
|-------------------------------|----------------------------------------------------------------------------------------------------------------------|---------------------|
| Varmuuskopiointi RDB käytössä            | Onko [Redis tietojen pysyvyyttä](cache-how-to-premium-persistence.md) on käytössä                                     | Vain Premium        |
| RDB-tallennustilan--yhteysmerkkijonon | Yhteysmerkkijonon [Redis tietojen pysyvyyttä](cache-how-to-premium-persistence.md) tallennustilan tiliin       | Vain Premium        |
| RDB varmuuskopiointi-korkojakso          | Varmuuskopion aikaväli [Redis tietojen pysyvyyttä](cache-how-to-premium-persistence.md)                               | Vain Premium        |
| maxmemory varattu            | Määrittää [varatun muistin](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) välimuisti kannalta | Vakio- ja Premium |
| maxmemory käytäntö              | Määrittää välimuistin [eviction käytäntö](cache-configure.md#maxmemory-policy-and-maxmemory-reserved)           | Kaikki hinnat tasoa   |
| Ilmoita-keyspace-tapahtumat        | Määrittää [keyspace ilmoitukset](cache-configure.md#keyspace-notifications-advanced-settings)                     | Vakio- ja Premium |
| Hash-max-ziplist-tapahtumat      | Määrittää pieni kooste tietotyyppien [muistin optimointi](http://redis.io/topics/memory-optimization)          | Vakio- ja Premium |
| Hash-max-ziplist-arvo        | Määrittää pieni kooste tietotyyppien [muistin optimointi](http://redis.io/topics/memory-optimization)          | Vakio- ja Premium |
| Set-max-intset-tapahtumat        | Määrittää pieni kooste tietotyyppien [muistin optimointi](http://redis.io/topics/memory-optimization)          | Vakio- ja Premium |
| zset-max-ziplist-tapahtumat      | Määrittää pieni kooste tietotyyppien [muistin optimointi](http://redis.io/topics/memory-optimization)          | Vakio- ja Premium |
| zset-max-ziplist-arvo        | Määrittää pieni kooste tietotyyppien [muistin optimointi](http://redis.io/topics/memory-optimization)          | Vakio- ja Premium |
| tietokannat                     | Määrittää tietokantojen määrä. Tämä ominaisuus voidaan määrittää vain välimuistin luominen.                          | Vakio- ja Premium |

## <a name="to-create-a-redis-cache"></a>Voit luoda Redis välimuisti

Azure Redis välimuistin uusia esiintymiä luodaan [Uusi AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) cmdlet-komennolla.

>[AZURE.IMPORTANT] Ensimmäistä kertaa, voit luoda Redis.txt välimuistin Azure-portaalissa tilauksen portaalin Rekisteröi `Microsoft.Cache` nimitilan kyseiseen tilaukseen. Jos yrität luoda ensimmäisen Redis.txt välimuistin PowerShellin tilaus, sinun on rekisteröitävä, käyttämällä seuraavaa komentoa; nimitila Muussa tapauksessa Cmdlet-komentoja, kuten `New-AzureRmRedisCache` ja `Get-AzureRmRedisCache` epäonnistuu.
>
>`Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Cache"`

Saat luettelon käytettävissä olevien parametrien ja niiden kuvaukset `New-AzureRmRedisCache`, suorita seuraava komento.

    PS C:\> Get-Help New-AzureRmRedisCache -detailed
    
    NAME
        New-AzureRmRedisCache
    
    SYNOPSIS
        Creates a new redis cache.
    
    
    SYNTAX
        New-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Location <String> [-RedisVersion <String>]
        [-Size <String>] [-Sku <String>] [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort
        <Boolean>] [-ShardCount <Integer>] [-VirtualNetwork <String>] [-Subnet <String>] [-StaticIP <String>]
        [<CommonParameters>]
    
    
    DESCRIPTION
        The New-AzureRmRedisCache cmdlet creates a new redis cache.
    
    
    PARAMETERS
        -Name <String>
            Name of the redis cache to create.
    
        -ResourceGroupName <String>
            Name of resource group in which to create the redis cache.
    
        -Location <String>
            Location in which to create the redis cache.
    
        -RedisVersion <String>
            RedisVersion is deprecated and will be removed in future release.
    
        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.
    
        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.
    
        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}
    
        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value, databases.
    
        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. If no value is provided, the default value is false and the
            non-SSL port will be disabled. Possible values are true and false.
    
        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.
    
        -VirtualNetwork <String>
            The exact ARM resource ID of the virtual network to deploy the redis cache in. Example format: /subscriptions/{
            subid}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicNetwork/VirtualNetworks/{vnetName}
    
        -Subnet <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.
    
        -StaticIP <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Voit luoda välimuistin oletusarvon parametreilla suorittamalla seuraavan komennon.

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US"

`ResourceGroupName`, `Name`, ja `Location` ovat pakollisia parametreja, mutta muiden ovat valinnaisia ja on oletusarvo. Edellisen komennon Luo vakio SKU Azure Redis-välimuistin esiintymän määritetty nimi, sijainti ja resurssiryhmä-, joka on vähintään 1 gt kokoisia käytöstä-SSL porttiin.

Voit luoda premium välimuistin, määrittää P1 (6 Gigatavua - 60 gt), P2 (13 gt - 130 gt), koosta P3 (26 gt - 260 gt), tai P4 (53 gt - 530 gt). Jotta klusterointi, Määritä shard laskeminen käyttämällä `ShardCount` parametria. Seuraavassa esimerkissä luodaan P1 premium-välimuistin 3 shards kanssa. P1 premium-välimuisti on 6 gt kokoisia ja on määritetty kolme shards yhteiskoko on 18 Gigatavua (gt 3 x 6).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P1 -ShardCount 3

Voit määrittää arvot `RedisConfiguration` parametrin kehystä sisällä arvot `{}` avaimen/arvona paria, kuten `@{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}`. Seuraavassa esimerkissä luodaan vakio 1 Gigatavua välimuistia kanssa `allkeys-random` maxmemory käytäntö ja keyspace ilmoitukset määritetty `KEA`. Lisätietoja on artikkeleissa [ilmoitusten Keyspace (Lisäasetukset)](cache-configure.md#keyspace-notifications-advanced-settings) ja [Maxmemory käytännön ja maxmemory varattu](cache-configure.md#maxmemory-policy-and-maxmemory-reserved).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}

<a name="databases"></a>
## <a name="to-configure-the-databases-setting-during-cache-creation"></a>Voit määrittää tietokannat-asetuksen välimuistin luonnin aikana

`databases` Asetus voidaan määrittää vain välimuistin luonnin aikana. Seuraavassa esimerkissä luodaan premium P3 (26 gt) välimuistin 48 tietokantoja, jotka on [Uusi AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) cmdlet-komennolla.

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P3 -RedisConfiguration @{"databases" = "48"}

Lisätietoja `databases` ominaisuutta, katso [Oletus Azure Redis välimuistin server-määritys](cache-configure.md#default-redis-server-configuration). Lisätietoja [Uusi AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) cmdlet-komennolla välimuistin luomisesta on kohdassa edellisen [Redis välimuistin luomiseen](#to-create-a-redis-cache) .

## <a name="to-update-a-redis-cache"></a>Jos haluat päivittää Redis.txt välimuisti

Azure Redis välimuistin esiintymät päivitetään [Määrittäminen AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) cmdlet-komennolla.

Saat luettelon käytettävissä olevien parametrien ja niiden kuvaukset `Set-AzureRmRedisCache`, suorita seuraava komento.

    PS C:\> Get-Help Set-AzureRmRedisCache -detailed
    
    NAME
        Set-AzureRmRedisCache
    
    SYNOPSIS
        Set redis cache updatable parameters.
    
    SYNTAX
        Set-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Size <String>] [-Sku <String>]
        [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort <Boolean>] [-ShardCount
        <Integer>] [<CommonParameters>]
    
    DESCRIPTION
        The Set-AzureRmRedisCache cmdlet sets redis cache parameters.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache to update.
    
        -ResourceGroupName <String>
            Name of the resource group for the cache.
    
        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.
    
        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.
    
        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}
    
        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value.
    
        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. The default value is null and no change will be made to the
            currently configured value. Possible values are true and false.
    
        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

`Set-AzureRmRedisCache` Cmdlet-komennon avulla voidaan päivittää ominaisuuksia, kuten `Size`, `Sku`, `EnableNonSslPort`, ja `RedisConfiguration` arvot. 

Seuraava komento päivittää maxmemory-käytännön nimeltä myCache Redis välimuistin.

    Set-AzureRmRedisCache -ResourceGroupName "myGroup" -Name "myCache" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random"}

<a name="scale"></a>
## <a name="to-scale-a-redis-cache"></a>Jos haluat skaalata Redis.txt välimuisti

`Set-AzureRmRedisCache`voidaan skaalata Azure Redis välimuistin milloin esiintymän `Size`, `Sku`, tai `ShardCount` ominaisuuksia muokataan. 

>[AZURE.NOTE]Skaalaus PowerShellin välimuistin peritään samat rajoitukset ja ohjeita kuin skaalaus välimuistin Azure-portaalista. Voit skaalata, seuraavin poikkeuksin eri hinnoittelu taso.
>
>-  Ei skaalaudu suurempi hinta taso, pienet hinnoittelu taso.
>    -    Ei skaalaudu **Premium** -välimuistin alaspäin **Normaali** tai **Basic** välimuistin.
>    -    Ei skaalaudu **Vakio** välimuistista **Basic** välimuistin alaspäin.
>-  Voit skaalata **Basic** välimuistista **Vakio** välimuistiin, mutta et voi muuttaa koon yhtä aikaa. Jos tarvitset erikokoinen, tee myöhemmin skaalauksen toiminnon haluamasi kokoiseksi.
>-  Ei skaalaudu **Basic** välimuistista siirtyä **Premium** -välimuistin. On skaalata **Basic** **Vakio** skaalauksen kerralla ja sitten **Standard** **Premium** myöhemmin skaalauksen toiminnossa.
>-  Ei skaalaudu-suurentaminen alaspäin **C0 (250 Mt)** koon.
>
>Lisätietoja on artikkelissa [asteikko Azure Redis välimuistin poistamisesta](cache-how-to-scale.md).

Seuraavassa esimerkissä esitetään, miten nimeltä välimuistin `myCache` 2,5 Gigatavua välimuistiin. Huomaa, että tämä komento toimii Basic- tai vakio välimuistin.

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

Kun tämä komento on annettu, funktio palauttaa välimuistin tila (samalla tavalla kuin soitat `Get-AzureRmRedisCache`). Huomaa, että `ProvisioningState` on `Scaling`.

    PS C:\> Set-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup -Size 2.5GB
    
    
    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/mygroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Scaling
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 150], [notify-keyspace-events, KEA],
                         [maxmemory-delta, 150]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : mygroup
    PrimaryKey         : ....
    SecondaryKey       : ....
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

Kun skaalauksen toiminto on valmis, `ProvisioningState` muutoksista `Succeeded`. Jos haluat myöhemmin skaalauksen toiminto, kuten muuttaminen standardiksi perustiedot ja muuttamalla sitten kokoa, on odotettava, kunnes edellinen toiminto on valmis tai näyttöön tulee seuraavanlainen virhesanoma.

    Set-AzureRmRedisCache : Conflict: The resource '...' is not in a stable state, and is currently unable to accept the update request.

## <a name="to-get-information-about-a-redis-cache"></a>Saat lisätietoja Redis.txt välimuisti

Voit noutaa tietoja välimuistin [Get-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634514.aspx) cmdlet-komennolla.

Saat luettelon käytettävissä olevien parametrien ja niiden kuvaukset `Get-AzureRmRedisCache`, suorita seuraava komento.

    PS C:\> Get-Help Get-AzureRmRedisCache -detailed
    
    NAME
        Get-AzureRmRedisCache
    
    SYNOPSIS
        Gets details about a single cache or all caches in the specified resource group or all caches in the current
        subscription.
    
    SYNTAX
        Get-AzureRmRedisCache [-Name <String>] [-ResourceGroupName <String>] [<CommonParameters>]
    
    DESCRIPTION
        The Get-AzureRmRedisCache cmdlet gets the details about a cache or caches depending on input parameters. If both
        ResourceGroupName and Name parameters are provided then Get-AzureRmRedisCache will return details about the
        specific cache name provided.
    
        If only ResourceGroupName is provided than it will return details about all caches in the specified resource group.
    
        If no parameters are given than it will return details about all caches the current subscription.
    
    PARAMETERS
        -Name <String>
            The name of the cache. When this parameter is provided along with ResourceGroupName, Get-AzureRmRedisCache
            returns the details for the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache or caches. If ResourceGroupName is provided with Name
            then Get-AzureRmRedisCache returns the details of the cache specified by Name. If only the ResourceGroup
            parameter is provided, then details for all caches in the resource group are returned.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Palauttaa kaikki tallentaa tietoja nykyisen tilauksesi, suorita `Get-AzureRmRedisCache` ilman parametreja.

    Get-AzureRmRedisCache

Palauttaa kaikki tallentaa tietoja tietyn resurssiryhmän, suorita `Get-AzureRmRedisCache` kanssa `ResourceGroupName` parametria.

    Get-AzureRmRedisCache -ResourceGroupName myGroup

Palauttamaan tietoja tietyn välimuisti suorittamisen `Get-AzureRmRedisCache` kanssa `Name` välimuisti-nimen sisältävä parametrin ja `ResourceGroupName` parametrin sisältävä välimuistin resurssiryhmä.

    PS C:\> Get-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup
    
    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/myGroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Succeeded
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 62], [notify-keyspace-events, KEA],
                         [maxclients, 1000]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : myGroup
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

## <a name="to-retrieve-the-access-keys-for-a-redis-cache"></a>Noutaa Redis.txt välimuistin pikanäppäimet

Noutaa välimuistin pikanäppäimet, voit käyttää [Get-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634516.aspx) cmdlet-komento.

Saat luettelon käytettävissä olevien parametrien ja niiden kuvaukset `Get-AzureRmRedisCacheKey`, suorita seuraava komento.

    PS C:\> Get-Help Get-AzureRmRedisCacheKey -detailed
    
    NAME
        Get-AzureRmRedisCacheKey
    
    SYNOPSIS
        Gets the accesskeys for the specified redis cache.
    
    
    SYNTAX
        Get-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> [<CommonParameters>]
    
    DESCRIPTION
        The Get-AzureRmRedisCacheKey cmdlet gets the access keys for the specified cache.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache.
    
        -ResourceGroupName <String>
            Name of the resource group for the cache.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Noutaa välimuistin näppäimet, soita `Get-AzureRmRedisCacheKey` cmdlet-komento ja välittää välimuistin Kirjoita nimi, joka sisältää välimuistin resurssiryhmän nimi.

    PS C:\> Get-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup
    
    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : ABhfB757JgjIgt785JgKH9865eifmekfnn649303JKL=

## <a name="to-regenerate-access-keys-for-your-redis-cache"></a>Uudelleen Redis.txt välimuistin pikanäppäimet

Uudelleen välimuistin pikanäppäimet, voit käyttää [Uutta AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634512.aspx) cmdlet-komento.

Saat luettelon käytettävissä olevien parametrien ja niiden kuvaukset `New-AzureRmRedisCacheKey`, suorita seuraava komento.

    PS C:\> Get-Help New-AzureRmRedisCacheKey -detailed
    
    NAME
        New-AzureRmRedisCacheKey
    
    SYNOPSIS
        Regenerates the access key of a redis cache.
    
    SYNTAX
        New-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> -KeyType <String> [-Force] [<CommonParameters>]
    
    DESCRIPTION
        The New-AzureRmRedisCacheKey cmdlet regenerate the access key of a redis cache.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache.
    
        -ResourceGroupName <String>
            Name of the resource group for the cache.
    
        -KeyType <String>
            Specifies whether to regenerate the primary or secondary access key. Possible values are Primary or Secondary.
    
        -Force
            When the Force parameter is provided, the specified access key is regenerated without any confirmation prompts.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
    
Välimuistin ensisijaisen tai toissijaisen näppäintä uudelleen, soita `New-AzureRmRedisCacheKey` cmdlet-komento ja kirjoita nimi-resurssiryhmä-hyväksytty ja määrittää joko `Primary` tai `Secondary` varten `KeyType` parametri. Seuraavassa esimerkissä luodaan uudelleen välimuistiin toissijaisen pikanäppäin.

    PS C:\> New-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup -KeyType Secondary
    
    Confirm
    Are you sure you want to regenerate Secondary key for redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
    
    
    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : c53hj3kh4jhHjPJk8l0jji785JgKH9865eifmekfnn6=

## <a name="to-delete-a-redis-cache"></a>Jos haluat poistaa Redis.txt välimuisti

Voit poistaa Redis.txt välimuisti-sovelluksella [Poista AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634515.aspx) cmdlet-komento.

Saat luettelon käytettävissä olevien parametrien ja niiden kuvaukset `Remove-AzureRmRedisCache`, suorita seuraava komento.

    PS C:\> Get-Help Remove-AzureRmRedisCache -detailed
    
    NAME
        Remove-AzureRmRedisCache
    
    SYNOPSIS
        Remove redis cache if exists.
    
    SYNTAX
        Remove-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Force] [-PassThru] [<CommonParameters>
    
    DESCRIPTION
        The Remove-AzureRmRedisCache cmdlet removes a redis cache if it exists.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache to remove.
    
        -ResourceGroupName <String>
            Name of the resource group of the cache to remove.
    
        -Force
            When the Force parameter is provided, the cache is removed without any confirmation prompts.
    
        -PassThru
            By default Remove-AzureRmRedisCache removes the cache and does not return any value. If the PassThru par
            is provided then Remove-AzureRmRedisCache returns a boolean value indicating the success of the operatio
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Seuraavassa esimerkissä välimuistin nimeltä `myCache` poistetaan.

    PS C:\> Remove-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup
    
    Confirm
    Are you sure you want to remove redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


## <a name="to-import-a-redis-cache"></a>Voit tuoda Redis.txt välimuisti

Voit tuoda tietoja Azure Redis välimuistin esiintymän avulla `Import-AzureRmRedisCache` cmdlet-komento.

>[AZURE.IMPORTANT] Tuo tai Vie on käytettävissä vain [premium taso](cache-premium-tier-intro.md) tallentaa. Saat lisätietoja Tuo/Vie [Azure Redis välimuistin tietojen tuominen ja vieminen](cache-how-to-import-export-data.md).

Saat luettelon käytettävissä olevien parametrien ja niiden kuvaukset `Import-AzureRmRedisCache`, suorita seuraava komento.

    PS C:\> Get-Help Import-AzureRmRedisCache -detailed
    
    NAME
        Import-AzureRmRedisCache
    
    SYNOPSIS
        Import data from blobs to Azure Redis Cache.
    
    
    SYNTAX
        Import-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Files <String[]> [-Format <String>] [-Force]
        [-PassThru] [<CommonParameters>]
    
    
    DESCRIPTION
        The Import-AzureRmRedisCache cmdlet imports data from the specified blobs into Azure Redis Cache.
    
    
    PARAMETERS
        -Name <String>
            The name of the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache.
    
        -Files <String[]>
            SAS urls of blobs whose content should be imported into the cache.
    
        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.
    
        -Force
            When the Force parameter is provided, import will be performed without any confirmation prompts.
    
        -PassThru
            By default Import-AzureRmRedisCache imports data in cache and does not return any value. If the PassThru
            parameter is provided then Import-AzureRmRedisCache returns a boolean value indicating the success of the
            operation.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
    

Seuraava komento tuo tiedot määrittämää SAS uri Azure Redis välimuistiin Blob-objektien.


    PS C:\>Import-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Files @("https://mystorageaccount.blob.core.windows.net/mycontainername/blobname?sv=2015-04-05&sr=b&sig=caIwutG2uDa0NZ8mjdNJdgOY8%2F8mhwRuGNdICU%2B0pI4%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwd") -Force

## <a name="to-export-a-redis-cache"></a>Vie Redis.txt välimuisti

Voit viedä tietoja Azure Redis välimuistin esiintymän käyttämisen `Export-AzureRmRedisCache` cmdlet-komento.

>[AZURE.IMPORTANT] Tuo tai Vie on käytettävissä vain [premium taso](cache-premium-tier-intro.md) tallentaa. Saat lisätietoja Tuo/Vie [Azure Redis välimuistin tietojen tuominen ja vieminen](cache-how-to-import-export-data.md).

Saat luettelon käytettävissä olevien parametrien ja niiden kuvaukset `Export-AzureRmRedisCache`, suorita seuraava komento.

    PS C:\> Get-Help Export-AzureRmRedisCache -detailed
    
    NAME
        Export-AzureRmRedisCache
    
    SYNOPSIS
        Exports data from Azure Redis Cache to a specified container.
    
    
    SYNTAX
        Export-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Prefix <String> -Container <String> [-Format
        <String>] [-PassThru] [<CommonParameters>]
    
    
    DESCRIPTION
        The Export-AzureRmRedisCache cmdlet exports data from Azure Redis Cache to a specified container.
    
    
    PARAMETERS
        -Name <String>
            The name of the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache.
    
        -Prefix <String>
            Prefix to use for blob names.
    
        -Container <String>
            SAS url of container where data should be exported.
    
        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.
    
        -PassThru
            By default Export-AzureRmRedisCache does not return any value. If the PassThru parameter is provided
            then Export-AzureRmRedisCache returns a boolean value indicating the success of the operation.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


Seuraava komento vie tiedot SAS uri määrittämää säilöön Azure Redis välimuistin esiintymä.


        PS C:\>Export-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Prefix "blobprefix"
        -Container "https://mystorageaccount.blob.core.windows.net/mycontainer?sv=2015-04-05&sr=c&sig=HezZtBZ3DURmEGDduauE7
        pvETY4kqlPI8JCNa8ATmaw%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwdl"

## <a name="to-reboot-a-redis-cache"></a>Käynnistämään Redis.txt välimuisti

Azure Redis välimuisti-esiintymä-avulla voit käynnistää uudelleen `Reset-AzureRmRedisCache` cmdlet-komento.

>[AZURE.IMPORTANT] Käynnistä on käytettävissä vain [premium taso](cache-premium-tier-intro.md) tallentaa. Saat lisätietoja uudelleenkäynnistyksen välimuistin [Välimuistin hallinnan - uudelleen](cache-administration.md#reboot).

Saat luettelon käytettävissä olevien parametrien ja niiden kuvaukset `Reset-AzureRmRedisCache`, suorita seuraava komento.

    PS C:\> Get-Help Reset-AzureRmRedisCache -detailed
    
    NAME
        Reset-AzureRmRedisCache
    
    SYNOPSIS
        Reboot specified node(s) of an Azure Redis Cache instance.
    
    
    SYNTAX
        Reset-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -RebootType <String> [-ShardId <Integer>]
        [-Force] [-PassThru] [<CommonParameters>]
    
    
    DESCRIPTION
        The Reset-AzureRmRedisCache cmdlet reboots the specified node(s) of an Azure Redis Cache instance.
    
    
    PARAMETERS
        -Name <String>
            The name of the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache.
    
        -RebootType <String>
            Which node to reboot. Possible values are "PrimaryNode", "SecondaryNode", "AllNodes".
    
        -ShardId <Integer>
            Which shard to reboot when rebooting a premium cache with clustering enabled.
    
        -Force
            When the Force parameter is provided, reset will be performed without any confirmation prompts.
    
        -PassThru
            By default Reset-AzureRmRedisCache does not return any value. If the PassThru parameter is provided
            then Reset-AzureRmRedisCache returns a boolean value indicating the success of the operation.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
    

Seuraava komento käynnistetään uudelleen molemmat solmut määritetyn välimuistin.

    
        PS C:\>Reset-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -RebootType "AllNodes"
        -Force
    

## <a name="next-steps"></a>Seuraavat vaiheet

Saat lisätietoja Windows PowerShellin Azure kanssa on seuraavissa resursseissa:

- [Azure Redis välimuistin cmdlet-komento MSDN: N ohjeet](https://msdn.microsoft.com/library/azure/mt634513.aspx)
- [Azure resurssien hallinnan cmdlet-komennot](http://go.microsoft.com/fwlink/?LinkID=394765): Opi käyttämään AzureResourceManager-moduulissa Cmdlet-komentoja.
- [Resurssin ryhmiä hallitsemaan Azure resursseja](../resource-group-template-deploy-portal.md): Katso, miten voit luoda ja hallita resurssin Azure-portaalissa.
- [Azure blogi](http://blogs.msdn.com/windowsazure): Lisätietoja uusista toiminnoista Azure-tietokannassa.
- [Windows PowerShell-blogi](http://blogs.msdn.com/powershell): Lisätietoja Windows PowerShell-uudet ominaisuudet.
- ["Hei, Scripting Guy!" Blogin](http://blogs.technet.com/b/heyscriptingguy/): Pyydä tosielämän vihjeitä ja vinkkejä Windows PowerShell-yhteisöltä.
