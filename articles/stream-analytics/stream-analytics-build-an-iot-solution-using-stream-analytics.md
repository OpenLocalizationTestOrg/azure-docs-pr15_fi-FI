<properties
    pageTitle="Voit muodostaa IoT-ratkaisun Stream Analytics | Microsoft Azure"
    description="Aloittaminen opetusohjelma maksupisteen skenaarion muodossa Analytics IoT-ratkaisun"
    keywords="IOT ratkaisu-ikkunassa Funktiot"
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>

# <a name="build-an-iot-solution-by-using-stream-analytics"></a>Voit muodostaa IoT-ratkaisun Stream Analytics

## <a name="introduction"></a>Johdanto

Tässä opetusohjelmassa opit noutaa reaaliaikaisia tietoja tiedoista Azure Stream Analytics avulla. Kehittäjät helposti yhdistää tietoja, kuten napsauttamalla virtaa lokit ja laitteen luomiin tapahtumiin virtaa historiatiedoissa tai viitetiedot saada business tiedot. Täysin hallitun, reaaliaikainen stream laskenta palvelu, jota isännöidään Microsoft Azure-Azure Stream Analytics sisältää valmiita vikasietoisuudesta, pieni viive ja skaalattavuus voit ylöspäin ja minuutin käynnissä.

Kun olet suorittanut tämän opetusohjelman, saa oikeuden:

-   Valintanauhaan tutustuminen Azure Stream Analytics-portaalissa.
-   Määritä ja ota streaming työn.
-   Articulate tosielämän ongelmia ja ratkaista ne Stream Analytics-kyselykieltä käyttäen.
-   Kehittää streaming ratkaisuja käyttämällä Stream Analytics LUOTTAMUSVÄLI asiakkaillesi.
-   Ongelmien vianmääritys seurantaa ja kirjaamisen kokemus avulla.

## <a name="prerequisites"></a>Edellytykset

Sinun on suoritettava tässä opetusohjelmassa seuraavat edellytykset:

-   [PowerShellin Azure](../powershell-install-configure.md) uusin versio
-   Visual Studio 2015 tai vapaan [Visual Studio yhteisön](https://www.visualstudio.com/products/visual-studio-community-vs.aspx)
-   On [Azure tilaus](https://azure.microsoft.com/pricing/free-trial/)
-   Tietokoneen järjestelmänvalvojan oikeudet
-   Lataa [TollApp.zip](http://download.microsoft.com/download/D/4/A/D4A3C379-65E8-494F-A8C5-79303FD43B0A/TollApp.zip) Microsoft Download Centeristä.
-   Valinnainen: Lähdekoodin [GitHub](https://aka.ms/azure-stream-analytics-toll-source) TollApp tapahtuma-luontitoiminto

## <a name="scenario-introduction-hello-toll"></a>Skenaario esittely: "Hei, maksullinen!"


Maksullinen aseman on yleisiä ilmiö. Selaat ne monta pikateiden, siltoja ja tunnelit kaikkialla maailmassa. Maksullinen jokaisen aseman on useita maksullinen booths. Manuaalinen booths, voit lopettaa maksaa maksullinen attendant. Milloin automaattisen booths tunnistimen kunkin booth päälle tarkistaa RFID-kortti, joka on kiinnitetty oman ajoneuvon windshield kun siirrät maksullinen booth. On helppo visualisointi – maksullinen nämä asemat ajoneuvot tekstikatkelman kuin tulosjoukko, jolle kiinnostavat toimintoja voi käyttää tapahtuma-muodossa.

![Autojen osoitteessa maksullinen booths kuva](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image1.jpg)

## <a name="incoming-data"></a>Saapuvat tiedot

Tässä opetusohjelmassa toimii kaksi virtaa tiedot. Aloitus ja Lopetus maksullinen asemat on asennettu anturit tuottaa ensimmäisen muodossa. Toisessa muodossa on staattinen haku-tietojoukko, joka on ajoneuvon rekisteröintitietojen.

### <a name="entry-data-stream"></a>Tapahtuma-tietovirta

Tapahtuma-tietovirta sisältää tietoja autot, kun he kirjoittavat maksullinen asemat.


| TollID | EntryTime               | LicensePlate | Tila | Tee   | Malli   | VehicleType | VehicleWeight | Maksullinen | Tunniste       |
|--------|-------------------------|--------------|-------|--------|---------|-------------|---------------|------|-----------|
| 1      | 2014-09-10 12:01:00.000 | JNB 7001     | NY    | Honda  | CRV     | 1           | 0             | 7    |           |
| 1      | 2014-09-10 12:02:00.000 | YXZ 1001     | NY    | Toyota | Camry   | 1           | 0             | 4    | 123456789 |
| 3      | 2014-09-10 12:02:00.000 | ABC 1004     | CT    | Ford   | Taurus  | 1           | 0             | 5    | 456789123 |
| 2      | 2014-09-10 12:03:00.000 | XYZ 1003     | CT    | Toyota | Corolla | 1           | 0             | 4    |           |
| 1      | 2014-09-10 12:03:00.000 | BNJ 1007     | NY    | Honda  | CRV     | 1           | 0             | 5    | 789123456 |
| 2      | 2014-09-10 12:05:00.000 | CDE 1007     | NJ    | Toyota | 4 x 4     | 1           | 0             | 6    | 321987654 |


Seuraavassa on lyhyt kuvaus sarakkeet:

| Sarakkeen | Kuvaus |
|--------------|--------------------------------------------------------------------|
| TollID       | Maksullinen booth tunnus, joka yksilöi maksullinen booth            |
| EntryTime    | Päivämäärän ja kellonajan olevan ajoneuvon maksullinen booth UTC-muodossa. |
| LicensePlate | Ajoneuvon että käyttöoikeuksien määrä                            |
| Tila        | Yhdysvalloissa tila                                           |
| Tee         | Auto valmistajan                                 |
| Malli        | Mallin määrä Auto                                 |
| VehicleType  | Joko 1 henkilöajoneuvon tai kaupallisen ajoneuvot 2       |
| WeightType   | Ajoneuvon paino tonni; henkilöajoneuvon 0                   |
| Maksullinen         | Maksullinen arvon dollareina                                              |
| Tunniste          | E-tunniste-Auto, joka automatisoi maksu. tyhjä, jossa maksu määritetty manuaalisesti |


### <a name="exit-data-stream"></a>Lopeta tietovirta

Lopeta-tietovirta sisältää tietoja autojen jätä maksullinen-asema.


| **TollId** | **ExitTime**                 | **LicensePlate** |
|------------|------------------------------|------------------|
| 1          | 2014-09-10T12:03:00.0000000Z | JNB 7001         |
| 1          | 2014-09-10T12:03:00.0000000Z | YXZ 1001         |
| 3          | 2014-09-10T12:04:00.0000000Z | ABC 1004         |
| 2          | 2014-09-10T12:07:00.0000000Z | XYZ 1003         |
| 1          | 2014-09-10T12:08:00.0000000Z | BNJ 1007         |
| 2          | 2014-09-10T12:07:00.0000000Z | CDE 1007         |

Seuraavassa on lyhyt kuvaus sarakkeet:


| Sarakkeen | Kuvaus |
|--------------|-----------------------------------------------------------------|
| TollID       | Maksullinen booth tunnus, joka yksilöi maksullinen booth         |
| ExitTime     | Päivämäärä- ja Lopeta ajoneuvon maksullinen booth UTC-aika |
| LicensePlate | Ajoneuvon että käyttöoikeuksien määrä                         |

### <a name="commercial-vehicle-registration-data"></a>Kaupallisen ajoneuvon rekisteröintitietojen

Opetusohjelman käyttää kaupallisen ajoneuvon rekisteröinnin tietokannan staattisen tilannevedoksen.


| LicensePlate | RegistrationId | Vanhentunut |
|--------------|----------------|---------|
| SVT 6023     | 285429838      | 1       |
| XLZ 3463     | 362715656      | 0       |
| SKV 1005     | 876133137      | 1       |
| RIV 8632     | 992711956      | 0       |
| SNY 7188     | 592133890      | 0       |
| ELH 9896     | 678427724      | 1       |

Seuraavassa on lyhyt kuvaus sarakkeet:


| Sarakkeen | Kuvaus |
|--------------|-----------------------------------------------------------------|
| LicensePlate | Ajoneuvon että käyttöoikeuksien määrä                         |
| RegistrationId     | Ajoneuvon Tunnuksella |
| Vanhentunut | Ajoneuvon rekisteröinnin tila: 0, jos ajoneuvon rekisteröinti on aktiivinen, 1, jos rekisteröinti on vanhentunut                 |


## <a name="set-up-the-environment-for-azure-stream-analytics"></a>Azure Stream analysoinnissa ympäristön määrittäminen

Tässä opetusohjelmassa suorittamiseen tarvitset Microsoft Azure-tilaus. Microsoft tarjoaa Microsoft Azure-palveluiden maksuttoman kokeiluversion.

Jos sinulla ei ole Azure-tili, voit [pyynnön maksuttoman kokeiluversion](http://azure.microsoft.com/pricing/free-trial/).

> [AZURE.NOTE] Voit rekisteröityä maksuttoman kokeiluversion käyttäjäksi, tarvitset mobiililaitteella, joka voi vastaanottaa tekstiviestejä ja luottokortilla.

Muista noudattamalla tämän artikkelin lopussa "Puhdista Azure-tili"-osassa niin, että voit tehdä $-200 parhaat käyttö Azure hyvitys vapaasti.

## <a name="provision-azure-resources-required-for-the-tutorial"></a>Varaa Azure opetusohjelman tarvittavat resurssit

Tässä opetusohjelmassa edellyttää kahta tapahtuman keskittimet vastaanottamaan *tapahtuma* - ja *Lopeta* tietojen virtaa. Azure SQL-tietokantaan tulostaa Stream Analytics-töiden tulokset. Azure-tallennustilan tallentaa viittaus ajoneuvon merkintöjen tietoja.

Voit käyttää Setup.ps1 komentosarjaa GitHub TollApp-kansiossa Luo kaikki tarvittavat resurssit. Vuoksi aika on suositeltavaa, että suoritat sen. Jos haluat lisätietoja näistä resursseista määrittämisestä Azure-portaalissa, viittaa "Määrittäminen opetusohjelman resurssien Azure-portaalissa" lisäys.

Lataa ja Tallenna hyödyt [TollApp](http://download.microsoft.com/download/D/4/A/D4A3C379-65E8-494F-A8C5-79303FD43B0A/TollApp.zip) -kansio ja tiedostot.

Avaa **Microsoft Azure PowerShell** ikkunan _järjestelmänvalvojana_. Jos sinulla ei vielä ole PowerShellin Azure, noudata ohjeita [asennetaan ja määritetään PowerShellin Azure](../powershell-install-configure.md) asennusohjeet.

Koska Windows estää automaattisesti .ps1 .dll ja .exe-tiedostojen, sinun täytyy määrittää suorittamisen käytännön, ennen kuin suoritat komentosarjan. Varmista, että PowerShellin Azure-ikkuna on käynnissä _järjestelmänvalvojan oikeuksilla_. Suorita **määrittäminen suorituskäytäntöä rajoittamaton**. Kirjoita pyydettäessä **Y**.

![Näyttökuva "Määritä-suorituskäytäntöä rajoittamaton" käynnissä PowerShellin Azure-ikkunassa](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image2.png)

Suorittamalla **Get suorituskäytäntöä** varmistaaksesi, että komento työskenteli.

![Näyttökuva "Get-suorituskäytäntöä" käynnissä PowerShellin Azure-ikkunassa](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image3.png)

Siirry kansioon, jonka komentosarjojen ja luontitoiminto sovelluksen.

![Näyttökuva "CD-levylle .\TollApp\TollApp" käynnissä PowerShellin Azure-ikkunassa](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image4.png)

Kirjoita **.\\ Setup.ps1** Azure tilin määrittäminen, luominen ja määrittää kaikki tarvittavat resurssit ja aloita tapahtumien luominen. Komentosarjan vastataan satunnaisesti luominen resurssien alueen. Jos haluat määrittää alueen erikseen, voit siirtää **-sijainti** parametri, kuten seuraavassa esimerkissä:

**. \\Setup.ps1-sijainti "Keskitetyn Yhdysvallat"**

![Näyttökuva Azure-kirjautuminen](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image5.png)

Komentosarjan avautuu **Kirjaudu sisään** -sivulla Microsoft Azure varten. Kirjoita tilin tunnukset.

> [AZURE.NOTE] Jos tilisi käyttää useita tilauksia, sinua pyydetään antamaan tilauksen nimeä, jota haluat käyttää opetusohjelman.

Komentosarja voi kestää useita minuutteja, jos haluat suorittaa. Kun se päättyy, tulos pitäisi näyttää seuraavat näyttökuvan.

![Näyttökuva tulosteen komentosarja PowerShellin Azure-ikkunassa](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image6.PNG)

Näet myös toisessa ikkunassa, joka muistuttaa seuraavista näyttökuvan. Tämän sovelluksen lähettää tapahtumien Azure tapahtuman porttiin, joka suorittaa opetusohjelman vaaditaan. Näin on Älä sovellus tai sulje ikkuna, kunnes olet opetusohjelman.

![Näyttökuva "Lähettäminen tapahtuman keskittimeen tiedot"](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image7.png)

Sinun pitäisi nähdä kaikki luodut resurssit Azure-portaalissa nyt. Siirry <https://manage.windowsazure.com>ja kirjaudu sisään tilin tunnistetiedot.

### <a name="azure-event-hubs"></a>Azure tapahtuman keskittimet

Valitse **Palvelun BUS** vasemmassa reunassa Azure-portaalin näkevän tapahtuman keskittimet, jossa komentosarja on luotu edellisessä osassa.

![Palvelun Bus](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image8.png)

Näet kaikki saatavilla nimitilan-tilaukseesi. Valitse haluamasi vaihtoehto, joka alkaa *tolldata* (Tässä esimerkissä tolldata4637388511). Valitse **Tapahtuma KESKITTIMET** -välilehti.

![Tapahtuma keskittimet välilehden Azure-portaalissa](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image9.png)

Näyttöön tulee kaksi tapahtumaa keskittimet nimeltä *tapahtuma* ja *Lopeta* Tämä nimitila luotu.

![Näyttökuva "tekstin" ja "lopetus" tapahtuman keskittimet Azure-portaalissa](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image10.png)

### <a name="azure-storage-container"></a>Azure tallennustilan säilö

1. Valitse **TALLENNUSTILAN** vasemmassa reunassa Azure-portaalin nähdäksesi, jota käytetään opetusohjelman Azure-tallennustilan säilö.

    ![Tallennustilan-valikkovaihtoehto](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image11.png)

2. Valitse, jotka alkavat *tolldata* (Tässä esimerkissä tolldata4637388511). Valitse Nähdäksesi luotu säilö **SÄILÖT** -välilehti.

    ![Säilöjen välilehti Azure-portaalissa](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image12.png)

3. Valitse ladatut JSON-tiedoston, jossa on ajoneuvon rekisteröintitietojen **tolldata** säilö.

    ![Näyttökuva registration.json tiedosto säilöön](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image13.png)

### <a name="azure-sql-database"></a>Azure SQL-tietokanta

1. Valitse **SQL-TIETOKANTOJA** Azure-portaalin vasemmassa reunassa SQL-tietokanta, jota käytetään opetusohjelman näkevän.

    ![SQL-tietokannat](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image14.png)

2. Valitse **tolldatadb**.

    ![Näyttökuva luotu SQL-tietokantaan](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image15.png)

3. Kopioi SMTP-palvelimen nimi ilman porttinumero (*palvelimen nimi*. database.windows.net, esimerkiksi).

## <a name="connect-to-the-database-from-visual-studio"></a>Muodosta yhteys tietokantaan Visual Studio

Käytä Visual Studio kyselytulokset tulostus-tietokannassa.

Yhteyden muodostaminen SQL-tietokantaan (kohde) Visual Studio:

1. Avaa Visual Studio ja valitse sitten **Työkalut** > **Muodosta yhteys tietokantaan**.

2. Jos ohjelma kysyy, valitse **Microsoft SQL Server** tietolähteenä.

    ![Muuta tietolähde-valintaikkuna](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image16.png)

3. Liitä **palvelimen nimi** -kenttään nimi, joka on kopioitu edellisessä osassa Azure-portaalista (eli *palvelimen nimi*. database.windows.net).

4. Valitse **Käytä SQL Server-todennusta**.

5. Kirjoita **käyttäjänimi** -kenttään **tolladmin** ja **123toll!** Kirjoita **salasana** -kenttään.

6. Napsauta **Valitse tai Kirjoita tietokannan nimi**ja valitse **TollDataDB** kuin tietokanta.

    ![Lisää yhteys-valintaikkuna](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image17.jpg)

7. Valitse **OK**.

8. Avaa Resurssienhallinta palvelimeen.

    ![Palvelimen Explorer](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image18.png)

9. Katso TollDataDB tietokannan neljä taulukkoa.

    ![TollDataDB tietokannan taulukkoa](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image19.jpg)

## <a name="event-generator-tollapp-sample-project"></a>Tapahtuman luominen: TollApp otoksen projektin

PowerShell-komentosarjaa käynnistyy automaattisesti lähettää tapahtumien TollApp malli-sovelluksen avulla. Ei tarvitse tehdä mitään lisätoimia.

Jos olet kiinnostunut säännöt, olet löytänyt TollApp sovelluksen lähdekoodi-GitHub [Näytteiden/TollApp](https://aka.ms/azure-stream-analytics-toll-source).

![Näyttökuva otoksen koodi näkyvät Visual Studio](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image20.png)

## <a name="create-a-stream-analytics-job"></a>Luo Stream Analytics-työ

1. Azure-portaalissa Stream Analytics Avaa ja valitse **Uusi** Luo uuden analytics-työ sivun vasemmassa alakulmassa.

    ![Uusi-painike](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image21.png)

2. Valitse **Luo NOPEASTI**. Valitse missä muut resurssit luodaan komentosarjaa saman alue.

3. **ALUEELLISEN seuranta TALLENNUSTILAN tilin** -asetus, valitse **Luo uusi TALLENNUSTILAN tili** ja anna sille yksilöivä nimi. Azure Stream Analytics tallentaa kaikki tulevien projektien seurantatiedot käyttämällä tähän tiliin.

4. Valitse sivun alaosassa **Luo STREAM ANALYTICS työn** .

    ![Luo Stream Analytics työ-asetus](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image22.png)

## <a name="define-input-sources"></a>Määritä lähteitä

1. Valitse luotu analytics-työ-portaalissa.

    ![Näyttökuva analytics-työ-portaalissa](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image23.jpg)

2. Määritä lähdetiedot **SYÖTTEIDEN** -välilehdestä.

    ![Syötteiden-välilehti](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image24.jpg)

3. Valitse **Lisää SYÖTE**.

    ![Lisää Input-vaihtoehto](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image25.png)

4. Valitse **TIETOVIRTA** ensimmäisellä sivulla.

    ![Tietovirta-vaihtoehto](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image26.png)

5. Valitse ohjatun toiminnon toinen sivu **Tapahtumaa-TOIMINNOSSA** .

    ![Tapahtuma-toiminnossa-vaihtoehto](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image27.png)

6. Kirjoita **EntryStream** **NÄYTETTÄVÄ ALIAS**.

7. Napsauta **Tapahtumaa-toiminnossa** avattavasta ja valitse haluamasi vaihtoehto, joka alkaa sanalla "TollData" (esimerkiksi TollData9518658221).

8. Valitse **tapahtuma** tapahtuman keskittimeen nimi ja **kaikki** tapahtuman keskittimeen käytännön nimen.

    Näyttää asetukset seuraavasti:

    ![Tapahtuma-toiminnossa asetukset](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image28.png)

9. Valitse seuraavalla sivulla **JSON** **KOODAUS** **Tapahtuman SARJATOIMINTO muoto** ja **UTF8** .

    ![Sarjatoiminto asetukset](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image29.png)

10. Valitse **OK** sivun alareunassa ohjatun toiminnon loppuun.

    ![EntryStream näyttökuva syötteen Azure-portaalissa](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image30.jpg)

    Nyt kun olet luonut tapahtuma-muodossa, voit noudattaa samoja vaiheita luominen Lopeta-muodossa. Valitse ohjatun toiminnon kolmannella sivulla muista kirjoittaa arvoja seuraavista näyttökuvan.

    ![Lopeta virta-asetukset](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image31.png)

    Olet määrittänyt kaksi syötteen virtaa:

    ![Määritetty syötteen virtaa Azure-portaalissa](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image32.jpg)

    Lisää seuraavaksi viittaus tiedot tarkistavan blob-tiedoston, joka sisältää auton rekisteröintitietojen.

11. Valitse **Lisää SYÖTE**ja valitse sitten **Viitetiedot**.

    ![Viittaus tiedoilla valittuna "Lisää arvon" asetukset](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image33.png)

12. Valitse seuraavalla sivulla tallennustilan tilin, joka alkaa **tolldata**. Säilön nimen tulisi olla **tolldata**ja valitse **POLKU kohdalla** blob-nimen tulisi olla **registration.json**. Tämä tiedostonimi kirjainkoko on merkitsevä ja pitäisi olla kirjaimiksi.

    ![Blogin tallennustilan asetukset](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image34.png)

13. Valitse arvot, kuten seuraavassa näyttökuvassa seuraavalla sivulla ja valitse sitten ohjatun toiminnon lopuksi **OK** .

    ![Valinnasta, JSON "jopa Sarjatoiminto muodossa" ja UTF8 "Koodaus"](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image35.png)

Nyt kaikki syötteiden on määritetty.

![Näyttökuva kolme määritetyn syötteiden](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image36.jpg)

## <a name="define-output"></a>Määritä tulostus

1. **Tulostus** -välilehti ja valitse sitten **Lisää tulos**.

    ![Tulostus-välilehti ja "Lisää tulos-asetus](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image37.jpg)

2. Valitse **SQL-tietokanta**.

3. Valitse palvelimen nimi, jota käytettiin artikkelin "Yhteyden, tietokanta-Visual Studio"-osassa. Tietokannan nimi on **TollDataDB**.

4. Kirjoita **käyttäjänimi** -kenttään **tolladmin** **123toll!** **salasana** -kenttään ja **TollDataRefJoin** **taulukon** kentässä.

    ![SQL-tietokanta-asetukset](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image38.jpg)

## <a name="azure-stream-analytics-query"></a>Azure Stream analytics-kysely

**Kysely** -välilehti sisältää SQL-kysely, joka muuntaa saapuvat tiedot.

![Kysely-välilehti lisätään kyselyyn](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image39.jpg)

Tässä opetusohjelmassa yrittää useita business kysymyksiin, jotka liittyvät tiedot ja rakenteet Stream Analytics-kyselyjä, jotka avulla voidaan Azure Stream Analytics asiaa vastaus maksullinen.

Ennen kuin aloitat ensimmäisen Stream Analytics-työ, tarkastellaan muutamia skenaarioita ja kyselysyntaksia.

## <a name="introduction-to-azure-stream-analytics-query-language"></a>Azure Stream Analytics-kyselykieltä esittely
-----------------------------------------------------

Oletetaan, että sinun on ajoneuvot, kirjoita maksullinen booth määrän laskeminen. Koska kyseessä on tapahtumien jatkuvan muodossa, on määritettävä "tietyn ajan." Muokkaa seuraavaksi kysymyksen on "kuinka monta ajoneuvot Anna maksullinen booth kolmen minuutin välein?". Tätä kutsutaan usein tumbling määrä.

Katsotaan Azure Stream Analytics-kysely, joka kysymykseen vastataan:

    SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*) AS Count
    FROM EntryStream TIMESTAMP BY EntryTime
    GROUP BY TUMBLINGWINDOW(minute, 3), TollId

Kuten näet, Azure Stream Analytics käyttää kyselykieltä, joka on kuin SQL ja Lisää muutama tunnisteet, voit määrittää kyselyn aika liittyviä ominaisuuksia.

Lisätietoja Lue lisätietoja käyttää kyselyä kohteesta MSDN [Ajanhallinta](https://msdn.microsoft.com/library/azure/mt582045.aspx) ja [Windowing](https://msdn.microsoft.com/library/azure/dn835019.aspx) rakenteita.

## <a name="testing-azure-stream-analytics-queries"></a>Azure Stream Analytics kyselyjen testaaminen

Nyt kun olet kirjoittanut ensimmäisen Azure Stream Analytics-kyselyn, se on testausvaiheessa käyttämällä mallidatatiedostot TollApp-kansiossa seuraavassa polussa:

**.. \\TollApp\\TollApp\\tiedot**

Tämä kansio sisältää seuraavat tiedostot:

- Entry.JSON
- Exit.JSON
- Registration.JSON

## <a name="question-1-number-of-vehicles-entering-a-toll-booth"></a>Kysymys 1: Ajoneuvot kirjoittaminen maksullinen booth määrä

1. Avaa Azure-portaali ja siirry luotu Azure Stream Analytics-työ. Valitse **kysely** -välilehti ja liitä kyselyn linkitys edellisestä osasta.

    ![Kysely, joka on liitetty kysely-välilehti](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image40.png)

2. Vahvista tämä kysely mallitiedot napsauttamalla **Testaa** -painiketta. Valitse avautuvassa valintaruudussa Entry.json-tiedosto, joka on mallitietoja **EntryTime** tapahtuma-muodossa.

    ![Näyttökuva Entry.json-tiedostosta](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image41.png)

3. Vahvista, että kyselyn tulos on oikein:

    ![Testin tulokset](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image42.jpg)

## <a name="question-2-report-total-time-for-each-car-to-pass-through-the-toll-booth"></a>Kysymys 2: Raporttiin kokonaisaika maksullinen booth läpi kunkin Auto

Keskimääräinen aika, joka n Auto maksullinen välitystä voit arvioida prosessin ja asiakkaan kokemukset tehokkuutta.

Kokonaisaika etsimällä haluat liittyä EntryTime virta kanssa ExitTime-muodossa. Liityt virtaa TollId ja LicencePlate sarakkeissa. **JOIN** -operaattorin edellyttää, että määrität ajallinen ulottumalle, joka kuvaa liitettyjen tapahtumat hyväksyttävä ajan erotuksen. **DATEDIFF** -funktiota käytetään tapahtumien on 15 minuutin päähän toisistaan. Voit myös koskevat **DATEDIFF** -funktio, Lopeta ja tapahtuma kertaa laskemiseen Auto käyttää maksullinen aseman todellinen aika. Huomautus **DATEDIFF** käyttö erotus, kun sitä käytetään **SELECT** -lauseen **liittyä** ehdon sijaan.

    SELECT EntryStream.TollId, EntryStream.EntryTime, ExitStream.ExitTime, EntryStream.LicensePlate, DATEDIFF (minute , EntryStream.EntryTime, ExitStream.ExitTime) AS DurationInMinutes
    FROM EntryStream TIMESTAMP BY EntryTime
    JOIN ExitStream TIMESTAMP BY ExitTime
    ON (EntryStream.TollId= ExitStream.TollId AND EntryStream.LicensePlate = ExitStream.LicensePlate)
    AND DATEDIFF (minute, EntryStream, ExitStream ) BETWEEN 0 AND 15

1. Voit esikatsella tämä kysely-päivityskysely työtäsi **kysely** -välilehdessä:

    ![Päivitetty kyselyn kysely-välilehti](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image43.jpg)

2. Valitse **Testaa** ja määritä syötteen mallitiedostojen EntryTime ja ExitTime.

    ![Näyttökuva syötteen tiedostot](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image44.png)

3. Valitse Testaa kysely ja Näytä tulos valintaruutu on valittuna:

    ![Testin tulos](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image45.png)

## <a name="question-3-report-all-commercial-vehicles-with-expired-registration"></a>Kysymys 3: Raportin kaikki vanhentuneet rekisteröinnin kaupallisen ajoneuvot

Azure virta-analyysin avulla staattinen tietojen tilannevedosten ajallinen tietojen virtaa osallistuminen. Kuvaavat tätä ominaisuutta, käytä seuraava näyte kysymykseen.

Jos kaupallisen ajoneuvon rekisteröinyt maksullinen yrityksessä, sen kulkevat maksullinen booth ilman pysäytetään nähtäville. Kaupallisen ajoneuvon rekisteröinnin hakutaulukko käytetään tunnistaa kaikki kaupallisen ajoneuvot, jotka ovat vanhentuneet merkintöjä.

    SELECT EntryStream.EntryTime, EntryStream.LicensePlate, EntryStream.TollId, Registration.RegistrationId
    FROM EntryStream TIMESTAMP BY EntryTime
    JOIN Registration
    ON EntryStream.LicensePlate = Registration.LicensePlate
    WHERE Registration.Expired = '1'

Voit testata kyselyn viitetiedot avulla, sinun täytyy määrittää viitetiedot, joita vielä syöttölähde.

1. Voit esikatsella tämä kysely-kyselyn liittäminen **kysely** -välilehti, valitse **Testaa**ja määrittää kahden syötteen lähteet:

    ![Näyttökuva syötteen tiedostot](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image46.png)

2. Näytä kyselyn tulos:

    ![Näyttökuva kyselyn tuloksiin](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image47.png)

## <a name="start-the-stream-analytics-job"></a>Aloita Stream Analytics-työ


Nyt on aika Lopeta määritystä ja aloittaa työn. Tallenna kysely kysymys 3, joka tuottaa tulosteen, joka vastaa **TollDataRefJoin** tulosteen taulukko rakenne.

Siirry projektin **raporttinäkymät-IKKUNAN**ja **Käynnistä-painiketta**.

![Työn Raporttinäkymät-ikkunan aloitus-painike](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image48.jpg)

Muuta avautuvassa valintaruudussa **Käynnistä TULOSTEEN** aika **Mukautettu**ajaksi. Muuta tunnin tuntia ennen nykyisen kellonajan. Tämä muutos varmistaa, että kaikki tapahtumat, tapahtuma-toiminnosta käsitellään luomiseen tapahtumat opetusohjelman alussa aloittamisen jälkeen. Valitse nyt aloittaa työn valintamerkkiä.

![Mukautetun ajan valinta](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image49.png)

Työn käynnistäminen saattaa kestää muutaman minuutin kuluttua. Näet tilan Stream Analytics ylimmän tason sivustossa.

![Näyttökuva projektin tila](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image50.jpg)

## <a name="check-results-in-visual-studio"></a>Tarkista tulokset Visual Studiossa

1. Avaa Visual Studio palvelimen Resurssienhallinta ja **TollDataRefJoin** taulukkoa hiiren kakkospainikkeella.
2. Valitse **Näytä taulukkotiedot** nähdä työtäsi tulosteen.

    !["Näytä taulukkotiedot" palvelimen Explorerissa valinta](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image51.jpg)

## <a name="scale-out-azure-stream-analytics-jobs"></a>Skaalaa ulos Azure Stream Analytics töitä

Azure Stream Analytics on suunniteltu elastically mittakaava niin, että se voidaan käsitellä paljon tietoja. Azure Stream Analytics-kysely, joka tässä vaiheessa Skaalaa, järjestelmä **OSION BY** -lauseen avulla. **PartitionId** on erityisen sarake, järjestelmä lisää vastaa osion Input (tapahtumaa-toiminnossa).

    SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*)AS Count
    FROM EntryStream TIMESTAMP BY EntryTime PARTITION BY PartitionId
    GROUP BY TUMBLINGWINDOW(minute,3), TollId, PartitionId

1. Lopeta nykyinen työ, Päivitä kyselyn **kysely** -välilehti ja Avaa **Mittakaava** -välilehti.

    **STREAMING yksikkö** määrittää Laske power, joka voi vastaanottaa työn määrää.

2. Siirtämällä 6.

    ![Näyttökuva 6 streaming yksiköt valitseminen](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image52.jpg)

3. Avaa **tulostus** -välilehti ja muuta **TollDataTumblingCountPartitioned**SQL-taulukon nimi.

Jos aloitat työn nyt Azure Stream analyysin voi jakaa työmäärän Laske lisämateriaalia ja saavuttamiseksi paremmin siirtonopeuden. Huomaa, että TollApp-sovellus on myös lähettäminen TollId osioinut tapahtumat.

## <a name="monitor"></a>Valvonta

**Valvonta** -välilehdessä on käynnissä olevan projektin tilastotietoja.

![Näyttökuva tilastotietoja töitä](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image53.png)

Voit käyttää **Toimintoa lokit** **KOONTINÄYTTÖ** -välilehti.

!["Toimintojen lokit"-vaihtoehto](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image54.jpg)

![Näyttökuva töiden tilan näet toiminnon lokit](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image55.png)

Saat lisätietoja tietyn tapahtuman, valitse tapahtuma ja valitse sitten **tiedot** -painiketta.

![Näyttökuva valitun tapahtuman tiedot](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image56.png)

## <a name="conclusion"></a>Tekemistä

Tässä opetusohjelmassa käyttöön Azure Stream Analytics-palveluun. Se on osoittanut syötteiden ja tulostaa Stream Analytics työn määrittämisestä. Käytä maksullinen tietojen skenaariota, opetusohjelman selitetään tavallisia ongelmia, jotka syntyy liikerata ja miten niitä voidaan ratkaista tietojen yksinkertainen SQL kaltaisessa kyselyitä Azure Stream Analytics tilaa. Opetusohjelman on kuvattu SQL-tunniste rakenteita ajallinen tietojen käsittelyyn. Se ilmeni liittyminen tietojen virtaa, voit täydentää tietovirta staattinen viitetiedot kanssa ja kyselyn saavuttamiseksi suurempi siirtonopeuden, miten.

Tässä opetusohjelmassa on hyvä johdanto, vaikka se ei ole täydellinen millä tahansa. Löydät Lisää kyselyn kuviot käyttämällä SAQL kielen [kyselyn esimerkkejä yleisten Stream Analytics-käyttötavat](stream-analytics-stream-analytics-query-patterns.md).
Katso lisätietoja Azure Stream Analytics [ohjeessa](https://azure.microsoft.com/documentation/services/stream-analytics/) .

## <a name="clean-up-your-azure-account"></a>Puhdista Azure-tili

1. Lopeta Stream Analytics-työ Azure-portaalissa.

    Setup.ps1 komentosarja luo kaksi tapahtumaa keskittimet ja SQL-tietokantaan. Seuraavien ohjeiden avulla voit Puhdista resurssien opetusohjelman lopussa.

2. Kirjoita PowerShell-ikkunaan **.\\ CleanUp.ps1** komentosarja, joka poistaa opetusohjelman käytettävien resurssien alkavan.

    > [AZURE.NOTE] Resurssien tunnistetaan nimi. Varmista, että voit tarkastella kunkin kohteen huolellisesti ennen poiston vahvistaminen.

    ![Näyttökuva uudelleenjärjestäminen prosessi](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image57.png)
