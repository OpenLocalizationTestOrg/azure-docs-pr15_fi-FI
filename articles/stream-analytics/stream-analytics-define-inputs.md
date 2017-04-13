<properties
    pageTitle="Tietoyhteyden: tietojen virtauttaa peräisin tapahtuman stream | Microsoft Azure"
    description="Lisätietoja Stream Analytics nimeltä 'syötteiden' tietoyhteyden määrittämisestä. Syötteiden ovat tietovirta tapahtumien ja myös viitetiedot."
    keywords="tietovirta, tietoyhteyden, tapahtuma-muodossa"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="data-connection-learn-about-data-stream-inputs-from-events-to-stream-analytics"></a>Tietoyhteyden: stream syötteiden tapahtumien Stream Analytics tietoja

Virta Analytics tietoyhteys on tietovirta tapahtumien tietolähteestä. Tätä kutsutaan "arvon". Virta Analytics on pääosin integrointi Azure stream lähteistä tapahtumaa-toiminnossa, IoT keskittimeen ja Blob-objektien tietosäilö, joka voi olla saman tai toisen Azure tilauksesta kuin analytics-työ.

## <a name="data-input-types-data-stream-and-reference-data"></a>Tietoja input tyypit: tiedot muodossa- ja viitefunktiot tiedot
Kun tiedot on miten tietolähteeseen, on kulutettu Stream Analytics projektin ja käsitellä reaaliajassa. Syötteiden jaetaan kaksi eri tyyppiä: tietojen virtauttaa syötteiden ja tietojen syötteiden viittaus.

### <a name="data-stream-inputs"></a>Tietoja stream syötteiden
Tietoja stream on rajoittamaton sarjan tapahtumien tulossa ajan kuluessa. Virta Analytics työt on sisällettävä vähintään yksi stream tietosyöte kulutettu ja muuntaa projektin. Blob-objektien tallennustilaan tapahtuman keskittimet ja IoT keskittimet tuetaan stream syötteen tietolähteet. Tapahtuman keskittimet käytetään tapahtuman virtaa kerätään useita laitteita ja palveluja, kuten Yhteisöpalvelut tehtävän syötteet, varasto kaupan tiedot tai anturit tiedot. IoT keskittimet on optimoitu kerää tietoja yhdistettyjen laitteiden asioita Internet (IoT) skenaarioita.  Blob-objektien tallennustilaan voi käyttää syötteen tietolähteeksi ingesting stream tiedot joukkona.  

### <a name="reference-data"></a>Viitetiedot
Virta Analytics tukee toisen viitetiedot nimeltään syöte. Tämä on Aux tietoja, jotka on staattinen tai hitaasti muuttaminen ajan ja käytetään yleensä korrelaatio ja look-ups tekemistä varten. Azure-Blob-säiliö ei tällä hetkellä vain tuetut syöttölähde viitetiedot. Viittaus tietojen lähteen BLOB-objektit on oikeutettu enintään 100 Megatavua kokoinen.
Viittauksen luominen tietojen syötteiden on artikkelissa [Käytä viitetiedot](stream-analytics-use-reference-data.md)  

## <a name="create-a-data-stream-input-with-an-event-hub"></a>Luo stream tietosyöte tapahtumaa-toiminnossa

[Azure tapahtuman keskittimet](https://azure.microsoft.com/services/event-hubs/) on erittäin skaalattava tapahtuman ingestor Julkaise-tilaa. Se voi kerätä tapahtumia sekunnissa miljoonia niin, että voit käsitellä ja analysoida yhdistetyn laitteita ja sovelluksia tuottamat tiedot mahdutettavia. Se on yksi yleisimmin käytettyjä syötteiden Stream analysoinnissa. Tapahtuman keskittimet ja Stream analysoinnin yhdessä avulla asiakkaiden lopusta loppuun-ratkaisun reaaliaikainen analytics. Tapahtuman keskittimet Salli asiakkaita syötteen tapahtumien Azure reaaliajassa ja Stream Analytics töitä voit käsitellä niitä reaaliajassa. Asiakkaiden voit esimerkiksi lähettää web napsautuksella, tunnistimen lukemat, online tapahtumien poistaminen tapahtuman porttiin ja luoda Stream Analytics töitä käytettävän tapahtuman keskittimet syöttötiedot virtaa reaaliaikainen suodattaminen, yhdistäminen ja tulomomenttikorrelaatiokertoimen.

On tärkeää muistaa, että tapahtumat tulevat tapahtuman keskittimet Stream Analytics oletusarvon aikaleimaa on aikaleiman, tapahtuman saapunut tapahtumaa-toiminnossa, joka on EventEnqueuedUtcTime. [AIKALEIMA mukaan](https://msdn.microsoft.com/library/azure/dn834998.aspx) -avainsana on käytettävä käsittelemään tietoja muodossa, aikaleima käyttäminen tapahtuman tiedot.

### <a name="consumer-groups"></a>Kuluttaja-ryhmät

Jokainen Stream Analytics tapahtumaa-toiminnossa-syöte on määritettävä on oma kuluttaja-ryhmä. Kun työ on itse liittyminen tai useita syötteiden, jotkin syötteen voi lukea useita reader myötävirtaan, jotka vaikuttaa lukijat yksittäisen kuluttaja-ryhmässä määrän mukaan. Enintään 5 lukijat kuluttaja ryhmittäin kohti osion tapahtumaa-toiminnossa enimmäismäärä välttämiseksi on paras käytäntö on nimettävä kuluttaja ryhmän Stream Analytics kunkin projektin. Huomaa, että on myös enintään 20 ryhmiä tapahtumaa-toiminnossa kohden. Lisätietoja on artikkelissa [Tapahtuman keskittimet Programming Guide](../event-hubs/event-hubs-programming-guide.md).

### <a name="configure-event-hub-as-an-input-data-stream"></a>Määrittää syötteen tietovirta tapahtumaa-toiminnossa

Seuraavassa taulukossa kerrotaan kullekin ominaisuudelle tapahtuma keskittimeen Lisää kuvaus-välilehti:

| OMINAISUUDEN NIMI | KUVAUS |
|------|------|
| Syötteen Alias | Kutsumanimi, jota käytetään viittaamaan tämän syötteen työ-kysely |
| Palvelun Bus Namespace | Palvelun Bus nimitila on joukko messaging kohteiden säilö. Kun olet luonut uuden tapahtuman-keskittimeen, olet luonut palvelun Bus nimitila. |
| Tapahtuma-toiminnossa | Oman tapahtumaa-toiminnossa syötteen nimi. |
| Tapahtuman keskittimeen käytännön nimi | Jaetun access-käytäntö, joka voidaan luoda tapahtuman keskittimeen määrittäminen-välilehdessä. Kukin jaetun access-käytäntö on nimi määritetyt käyttöoikeudet, ja valintanäppäimet. |
| Tapahtuman keskittimeen käytännön avain | Jaettu pikanäppäin todennetaan palvelun Bus nimitilan käyttöoikeus. |
| Tapahtuman keskittimeen kuluttaja ryhmä (valinnainen) | Kuluttaja-ryhmä ingest tietojen tapahtuma-toiminnosta. Jos ei määritetä, Stream Analytics työt käyttämällä oletus kuluttaja-ryhmän ingest tietojen tapahtuma-toiminnosta. On suositeltavaa käyttää eri kuluttaja ryhmän Stream Analytics kunkin projektin. |
| Tapahtuman Sarjatoiminto muoto | Varmista, että kyselyissä toimi odotetulla tavalla, Stream Analytics tarvitsee tietää, mitkä Sarjatoiminto muodossa (JSON, CSV tai Avro) käytät saapuvien tietojen virtaa varten. |
| Koodaus | UTF-8 on ainoa tuettu koodauksen tiedostomuoto tällä hetkellä. |

Kun tiedot on pian saatavilla tapahtumaa-toiminnossa lähteestä, voit käyttää joitakin metatietoja kenttiin Stream Analytics kyselyssä. Seuraavassa taulukossa on lueteltu kenttiä ja niiden kuvaukset.

| OMINAISUUS | KUVAUS |
|------|------|
| EventProcessedUtcTime | Päivämäärä ja aika, tapahtuman käsitteli Stream Analytics. |
| EventEnqueuedUtcTime | Päivämäärä ja aika, tapahtuma on vastaanottanut tapahtuman keskittimet. |
| PartitionId | Syötteen sovittimen nollasta osion tunnus. |

Esimerkiksi voi kirjoittaa kyselyn, kuten jompikumpi seuraavista:

````
SELECT
    EventProcessedUtcTime,
    EventEnqueuedUtcTime,
    PartitionId
FROM Input
````

## <a name="create-an-iot-hub-data-stream-input"></a>Luo IoT keskittimeen tietojen virta-syöte

Azure Iot-toiminnossa on erittäin skaalattava Julkaise tilaa tapahtuman ingestor optimoitu IoT skenaarioita.
On tärkeää muistaa, että tapahtumat tulevat IoT keskittimet Stream Analytics oletusarvon aikaleimaa on aikaleiman, tapahtuman saapuneiden IoT-toiminnossa, joka on EventEnqueuedUtcTime. [AIKALEIMA mukaan](https://msdn.microsoft.com/library/azure/dn834998.aspx) -avainsana on käytettävä käsittelemään tietoja muodossa, aikaleima käyttäminen tapahtuman tiedot.

> [AZURE.NOTE] Käsitellään vain DeviceClient ominaisuuden lähetettäviin viesteihin.

### <a name="consumer-groups"></a>Kuluttaja-ryhmät

Jokainen Stream Analytics IoT keskittimeen syöte on määritettävä on oma kuluttaja-ryhmä. Kun työ on itse liittyminen tai useita syötteiden, jotkin syötteen voi lukea useita reader myötävirtaan, jotka vaikuttaa lukijat yksittäisen kuluttaja-ryhmässä määrän mukaan. Enintään 5 lukijat kuluttaja ryhmittäin kohti osion IoT toiminnossa enimmäismäärä välttämiseksi on paras käytäntö on nimettävä kuluttaja ryhmän Stream Analytics kunkin projektin.

### <a name="configure-iot-hub-as-an-data-stream-input"></a>Määritä IoT keskittimeen tietovirta, kirjoita

Seuraavassa taulukossa kerrotaan kullekin ominaisuudelle sen kuvaukset IoT keskittimeen syöttö-välilehdessä:

| OMINAISUUDEN NIMI | KUVAUS |
|------|------|
| Syötteen Alias | Kutsumanimi, jota käytetään viittaamaan tämän syötteen työn kysely. |
| IoT-toiminnossa | IoT-toiminnossa on joukko tekstiviesti kohteiden säilö. |
| Päätepisteen | IoT keskittimeen päätepisteen nimi. |
| Jaettu käyttö käytännön nimi | Voit antaa IoT keskittimeen jaetun käyttöoikeuskäytäntö. Kukin jaetun access-käytäntö on nimi määritetyt käyttöoikeudet, ja valintanäppäimet. |
| Jaetun käytännön pikanäppäin | Jaettu pikanäppäin todennetaan pääsy IoT-toiminnossa. |
| Kuluttaja-ryhmä (valinnainen) | Kuluttaja-ryhmä ingest tietojen IoT-toiminnosta. Jos ei määritetä, Stream Analytics työt käyttämällä oletus kuluttaja-ryhmän ingest tietojen IoT-toiminnosta. On suositeltavaa käyttää eri kuluttaja ryhmän Stream Analytics kunkin projektin. |
| Tapahtuman Sarjatoiminto muoto | Varmista, että kyselyissä toimi odotetulla tavalla, Stream Analytics tarvitsee tietää, mitkä Sarjatoiminto muodossa (JSON, CSV tai Avro) käytät saapuvien tietojen virtaa varten. |
| Koodaus | UTF-8 on ainoa tuettu koodauksen tiedostomuoto tällä hetkellä. |

Kun tiedot on pian saatavilla IoT keskittimeen lähteestä, voit käyttää joitakin metatietoja kenttiin Stream Analytics kyselyssä. Seuraavassa taulukossa on lueteltu kenttiä ja niiden kuvaukset.

| OMINAISUUS | KUVAUS |
|------|------|
| EventProcessedUtcTime | Päivämäärä ja aika, tapahtuman käsittelemistä. |
| EventEnqueuedUtcTime | Päivämäärä ja aika, tapahtuma on vastaanottanut IoT-toiminnossa. |
| PartitionId | Syötteen sovittimen nollasta osion tunnus. |
| IoTHub.MessageId | Voidaan yhdistää kaksisuuntaisesti IoT toiminnossa. |
| IoTHub.CorrelationId | Käyttää vastaukset ja palautetta IoT toiminnossa. |
| IoTHub.ConnectionDeviceId | Lähetä tämä viesti IoT keskittimeen servicebound viesteille aikaleimattu käytettävä todennetut tunnus. |
| IoTHub.ConnectionDeviceGenerationId | Todennettu laitetta, jota käytetään Lähetä tämä viesti generationId servicebound viesteille aikaleimattu IoT toiminnossa. |
| IoTHub.EnqueuedTime | Kun viesti on vastaanottanut IoT keskittimeen aika. |
| IoTHub.StreamId | Mukautetun tapahtumaominaisuus lisätyt lähettäjä-laitteella. |

## <a name="create-a-blob-storage-data-stream-input"></a>Luo Blob-tallennustilan stream tietosyöte

Suurien rakenteeton tietojen tallentamiseen pilveen skenaarioille Blob-objektien tallennustilaan on edullinen ja skaalattava ratkaisu. Tietoja [Blob-objektien](https://azure.microsoft.com/services/storage/blobs/) tallennustilaan pidetään yleensä tietoja "muut", mutta sen voi käsitellä tietoja muodossa kuin Stream Analytics. Yksi käytetty vaihtoehto Blob storage syötteiden Stream Analytics kanssa, on käsitteleminen, jossa telemetriatietojen kirjataan järjestelmästä ja täytyy jäsentää ja käsitellä poimimaan kuvaava loki.

On tärkeää Huomaa, että oletusarvoinen aikaleimaa Stream Analytics Blob storage tapahtumat on blob on viimeksi muokattu mitä *isBlobLastModifiedUtcTime*aikaleimaa. [AIKALEIMA mukaan](https://msdn.microsoft.com/library/azure/dn834998.aspx) -avainsana on käytettävä käsittelemään tietoja muodossa, aikaleima käyttäminen tapahtuman tiedot.

Huomaa, että CSV muotoiltu syötteiden **edellyttävät** tietojoukon kenttien määrittäminen otsikkorivi. Lisää otsikko-rivikenttien on oltava **yksilöllinen**.

> [AZURE.NOTE] Virta Analytics ei tue aiemmin Blob-objektien lisääminen sisältöä. Virta Analytics vain tarkastella blob, kun ja kaikki muutokset, kun tämä luku ei käsitellä valmis. Paras käytäntö on Lataa kaikki tiedot kerran ja lisää muita tapahtumia blob-säilö.

Seuraavassa taulukossa kerrotaan kullekin ominaisuudelle sen kuvaukset Blob storage syötteen-välilehdessä:

<table>
<tbody>
<tr>
<td>OMINAISUUDEN NIMI</td>
<td>KUVAUS</td>
</tr>
<tr>
<td>Syötteen Alias</td>
<td>Kutsumanimi, jota käytetään viittaamaan tämän syötteen työn kysely.</td>
</tr>
<tr>
<td>Tallennustilan tilin</td>
<td>Jossa blob-tiedostot sijaitsevat tallennustilan tilin nimi.</td>
</tr>
<tr>
<td>Tallennustilan tilin avain</td>
<td>Tallennustilan-tiliin liittyvät salausavaimen.</td>
</tr>
<tr>
<td>Tallennustilan säilö
</td>
<td>Säilöjen on tallennettu Microsoft Azure-Blob-palveluun BLOB looginen ryhmittely. Kun lataat blob Blob-palveluun, sinun on määritettävä kyseisen blob säilö.</td>
</tr>
<tr>
<td>Polun etuliite kuvion [valinnainen]</td>
<td>Käytetään etsimiseen oman BLOB määritetyn säilöön tiedostopolku.
PATH voit määrittää yhden tai useamman esiintymät seuraavat 3 muuttujat:<BR>{date}, {aika}<BR>{osion}<BR>Esimerkki 1: cluster1/lokit / {date} / {aika} / {osion}<BR>Esimerkki 2: cluster1/lokit / {date}<P>Huomaa, että "*" ei ole pathprefix sallittu arvo. Voit käyttää vain kelvollisia <a HREF="https://msdn.microsoft.com/library/azure/dd135715.aspx">Azure-blob-merkkiä</a> .</td>
</tr>
<tr>
<td>Päivämäärämuoto [valinnainen]</td>
<td>Jos etuliite polku käytetään päivämäärä-tunnuksen, voit valita päivämäärämuodon jossa tiedostojen järjestetään. Esimerkki: VVVV/KK/PP</td>
</tr>
<tr>
<td>Aikamuoto [valinnainen]</td>
<td>Jos aika-tunnuksen käytetään etuliite-polku, Määritä Aikamuoto, johon tiedostot on järjestetty. Ainoa tuettu arvo on tällä hetkellä HH.</td>
</tr>
<tr>
<td>Tapahtuman Sarjatoiminto muoto</td>
<td>Varmista, että kyselyissä toimi odotetulla tavalla, Stream Analytics tarvitsee tietää, mitkä Sarjatoiminto muodossa (JSON, CSV tai Avro) käytät saapuvien tietojen virtaa varten.</td>
</tr>
<tr>
<td>Koodaus</td>
<td>CSV-ja JSON UTF-8 on tällä hetkellä vain tuettu koodauksen muoto.</td>
</tr>
<tr>
<td>Erottimen</td>
<td>Virta Analytics tukee useita yleisiä erottimet sarjoittamista tiedot CSV-muodossa. Tuetut arvot ovat pilkku, puolipiste, tila, välilehden ja pystysuora palkki.</td>
</tr>
</tbody>
</table>

Kun tiedot on peräisin Blob storage lähteen, voit käyttää joitakin metatietokentät Stream Analytics kyselyssä. Seuraavassa taulukossa on lueteltu kenttiä ja niiden kuvaukset.

| OMINAISUUS | KUVAUS |
|------|------|
| BlobName | Syötteen blob, johon tapahtuma on peräisin nimi. |
| EventProcessedUtcTime | Päivämäärä ja aika, tapahtuman käsitteli Stream Analytics. |
| BlobLastModifiedUtcTime | Päivämäärä ja kellonaika, blob on viimeksi muokattu. |
| PartitionId | Syötteen sovittimen nollasta osion tunnus. |

Esimerkiksi voi kirjoittaa kyselyn, kuten jompikumpi seuraavista:

````
SELECT
    BlobName,
    EventProcessedUtcTime,
    BlobLastModifiedUtcTime
FROM Input
````


## <a name="get-help"></a>Ohjeiden saaminen
Saat lisäohjeita yritä Microsoftin [Azure Stream Analytics-keskustelupalsta](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Seuraavat vaiheet
Olet oppinut tietojen yhteysasetukset Azure-tietokannassa Stream Analytics-projekteille. Lisätietoja Stream Analytics on seuraavissa artikkeleissa:

- [Azure Stream Analytics käytön aloittaminen](stream-analytics-get-started.md)
- [Skaalaa Azure Stream Analytics töitä](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics kyselyn kieliohje](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics hallinta REST API-viittaus](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
