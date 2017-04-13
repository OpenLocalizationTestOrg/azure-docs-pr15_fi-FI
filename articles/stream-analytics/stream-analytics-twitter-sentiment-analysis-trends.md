<properties
    pageTitle="Reaaliaikainen Twitter markkinatunnelma analysointi Stream Analytics | Microsoft Azure"
    description="Opettele käyttämään Stream Analytics reaaliaikainen Twitter-markkinatunnelma analyysia varten. Vaiheittaisia ohjeita tapahtuman luonti-tietoihin live Raporttinäkymät-ikkunan."
    keywords="Reaaliaikainen twitter trendi analyysi, markkinatunnelma analyysi, Yhteisöpalvelut analysis, trendi analysis-Esimerkki"
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
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>


# <a name="social-media-analysis-real-time-twitter-sentiment-analysis-in-azure-stream-analytics"></a>Yhteisöpalvelut analyysi: reaaliaikaisen Twitter-markkinatunnelma analyyseja Azure Stream Analytics

Opettele muodostaa markkinatunnelma analysis-ratkaisun Yhteisöpalvelut analysoinnissa tuo Azure tapahtuman keskittimet reaaliaikainen Twitter-tapahtumat. Kirjoitat Azure Stream Analytics-kyselyn tietojen analysoimista varten. Voit sitten joko myöhemmin perusal tulosten tallentaminen tai Raporttinäkymät-ikkunan ja [Power BI](https://powerbi.com/) antamaan tietoja reaaliajassa.

Yhteisöpalvelut analytics Työkalut auttaa organisaatioita ymmärtää tykkäysten aiheista, eli aiheet ja asenteitaan, joissa on paljon viestejä Yhteisöpalvelut. Markkinatunnelma analyysi, jota kutsutaan myös *mielestä kaivos*, käyttää Yhteisöpalvelut analytics Työkalut asenteet kohti tuotteen, verrata ja niin edelleen. Reaaliaikainen Twitter-trendi analyysi on hyvä esimerkki, koska hashtag Tilauksen mallin avulla voit kuunnella avainsanoja ja kehittää syötteen markkinatunnelma analyysi.

## <a name="scenario-sentiment-analysis-in-real-time"></a>Skenaario: Markkinatunnelma analyysi reaaliajassa

Yritys, joka on uutisia media-sivusto on kiinnostunut käytön mukaan, jossa kerrotaan sivuston sisältöön, joka on heti sen lukijat etu kilpailijoiden verrattuna. Yritys käyttää Yhteisöpalvelut analyysi aiheista, jotka liittyvät lukijat tekemällä reaaliaikainen markkinatunnelma analyysi Twitter-tiedot. Tarkemmin sanottuna tunnistavan tykkäysten aiheet reaaliajassa Twitter yrityksen on reaaliaikainen analytics tweetin ja markkinatunnelma keskeisiä ohjeita. Oikeastaan edellyttää, on tietokoneessa markkinatunnelma analyysi analytics engine, joka perustuu tämän syötteen Yhteisöpalvelut.

## <a name="prerequisites"></a>Edellytykset
-   Twitter-tili ja [OAuth-käyttöoikeustietue](https://dev.twitter.com/oauth/overview/application-owner-access-tokens)
-   [TwitterClient.zip](http://download.microsoft.com/download/1/7/4/1744EE47-63D0-4B9D-9ECF-E379D15F4586/TwitterClient.zip) Microsoft Download Centeristä
-   Valinnainen: Lähdekoodin [GitHub](https://aka.ms/azure-stream-analytics-twitterclient) twitter-asiakas

## <a name="create-an-event-hub-input-and-a-consumer-group"></a>Kirjoita tapahtumaa-toiminnossa ja kuluttaja-ryhmän luominen

Sovelluksen malli luo tapahtumia ja push-tapahtuman keskittimet erillisen (tapahtumaa-toiminnossa lyhyen varten). Palvelun Bus tapahtuman keskittimet on suositeltavampaa tapahtuman annosmuuntokertoimien Stream analysoinnissa. Tapahtuman keskittimet ohjeissa [Palvelun Bus](/documentation/services/service-bus/)ohjeissa.

Seuraavien vaiheiden avulla voit luoda tapahtumaa-toiminnossa.

1.  Azure-portaalissa, valitse **Uusi** > **Sovelluksen SERVICES** > **Palvelun BUS** > **Tapahtumaa-TOIMINNOSSA** > **Nopea luominen**, ja Anna nimi, alue ja uuteen tai aiemmin luotuun nimitilan.  
2.  Paras käytäntö on Stream Analytics kunkin projektin pitäisi lukea yhden tapahtuman keskittimet kuluttaja ryhmän. Olemme edetään ohjatusti luominen kuluttaja ryhmän myöhemmin. Voit lukea lisää ryhmiä osoitteessa [Azure tapahtuman keskittimet yleiskatsaus](https://msdn.microsoft.com/library/azure/dn836025.aspx). Luo kuluttaja ryhmä, siirry juuri luomasi tapahtumaa-toiminnossa valitsemalla **Ryhmiä** -välilehti, valitse **Luo** -sivun alareunaan ja anna kuluttaja-ryhmän nimi.
3.  Tapahtuma-toiminnossa käyttöoikeuden myöntäminen annettava jaetun access-käytännön luominen. Valitse tapahtuma-toiminnossa **määritys** -välilehti.
4.  Luo uuden käytännön **JAETTU käytäntöjen** **hallinta** käyttöoikeuksien avulla.

    ![Jaettu Access käytäntöjä, jossa voit luoda käyttöoikeuksien hallinta käytännön.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-ananlytics-shared-access-policies.png)

5.  Valitse sivun alareunassa **Tallenna** .
6.  Siirry **raporttinäkymät-IKKUNAN**, valitse sivun alareunassa **Yhteystiedot** ja kopioi ja Tallenna yhteystiedot. (Käytä Kopioi kuvake, joka näkyy kohdassa Hae-kuvaketta.)

## <a name="configure-and-start-the-twitter-client-application"></a>Määritettäessä ja käynnistettäessä Twitter-asiakassovellukseen

On annettu asiakassovellus, joka muodostaa yhteyden Twitter tietoja [Twitter's Streaming ohjelmointirajapinnan](https://dev.twitter.com/streaming/overview) kerätä tietoja Parametroitu seuraamasi aiheet tapahtumia Tweetin kautta. [Sentiment140](http://help.sentiment140.com/) Avaa lähde-työkalun avulla määritetään kunkin tweetin markkinatunnelma arvo.

- 0 = negatiivinen
- 2 = nolla
- 4 = positiivinen

Valitse Tweetin tapahtumat siirretty tapahtuma-keskittimeen.  

Voit määrittää sovelluksen seuraavasti:

1.  [Lataa TwitterClient-ratkaisun](http://download.microsoft.com/download/1/7/4/1744EE47-63D0-4B9D-9ECF-E379D15F4586/TwitterClient.zip).
2.  Avaa TwitterClient.exe.config ja oauth_consumer_key, oauth_consumer_secret, oauth_token ja oauth_token_secret korvaaminen Twitter-tunnuksia, joiden arvot.  

    [Luo OAuth-käyttöoikeustietue vaiheet](https://dev.twitter.com/oauth/overview/application-owner-access-tokens)  

    Huomaa, että sinun on tehdä tyhjän sovelluksen luomiseen tunnusta.  
3.  Korvaa TwitterClient.exe.config EventHubConnectionString ja EventHubName arvot yhteysmerkkijonon ja tapahtumaa-toiminnossa nimi. Yhteysmerkkijonon, joka on kopioitu aiemmin antaa yhteysmerkkijonon ja tapahtumaa-toiminnossa nimi, joten varmista, että jokainen oikean kentän valitseminen ja erota ne. Esimerkkinä yhteysmerkkijonon seuraavasti:

        Endpoint=sb://your.servicebus.windows.net/;SharedAccessKeyName=yourpolicy;SharedAccessKey=yoursharedaccesskey;EntityPath=yourhub

    TwitterClient.exe.config-tiedostossa on oltava asetusten, kuten seuraavassa esimerkissä:

        add key="EventHubConnectionString" value="Endpoint=sb://your.servicebus.windows.net/;SharedAccessKeyName=yourpolicy;SharedAccessKey=yoursharedaccesskey"
        add key="EventHubName" value="yourhub"

    On tärkeää Huomaa, että teksti "EntityPath =" onko __ei__ näy EventHubName-arvo.

4.  *Valinnainen:* Säädä avainsanoista etsittävät.  Tämän sovelluksen hakee oletuksena "Azure, Skype, XBox, Microsoft, Seattle".  Voit halutessasi säätää **twitter_keywords** TwitterClient.exe.config, valitse arvot.
5.  Suorita TwitterClient.exe Käynnistä sovellus. Näkyviin tulee Tweetin tapahtumien **CreatedAt**, **Aihe**ja **SentimentScore** arvot, jotka on lähetetty tapahtumaa-toiminnossa.

    ![Markkinatunnelma analyysi: SentimentScore arvot lähetetään tapahtumaa-toiminnossa.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-twitter-sentiment-output-to-event-hub.png)

## <a name="create-a-stream-analytics-job"></a>Luo Stream Analytics-työ

Nyt kun Tweetin tapahtumat ovat streaming reaaliajassa Twitter-olemme määrittää Stream Analytics-työ analysoida näitä tapahtumia reaaliajassa.

### <a name="provision-a-stream-analytics-job"></a>Valmistele Stream Analytics-työ

1.  [Azure-portaali](https://manage.windowsazure.com/)valitsemalla **Uusi** > **TIETOPALVELUJEN** > **STREAM ANALYTICS** > **Nopea luominen**.
2.  Määritä seuraavat arvot ja valitse sitten **Luo STREAM ANALYTICS työ**:

    * **Työn nimi**: Kirjoita projektin nimi.
    * **Alue**: alue, jonka haluat suorittaa työn. Harkitse siten työ- ja tapahtumaa-toiminnossa samalla alueella, jotta parempi suorituskyky ja haluat varmistaa, että sinun ei maksat tiedonsiirron alueiden välillä.
    * **TALLENNUSTILAN tilin**: Valitse Azure-tallennustilan tilin, jotka haluat tallentaa kaikki Stream Analytics-työt, jotka suoritetaan tämän alueen seurantatiedot. Voit halutessasi valita käytössä olevan tallennustilan-tilin tai luoda uuden.

3.  Valitse vasemmanpuoleisessa ruudussa luettelon Stream Analytics työt **STREAM ANALYTICS** .  
    ![Virta Analytics service-kuvake](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-service-icon.png)

    Uusi projekti näytetään tila on **luotu**. Huomaa, että sivun alareunaan **Käynnistä** -painiketta ei ole käytössä. Työn syöte, tulostus ja kysely on määritettävä, ennen kuin aloitat työn.


### <a name="specify-job-input"></a>Työn syöte
1.  Työtäsi Stream Analytics- **SYÖTTEIDEN** sivun yläreunasta ja valitse sitten **Lisää SYÖTE**. Näkyviin tulevan valintaikkunan käy läpi määrä syötettäsi määrittämisen vaiheita.
2.  Valitse **TIETOVIRTA**, ja valitse sitten oikealle-painike.
3.  Napsauta **Tapahtumaa-TOIMINNOSSA**ja valitse sitten oikealle-painike.
4.  Kirjoita tai valitse kolmannella sivulla seuraavat arvot:

    * **SYÖTTEEN ALIAS**: Kirjoita kutsumanimi työlle syötteenä, kuten *TwitterStream*. Huomaa, että voit käyttää tätä nimeä kyselyn myöhemmin.
    **Tapahtuma-TOIMINNOSSA**: Jos, jonka loit tapahtumaa-toiminnossa on samassa muodossa Analytics-työ-tilauksen, valitse nimitilan, joka on tapahtumaa-toiminnossa.

        Jos tapahtuma-toiminnossa on eri tilauksen, valitse **Käytä tapahtumaa-toiminnossa toiseen tilaukseen**ja anna tiedot **Palvelun BUS nimitila**, **Tapahtuman KESKITTIMEEN nimi**, **Tapahtuman KESKITTIMEEN käytännön nimi**, **Tapahtuman KESKITTIMEEN käytännön avain**ja **Tapahtumien KESKITTIMEEN OSION määrä**manuaalisesti.

    * **Tapahtuman KESKITTIMEEN nimi**: Valitse tapahtumaa-toiminnossa nimi.

    * **Tapahtuman KESKITTIMEEN käytännön nimi**: Valitse tapahtuma-toiminnossa käytäntö, jonka loit aiemmin tässä opetusohjelmassa.

    * **Tapahtuman KESKITTIMEEN KULUTTAJA ryhmän**: kuluttaja-ryhmä, jonka loit aiemmin tässä opetusohjelmassa nimi.
5.  Napsauta hiiren kakkospainikkeella.
6.  Määritä seuraavat arvot:

    * **Tapahtuman SARJATOIMINTO muoto**: JSON
    * **KOODAUS**: UTF8

7.  Valitse **Tarkista** -painike, Lisää tämä lähde ja tarkista, että Stream Analytics voit muodostaa tapahtumaa-toiminnossa.

### <a name="specify-job-query"></a>Määritä projektin kysely

Virta Analytics tukee yksinkertainen, määritettäviä kysely-malli, joka kuvaa muunnoksia. Saat lisätietoja kielen Hae [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx).  Tässä opetusohjelmassa auttaa sinua tekijän ja testaa useiden kyselyjen Twitter tietojen päälle.

#### <a name="sample-data-input"></a>Esimerkki tietosyöte

Vahvista kyselyn todellinen työ tiedoista, voit **MALLITIEDOT** -ominaisuus poimia tapahtumien lisääminen virta ja luo .json tiedoston tilanteista testikäyttöön.

1.  Valitse tapahtuma-toiminnossa syöte ja valitse sitten **MALLITIEDOT** sivun alareunassa.
2.  Määritä avautuvassa valintaruudussa **ALOITUSAIKA** Aloita kerätä tietoja ja kuinka paljon uusia tietoja tarjoaman **kesto** .
3.  Valitse **tiedot** -painiketta ja valitse sitten Lataa ja tallenna se luotu .json **napsauttamalla tätä** linkkiä.

#### <a name="pass-through-query"></a>Läpivientikysely
Alkavan, emme tällöin toimi yksinkertainen läpivientikysely, projektien tapahtuman kaikkia kenttiä.

1.  Valitse **KYSELYN** Stream Analytics työ-sivun yläreunassa.
2.  Koodi-editorissa korvaa alkuperäinen kyselymalli seuraavasti:

        SELECT * FROM TwitterStream

    Varmista, että syötteen lähteen nimi vastaa syöte, jonka olet määrittänyt aiemmin nimeä.

3.  Valitse **Testaa** kohdassa kyselyeditori.
4.  Siirry .json mallitiedostossa.
5.  Valitse **Tarkista** -painike sekä tarkastella tuloksia kyselymääritystä alapuolella.

    ![Kyselyn kuvauksen alapuolella tulokset](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-sentiment-by-topic.png)

#### <a name="count-of-tweets-by-topic-tumbling-window-with-aggregation"></a>Aiheen mukaan tweets määrä: Tumbling, joissa on kooste

Voit verrata maininnat aiheiden määrä-käytetään [TumblingWindow](https://msdn.microsoft.com/library/azure/dn835055.aspx) saat maininnat määrää aiheen mukaan viiden sekunnin välein.

1.  Koodieditori kyselyn muuttaminen:

        SELECT System.Timestamp as Time, Topic, COUNT(*)
        FROM TwitterStream TIMESTAMP BY CreatedAt
        GROUP BY TUMBLINGWINDOW(s, 5), Topic

    Tämä kysely käyttää **AIKALEIMA mukaan** -avainsanan Aikaleimakenttä on määritettävä ajallinen laskenta käytettävät tiedot. Jos kentässä ei ole määritetty, windowing toiminto suoritettava käyttämällä aika, joka kuhunkin tapahtumaan on saapunut tapahtumaa-toiminnossa.  Lue lisää [Stream Analytics kyselyn viittaus](https://msdn.microsoft.com/library/azure/dn834998.aspx)"Saapumisen ja sovelluksen aika"-osassa.

    Tämä kysely käyttää myös jokaisen ikkunan loppuun aikaleima **System.Timestamp** -ominaisuuden avulla.

2.  **Suorita** kohdassa kyselyeditori näet kyselyn tulokset.

#### <a name="identify-trending-topics-sliding-window"></a>Määritä tykkäysten aiheita: liukuva ikkuna

Laji tykkäysten aiheet, näet ohjeita, jotka kulkevat tietyn ajanjakson aikana maininnat kynnysarvo. Tässä opetusohjelmassa tarkoitetaan on tarkistaa aiheisiin, joissa on mainittu yli 20 kertaa edellisen viiden sekunnin kuluttua [SlidingWindow](https://msdn.microsoft.com/library/azure/dn835051.aspx)avulla.

1.  Koodieditori kyselyn muuttaminen: Valitse System.Timestamp as kellonaika, aihe, Laske (*) maininnat-TwitterStream AIKALEIMA mukaan CreatedAt ryhmän mukaan SLIDINGWINDOW(s, 5), ohjeaiheen, laskea ottaa nimellä (*) > 20

2.  **Suorita** kohdassa kyselyeditori näet kyselyn tulokset.

    ![Liukuva ikkunan kyselyn tuloksiin](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-query-output.png)

#### <a name="count-of-mentions-and-sentiment-tumbling-window-with-aggregation"></a>Laske mainintoja ja markkinatunnelma: Tumbling, joissa on kooste
Lopulliset kyselyn, joka on Testaa käyttää **TumblingWindow** saat mainintoja, keskiarvo, vähintään, enintään ja markkinatunnelma pistemäärän keskihajonnan määrän aihe viiden sekunnin välein.

1.  Koodieditori kyselyn muuttaminen:

        SELECT System.Timestamp as Time, Topic, COUNT(*), AVG(SentimentScore), MIN(SentimentScore),
        Max(SentimentScore), STDEV(SentimentScore)
        FROM TwitterStream TIMESTAMP BY CreatedAt
        GROUP BY TUMBLINGWINDOW(s, 5), Topic

2.  **Suorita** kohdassa kyselyeditori näet kyselyn tulokset.
3.  Tämä on kysely, joka Käytämme Microsoftin raporttinäkymät-ikkunaa.  Valitse sivun alareunassa **Tallenna** .


## <a name="create-output-sink"></a>Luo tulosteen käsittelytoiminto

Nyt kun tapahtuma-muodossa, ingest tapahtuma- ja kysely, joka suorittaa muunnoksen virta-syöte tapahtumaa-toiminnossa määritimme viimeinen vaihe on Määritä tulostus-käsittelytoiminto projektille.  Olemme kirjoittaa koostetun tweetin tapahtumat Azure-Blob-säiliö työn Microsoftin kyselystä.  Voi myös siirtää tulokset Azure SQL-tietokanta ja Azure-taulukkotallennus tai tapahtuman keskittimet tarvitsee tietyn sovelluksen mukaan.

Seuraavien vaiheiden avulla voit luoda Blob-objektien tallennustilaan säilö Jos sinulla ei ole vielä yksi:

1.  Käytä aiemmin luotua tallennustilan-tiliä tai Luo uusi tallennustilan tili valitsemalla **Uusi** > **TIETOPALVELUJEN** > **TALLENNUSTILAN** > **Nopea luominen**ja noudattamalla sitten näytön ohjeita.
2.  Tallennustilan-tili, valitse **SÄILÖJEN** sivun yläreunassa ja valitse sitten **Lisää**.
3.  Oman säilön **nimi** ja määritä **Julkinen Blob** **ACCESS** .

## <a name="specify-job-output"></a>Määritä projektin tulostus

1.  Työtäsi Stream Analytics- **tulostus** sivun yläreunassa ja valitse sitten **Lisää tulos**. Näkyviin tulevan valintaikkunan opastaa jaettavaan määrittämisen useita vaiheita.
2.  Valitse **BLOB-SÄILIÖ**ja valitse sitten oikealle-painike.
3.  Kirjoita tai valitse kolmannella sivulla seuraavat arvot:

    * **Tunnus**: Kirjoita kutsumanimi työ-tulostusta varten.

    * **TILAUS**: Jos olet luonut Blob-objektien tallennustilaan on samassa muodossa Analytics-työ-tilauksen, valitse **Käytä tallennustilan tilin nykyisen tilauksesi**. Jos tallennustilaa on eri tilauksen, valitse **Käytä tallennustilan tilin toiseen tilaukseen**ja tietoja **TALLENNUSTILAN tilin**, **TALLENNUSTILAN tilin avain**ja **SÄILÖ**.

    * **TALLENNUSTILAN tilin**: Valitse tallennustilan tilin nimi.

    * **SÄILÖN**: Valitse säilön nimi.

    * **Tiedostonimi ETULIITE**: tiedoston etuliite, Käytä kirjoitettaessa blob-tuloste.

4.  Napsauta hiiren kakkospainikkeella.
5.  Määritä seuraavat arvot:
    * **Tapahtuman SARJATOIMINTO muoto**: JSON
    * **KOODAUS**: UTF8
6.  Valitse **Tarkista** -painike, Lisää tämä lähde ja tarkista, että Stream Analytics voit muodostaa tallennustilan tilin.

## <a name="start-job"></a>Aloita työ

Koska työn syöte, kyselyssä ja tulos on kaikki määritetty, emme jatkaa Stream Analytics-työ.

1.  Projektin **raporttinäkymät-IKKUNAN**Valitse sivun alareunassa **Käynnistä-painiketta** .
2.  Valitse avautuvassa valintaruudussa **työn ALKAMISAIKA**ja valitse sitten valintaikkunan alareunassa **Tarkista** -painiketta. Työn tilaksi muuttuu **Aloitus** ja pian muuttaa **käynnissä**.


## <a name="view-output-for-sentiment-analysis"></a>Näytä tuloste markkinatunnelma analysointia varten

Kun työ on käynnissä ja käsittelyn reaaliaikainen Twitter-muodossa, valitse kuinka haluat tarkastella tulosteen markkinatunnelma analyysia varten. Työkalu, kuten [Azure-tallennustilan Explorer](https://azurestorageexplorer.codeplex.com/) tai [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) avulla voit tarkastella projektin output reaaliajassa. Tässä näkymässä voit [Power BI](https://powerbi.com/) siirtämisestä sisältämään mukautetun koontinäytön seuraavassa näyttökuvassa samanlaiseksi sovelluksen.

![Yhteisöpalvelut analyysi: Stream Analytics markkinatunnelma analyysi (mielestä kaivos) tulosteen Power BI-koontinäytön.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-output-power-bi.png)

## <a name="get-support"></a>Käytä tukipalveluita
Saat lisäohjeita voit kokeilla Microsoftin [Azure Stream Analytics-keskustelupalstalla](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Seuraavat vaiheet

- [Azure Stream Analytics esittely](stream-analytics-introduction.md)
- [Azure Stream Analytics käytön aloittaminen](stream-analytics-get-started.md)
- [Skaalaa Azure Stream Analytics töitä](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics kyselyn kieliohje](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics hallinta REST API-viittaus](https://msdn.microsoft.com/library/azure/dn835031.aspx)
