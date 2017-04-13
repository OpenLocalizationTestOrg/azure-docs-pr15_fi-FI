<properties
    pageTitle="Välimuistin ASP.NET-istunnon tila palvelun | Microsoft Azure"
    description="Lue, miten voit tallentaa käyttämällä Azure Redis välimuistin ASP.NET-istunnon tila"
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
    ms.date="09/01/2016"
    ms.author="sdanie" />

# <a name="aspnet-session-state-provider-for-azure-redis-cache"></a>ASP.NET-istunnon tila-palvelu Azure Redis välimuisti

Azure Redis välimuistin on istunnon tilan palveluntarjoaja, joiden avulla voit tallentaa istunnon tilan sen sijaan, että ladatun välimuistiin tai SQL Server-tietokantaan. Välimuistiin istunnon tila-palveluntarjoajan käyttäminen, määritä ensin välimuistin ja määrittää ASP.NET-sovelluksen välimuistin Redis välimuistin istunnon tilan NuGet pakkaaminen.

Se ei ole usein käytännön tosielämän cloud-sovelluksessa voit välttää tallentamiseen joitakin lomakkeen valtion käyttäjän istuntoon, mutta jotkin tavoista vaikuttaa suorituskyky ja skaalattavuus enemmän kuin muiden. Jos sinun pitää tallentaa tilan paras mahdollinen ratkaisu on säilyttää tilan määrää pieni ja tallentaa sen evästeitä. Jos, joka ei ole mahdollista, seuraava paras mahdollinen ratkaisu on käyttää ASP.NET-istunnon tila tarjoajan kanssa eri aikajaksoille, ladatun välimuistin. Suorituskyky ja skaalattavuus kannalta huonoin ratkaisu on käytettävä tietokannan palautettua istunnon tila palvelun. Tämä artikkeli sisältää ohjeet, Azure Redis välimuistin ASP.NET istunnon tila-palvelun avulla. Lisätietoja muiden istunnon tila-asetukset on artikkelissa [ASP.NET-istunnon tila-asetukset](#aspnet-session-state-options).

## <a name="store-aspnet-session-state-in-the-cache"></a>ASP.NET-istunnon tila tallennetaan välimuisti

Määritä asiakassovellus Redis välimuistin istunnon tilan NuGet pakkaaminen Visual Studiossa, napsauta **Ratkaisunhallinnassa** projektin hiiren kakkospainikkeella ja valitse **NuGet pakettien hallinta**.

![Azure Redis.txt välimuistin NuGet pakettien hallinta](./media/cache-aspnet-session-state-provider/redis-cache-manage-nuget-menu.png)

Kirjoita hakuruutuun **RedisSessionStateProvider** , valitse se tulokset ja valitse **Asenna**.

>[AZURE.IMPORTANT] Jos käytössäsi klusteroinnin premium taso-toimintoa, sinun on käytettävä [RedisSessionStateProvider](https://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider) 2.0.1 tai suurempi tai poikkeus ilmenee. Tämä on jakautumisen; muutos Katso lisätietoja [v2.0.0 purkaa muuttaa tietoja](https://github.com/Azure/aspnet-redis-providers/wiki/v2.0.0-Breaking-Change-Details).

![Azure Redis.txt välimuistin istunnon tila-palvelu](./media/cache-aspnet-session-state-provider/redis-cache-session-state-provider.png)

Redis istunnon tilan tarjoajan NuGet paketin riippuvuuden StackExchange.Redis.StrongName-paketti. Jos StackExchange.Redis.StrongName paketin ei ole projektin se asennetaan. Huomaa, että vahva nimeltä StackExchange.Redis.StrongName pakkaus on myös StackExchange.Redis ei-strong-niminen versio. Jos projektisi on ei-strong-niminen-StackExchange.Redis versiota, se on poistettava ennen tai asennettuasi Redis istunnon tilan tarjoajan NuGet-paketti, muuten sinun Hae nimeäminen ristiriidassa projektin. Saat lisätietoja näiden pakettien [määrittäminen .NET välimuistin asiakkaat](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

NuGet-paketin lataaminen ja lisää tarvittavaa kokoonpanoa viittaa ja lisää seuraavat Lisää seuraavan osan web.config-tiedostoon, joka sisältää ASP.NET-sovelluksen käyttämään Redis välimuistin istunnon tilan tarjoajan vaadittu.

    <sessionState mode="Custom" customProvider="MySessionStateStore">
        <providers>
        <!--
        <add name="MySessionStateStore"
            host = "127.0.0.1" [String]
            port = "" [number]
            accessKey = "" [String]
            ssl = "false" [true|false]
            throwOnError = "true" [true|false]
            retryTimeoutInMilliseconds = "0" [number]
            databaseId = "0" [number]
            applicationName = "" [String]
            connectionTimeoutInMilliseconds = "5000" [number]
            operationTimeoutInMilliseconds = "5000" [number]
        />
        -->
        <add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="127.0.0.1" accessKey="" ssl="false"/>
        </providers>
    </sessionState>

Kommentin sisältävän osassa on esimerkki määritteet ja kunkin määritteen otoksen asetuksia.

Määritä määritteet välimuisti-sivu Microsoft Azure-portaalissa arvot ja määritä muut arvot haluamallasi tavalla. Saat ohjeita välimuisti-ominaisuuksien [määrittäminen Redis välimuistiasetukset](cache-configure.md#configure-redis-cache-settings).

-   **Host (isäntä)** – Määritä välimuistin päätepiste.
-   **portti** – Käytä-SSL portin tai SSL portti ssl-asetusten mukaan.
-   **accessKey** – Käytä välimuistin joko ensisijaisen tai toissijaisen-näppäintä.
-   **ssl** – TOSI, jos haluat suojata välimuistin/asiakkaan kanssa ssl; Muutoin EPÄTOSI. Muista määrittää portin.
    -   SSL portin on poissa käytöstä oletusarvoisesti uusi tallentaa. Määritä tämä asetus käyttää SSL-porttia tosi. Lisätietoja ottaminen käyttöön-SSL portin [määrittäminen välimuistin](cache-configure.md) aiheen kohdassa [Access-portit](cache-configure.md#access-ports) .
-   **throwOnError** – TOSI, jos haluat, että poikkeuksen ilmenee virhe tai EPÄTOSI, jos haluat, että toiminto ei onnistu äänettömästi. Voit tarkistaa virhe valitsemalla staattinen Microsoft.Web.Redis.RedisSessionStateProvider.LastException-ominaisuus. Oletusarvo on TOSI.
-   **retryTimeoutInMilliseconds** – toiminnot, jotka eivät läpäise yritetään uudelleen aikana tällä aikavälillä millisekunteina. Ensimmäinen uudelleen toteutuu 20 millisekuntia jälkeen ja sitten uudelleenyritykset ilmetä sekunnin välein, kunnes retryTimeoutInMilliseconds välin vanhenee. Heti, kun tällä aikavälillä toiminnon yritetään lopullinen kerran. Jos edelleen epäonnistuu, poikkeus ilmenee takaisin soittajalle throwOnError koskevien mukaan. Oletusarvo on 0, mikä tarkoittaa sitä ei ole uudelleenyritykset.
-   **databaseId** – määrittää tietokannan käytettävät välimuistiin tulostaa tiedot. Jos ei ole määritetty, käytetään oletusarvo 0.
-   **applicationName** – näppäimet on tallennettu Redis.txt `{<Application Name>_<Session ID>}_Data`. Ottaa käyttöön useita sovellusten kesken samaa avainta. Tämä parametri on valinnainen, ja jos se ei tarjoa oletusarvo on käytössä.
-   **connectionTimeoutInMilliseconds** – tämän asetuksen avulla voit ohittaa StackExchange.Redis asiakkaan connectTimeout-asetusta. Jos ei ole määritetty, käytetään 5000 connectTimeout oletusasetusta. Lisätietoja on artikkelissa [StackExchange.Redis määritysten malli](http://go.microsoft.com/fwlink/?LinkId=398705).
-   **operationTimeoutInMilliseconds** – tämän asetuksen avulla voit ohittaa StackExchange.Redis asiakkaan syncTimeout-asetusta. Jos ei ole määritetty, käytetään syncTimeout oletusarvo on 1 000. Lisätietoja on artikkelissa [StackExchange.Redis määritysten malli](http://go.microsoft.com/fwlink/?LinkId=398705).

Saat lisätietoja näiden ominaisuuksien alkuperäisen blogin kirjaa ilmoituksen palvelussa [julkaisu ASP.NET istunnon tilan Redis.txt varten](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx).

Älä unohda ulos vakio InProc istunnon tila palvelun kohdassa oman web.config kommentti.

    <!-- <sessionState mode="InProc"
         customProvider="DefaultSessionProvider">
         <providers>
            <add name="DefaultSessionProvider"
                  type="System.Web.Providers.DefaultSessionStateProvider,
                        System.Web.Providers, Version=1.0.0.0, Culture=neutral,
                        PublicKeyToken=31bf3856ad364e35"
                  connectionStringName="DefaultConnection" />
          </providers>
    </sessionState> -->

Nämä vaiheet suoritetaan, kun sovellus on määritetty käyttämään Redis välimuistin istunnon tilan toimittaja. Kun käytät istunnon tila-sovelluksessa, se tallennetaan Azure Redis välimuistin esiintymän.

>[AZURE.NOTE] Huomaa, että välimuistiin tallennetut tiedot on oltava jaksotettavissa, toisin kuin tiedot, jotka voidaan tallentaa oletusarvo ladatun ASP.NET-istunto State toimittaja. Kun Redis.txt istunnon tila-palvelu on käyttänyt, muista tietotyyppejä, jotka tallennetaan istunnon tila on voi sarjoittaa.

## <a name="aspnet-session-state-options"></a>ASP.NET-istunnon tila-asetukset

- Muistin istunnon tilaa palvelu - palvelu tallentaa istunnon tila muistiin. Tämän palvelun avulla etuna on se on helppoa ja nopeaa. Ei voi kuitenkin skaalata Web Apps-sovellusten, jos muisti-palvelu on käyttäminen, koska se on ei jaettu.

- SQL Server-istunnon tila palvelun - palvelu tallentaa istunnon tila Sql Server. Tämä palvelu pitäisi käyttää, jos haluat säilyttää pysyvän säilön istunnon-tila. Voi skaalata Web Appissa, mutta Sql Serverin avulla istunnon on suorituskykyyn vaikuttavia Web Appissa.

- Jaettu muistin istunnon tilan tarjoajan esimerkiksi Redis välimuistin istunnon tilan Provider - palvelun avulla voit sekä maailman paras. Web-sovellus voi olla yksinkertainen, nopea ja skaalattava istunnon tilan palveluntarjoaja. Sinun täytyy kannattaa ottaa huomioon, että tämä palvelu tallentaa istunnon tila välimuistin ja sovelluksen on otettava huomioon on liitetty kun puhelu jaettu-välimuistissa kuten lyhytkestoisia verkon virheiden ominaisuudet. Katso toimintaohjeita välimuistin [välimuisti ohjeita](../best-practices-caching.md) Microsoft Patterns ja käytäntöjen [Azure Cloud sovelluksen suunnittelu ja käyttöönotto ohjeita](https://github.com/mspnp/azure-guidance).

Saat lisätietoja istunnon tila ja muista parhaista käytännöistä [Web kehittäminen parhaita käytäntöjä (rakennuksen tosielämän Cloud sovellukset Azure kanssa)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices).

## <a name="next-steps"></a>Seuraavat vaiheet

Tutustu [ASP.NET-tulostus välimuistin palvelu Azure Redis välimuistin](cache-aspnet-output-cache-provider.md).
