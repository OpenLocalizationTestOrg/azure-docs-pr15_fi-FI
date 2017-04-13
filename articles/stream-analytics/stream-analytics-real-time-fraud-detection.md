<properties
    pageTitle="Virta Analytics: Reaaliaikainen petoksen tunnistus | Microsoft Azure"
    description="Opettele luomaan reaaliaikaisia petoksen tunnistus ratkaista Stream Analytics kanssa. Käytä tapahtumaa-toiminnossa reaaliaikainen tapahtuman käsittelyä varten."
    keywords="poikkeavuuksista tunnistus, petoksilta tunnistus, reaaliaikainen poikkeavuuksista tunnistus"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok" />



# <a name="get-started-using-azure-stream-analytics-real-time-fraud-detection"></a>Azure Stream Analytics käytön aloittamisessa: reaaliaikainen petoksen tunnistus

Opettele luomaan lopusta loppuun-ratkaisun reaaliaikainen petoksen tunnistaminen Azure Stream Analytics kanssa. Tuo tapahtumia Azure tapahtuman keskittimet, kirjoittaa kooste tai ilmoitat Stream Analytics kyselyt ja lähetä tulokset tulostus-käsittelytoiminto lisäämään tietoja tietojen kanssa reaaliajassa käsittely päälle. Reaaliaikainen poikkeavuuksista tunnistus tietoliikennettä varten selitetään, mutta Esimerkki-tekniikka sopii tasaisesti muunlaisia petoksen tunnistus kuten luottokorttia tai käyttäjätiedot varkaus skenaarioita.

Virta Analytics on täysin hallitun palvelun, joka tarjoaa pieni viive erittäin käytettävissä skaalattava, monimutkaisia käsittely päälle streaming tietojen pilveen. Lisätietoja on artikkelissa [Azure Stream Analytics esittely](stream-analytics-introduction.md).


## <a name="scenario-telecommunications-and-sim-fraud-detection-in-real-time"></a>Skenaario: Televiestintä ja SIM petoksen tunnistus reaaliajassa

Televiestinnän yrityksellä on erittäin paljon tietoja saapuvia puheluita. Yrityksen täytyy sen tiedoista seuraavasti:

* Pienennä tietojen helpommin hallittaviin summan ja saada tietoa asiakkaan käyttö eri ajankohtina ja eri maantieteellisillä alueilla.
* Tunnista SIM petoksen (useita kutsuja tunnistetiedot samaan aikaan, mutta maantieteellisesti eri sijainneissa) reaaliajassa, niin, että yrityksen helposti vastata siihen luomalla ilmoituksen asiakkaiden tai palvelun suljetaan.

Kanoninen asioita Internet (IoT) skenaarioita jäsenillä telemetriatietojen tai anturit tiedot. Asiakkaiden haluat koostaa tietoja tai vastaanottaa ilmoituksia poikkeamia reaaliajassa.

## <a name="prerequisites"></a>Edellytykset

- Lataa [TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip) Microsoft Download Centeristä
- Valinnainen: Tapahtuma-luontitoiminnon [GitHub](https://aka.ms/azure-stream-analytics-telcogenerator) -lähdekoodi

## <a name="create-azure-event-hubs-input-and-consumer-group"></a>Azure tapahtuman keskittimet lisätiedot ja kuluttaja-ryhmän luominen

Sovelluksen malli luo tapahtumia ja push-tapahtuman keskittimet esiintymä reaaliaikainen käsittelyä varten. Palvelun Bus tapahtuman keskittimet on suositeltavampaa tapahtuman annosmuuntokertoimien Stream analysoinnissa. Voit lukea lisää tapahtuman keskittimet [Azure palvelun Bus](/documentation/services/service-bus/)ohjeissa.

Voit luoda tapahtumaa-toiminnossa seuraavasti:

1.  [Azure-portaali](https://manage.windowsazure.com/)valitsemalla **Uusi** > **Sovelluksen SERVICES** > **Palvelun BUS** > **Tapahtumaa-TOIMINNOSSA** > **Nopea luominen**. Anna nimi, alue ja uuteen tai aiemmin luotuun nimitilan, Luo uusi tapahtuma-toiminnossa.  
2.  Paras käytäntö on Stream Analytics kunkin projektin Lue tarkasteltavat keskittimeen kuluttaja-ryhmästä. Olemme edetään ohjatusti luominen kuluttaja ryhmän myöhemmin. [Lisätietoja ryhmiä](https://msdn.microsoft.com/library/azure/dn836025.aspx). Luo kuluttaja ryhmä, siirry juuri luomasi tapahtumaa-toiminnossa valitsemalla **Ryhmiä** -välilehti, valitse **Luo** -sivun alareunaan ja anna kuluttaja-ryhmän nimi.
3.  Tapahtuma-toiminnossa käyttöoikeuden myöntäminen annettava jaetun access-käytännön luominen. Valitse tapahtuma-toiminnossa **määritys** -välilehti.
4.  Valitse **JAETTU käytäntöjen**Luo uuden käytännön, joka on käyttöoikeuksien **hallinta** .

    ![Jaettu Access käytäntöjä, jossa voit luoda käyttöoikeuksien hallinta käytännön.](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-shared-access-policies.png)

5.  Valitse sivun alareunassa **Tallenna** .
6.  Siirry **raporttinäkymät-ikkunan**, valitse sivun alareunassa **Yhteystiedot** ja kopioi ja Tallenna yhteystiedot.

## <a name="configure-and-start-the-event-generator-application"></a>Määritä ja Käynnistä sovellus tapahtuman luominen

On annettu asiakassovellus, joka luo otoksen saapuvan puhelun metatietojen ja push tapahtuma-porttiin. Seuraavien vaiheiden avulla voit määrittää tämän sovelluksen.  

1.  Lataa [Tiedosto TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip)ja unzip hakemistoon.

    > [AZURE.NOTE] Windowsin saattavat estää ladatut zip-tiedoston. Napsauta tiedostoa hiiren kakkospainikkeella ja valitse **Ominaisuudet**. Jos saat "tiedosto oli toisesta tietokoneesta, ja se on estetty tämän tietokoneen suojaamiseen" ilmoituksen, valitse **Salli** -ruudusta ja valitse sitten Käytä zip-tiedoston avulla.

2.  Korvaa telcodatagen.exe.config Microsoft.ServiceBus.ConnectionString ja EventHubName arvot tapahtumaa-toiminnossa yhteysmerkkijonon ja nimi.

    Yhteysmerkkijonon, jonka kopioit Azure portaalin sijoittaa yhteyden nimen lopussa. Muista poistaa "; EntityPath =<value>"-" Lisää avain = "kentän.

3.  Käynnistä sovellus. Käyttö on seuraavanlainen:

    telcodatagen.exe #NumCDRsPerHour [SIM kortin petoksen todennäköisyys] [#DurationHours]

Seuraavassa esimerkissä Luo 1 000 tapahtumien 20 prosenttia petoksen todennäköisyydellä kahden tunnin kuluessa.

    telcodatagen.exe 1000 .2 2

Näet tietueet, jotka on lähetetty tapahtumaa-toiminnossa. Määritetään joissakin avainkenttien, joka on käytössä reaaliaikainen petoksen tunnistus sovelluksessa tähän:

| Tietueen | Määritys |
| ------------- | ------------- |
| CallrecTime | Puhelun aikaleima aloitusaika. |
| SwitchNum | Yhdistää puhelun puhelinkeskuksen. |
| CallingNum | Soittajan puhelinnumero. |
| CallingIMSI | Kansainvälinen Mobile tilaaja tunnistetiedot (IMSIN).  Yksilöllinen tunnus kutsuja. |
| CalledNum | Puhelinnumero soitettaessa vastaanottajalle. |
| CalledIMSI | Kansainvälinen Mobile tilaaja tunnistetiedot (IMSIN).  Yksilöllinen tunnus soitettaessa vastaanottajalle. |


## <a name="create-a-stream-analytics-job"></a>Luo Stream Analytics-työ
Nyt, emme ole muodossa televiestinnän tapahtumista, emme määrittää Stream Analytics työn analysoida näitä tapahtumia reaaliajassa.

### <a name="provision-a-stream-analytics-job"></a>Valmistele Stream Analytics-työ

1.  Azure-portaalissa, valitse **Uusi** > **TIETOPALVELUJEN** > **STREAM ANALYTICS** > **Nopea luominen**.
2.  Määritä seuraavat arvot ja valitse sitten **Luo STREAM ANALYTICS työ**:

    * **Työn nimi**: Kirjoita projektin nimi.

    * **Alue**: alue, jonka haluat suorittaa työn. Harkitse siten työ- ja tapahtumaa-toiminnossa samalla alueella, jotta parempi suorituskyky ja haluat varmistaa, että sinun ei maksat tiedonsiirron alueiden välillä.

    * **TALLENNUSTILAN tilin**: Valitse Azure-tallennustilan tilin, jotka haluat tallentaa kaikki Stream Analytics-työt, jotka suoritetaan tämän alueen seurantatiedot. Voit halutessasi valita käytössä olevan tallennustilan-tilin tai luoda uuden.

3.  Valitse vasemmanpuoleisessa ruudussa luettelon Stream Analytics työt **STREAM ANALYTICS** .

    ![Virta Analytics service-kuvake](./media/stream-analytics-real-time-fraud-detection/stream-analytics-service-icon.png)

    Uusi projekti näytetään tila on **luotu**. Huomaa, että sivun alareunaan **Käynnistä** -painiketta ei ole käytössä. Työn syöte, tulostus ja kysely on määritettävä, ennen kuin aloitat työn.

### <a name="specify-job-input"></a>Työn syöte
1.  Työtäsi Stream Analytics- **SYÖTTEIDEN** sivun yläreunassa ja valitse sitten **Lisää SYÖTE**. Näkyviin tulevan valintaikkunan opastaa syötettäsi määrittämisen useita vaiheita.
2.  Valitse **TIETOVIRTA**, ja valitse sitten oikealle-painike.
3.  Napsauta **Tapahtumaa-TOIMINNOSSA**ja valitse sitten oikealle-painike.
4.  Kirjoita tai valitse kolmannella sivulla seuraavat arvot:

    * **SYÖTTEEN ALIAS**: Kirjoita kutsumanimi, kuten *CallStream*tälle projektille. Huomaa, että käytät tätä nimeä kyselyn myöhemmin.

    * **Tapahtuma-TOIMINNOSSA**: Jos, jonka loit tapahtumaa-toiminnossa on samassa muodossa Analytics-työ-tilauksen, valitse nimitilan, joka on tapahtumaa-toiminnossa.

        Jos tapahtumaa-toiminnossa on eri tilauksen, valitse **Käytä tapahtumaa-toiminnossa toiseen tilaukseen**ja anna **Palvelun BUS nimitila**, **Tapahtuman KESKITTIMEEN nimi**, **Tapahtuman KESKITTIMEEN käytännön nimi**, **Tapahtuman KESKITTIMEEN käytännön avain**ja **Tapahtumien KESKITTIMEEN OSION määrä**tiedot manuaalisesti.

    * **Tapahtuman KESKITTIMEEN nimi**: Valitse tapahtumaa-toiminnossa nimi.

    * **Tapahtuman KESKITTIMEEN käytännön nimi**: Valitse tapahtuma-toiminnossa käytäntö, jonka loit aiemmin tässä opetusohjelmassa.

    * **Tapahtuman KESKITTIMEEN KULUTTAJA ryhmän**: kuluttaja-ryhmä, jonka loit aiemmin tässä opetusohjelmassa nimi.

5.  Napsauta hiiren kakkospainikkeella.
6.  Määritä seuraavat arvot:

    * **Tapahtuman SARJATOIMINTO muoto**: JSON
    * **KOODAUS**: UTF8
7.  Valitse **Tarkista** -painike, Lisää tämä lähde ja tarkista, että Stream Analytics voit muodostaa tapahtumaa-toiminnossa.

### <a name="specify-job-query"></a>Määritä projektin kysely

Virta Analytics tukee yksinkertainen, määritettäviä kysely-malli, joka kuvaa muunnoksia reaaliaikainen käsittelyä varten. Saat lisätietoja kielen Hae [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/dn834998.aspx). Tässä opetusohjelmassa auttaa sinua tekijän ja testaa useiden kyselyjen oman reaaliaikainen stream puhelun tietojen päälle.

#### <a name="optional-sample-input-data"></a>Valinnainen: Kirjoita mallitiedot
Vahvistamiseen kyselyn vastaan todellinen työ tietojen avulla voit **MALLITIEDOT** ominaisuus poimia tapahtumien lisääminen virta ja luo. JSON-tiedoston tilanteista testikäyttöön.  Seuraavissa vaiheissa kuvataan tähän. On annettu myös [telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json) mallitiedosto testausta varten.

1.  Valitse tapahtuma-toiminnossa syöte ja valitse sitten **MALLITIEDOT** sivun alareunassa.
2.  Määritä avautuvassa valintaruudussa **ALOITUSAIKA** Aloita kerätä tietoja ja kuinka paljon uusia tietoja tarjoaman **kesto** .
3.  Esimerkkejä tietojen käynnistäminen syötteen valitsemalla **Tarkista** .  Se voi kestää muutaman minuutin kaksi, voidaan tuottaa datatiedosto.  Kun prosessi on valmis, valitse **tiedot**, lataa luotu. JSON tiedosto ja tallenna se.

    ![Lataa ja käsitellyt tiedot tallennetaan JSON-tiedostoon](./media/stream-analytics-real-time-fraud-detection/stream-analytics-download-save-json-file.png)

#### <a name="pass-through-query"></a>Läpivientikysely

Jos haluat arkistoida jokaisen tapahtuman, voit lukea viestin tai tapahtuman tiedot kaikkia kenttiä läpivientikysely. Alkavan, tehdä yksinkertaisia läpivientikysely, projektien tapahtuman kaikkia kenttiä.

1.  Valitse **KYSELYN** Stream Analytics työ-sivun yläreunasta.
2.  Lisää seuraavat koodieditorin:

        SELECT * FROM CallStream

    > [AZURE.IMPORTANT] Varmista, että syötteen lähteen nimi vastaa syöte, jonka olet määrittänyt aiemmin nimeä.

3.  Valitse **Testaa** kohdassa kyselyeditori.
4.  Anna testitiedosto. Käytä itse käyttämällä edellisessä vaiheessa luomasi, tai [telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/SampleDataFiles/Telco.json).
5.  Valitse **Tarkista** -painike sekä tarkastella tuloksia kyselymääritystä alapuolella.

    ![Kyselytulokset määritys](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sim-fraud-output.png)


### <a name="column-projection"></a>Sarakkeen ennuste

On nyt vähentää palautettujen kentät pienempi.

1.  Koodieditori kyselyn muuttaminen:

        SELECT CallRecTime, SwitchNum, CallingIMSI, CallingNum, CalledNum
        FROM CallStream

2.  **Suorita** kohdassa kyselyeditori näet kyselyn tulokset.

    ![Kyselyeditori-tulos.](./media/stream-analytics-real-time-fraud-detection/stream-analytics-query-editor-output.png)

### <a name="count-of-incoming-calls-by-region-tumbling-window-with-aggregation"></a>Laske saapuvien puheluiden alueittain: Tumbling, joissa on kooste

Voit verrata saapuvat puhelut alueittain määrä-käytetään [TumblingWindow](https://msdn.microsoft.com/library/azure/dn835055.aspx) saat saapuvia puheluita, jotka on ryhmitelty **SwitchNum** viiden sekunnin välein määrää.

1.  Koodieditori kyselyn muuttaminen:

        SELECT System.Timestamp as WindowEnd, SwitchNum, COUNT(*) as CallCount
        FROM CallStream TIMESTAMP BY CallRecTime
        GROUP BY TUMBLINGWINDOW(s, 5), SwitchNum

    Tämä kysely käyttää **Aikaleima mukaan** -avainsanan Aikaleimakenttä on määritettävä ajallinen laskenta käytettävät tiedot. Jos kentässä ei ole määritetty, windowing toiminto suoritettava käyttämällä aika, joka kuhunkin tapahtumaan on saapunut tapahtumaa-toiminnossa. Katso ["Saapumisen ja sovelluksen aika" Stream Analytics kyselyn kieliohje](https://msdn.microsoft.com/library/azure/dn834998.aspx).

    Huomaa, että voit käyttää jokaisen ikkunan loppuun aikaleima **System.Timestamp** -ominaisuuden avulla.

2.  **Suorita** kohdassa kyselyeditori näet kyselyn tulokset.

    ![Kyselytulokset Timestand mukaan](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-query-editor-rerun.png)

### <a name="sim-fraud-detection-with-a-self-join"></a>SIM petoksen tunnistaminen ja itse liittyminen

Laji mahdollisesti väärennetty käyttö, näet puheluihin, jotka ovat peräisin sama käyttäjä mutta eri sijainneissa pienempi kuin 5 sekuntia.  Olemme [Liity](https://msdn.microsoft.com/library/azure/dn835026.aspx) puhelun tapahtumat itse Tarkista ongelmatapauksissa muodossa.

1.  Koodieditori kyselyn muuttaminen:

        SELECT System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1,
        CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
        JOIN CallStream CS2 TIMESTAMP BY CallRecTime
        ON CS1.CallingIMSI = CS2.CallingIMSI
        AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
        WHERE CS1.SwitchNum != CS2.SwitchNum

2.  **Suorita** kohdassa kyselyeditori näet kyselyn tulokset.

    ![Liitoksen kyselytulokset](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-query-editor-join.png)

### <a name="create-output-sink"></a>Luo tulosteen käsittelytoiminto

Nyt kun tapahtuma-muodossa, ingest tapahtuma- ja kysely, joka suorittaa muunnoksen virta-syöte tapahtumaa-toiminnossa määritimme viimeinen vaihe on Määritä tulostus-käsittelytoiminto projektille. Olemme kirjoittaa vilpilliseen toiminta tapahtumien Azure-Blob-säiliö.

Seuraavien vaiheiden avulla voit luoda Blob-objektien tallennustilaan säilö, jos sinulla ei ole vielä yksi.

1.  Käytä aiemmin luotua tallennustilan-tiliä tai Luo uusi tallennustilan tili valitsemalla **Uusi > TIETOPALVELUJEN > TALLENNUSTILAN > nopea luominen**, ja noudata ohjeita.
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

## <a name="start-job-for-real-time-processing"></a>Aloita työ reaaliaikainen käsittelyä varten

Koska työn syöte, kyselyssä ja tulos on kaikki määritetty, emme jatkaa Stream Analytics työn reaaliaikainen petoksen tunnistamista varten.

1.  Projektin **raporttinäkymät-IKKUNAN**Valitse sivun alareunassa **Käynnistä-painiketta** .
2.  Valitse avautuvassa valintaruudussa **työn ALKAMISAIKA**ja valitse sitten valintaikkunan alareunassa **Tarkista** -painiketta. Työn tilaksi muuttuu **Aloitus** ja pian muuttaa **käynnissä**.

## <a name="view-fraud-detection-output"></a>Näytä petoksen tunnistus tuloste

Työkalu, kuten [Azure-tallennustilan Explorer](http://storageexplorer.com/) tai [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) avulla voit tarkastella vilpilliseen tapahtumia reaaliajassa jaettavaan sellaisenaan.  

![Ilmoituksen tunnistus: vilpilliseen tapahtumien tarkastella reaaliajassa](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-view-real-time-fraudent-events.png)

## <a name="get-support"></a>Käytä tukipalveluita
Saat lisäohjeita voit kokeilla Microsoftin [Azure Stream Analytics-keskustelupalstalla](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Seuraavat vaiheet

- [Azure Stream Analytics esittely](stream-analytics-introduction.md)
- [Skaalaa Azure Stream Analytics töitä](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics kyselyn kieliohje](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics hallinta REST API-viittaus](https://msdn.microsoft.com/library/azure/dn835031.aspx)
