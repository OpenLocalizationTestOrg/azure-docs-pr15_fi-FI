<properties 
    pageTitle="Suodattaminen ja sovelluksen havainnollistamisen SDK esikäsittely | Microsoft Azure" 
    description="Kirjoita Telemetriatietojen suorittimien ja Telemetriatietojen alustajia SDK suodatuksessa tai Lisää ominaisuus tietoja ennen telemetriatietojen lähetetään sovelluksen tiedot-portaaliin." 
    services="application-insights"
    documentationCenter="" 
    authors="beckylino" 
    manager="douge"/>
 
<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="08/30/2016" 
    ms.author="borooji"/>

# <a name="filtering-and-preprocessing-telemetry-in-the-application-insights-sdk"></a>Suodattaminen ja esikäsittely telemetriatietojen sovelluksen havainnollistamisen SDK: ssa

*Hakemuksen tiedot on esikatselu.*

Voit kirjoittaa ja määritä laajennuksia sovelluksen havainnollistamisen SDK mukauttamiseen kuinka telemetriatietojen tallennetaan ja käsitellään ennen sen lähettämistä sovelluksen tiedot-palveluun. 

Nämä ominaisuudet ovat tällä hetkellä käytettävissä ASP.NET-SDK.

* Telemetriatietojen äänenvoimakkuuden [näyte](app-insights-sampling.md) vähentää vaikuttamatta tilaston. Säilyttää toisiinsa liittyvät arvopisteet niin, että voit siirtyä toiseen, kun ongelma ohjelmistossa. Valitse-portaalissa yhteensä-arvo kerrotaan korvaamaan esimerkkejä.
* [Suodattaminen ja Telemetriatietojen suorittimien](#filtering) voit valita tai muokata telemetriatietojen SDK: ssa, ennen kuin se lähetetään palvelimeen. Voit esimerkiksi pienentää telemetriatietojen äänenvoimakkuuden jättämällä pois robots-pyynnöt. Mutta suodatus on enemmän basic vähentää liikenne kuin esimerkkejä. Sen avulla voit määrittää tarkemmin mitä lähetetään, mutta sinun on otettava huomioon, että se vaikuttaa tilaston – esimerkiksi, jos voit suodattaa kaikki onnistuneen pyynnöt.
* Kaikki sovellukset, kuten telemetriatietojen vakio moduuleista lähetetään telemetriatietojen [telemetriatietojen Alustajien Lisää ominaisuus](#add-properties) . Voit esimerkiksi lisätä laskettuja arvoja; tai versio numeroiden, jolla voit suodattaa tietoja portaalissa.
* Mukautetut tapahtumat ja arvot lähettämiseen käytettävää [SDK API](app-insights-api-custom-events-metrics.md) .


Ennen aloittamista:

* Asenna [sovellus havainnollistamisen ASP.NET v2 SDK](app-insights-asp-net.md) -sovelluksen. 


<a name="filtering"></a>
## <a name="filtering-itelemetryprocessor"></a>Suodatus: ITelemetryProcessor

Tällä menetelmällä antaa Lisää suoran päättää, mitä ohjelmistopakettia tai telemetriatietojen virta ulkopuolelle. Voit käyttää sitä yhdessä esimerkkejä, tai erikseen.

Voit suodattaa telemetriatietojen, kirjoittaa telemetriatietojen suorittimen ja rekisteröityä SDK. Kaikki telemetriatietojen käy läpi suorittimen ja voit halutessasi poistaa virta tai Lisää ominaisuus. Tämä sisältää telemetriatietojen, kuten HTTP-pyyntö kerääminen ja riippuvuuden kerääminen vakio moduuleista ja telemetriatietojen olet kirjoittanut itse. Voit esimerkiksi suodattaa tietoja pyynnöt robots tai onnistuneen riippuvuuden puheluita telemetriatietojen. 

> [AZURE.WARNING] Suodattaminen SDK lähettämät telemetriatietojen suorittimien voit jakauman.vinous tilastotiedot näkyvissä olevat portaalin ja vaikeuttaa noudattamalla liittyvät kohteet.
> 
> Sen sijaan, kannattaa käyttää [Esimerkkejä](app-insights-sampling.md).

### <a name="create-a-telemetry-processor"></a>Luo telemetriatietojen-suoritin

1. Varmista, että projektin sovelluksen tiedot-SDK on 2.0.0 versio tai sitä uudemmalla versiolla. Visual Studio ratkaisunhallinnassa projektin hiiren kakkospainikkeella ja valitse NuGet pakettien hallinta. Tarkista Microsoft.ApplicationInsights.Web NuGet pakettien hallinta.

1. Suodattimen luominen Toteuta ITelemetryProcessor. Tämä on toista laajennettavuus pistettä, kuten telemetriatietojen moduulin telemetriatietojen alustaja ja telemetriatietojen kanava. 

    Huomaa Telemetriatietojen suorittimien käyttää komentoketju, jonka käsittely. Kun vahvistaa telemetriatietojen suoritin-linkin välitetään seuraava yhdistettävää suoritin. Kun telemetriatietojen arvopiste siirretään prosessin menetelmä, se ei työnsä ja sitten tallenteita seuraavan Telemetriatietojen suoritin ketju.

    ``` C#

    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.Extensibility;

    public class SuccessfulDependencyFilter : ITelemetryProcessor
      {
        private ITelemetryProcessor Next { get; set; }

        // You can pass values from .config
        public string MyParamFromConfigFile { get; set; }

        // Link processors to each other in a chain.
        public SuccessfulDependencyFilter(ITelemetryProcessor next)
        {
            this.Next = next;
        }
        public void Process(ITelemetry item)
        {
            // To filter out an item, just return 
            if (!OKtoSend(item)) { return; }
            // Modify the item if required 
            ModifyItem(item);

            this.Next.Process(item);
        }

        // Example: replace with your own criteria.
        private bool OKtoSend (ITelemetry item)
        {
            var dependency = item as DependencyTelemetry;
            if (dependency == null) return true;

            return dependency.Success != true;
        }

        // Example: replace with your own modifiers.
        private void ModifyItem (ITelemetry item)
        {
            item.Context.Properties.Add("app-version", "1." + MyParamFromConfigFile);
        }
    }
    

    ```
2. Lisää tämä ApplicationInsights.config: 

```XML

    <TelemetryProcessors>
      <Add Type="WebApplication9.SuccessfulDependencyFilter, WebApplication9">
         <!-- Set public property -->
         <MyParamFromConfigFile>2-beta</MyParamFromConfigFile>
      </Add>
    </TelemetryProcessors>

```

(Tämä on samassa osassa, jossa esimerkkejä suodattimen alustaa.)

Voit siirtää merkkijonoarvoa .config-tiedostosta antamalla julkisen nimettyjen ominaisuuksien omaa luokkaasi. 

> [AZURE.WARNING] Huolehtia vastaamaan tyyppinimi- ja minkä tahansa ominaisuuksien nimiä koodin luokan ja ominaisuuden nimiä .config-tiedostossa. Jos .config-tiedostossa viittaa olematon tyypin tai ominaisuuden, SDK äänettömästi voi epäonnistua Lähetä kaikki telemetriatietojen.

 
**Vaihtoehtoisesti** voit valmistella koodin suodattimen. Lisää sopiva alustus luokka - esimerkiksi AppStart-Global.asax.cs - suoritin ketjussa:

```C#

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;
    builder.Use((next) => new SuccessfulDependencyFilter(next));

    // If you have more processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

Kun tässä kohdassa käyttää järjestelmän suorittimien TelemetryClients.

### <a name="example-filters"></a>Esimerkki suodattimet

#### <a name="synthetic-requests"></a>Synteettiset pyynnöt

Voit suodattaa bots ja web testejä. Vaikka arvot Explorer antaa mahdollisuuden suodattamaan synteettistä lähteistä, tämä asetus vähentää liikenne suodattamalla SDK-palvelussa.

``` C#

    public void Process(ITelemetry item)
    {
      if (!string.IsNullOrEmpty(item.Context.Operation.SyntheticSource)) {return;}

      // Send everything else: 
      this.Next.Process(item);
    }

```

#### <a name="failed-authentication"></a>Epäonnistuneiden todennus

Voit suodattaa pois pyynnöt ja "401" vastaus. 

```C#

public void Process(ITelemetry item)
{
    var request = item as RequestTelemetry;

    if (request != null &&
    request.ResponseCode.Equals("401", StringComparison.OrdinalIgnoreCase))
    {
        // To filter out an item, just terminate the chain: 
        return;
    }
    // Send everything else: 
    this.Next.Process(item);
}

```

#### <a name="filter-out-fast-remote-dependency-calls"></a>Nopea remote riippuvuuden puhelut suodattaminen

Jos haluat vain vianmäärityksen puhelut, joka on hidas, voit suodattaa pois nopea niistä. 

> [AZURE.NOTE] Tämä jakauman.vinous näet portaalissa tilastotiedot. Riippuvuus-kaavio näyttää, ikään kuin riippuvuuden kutsut ovat kaikki virheet.

``` C#

public void Process(ITelemetry item)
{
    var request = item as DependencyTelemetry;
            
    if (request != null && request.Duration.Milliseconds < 100)
    {
        return;
    }
    this.Next.Process(item);
}

```

#### <a name="diagnose-dependency-issues"></a>Riippuvuus-ongelmien vianmääritys

[Tässä blogissa](https://azure.microsoft.com/blog/implement-an-application-insights-telemetry-processor/) kerrotaan projektin riippuvuuden ongelmien vianmääritys lähettämällä tavallisen Testaa riippuvuudet automaattisesti.


<a name="add-properties"></a>
## <a name="add-properties-itelemetryinitializer"></a>Lisää ominaisuus: ITelemetryInitializer

Telemetriatietojen Alustajien avulla voit määrittää yleisiä ominaisuuksia, jotka lähetetään kaikille telemetriatietojen; ja jos haluat ohittaa valitun vakio telemetriatietojen moduuleja. 

Web-pakkauksen sovelluksen tietoa esimerkiksi kerää tietoja pyyntöjen telemetriatietojen. Oletusarvon mukaan se merkitsee kuin epäonnistui vastauksen koodilla pyyntö > = 400. Mutta jos haluat käsitellä 400 onnistumisen, voit kirjoittaa telemetriatietojen alustaja, joka määrittää onnistui-ominaisuuden.

Jos annat telemetriatietojen alustaja, jonka nimi on aina, kun kutsutaan Track*() tavoilla. Tämä sisältää menetelmiä kutsutaan vakio telemetriatietojen moduuleissa. Käytännön mukaan näitä moduuleja ei määritetä ominaisuus, joka on jo määritetty alustajaluettelon. 

**Määrittää, että alustimessa**

*C#*

```C#

    using System;
    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.DataContracts;
    using Microsoft.ApplicationInsights.Extensibility;

    namespace MvcWebRole.Telemetry
    {
      /*
       * Custom TelemetryInitializer that overrides the default SDK 
       * behavior of treating response codes >= 400 as failed requests
       * 
       */
      public class MyTelemetryInitializer : ITelemetryInitializer
      {
        public void Initialize(ITelemetry telemetry)
        {
            var requestTelemetry = telemetry as RequestTelemetry;
            // Is this a TrackRequest() ?
            if (requestTelemetry == null) return;
            int code;
            bool parsed = Int32.TryParse(requestTelemetry.ResponseCode, out code);
            if (!parsed) return;
            if (code >= 400 && code < 500)
            {
                // If we set the Success property, the SDK won't change it:
                requestTelemetry.Success = true;
                // Allow us to filter these requests in the portal:
                requestTelemetry.Context.Properties["Overridden400s"] = "true";
            }
            // else leave the SDK to set the Success property      
        }
      }
    }
```

**Lataa oman alustimessa**

Valitse ApplicationInsights.config:

    <ApplicationInsights>
      <TelemetryInitializers>
        <!-- Fully qualified type name, assembly name: -->
        <Add Type="MvcWebRole.Telemetry.MyTelemetryInitializer, MvcWebRole"/> 
        ...
      </TelemetryInitializers>
    </ApplicationInsights>

*Vaihtoehtoisesti* voit vahvistaa koodi, kuten Global.aspx.cs alustaja:


```C#
    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```


[Lisätietoja on tässä esimerkissä.](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole)

<a name="js-initializer"></a>
### <a name="javascript-telemetry-initializers"></a>JavaScript telemetriatietojen Alustajien

*JavaScript*

Lisää telemetriatietojen alustaja heti, kun alustus tunnus, jolla olet saanut-portaalista: 

```JS

    <script type="text/javascript">
        // ... initialization code
        ...({
            instrumentationKey: "your instrumentation key"
        });
        window.appInsights = appInsights;


        // Adding telemetry initializer.
        // This is called whenever a new telemetry item
        // is created.

        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;

                // To check the telemetry item’s type - for example PageView:
                if (envelope.name == Microsoft.ApplicationInsights.Telemetry.PageView.envelopeType) {
                    // this statement removes url from all page view documents
                    telemetryItem.url = "URL CENSORED";
                }

                // To set custom properties:
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["globalProperty"] = "boo";

                // To set custom metrics:
                telemetryItem.measurements = telemetryItem.measurements || {};
                telemetryItem.measurements["globalMetric"] = 100;
            });
        });

        // End of inserted code.

        appInsights.trackPageView();
    </script>
```

Luettelo käytettävissä telemetryItem mukautettu ominaisuuksista on artikkelissa [tietomallin](app-insights-export-data-model.md#lttelemetrytypegt).

Voit lisätä niin monta Alustajien kuin haluat. 


## <a name="itelemetryprocessor-and-itelemetryinitializer"></a>ITelemetryProcessor ja ITelemetryInitializer

Mikä on telemetriatietojen suorittimien ja telemetriatietojen Alustajien välinen ero?

* On joitakin päällekkäisyyksiä-toiminnot heidän kanssaan: molemmat avulla voidaan lisätä telemetriatietojen ominaisuuksia.
* TelemetryInitializers suoritetaan aina ennen TelemetryProcessors.
* TelemetryProcessors avulla voit korvata tai hylätä telemetriatietojen kohteen kokonaan.
* TelemetryProcessors ei käsitellä suorituskyvyn laskuri telemetriatietojen.



## <a name="persistence-channel"></a>Pysyvyyttä kanava 

Sovelluksen käytetään, jossa internet-yhteyttä ei ole aina käytettävissä tai hidas, kannattaa käyttää sen sijaan, että oletusarvoinen ladatun kanavan pysyvyyttä kanava. 

Oletusarvoinen ladatun kanavan menettää telemetriatietojen, joka ei ole lähetetty sovellus suljetaan aika. Vaikka voit käyttää `Flush()` yrittää jäljellä olevan puskurin tietoja Lähetä se edelleen menettää tietojen Jos ei ole internet-yhteyttä, tai jos sovellus suljetaan ennen siirto on valmis.

Sen sijaan pysyvyyttä kanavan puskuroi telemetriatietojen tiedostossa ennen sen lähettämistä-portaaliin. `Flush()`Voit varmistaa, että tiedot on tallennettu tiedosto. Mitään tietoja ei lähetetä mennessä, kun sovellus suljetaan, pysyy tiedosto. Kun sovellus käynnistyy, tiedot lähetetään sitten, onko internet-yhteys. Tietoja kasvaa tiedoston niin kauan kuin on tarpeen, kunnes yhteys on käytettävissä. 

### <a name="to-use-the-persistence-channel"></a>Jos haluat käyttää pysyvyyttä kanavan

1. Tuo NuGet paketin [Microsoft.ApplicationInsights.PersistenceChannel](https://www.nuget.org/packages/Microsoft.ApplicationInsights.PersistenceChannel/1.2.3).
2. Sisällytä tämä koodi sovelluksen, sopivan alustus sijainnissa:
 
    ```C# 

      using Microsoft.ApplicationInsights.Channel;
      using Microsoft.ApplicationInsights.Extensibility;
      ...

      // Set up 
      TelemetryConfiguration.Active.InstrumentationKey = "YOUR INSTRUMENTATION KEY";
 
      TelemetryConfiguration.Active.TelemetryChannel = new PersistenceChannel();
    
    ``` 
3. Käytä `telemetryClient.Flush()` ennen sovelluksen sulkeutuu, varmista, että tiedot on lähetetty portaaliin tai tallentaa tiedostoon.

    Huomaa, että Flush() on synkronoitu pysyvyyttä kanavan, mutta muiden lähteiden asynkroninen.

 
Pysyvyyttä kanavan on optimoitu tilanteessa, jossa sovellus tuottamat tapahtumien määrä on suhteellisen pieni ja yhteys on usein epäluotettavista laitteet. Kanavan kirjoitettava tapahtumien levylle luotettava varastoon ensin ja yritä sitten lähettää. 

#### <a name="example"></a>Esimerkki

Oletetaan, että haluat valvoa käsittelemättömän poikkeukset. Voit tilata `UnhandledException` tapahtuma. Takaisinsoitto voit lisätä kohdistus kutsu, varmista, että telemetriatietojen on samanlainen.
 
```C# 

AppDomain.CurrentDomain.UnhandledException += CurrentDomain_UnhandledException; 
 
... 
 
private void CurrentDomain_UnhandledException(object sender, UnhandledExceptionEventArgs e) 
{ 
    ExceptionTelemetry excTelemetry = new ExceptionTelemetry((Exception)e.ExceptionObject); 
    excTelemetry.SeverityLevel = SeverityLevel.Critical; 
    excTelemetry.HandledAt = ExceptionHandledAt.Unhandled; 
 
    telemetryClient.TrackException(excTelemetry); 
 
    telemetryClient.Flush(); 
} 

``` 

Kun sovellus suljetaan, näyttöön tulee tiedoston `%LocalAppData%\Microsoft\ApplicationInsights\`, joka sisältää pakattu tapahtumat. 
 
Seuraavan kerran käynnistät tämän sovelluksen kanavan Nosta tämän tiedoston ja pitää telemetriatietojen hakemuksen tiedot, jos se on mahdollista.

#### <a name="test-example"></a>Testi-Esimerkki

```C#

using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.Extensibility;

namespace ConsoleApplication1
{
    class Program
    {
        static void Main(string[] args)
        {
            // Send data from the last time the app ran
            System.Threading.Thread.Sleep(5 * 1000);

            // Set up persistence channel

            TelemetryConfiguration.Active.InstrumentationKey = "YOUR KEY";
            TelemetryConfiguration.Active.TelemetryChannel = new PersistenceChannel();

            // Send some data

            var telemetry = new TelemetryClient();

            for (var i = 0; i < 100; i++)
            {
                var e1 = new Microsoft.ApplicationInsights.DataContracts.EventTelemetry("persistenceTest");
                e1.Properties["i"] = "" + i;
                telemetry.TrackEvent(e1);
            }

            // Make sure it's persisted before we close
            telemetry.Flush();
        }
    }
}

```


Pysyvyyttä kanavan koodi on [github](https://github.com/Microsoft/ApplicationInsights-dotnet/tree/v1.2.3/src/TelemetryChannels/PersistenceChannel). 


## <a name="reference-docs"></a>Viittaus asiakirjoja

* [API yleiskatsaus](app-insights-api-custom-events-metrics.md)

* [ASP.NET-viittaus](https://msdn.microsoft.com/library/dn817570.aspx)


## <a name="sdk-code"></a>SDK-koodi

* [ASP.NET-Core SDK-paketissa](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [ASP.NET 5](https://github.com/Microsoft/ApplicationInsights-aspnet5)
* [JavaScript-SDK](https://github.com/Microsoft/ApplicationInsights-JS)


## <a name="next"></a>Seuraavat vaiheet


* [Etsi tapahtuma- ja lokit][diagnostic]
* [Esimerkkejä](app-insights-sampling.md)
* [Vianmääritys][qna]


<!--Link references-->

[client]: app-insights-javascript.md
[config]: app-insights-configuration-with-applicationinsights-config.md
[create]: app-insights-create-new-resource.md
[data]: app-insights-data-retention-privacy.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[greenbrown]: app-insights-asp-net.md
[java]: app-insights-java-get-started.md
[metrics]: app-insights-metrics-explorer.md
[qna]: app-insights-troubleshoot-faq.md
[trace]: app-insights-search-diagnostic-logs.md
[windows]: app-insights-windows-get-started.md

 
