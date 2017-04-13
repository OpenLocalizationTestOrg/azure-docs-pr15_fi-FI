<properties
    pageTitle="Virta Analytics käytön aloittaminen: reaaliaikainen petoksen tunnistus | Microsoft Azure"
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

Opettele luomaan lopusta loppuun-ratkaisun reaaliaikainen petoksen tunnistaminen Azure Stream Analytics kanssa. Azure tapahtumaa-toiminnossa tuo tapahtumia, kirjoittaa kooste tai ilmoitat Stream Analytics kyselyt ja lähetä tulokset tulostus-käsittelytoiminto, hanki tietoja tietojen kanssa reaaliajassa käsittely päälle. Reaaliaikainen poikkeavuuksista tunnistus tietoliikennettä varten tarkoitettujen mutta Esimerkki-tekniikka sopii tasaisesti muuntyyppisten petoksen tunnistus kuten luottokorttia tai käyttäjätiedot varkaus skenaarioita.

Virta Analytics on pieni viive, helposti saatavilla, skaalattava monimutkaisia tapahtuman käsittely antamisen päälle streaming tiedot pilvipalveluun täysin hallitun palvelu. Lisätietoja on artikkelissa [Azure Stream Analytics esittely](stream-analytics-introduction.md).


## <a name="scenario-telecommunications-and-sim-fraud-detection-in-real-time"></a>Skenaario: Televiestintä ja SIM petoksen tunnistus reaaliajassa

Televiestinnän yrityksellä on erittäin paljon tietoja saapuvia puheluita. Yrityksen täytyy sen tiedoista seuraavasti:
* Vertaa tiedoista alaspäin helpommin hallittaviin summa ja saada tietoa asiakkaan käyttö ajan ja maantieteellisillä alueilla.
* Tunnista SIM petoksen (useita kutsuja tulevat tunnistetiedot samaan aikaan, mutta maantieteellisesti eri sijainneissa) reaaliajassa niin, että ne helposti vastata siihen luomalla ilmoituksen asiakkaiden tai palvelun suljetaan.

Kanoninen siellä tilanteissa asioita Internet (IoT) tonni telemetriatietojen tai tunnistimen tiedoista luodaan – ja asiakkaiden haluat koostaa ne tai Ilmoita poikkeamia reaaliajassa päälle.

## <a name="prerequisites"></a>Edellytykset

- Lataa [TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip) Microsoft Download Centeristä 
- Valinnainen: Tapahtuma-luontitoiminnon [GitHub](https://github.com/Azure/azure-stream-analytics/tree/master/DataGenerators/TelcoGenerator) -lähdekoodi

## <a name="create-an-azure-event-hubs-input-and-consumer-group"></a>Luo Azure tapahtuman keskittimet-syöte ja kuluttaja-ryhmä

Sovelluksen malli luo tapahtumia ja push-tapahtumaa-toiminnossa esiintymä reaaliaikainen käsittelyä varten. Palvelun Bus tapahtuman keskittimet on suositeltavampaa tapahtuman annosmuuntokertoimien Stream analysoinnissa ja lisätietoja [Azure palvelun Bus](/documentation/services/service-bus/)ohjeissa keskittimet tapahtuman tietoja.

Voit luoda tapahtumaa-toiminnossa seuraavasti:

1.  [Azure-portaali](https://manage.windowsazure.com/) valitsemalla **Uusi** > **Sovelluksen Services** > **Palvelun Bus** > **Tapahtumaa-toiminnossa** > **Nopea luominen**. Anna nimi, alue ja uuteen tai aiemmin luotuun nimitilan, Luo uusi tapahtuma-toiminnossa.  
2.  Paras käytäntö on Stream Analytics kunkin projektin pitäisi lukea yhden tapahtumaa-toiminnossa kuluttaja-ryhmän. Olemme edetään ohjatusti alla kuluttaja ryhmän luominen ja voit [tietää lisää ryhmiä](https://msdn.microsoft.com/library/azure/dn836025.aspx). Kuluttaja ryhmän luominen Siirry juuri luomasi tapahtumaa-toiminnossa ja **Ryhmiä** -välilehti ja valitse sivun alaosassa **Luo** ja kuluttaja-ryhmän nimi.
3.  Tapahtuma-toiminnossa käyttöoikeuden myöntäminen annettava jaetun access-käytännön luominen.  Valitse tapahtuma-toiminnossa **määritys** -välilehti.
4.  Luo uuden käytännön **Jaettu käytäntöjen** **hallinta** käyttöoikeuksien avulla.

    ![Jaettu Access käytäntöjä, jossa voit luoda käyttöoikeuksien hallinta käytännön.](./media/stream-analytics-get-started/stream-ananlytics-shared-access-policies.png)

5.  Valitse sivun alareunassa **Tallenna** .
6.  Siirry **raporttinäkymät-ikkunan** ja valitse sivun alareunassa **Yhteystiedot** ja kopioi sitten ja Tallenna yhteystiedot.

## <a name="configure-and-start-event-generator-application"></a>Määritä ja käynnistää tapahtuman luominen

On annettu asiakassovellus, joka luo otoksen saapuvan puhelun metatietojen ja siirtää tapahtumaa-toiminnossa. Noudata seuraavia ohjeita voit määrittää tämän sovelluksen.  

1.  Lataa [TelcoGenerator.zip-tiedosto](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip). Pura se sitten kansio.

    **Huomautus**: Windows saattavat estää ladatut zip-tiedoston. Napsauta tiedostoa hiiren kakkospainikkeella ja valitse Ominaisuudet. Jos viesti "tiedosto oli toisesta tietokoneesta, voi olla estetty tämän tietokoneen suojaamiseen." tick "Salli"-ruutuun ja valitse Käytä zip-tiedoston avulla.

2.  Korvaa **telcodatagen.exe.config** Microsoft.ServiceBus.ConnectionString ja EventHubName arvot tapahtumaa-toiminnossa yhteysmerkkijonon ja nimi.

    **Huomautus**: yhteysmerkkijonon kopioitu Azure portaalin paikasta lopussa yhteyden nimeä. Muista poistaa "; EntityPath =<value>"Lisää Key = kenttä.

3.  Käynnistä sovellus. Käyttö on seuraavanlainen:

   telcodatagen.exe #NumCDRsPerHour [SIM kortin petoksen todennäköisyys] [#DurationHours]

Seuraavassa esimerkissä Luo 1000 tapahtumien 20 prosenttia petoksen todennäköisyydellä 2 tuntia tilistäsi.

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


## <a name="create-stream-analytics-job"></a>Luo Stream Analytics-työ
Nyt, emme ole muodossa televiestinnän tapahtumista, emme määrittää Stream Analytics työn analysoida näitä tapahtumia reaaliajassa.

### <a name="provision-a-stream-analytics-job"></a>Valmistele Stream Analytics-työ

1.  Valitse Azure-portaalissa **Uusi > tietopalvelujen > Stream Analytics > nopea luominen**.
2.  Määritä seuraavat arvot ja valitse sitten **Luo Stream Analytics työ**:

    * **Työn nimi**: Kirjoita projektin nimi.

    * **Alue**: alue, jonka haluat suorittaa työn. Harkitse siten työ- ja tapahtumaa-toiminnossa samalla alueella, jotta parempi suorituskyky ja haluat varmistaa, että sinun ei maksat tiedonsiirron alueiden välillä.

    * **Tallennustilan tilin**: Valitse Azure-tallennustilan tilin, jotka haluat tallentaa kaikki tämän alueen töitä Stream Analytics seurantatiedot. Voit halutessasi valita käytössä olevan tallennustilan-tilin tai luoda uuden.

3.  Valitse vasemmanpuoleisessa ruudussa luettelon Stream Analytics työt **Stream Analytics** .

    ![Virta Analytics service-kuvake](./media/stream-analytics-get-started/stream-analytics-service-icon.png)

4.  Uusi projekti näytetään tila on **luotu**. Huomaa, että sivun alareunaan **Käynnistä** -painiketta ei ole käytössä. Työn syöte, tulostus ja kysely on määritettävä, ennen kuin aloitat työn.

### <a name="specify-job-input"></a>Työn syöte
1.  Virta Analytics-työn **syötteiden** sivun yläreunasta ja valitse sitten **Lisää syöte**. Näkyviin tulevan valintaikkunan käy läpi määrä syötettäsi määrittämisen vaiheita.
2.  Valitse **tietovirta**, ja valitse sitten oikealle-painike.
3.  Valitse **Tapahtuma-toiminnosta**ja valitse sitten oikealle-painike.
4.  Kirjoita tai valitse kolmannella sivulla seuraavat arvot:

    * **Syötteen Alias**: Kirjoita kutsumanimi työlle, kuten *CallStream*syöte. Huomaa, että käytät tätä nimeä kyselyn myöhemmin.
    * **Tapahtuma-toiminnossa**: Jos olet luonut tapahtumaa-toiminnossa on samassa muodossa Analytics-työ-tilauksen, valitse nimitilan, joka on tapahtumaa-toiminnossa.

    Jos tapahtuma-toiminnossa on eri tilauksen, valitse **Käytä tapahtumaa-toiminnossa toiseen tilaukseen** ja kirjoita manuaalisesti tiedot **Palvelun Bus Namespace**, **Tapahtuman keskittimeen nimi**, **Tapahtuman keskittimeen käytännön nimi**, **Tapahtuman keskittimeen käytännön avain**ja **Tapahtumien keskittimeen osion määrä**.

    * **Tapahtuman keskittimeen nimi**: Valitse tapahtumaa-toiminnossa nimi.

    * **Tapahtuman keskittimeen käytännön nimi**: Valitse tapahtumaa-toiminnossa käytäntö loit aiemmin tässä opetusohjelmassa.

    * **Tapahtuman keskittimeen kuluttaja ryhmän**: Kirjoita loit aiemmin tässä opetusohjelmassa kuluttaja-ryhmä.
5.  Napsauta hiiren kakkospainikkeella.
6.  Määritä seuraavat arvot:

    * **Tapahtuman Sarjatoiminto muoto**: JSON
    * **Koodaus**: UTF8
7.  Valitse Tarkista-painiketta, voit lisätä tämän lähteen ja tarkista, että Stream Analytics voit muodostaa tapahtumaa-toiminnossa.

### <a name="specify-job-query"></a>Määritä projektin kysely

Virta Analytics tukee yksinkertaisia ja määritettäviä kyselyn mallin kuvaava muunnoksia reaaliaikainen käsittelyä varten. Saat lisätietoja kielen Hae [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/dn834998.aspx). Tässä opetusohjelmassa auttaa sinua tekijän ja testaa useiden kyselyjen oman reaaliaikainen stream puhelun tietojen päälle.

#### <a name="optional-sample-input-data"></a>Valinnainen: Kirjoita mallitiedot
Vahvistamiseen kyselyn vastaan todellinen työ tietojen avulla voit **Mallitiedot** ominaisuus poimia tapahtumien lisääminen virta ja luo. JSON-tiedoston tilanteista testikäyttöön.  Seuraavissa vaiheissa kuvataan hallinnasta ja on annettu [telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json) mallitiedosto testausta varten.

1.  Valitse tapahtumaa-toiminnossa-syöte ja valitse sivun alareunassa **Mallitiedot** .
2.  Määritä **Alkamisaika** Aloita kerätä tietoja ja **keston** lisätietoja, kuinka paljon tarjoaman tulevasta valintaikkunasta.
3.  Esimerkkejä tietojen käynnistäminen syötteen valitsemalla valintaruutu.  Se voi kestää muutaman minuutin kaksi, voidaan tuottaa datatiedosto.  Kun prosessi on valmis, valitse **tiedot** ja lataa ja tallentaa. JSON-tiedosto, joka on luotu.

    ![Lataa ja käsitellyt tiedot tallennetaan JSON-tiedostoon](./media/stream-analytics-get-started/stream-analytics-download-save-json-file.png)

#### <a name="passthrough-query"></a>Läpivienti-kysely

Jos haluat arkistoida jokaisen tapahtuman, voit lukea viestin tai tapahtuman tiedot kaikkia kenttiä läpivienti kyselyn. Tee yksinkertainen läpivienti kyselyn aluksi kyseisen projektien tapahtuman kaikkia kenttiä.

1.  Valitse **kyselyn** Stream Analytics työ-sivun yläreunasta.
2.  Lisää seuraavat koodieditorin:

        SELECT * FROM CallStream

    > Varmista, että syötteen lähteen nimi vastaa entisellään syötteen nimi.

3.  Valitse **Testaa** kohdassa kyselyeditori.
4.  Anna testitiedosto-joko sellaisen, jonka avulla edellisessä vaiheessa luomasi tai käyttää [telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json).
5.  Valitse Tarkista-painiketta sekä tarkastella tuloksia kyselymääritystä alapuolella.

    ![Kyselytulokset määritys](./media/stream-analytics-get-started/stream-analytics-sim-fraud-output.png)


### <a name="column-projection"></a>Sarakkeen ennuste

Microsoft nyt vertaa alaspäin pienempi palautetut kentät.

1.  Koodieditori kyselyn muuttaminen:

        SELECT CallRecTime, SwitchNum, CallingIMSI, CallingNum, CalledNum
        FROM CallStream

2.  **Suorita** kohdassa kyselyeditori näet kyselyn tulokset.

    ![Kyselyeditori-tulos.](./media/stream-analytics-get-started/stream-analytics-query-editor-output.png)

### <a name="count-of-incoming-calls-by-region-tumbling-window-with-aggregation"></a>Laske saapuvien puheluiden alueittain: Tumbling, joissa on kooste

Haluat verrata, että saapuvat puhelut alueittain olemme hyödyntää [TumblingWindow](https://msdn.microsoft.com/library/azure/dn835055.aspx) saat saapuvia puheluita, jotka on ryhmitelty SwitchNum viiden sekunnin välein määrää.

1.  Koodieditori kyselyn muuttaminen:

        SELECT System.Timestamp as WindowEnd, SwitchNum, COUNT(*) as CallCount
        FROM CallStream TIMESTAMP BY CallRecTime
        GROUP BY TUMBLINGWINDOW(s, 5), SwitchNum

    Tämä kysely käyttää **Aikaleima mukaan** -avainsanan Aikaleimakenttä on määritettävä ajallinen laskenta käytettävät tiedot. Jos kentässä ei ole määritetty, windowing-toimintoa voi suorittaa tapahtumaa-toiminnossa saapunut kuhunkin tapahtumaan aikaa. Katso ["Saapumisen ja sovelluksen aika" Stream Analytics kyselyn kieliohje](https://msdn.microsoft.com/library/azure/dn834998.aspx).

    Huomaa, että voit käyttää jokaisen ikkunan loppuun aikaleima **System.Timestamp** -ominaisuuden avulla.

2.  **Suorita** kohdassa kyselyeditori näet kyselyn tulokset.

    ![Kyselytulokset Timestand mukaan](./media/stream-analytics-get-started/stream-ananlytics-query-editor-rerun.png)

### <a name="sim-fraud-detection-with-a-self-join"></a>SIM petoksen tunnistaminen ja itse liittyminen

Tunnistaa mahdollisesti väärennetty käyttö näet puheluihin peräisin sama käyttäjä mutta eri sijainneissa pienempi kuin 5 sekuntia.  Olemme [Liity](https://msdn.microsoft.com/library/azure/dn835026.aspx) puhelun tapahtumat itse Tarkista ongelmatapauksissa muodossa.

1.  Koodieditori kyselyn muuttaminen:

        SELECT System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1,
        CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
        JOIN CallStream CS2 TIMESTAMP BY CallRecTime
        ON CS1.CallingIMSI = CS2.CallingIMSI
        AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
        WHERE CS1.SwitchNum != CS2.SwitchNum

2.  **Suorita** kohdassa kyselyeditori näet kyselyn tulokset.

    ![Liitoksen kyselytulokset](./media/stream-analytics-get-started/stream-ananlytics-query-editor-join.png)

### <a name="create-output-sink"></a>Luo tulosteen käsittelytoiminto

Nyt kun tapahtuma-muodossa-tapahtumaa-toiminnossa syöte ingest tapahtumista ja kyselyn suorittaa muunnoksen virta-määritimme viimeinen vaihe on Määritä tulostus-käsittelytoiminto projektille.  Olemme kirjoittaa vilpilliseen toiminta tapahtumien Blob-objektien tallennustilaan.

Noudata seuraavia ohjeita voit luoda Blob-objektien tallennustilaan säilö, jos sinulla ei ole vielä yksi.

1.  Käytä aiemmin luotua tallennustilan-tiliä tai Luo uusi tallennustilan tili valitsemalla **Uusi > TIETOPALVELUJEN > TALLENNUSTILAN > nopea luominen** ja noudattamalla ohjeita.
2.  Tallennustilan-tili, valitse **SÄILÖJEN** sivun yläreunassa ja valitse sitten **Lisää**.
3.  Oman säilön **nimi** ja määritä julkinen Blob **ACCESS** .

## <a name="specify-job-output"></a>Määritä projektin tulostus

1.  Virta Analytics-työn **tulostus** sivun yläreunasta ja valitse sitten **Lisää tulos**. Näkyviin tulevan valintaikkunan käy läpi määrä jaettavaan määrittämisen vaiheita.
2.  Valitse **BLOB-SÄILIÖ**ja valitse sitten oikealle-painike.
3.  Kirjoita tai valitse kolmannella sivulla seuraavat arvot:

    * **Tunnus**: Kirjoita kutsumanimi työ-tulostusta varten.
    * **TILAUS**: Jos olet luonut Blob-säiliö on samassa muodossa Analytics-työ-tilauksen, valitse **Käytä tallennustilan tilin voimassa oleva tilaus**. Jos tallennustilaa on eri tilauksen, valitse **Käytä tallennustilan tilin toiseen tilaukseen** ja kirjoita manuaalisesti tiedot **TALLENNUSTILAN tilin**- **TALLENNUSTILAN tilin AVAIMEN** **SÄILÖ**.
    * **TALLENNUSTILAN tilin**: Valitse tallennustilan tilin nimi.
    * **SÄILÖN**: Valitse säilön nimi.
    * **Tiedostonimi ETULIITE**: Kirjoita tiedosto etuliite käyttämään blob tulosteen kirjoitettaessa.

4.  Napsauta hiiren kakkospainikkeella.
5.  Määritä seuraavat arvot:

    * **Tapahtuman SARJATOIMINTO muoto**: JSON
    * **KOODAUS**: UTF8

6.  Valitse Tarkista-painiketta, voit lisätä tämän lähteen ja tarkista, että Stream Analytics voit muodostaa tallennustilan tilin.

## <a name="start-job-for-real-time-processing"></a>Aloita työ reaaliaikainen käsittelyä varten

Koska työn syöte, kyselyssä ja tulos on kaikki määritetty, emme jatkaa Stream Analytics työn reaaliaikainen petoksen tunnistamista varten.

1.  Projektin **raporttinäkymät-IKKUNAN**Valitse sivun alareunassa **Käynnistä-painiketta** .
2.  Valitse tulevasta valintaikkunasta **työn ALKAMISAIKA** ja valitse valintaikkunan alareunassa Tarkista-painiketta. Työn tilaksi muuttuu **Aloitus** ja siirtyvät pian **käynnissä**.

## <a name="view-fraud-detection-output"></a>Näytä petoksen tunnistus tuloste

Työkalu, kuten [Azure-tallennustilan Explorer](https://azurestorageexplorer.codeplex.com/) tai [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) avulla voit tarkastella vilpilliseen tapahtumia reaaliajassa jaettavaan sellaisenaan.  

![Ilmoituksen tunnistus: vilpilliseen tapahtumien tarkastella reaaliajassa](./media/stream-analytics-get-started/stream-ananlytics-view-real-time-fraudent-events.png)

## <a name="get-support"></a>Käytä tukipalveluita
Saat lisäohjeita voit kokeilla Microsoftin [Azure Stream Analytics-keskustelupalstalla](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Seuraavat vaiheet

- [Azure Stream Analytics esittely](stream-analytics-introduction.md)
- [Azure Stream Analytics käytön aloittaminen](stream-analytics-get-started.md)
- [Skaalaa Azure Stream Analytics töitä](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics kyselyn kieliohje](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics hallinta REST API-viittaus](https://msdn.microsoft.com/library/azure/dn835031.aspx)
