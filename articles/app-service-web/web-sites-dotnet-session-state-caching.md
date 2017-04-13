<properties 
    pageTitle="Istunnon tila Azure Redis välimuistin Azure sovelluksen-palvelun kanssa" 
    description="Opettele käyttämään ASP.NET istunnon tilan välimuistiin tukemaan Azure välimuisti-palvelun." 
    services="app-service\web" 
    documentationCenter=".net" 
    authors="Rick-Anderson" 
    manager="wpickett" 
    editor="none"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="get-started-article" 
    ms.date="06/27/2016" 
    ms.author="riande"/>


# <a name="session-state-with-azure-redis-cache-in-azure-app-service"></a>Istunnon tila Azure Redis välimuistin Azure sovelluksen-palvelun kanssa


Tässä ohjeaiheessa kerrotaan, miten voit käyttää Azure Redis välimuisti-palvelun istunnon tila.

ASP.NET web-sovelluksen käyttää istunnon tila, jos haluat määrittää ulkoisen istunnon-tilan tarjoajan (Redis välimuisti-palvelun tai SQL Server-istunnon tila-palvelua). Jos käytät istunnon tila ja Älä käytä ulkoisen palveluntarjoajan, voi rajoitettu koodiin yhdessä paikassa. Redis välimuisti-palvelun on nopein ja yksinkertaisin käyttöön.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

##<a id="createcache"></a>Luo välimuisti
Noudata [seuraavia ohjeita](../cache-dotnet-how-to-use-azure-redis-cache.md#create-cache) voit luoda välimuistin.

##<a id="configureproject"></a>Lisää RedisSessionStateProvider NuGet pakkaaminen web App-sovellukseen
Asenna NuGet `RedisSessionStateProvider` paketti.  Käytä seuraavaa komentoa Asenna paketti hallinta-konsolin (**Työkalut** > **NuGet pakettien hallinta** > **Paketin hallinta-konsolin**):

  `PM> Install-Package Microsoft.Web.RedisSessionStateProvider`
  
Asenna **Työkalut** > **NuGet pakettien hallinta** > **Ratkaisu NugGet pakettien hallinta**, Etsi `RedisSessionStateProvider`.

Katso lisätietoja [NuGet RedisSessionStateProvider sivulle](http://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider/ ) ja [Määritä välimuistin asiakas](../cache-dotnet-how-to-use-azure-redis-cache.md#NuGet).

##<a id="configurewebconfig"></a>Seuraavan koodin korostetut muokkaaminen
Lisäksi tehdä Kokoonpanoviittaukset välimuistin, NuGet paketin Lisää *seuraavan koodin korostetut* kanta tapahtumat. 

1. Avaa *web.config* ja Etsi **sessionState** elementti.

1. Kirjoita arvot `host`, `accessKey`, `port` (SSL-portti pitäisi olla 6380), ja määritä `SSL` , `true`. Nämä arvot saadaan välimuistin esiintymän [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715) -sivu. Lisätietoja on artikkelissa [etäyhteyden muodostaminen välimuistissa](../cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-cache). Huomaa, että-SSL portti ei ole käytössä oletusarvoisesti uusi tallentaa. Lisätietoja ottaminen käyttöön-SSL portin [määrittäminen välimuistin Azure Redis välimuistin](https://msdn.microsoft.com/library/azure/dn793612.aspx) aiheen kohdassa [Access-portit](https://msdn.microsoft.com/library/azure/dn793612.aspx#AccessPorts) . Seuraavat merkinnät-näkymässä näkyvät *seuraavan koodin korostetut, erityisesti *portin*, *host*, accessKey muutokset* muutokset*, ja *ssl *.

          <system.web>;
            <customErrors mode="Off" />;
            <authentication mode="None" />;
            <compilation debug="true" targetFramework="4.5" />;
            <httpRuntime targetFramework="4.5" />;
            <sessionState mode="Custom" customProvider="RedisSessionProvider">;
              <providers>;  
                  <!--<add name="RedisSessionProvider" 
                    host = "127.0.0.1" [String]
                    port = "" [number]
                    accessKey = "" [String]
                    ssl = "false" [true|false]
                    throwOnError = "true" [true|false]
                    retryTimeoutInMilliseconds = "0" [number]
                    databaseId = "0" [number]
                    applicationName = "" [String]
                  />;-->;
                 <add name="RedisSessionProvider" 
                      type="Microsoft.Web.Redis.RedisSessionStateProvider" 
                      port="6380"
                      host="movie2.redis.cache.windows.net" 
                      accessKey="m7PNV60CrvKpLqMUxosC3dSe6kx9nQ6jP5del8TmADk=" 
                      ssl="true" />;
              <!--<add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="127.0.0.1" accessKey="" ssl="false" />;-->;
              </providers>;
            </sessionState>;
          </system.web>;


##<a id="usesessionobject"></a>Tallentaa Session koodissa
Viimeinen vaihe on käytössä objektiin ASP.NET-koodi. Objektien lisääminen istunnon tila **Session.Add** -menetelmällä. Tässä menetelmässä avain-arvo-pareina istunnon tilan välimuistin kohteiden tallentamiseen.

    string strValue = "yourvalue";
    Session.Add("yourkey", strValue);

Seuraava koodi hakee arvon istunnon tila.

    object objValue = Session["yourkey"];
    if (objValue != null)
       strValue = (string)objValue; 

Voit käyttää myös Redis välimuistin välimuistin objekteihin-sovellukseen. Saat lisätietoja, [MVC elokuva-sovelluksessa, jossa Azure Redis välimuistin 15 minuuttia](https://azure.microsoft.com/blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/).
Lisätietoja ASP.NET-istunnon tila käyttämisestä on artikkelissa [ASP.NET istunnon tilan yleiskatsaus][].

>[AZURE.NOTE] Jos haluat aloittaa Azure App palvelun ennen rekisteröimässä Azure-tili, siirry [Yritä App palvelu](http://go.microsoft.com/fwlink/?LinkId=523751), jossa lyhytkestoinen starter verkkosovellukseen heti voit luoda sovelluksen-palvelussa. Ei ole pakollinen; luottokortit ei ole sitoumukset.

## <a name="whats-changed"></a>Mikä on muuttunut
* Katso muutoksen opas verkkosivuilta App palveluun: [Azure App palvelu ja sen vaikutus aiemmin Azure-palvelut](http://go.microsoft.com/fwlink/?LinkId=529714)

  *[Luokiteltavaa henkilöä hiiren kakkospainikkeella Anderson](https://twitter.com/RickAndMSFT) mukaan*
  
  [installed the latest]: http://www.windowsazure.com/downloads/?sdk=net  
  [ASP.NET-istunnon tila yleiskatsaus]: http://msdn.microsoft.com/library/ms178581.aspx

  [NewIcon]: ./media/web-sites-dotnet-session-state-caching/CacheScreenshot_NewButton.png
  [NewCacheDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CreateOptions.png
  [CacheIcon]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheIcon.png
  [NuGetDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_NuGet.png
  [OutputConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_OC_WebConfig.png
  [CacheConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheConfig.png
  [EndpointURL]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_EndpointURL.png
  [ManageKeys]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_ManageAccessKeys.png
 
