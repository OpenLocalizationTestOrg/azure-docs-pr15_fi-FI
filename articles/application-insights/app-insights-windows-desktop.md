<properties 
    pageTitle="Käyttö- ja Windows-työpöytäsovellusten suorituskyvyn seuranta" 
    description="Analysoi käyttö- ja Windows-työpöytäsovelluksessa avattava HockeyApp ja sovelluksen havainnollistamisen suorituskykyä." 
    services="application-insights" 
    documentationCenter="windows"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/26/2016" 
    ms.author="awills"/>

# <a name="monitoring-usage-and-performance-in-windows-desktop-apps"></a>Käyttö- ja Windows-työpöytä-sovellusten suorituskykyä seuranta

*Hakemuksen tiedot on esikatselu.*

[Visual Studio hakemuksen tiedot](app-insights-overview.md) ja [HockeyApp](https://hockeyapp.net) avulla voit valvoa käyttöön sovelluksesi käyttö ja suorituskykyä.

> [AZURE.IMPORTANT] On suositeltavaa [HockeyApp](https://hockeyapp.net) jakaa ja työpöydän ja laitteen sovellusten valvonta. HockeyApp, jossa voit voit hallita jakauman, live testaus ja Käyttäjäpalaute sekä valvoa käyttö-ja kaatumisen. Voit myös [viedä ja että kanssa analyysin telemetriatietojen kysely](app-insights-hockeyapp-bridge-app.md).

> Vaikka telemetriatietojen voidaan lähettää sovelluksen havainnollistamisen työpöydän sovelluksesta, tämä on hoitopalveluja hyödyllisiä muistin ja koe varten.


## <a name="to-send-telemetry-to-application-insights-from-a-windows-application"></a>Hakemuksen tiedot telemetriatietojen lähettäminen Windows-sovellus

1. [Azure portal](https://portal.azure.com), [Luo sovelluksen tiedot-resurssi](app-insights-create-new-resource.md). Sovelluksen tyyppi-Valitse ASP.NET-sovellus.
2. Ota kopion Instrumentation-näppäintä. Etsi juuri luomasi uusi resurssi Essentials avattavasta avain. 
3. Visual Studiossa Muokkaa sovelluksen projektin NuGet-paketit ja lisää Microsoft.ApplicationInsights.WindowsServer. (Tai valitse Microsoft.ApplicationInsights, jos haluat ainoastaan ajoneuvon API ilman vakio telemetriatietojen sivustokokoelman moduulit.)
4. Määrittää instrumentation käyttäjäavainten joko koodi:

    `TelemetryConfiguration.Active.InstrumentationKey = "`*key-tuotetunnuksen*`";` 

    tai ApplicationInsights.config (Jos olet asentanut jotakin vakio telemetriatietojen):
 
    `<InstrumentationKey>`*key-tuotetunnuksen*`</InstrumentationKey>` 

    Jos käytät ApplicationInsights.config, varmista, että sen ominaisuuksia, napsauta ratkaisunhallinnassa määritetään **luominen toiminnon = sisältöä, kopioi tulosteen kansioon = kopioi**.
5. [Käytä Ohjelmointirajapinnan](app-insights-api-custom-events-metrics.md) lähettää telemetriatietojen.
6. Suorita sovellus ja tuo näkyviin telemetriatietojen resurssin luomasi Azure-portaalissa.

## <a name="telemetry"></a>Esimerkkikoodi

```C#

    public partial class Form1 : Form
    {
        private TelemetryClient tc = new TelemetryClient();
        ...
        private void Form1_Load(object sender, EventArgs e)
        {
            // Alternative to setting ikey in config file:
            tc.InstrumentationKey = "key copied from portal";

            // Set session data:
            tc.Context.User.Id = Environment.UserName;
            tc.Context.Session.Id = Guid.NewGuid().ToString();
            tc.Context.Device.OperatingSystem = Environment.OSVersion.ToString();

            // Log a page view:
            tc.TrackPageView("Form1");
            ...
        }

        protected override void OnClosing(CancelEventArgs e)
        {
            stop = true;
            if (tc != null)
            {
                tc.Flush(); // only for desktop apps

                // Allow time for flushing:
                System.Threading.Thread.Sleep(1000);
            }
            base.OnClosing(e);
        }

```

## <a name="next-steps"></a>Seuraavat vaiheet

* [Koontinäytön luominen](app-insights-dashboards.md)
* [Diagnostiikan haku](app-insights-diagnostic-search.md)
* [Tutustu arvot](app-insights-metrics-explorer.md)
* [Kirjoita Analytics-kyselyt](app-insights-analytics.md)
 
