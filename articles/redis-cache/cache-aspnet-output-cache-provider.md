<properties
    pageTitle="Välimuistin ASP.NET tulosteen välimuistin toimittaja"
    description="Lue, miten voit tallentaa ASP.NET-sivun tulosteen käyttämällä Azure Redis välimuistin"
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
    ms.date="09/27/2016"
    ms.author="sdanie" />

# <a name="aspnet-output-cache-provider-for-azure-redis-cache"></a>ASP.NET-tulostus välimuisti-palvelu Azure Redis välimuisti

Redis tulosteen välimuisti-palvelu on prosessi tallennustilan-järjestelmä tulosteen välimuistin tiedoille. Tämä tieto koskee erityisesti koko HTTP-vastauksia (sivu tulostusvälimuistin). Palvelun kytketään uusi tulosteen välimuistin tarjoajan laajennettavuus hiiren osoitin kohtaan, otettiin käyttöön ASP.NET 4.

Redis tulosteen välimuisti-palveluntarjoajan käyttäminen, määritä ensin välimuistin ja määrittämällä sitten ASP.NET-sovelluksesi Redis tulosteen välimuistin tarjoajan NuGet pakkaaminen. Tämä artikkeli sisältää ohjeet sovelluksesi Redis tulosteen välimuisti-palveluntarjoajan käyttäminen määrittämisestä. Lisätietoja luominen ja määrittäminen Azure Redis välimuistin esiintymä artikkelissa [välimuistin luominen](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).

## <a name="store-aspnet-page-output-in-the-cache"></a>ASP.NET-sivun tulosteen tallennetaan välimuisti

Määritä asiakassovellus Redis tulosteen välimuistin tarjoajan NuGet pakkaaminen Visual Studiossa, napsauta **Ratkaisunhallinnassa** projektin hiiren kakkospainikkeella ja valitse **NuGet pakettien hallinta**.

![Azure Redis.txt välimuistin NuGet pakettien hallinta](./media/cache-aspnet-output-cache-provider/redis-cache-manage-nuget-menu.png)

Kirjoita hakuruutuun **RedisOutputCacheProvider** , valitse se tulokset ja valitse **Asenna**.

![Azure Redis välimuistin tulosteen välimuistin toimittaja](./media/cache-aspnet-output-cache-provider/redis-cache-page-output-provider.png)

Redis tulostus välimuistin tarjoajan NuGet paketin riippuvuuden StackExchange.Redis.StrongName-paketti. Jos StackExchange.Redis.StrongName paketin ei ole projektin se asennetaan. Huomaa, että vahva nimeltä StackExchange.Redis.StrongName pakkaus on myös StackExchange.Redis ei-strong-niminen versio. Jos projektisi on ei-strong-niminen-StackExchange.Redis versiota, se on poistettava ennen tai asennettuasi Redis tulosteen välimuistin tarjoajan NuGet-paketti, muuten sinun Hae nimeäminen ristiriidassa projektin. Saat lisätietoja näiden pakettien [määrittäminen .NET välimuistin asiakkaat](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

NuGet-paketin lataaminen ja lisää tarvittavaa Kokoonpanoviittaukset ja lisää seuraavassa osassa web.config-tiedostoon, joka sisältää vaadittu ASP.NET-sovelluksen käyttämään Redis tulosteen välimuisti-palvelun.

    <caching>
      <outputCachedefault Provider="MyRedisOutputCache">
        <providers>
          <!--
          <add name="MyRedisOutputCache"
            host = "127.0.0.1" [String]
            port = "" [number]
            accessKey = "" [String]
            ssl = "false" [true|false]
            databaseId = "0" [number]
            applicationName = "" [String]
            connectionTimeoutInMilliseconds = "5000" [number]
            operationTimeoutInMilliseconds = "5000" [number]
          />
          -->
          <add name="MyRedisOutputCache" type="Microsoft.Web.Redis.RedisOutputCacheProvider" host="127.0.0.1" accessKey="" ssl="false"/>
        </providers>
      </outputCache>
    </caching>

Kommentin sisältävän osassa on esimerkki määritteet ja kunkin määritteen otoksen asetuksia.

Määritä määritteet välimuisti-sivu Microsoft Azure-portaalissa arvot ja määritä muut arvot haluamallasi tavalla. Saat ohjeita välimuisti-ominaisuuksien [määrittäminen Redis välimuistiasetukset](cache-configure.md#configure-redis-cache-settings).

-   **Host (isäntä)** – Määritä välimuistin päätepiste.
-   **portti** – Käytä-SSL portin tai SSL portti ssl-asetusten mukaan.
-   **accessKey** – Käytä välimuistin joko ensisijaisen tai toissijaisen-näppäintä.
-   **ssl** – TOSI, jos haluat suojata välimuistin/asiakkaan kanssa ssl; Muutoin EPÄTOSI. Muista määrittää portin.
    -   SSL portin on poissa käytöstä oletusarvoisesti uusi tallentaa. Määritä tämä asetus käyttää SSL-porttia tosi. Lisätietoja ottaminen käyttöön-SSL portin [määrittäminen välimuistin](cache-configure.md) aiheen kohdassa [Access-portit](cache-configure.md#access-ports) .
-   **databaseId** – määritetty välimuistin tulosteen tietojen käytettävä tietokanta. Jos ei ole määritetty, käytetään oletusarvo 0.
-   **applicationName** – näppäimet on tallennettu Redis.txt <AppName>_<SessionId>_Data. Ottaa käyttöön useita sovellusten kesken samaa avainta. Tämä parametri on valinnainen, ja jos se ei tarjoa oletusarvo on käytössä.
-   **connectionTimeoutInMilliseconds** – tämän asetuksen avulla voit ohittaa StackExchange.Redis asiakkaan connectTimeout-asetusta. Jos ei ole määritetty, käytetään 5000 connectTimeout oletusasetusta. Lisätietoja on artikkelissa [StackExchange.Redis määritysten malli](http://go.microsoft.com/fwlink/?LinkId=398705).
-   **operationTimeoutInMilliseconds** – tämän asetuksen avulla voit ohittaa StackExchange.Redis asiakkaan syncTimeout-asetusta. Jos ei ole määritetty, käytetään syncTimeout oletusarvo on 1 000. Lisätietoja on artikkelissa [StackExchange.Redis määritysten mallia](http://go.microsoft.com/fwlink/?LinkId=398705).

Lisää OutputCache-direktiivissä jokaiselle sivulle, jonka haluat tallentaa tulos.

    <%@ OutputCache Duration="60" VaryByParam="*" %>

Tässä esimerkissä sivun välimuistiin tallennetut tiedot pysyvät välimuistin 60 sekunnin ajan ja kunkin parametrin yhdistelmän välimuistiin sivun eri versiota. Saat lisätietoja OutputCache-direktiivissä [@OutputCache](http://go.microsoft.com/fwlink/?linkid=320837).

Nämä vaiheet suoritetaan, kun sovellus on määritetty käyttämään Redis tulosteen välimuisti-palvelun.

## <a name="next-steps"></a>Seuraavat vaiheet

Tutustu [ASP.NET-istunnon tilan palvelu Azure Redis välimuistin](cache-aspnet-session-state-provider.md).
