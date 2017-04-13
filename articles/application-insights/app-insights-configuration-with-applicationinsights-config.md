<properties 
    pageTitle="Hakemuksen tiedot SDK määrittäminen ApplicationInsights.config tai .xml" 
    description="Ottaminen käyttöön tai poistaminen käytöstä tietojen keräämisen moduulit ja lisää suorituskyvyn laskureita ja muut parametrit" 
    services="application-insights"
    documentationCenter="" 
    authors="OlegAnaniev-MSFT"
    editor="alancameronwills" 
    manager="douge"/>
 
<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="03/12/2016" 
    ms.author="awills"/>

# <a name="configuring-the-application-insights-sdk-with-applicationinsightsconfig-or-xml"></a>Hakemuksen tiedot SDK määrittäminen ApplicationInsights.config tai .xml

Hakemuksen tiedot .NET SDK koostuu NuGet pakettien määrä. [Core paketti](http://www.nuget.org/packages/Microsoft.ApplicationInsights) sisältää Ohjelmointirajapinnan telemetriatietojen lähettäminen hakemuksen tiedot. [Lisää pakettien](http://www.nuget.org/packages?q=Microsoft.ApplicationInsights) avulla telemetriatietojen _Moduulit_ ja _Alustajien_ seuraamalla automaattisesti sovelluksen ja sen kontekstin telemetriatietojen. Voit ottaa tai telemetriatietojen moduulit ja Alustajien poistaminen käytöstä ja parametreja ja määrittää niitä joistakin säätämällä määritystiedosto.

Määritystiedoston nimeltä `ApplicationInsights.config` tai `ApplicationInsights.xml`sovelluksen tyypin mukaan. Tiedot lisätään automaattisesti projektin kun [asentaa useimmat SDK-versiot][start]. Se lisätään myös verkkosovellukseen [tila]tarkkailu IIS-palvelimessa[redfield], tai kun valitset Appplication havainnollistamisen [Azure-sivustossa tai AM-laajennus](app-insights-azure-web-apps.md).

Vastaavat tiedoston, voit hallita [SDK web-sivua]ei ole[client].

Tässä asiakirjassa kuvataan näkyy määritykset tiedoston, kuinka osat SDK-paketissa, hallita ja NuGet pakettien ladata kyseisten komponenttien osat.

## <a name="telemetry-modules-aspnet"></a>Telemetriatietojen moduulit (ASP.NET)

Telemetriatietojen kunkin moduulin kerää tietyntyyppisiä tietoja ja käyttää core API Lähetä tiedot. Moduulit asennetaan eri NuGet-paketteja, jotka myös lisätä tarvittavat rivit .config-tiedostossa.

Kunkin moduulin määritystiedostossa on solmu. Käytöstä moduuli, poista solmun tai sen kommentti.



### <a name="dependency-tracking"></a>Riippuvuus seuranta

[Riippuvuus seuranta](app-insights-dependencies.md) kerää telemetriatietojen puheluista sovelluksen tekee ja ulkoisia palveluja ja tietokantoihin. Voit sallia tämän moduulin avulla voit tutustua IIS-palvelimeen, sinun täytyy [asentaa tilan valvonta][redfield]. Voit käyttää sitä Azure verkkosovelluksissa tai VMs, [Valitse sovelluksen tiedot-tunniste](app-insights-azure-web-apps.md).

Voit myös kirjoittaa omia riippuvuuden seuranta [TrackDependency Ohjelmointirajapinnan](app-insights-api-custom-events-metrics.md#track-dependency)käyttäminen koodi.


* `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.DependencyCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) NuGet paketti.

### <a name="performance-collector"></a>Suorituskyvyn kerääminen

[Kerää järjestelmän suorituskyvyn laskureita](app-insights-performance-counters.md) , kuten suorittimen ja muistin verkon kuormituksen IIS-asennusten. Voit määrittää mitä laskureita voi kerätä, mukaan lukien suorituskyvyn laskureita itse määrittämiäsi.

* `Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule`
* [Microsoft.ApplicationInsights.PerfCounterCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector) NuGet paketti.


### <a name="application-insights-diagnostics-telemetry"></a>Hakemuksen tiedot diagnostiikka Telemetriatietojen

`DiagnosticsTelemetryModule` Itse sovelluksen havainnollistamisen instrumentation koodin virheistä. Esimerkiksi jos koodi ei voi käyttää suorituskyvyn laskureita tai `ITelemetryInitializer` ilmoittaa poikkeuksen. Jäljitä telemetriatietojen jäljittää moduuli näkyy [Diagnostiikan haun][diagnostic]. Lähettää diagnostiikkatiedot dc.services.vsallin.net.
 
* `Microsoft.ApplicationInsights.Extensibility.Implementation.Tracing.DiagnosticsTelemetryModule`
* [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet paketti. Jos asennat vain tämän paketin, ApplicationInsights.config-tiedostoa ei luoda automaattisesti. 

### <a name="developer-mode"></a>Kehittäjätilan

`DeveloperModeWithDebuggerAttachedTelemetryModule`pakottaa sovelluksen havainnollistamisen `TelemetryChannel` lähettää tietoja välittömästi, telemetriatietojen yhden kohteen kerrallaan, kun virheenkorjaus on liitetty hakeminen. Tämä pienentää tällä hetkellä välisen ajan, kun sovellus seuraa telemetriatietojen ja se näkyy sovelluksen tiedot-portaalissa. Se aiheuttaa merkittäviä katseltavan suorittimen ja verkon kaistanleveyden.

* `Microsoft.ApplicationInsights.WindowsServer.DeveloperModeWithDebuggerAttachedTelemetryModule`
* [Hakemuksen tiedot Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet-paketti

### <a name="web-request-tracking"></a>WWW-pyyntö seuranta

Raporttien pyyntöjen [vastaus kerran ja tulos koodi](app-insights-asp-net.md) . 

* `Microsoft.ApplicationInsights.Web.RequestTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet paketti

### <a name="exception-tracking"></a>Poikkeus seuranta

`ExceptionTrackingTelemetryModule`Seuraa käsittelemättömän poikkeukset-sovellukseen. [Virheet]ja poikkeukset[exceptions].

* `Microsoft.ApplicationInsights.Web.ExceptionTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet paketti


* `Microsoft.ApplicationInsights.WindowsServer.UnobservedExceptionTelemetryModule`-Seuraa [tapauksessa tehtävän poikkeukset](http://blogs.msdn.com/b/pfxteam/archive/2011/09/28/task-exception-handling-in-net-4-5.aspx).
* `Microsoft.ApplicationInsights.WindowsServer.UnhandledExceptionTelemetryModule`-Seuraa käsittelemättömän poikkeukset työntekijä roolit ja windows-palvelujen console-sovellukset.
* [Hakemuksen tiedot Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet paketti.

### <a name="microsoftapplicationinsights"></a>Microsoft.ApplicationInsights

Microsoft.ApplicationInsights-paketti sisältää SDK [core API](https://msdn.microsoft.com/library/mt420197.aspx) . Telemetriatietojen-moduulit tällä ja voit myös [käyttää sitä, voit määrittää oman telemetriatietojen](app-insights-api-custom-events-metrics.md).

* Ei-ApplicationInsights.config merkintä.
* [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet paketti. Jos asennat vain tämän NuGet, ei ole määritetty .config-tiedosto luodaan.

## <a name="telemetry-channel"></a>Telemetriatietojen kanava

Telemetriatietojen kanavan hallitsee puskurointi ja toimittaminen telemetriatietojen sovelluksen tiedot-palveluun. 

* `Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel`on oletusarvon kanavan Services. Se puskuroi tiedot muistiin.
* `Microsoft.ApplicationInsights.PersistenceChannel`on konsolin sovellusten löytämiseen. Se voi tallentaa unflushed tiedot pysyvät tallennustilan sovelluksen sulkee ja lähetä se kun sovellus käynnistetään uudelleen.


## <a name="telemetry-initializers-aspnet"></a>Telemetriatietojen Alustajien (ASP.NET)

Telemetriatietojen Alustajien määrittää konteksti-ominaisuuksia, jotka lähetetään sekä jokaisen kohteen telemetriatietojen. 

Voit [kirjoittaa omaa Alustajien](app-insights-api-filtering-sampling.md#add-properties) konteksti-ominaisuuksien määrittäminen.

Vakio Alustajien kaikki määritetään joko verkossa tai WindowsServer NuGet paketit:


* `AccountIdTelemetryInitializer`asettaa AccountId-ominaisuuden.
* `AuthenticatedUserIdTelemetryInitializer`asettaa AuthenticatedUserId-ominaisuuden kuten JavaScript-SDK on määritetty.
* `AzureRoleEnvironmentTelemetryInitializer`päivitykset `RoleName` ja `RoleInstance` ominaisuudet `Device` kaikki telemetriatietojen-tietueita, joiden tiedot, jotka Azure runtime-ympäristöstä kontekstissa.
* `BuildInfoConfigComponentVersionTelemetryInitializer`päivitykset `Version` -ominaisuuden `Component` kaikkien telemetriatietojen kohteiden poimittujen arvona konteksti `BuildInfo.config` tiedoston tuottamat MS Build.
* `ClientIpHeaderTelemetryInitializer`päivitykset `Ip` -ominaisuuden `Location` kaikkien telemetriatietojen kohteiden kontekstin perusteella `X-Forwarded-For` HTTP-otsikon pyynnön.
* `DeviceTelemetryInitializer`päivittää seuraavia ominaisuuksia `Device` kaikkien telemetriatietojen kohteiden kontekstissa.
 - `Type`on määritetty "PC"
 - `Id`asetetaan tietokoneen toimialuenimi, jossa WWW-sovellus on käynnissä.
 - `OemName`on määritetty arvo poimittujen `Win32_ComputerSystem.Manufacturer` kentän WMI: N avulla.
 - `Model`on määritetty arvo poimittujen `Win32_ComputerSystem.Model` kentän WMI: N avulla.
 - `NetworkType`on määritetty arvo poimittujen `NetworkInterface`.
 - `Language`on määritetty nimi `CurrentCulture`.
* `DomainNameRoleInstanceTelemetryInitializer`päivitykset `RoleInstance` -ominaisuuden `Device` kaikki telemetriatietojen-tietueita, joiden toimialuenimi, jossa WWW-sovellus on käynnissä tietokoneessa kontekstissa.
* `OperationNameTelemetryInitializer`päivitykset `Name` -ominaisuuden `RequestTelemetry` ja `Name` -ominaisuuden `Operation` kaikkien telemetriatietojen kohteiden kontekstin perusteella HTTP-menetelmä sekä ASP.NET MVC ohjauskoneen ja pyynnön käynnistää toiminnon nimet.
* `OperationIdTelemetryInitializer`tai `OperationCorrelationTelemetryInitializer` päivitykset `Operation.Id` konteksti-ominaisuuden kaikkien telemetriatietojen kohteiden seuranta käsiteltäessä pyyntö, jonka automaattisesti luotu `RequestTelemetry.Id`.
* `SessionTelemetryInitializer`päivitykset `Id` -ominaisuuden `Session` kaikki telemetriatietojen-tietueita, joiden arvo poimittujen kontekstissa `ai_session` evästeiden luoma ApplicationInsights JavaScript instrumentation koodin suorittaminen käyttäjän selaimessa. 
* `SyntheticTelemetryInitializer`tai `SyntheticUserAgentTelemetryInitializer` päivitykset `User`, `Session` ja `Operation` kaikki telemetriatietojen kohteiden ominaisuuksien kontekstit seurataan käsiteltäessä pyyntö synteettistä lähteestä, kuten saatavuus testata tai etsi ohjelma robotti. Oletusarvon mukaan [Arvot Explorer](app-insights-metrics-explorer.md) ei Näytä synteettiset telemetriatietojen. 

    `<Filters>` Tunnistaminen pyynnöt ominaisuuksien määrittäminen.
* `UserAgentTelemetryInitializer`päivitykset `UserAgent` -ominaisuuden `User` kaikkien telemetriatietojen kohteiden kontekstin perusteella `User-Agent` HTTP-otsikon pyynnön.
* `UserTelemetryInitializer`päivitykset `Id` ja `AcquisitionDate` ominaisuuksien `User` kaikki arvot poimittujen telemetriatietojen kohteet kontekstissa `ai_user` evästeiden luotu sovelluksen havainnollistamisen JavaScript instrumentation koodin suorittaminen käyttäjän selaimessa.
* `WebTestTelemetryInitializer`määrittää käyttäjän, istunnon tunnus ja synteettistä tietolähteen ominaisuudet, jotka ovat peräisin [käytettävyys testien](app-insights-monitor-web-app-availability.md)HTTP-pyyntöjen.
`<Filters>` Tunnistaminen pyynnöt ominaisuuksien määrittäminen.

## <a name="telemetry-processors-aspnet"></a>Telemetriatietojen suorittimien (ASP.NET)

Telemetriatietojen suorittimien voit suodattaa ja muokata telemetriatietojen kunkin kohteen, ennen kuin se lähetetään SDK-portaaliin.

Voit [kirjoittaa omia telemetriatietojen-suorittimista](app-insights-api-filtering-sampling.md#filtering).


#### <a name="adaptive-sampling-telemetry-processor-from-200-beta3"></a>Mukautuvat esimerkkejä telemetriatietojen suorittimen (2.0.0-beta3)

Oletusarvon mukaan käytössä. Jos sovelluksen lähettää telemetriatietojen useita, tämä suoritin poistaa tekstien.

```xml

    <TelemetryProcessors>
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    </TelemetryProcessors>

```

Parametri on kohde, jota algoritmin yrittää saavuttamiseksi. Jokaisen esiintymän SDK toimii itsenäisesti, joten jos palvelin on useita koneet klusterin, telemetriatietojen todellinen äänenvoimakkuuden kerrotaan vastaavasti.

[Lisätietoja esimerkkejä](app-insights-sampling.md).



#### <a name="fixed-rate-sampling-telemetry-processor-from-200-beta1"></a>Esimerkkejä vahvistetaan telemetriatietojen suorittimen (2.0.0-beta1)

On myös Vakio [Esimerkkejä telemetriatietojen suoritin](app-insights-api-filtering-sampling.md#sampling) (mistä 2.0.1):

```XML

    <TelemetryProcessors>
     <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">

     <!-- Set a percentage close to 100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
     <SamplingPercentage>10</SamplingPercentage>
     </Add>
   </TelemetryProcessors>

```



## <a name="channel-parameters-java"></a>Kanavan parametrit (Java)

Nämä parametrit vaikuttavat Java-SDK kannattaa tallentaa ja Tyhjennä se keräämiä telemetriatietojen tietoja.

#### <a name="maxtelemetrybuffercapacity"></a>MaxTelemetryBufferCapacity

Telemetriatietojen kohteet, jotka voidaan tallentaa on SDK ladatun tallennustilan määrä. Kun enimmäismäärä saavutetaan, telemetriatietojen puskuri on tyhjennetty - eli telemetriatietojen kohteet lähetetään sovelluksen tiedot-palvelimeen.

-   Min: 1
-   Max: 1000
-   Oletus: 500

```

  <ApplicationInsights>
      ...
      <Channel>
       <MaxTelemetryBufferCapacity>100</MaxTelemetryBufferCapacity>
      </Channel>
      ...
  </ApplicationInsights>
```

#### <a name="flushintervalinseconds"></a>FlushIntervalInSeconds 

Määrittää, kuinka usein tiedot, jotka on tallennettu ladatun tallennustilan pitäisi olla tyhjennetty (lähetettyjä sovelluksen havainnollistamisen).

-   Min: 1
-   Max: 300
-   Oletus: 5

```

    <ApplicationInsights>
      ...
      <Channel>
        <FlushIntervalInSeconds>100</FlushIntervalInSeconds>
      </Channel>
      ...
    </ApplicationInsights>
```

#### <a name="maxtransmissionstoragecapacityinmb"></a>MaxTransmissionStorageCapacityInMB

Määrittää enimmäiskoko megatavuina, pysyvä tallennustilan paikallisella levyllä toiminnolle. Tämä käytetään pysyvä telemetriatietojen kohteet, jotka toimitetaan sovelluksen tiedot-päätepisteen epäonnistui. Tallennustilan koko on saavutettu, kun uusia telemetriatietojen kohteita hylätään.

-   Min: 1
-   Max: 100
-   Oletus: 10

```

   <ApplicationInsights>
      ...
      <Channel>
        <MaxTransmissionStorageCapacityInMB>50</MaxTransmissionStorageCapacityInMB>
      </Channel>
      ...
   </ApplicationInsights>
```



## <a name="instrumentationkey"></a>InstrumentationKey

Määrittää, jossa tiedot on hakemuksen tiedot resurssin. Yleensä voit luoda erilliset resurssin erillisessä avaimella kunkin sovellustesi.

Jos haluat määrittää käyttäjäavainten dynaamisesti – esimerkiksi, jos haluat lähettää tulokset eri resurssit - sovelluksestasi pois määritystiedosto-näppäintä ja määrittää sen koodin.

Voit määrittää kaikki esiintymät TelemetryClient avain-standardin telemetriatietojen moduulit, mukaan lukien määrittää avaimen TelemetryConfiguration.Active. Tee näin alustus-menetelmää, kuten global.aspx.cs ASP.NET-palvelussa:

```C#

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey = 
          // - for example -
          WebConfigurationManager.Settings["ikey"];
      //...
```

Jos haluat lähettää tapahtumien tietyt resurssia, voit määrittää avaimen tietyn TelemetryClient varten:

```C#

    var tc = new TelemetryClient();
    tc.Context.InstrumentationKey = "----- my key ----";
    tc.TrackEvent("myEvent");
    // ...

```

[Lisätietoja Ohjelmointirajapinnan][api].

Saat uusi avain, [Luo uusi resurssi sovelluksen tiedot-portaalissa][new].

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[netlogs]: app-insights-asp-net-trace-logs.md
[new]: app-insights-create-new-resource.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md

