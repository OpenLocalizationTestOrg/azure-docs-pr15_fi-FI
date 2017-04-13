<properties 
    pageTitle="Sovelluksen tiedot-tietomalli" 
    description="Tässä artikkelissa kuvataan ominaisuudet viety JSON jatkuva vienti ja käyttää suodattimina." 
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
    ms.date="03/21/2016" 
    ms.author="awills"/>

# <a name="application-insights-export-data-model"></a>Sovelluksen tiedot Vie-tietomalli

Seuraavassa taulukossa on lähetetty [Hakemuksen tiedot](app-insights-overview.md) SDK: T-portaaliin telemetriatietojen ominaisuudet. Näkyviin tulee tietojen tulosteen [Jatkuva Vie](app-insights-export-telemetry.md)nämä ominaisuudet.
Ne näkyvät myös ominaisuussuodattimia [Metrijärjestelmä Explorerissa](app-insights-metrics-explorer.md) ja [Diagnostiikan haku](app-insights-diagnostic-search.md).

Määritä Points to Huomautus

* `[0]`näissä taulukoissa ilmaisee vaiheessa polku, joissa voit lisätä indeksin; mutta ei aina 0.
* Aika-kestot ovat lähimpään kymmenesosaan, joten 10000000 microsecond == 1 sekunti.
* Päivämäärät ja kellonajat ovat UTC-ajan ja annetaan ISO-muodossa`yyyy-MM-DDThh:mm:ss.sssZ`

On useita [objektit](app-insights-export-telemetry.md#code-samples) , jotka kuvaavat kuinka niitä käytetään.



## <a name="example"></a>Esimerkki

    // A server report about an HTTP request
    {
    "request": [ 
      {
        "urlData": { // derived from 'url'
          "host": "contoso.org",
          "base": "/",
          "hashTag": "" 
        },
        "responseCode": 200, // Sent to client
        "success": true, // Default == responseCode<400
        // Request id becomes the operation id of child events 
        "id": "fCOhCdCnZ9I=",  
        "name": "GET Home/Index",
        "count": 1, // 100% / sampling rate
        "durationMetric": {
          "value": 1046804.0, // 10000000 == 1 second
          // Currently the following fields are redundant:
          "count": 1.0,
          "min": 1046804.0,
          "max": 1046804.0,
          "stdDev": 0.0,
          "sampledValue": 1046804.0
        },
        "url": "/"
      }
    ],
    "internal": {
      "data": {
        "id": "7f156650-ef4c-11e5-8453-3f984b167d05",
        "documentVersion": "1.61"
      }
    },
    "context": {
      "device": { // client browser
        "type": "PC",
        "screenResolution": { },
        "roleInstance": "WFWEB14B.fabrikam.net"
      },
      "application": { },
      "location": { // derived from client ip
        "continent": "North America",
        "country": "United States",
        // last octagon is anonymized to 0 at portal:
        "clientip": "168.62.177.0", 
        "province": "",
        "city": ""
      },
      "data": {
        "isSynthetic": true, // we identified source as a bot
        // percentage of generated data sent to portal:
        "samplingRate": 100.0, 
        "eventTime": "2016-03-21T10:05:45.7334717Z" // UTC
      },
      "user": {
        "isAuthenticated": false,
        "anonId": "us-tx-sn1-azr", // bot agent id
        "anonAcquisitionDate": "0001-01-01T00:00:00Z",
        "authAcquisitionDate": "0001-01-01T00:00:00Z",
        "accountAcquisitionDate": "0001-01-01T00:00:00Z"
      },
      "operation": {
        "id": "fCOhCdCnZ9I=",
        "parentId": "fCOhCdCnZ9I=",
        "name": "GET Home/Index"
      },
      "cloud": { },
      "serverDevice": { },
      "custom": { // set by custom fields of track calls
        "dimensions": [ ],
        "metrics": [ ]
      },
      "session": {
        "id": "65504c10-44a6-489e-b9dc-94184eb00d86",
        "isFirst": true
      }
    }
  }




## <a name="context"></a>Konteksti

Telemetriatietojen kaikenlaisia mukana konteksti-osassa. Kaikki kentät lähetetään jokaisen arvopistettä.



|Polku|Tyyppi|Huomautuksia|
|---|---|---|
| context.Custom.Dimensions [0]  | objektin]  | Mukautetut ominaisuudet-parametrin määrittämän merkkijonon avain-arvo-parit. Avaimen enimmäispituus 100, arvot enimmäispituus vähintään 1 024. Yli 100 yksilölliset arvot-ominaisuuden voi etsiä, mutta ei voi käyttää Segmentointi. Enintään 200 näppäimet ikey kohden.  |
| context.Custom.Metrics [0]  | objektin]  | Määritä mukautettu mitat-parametrin ja TrackMetrics avain-arvo-parit. Avaimen enimmäispituus 100 arvot voivat olla numeerinen. |
| context.data.eventTime | merkkijono | UTC-AIKA |
| context.data.isSynthetic | Totuusarvo | Pyyntö tulee näkyviin tulevat robotti tai web-testi. |
| context.data.samplingRate | numero | Telemetriatietojen SDK-paketissa, joka lähetetään portaalin luoma prosenttiosuus. Alueen 0,0 100.0.|
| context.Device | objektin | Asiakas-laite |
| context.Device.Browser | merkkijono | IE Chrome... |
| context.device.browserVersion | merkkijono | Chrome 48.0... |
| context.device.deviceModel | merkkijono | |
| context.device.deviceName | merkkijono | |
| context.Device.ID | merkkijono | |
| context.Device.Locale | merkkijono | en-GB, de-DE... |
| context.Device.Network | merkkijono | |
| context.device.oemName | merkkijono | |
| context.device.osVersion | merkkijono | Host OS |
| context.device.roleInstance | merkkijono | Server host tunnus |
| context.device.roleName | merkkijono | |
| context.Device.Type | merkkijono | PC-selainta... |
| context.Location | objektin | Johdettu clientip. |
| context.location.City | merkkijono | Johdettu clientip, jos tiedossa  |
| context.location.clientip | merkkijono | Viimeinen kahdeksankulmio on anonymized 0. |
| context.location.Continent | merkkijono | |
| context.location.Country | merkkijono | |
| context.location.province | merkkijono | Osavaltio tai provinssi |
| context.Operation.ID | merkkijono | Kohteet, joilla on sama toimintotunnus on näkyy liittyvät kohteet-portaalissa. Yleensä pyynnön tunnus. |
| context.Operation.name | merkkijono | URL-osoite tai pyynnön nimi |
| context.operation.parentId | merkkijono | Sallii sisäkkäisiä liittyvät kohteet. |
| context.Session.ID | merkkijono | Toimintojen samasta lähteestä ryhmän tunnus. Toiminnon ilman 30 minuutin ajan signaalit istunnon loppuun. |
| context.session.isFirst | Totuusarvo | |
| context.user.accountAcquisitionDate | merkkijono | |
| context.user.anonAcquisitionDate | merkkijono | |
| context.user.anonId | merkkijono | |
| context.user.authAcquisitionDate | merkkijono | [Todennetun käyttäjän](app-insights-api-custom-events-metrics.md#authenticated-users) |
| context.user.isAuthenticated | Totuusarvo | |
| internal.data.documentVersion | merkkijono | |
| Internal.Data.ID | merkkijono | |



## <a name="events"></a>Tapahtumat

Mukautetut tapahtumat [TrackEvent()](app-insights-api-custom-events-metrics.md#track-event)luoma. 


|Polku|Tyyppi|Huomautuksia|
|---|---|---|
| [0] tapahtumien määrä | kokonaisluku | 100 / ([Esimerkkejä](app-insights-sampling.md) kurssi). Esimerkki 4 =&gt; 25 %. |
| tapahtuman [0] nimi | merkkijono | Tapahtuman nimi.  Enimmäispituus 250. |
| tapahtuman [0] URL-osoite | merkkijono | |
| tapahtuman [0] urlData.base | merkkijono | |
| tapahtuman [0] urlData.host | merkkijono | |

## <a name="exceptions"></a>Poikkeukset

Raporttien [Poikkeukset](app-insights-asp-net-exceptions.md) -palvelimessa ja selaimessa. 


|Polku|Tyyppi|Huomautuksia|
|---|---|---|
| basicException [0] kokoonpanon | merkkijono | |
| Laske basicException [0] | kokonaisluku | 100 / ([Esimerkkejä](app-insights-sampling.md) kurssi). Esimerkki 4 =&gt; 25 %. |
| basicException [0] exceptionGroup | merkkijono | |
| basicException [0] exceptionType | merkkijono | |merkkijono | |
| basicException [0] failedUserCodeMethod | merkkijono | |
| basicException [0] failedUserCodeAssembly | merkkijono | |
| basicException [0] handledAt | merkkijono | |
| basicException [0] hasFullStack | Totuusarvo | |
| basicException [0] tunnus | merkkijono | |
| menetelmä basicException [0] | merkkijono | |
| basicException [0]-viesti | merkkijono | Poikkeusviesti. Enimmäispituus 10k.|
| basicException [0] outerExceptionMessage | merkkijono | |
| basicException [0] outerExceptionThrownAtAssembly | merkkijono | |
| basicException [0] outerExceptionThrownAtMethod | merkkijono | |
| basicException [0] outerExceptionType | merkkijono | |
| basicException [0] outerId | merkkijono | |
| basicException [0] parsedStack [0] kokoonpanon | merkkijono | |
| basicException [0] parsedStack [0] tiedostonimi | merkkijono | |
| basicException [0] [0] parsedStack taso | kokonaisluku | |
| basicException [0] parsedStack [0]-rivi | kokonaisluku | |
| basicException [0] [0] parsedStack menetelmä | merkkijono | |
| basicException [0] pino | merkkijono | Enimmäispituus 10k|
| typeName basicException [0] | merkkijono | |



## <a name="trace-messages"></a>Viestien seuranta

Lähetetty [TrackTrace](app-insights-api-custom-events-metrics.md#track-trace)ja [sovittimet lokiin kirjaaminen](app-insights-asp-net-trace-logs.md).


|Polku|Tyyppi|Huomautuksia|
|---|---|---|
| viestin [0] loggerName | merkkijono ||
| viestin [0] Parametrit | merkkijono ||
| viestin [0] raaka | merkkijono | Log-viestin enimmäispituus 10k. |
| viestin [0] severityLevel | merkkijono | |



## <a name="remote-dependency"></a>Remote riippuvuus

Lähettää TrackDependency. Raportin suorituskyky ja palvelimen [riippuvuudet puhelut](app-insights-asp-net-dependencies.md) käyttö ja AJAX soittaa selaimessa.

|Polku|Tyyppi|Huomautuksia|
|---|---|---|
| remoteDependency [0] asynkroninen | Totuusarvo | |
| remoteDependency [0] baseName | merkkijono |  |
| remoteDependency [0] Komennon_nimi | merkkijono | Esimerkiksi "aloitus/hakemisto" |
| Laske remoteDependency [0] | kokonaisluku | 100 / ([Esimerkkejä](app-insights-sampling.md) kurssi). Esimerkki 4 =&gt; 25 %. |
| remoteDependency [0] dependencyTypeName | merkkijono | HTTP-SQL... |
| remoteDependency [0] durationMetric.value | numero | Puhelun loppuun riippuvuudet vastaus aika |
| remoteDependency [0] tunnus | merkkijono | |
| remoteDependency [0] nimi | merkkijono | URL-osoite. Enimmäispituus 250.|
| remoteDependency [0] resultCode | merkkijono | Valitse HTTP-riippuvuus |
| remoteDependency [0] onnistui | Totuusarvo | |
| remoteDependency [0] tyyppi | merkkijono | HTTP-Sql... |
| remoteDependency [0] URL-osoite | merkkijono |  Enimmäispituus 2000 |
| remoteDependency [0] urlData.base | merkkijono | Enimmäispituus 2000 |
| remoteDependency [0] urlData.hashTag | merkkijono | |
| remoteDependency [0] urlData.host | merkkijono | Enimmäispituus 200 |


## <a name="requests"></a>Palvelupyynnöt

Lähettää [TrackRequest](app-insights-api-custom-events-metrics.md#track-request). Vakio moduulit tällä komennolla voit raporttien palvelimen vastauksen aika palvelimen mitattuna. 


|Polku|Tyyppi|Huomautuksia|
|---|---|---|
| pyyntö [0] määrä | kokonaisluku | 100 / ([Esimerkkejä](app-insights-sampling.md) kurssi). Esimerkki: 4 =&gt; 25 %. |
| pyyntö [0] durationMetric.value | numero | Pyyntö saapuvista vastauksen ajan. 1e7 == 1s |
| pyynnön [0] tunnus | merkkijono | Toiminnon tunnus |
| pyyntö [0] nimi | merkkijono | HAE ja kirjaa + perus URL-osoite.  Enimmäispituus 250 |
| pyyntö [0] responseCode | kokonaisluku | HTTP-vastaus asiakkaalle lähetetty |
| pyyntö [0] onnistui | Totuusarvo | Oletusarvoinen == (responseCode &lt; 400) |
| pyyntö [0] URL-osoite | merkkijono | Lukuun ottamatta Host (isäntä) |
| pyyntö [0] urlData.base | merkkijono | |
| pyyntö [0] urlData.hashTag | merkkijono |  |
| pyyntö [0] urlData.host | merkkijono | |


## <a name="page-view-performance"></a>Sivun katselu-suorituskyky

Lähettää selaimessa. Mittaa aikaa käsitellä sivun käyttäjän aloitetaan pyynnön näyttää valmiina (lukuun ottamatta asynkroninen AJAX kutsut).

Konteksti-arvojen näyttäminen asiakkaan käyttöjärjestelmän ja selaimen versio. 


|Polku|Tyyppi|Huomautuksia|
|---|---|---|
| clientPerformance [0] clientProcess.value | kokonaisluku | Ajan vastaanottaminen HTML-koodin näyttäminen sivun loppuun. |
| clientPerformance [0] nimi | merkkijono | |
| clientPerformance [0] networkConnection.value | kokonaisluku | Aika, muodosta verkkoyhteys. |
| clientPerformance [0] receiveRequest.value | kokonaisluku | Lopusta lähettää pyynnön vastaanottaminen HTML vastaukseen aika. |
| clientPerformance [0] sendRequest.value | kokonaisluku | Aika keskimäärin HTTP-Lähetyspyyntö. |
| clientPerformance [0] total.value | kokonaisluku | Aika aloittamasta näyttäminen sivun Lähetä pyyntö. |
| clientPerformance [0] URL-osoite | merkkijono | Pyyntö URL-osoite |
| clientPerformance [0] urlData.base | merkkijono | |
| clientPerformance [0] urlData.hashTag | merkkijono | |
| clientPerformance [0] urlData.host | merkkijono | |
| clientPerformance [0] urlData.protocol | merkkijono | |

## <a name="page-views"></a>Page Views

TrackPageView() tai [stopTrackPage](app-insights-api-custom-events-metrics.md#page-view) lähettää

|Polku|Tyyppi|Huomautuksia|
|---|---|---|
| Näytä [0] määrä | kokonaisluku | 100 / ([Esimerkkejä](app-insights-sampling.md) kurssi). Esimerkki 4 =&gt; 25 %. |
| Näytä [0] durationMetric.value | kokonaisluku | Voit myös määrittää trackPageView() eikä startTrackPage() - arvon stopTrackPage(). Ei ole sama kuin clientPerformance arvot. |
| [0] näkymän nimi | merkkijono | Sivun otsikon.  Enimmäispituus 250 |
| Näytä [0] URL-osoite | merkkijono | |
| Näytä [0] urlData.base | merkkijono | |
| Näytä [0] urlData.hashTag | merkkijono | |
| Näytä [0] urlData.host | merkkijono | |



## <a name="availability"></a>Käytettävyys

Raporttien [käytettävyys web testit](app-insights-monitor-web-app-availability.md).

|Polku|Tyyppi|Huomautuksia|
|---|---|---|
| käytettävyys [0] availabilityMetric.name | merkkijono | käytettävyys |
| käytettävyys [0] availabilityMetric.value | numero |1.0 tai 0,0 |
| käytettävyys [0] määrä | kokonaisluku | 100 / ([Esimerkkejä](app-insights-sampling.md) kurssi). Esimerkki 4 =&gt; 25 %. |
| käytettävyys [0] dataSizeMetric.name | merkkijono | |
| käytettävyys [0] dataSizeMetric.value | kokonaisluku | |
| käytettävyys [0] durationMetric.name | merkkijono | |
| käytettävyys [0] durationMetric.value | numero | Testin kesto. 1e7 == 1s |
| käytettävyys [0]-viesti | merkkijono | Diagnostiikan virhe |
| käytettävyys [0] tulos | merkkijono | Vaiheen/epäonnistuvat |
| käytettävyys [0] runLocation | merkkijono | Http-req GEO lähde |
| käytettävyys [0] testName | merkkijono | |
| käytettävyys [0] testRunId | merkkijono | |
| käytettävyys [0] testTimestamp | merkkijono | |




## <a name="metrics"></a>Arvot

Luo TrackMetric().

Metrijärjestelmän arvo löytyy context.custom.metrics[0]

Esimerkki:

    {
     "metric": [ ],
     "context": {
     ...
     "custom": {
        "dimensions": [
          { "ProcessId": "4068" }
        ],
        "metrics": [
          {
            "dispatchRate": {
              "value": 0.001295,
              "count": 1.0,
              "min": 0.001295,
              "max": 0.001295,
              "stdDev": 0.0,
              "sampledValue": 0.001295,
              "sum": 0.001295
            }
          }
         } ] }
    }

## <a name="about-metric-values"></a>Tietoja metrisillä arvoista

Metrijärjestelmän arvot sekä metrisillä raporteissa ja muualla, raportoidaan vakio objektin rakenteen kanssa. Esimerkki:

      "durationMetric": {
        "name": "contoso.org",
        "type": "Aggregation",
        "value": 468.71603053650279,
        "count": 1.0,
        "min": 468.71603053650279,
        "max": 468.71603053650279,
        "stdDev": 0.0,
        "sampledValue": 468.71603053650279
      }

Tällä hetkellä – vaikka tämä voi muuttua myöhemmin - kaikki arvot raportoitu vakio SDK-moduulit `count==1` ja vain `name` ja `value` kentistä on hyötyä. Vain kirjainkokoa kohtaa, johon ne on eri olisi, jos kirjoitat TrackMetric-puhelujen joukko, jonka olet muut parametrit. 

Muut kentät tarkoituksena on sallimaan arvot Koostettavan SDK-paketissa, voit pienentää liikenne-portaaliin. Voi esimerkiksi keskimääräinen useita peräkkäiset lukemat ennen lähettämistä kunkin metrisillä raportin. Sitten voit laskea min, max, keskihajonta ja koostearvon (summan tai keskiarvon) ja määrittäminen laskeminen raporttia vastaavan lukemat määrän. 

Edellä olevassa taulukossa on on pois harvoin käytetyt kentät määrä, min, max, stdDev ja sampledValue.

Valmiiksi Keskusportaalisivustoihin arvot sijaan voit käyttää [Esimerkkejä](app-insights-sampling.md) , jos haluat pienentää äänenvoimakkuutta telemetriatietojen.


### <a name="durations"></a>Kestot

Ellei toisin mainita kestot esiintyvät lähimpään kymmenesosaan, microsecond, niin, että 10000000.0 tarkoittaa sekunnin.



## <a name="see-also"></a>Katso myös

* [Hakemuksen tiedot](app-insights-overview.md) 
* [Jatkuva vienti](app-insights-export-telemetry.md)
* [MALLIKOODEJA](app-insights-export-telemetry.md#code-samples)


