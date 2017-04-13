<properties 
    pageTitle="Välimuistin siirtäminen Redis.txt | Microsoft Azure"
    description="Opi siirtämään hallitun välimuisti-palvelun sovellusten Azure Redis välimuisti"
    services="redis-cache"
    documentationCenter="na"
    authors="steved0x"
    manager="douge"
    editor="tysonn" />
<tags 
    ms.service="cache"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="09/30/2016"
    ms.author="sdanie" />

# <a name="migrate-from-managed-cache-service-to-azure-redis-cache"></a>Siirtää hallitun välimuisti-palvelun Azure Redis.txt välimuisti

Sovellukset, jotka Azure hallitun välimuisti-palvelun avulla Azure Redis välimuistin siirtäminen onnistuu mahdollisimman vähän muutokset sovelluksesi välimuistiin sovellus käyttää hallitun välimuisti-palvelun ominaisuuksien mukaan. Kun API ei ole täysin sama ne vastaavat ja paljon aiemmin luotu koodi, joka käyttää hallitun välimuisti-palvelun välimuistiin voi käyttää uudelleen vähäisiä muutoksia. Tässä ohjeaiheessa kerrotaan, miten voit tehdä tarvittavat määritykset ja sovelluksen muutoksia siirtämään hallitun välimuisti-palvelun-sovellusten käyttäminen Azure Redis välimuistin ja näyttää, miten joitakin ominaisuuksia Azure Redis välimuistin avulla voidaan toteuttaa hallitun välimuisti-palvelun välimuistin toimintoja.

## <a name="migration-steps"></a>Siirron vaiheet

Siirtää hallitun välimuisti-palvelun-sovelluksen käyttämään Azure Redis välimuistin edellyttää seuraavien ohjeiden mukaisesti.

-   Välimuisti-palvelun hallittujen ominaisuuksien yhdistäminen Azure Redis välimuisti
-   Valitse välimuistin tarjoaa
-   Luoda välimuistin
-   Välimuistin-asiakkaiden määrittäminen
    -   Poista hallitun välimuisti-palvelun määritykset
    -   Määrittää välimuistin asiakkaan StackExchange.Redis NuGet pakkaaminen
-   Siirtää välimuistin palvelukoodi hallinnasta
    -   Yhteyden muodostaminen välimuistin ConnectionMultiplexer-luokan avulla
    -   Accessin alkeistyyppi tietotyyppien välimuistin
    -   Välimuistin .NET-objektien käyttäminen
-   ASP.NET-istunnon tila ja tulostusvälimuistin Azure Redis välimuistiin 

## <a name="map-managed-cache-service-features-to-azure-redis-cache"></a>Välimuisti-palvelun hallittujen ominaisuuksien yhdistäminen Azure Redis välimuisti

Azure hallitun välimuisti-palvelun ja Azure Redis välimuisti muistuttavat mutta Toteuta joitakin ominaisuuksia, niiden eri tavoilla. Tässä osassa käsitellään joitakin eroja ja toteuttaminen välimuisti-palvelun hallittujen ominaisuuksien Azure Redis välimuistin selvennetään.

|Välimuisti-palvelun hallittu ominaisuus|Hallitut välimuisti-palvelun tuki|Azure Redis välimuisti-tuki|
|---|---|---|
|Nimetty tallentaa|Oletus-välimuistin on määritetty ja Standard- ja Premium välimuistin palveluja, enintään yhdeksän muita nimeltä tallentaa voi määrittää tarvittaessa.|Azure Redis.txt tallentaa on määritettävä määrä tietokantoja (16 oletus), joiden avulla voidaan toteuttaa samankaltaisia toimintoja, nimetyn tallentaa. Lisätietoja on artikkelissa [Oletus Redis server-määritys](cache-configure.md#default-redis-server-configuration).|
|Suuri käytettävyys|On suuri käytettävyys vakio- ja Premium välimuisti-versioiden välimuistissa olevien kohteiden. Jos kohteet ovat kadonneet virheen vuoksi, varmuuskopioita välimuistin kohteet ovat käytettävissä. Toissijainen välimuisti kirjoittaa tehdään synkronoidusti.|Suuren käytettävyyden on käytettävissä vakio- ja Premium välimuistin palveluja, joilla on kaksi solmu ensisijainen/replikan määrityksen (kunkin shard Premium välimuistiin on ensisijainen/replikan pari). Kirjoittaa se tehdään asynkronisesti. Lisätietoja on artikkelissa [Azure Redis välimuistin hinnat](https://azure.microsoft.com/pricing/details/cache/).|
|Ilmoitukset|Sallii asiakkaiden ilmoituksia asynkroninen välimuistin toimintoja eri toteutuessa nimetty välimuistin.|Asiakassovellukset käyttää Redis.txt kirja ja sub tai [Keyspace ilmoitukset](cache-configure.md#keyspace-notifications-advanced-settings) saavuttamiseksi ilmoitukset samankaltaisia toimintoja.|
|Paikallisen välimuistin|Tallentaa kopion välimuistiin tallennettujen objektien paikallisesti asiakkaan tuomioistuinten ulkopuolella nopeasti.|Asiakassovellukset on tämän toiminnon sanaston tai samanlaisia tietorakenne pantava täytäntöön.|
|Eviction käytäntö|Ei mitään tai LRU. Oletuskäytäntö on LRU.|Azure Redis välimuistin tukee seuraavia eviction käytäntöjä: volatile lru, allkeys lru, volatile satunnaisia, allkeys-satunnainen volatile-ttl noeviction. Oletuskäytäntö on volatile lru. Lisätietoja on artikkelissa [Oletus Redis server-määritys](cache-configure.md#default-redis-server-configuration).|
|Vanhenemiskäytännön|Oletus-vanhenemiskäytännön on absoluuttisen ja oletusarvon vanhentumispäivä väli on 10 minuuttia. Liukuva ja ei koskaan ovat käytettävissä.|Oletusarvoisesti välimuistissa olevien kohteiden ei vanhene, mutta vanhentumispäivä voi määrittää käyttämällä välimuistin määrittäminen Osastollasi kohti kirjoittaminen välein. Lisätietoja on artikkelissa [objektien lisääminen ja Nouda välimuistista](cache-dotnet-how-to-use-azure-redis-cache.md#add-and-retrieve-objects-from-the-cache).|
|Alueiden ja tunnisteet|Alueiden ovat aliryhmistä välimuistiin tallennetut kohteet. Alueiden tukevat välimuistiin tallennetut kohteet huomautuksen myös muita kuvaava merkkijonot kutsutaan tunnisteet. Alueiden tukevat voi suorittaa haun toimenpiteet merkityt kohteet alueen. Kaikki kohteet alueella on yksittäinen solmu välimuistin klusterin sijaitsevat.|Redis.txt välimuistin koostuu yksittäinen solmu (paitsi jos Redis.txt klusterin otettu käyttöön) niin hallitun välimuisti-palvelun alueiden käsite ei koske. Redis tukee etsiminen ja yleismerkkien toimintojen noudettaessa näppäimet, jotta kuvaavia tunnisteita voidaan upottaa avaimen nimet ja hakea niitä myöhemmin käyttää. Esimerkki käyttöönoton tunnisteiden avulla Redis.txt ratkaisu on artikkelissa [käyttöönoton välimuistin tunnisteita Redis.txt kanssa](http://stackify.com/implementing-cache-tagging-redis/).|
|Sarjatoiminto|Hallitut välimuistin tukee NetDataContractSerializer BinaryFormatter ja mukautetun serializers käyttö. Oletusarvo on NetDataContractSerializer.|Näin sovelluksen onnistu .NET-objektit ennen sijoittamisen välimuistin ylöspäin asiakkaan sovelluskehittäjän Sarjatoiminto vaihtoehto. Saat lisätietoja ja esimerkkejä koodin [välimuistin .NET objektien käsitteleminen](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).|
| Välimuistin emulaattorin | Hallitut välimuistin on paikallisen välimuistin emulaattorin. | Azure Redis välimuistia ei ole emulaattorin, mutta voi [suorittaa Redis.txt-server.exe paikallisesti MSOpenTech koontiversiota](cache-faq.md#cache-emulator) antamaan emulaattorin-toiminto. |

## <a name="choose-a-cache-offering"></a>Valitse välimuistin tarjoaa

Microsoft Azure Redis välimuistin on käytettävissä seuraavassa tasoa:

-   **Tavallinen** – yksi solmu. Useita koko on enintään 53 Gigatavua.
-   **Vakio** – kahden solmun ensisijainen/replikan. Useita koko on enintään 53 Gigatavua. 99,9 % SLA.
-   **Premium** – kahden solmun ensisijainen/replikaa enintään 10 shards. Useita koot-6 Gigatavua 530 Gigatavua (yhteystiedot Lisää). Kaikkia vakio taso-ominaisuuksia ja [Redis klusterin](cache-how-to-premium-clustering.md), [Redis pysyvyys](cache-how-to-premium-persistence.md)ja [Azure Virtual Network](cache-how-to-premium-vnet.md)mukaan lukien lisäohjeita. 99,9 % SLA.

Kunkin tason eroaa ominaisuudet ja hinnoittelusta. Toiminnot kuuluvat tämän oppaan ja hinnoittelu Lisätietoja on artikkelissa [Välimuistin hinnat tiedot](https://azure.microsoft.com/pricing/details/cache/).

Siirron aloituskohta on Valitse sopiva koko, joka vastaa edellisen hallitun välimuisti-palvelun välimuistin kokoa ja skaalata ylös tai alas sen mukaan, sovelluksen vaatimukset. Lisää ohjeita oikean Azure Redis välimuistin tarjoaa valitsemisesta on artikkelissa [mitä Redis.txt välimuistin tarjoaa ja koko kannattaa käyttää?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## <a name="create-a-cache"></a>Luoda välimuistin

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="configure-the-cache-clients"></a>Välimuistin-asiakkaiden määrittäminen

Kun välimuistin on luotu ja määritetty, seuraava vaihe hallitun välimuisti-palvelun poistaminen ja lisääminen Lisää Azure Redis välimuisti-määritys on ja viittaa niin, että välimuistin asiakkaat voivat käyttää välimuistin.

-   Poista hallitun välimuisti-palvelun määritykset
-   Määrittää välimuistin asiakkaan StackExchange.Redis NuGet pakkaaminen

### <a name="remove-the-managed-cache-service-configuration"></a>Poista hallitun välimuisti-palvelun määritykset

Ennen kuin Azure Redis välimuistin, hallita välimuisti-palvelun määritys voi määrittää asiakassovellusten ja Kokoonpanoviittaukset on poistettava poistamalla hallitun välimuistin palvelun NuGet-paketti.

Poista hallitun välimuisti-palvelun NuGet-paketti, **Ratkaisunhallinnassa** asiakkaan projektin hiiren kakkospainikkeella ja valitse **NuGet pakettien hallinta**. Valitse **asennetut pakettien** -solmu ja kirjoita W**indowsAzure.Caching** asennetuista paketeista hakuruutuun. Valitse **Windows** **Azure-välimuistin** (tai **Windows** **Azure välimuistiin** NuGet-paketin version mukaan), valitse **Poista**ja valitse sitten **Sulje**.

![Poista Azure hallitun välimuistin NuGet palvelupakettiin](./media/cache-migrate-to-redis/IC757666.jpg)

Hallitun välimuistin palvelun NuGet-paketin asennuksen poistaminen poistaa hallitun välimuisti-palvelun kokoonpanon ja hallita välimuisti-palvelun tapahtumat app.config tai asiakassovellus web.config. Koska joitakin mukautettuja asetuksia ei voi poistaa poistettaessa NuGet-paketti, avaa web.config tai app.config ja varmista, että seuraavat osat on poistettu kokonaan.

Varmistaa, että `dataCacheClients` tapahtuma poistetaan `configSections` elementti. Älä poista kokonaan `configSections` elementin; Poista juuri `dataCacheClients` tekstiä, jos se ei sisällä tietoja.

    <configSections>
      <!-- Existing sections omitted for clarity. -->
      <section name="dataCacheClients"type="Microsoft.ApplicationServer.Caching.DataCacheClientsSection, Microsoft.ApplicationServer.Caching.Core" allowLocation="true" allowDefinition="Everywhere"/>
    </configSections>

Varmistaa, että `dataCacheClients` osa poistetaan. `dataCacheClients` Osassa on samankaltainen kuin seuraavassa esimerkissä.

    <dataCacheClients>
      <dataCacheClientname="default">
        <!--To use the in-role flavor of Azure Cache, set identifier to be the cache cluster role name -->
        <!--To use the Azure Managed Cache Service, set identifier to be the endpoint of the cache cluster -->
        <autoDiscoverisEnabled="true"identifier="[Cache role name or Service Endpoint]"/>

        <!--<localCache isEnabled="true" sync="TimeoutBased" objectCount="100000" ttlValue="300" />-->
        <!--Use this section to specify security settings for connecting to your cache. This section is not required if your cache is hosted on a role that is a part of your cloud service. -->
        <!--<securityProperties mode="Message" sslEnabled="true">
          <messageSecurity authorizationInfo="[Authentication Key]" />
        </securityProperties>-->
      </dataCacheClient>
    </dataCacheClients>

Hallitun välimuisti-palvelun määritykset poistetaan, kun voit määrittää välimuistin asiakkaan seuraavassa kohdassa kuvatulla tavalla.

### <a name="configure-a-cache-client-using-the-stackexchangeredis-nuget-package"></a>Määrittää välimuistin asiakkaan StackExchange.Redis NuGet pakkaaminen

[AZURE.INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

## <a name="migrate-managed-cache-service-code"></a>Siirtää välimuistin palvelukoodi hallinnasta

StackExchange.Redis välimuistin asiakkaan Ohjelmointirajapinnan muistuttaa hallitun välimuisti-palvelun. Tässä osassa on yleisiä tietoja eroja.

### <a name="connect-to-the-cache-using-the-connectionmultiplexer-class"></a>Yhteyden muodostaminen välimuistin ConnectionMultiplexer-luokan avulla

Käsitteli hallitun välimuisti-palvelun välimuistiin yhteydet `DataCacheFactory` ja `DataCache` luokat. Hallinnoi Azure Redis välimuistin asiakasyhteydet `ConnectionMultiplexer` luokka.

Lisää seuraava teksti, josta minkä tahansa tiedoston, jota haluat käyttää välimuistin ylös-lauseella.

    using StackExchange.Redis
                                
Jos tämä nimitila ei vastaa, varmista, että olet lisännyt StackExchange.Redis NuGet paketin kuvatulla tavalla [välimuisti-asiakkaiden määrittäminen](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

>[AZURE.NOTE] Huomaa, että StackExchange.Redis Clientin .NET Framework 4 tai uudempi.

Muodosta yhteys Azure Redis välimuistin esiintymä Soita staattisen `ConnectionMultiplexer.Connect` menetelmä ja Välitä päätepiste ja avaimen avulla. Yksi tapa jakaminen `ConnectionMultiplexer` sovelluksen esiintymää on staattinen ominaisuus, joka palauttaa yhdistetyn esiintymän, samalla tavalla kuin seuraavassa esimerkissä. Viestiketjun sopivaa näin alustaa vain yksi yhdistetty `ConnectionMultiplexer` esiintymä. Tässä esimerkissä `abortConnect` on määritetty epätosi, mikä tarkoittaa, että puhelu onnistuu, vaikka välimuistiin yhteys muodostetaan ei. Yksi tärkeimmistä ominaisuus `ConnectionMultiplexer` on, että se palauttaa yhteyden automaattisesti välimuistiin kerran verkko-ongelma tai muista syistä selvitetään.

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

Välimuistin päätepisteen, avaimet ja portit saadaan välimuistin esiintymän **Redis välimuisti** -sivu. Lisätietoja on artikkelissa [Redis välimuistin ominaisuudet](cache-configure.md#properties).

Kun yhteys on muodostettu, palauttaa viittauksen Redis.txt välimuistin tietokannan soittamalla `ConnectionMultiplexer.GetDatabase` menetelmää. Palauttaa objektin `GetDatabase` menetelmä on kevyt läpivienti-objekti ja ei tarvitse tallennetaan.

    IDatabase cache = Connection.GetDatabase();
    
    // Perform cache operations using the cache object...
    // Simple put of integral data types into the cache
    cache.StringSet("key1", "value");
    cache.StringSet("key2", 25);
    
    // Simple get of data types from the cache
    string key1 = cache.StringGet("key1");
    int key2 = (int)cache.StringGet("key2");

StackExchange.Redis-asiakasohjelman käyttöön `RedisKey` ja `RedisValue` tyypit, käyttämiseen ja säilytetään välimuistissa. Tällaisia sivulle eniten kielen primitiivityyppejä, mukaan lukien merkkijono, määrittäminen ja usein ei käytetä suoraan. Redis merkkijonot on yleisin Redis.txt arvon tyyppi ja voivat sisältää erilaisia tietotyyppejä, mukaan lukien sarjoitettu binaarinen virtaa ja tyyppi ei saa käyttää suoraan, kun käytät menetelmiä, jotka sisältävät `String` nimessä. Useimmat alkeistyyppi tietotyyppien tallentaa ja noutaa kohteita välimuistin avulla `StringSet` ja `StringGet` tavoista, paitsi, jos tallennat välimuistin sivustokokoelmia tai muita Redis.txt tietotyyppejä. 

`StringSet`ja `StringGet` hallitun välimuisti-palvelun muistuttavat `Put` ja `Get` menetelmiä yksi pää erotus on, että ennen kuin määrität ja Hae .NET-objektin välimuistiin on onnistu se ensin. 

Soitettaessa `StringGet`, jos objekti on luotu, se palautetaan ja jos se ei ole null palautetaan. Tässä tapauksessa voit hakea arvon haluamasi tietolähteestä ja tallentaa sen myöhempää käyttöä varten välimuistin. Tätä kutsutaan välimuistin kesantoala kuvio.

Voit määrittää kohteen voimassaolon välimuistin `TimeSpan` parametri `StringSet`.

    cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));

Azure Redis välimuistin käsitellä .NET objektit sekä alkeistyyppi tietotyyppejä, mutta ennen kuin .NET-objektin voi olla välimuistiin se on voi muuntaa sarjaksi. Tämä on sovelluksen kehittäjä on vastuussa. Näin developer joustavuutta Sarjatoiminto valinta. Saat lisätietoja ja esimerkkejä koodin [välimuistin .NET objektien käsitteleminen](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).

## <a name="migrate-aspnet-session-state-and-output-caching-to-azure-redis-cache"></a>ASP.NET-istunnon tila ja tulostusvälimuistin Azure Redis välimuistiin

Azure Redis välimuistin on tarjoajat ASP.NET-istunnon tila ja välimuistiin tulostukseen. Voit siirtää sovellusta, joka käyttää hallitun välimuisti-palvelun versiot näistä palveluista, ensin poistettava aiemmin osien oman web.config ja määrittämällä sitten tarjoajien Azure Redis välimuisti-versioissa. Ohjeita Azure Redis välimuistin ASP.NET-palveluntarjoajat käyttämisestä on artikkelissa [Azure Redis välimuistin ASP.NET istunnon tila-palvelu](cache-aspnet-session-state-provider.md) ja [ASP.NET tulosteen välimuistin Provider for Azure Redis välimuistin](cache-aspnet-output-cache-provider.md).

## <a name="next-steps"></a>Seuraavat vaiheet

Tutustu [Azure Redis välimuistin asiakirjat](https://azure.microsoft.com/documentation/services/cache/) , opetusohjelmat, näytteiden tai videoita.

