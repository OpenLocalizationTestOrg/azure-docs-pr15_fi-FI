<properties
    pageTitle="Aloittaminen Azure Stream Analytics prosessin tietoihin IoT laitteilla. | Microsoft Azure"
    description="IoT tunnistimen tunnisteet ja tiedot virtaa stream analytics ja reaaliaikainen tietojen käsittely"
    keywords="ratkaisu IOT iot käytön aloittaminen"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="10/19/2016"
    ms.author="jeffstok"
/>

# <a name="get-started-with-azure-stream-analytics-to-process-data-from-iot-devices"></a>Aloita Azure Stream Analytics prosessin tietoihin IoT laitteilla

Tässä opetusohjelmassa kerrotaan luomisesta stream käsittely logiikan tietojen keräämistä asioita Internet (IoT) laitteet. Käytämme tosielämän, asiat Internet (IoT) Käyttötapaus näytetään, miten voit luoda nopeasti ja taloudellisesti ratkaisu.

## <a name="prerequisites"></a>Edellytykset

-   [Azure-tilaus](https://azure.microsoft.com/pricing/free-trial/)
-   Esimerkki kyselyn ja datatiedostot ladattavan [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot) kohteesta

## <a name="scenario"></a>Skenaario

Contoso, joka on yrityksen teollisuuden automaatio-tilaan, on täysin automaattisesti sen tuotantoprosessia. Tämä tehdas niin koneen on anturit, joita voi lähettää tietoja reaaliajassa virtaa. Tässä skenaariossa tuotannon kerroksen esimies haluaa on reaaliaikaiset tiedot kuviot etsiä ja käsitellä niitä tunnistimen tiedoista. Käytämme Stream Analytics Query Language (SAQL) tunnistimen tietojen kiinnostavat kuviot-saapuvat virta tietojen etsimiseen.

Tähän tiedot luodaan Texas säädösten tunnistimen tunnisteen laitteesta.

![Texas säädösten tunnistimen tunniste](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-01.jpg)

Tietoja sisällön on JSON-muodossa ja näyttää seuraavalta:


    {
        "time": "2016-01-26T20:47:53.0000000",  
        "dspl": "sensorE",  
        "temp": 123,  
        "hmdt": 34  
    }  

Tosielämän tilanteessa voi olla satoja nämä anturit luodaan tapahtumien stream nimellä. Ihannetapauksessa yhdyskäytävälaitteen suorittaa koodin ja push näitä tapahtumia [Azure tapahtuman keskittimet](https://azure.microsoft.com/services/event-hubs/) tai [Azure IoT keskittimet](https://azure.microsoft.com/services/iot-hub/). Stream Analytics-työ tapauksessa ingest tapahtuman keskittimet näitä tapahtumia ja suorittaa virtaa reaaliaikainen analytics kyselyjä. Voit sitten lähettää tulokset yhteen [tuettu tulostaa](stream-analytics-define-outputs.md).

Käytön helppous hakeminen aloittaminen oppaan tarjoaa mallitiedosto, jonka reaali tunnistimen tunnisteen laitteilla sieppaaminen. Voit suorittaa kyselyjä esimerkkitiedot sekä tarkastella tuloksia. Seuraavien Opetusohjelmissa sinun opettele muodostamaan työtäsi syötteiden ja tulostaa ja ota käyttöön Azure-palveluun.

## <a name="create-a-stream-analytics-job"></a>Luo Stream Analytics-työ

1. [Azure portal](http://portal.azure.com)napsauttamalla plusmerkkiä ja kirjoita teksti-ikkunan oikeassa **Muodossa ANALYTICS** . Valitse tulosluettelon **Stream Analytics työn** .

    ![Luo uusi Stream Analytics työ](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-02.png)

2. Työn yksilöivä nimi, ja tarkista tilaus on oikea työtä varten. Valitse Luo uusi resurssiryhmä tai valitse aiemmin luotu tilauksen.

3. Valitse työtäsi sijainti. Suositellaan käsittely nopeuden ja vähentäminen kustannukset tietojen valitseminen samassa sijainnissa kuin resurssiryhmä ja tarkoitettu tallennustilan tilin siirto.

    ![Luo uusi Stream Analytics työ tiedot](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03.png)

    > [AZURE.NOTE] Sinun on luotava tämä tallennustilan tilin vain kerran alueittain. Tämä jakaa kaikki Stream Analytics-työt, jotka on luotu alueen.

4. Jos haluat sijoittaa työtäsi koontinäyttöön ja valitse sitten **Luo**valintaruutu.

    ![työpaikkojen käynnissä](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03a.png)

5. Näkyviin tulee "Käytön aloittaminen..." näkyvän oikeasta selainikkunan. Pian se muuttuu Valmis ikkunan alla kuvatulla tavalla.

    ![työpaikkojen käynnissä](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03b.png)

### <a name="create-an-azure-stream-analytics-query"></a>Azure Stream Analytics kyselyn luominen

Kun työ on luotu on aikaa kyselyn luominen ja avaa se. Voit käyttää työtäsi helposti napsauttamalla sen olevaa ruutua.

![Työ-ruutu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-04.png)

Valitse **Projektin topologian** -ruudun **Siirry Kyselyeditoriin kyselyruutu** . **Kyselyeditorin** avulla voit kirjoittaa T-SQL-kysely, joka suorittaa muunnoksen saapuvan tapahtuman tietojen päälle.

![Kyselyruutu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-05.png)

### <a name="query-archive-your-raw-data"></a>Kyselyn: Arkistoida raaka tietoja

Kyselyn helpoin muoto on läpivientikysely, Arkistoi kaikki nimetyn tulos syötteen tietoja. Lataa mallitiedosto [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot) tietokoneen sijaintiin. 

1. Liitä kyselyn PassThrough.txt-tiedostosta. 

    ![Testaa syötteen muodossa](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06.png)

2. Napsauta syötettäsi vieressä kolmea pistettä ja valitse **Lataa mallitiedot tiedostosta** -ruutu.

    ![Testaa syötteen muodossa](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06a.png)

3. Ruutu avautuu oikealle seurauksena, se HelloWorldASA InputStream.json datatiedoston valitseminen ladatut sijainti ja valitse ruudun alaosassa **OK** .

    ![Testaa syötteen muodossa](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06b.png)

4. Napsauta **Testaa** vaihde vasemmalle alueen ikkunan yläosassa ja käsitellä testi-kysely otoksen tietojoukko. Tulokset-ikkuna tulee näkyviin kyselyn alla, kuin käsittely on valmis.

    ![Testitulokset](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-07.png)

### <a name="query-filter-the-data-based-on-a-condition"></a>Kyselyn: Ehdon perusteella tietojen suodattaminen

Yritetään ehdon perusteella tulosten suodattamiseen. Haluamme näet tulokset ainoastaan tapahtumat, jotka ovat peräisin "sensorA." Kysely on Filtering.txt-tiedostossa.

![Suodatus tietovirta](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-08.png)

Huomaa, että kirjainkoko on merkitsevä kyselyn vertaa merkkijonoarvo. Valitse uudelleen, jos haluat suorittaa kyselyä **testi** -hammaspyöräkuvaketta. Kyselyn tuloksessa 389 rivien ulos 1860 tapahtumat.

![Toisen kyselyn testin tulos tulokset](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-09.png)

### <a name="query-alert-to-trigger-a-business-workflow"></a>Kyselyn: Ilmoitusta business työnkulun käynnistäminen

Oletetaan, että Varmista yksityiskohtaisempia Microsoftin kysely. Jokaiselle anturin haluamme keskilämpötila 30 sekunnin ikkunan kohden ja tulosten esittämistä varten vain, jos keskilämpötila on yli 100 astetta. Kirjoita seuraava kysely ja valitse sitten **testi** , näet tulokset. Kysely on ThresholdAlerting.txt-tiedostossa.

![30 sekunnin suodatin-kysely](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-10.png)

Pitäisi tulla näkyviin tuloksia, jotka sisältävät vain 245 rivien ja nimet anturit, joiden Lauhkean keskiarvo on suurempi kuin 100. Tämä kysely ryhmittelee tapahtumien stream **dspl**, joka on 30 sekuntia **Tumbling ikkunan** päälle tunnistimen, nimi. Ajallinen kyselyt on ilmoitettava miten haluamme edistymiseen aika. Käyttämällä **AIKALEIMA BY** -lause on määrittänyt kertaa liitettävä kaikki ajallinen laskutoimituksia **OUTPUTTIME** -sarake. Lisätietoja on MSDN artikkeleita [Ajanhallinta](https://msdn.microsoft.com/library/azure/mt582045.aspx) ja [Windowing funktioiden](https://msdn.microsoft.com/library/azure/dn835019.aspx)lukeminen

### <a name="query-detect-absence-of-events"></a>Kyselyn: Tunnistaa puuttuminen tapahtumat

Miten Microsoft kirjoittaa kyselyyn, kun haluat puuttui syötteen tapahtumat? Etsi oletetaan, että viimeksi tunnistimen lähetetyt tiedot ja sitten ei lähettänyt tapahtumien seuraavan minuutin ajan. Kysely on AbsenseOfEvent.txt-tiedostossa.

![Tunnista puuttuminen tapahtumat](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-11.png)

Seuraavassa **Vasen** ulkoliitos avulla saman tietovirta (itse liittyä). **Sisäliitoksen** ja tuloksena palautetaan vain, jos vastine löytyy.  **Vasen** ulkoliitos, jos tapahtumaa liitoksen vasemmalla puolella on vastaamattomia, rivin, jossa on NULL kaikkien sarakkeiden oikealla puolella olevan palautetaan. Tällä menetelmällä on erittäin hyödyllinen etsiminen poissaolon tapahtumat. Jos haluat lisätietoja [liittyä](https://msdn.microsoft.com/library/azure/dn835026.aspx)Microsoftin MSDN ohjeissa.

## <a name="conclusion"></a>Tekemistä

Tässä opetusohjelmassa tarkoituksena on miten kirjoittaa eri Stream Analytics-kyselykieltä kyselyissä ja hakutuloksia selaimessa. Kuitenkin tämä on vasta. Tee Stream Analytics ja paljon muuta. Virta analyysin tukee monenlaisia syötteiden ja tulostaa ja voit myös funktioita käytetään Azure koneen Learning, jotta se tehokkaat työkalun virtaa tietojen analysointiin. Voit aloittaa kannattaa tutustua enemmän tietoja Stream Analytics Microsoftin [koulujen kartan](https://azure.microsoft.com/documentation/learning-paths/stream-analytics/)avulla. Lisätietoja viestin kirjoittaminen kyselyjä, lue artikkeli tietoja [Yleiset kyselyn mallit](./stream-analytics-stream-analytics-query-patterns.md).
