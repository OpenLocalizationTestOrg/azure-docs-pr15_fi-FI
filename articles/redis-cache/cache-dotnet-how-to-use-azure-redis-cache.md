<properties 
    pageTitle="Käyttämisestä Azure Redis.txt välimuistin | Microsoft Azure" 
    description="Opettele Azure sovellustesi Azure Redis välimuistin ja suorituskyvyn parantaminen" 
    services="redis-cache,app-service" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="dotnet" 
    ms.topic="hero-article" 
    ms.date="08/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache"></a>Opi käyttämään Azure Redis.txt välimuisti

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Tämän oppaan avulla voit **Azure Redis välimuistin**käytön aloittamisessa. Microsoft Azure Redis välimuistin perustuu Suositut Avaa lähde Redis välimuistin. Sen kautta pääset käsiksi suojattua ja erillinen Redis.txt välimuistiin, Microsoft hallitsee. Välimuistin, joka on luotu Azure Redis välimuistin on käytettävissä mistä tahansa sovelluksesta Microsoft Azure kuluessa.

Microsoft Azure Redis välimuistin on käytettävissä seuraavassa tasoa:

-   **Tavallinen** – yksi solmu. Useita koko on enintään 53 Gigatavua.
-   **Vakio** – kahden solmun ensisijainen/replikan. Useita koko on enintään 53 Gigatavua. 99,9 % SLA.
-   **Premium** – kahden solmun ensisijainen/replikaa enintään 10 shards. Useita koot-6 Gigatavua 530 Gigatavua (yhteystiedot Lisää). Kaikkia vakio taso-ominaisuuksia ja [Redis klusterin](cache-how-to-premium-clustering.md), [Redis pysyvyys](cache-how-to-premium-persistence.md)ja [Azure Virtual Network](cache-how-to-premium-vnet.md)mukaan lukien lisäohjeita. 99,9 % SLA.

Kunkin tason eroaa ominaisuudet ja hinnoittelusta. Lisätietoja hinnat on artikkelissa [Välimuistin hinnat tiedot][].

Tämän oppaan avulla voit [StackExchange.Redis][] -asiakasohjelman käyttäminen C\# koodi. Kattaa käyttötavoista Sisällytä **luominen ja määrittäminen välimuistin** **määrittäminen välimuistin asiakkaat**ja **lisäämällä ja poistamalla objektien välimuistista**. Lisätietoja saat lisätietoja Azure Redis välimuistin [Seuraavat vaiheet][] -osassa. Katso vaiheittaiset opetusohjelmassa muodostamisen ASP.NET-MVC web app, jossa Redis välimuistin [luomisesta ja Redis välimuistin verkkosovellukseen](cache-web-app-howto.md).

<a name="getting-started-cache-service"></a>
## <a name="get-started-with-azure-redis-cache"></a>Azure Redis.txt välimuistin käytön aloittaminen

Azure Redis välimuistin käytön aloittaminen on helppoa. Aloita valmistelu ja määritä välimuistin. Seuraavaksi välimuistin asiakkaiden Määritä, jotta he pääsevät välimuistin. Kun välimuistin asiakkaiden on määritetty, voit aloittaa työskentelyn ne.

-   [Luo välimuisti][]
-   [Välimuistin-asiakkaiden määrittäminen][]

<a name="create-cache"></a>
## <a name="create-a-cache"></a>Luoda välimuistin

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

### <a name="to-access-your-cache-after-its-created"></a>Jos haluat käyttää välimuistin sen jälkeen luodaan

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Lisätietoja välimuistin määrittämisestä on artikkelissa [Azure Redis välimuistin määrittämisestä](cache-configure.md).

<a name="NuGet"></a>
## <a name="configure-the-cache-clients"></a>Välimuistin-asiakkaiden määrittäminen

[AZURE.INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

Kun asiakkaan projektin on määritetty tallentamisesta välimuistiin, voit käyttää välimuistin käsittelyyn seuraavissa osissa kuvataan tapoja, joilla.

<a name="working-with-caches"></a>
## <a name="working-with-caches"></a>Tallentaa käsitteleminen

Tässä osassa kuvataan, miten välimuistin yleisten tehtävien tekemiseen.

-   [Yhteyden muodostaminen välimuisti][]
-   [Lisää ja hakea objekteja välimuistista][]
-   [Välimuistin .NET-objektien käyttäminen](#work-with-net-objects-in-the-cache)

<a name="connect-to-cache"></a>
## <a name="connect-to-the-cache"></a>Yhteyden muodostaminen välimuisti

Jotta voit käsitellä ohjelmallisesti välimuistiin, on välimuistin viittaus. Lisää seuraavat minkä tahansa tiedoston, josta haluat käyttämään StackExchange.Redis asiakkaan Azure Redis välimuistin yläpuolelle.

    using StackExchange.Redis;

>[AZURE.NOTE] StackExchange.Redis Clientin .NET Framework 4 tai uudempi.

Azure Redis välimuistin muodostaminen hallitsee `ConnectionMultiplexer` luokka. Tähän luokkaan tarkoituksena on tallennettu ja koko asiakassovellus uudelleen ja ei tarvitse luoda kohti toiminnon välein. 

Muodosta yhteys Azure Redis välimuistin ja palautetaan esiintymä yhdistetty `ConnectionMultiplexer`, Soita staattisen `Connect` menetelmä ja Välitä välimuistin päätepiste ja avaimen avulla, kuten seuraavassa esimerkissä. Käytä sitten luotu salasana parametrina Azure-portaalista.

    ConnectionMultiplexer connection = ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");

>[AZURE.IMPORTANT] Varoitus: Koskaan säilön tunnistetietojen lähdekoodin. Jos haluat säilyttää tässä esimerkissä yksinkertainen, voin 'M näyttämättä niitä lähdekoodin. Saat lisätietoja tunnistetietojen tallentaminen [miten sovelluksen merkkijonot ja yhteyden merkkijonoja][] .

Jos et halua käyttää SSL: ää, joko määrittää `ssl=false` tai jätä pois `ssl` parametria.

>[AZURE.NOTE] SSL portin on poissa käytöstä oletusarvoisesti uusi tallentaa. Ohjeita-SSL portin ottamisesta käyttöön on artikkelissa [Access-portit](cache-configure.md#access-ports)...

Yksi tapa jakaminen `ConnectionMultiplexer` sovelluksen esiintymää on staattinen ominaisuus, joka palauttaa yhdistetyn esiintymän, samalla tavalla kuin seuraavassa esimerkissä. Viestiketjun sopivaa näin alustaa vain yksi yhdistetty `ConnectionMultiplexer` esiintymä. Näissä esimerkeissä `abortConnect` on määritetty epätosi, mikä tarkoittaa, että puhelu onnistuu, vaikka Azure Redis välimuistiin yhteys muodostetaan ei. Yksi tärkeimmistä ominaisuus `ConnectionMultiplexer` on, että se palauttaa yhteyden automaattisesti välimuistiin kerran verkko-ongelma tai muista syistä selvitetään.

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

Lisätietoja yhteyden Lisäasetukset asetukset-kohdassa [StackExchange.Redis määritysmalli][].

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

Kun yhteys on muodostettu, palauttaa viittauksen Redis.txt välimuistin tietokannan soittamalla `ConnectionMultiplexer.GetDatabase` menetelmää. Palauttaa objektin `GetDatabase` menetelmä on kevyt läpivienti-objekti ja ei tarvitse tallennetaan.

    // Connection refers to a property that returns a ConnectionMultiplexer
    // as shown in the previous example.
    IDatabase cache = Connection.GetDatabase();

    // Perform cache operations using the cache object...
    // Simple put of integral data types into the cache
    cache.StringSet("key1", "value");
    cache.StringSet("key2", 25);

    // Simple get of data types from the cache
    string key1 = cache.StringGet("key1");
    int key2 = (int)cache.StringGet("key2");

Nyt kun osaat Azure Redis välimuistin esiintymään ja palauttaa viittauksen välimuisti-tietokantaan, voit tarkastella välimuistin käsitteleminen.

<a name="add-object"></a>
## <a name="add-and-retrieve-objects-from-the-cache"></a>Lisää ja hakea objekteja välimuistista

Kohteet tallennetaan- ja noutaa välimuistista käyttämällä `StringSet` ja `StringGet` tavoista.

    // If key1 exists, it is overwritten.
    cache.StringSet("key1", "value1");

    string value = cache.StringGet("key1");

Redis eniten tietoja Redis.txt merkkijonoja, mutta näiden merkkijonojen voivat sisältää erilaisia tietotyyppejä, kuten sarjoitettu binaaritietoja, jota voidaan käyttää, kun tallentaminen .NET objektien välimuistin stores.

Soitettaessa `StringGet`, jos objekti on luotu, se palautetaan, jos se ei ole, `null` palautetaan. Tässä tapauksessa voit hakea arvon haluamasi tietolähteestä ja tallentaa sen myöhempää käyttöä varten välimuistin. Tätä kutsutaan välimuistin kesantoala kuvio.

    string value = cache.StringGet("key1");
    if (value == null)
    {
        // The item keyed by "key1" is not in the cache. Obtain
        // it from the desired data source and add it to the cache.
        value = GetValueFromDataSource();

        cache.StringSet("key1", value);
    }

Voit määrittää kohteen voimassaolon välimuistin `TimeSpan` parametri `StringSet`.

    cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));

## <a name="work-with-net-objects-in-the-cache"></a>Välimuistin .NET-objektien käyttäminen

Azure Redis välimuistin voit välimuistiin .NET objektit sekä alkeistyyppi tietotyyppejä, mutta ennen kuin .NET-objektin voi olla välimuistiin se on voi muuntaa sarjaksi. Tämä on sovelluksen kehittäjä on vastuussa ja luo developer joustavuutta Sarjatoiminto valinta.

Yksi helppo tapa onnistu objektit on käytettävä `JsonConvert` Sarjatoiminto esitetyssä [Newtonsoft.Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/8.0.1-beta1) ja muuntaa sarjaksi ja sieltä pois JSON. Seuraavassa esimerkissä esitetään, Hae ja määritä käyttämällä `Employee` objektiesiintymän.


    class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }
    
        public Employee(int EmployeeId, string Name)
        {
            this.Id = EmployeeId;
            this.Name = Name;
        }
    }

    // Store to cache
    cache.StringSet("e25", JsonConvert.SerializeObject(new Employee(25, "Clayton Gragg")));

    // Retrieve from cache
    Employee e25 = JsonConvert.DeserializeObject<Employee>(cache.StringGet("e25"));

<a name="next-steps"></a>
## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet oppinut perustiedot, näistä linkeistä saat lisätietoja Azure Redis välimuistin.

-   Tutustu Azure Redis välimuistin ASP.NET-palvelut.
    -   [Azure Redis.txt istunnon tila-palvelu](cache-aspnet-session-state-provider.md)
    -   [Azure Redis välimuistin ASP.NET tulosteen välimuistin toimittaja](cache-aspnet-output-cache-provider.md)
-   [Käyttöön välimuistin diagnostiikka](cache-how-to-monitor.md#enable-cache-diagnostics) , niin voit [näytön](cache-how-to-monitor.md) välimuistin kunto. Voit tarkastella määritetty Azure-portaalin ja voit myös [ladata ja tarkastella](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) niitä valittua työkaluilla.
-   Tutustu [StackExchange.Redis välimuistin asiakkaan ohjeissa][].
    -   Azure Redis välimuistin niitä voi käyttää Redis.txt asiakkaat ja kehitys kieliä. Lisätietoja on artikkelissa [http://redis.io/clients][].
-   Azure Redis välimuistin voidaan myös kolmannen osapuolen palvelujen ja työkaluilla, kuten Redsmin ja Redis Desktop Manager.
    -   Saat lisätietoja Redsmin [Azure Redis yhteysmerkkijonon noutaminen ja käyttää sitä Redsmin kanssa][].
    -   Käyttäminen ja tarkistaminen ja Graafisen, käyttämällä [RedisDesktopManager](https://github.com/uglide/RedisDesktopManager)Azure Redis välimuistin tietoja.
-   Ohjeissa [redis][] ja lisätietoja [redis tietotyypit][] ja [15 minuutin-johdanto Redis tietotyypit][].



<!-- INTRA-TOPIC LINKS -->
[Seuraavat vaiheet]: #next-steps
[Introduction to Azure Redis Cache (Video)]: #video
[What is Azure Redis Cache?]: #what-is
[Create an Azure Cache]: #create-cache
[Which type of caching is right for me?]: #choosing-cache
[Prepare Your Visual Studio Project to Use Azure Caching]: #prepare-vs
[Configure Your Application to Use Caching]: #configure-app
[Get Started with Azure Redis Cache]: #getting-started-cache-service
[Luo välimuisti]: #create-cache
[Configure the cache]: #enable-caching
[Välimuistin-asiakkaiden määrittäminen]: #NuGet
[Working with Caches]: #working-with-caches
[Yhteyden muodostaminen välimuisti]: #connect-to-cache
[Lisää ja hakea objekteja välimuistista]: #add-object
[Specify the expiration of an object in the cache]: #specify-expiration
[Store ASP.NET session state in the cache]: #store-session

  
<!-- IMAGES -->


[StackExchangeNuget]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-stackexchange-redis.png

[NuGetMenu]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-nuget-menu.png

[CacheProperties]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-properties.png

[ManageKeys]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-keys.png

[SessionStateNuGet]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-session-state-provider.png

[BrowseCaches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-browse-caches.png

[Caches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-caches.png






   
<!-- LINKS -->
[http://Redis.IO/clients]: http://redis.io/clients
[Develop in other languages for Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn690470.aspx
[Azure Redis yhteysmerkkijonon noutaminen ja käyttää sitä Redsmin]: https://redsmin.uservoice.com/knowledgebase/articles/485711-how-to-connect-redsmin-to-azure-redis-cache
[Azure Redis Session State Provider]: http://go.microsoft.com/fwlink/?LinkId=398249
[How to: Configure a Cache Client Programmatically]: http://msdn.microsoft.com/library/windowsazure/gg618003.aspx
[Session State Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320835
[Azure AppFabric Cache: Caching Session State]: http://www.microsoft.com/showcase/details.aspx?uuid=87c833e9-97a9-42b2-8bb1-7601f9b5ca20
[Output Cache Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320837
[Azure Shared Caching]: http://msdn.microsoft.com/library/windowsazure/gg278356.aspx
[Team Blog]: http://blogs.msdn.com/b/windowsazure/
[Azure Caching]: http://www.microsoft.com/showcase/Search.aspx?phrase=azure+caching
[How to Configure Virtual Machine Sizes]: http://go.microsoft.com/fwlink/?LinkId=164387
[Azure Caching Capacity Planning Considerations]: http://go.microsoft.com/fwlink/?LinkId=320167
[Azure Caching]: http://go.microsoft.com/fwlink/?LinkId=252658
[How to: Set the Cacheability of an ASP.NET Page Declaratively]: http://msdn.microsoft.com/library/zd1ysf1y.aspx
[How to: Set a Page's Cacheability Programmatically]: http://msdn.microsoft.com/library/z852zf6b.aspx
[Configure a cache in Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn793612.aspx

[StackExchange.Redis määritys mallia]: http://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Configuration.md

[Work with .NET objects in the cache]: http://msdn.microsoft.com/library/dn690521.aspx#Objects


[NuGet Package Manager Installation]: http://go.microsoft.com/fwlink/?LinkId=240311
[Välimuistin hinnoittelutiedot]: http://www.windowsazure.com/pricing/details/cache/
[Azure Portal]: https://portal.azure.com/

[Overview of Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=320830
[Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=398247

[Migrate to Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=317347
[Azure Redis Cache Samples]: http://go.microsoft.com/fwlink/?LinkId=320840
[Using Resource groups to manage your Azure resources]: ../azure-resource-manager/resource-group-overview.md

[StackExchange.Redis]: http://github.com/StackExchange/StackExchange.Redis
[StackExchange.Redis välimuistin asiakkaan dokumentaatio]: http://github.com/StackExchange/StackExchange.Redis#documentation

[Redis.txt]: http://redis.io/documentation
[Redis tietotyypit]: http://redis.io/topics/data-types
[15 minuutin-johdanto Redis.txt-tietotyypit]: http://redis.io/topics/data-types-intro

[Sovelluksen merkkijonot ja yhteyden merkkijonojen maksaminen]: http://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/


