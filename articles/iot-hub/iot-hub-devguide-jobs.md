<properties
 pageTitle="Sovelluskehittäjän opas - työt | Microsoft Azure"
 description="Azure IoT keskittimeen Sovelluskehittäjän opas - ajoituksen töitä useilla eri laitteilla yhdistetty oman keskittimeen"
 services="iot-hub"
 documentationCenter=".net"
 authors="juanjperez"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="juanpere"/>

# <a name="schedule-jobs-on-multiple-devices-preview"></a>Ajoita työt useilla eri laitteilla (ennakkoversio)

## <a name="overview"></a>Yleiskatsaus

Edellisen artikkeleissa kuvatulla Azure IoT keskittimeen mahdollistaa rakenneosien määrä ([laitteen sekä ominaisuudet ja tunnisteita] [ lnk-twin-devguide] ja [cloud laitteen menetelmiä][lnk-dev-methods]).  Yleensä IoT uudelleen sovellusten käyttöön laitteen Järjestelmänvalvojat ja operaattorit ja käsitellä IoT laitteet joukkona ja ajoitetun kerrallaan.  Töiden kapseloida laitteen sekä päivitykset ja C2D menetelmiä vastaan laitteiden suorittamista aikataulun kerrallaan.  Esimerkiksi operaattori käyttäisi uudelleen sovellus, jossa aloittaa ja seurata projektin käynnistämään laitteiden muodostaminen 43 ja kerroksen 3 kerrallaan, joka ei ole häiritä rakennuksen toimintoja.

### <a name="when-to-use"></a>Käyttäminen

Harkitse käyttämällä työt, kun: ratkaisun Edellinen lopettaa tarvitsee ajoittaa ja seurata edistymistä tietylle laitteen seuraavat toiminnot:

- Päivitä laitteen sekä haluamasi ominaisuudet
- Päivitä laitteen sekä tunnisteet
- Käynnistää C2D menetelmät

## <a name="job-lifecycle"></a>Projektin elinkaari

Työt aloittamia ratkaisu takaisin loppuun ja ylläpitämä IoT toiminnossa.  Voit aloittaa työn – palvelu osoittava URI (`{iot hub}/jobs/v2/{device id}/methods/<jobID>?api-version=2016-09-30-preview`) ja kysely suoritetaan työ, – palvelu osoittava URI edistymisen (`{iot hub}/jobs/v2/<jobId>?api-version=2016-09-30-preview`).  Kun työ on aloitettu, kyselyt töiden käyttöön uudelleen sovelluksen Päivitä tila töitä.

> [AZURE.NOTE] Kun aloitat työn, ominaisuuden nimet ja arvot voivat sisältää US-ASCII tulostettavan aakkosnumeerinen, ellei jokin seuraavista määrittäminen: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.

## <a name="reference-topics"></a>Viittaus aiheet:

Viittaus seuraavissa ohjeaiheissa on lisätietoja työt.

## <a name="jobs-to-execute-direct-methods"></a>Töiden suorittaminen suoraan menetelmät

Seuraavassa on tietoja HTTP 1.1 pyynnön suorittaminen [Suora menetelmä] [ lnk-dev-methods] -laitteiden työn avulla:

    ```
    PUT /jobs/v2/<jobId>?api-version=2016-09-30-preview
    
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleDirectRequest', 
        cloudToDeviceMethod: {
            methodName: '<methodName>',
            payload: <payload>,                 
            timeoutInSeconds: methodTimeoutInSeconds 
        },
        queryCondition: '<queryOrDevices>', // if the queryOrDevices parameter is a string
        deviceIds: '<queryOrDevices>',      // if the queryOrDevices parameter is an array
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        
    }
    ```
    
## <a name="jobs-to-update-device-twin-properties"></a>Töiden laitteen sekä ominaisuuksien päivittäminen

Seuraavassa on tietoja HTTP 1.1 pyynnön laitteen sekä ominaisuuksien käyttäminen työn päivittäminen:

    ```
    PUT /jobs/v2/<jobId>?api-version=2016-09-30-preview
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleTwinUpdate', 
        updateTwin: <patch>                 // Valid JSON object
        queryCondition: '<queryOrDevices>', // if the queryOrDevices parameter is a string
        deviceIds: '<queryOrDevices>',      // if the queryOrDevices parameter is an array
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        // format TBD
    }
    ```

## <a name="querying-for-progress-on-jobs"></a>Edistymisen töihin kyselyistä

Seuraavassa on HTTP 1.1 pyynnön tietoja [kyselyistä työt][lnk-query]:

    ```
    GET /jobs/v2/query?api-version=2016-09-30-preview[&jobType=<jobType>][&jobStatus=<jobStatus>][&pageSize=<pageSize>][&continuationToken=<continuationToken>]
    
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>
    ```
    
ContinuationToken on tarkoitettu vastaus.  

## <a name="jobs-properties"></a>Töiden ominaisuudet

Seuraavassa on luettelo ominaisuudet ja niiden kuvaukset, jota voidaan käyttää, kun kysely suoritetaan työt tai työn tulokset.

| Ominaisuus | Kuvaus |
| -------------- | -----------------|
| **Työn tunnus** | Sovelluksen annettu tunnus projektille. |
| **Aloitusajan** | Sovelluksen annettu työn alkamisaika (ISO-8601). |
| **Lopetusaika** | IoT keskittimeen antaa päivämäärä (ISO-8601) kun työ on valmis. Kelvollinen vasta, kun työ saavuttaa "Valmis"-tilan. | 
| **tyyppi** | Töiden tyypit: |
| | **scheduledUpdateTwin**: työn päivittämiseen sekä haluamasi ominaisuudet tai tunnisteet. |
| | **scheduledDeviceMethod**: työn sekä joukko laitteen menetelmän avulla. |
| **tila** | Työn nykyinen tila. Mahdolliset arvot tila: |
| | **odottaa** : niin ja odottaa kerätään työ-palvelu. |
| | **aikataulun mukainen** : suunniteltu aika tulevaisuudessa. |
| | **käyttöjärjestelmä** : aktiivinen työ. |
| | **peruuttaa** : työ on peruutettu. |
| | **epäonnistui** : työn epäonnistui. |
| | **Valmis** : työ on valmis. |
| **deviceJobStatistics** | Projektin suorittaminen tilastotietoja. |

Esikatselussa aikana deviceJobStatistics objekti on käytettävissä vain, kun työ on valmis.

| Ominaisuus | Kuvaus |
| -------------- | -----------------|
| **deviceJobStatistics.deviceCount** | Työ, mikä numero. |
| **deviceJobStatistics.failedCount** | Useita laitteita, jossa työ epäonnistui. |
| **deviceJobStatistics.succeededCount** | Useita laitteita, joissa työn onnistui. |
| **deviceJobStatistics.runningCount** | Laitteet, jotka ovat parhaillaan käynnissä työn määrä. |
| **deviceJobStatistics.pendingCount** | Laitteet, jotka odottavat työn määrä. |


### <a name="additional-reference-material"></a>Lisää aineistoa

Kehittäjän opas muiden viittauksen aiheet ovat seuraavat:

- [IoT keskittimeen päätepisteet] [ lnk-endpoints] kuvataan eri päätepisteet, jotka kunkin IoT-toiminnossa paljastaa avaamiseen runtime ja hallintaa varten.
- [Rajoitusten ja kiintiöiden] [ lnk-quotas] kuvataan kiintiön, jotka koskevat IoT keskittimeen-palveluun ja rajoittava toiminnan toiminta palvelua käytettäessä.
- [IoT keskittimeen laite- ja palveluluettelon SDK: T] [ lnk-sdks] näyttää eri kielellä SDK: T voit Käytä tätä, kun laite ja palvelun sovelluksista, jotka toimivat IoT keskittimeen ja kehittää.
- [Kyselyn kielen twins, menetelmistä ja töiden] [ lnk-query] kuvataan kyselykieltä, voit hakea tietoja IoT-toiminnosta laitteen twins, menetelmät ja projektit.
- [IoT keskittimeen MQTT tuki] [ lnk-devguide-mqtt] on lisätietoja IoT keskittimeen tuki MQTT-protokollaa.

## <a name="next-steps"></a>Seuraavat vaiheet

Jos haluat kokeilla osa käsitteistä, joka on kuvattu tämän artikkelin, voit tarkastella käyttämällä seuraavia IoT keskittimeen opetusohjelma:

- [Aikataulun ja Lähetä töitä][lnk-jobs-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-jobs-tutorial]: iot-hub-schedule-jobs.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-devguide]: iot-hub-devguide-device-twins.md
