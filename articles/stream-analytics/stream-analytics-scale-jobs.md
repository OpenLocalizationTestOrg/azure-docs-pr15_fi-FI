<properties
    pageTitle="Skaalaa Stream Analytics työt niin, että siirtonopeuden | Microsoft Azure"
    description="Lue, miten skaalata Stream Analytics työt syötteen osioiden määrittäminen, säätäminen kyselymääritys ja miten työn streaming yksiköt."
    keywords="Data streaming-tietovirta tietojen käsittely-paranna analytics"
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

# <a name="scale-azure-stream-analytics-jobs-to-increase-stream-data-processing-throughput"></a>Skaalaa Azure Stream Analytics työt stream tietojen käsittely siirtonopeuden lisäämiseksi

Opettele paranna analytics työt ja laskea *streaming yksiköt* Stream Analytics miten Stream Analytics työt syötteen osioiden määrittäminen, säätäminen analytics kyselyn kuvauksen ja määrittämällä työn streaming yksiköt. 

## <a name="what-are-the-parts-of-a-stream-analytics-job"></a>Mitkä ovat Stream Analytics työn osat?
Virta Analytics työn määrityksen sisältää syötteiden, kyselyssä ja tulos. Syötteiden ovat missä työn lukee tietovirta, kysely käytetään muuntamiseen syötteen tietovirta ja tulos on, jossa työn lähettää työn tulokset.  

Työn tietojen streaming edellyttää vähintään yksi lähde. Virta-syötteen tietolähteen voidaan tallentaa Azure palvelun Bus tapahtuman toiminnossa tai Azure-Blob-objektien tallennustilaan. Lisätietoja on artikkelissa [Johdanto Azure Stream Analytics](stream-analytics-introduction.md) ja [Aloita Azure Stream Analytics](stream-analytics-get-started.md).

## <a name="configuring-streaming-units"></a>Streaming yksiköiden määrittäminen
Streaming yksiköt (SUs) edustavat resursseja ja tietojenkäsittely tarvittavia Azure Stream Analytics-työ power. SUs lisäämistapaa kuvaavat suhteellinen tapahtuman käsittelyn kapasiteetin suorittimen, muistin sekoitetun arvojoukon perusteella ja lukeminen ja kirjoittaminen korvaukset. Kunkin streaming yksikkö vastaa noin 1 Megatavu/sekuntia siirtonopeuden. 

Kuinka monta SUs valitsemalla tarvittavat tietyn projektin määräytyy syötteiden ja työn määritetyn kyselyn osion määritystä varten. Voit valita enintään kiintiö-tietovirta työn yksikön mukaan perinteinen Azure-portaalissa. Jokaisen Azure tilauksen oletusarvoisesti on enintään 50 streaming yksiköt analytics töiden kiintiön tietyllä alueella. Niin, että streaming yksiköt tilauksistasi, ota yhteys [Microsoftin tuotetukeen](http://support.microsoft.com).

Työn voit hyödyntää streaming yksiköiden määrä määräytyy syötteiden ja työn määritetyn kyselyn osio-määritys. Huomaa myös stream yksiköt kelvollinen arvo on käytettävä. Kelvollisia arvoja Aloittava nro 1, 3, 6 ja sitten ylöspäin-kokoisissa 6, alla kuvatulla tavalla.

![Azure Stream Analytics virtauttaa yksiköt asteikko][img.stream.analytics.streaming.units.scale]

Tässä artikkelissa kerrotaan, miten laskeminen ja virittää niin, että siirtonopeuden analytics töiden kysely.

## <a name="embarrassingly-parallel-job"></a>Työn embarrassingly rinnakkain
Embarrassingly rinnakkain työ on Azure Stream Analytics on eniten skaalattava skenaario. Yhden esiintymän kyselyn syöte-osion muodostaa yhteyden yksi tulos-osio. Tämä rinnakkaisuus saavuttamiseksi edellyttää asioita:

1.  Jos kyselyn logiikan on riippuvainen saman kyselyn samassa esiintymässä käsittelemien avaimen, sitten sinun on varmistettava, että tapahtumat Siirry syötettäsi samaan osioon. Tapahtuman keskittimet kyseessä tämä tarkoittaa, että tapahtumatietoja täytyy olla **PartitionKey** määrittäminen tai voit käyttää osioitua lähettäjät. Blob-Tämä tarkoittaa, että tapahtumat lähetetään osio samassa kansiossa. Jos kyselyn logiikan ei edellytä saman avaimen käsittelemien kyselyn samassa esiintymässä, voit ohittaa tämä vaatimus. Tämä esimerkki olisi Valitse tai projektin/suodatus yksinkertaisen kyselyn.  
2.  Kun tiedot on korttimuodossa, kuten sen on oltava syötteen reunassa, annettava varmistaa, että kysely on osioitu. Tämä edellyttää, että Käytä **Osion mukaan** kaikki vaiheita. Useita vaiheita sallittu, mutta ne kaikki osioitu saman avaimen avulla. Huomaa myös, tällä hetkellä osioinnin avain on asetettuina **PartitionId** on täysin rinnakkain työn.  
3.  Tällä hetkellä vain tapahtuman keskittimet ja Blob-objektien tue osioitua tulos. Tapahtuman keskittimet tulosteen sinun on **PartitionKey** kenttä **PartitionId**määrittämiseen. Saat Blob sinun ei tarvitse tehdä mitään.  
4.  Toinen vaihtoehto huomata syötteen osioiden määrä on yhtä suuri kuin tulos osioiden määrää. Blob-objektien tulos ei tällä hetkellä tueta osioita, mutta tämä sopii, koska se Peri osiointimalli seuraavat kyselyn. Osion arvot, jotka sallia täysin rinnakkain työn Esimerkkejä:  
    1.  8 tapahtuman keskittimet syötteen osiot ja 8 tapahtuman keskittimet tulosteen osiot
    2.  8 tapahtuman keskittimet lisää osioita ja Blob-tulostus  
    3.  8 blob syötteen osiot ja Blob-tulostus  
    4.  8 blob syötteen osiot ja 8 tapahtuman keskittimet tulosteen osiot  

Seuraavassa on joitakin esimerkkejä eri tilanteista, jotka ovat embarrassingly rinnakkain.

### <a name="simple-query"></a>Yksinkertainen kysely
Syötteen – tapahtuman lihavoituna 8 osioiden tulosteen – tapahtumaa-toiminnossa 8 osioiden kanssa

**Kyselyn:**

    SELECT TollBoothId
    FROM Input1 Partition By PartitionId
    WHERE TollBoothId > 100

Tämä kysely on yksinkertainen suodatus ja sellaisenaan, Microsoft ei tarvitse huolehtia jakaminen tapahtuman porttiin lähettämistä syöte. Huomaat, että kysely on **Osion mukaan** , **OsionID**, joten on täytettävä edellä vaatimus #2. Tulostukseen annettava määrittäminen tapahtuman keskittimet tulosteen, projekti on **PartitionKey** -kentän arvoksi **PartitionId**. Yksi viimeisen tarkistuksen, syötteen osioiden == tulosteen osiot. Tämä topologian on embarrassingly rinnakkain.

### <a name="query-with-grouping-key"></a>Kyselyn ja ryhmittely
Syötteen – tapahtuman lihavoituna 8 osioiden tulosteen – Blob

**Kyselyn:**

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Tämä kysely on ryhmittely-avain ja näin ollen saman avaimen on voidaan käsitellä samaa kyselyn esiintymää. Tämä tarkoittaa annettava lähettää Microsoftin tapahtumat tapahtumien porttiin osioitua pikaviestikeskustelun. Mitä avain olemme tärkeisiin? **PartitionId** on työn logiikan-käsitteestä, emme tärkeisiin reaali avaimen **TollBoothId**. Tämä tarkoittaa sitä on asetettava **PartitionKey** tapahtuman tietojen lähettämistä tapahtuman porttiin on **TollBoothId** tapahtuman. Kyselyssä on **Osion mukaan** **PartitionId**, niin, että ovat hyvä. Tulostukseen, koska se on Blob-olemme ei tarvitse huolehtia määrittäminen **PartitionKey**. Tarpeen #4 uudelleen näin Blob-ei tarvitse huolehtia. Tämä topologian on embarrassingly rinnakkain.

### <a name="multi-step-query-with-grouping-key"></a>Usean vaiheen kyselyn ja ryhmittely ###
Kirjoita – tapahtumaa-toiminnossa 8: aan osioiden tulosteen – tapahtumaa-toiminnossa 8 osioiden kanssa

**Kyselyn:**

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )
    
    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Tämä kysely on ryhmittely-avain ja näin ollen saman avaimen on voidaan käsitellä samaa kyselyn esiintymää. Emme voi käyttää samaa strategia edellisen kyselyn. Kyselyssä on useita vaiheita. Onko jokaisen vaiheen **Osion mukaan** , **PartitionId**? Kyllä, joten on hyvä. Tuloksessa annettava määritetään **PartitionKey** **PartitionId** , edellä mainittujen ja näemme myös siinä on yhtä monta osioiden syötteenä. Tämä topologian on embarrassingly rinnakkain.


## <a name="example-scenarios-that-are-not-embarrassingly-parallel"></a>Esimerkkejä eri tilanteista, jotka eivät ole embarrassingly rinnakkain

### <a name="mismatched-partition-count"></a>Ristiriitaiset osion määrä ###
Syötteen – tapahtuman lihavoituna 8 osioiden tulosteen – tapahtumaa-toiminnossa 32 osioiden kanssa

Valintajärjestyksellä ei ole kyselyn ominaisuudet tällöin koska syötteen osion Laske! = tulosteen osion määrä.

### <a name="not-using-event-hubs-or-blobs-as-output"></a>Käytä tulosteen tapahtuman keskittimet tai BLOB-objektit
Syötteen – tapahtuman lihavoituna 8 osioiden tulosteen – PowerBI

PowerBI tulos ei tällä hetkellä tue jakaminen.

### <a name="multi-step-query-with-different-partition-by-values"></a>Usean vaiheen kysely, joka sisältää eri osion mukaan arvot
Kirjoita – tapahtumaa-toiminnossa 8: aan osioiden tulosteen – tapahtumaa-toiminnossa 8 osioiden kanssa

**Kyselyn:**

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )
    
    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By TollBoothId
    GROUP BY TumblingWindow(minute, 3), TollBoothId
    
Kuten näet, toinen vaihe käyttää **TollBoothId** osioinnin avaimeksi. Tämä ei ole sama kuin ensimmäiseksi ja näin ollen edellyttävät us tehtävä sekoitus. 

Nämä on joitakin esimerkkejä ja counterexamples Stream Analytics työt, jotka voivat saada embarrassingly rinnakkain topologian ja sitä mahdollisten suurin asteikko. Työt, jotka eivät sovellu jokin profiilit joka käsittelee voit skaalata vertailuvakiona joitakin muita canonical Stream Analytics skenaarioita tulevia päivityksiä tehdään.

Nyt tehdä käyttö alla yleisohjeita:

## <a name="calculate-the-maximum-streaming-units-of-a-job"></a>Laske streaming työn yksiköiden enimmäismäärä
Streaming yksiköitä, joita voidaan käyttää Stream Analytics työn määrä määräytyy määritetyn työn sekä kunkin vaiheen osioiden määrää kyselyn vaiheita numeroa.

### <a name="steps-in-a-query"></a>Kyselyn vaiheita
Kyselyn voi olla yksi tai useita vaiheita. Jokaisen vaiheen on määritetty **Korvaava** -avainsanalla aliraportti kysely. Vain kysely, joka on **Korvaava** -avainsanan ulkopuolella lasketaan myös vaiheena, esimerkiksi **SELECT** -lauseen seuraavalla kyselyllä:

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute,3), TollBoothId

Edellisessä kyselyssä on kaksi vaihetta.

> [AZURE.NOTE] Tämä esimerkkikyselyn selitetään artikkelin.

### <a name="partition-a-step"></a>Osion vaiheen

Vaiheen jakaminen edellyttää, että seuraavat edellytykset:

- Syötteen lähde on osioitu. Lisätietoja on artikkelissa [Tapahtuman keskittimet Programming Guide](../event-hubs/event-hubs-programming-guide.md).
- **SELECT** -lauseen kyselyn on lukea osioitua lähde.
- Kyselyn kohdan on oltava **Osion mukaan** -avainsana

Kun kysely on osioitu, syötteen tapahtumat ole erillinen osio käsitellyt ja koostetut-ryhmät ja tulostaa tapahtumat luodaan kunkin ryhmät. Jos yhdistetyt kooste ei olisi, sinun on luotava toinen osittamattoman vaihe koostefunktioita.

### <a name="calculate-the-max-streaming-units-for-a-job"></a>Laske streaming yksikkö projektin max

Osittamattoman vaiheiden suorittamisesta skaalata yhdessä enintään kuusi streaming yksiköt Stream Analytics projektille. Voit lisätä muita streaming yksiköt-vaihe on voi osioida. Kunkin osion voi olla kuusi streaming yksiköt.

<table border="1">
<tr><th>Kyselyn työn</th><th>Max streaming työn yksiköt</th></td>

<tr><td>
<ul>
<li>Kyselyssä on yksi vaihe.</li>
<li>Vaihe ei ole osioitu.</li>
</ul>
</td>
<td>6</td></tr>

<tr><td>
<ul>
<li>Syötteen tietovirta on osioinut 3.</li>
<li>Kyselyssä on yksi vaihe.</li>
<li>Vaihe on osioitu.</li>
</ul>
</td>
<td>18</td></tr>

<tr><td>
<ul>
<li>Kyselyssä on kaksi vaihetta.</li>
<li>Ei kumpaakaan vaiheita on osioitu.</li>
</ul>
</td>
<td>6</td></tr>



<tr><td>
<ul>
<li>Virta tietosyöte on osioinut 3.</li>
<li>Kyselyssä on kaksi vaihetta. Kirjoita vaihe on osioitu ja toinen vaihe ei ole.</li>
<li>SELECT-lauseen lukee osioitua syöte.</li>
</ul>
</td>
<td>24 (18 osioitua ohjeet) + osittamattoman vaiheet 6</td></tr>
</table>

### <a name="examples-of-scale"></a>Esimerkkejä asteikko
Seuraava kysely laskee autoista kautta maksullinen-asema, jossa on kolme tollbooths kolmen minuutin keskusteluikkunasta käsin. Tämä kysely skaalata enintään kuusi streaming yksiköt.

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Käytettävä kysely lisää streaming yksiköt sekä tiedot virtauttaa syötteen ja kysely on osioitu. Koska, että tiedot muodossa-osio on määritetty 3, seuraava muokattu kysely skaalata ylöspäin 18 streaming yksiköt:

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Kun kysely on osioitu, syötteen tapahtumat käsitellään ja koostetun erillinen osio ryhmissä. Tulosteen tapahtumia luodaan myös kunkin ryhmät. Jakaminen voi aiheuttaa joidenkin odottamattomia tuloksia, kun **Group by** -kenttä ei ole stream tietosyöte osio-näppäintä. Esimerkiksi edellisen esimerkkikyselyn **TollBoothId** -kenttä ei ole osion avaimen Input1. Maksupisteen #1 tiedot voit levittää useita osiossa.

Jokaisen Input1 erikseen avulla käsitellään Stream Analytics ja auton pass-yksisuuntaisen Laske saman maksupisteen samassa tumbling-ikkunassa useita tietueita luodaan. Jos syötteen osio-näppäin ei voi muuttaa, voidaan korjata tämän ongelman lisäämällä osio muita vaihe, kuten:

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute, 3), TollBoothId

Tämä kysely skaalata streaming yksiköt 24.

>[AZURE.NOTE] Jos liität kaksi virtaa, varmista virtaa osioinut sarake, voit tehdä liitokset ja sama määrä osiot ovat molemmat virtaa osio-näppäintä.


## <a name="configure-stream-analytics-job-partition"></a>Määritä Stream Analytics työ-osio

**Jos haluat säätää streaming yksikkö projektin**

1. Kirjautuminen [hallinta-portaalin](https://manage.windowsazure.com).
2. Valitse vasemmanpuoleisessa ruudussa **Stream Analytics** .
3. Valitse Stream Analytics-projekti, jonka haluat skaalata.
4. Valitse sivun yläosassa **ASTEIKKO** .

![Azure Stream Analytics virtauttaa yksiköt asteikko][img.stream.analytics.streaming.units.scale]

Azure-portaalissa mittakaava-asetuksia voi käyttää asetukset-kohdassa:

![Azure Portal Stream Analytics projektin määrittäminen][img.stream.analytics.preview.portal.settings.scale]

## <a name="monitor-job-performance"></a>Työn suorituskyvyn seuranta

Käytä hallinta-portaalissa, voit seurata projektin siirtonopeuden tapahtumien/toisen:

![Töiden Azure Stream Analytics-valvominen][img.stream.analytics.monitor.job]

Laske työmäärää odotettu siirtonopeuden tapahtumien/toisen. Jos siirtonopeuden on pienempi kuin odotetaan, paranna syötteen osio, paranna kysely ja lisää muita streaming yksiköt työtäsi.

## <a name="stream-analytics-throughput-at-scale---raspberry-pi-scenario"></a>Virta Analytics siirtonopeuden tasolla - vadelma pii-skenaario


Selvittääksesi, miten stream analytics työt skaalata tavallisessa tilanteessa kannalta käsitellään liikenteen yli useita Streaming yksiköiden, seuraavassa on koe, joka lähettää tunnistimen tiedot (asiakkaat) tapahtuman keskittimeen, käsittelee sen ja lähettää ilmoituksen tai tilastotiedot tulos toisen tapahtuman keskittimeen.

Asiakas on lähettäminen synteettiseksi tunnistimen tietojen tapahtuman porttiin Stream Analytics JSON-muodossa ja tietojen tulos on myös JSON-muodossa.  Miten esimerkkitiedot näyttää kaltaisten –  

    {"devicetime":"2014-12-11T02:24:56.8850110Z","hmdt":42.7,"temp":72.6,"prss":98187.75,"lght":0.38,"dspl":"R-PI Olivier's Office"}

Kyselyn: "lähettää ilmoituksen, kun valo pois päältä"  

    SELECT AVG(lght),
     “LightOff” as AlertText
    FROM input TIMESTAMP
    BY devicetime
     WHERE
        lght< 0.05 GROUP BY TumblingWindow(second, 1)

Mittaaminen siirtonopeuden: Siirtonopeuden tässä yhteydessä on käsittelemien Stream Analytics rahassa ajan (10 minuuttia)-kenttään annettavat tiedot määrä. Parhaat käsittely-siirtonopeuden syöttötiedot saavuttamiseksi sekä tiedot virtauttaa syötteen ja kysely on osioitu. Myös **COUNT()**sisältyy mittaa input kuinka monessa lajissa käsiteltiin kysely. Jotta työ ei yksinkertaisesti odottanut syötteen tapahtumien tulee kunkin osion syötteen tapahtumaa-toiminnossa on esilataamia riittävästi syöttötiedot (noin 300 Mt) kanssa.  

Seuraavassa on kasvavilla virtautetun median yksiköiden määrän, tulokset ja vastaava osio laskee tapahtuman keskittimet.  

<table border="1">
<tr><th>Syötteen osiot</th><th>Tulostus-osiot</th><th>Streaming yksiköt</th><th>Kestävä siirtonopeuden
</th></td>

<tr><td>12</td>
<td>12</td>
<td>6</td>
<td>4.06 Mt/s</td>
</tr>

<tr><td>12</td>
<td>12</td>
<td>12</td>
<td>8.06 Mt/s</td>
</tr>

<tr><td>48</td>
<td>48</td>
<td>48</td>
<td>38.32 Mt/s</td>
</tr>

<tr><td>192</td>
<td>192</td>
<td>192</td>
<td>172.67 Mt/s</td>
</tr>

<tr><td>480</td>
<td>480</td>
<td>480</td>
<td>454.27 Mt/s</td>
</tr>

<tr><td>720</td>
<td>720</td>
<td>720</td>
<td>609.69 Mt/s</td>
</tr>
</table>

![IMG.Stream.Analytics.perfgraph][img.stream.analytics.perfgraph]

## <a name="get-help"></a>Ohjeiden saaminen
Saat lisäohjeita voit kokeilla Microsoftin [Azure Stream Analytics-keskustelupalstalla](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Seuraavat vaiheet

- [Azure Stream Analytics esittely](stream-analytics-introduction.md)
- [Azure Stream Analytics käytön aloittaminen](stream-analytics-get-started.md)
- [Azure Stream Analytics kyselyn kieliohje](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics hallinta REST API-viittaus](https://msdn.microsoft.com/library/azure/dn835031.aspx)


<!--Image references-->

[img.stream.analytics.monitor.job]: ./media/stream-analytics-scale-jobs/StreamAnalytics.job.monitor.png
[img.stream.analytics.configure.scale]: ./media/stream-analytics-scale-jobs/StreamAnalytics.configure.scale.png
[img.stream.analytics.perfgraph]: ./media/stream-analytics-scale-jobs/perf.png
[img.stream.analytics.streaming.units.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsStreamingUnitsExample.jpg
[img.stream.analytics.preview.portal.settings.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsPreviewPortalJobSettings.png

<!--Link references-->

[microsoft.support]: http://support.microsoft.com
[azure.management.portal]: http://manage.windowsazure.com
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
 
