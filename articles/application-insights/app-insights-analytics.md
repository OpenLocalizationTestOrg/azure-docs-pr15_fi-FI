<properties 
    pageTitle="Analytics - sovelluksen tiedot tehokkaita hakutyökalun | Microsoft Azure" 
    description="Yleistä Analytics-sovelluksen tiedot tehokkaita diagnostiikan hakutyökalun. " 
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
    ms.date="07/26/2016" 
    ms.author="awills"/>


# <a name="analytics-in-application-insights"></a>Analytics-sovelluksen tiedot


[Analyysin](app-insights-analytics.md) on tehokas haku-toiminnon [Hakemuksen tiedot](app-insights-overview.md). Nämä sivut kuvaavat Analytics kyselyn lanquage. 

* **[Johdanto videossa](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.
* Jos sovellus ei ole lähettää tiedot sovelluksen havainnollistamisen vielä **[Kokeile Analytics-Simuloitu tietojamme](https://analytics.applicationinsights.io/demo)** .

## <a name="queries-in-analytics"></a>Kyselyjen analysointitietoja
 
Tyypillinen kysely on *sarjan *operaattorit* erotettu perään lähdetaulukkoon* `|`. 

Esimerkiksi japanin Katso, mitä kellonaika Hyderabad kansalaisten Kokeile Microsoftin WWW-sovellusta. Ja silloin, kun emme ole käytettävissä, katsotaan HTTP-pyyntöjä palautetaan tuloksen mitä koodit. 

```AIQL

    requests      // Table of events that log HTTP requests.
  	| where timestamp > ago(7d) and client_City == "Hyderabad"
  	| summarize clients = dcount(client_IP) 
      by tod_UTC=bin(timestamp % 1d, 1h), resultCode
  	| extend local_hour = (tod_UTC + 5h + 30min) % 24h + datetime("2001-01-01") 
```

Olemme laskea eri asiakkaan IP-osoitteet, ne ryhmittely vuorokauden tuntia viimeisten seitsemän päivän päälle. 

Oletetaan, että Näytä tulokset palkkikaavion esityksen, valitsemalla pinoaminen eri vastauksen koodit tuloksista:

![Valitse palkkikaaviossa, x ja y-akselit ja sitten Segmentointi](./media/app-insights-analytics/020.png)

Näyttää siltä, että monipuolisimmat on eniten käytetyt lunchtime ja upota-aikaa Hyderabad. (Ja on pitäisi tutkia 500 koodit.)


On myös tehokas tilastollinen toiminnot:

![](./media/app-insights-analytics/025.png)


Kieli on monia houkuttelevien ominaisuuksia:

* [Suodattimen](app-insights-analytics-reference.md#where-operator) raaka app-telemetriatietojen kentän mukaan lukien mukautetut ominaisuudet ja arvot.
* [Yhdistä](app-insights-analytics-reference.md#join-operator) useita taulukoita – correlate pyynnöt sivun näkymät, riippuvuuden puhelut, poikkeukset ja log jäljittää.
* Tehokas tilastollinen [koosteet](app-insights-analytics-reference.md#aggregations).
* Vain yhtä hyvä kuin SQL, mutta helpompi monimutkaisia kyselyjä: sen sijaan, että sisäkkäisiä lauseet pipe peruskoulun kerralla tiedot toiseen.
* Tehokas ja välitön visualisointeja.







## <a name="connect-to-your-application-insights-data"></a>Sovelluksen tiedot-tietojen yhdistäminen


Avaa Analytics-sovelluksen tiedot sinua sovelluksen [Yhteenveto-sivu](app-insights-dashboards.md) : 

![Avaa portal.azure.com, Avaa sovellus havainnollistamisen resurssi ja valitse Analytics.](./media/app-insights-analytics/001.png)


## <a name="limits"></a>Rajoitukset

Tällä hetkellä kyselytulokset on rajoitettu juuri viimeisten tietojen viikon ajan.



[AZURE.INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]


## <a name="next-steps"></a>Seuraavat vaiheet


* On suositeltavaa [kielen esittely](app-insights-analytics-tour.md)aloitetaan.