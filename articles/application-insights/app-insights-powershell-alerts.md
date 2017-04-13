<properties
    pageTitle="Ilmoitusten määrittäminen hakemuksen tiedot PowerShellin avulla"
    description="Automatisoida määrittämisen sovelluksen havainnollistamisen saat sähköpostit metrisillä muutoksista."
    services="application-insights"
    documentationCenter=""
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/19/2016"
    ms.author="awills"/>

# <a name="use-powershell-to-set-alerts-in-application-insights"></a>Ilmoitusten määrittäminen hakemuksen tiedot PowerShellin avulla

Voit automatisoida [Visual Studio hakemuksen tiedot](app-insights-overview.md) [ilmoitusten](app-insights-alerts.md) määrittäminen.

Lisäksi voit [määrittää webhooks automatisoida vastauksesi ilmoituksen](../monitoring-and-diagnostics/insights-webhooks-alerts.md).

## <a name="one-time-setup"></a>Erikseen asetukset

Jos et ole käyttänyt PowerShellin Azure-tilauksen ennen:

PowerShellin Azure-moduulin asentaminen tietokoneeseen, jonka haluat suorittaa komentosarjat.

 * Asenna [Microsoftin WWW-ympäristö asennusohjelma (v5 tai uudempi)](http://www.microsoft.com/web/downloads/platform.aspx).
 * Sen avulla Microsoft Azure PowerShellin asentaminen


## <a name="connect-to-azure"></a>Yhteyden muodostaminen Azure

Käynnistä PowerShellin Azure ja [Muodosta yhteys tilauksen](../powershell-install-configure.md):

```PowerShell

    Add-AzureAccount
    Switch-AzureMode AzureResourceManager
```


## <a name="get-alerts"></a>Ilmoitusten ottaminen käyttöön

    Get-AlertRule -ResourceGroup "Fabrikam" [-Name "My rule"] [-DetailedOutput]

## <a name="add-alert"></a>Lisää ilmoitus


    Add-AlertRule  -Name "{ALERT NAME}" -Description "{TEXT}" `
     -ResourceGroup "{GROUP NAME}" `
     -ResourceId "/subscriptions/{SUBSCRIPTION ID}/resourcegroups/{GROUP NAME}/providers/microsoft.insights/components/{APP RESOURCE NAME}" `
     -MetricName "{METRIC NAME}" `
     -Operator GreaterThan  `
     -Threshold {NUMBER}   `
     -WindowSize {HH:MM:SS}  `
     [-SendEmailToServiceOwners] `
     [-CustomEmails "EMAIL1@X.COM","EMAIL2@Y.COM" ] `
     -Location "East US"
     -RuleType Metric



## <a name="example-1"></a>Esimerkki 1

Sähköpostin minulle, jos HTTP-pyyntöjen keskiarvo yli 5 minuuttia palvelimen vastaus on hitaammin sekunnin. Sovelluksen tiedot-resurssin kutsutaan IceCreamWebApp, ja se on resurssiryhmä Fabrikam. Olen Azure tilauksen omistaja.

GUID-tunnus on Tilaustunnus (ei instrumentation avain sovelluksen).

    Add-AlertRule -Name "slow responses" `
     -Description "email me if the server responds slowly" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "request.duration" `
     -Operator GreaterThan `
     -Threshold 1 `
     -WindowSize 00:05:00 `
     -SendEmailToServiceOwners `
     -Location "East US" -RuleType Metric

## <a name="example-2"></a>Esimerkki 2

Minulla on jokin sovellus, jossa käytetään [TrackMetric()](app-insights-api-custom-events-metrics.md#track-metric) ja raportoi mittarin nimeltä "salesPerHour." Lähetä sähköpostia Työtoverini "salesPerHour" laskee alle 100, jos keskiarvo yli 24 tuntia.

    Add-AlertRule -Name "poor sales" `
     -Description "slow sales alert" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "salesPerHour" `
     -Operator LessThan `
     -Threshold 100 `
     -WindowSize 24:00:00 `
     -CustomEmails "satish@fabrikam.com","lei@fabrikam.com" `
     -Location "East US" -RuleType Metric

Sama sääntö voi käyttää ilmoitettu toisen seuranta-puhelun, kuten TrackEvent tai trackPageView [Mitta-parametrin](app-insights-api-custom-events-metrics.md#properties) arvo.

## <a name="metric-names"></a>Metrijärjestelmän nimet

Metrijärjestelmän nimi | Näytön nimi | Kuvaus
---|---|---
`basicExceptionBrowser.count`|Selaimen poikkeukset|Aiheutetaan selaimessa poikkeukset määrä.
`basicExceptionServer.count`|Palvelimen poikkeukset|Sovelluksen käsittelemättömän poikkeukset määrä
`clientPerformance.clientProcess.value`|Asiakkaan käsittelyaikaa|Vastaanottaa asiakirjan viimeinen tavu, kunnes DOM ladataan välisen ajan. Asynkronisten pyyntöjen edelleen saattaa käsitellään.
`clientPerformance.networkConnection.value`|Sivun verkon Yhdistä latausajasta| Aika, muodosta yhteys verkkoon kestää selaimessa. Voi olla 0, jos välimuistissa.
`clientPerformance.receiveRequest.value`|Vastausajan vastaanottaminen| Selaimen vaiheiden avulla saat vastauksen pyynnön lähettäminen välisen ajan.
`clientPerformance.sendRequest.value`|Lähettää pyynnön aika| Aika, jonka selaimen lähettää pyynnön.
`clientPerformance.total.value`|Selaimen sivun latausajasta|Aika-pyyntö, kunnes DOM, tyylisivut, komentosarjojen ja kuvat ladataan.
`performanceCounter.available_bytes.value`|Käytettävissä oleva muisti|Muistin heti käytettävissä prosessin tai järjestelmän käyttöön.
`performanceCounter.io_data_bytes_per_sec.value`|Prosessin IO korko|Tavujen sekunnissa lukea ja kirjoittaa tiedostoja, verkon ja laitteisiin.
`performanceCounter.number_of_exceps_thrown_per_sec`|poikkeus korko|Sekunnissa poikkeukset.
`performanceCounter.percentage_processor_time.value`|Prosessin Suoritin|Kulunut aika, kaikki prosessin yhteydet käyttämä suorittamisen ohjeiden suoritin sovellusten prosessin prosenttiosuus.
`performanceCounter.percentage_processor_total.value`|Suoritinaika|Aika, jonka aikana suoritin-vapaa viestiketjuissa siirtyminen-prosentteina.
`performanceCounter.process_private_bytes.value`|Prosessin yksityisten tavujen|Muisti yksinomaan myönnetyt valvottu sovelluksen prosessit.
`performanceCounter.request_execution_time.value`|ASP.NET-pyynnön suoritusaika|Suoritusaika viimeisimmän pyynnön.
`performanceCounter.requests_in_application_queue.value`|ASP.NET-pyyntöjen suorittamisen jonossa|Sovelluksen pyynnön jonon pituuden.
`performanceCounter.requests_per_sec`|ASP.NET-pyynnön korko|Kaikki pyynnöt sekunnissa sovelluksen ASP.NET korkokannan.
`remoteDependencyFailed.durationMetric.count`|Riippuvuus-virheet|Ulkoiset resurssit palvelimen sovelluksen epäonnistui puhelujen määrä.
`request.duration`|Palvelimen vastausajan|Vastaanottamisen HTTP-pyynnön ja vastauksen lähettämistä viimeistely välisen ajan.
`request.rate`|Pyydä korko|Kaikki pyynnöt sekunnissa sovelluksen korkokannan.
`requestFailed.count`|Epäonnistuneiden pyyntöjen|Laske, HTTP-pyyntöjen, tuloksena on > = 400
`view.count`|Sivun näkymät|Asiakkaan pyynnöt web-sivun määrä. Synteettiset liikenne on suodatettu.
{mukautetun metrisillä nimesi}|{Metrisillä nimi}|Metrijärjestelmän arvo raportoitu [TrackMetric](app-insights-api-custom-events-metrics.md#track-metric) tai [Seuranta puhelun mitat-parametri](app-insights-api-custom-events-metrics.md#properties).

Määritetty lähettämät eri telemetriatietojen moduulit:

Metrijärjestelmän ryhmä | Kerääminen moduuli
---|---
basicExceptionBrowser,<br/>clientPerformance,<br/>Näytä | [Selaimen JavaScript](app-insights-javascript.md)
performanceCounter | [Suorituskyky](app-insights-configuration-with-applicationinsights-config.md#nuget-package-3)
remoteDependencyFailed| [Riippuvuus](app-insights-configuration-with-applicationinsights-config.md#nuget-package-1)
pyynnön<br/>requestFailed|[Palvelimen pyyntö](app-insights-configuration-with-applicationinsights-config.md#nuget-package-2)

## <a name="webhooks"></a>Webhooks

Voit [automatisoida vastauksesi ilmoituksen](../monitoring-and-diagnostics/insights-webhooks-alerts.md). Azure kutsuu valittua verkko-osoitteesta, kun ilmoituksen korotetaan.

## <a name="see-also"></a>Katso myös


* [Komentosarjan, joka määrittää sovelluksen tiedot](app-insights-powershell-script-create-resource.md)
* [Hakemuksen tiedot ja WWW-testi resurssien luominen malleista](app-insights-powershell.md)
* [Microsoft Azure diagnostiikka liitäntä sovelluksen havainnollistamisen automatisointi](app-insights-powershell-azure-diagnostics.md)
* [Automaattinen vastaus ilmoituksen](../monitoring-and-diagnostics/insights-webhooks-alerts.md)
