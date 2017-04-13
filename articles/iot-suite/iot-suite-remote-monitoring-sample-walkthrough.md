<properties
 pageTitle="Remote seuranta valmiiksi ratkaisu ongelmatilanteita | Microsoft Azure"
 description="Azure-IoT kuvauksen valmiiksi ratkaisu remote seuranta- ja sen arkkitehtuuri."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/17/2016"
 ms.author="dobett"/>

# <a name="remote-monitoring-preconfigured-solution-walkthrough"></a>Ratkaisu ongelmatilanteita valmiiksi Remote seuranta

## <a name="introduction"></a>Johdanto

Seuranta [valmiiksi ratkaisun] IoT Suite etäyhteyksien[ lnk-preconfigured-solutions] toteutus lopusta loppuun valvoo useita, joissa käytetään remote sijainneissa ratkaisu. Ratkaisu yhdistää keskeisiä Azure palveluita yleinen toteutuksen business-skenaarion ja voit käyttää sitä lähtökohtana oman käyttöympäristösi. Voit [mukauttaa] [ lnk-customize] ratkaisun oman yrityksen ehtojen mukaan.

Tässä artikkelissa käydään läpi joitakin tärkeimmät elementit remote seuranta-ratkaisun, jotta voit ymmärtää, miten se toimii. Tietämyksesi avulla voit:

- Ongelmien ratkaisun.
- Suunnittele mukauttamisesta ratkaisun oman tiettyjen ehtojen mukaan. 
- Suunnittele oman IoT-ratkaisun, joka käyttää Azure palveluja.

## <a name="logical-architecture"></a>Looginen arkkitehtuuri

Seuraavassa kaaviossa ympärille esimääritettyjä ratkaisu loogisia osat:

![Looginen arkkitehtuuri](media/iot-suite-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)


## <a name="simulated-devices"></a>Simuloitu laitteet

Valmiiksi määritetyt ratkaisu Simuloitu laitteen edustaa jäähdytin (esimerkiksi rakennuksen ilman conditioner tai tilojen ilma käsittely-yksikkö). Kun otat käyttöön esimääritettyjä ratkaisun, neljä Simuloitu laitteissa, joissa [Azure WebJob]-valmistella automaattisesti myös[lnk-webjobs]. Simuloitu laitteiden helppoa tarkasteleminen ilman fyysistä laitteita tarpeellisuus ratkaisu toiminnan. Ottaa todellinen fyysistä laitetta, on artikkelissa [Yhdistä laitteesi kaukosäätimen seuranta esimääritettyjä ratkaisu] [ lnk-connect-rm] opetusohjelma.

Simuloitu laitteissa voit lähettää viestin seuraavanlaisia IoT-toiminnossa:

| Viestin  | Kuvaus |
|----------|-------------|
| Käynnistys  | Kun laite käynnistyy, se lähettää **laitteen tiedot** -sanoma, joka sisältää tietoja itse taustatietokantaan. Näitä tietoja ovat laitteen tunnus, laitteen metatiedot, komentojen luettelo laitteesi tukee ja laitteen nykyiset määritykset. |
| Tavoitettavuus | Laitteen lähettää säännöllisesti **tavoitettavuuden** viestin ilmoittamaan, onko laitteen ymmärtää tunnistimen esiintymistä. |
| Telemetriatietojen | Laitteen lähettää säännöllisesti **telemetriatietojen** sanoma, joka ilmoittaa lämpötilan ja kosteuden kerätty laitteen Simuloitu anturit Simuloitu arvot. |


Simuloitu laitteiden Lähetä **laitteen tiedot** viestin laitteen seuraavat ominaisuudet:

| Ominaisuus               |  Tarkoitus |
|------------------------|--------- |
| Laitteen tunnus              | Tunnus, joka on annettu tai kun laite on luotu ratkaisu. |
| Valmistajan           | Laitteen valmistajan |
| Mallinumero           | Laitteen mallin määrä |
| Järjestysluvuksi          | Järjestysluvun laite. |
| Laitteisto               | Nykyinen versio laitteisto laitteeseen |
| Käyttöympäristö               | Laitteen Platform-arkkitehtuuri |
| Suoritin              | Suorittimen laite |
| Asennetun RAM-Muistia          | Laitteeseen loppua |
| Toiminnosta on otettu käyttöön tila      | Laitteen IoT keskittimeen state-ominaisuus |
| Luotu aika           | Laite on luotu ratkaisun aika |
| Päivittymisajankohta           | Viimeksi ominaisuudet on päivitetty laitteen |
| Leveyttä               | Laitteen leveyttä sijainti |
| Pituutta              | Laitteen pituutta sijainti |

Simulator alustaa Simuloitu laitteet, joissa on esimerkkiarvojen nämä ominaisuudet.  Aina, kun simulator alustaa Simuloitu laitteen laitteen kirjaa ennalta määritetyt metatiedot IoT toiminnossa. Huomaa, miten se korvaa metatietojen laitteen portaalissa tehdyistä päivityksistä.


Simuloitu laitteet voivat käsitellä ratkaisu Raporttinäkymät-ikkunan kautta IoT-toiminnossa lähettämät seuraavia komentoja:

| Komento                | Kuvaus                                         |
|------------------------|-----------------------------------------------------|
| PingDevice             | Lähettää _ping_ laite on toiminnassa   |
| StartTelemetry         | Käynnistää telemetriatietojen lähettäminen laitteeseen                 |
| StopTelemetry          | Lopettaa lähettämästä telemetriatietojen laite             |
| ChangeSetPointTemp     | Set-kohdan-arvon, joiden ympärillä satunnaisia tietoja luodaan |
| DiagnosticTelemetry    | Käynnistää laitteen simulator lähettämään muita telemetriatietojen-arvo (externalTemp) |
| ChangeDeviceState      | Muuttaa laajennettua tilan-ominaisuuden laitteen ja lähettää viestin laitteen tiedot laitteesta |

Laitteen komento acknowledgment ratkaisu taustatietokantaan annetaan IoT-toiminnossa kautta.

## <a name="iot-hub"></a>IoT-toiminnossa

[IoT keskittimeen] [ lnk-iothub] ingests laitteet kyselyjä pilveen lähetetyt tiedot ja mahdollistaa sen Azure Stream Analytics (ASA) työt. IoT keskittimeen lähettää myös laitteet komennot laitteen portaalin puolesta. Kunkin stream ASA projektin käyttää eri IoT keskittimeen kuluttaja ryhmässä virta viestien lukeminen laitteilla.

## <a name="azure-stream-analytics"></a>Azure Stream Analytics

Remote seurantaa ratkaisussa [Azure Stream Analytics] [ lnk-asa] (ASA) lähettää laitteen viestien vastaanottanut IoT-toiminnossa käsittely tai taustatietokantaan muita osia. Eri ASA töiden suorittaa toimintoja viestit sisällön perusteella.

**Työtä 1: laitteen tiedot** suodattaa laitteen tiedot sähköpostiviestejä saapuvan viestin virta ja lähettää ne päätepisteen tapahtumaa-toiminnossa. Laitteen lähettää laitteen tiedot viestien käynnistyksen ja **SendDeviceInfo** -komennon suorittaminen. Työn käyttää kyselyn-määritys voidaan tunnistaa viestit, **laitteen tiedot** :

```
SELECT * FROM DeviceDataStream Partition By PartitionId WHERE  ObjectType = 'DeviceInfo'
```

Työn lähettää tulos tapahtumaa-toiminnossa lisäkäsittelyä varten.

**Työn 2: säännöt** arvioi saapuvat lämpötilan ja kosteuden telemetriatietojen arvot vastaan laitteen kohti raja-arvot. Raja-arvot on määritetty käytettävissä ratkaisu Raporttinäkymät-ikkunan säännöt-editorissa. Kunkin laitteen/arvo-pari tallennetaan mukaan aikaleima-Blob-objektien, joka lukee Stream Analytics **Viittaus**tietoina. Työn vertaa mikä tahansa muu kuin tyhjä arvo vastaan laitteen määrittäminen raja-arvo. Jos se on suurempi kuin ">" ehtoa, työn tulostaa **hälytys** tapahtuman, joka ilmaisee, että kynnysarvo ylittyy ja antaa laitteen, arvo ja aikaleiman arvot. Työn käyttämällä seuraavia kyselyn kuvauksen telemetriatietojen viesteistä, joka käynnistää hälytyksen:

```
WITH AlarmsData AS 
(
SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Temperature' as ReadingType,
     Stream.Temperature as Reading,
     Ref.Temperature as Threshold,
     Ref.TemperatureRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Temperature IS NOT null AND Stream.Temperature > Ref.Temperature

UNION ALL

SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Humidity' as ReadingType,
     Stream.Humidity as Reading,
     Ref.Humidity as Threshold,
     Ref.HumidityRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Humidity IS NOT null AND Stream.Humidity > Ref.Humidity
)

SELECT *
INTO DeviceRulesMonitoring
FROM AlarmsData

SELECT *
INTO DeviceRulesHub
FROM AlarmsData
```

Työn lähettää sen tulosteen tapahtumaa-toiminnossa lisäkäsittelyä varten, ja tallentaa tiedot jokaisen ilmoituksen Blob-objektien tallennustilaan-kohtaa, johon ratkaisu Raporttinäkymät-ikkunan voivat lukea ilmoituksen tietoja.

**Työn 3: Telemetriatietojen** toimii saapuvien laitteen telemetriatietojen virta kahdella tavalla. Ensimmäinen lähettää viestit telemetriatietojen laitteet pitkään tallennustilan jatkuva-blob-säiliö. Toinen laskee kosteus keskiarvo, pienimpien ja suurimpien arvojen viiden minuutin liukuva ikkunan päälle ja lähettää nämä tiedot Blob-objektien tallennustilaan. Ratkaisu Raporttinäkymät-ikkunan lukee telemetriatietojen tiedot Blob-objektien tallennustilaan täytä kaaviot. Työn käyttää seuraavia kyselymääritys:

```
WITH 
    [StreamData]
AS (
    SELECT
        *
    FROM [IoTHubStream]
    WHERE
        [ObjectType] IS NULL -- Filter out device info and command responses
) 

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    Temperature,
    Humidity,
    ExternalTemperature,
    EventProcessedUtcTime,
    PartitionId,
    EventEnqueuedUtcTime,
    * 
INTO
    [Telemetry]
FROM
    [StreamData]

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    AVG (Humidity) AS [AverageHumidity],
    MIN(Humidity) AS [MinimumHumidity],
    MAX(Humidity) AS [MaxHumidity],
    5.0 AS TimeframeMinutes 
INTO
    [TelemetrySummary]
FROM [StreamData]
WHERE
    [Humidity] IS NOT NULL
GROUP BY
    IoTHub.ConnectionDeviceId,
    SlidingWindow (mi, 5)
```

## <a name="event-hubs"></a>Tapahtuman keskittimet

**Laitteen tiedot** ja **säännöt** ASA työt tulosteen tietonsa tapahtuman porttiin välittäminen luotettavasti voin **Tapahtuman suoritin** WebJob käynnissä.

## <a name="azure-storage"></a>Azure-tallennustilan

Ratkaisu käyttää Azure-blob-säiliö jatkuvat kaikki raaka- ja tiivistetty telemetriatietojen laitteilla ratkaisun tiedot. Koontinäytön lukee telemetriatietojen tiedot Blob-objektien tallennustilaan täytä kaaviot. Näytä ilmoitukset-koontinäytön lukee tiedot Blob-objektien tallennustilaan tallentavan kun telemetriatietojen arvot ylittää määritetyn raja-arvot. Ratkaisu käyttää myös Blob-objektien tallennustilaan tallentavan määrität koontinäytön raja-arvot.

## <a name="webjobs"></a>WebJobs

Lisäksi isännöinnin laitteen opetuksessa käytettävät niitä ratkaisun WebJobs ylläpitää **Tapahtuman suoritin** käynnissä Azure-WebJob kyseisen kahvoja laitteen tiedot viestit ja vastaukset komento. Tiedostossa käytetään:

- Laitteen tiedot viestit (tallennettu DocumentDB tietokannan) laitteen rekisterin päivittäminen laitteen tiedot.
- Komennon vastauksen viestien laitteen komento historia (tallennettu DocumentDB tietokannan).

## <a name="documentdb"></a>DocumentDB

Ratkaisu käyttää DocumentDB tietokannan ratkaisun liitettyjen laitteiden tietojen tallentamiseen. Näitä tietoja ovat laitteen metatiedot ja laitteiden koontinäytöstä lähetetyt komennot historia.

## <a name="web-apps"></a>Web Apps-sovelluksista

### <a name="remote-monitoring-dashboard"></a>Remote seurantaa koontinäyttö
WWW-sovelluksessa sivu käyttää telemetriatietojen tietojen visualisoimiseen PowerBI javascript-ohjausobjekteja (katso [PowerBI tehosteiden repo](https://www.github.com/Microsoft/PowerBI-visuals)) laitteilla. Ratkaisu käyttää ASA telemetriatietojen työn blob storage kirjoittaa telemetriatietojen.


### <a name="device-administration-portal"></a>Laitteen hallinta-portaalissa

Web-sovelluksen avulla voit:

- Valmistele uusi laite. Tämä toiminto määrittää laitteen yksilöivä tunnus ja luo todennus-näppäintä. Laitteen tiedot kirjoitetaan IoT keskittimeen identity-rekisterin ja ratkaisu-kohtaisia DocumentDB tietokanta.
- Laitteen ominaisuuksien hallinta. Tämä toiminto on aiemmin ominaisuuksien tarkasteleminen ja päivittäminen uudet ominaisuudet.
- Lähettää komentoja laitteeseen.
- Näytä laitteen komennon historia.
- Ota käyttöön ja poistaminen käytöstä laitteet.

## <a name="next-steps"></a>Seuraavat vaiheet

TechNet blogimerkintöjen on enemmän tietoja remote seurantaa esimääritettyjä ratkaisu:

- [IoT Suite - kohdassa Näytä lisäasetukset - Remote seuranta](http://social.technet.microsoft.com/wiki/contents/articles/32941.iot-suite-under-the-hood-remote-monitoring.aspx)
- [IoT Suite - Remote seuranta - Live-ja luo Simuloitu lisääminen](http://social.technet.microsoft.com/wiki/contents/articles/32975.iot-suite-remote-monitoring-adding-live-and-simulated-devices.aspx)

Voit jatkaa aloittaminen IoT Suite lukemalla on seuraavissa artikkeleissa:

- [Laitteen yhdistäminen remote seurantaa esimääritettyjä ratkaisu][lnk-connect-rm]
- [Azureiotsuite.com oikeudet][lnk-permissions]

[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-iothub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-webjobs]: https://azure.microsoft.com/documentation/articles/websites-webjobs-resources/
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md