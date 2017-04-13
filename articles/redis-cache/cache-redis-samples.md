<properties 
    pageTitle="Azure Redis välimuistin näytteiden | Microsoft Azure" 
    description="Opettele käyttämään Azure Redis välimuisti" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="08/30/2016" 
    ms.author="sdanie"/>

# <a name="azure-redis-cache-samples"></a>Azure Redis välimuisti-objektit 

Tämä artikkeli sisältää Azure Redis välimuistin näytteiden skenaariot, kuten välimuistin, lukeminen ja kirjoittaminen tiedot ja välimuistista yhteyden ja käyttäviä yhteyshenkilöitä ASP.NET Redis välimuistin kattavan luettelon. Jotkin mallit ovat ladattavan projekteja ja joitakin ohjeet ja Sisällytä koodikatkelmat, mutta ei linkitä ladattavan projektin.

## <a name="hello-world-samples"></a>Hei maailma objektit

Tässä osassa mallit Näytä erillisen Azure Redis välimuistin muodostaminen ja lukeminen ja kirjoittaminen käyttämällä eri kielten välimuistin ja Redis asiakkaat.

[Hei](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) otosten esitetään, kuinka voit suorittaa eri välimuistin [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) .NET-asiakas.

Tässä esimerkissä Toimintaohje:

-   Eri yhteyden asetukset
-   Lukeminen ja kirjoittaminen objektit ja sieltä pois välimuistin synkronoidusti ja asynkroninen toimintojen käyttäminen
-   Palauttaa määritetyn näppäimet arvoja Redis MGET/MSET komentojen avulla
-   Suorittaa Redis.txt tapahtumien
-   Redis.txt luettelot ja lajiteltuja joukkojen käyttäminen
-   Tallentaa JsonConvert serializers käyttämällä .NET-objektit
-   Redis.txt joukot avulla voit toteuttaa tunnisteet
-   Redis.txt klusterin käsitteleminen

Lisätietoja on ohjeissa [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) github ja muita käyttötapoja kohdassa [StackExchange.Redis.Tests](https://github.com/StackExchange/StackExchange.Redis/tree/master/StackExchange.Redis.Tests) yksikön testejä.

[Käyttämisestä Azure Redis välimuistin kanssa Python](cache-python-get-started.md) näyttää, miten Azure Redis välimuistin käytön aloittaminen Python ja [Redis.txt kopioi](https://github.com/andymccurdy/redis-py) -asiakasohjelman avulla.

[Välimuistin .NET objektien käsitteleminen](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache) on yksi tapa onnistu .NET-objektit, jotta voit kirjoittaa ne ja lukea Azure Redis välimuistin esiintymä. 

## <a name="use-redis-cache-as-a-scale-out-backplane-for-aspnet-signalr"></a>Käyttää Redis välimuistin mittakaava, backplane-liitäntä ASP.NET SignalR

[Käyttää Redis välimuistin mittakaava, backplane-liitäntä ASP.NET SignalR varten](https://github.com/rustd/RedisSamples/tree/master/RedisAsSignalRBackplane) -esimerkissä esitetään, miten voit käyttää Azure Redis välimuistin SignalR backplane-liitäntä. Saat lisätietoja backplane-liitäntä [SignalR Scaleout Redis.txt kanssa](http://www.asp.net/signalr/overview/performance/scaleout-with-redis).

## <a name="redis-cache-customer-query-sample"></a>Redis välimuistin asiakkaan kyselyn Esimerkki

Tässä esimerkissä vertaa suorituskyvyn tietojen käyttäminen välimuistista ja tietojen käyttäminen pysyvyyttä tallennustilan välillä. Tässä esimerkissä on kaksi projektia.

-   [Miten Redis välimuistin voi parantaa suorituskykyä tallentamalla tiedot välimuistiin esittely](https://github.com/rustd/RedisSamples/tree/master/RedisCacheCustomerQuerySample)
-   [Määritä tietokannan ja välimuistin varten esittely](https://github.com/rustd/RedisSamples/tree/master/SeedCacheForCustomerQuerySample)

## <a name="aspnet-session-state-and-output-caching"></a>ASP.NET-istunnon tila ja uudet ominaisuudet

[Käytä Azure Redis välimuistin ASP.NET SessionState ja OutputCache](https://github.com/rustd/RedisSamples/tree/master/SessionState_OutputCaching) esimerkissä siitä, miten voit tallentaa ASP.NET-istunnon ja käyttämällä SessionState ja OutputCache tarjoajien Redis.txt tulostusvälimuistin Azure Redis välimuistin avulla.

## <a name="manage-azure-redis-cache-with-maml"></a>Hallitse Azure Redis.txt välimuistin MAML kanssa

[Azure hallinta kirjastojen avulla hallita Azure Redis välimuisti](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) -esimerkissä esitetään, miten voit käyttää Azure hallinta kirjastojen hallinta - (Luo / Päivitä / Poista) välimuistin. 

## <a name="custom-monitoring-sample"></a>Mukautettu seuranta malli

[Data Access Redis välimuistin seuranta](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) -esimerkissä kuvataan siitä, miten voit käyttää seurantatiedot Azure Redis välimuistin Azure-portaalin ulkopuolella.

## <a name="a-twitter-style-clone-written-using-php-and-redis"></a>Twitter-tyyli-Kloonaa kirjoitettu PHP ja Redis.txt

[Retwis](https://github.com/SyntaxC4-MSFT/retwis) malli on Redis Hei maailmaa. Kyseessä mahdollisimman vähän Twitter-tyyppiseksi sosiaalisen verkoston Kloonaa kirjoitettu Redis.txt ja PHP [Predis](https://github.com/nrk/predis) -asiakas. Lähdekoodin on tarkoitus olla erittäin yksinkertaisia ja samanaikaisesti näyttämään eri Redis rakenteet.

## <a name="bandwidth-monitor"></a>Kaistanleveyden näyttö

[Kaistanleveyden näyttö](https://github.com/JonCole/SampleCode/tree/master/BandWidthMonitor) -malli voit valvoa käytetään asiakaskoneeseen kaistanleveyden. Mittaa kaistanleveyden, suorita otosten välimuistin asiakaskoneeseen, puhelujen soittamiseen välimuistiin ja noudata ilmoittaa kaistanleveyden näytön otosten kaistanleveyden.
