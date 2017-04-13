<properties
    pageTitle="Power BI-koontinäyttö Stream Analytics | Microsoft Azure"
    description="Reaaliaikainen streaming Power BI-koontinäytön avulla voit kerätä liiketoimintatiedot ja analysoida suuria tietoja Stream Analytics-työstä."
    keywords="analysoinnin koontinäytössä reaaliaikainen Raporttinäkymät-ikkunan"
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

#  <a name="stream-analytics--power-bi-a-real-time-analytics-dashboard-for-streaming-data"></a>Virtauttaa Analytics ja Power BI: reaaliaikainen analytics Raporttinäkymät-ikkunan, streaming tiedot

Azure virta-analyysin avulla voit hyödyntää jokin alussa yritystietotyökalujen, Microsoft Power BI. Opettele Azure Stream analyysin avulla voit analysoida suuria streaming tiedot ja Hae reaaliaikaista Power BI analytics raporttinäkymän tietoja.

[Microsoft Power BI](https://powerbi.com/) avulla voit luoda live Raporttinäkymät-ikkunan nopeasti. [Havainnollistava skenaarion videosta](https://www.youtube.com/watch?v=SGUpT-a99MA).

Tässä artikkelissa kerrotaan kuinka luoda omia mukautettuja yritystietotyökalujen käyttämällä Power BI Azure Stream Analytics-töiden tulos ja hyödyntää reaaliaikaisia Raporttinäkymät-ikkunan.

## <a name="prerequisites"></a>Edellytykset

* Microsoft Azure-tiliin
* Syötteen Stream Analytics työn tarjoaman streaming tiedot. Virta Analytics hyväksyy Azure tapahtuman keskittimet tai Azure Blob storage syötettä.  
* Power BI työpaikan tai oppilaitoksen tiliä

## <a name="create-azure-stream-analytics-job"></a>Luo Azure Stream Analytics-työ

[Perinteinen Azure-portaaliin](https://manage.windowsazure.com)ja valitse **Uusi, tietopalvelujen, Stream Analytics, nopea luominen**.

Määritä seuraavat arvot, valitse sitten **Luo Stream Analytics työ**:

* **Projektin nimi** - projektin nimi. Esimerkiksi **DeviceTemperatures**.
* **Alue** - Valitse sijaitsevat projektin haluamaasi aluetta. Harkitse siten työ- ja tapahtumaa-toiminnossa saman alueen parhaan mahdollisen suorituskyvyn varmistamiseksi ja välttää sille tietojen siirto kustannukset alueiden välillä.
* **Tallennustilan tilin** – Valitse tallennustilan tili, jota haluat tallentaa kaikki tämän alueen töitä Stream Analytics seurantatiedot. Voit halutessasi valita käytössä olevan tallennustilan-tilin tai luoda uuden.

Valitse vasemmanpuoleisessa ruudussa luettelon Stream Analytics työt **Stream Analytics** .

![graphic1][graphic1]

> [AZURE.TIP] Uusi projekti näkyy luettelossa tilassa **Ei ole aloitettu**. Huomaa, että sivun alareunaan **Käynnistä** -painiketta ei ole käytössä. Tämä on määritettävä työn syöte, tulostus, kyselyn, ja niin edelleen ennen kuin aloitat työn toiminta.

## <a name="specify-job-input"></a>Työn syöte

Tässä opetusohjelmassa, emme ovat olettaen käytät tapahtumaa-toiminnossa syötteeksi JSON Sarjatoiminto ja UTF-8-koodausta.

* Napsauta projektin nimeä.
* Valitse sivun yläreunasta **syötteiden** ja valitse sitten **Lisää syöte**. Valintaikkuna, joka avautuu, käy läpi toimenpiteellä syötettäsi määrä.
*   Valitse **tietovirta**, ja valitse sitten oikealle-painike.
*   Valitse **Tapahtuma-toiminnosta**ja valitse sitten oikealle-painike.
*   Kirjoita tai valitse kolmannella sivulla seuraavat arvot:
  * **Syötteen Alias** - syöte työn helpossa muodossa nimi. Huomaa, että käytät tätä nimeä kyselyn myöhemmin.
  * **Tapahtuma-toiminnossa** – Jos olet luonut tapahtumaa-toiminnossa on Stream Analytics-työ, valitse saman tilauksen nimitilan, joka on tapahtumaa-toiminnossa.
*   Jos tapahtuma-toiminnossa on eri tilauksen, valitse **Käytä tapahtumaa-toiminnossa toiseen tilaukseen** ja kirjoita manuaalisesti tiedot **Palvelun Bus Namespace**, **Tapahtuman keskittimeen nimi**, **Tapahtuman keskittimeen käytännön nimi**, **Tapahtuman keskittimeen käytännön avain**ja **Tapahtumien keskittimeen osion määrä**.

> [AZURE.NOTE]  Tässä esimerkissä käytetään oletusmäärän osioita, joka on 16.

* **Tapahtuman keskittimeen nimi** – Valitse Azure tapahtumaa-toiminnossa on nimi.
* **Tapahtuman keskittimeen käytännön nimi** - tapahtuma-keskittimeen, valitse tapahtumaa-toiminnossa käytännön. Varmista, että käytäntö on käyttöoikeuksien hallinta.
*   **Tapahtuman keskittimeen kuluttaja ryhmän** – voit jättää tyhjäksi tai määrittää kuluttaja ryhmän sinulla tapahtumaa-toiminnossa. Huomaa, että tapahtumaa-toiminnossa kuluttaja kunkin ryhmän voi olla vain 5 lukijat kerrallaan. Päätä, työtäsi oikean kuluttaja-ryhmän vastaavasti. Jos jätät kentän tyhjäksi, se käyttää kuluttaja oletusryhmän.

*   Napsauta hiiren kakkospainikkeella.
*   Määritä seuraavat arvot:
  * **Tapahtuman Sarjatoiminto muoto** - JSON
  * **Koodaus** - UTF8
*   Valitse Tarkista-painiketta, voit lisätä tämän lähteen ja tarkista, että Stream Analytics voit muodostaa tapahtumaa-toiminnossa.

## <a name="add-power-bi-output"></a>Lisää Power BI-tulostus

1.  Valitse **tulostus** sivun yläreunasta ja valitse sitten **Lisää tulos**. Power BI peruutuksesta tulostus-vaihtoehto tulee näkyviin.

    ![graphic2][graphic2]  

2.  Valitse **Power BI** ja valitse sitten oikealle-painike.
3.  Näyttöön tulee näyttöön, kuten jompikumpi seuraavista:

    ![graphic3][graphic3]  

4.  Tässä vaiheessa Lisää työpaikan tai oppilaitoksen tiliä Stream Analytics työn tulostukseen. Jos sinulla on jo Power BI-tili, valitse **Määritä nyt**. Jos et, valitse **Rekisteröidy nyt**. [Tässä on hyvä blogia – Power BI Rekisteröidy tietoja ominaisuussäilöjen](http://blogs.technet.com/b/powerbisupport/archive/2015/02/06/power-bi-sign-up-walkthrough.aspx).

    ![graphic11][graphic11]  

5.  Seuraavaksi tulee näkyviin näytön, kuten jompikumpi seuraavista:

    ![graphic4][graphic4]  

Anna arvot alla seuraavasti:

* **Tulosteen Alias** – voit laittaa kaikki tunnus, joka on helppo viitata. Tämä tunnus on erityisen hyödyllisiä, jos haluat tulostaa useita työtä varten. Tässä tapauksessa sinun on tässä kyselyssä tulosteen viitata. Esimerkiksi käytetään tuloksena saadun arvon alias = "OutPbi".
* **Tietojoukon nimi** - tietojoukon nimi, jonka haluat siirtää on Power BI. Käyttää esimerkiksi "pbidemo".
*   **Taulukkonimi** - kohdassa Power BI-tulostus tietojoukko taulukon nimi. Oletetaan, että kutsu sitä "pbidemo". Tällä hetkellä Stream Analytics työt Power BI-tulosteen olla vain yksi taulukko DataSet.
*   **Työtilan** – Valitse kohdassa dataset luodaan Power BI vuokraajan työtilan.

>   [AZURE.NOTE] Älä erikseen Luo tämän tietojoukko ja taulukon Power BI-tilillesi. Ne luodaan automaattisesti kun käynnistät Stream Analytics työtehtävien ja projekti alkaa pumppaus tulosteen Power BI. Jos projektin kyselyn ei tuota tuloksia, tietojoukko ja taulukon ei luoda.

*   Valitse **OK**, **Testiyhteys** , ja kun siirrät configuraiton on valmis.

>   [AZURE.WARNING] Huomaa myös, jos Power BI oli jo tietojoukko ja taulukko, jossa on sama nimi kuin antamasi Stream Analytics työn, olemassa olevia tietoja korvataan.


## <a name="write-query"></a>Kirjoita kyselyn

Siirry projektin **kysely** -välilehti. Kirjoita kyselyn, jossa tulosteen, jonka haluat Power BI. Esimerkiksi se voi olla esimerkiksi seuraava SQL-kysely:

    SELECT
        MAX(hmdt) AS hmdt,
        MAX(temp) AS temp,
        System.TimeStamp AS time,
        dspl
    INTO
        OutPBI
    FROM
        Input TIMESTAMP BY time
    GROUP BY
        TUMBLINGWINDOW(ss,1),
        dspl



Aloita työtäsi. Vahvista, että tapahtumaa-toiminnossa lähetetään tapahtumista ja kyselyn Luo odotetut tulokset. Jos kyselyn tulostaa 0 riviä, Power BI-tietojoukko ja taulukot ei luodaan automaattisesti.

## <a name="create-the-dashboard-in-power-bi"></a>Power BI raporttinäkymän luominen

Siirry [Powerbi.com](https://powerbi.com) ja kirjaudu sisään käyttämällä työpaikan tai oppilaitoksen tiliä. Jos muodossa Analytics työn kyselyn tulostaa tulokset, näet, että tietojoukko on jo luotu:

![graphic5][graphic5]

Luomisen raporttinäkymät Siirry raporttinäkymät-vaihtoehto ja luo uusi raporttinäkymät-ikkuna.

![graphic6][graphic6]

Tässä esimerkissä on tulee vaakaviiva se "Esittely Raporttinäkymät-ikkunan".

Valitse nyt tietojoukko luoma Stream Analytics työtäsi (pbidemo nykyisen esimerkissä). Siirryt sivulle voit luoda kaavion tämä tietojoukko päälle. Seuraavassa on esimerkki siitä, voit luoda raportteja mutta:

Valitse Σ temp ja aikaa kentät. Ne automaattisesti Siirry arvo ja akseli kaavion:

![graphic7][graphic7]

Tällä saavat automaattisesti kaavioon kuin alla:

![graphic8][graphic8]

Arvo-osassa napsauttamalla avattavassa varten temp alaspäin ja valitse **keskiarvo** lämpötila ja kaavion, valitse **visualisointi** ja valitsemalla **viivakaavio**:

![graphic9][graphic9]

Nyt näet viivakaaviossa keskiarvon ajan kuluessa.  Käytä alla pin-asetuksen, voit kiinnittää tämä koontinäyttöön aiemmin luomasi:

![graphic10][graphic10]

Nyt kun tarkastelet raporttinäkymä, jonka kiinnitetty raportti, näet raportti päivitetään reaaliajassa. Kokeile tapahtumien – piikin temp tai jotain tietojen muuttaminen kuin ja lukee reaaliaikainen vaikutus, jotka näkyvät myös raporttinäkymän.

Huomaa, että tässä opetusohjelmassa osoittaa luomisesta, mutta yksi tietojoukko kaavion tyyppi. Power BI avulla voit luoda muiden asiakkaan yritystietotyökalujen organisaatiollesi. Toinen esimerkki Power BI-raporttinäkymästä videossa [Power BI käytön aloittaminen](https://youtu.be/L-Z_6P56aas?t=1m58s) .

Lisätietoja on artikkelissa Power BI-tulostus määrittämisestä ja hyödyntää Power BI-ryhmiä Lue [tietoja Stream Analytics tulostaa](stream-analytics-define-outputs.md "tietoja Stream Analytics tulostaa") [Power BI-osassa](stream-analytics-define-outputs.md#power-bi) . Lisätietoja raporttinäkymien luominen Power BI toiseen hyödyllisiä resurssi on [Power BI-koontinäytöt](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).

## <a name="limitations-and-best-practices"></a>Rajoitukset ja parhaat käytännöt

Power BI suojattu samanaikainen ja siirtonopeuden seuraavassa kuvatulla tavalla: [https://powerbi.microsoft.com/pricing](https://powerbi.microsoft.com/pricing "Power BI hinnat")

Koska ne Power BI viedään maihin itse eniten luonnollisesti tapaukset, joissa Azure Stream Analytics onko merkittäviä tietojen lataaminen vähennys.
On suositeltavaa, varmista, että tietojen push on enintään 1 push/toisen ja että kyselyn viedään maihin siirtonopeuden vaatimusten puitteissa – voit käyttää seuraavaa kaavaa laskemiseen ja anna ikkunan sekunteina arvo TumblingWindow tai HoppingWindow avulla:

![equation1](./media/stream-analytics-power-bi-dashboard/equation1.png)  

Esimerkkinä – Jos sinulla on 1 000 laitteiden tietojen lähettäminen sekunnin välein, käytössä on Power BI Pro kieli, joka tukee 1,000,000 rivien/tunti ja haluat laite keskimääräinen tietojen noutaminen Power BI Tee enintään push sekunnin välein 4 laite (Katso kuvaa alla):  

![equation2](./media/stream-analytics-power-bi-dashboard/equation2.png)  

Mikä tarkoittaa alkuperäinen kysely muutamme:

    SELECT
        MAX(hmdt) AS hmdt,
        MAX(temp) AS temp,
        System.TimeStamp AS time,
        dspl
    INTO
        OutPBI
    FROM
        Input TIMESTAMP BY time
    GROUP BY
        TUMBLINGWINDOW(ss,4),
        dspl

### <a name="powerbi-view-refresh"></a>PowerBI View'n päivittäminen

Yleisiä kysymyksiä on "Miksi ei koontinäyttö automaattinen päivitys-PowerBI?".

Tätä käyttämiseen PowerBI Q & A ja esimerkiksi "Suurimman arvon temp, jossa aikaleima on tänään" kysymys ja kiinnitä ruudun koontinäyttö.

### <a name="renew-authorization"></a>Käyttöoikeuksien uusiminen

Tarvitset tarkistamiseen Power BI-tilin uudelleen, jos salasana on muuttunut, vaikka työtäsi on luotu tai viimeksi todennettu. Jos multi-factor Authentication (MFA) on määritetty Azure Active Directory (AAD)-vuokraajan sinun on myös uusi Power BI-todennus kahden viikon välein. Tämä ongelma päätyy yleensä on työn tuloksia ja "Tarkista käyttäjän virhe"-toimintoa lokit:

![graphic12][graphic12]

Vastaavasti jos työn yrittää käynnistää, kun tunnus on vanhentunut, ilmenee virhe ja työn aloitusaika epäonnistuu. Virhe näyttää suurin piirtein alla, kuten:

![PowerBI kelpoisuustarkistusvirhe](./media/stream-analytics-power-bi-dashboard/stream-analytics-power-bi-dashboard-token-expire.png) 
 

Voit ratkaista ongelman pysäyttää käynnissä olevan projektin ja siirry Power BI-tuloste.  "Uusi luvan"-linkki ja Käynnistä työtäsi edellisen pysäytetty kerran tietojen menetyksen välttämiseksi.

![PowerBI vahvistus uusiminen](./media/stream-analytics-power-bi-dashboard/stream-analytics-power-bi-dashboard-token-renew.png) 

Kun luvan päivitetään Power BI luvan alueella on vihreä-ilmoitus tulee näkyviin:

![PowerBI vahvistus uusiminen](./media/stream-analytics-power-bi-dashboard/stream-analytics-power-bi-dashboard-token-renewed.png) 

## <a name="get-help"></a>Ohjeiden saaminen
Saat lisäohjeita yritä Microsoftin [Azure Stream Analytics-keskustelupalsta](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Seuraavat vaiheet

- [Azure Stream Analytics esittely](stream-analytics-introduction.md)
- [Azure Stream Analytics käytön aloittaminen](stream-analytics-get-started.md)
- [Skaalaa Azure Stream Analytics töitä](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics kyselyn kieliohje](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics hallinta REST API-viittaus](https://msdn.microsoft.com/library/azure/dn835031.aspx)


[graphic1]: ./media/stream-analytics-power-bi-dashboard/1-stream-analytics-power-bi-dashboard.png
[graphic2]: ./media/stream-analytics-power-bi-dashboard/2-stream-analytics-power-bi-dashboard.png
[graphic3]: ./media/stream-analytics-power-bi-dashboard/3-stream-analytics-power-bi-dashboard.png
[graphic4]: ./media/stream-analytics-power-bi-dashboard/4-stream-analytics-power-bi-dashboard.png
[graphic5]: ./media/stream-analytics-power-bi-dashboard/5-stream-analytics-power-bi-dashboard.png
[graphic6]: ./media/stream-analytics-power-bi-dashboard/6-stream-analytics-power-bi-dashboard.png
[graphic7]: ./media/stream-analytics-power-bi-dashboard/7-stream-analytics-power-bi-dashboard.png
[graphic8]: ./media/stream-analytics-power-bi-dashboard/8-stream-analytics-power-bi-dashboard.png
[graphic9]: ./media/stream-analytics-power-bi-dashboard/9-stream-analytics-power-bi-dashboard.png
[graphic10]: ./media/stream-analytics-power-bi-dashboard/10-stream-analytics-power-bi-dashboard.png
[graphic11]: ./media/stream-analytics-power-bi-dashboard/11-stream-analytics-power-bi-dashboard.png
[graphic12]: ./media/stream-analytics-power-bi-dashboard/12-stream-analytics-power-bi-dashboard.png
[graphic13]: ./media/stream-analytics-power-bi-dashboard/13-stream-analytics-power-bi-dashboard.png
