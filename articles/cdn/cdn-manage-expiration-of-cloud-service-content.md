<properties
 pageTitle="Vanhenemisen Azure Web Apps-/ pilvipalveluihin ASP.NET ja IIS sisällön Azure CDN hallinnasta | Microsoft Azure"
 description="Kerrotaan, miten voit hallita Azure CDN cloud palvelun sisällön vanheneminen"
 services="cdn"
 documentationCenter=".NET"
 authors="camsoper"
 manager="erikre"
 editor=""/>
<tags
 ms.service="cdn"
 ms.workload="media"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="09/19/2016"
 ms.author="casoper"/>

# <a name="how-to-manage-expiration-of-azure-web-appscloud-services-aspnet-or-iis-content-in-azure-cdn"></a>Voimassaolon Azure CDN Azure Web Apps-/ pilvipalveluihin, ASP.NET tai IIS sisällön hallinta

> [AZURE.SELECTOR]
- [Azure Web Apps-/ pilvipalveluihin, ASP.NET tai IIS](cdn-manage-expiration-of-cloud-service-content.md)
- [Azure Blob-objektien tallennuspalvelu](cdn-manage-expiration-of-blob-content.md)

Kaikkien käytettävissä origin minkä tahansa verkkopalvelin-tiedostoja voidaan välimuistiin Azure CDN vasta sen--elinaika (TTL) kuluu.  TTL määräytyy alkuperäisestä palvelimesta HTTP-vastaus [ *Välimuistin hallinnan* otsikko](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) .  Tässä artikkelissa kerrotaan, miten voit määrittää `Cache-Control` otsikoiden Azure Web Apps-sovelluksista, Azure pilvipalveluihin, ASP.NET-sovelluksia ja Internet Information Services-sivustoja, mikä on määritetty vastaavasti.

>[AZURE.TIP] Voit halutessasi määrittää ei TTL tiedostoa.  Tässä tapauksessa Azure CDN koskee automaattisesti oletusallekirjoitusta TTL seitsemän päivän.
>
>Lisätietoja Azure CDN osaa voidaan käyttää nopeuttaa tiedostoja ja muita resursseja on artikkelissa [Azure CDN yleiskatsaus](./cdn-overview.md).

## <a name="setting-cache-control-headers-in-configuration"></a>Määritysten asetus välimuisti-komponenttien ylätunnisteet

Staattiseksi sisällöksi, kuten kuvia ja tyylisivuja voit hallita päivityksen korkojakso web-sovelluksen **applicationHost.config** tai **web.config** -tiedostojen muokkaaminen.  Määritystiedostossa **system.webServer\staticContent\clientCache** -elementti Määritä `Cache-Control` sisällölle otsikko. Varten **web.config**määritykset vaikuttavat kaikki kansio ja alikansiot, ellei alikansion tasolla.  Voit esimerkiksi määrittää oletusarvoisesti aika-to-live päätasolla on kaikki kolme päivää välimuistiin staattiseksi sisällöksi, mutta alikansio, joka on enemmän muuttujan sisältöä 6 tuntia välimuisti-asetus on.  **ApplicationHost.config**kaikki sovellukset sivustossa vaikuttaako, mutta sovellusten **web.config** -tiedostoja voidaan ohittaa.

Seuraavat XML-esitetään ja Määritä suurin iän kolme päivää asetus **clientCache** Esimerkki:  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

Lisää määrittäminen **UseMaxAge** `Cache-Control: max-age=<nnn>` vastauksen otsikko- **CacheControlMaxAge** -määritteen arvon perusteella. Muoto aikajakson on **cacheControlMaxAge** -määrite on <days>. <hours>:<min>:<sec>. Katso lisätietoja **clientCache** -solmu [välimuistiin <clientCache> ](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).  

## <a name="setting-cache-control-headers-in-code"></a>Koodin asetus välimuisti-komponenttien ylätunnisteet

ASP.NET-sovelluksia voit määrittää CDN välimuistiin toiminta ohjelmallisesti **HttpResponse.Cache** ominaisuuden avulla. Lisätietoja **HttpResponse.Cache** -ominaisuus on artikkelissa [HttpResponse.Cache-ominaisuus](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) ja [HttpCachePolicy-luokka](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

Jos haluat tallentaa sovelluksen sisällön ASP.NET-ohjelmallisesti, varmista, että sisältö on merkitty välimuistiin tallennettava määrittämällä HttpCacheability *julkiseksi*. Varmista myös, että välimuistin tarkistajan on määritetty. Välimuistin tarkistajan on viimeksi muokattu soittamalla SetETag määrittäminen aikaleima määrittäminen soittamalla SetLastModified tai etag-arvo. Vaihtoehtoisesti voit myös määrittää välimuistin vanheneminen aika soittamalla SetExpires tai oletusarvo-välimuistin heuristiikkaa aiemmin tässä asiakirjassa on kuvattu, joihin voit luottaa.  

Jos esimerkiksi sisällön tunnin välimuistiin, Lisää seuraava:  

```csharp
// Set the caching parameters.
Response.Cache.SetExpires(DateTime.Now.AddHours(1));
Response.Cache.SetCacheability(HttpCacheability.Public);
Response.Cache.SetLastModified(DateTime.Now);
```

## <a name="next-steps"></a>Seuraavat vaiheet

- [Lue lisätietoja **clientCache** -elementti](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)
- [Ohjeissa **HttpResponse.Cache** -ominaisuus](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) 
- [Lue **HttpCachePolicy luokan**ohjeissa](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  
