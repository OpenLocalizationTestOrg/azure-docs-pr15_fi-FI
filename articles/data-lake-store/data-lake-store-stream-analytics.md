<properties
   pageTitle="Tietoja Stream Analytics virtauttaa järvi säilöön | Azure"
   description="Azure Stream Analytics avulla stream tietojen Azure järvi tietosäilö"
   services="data-lake-store,stream-analytics" 
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="07/07/2016"
   ms.author="nitinme"/>

# <a name="stream-data-from-azure-storage-blob-into-data-lake-store-using-azure-stream-analytics"></a>Azure tallennustilan Blob järvi säilöön käyttämällä Azure Stream Analytics Stream tiedot

Tässä artikkelissa kerrotaan käyttämisestä Azure järvi tietovaraston kuin tulos Azure Stream Analytics-työn. Tässä artikkelissa esitellään yksinkertainen tilanne, jossa lukee tiedot tallennustilan Azure-blob (IME) ja kirjoittaa tiedot järvi tietovarasto (tulos).

>[AZURE.NOTE] Tällä hetkellä luominen ja määrittäminen järvi tietovaraston tulostaa Stream analysoinnissa tuetaan vain [Azure perinteinen portaaliin](https://manage.windowsazure.com). Näin ollen Tässä opetusohjelmassa joitakin osia käyttämällä perinteinen Azure-portaalissa.

## <a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat Tässä opetusohjelmassa, sinun on oltava seuraavasti:

- **Azure-tilaus**. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/).

- Tietojen järvi kaupan julkisen esikatselu **käyttöön Azure tilauksen** . Katso [ohjeet](data-lake-store-get-started-portal.md#signup).

- **Azure-tallennustilan tilin**. Blob-säilö tältä tililtä käytetään syötetietoja Stream Analytics projektille. Tässä opetusohjelmassa oletetaan **datalakestoreasa** ja kutsua **datalakestoreasacontainer**tilin säilön tallennustilan tilin luominen. Kun olet luonut säilö, lataa se mallitietoja. Saat mallitietoja [Azure tietojen Lake Git säilöön](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt). Voit käyttää eri asiakkaiden, kuten [Azure-tallennustilan Explorer](http://storageexplorer.com/)tietojen lataaminen blob-säilö.

    >[AZURE.NOTE] Jos olet luonut tilin Azure-portaalista, varmista, että luot **perinteinen** käyttöönotto-malli. Näin Azure perinteinen-portaalista voi käyttää tallennustilan tilin, koska se on Käytämme Stream Analytics työn luominen. Lisätietoja tallennustilan tilin luominen käyttämällä Classic-käyttöönoton Azure-portaalista on artikkelissa [Azure-tallennustilan tilin luominen](../storage/storage-create-storage-account/#create-a-storage-account).
    >
    > Tai voit luoda tallennustilan tilin perinteinen Azure-portaalista.

- **Azure järvi tietovaraston tili**. Ohjeiden etsiminen [Azure Lake Tietosäilölle Azure-portaalissa käytön aloittaminen](data-lake-store-get-started-portal.md).  


## <a name="create-a-stream-analytics-job"></a>Luo Stream Analytics-työ

Aloita luomalla Stream Analytics-työ, joka sisältää syötteen lähde- ja tuloste. Tässä opetusohjelmassa lähde on Azure-blob-säilö ja kohde on järvi tietosäilö.

1. Kirjaudu [Azure perinteinen Portal](https://manage.windowsazure.com).

2. Valitse näytön alareunaan-left, **Uusi**, **Tietopalvelujen**, **Stream Analytics**, **Nopea luominen**. Sisältävät arvot alla kuvatulla tavalla ja valitse sitten **Luo Stream Analytics työn**.

    ![Luo Stream Analytics-työ] (./media/data-lake-store-stream-analytics/create.job.png "Luo Stream Analytics-työ")

## <a name="create-a-blob-input-for-the-job"></a>Luo Blob-syöte resurssit

1. Avaa sivu Stream Analytics-työ, **syötteiden** -välilehti ja valitse sitten **Lisää syöte** käynnistää ohjatun toiminnon.

2. **Lisää työ syöte** -sivulla Valitse **tietovirta**ja valitse sitten Seuraava-nuolta.

    ![Lisää työ syöte] (./media/data-lake-store-stream-analytics/create.input.1.png "Lisää työ syöte")

3. **Lisää tietovirta työsi** -sivulla Valitse **Blob-objektien tallennustilaan**ja valitse sitten Seuraava-nuolta.

    ![Lisää tietovirta projektille] (./media/data-lake-store-stream-analytics/create.input.2.png "Lisää tietovirta projektille")

4. Valitse **Blob storage asetukset** -sivun, jota käytät syötteen tietolähteenä Blob-objektien tallennustilaan lisätiedot.

    ![Anna blob storage asetukset] (./media/data-lake-store-stream-analytics/create.input.3.png "Anna blob storage asetukset")

    * **Enter syötteen alias**. Tämä on antaisit työn, Kirjoita yksilöllinen nimi.
    * **Valitse tallennustilan tili**. Varmista, että tallennustilan-tili on sama kuin Stream Analytics-työ alue tai maksamaan lisäkustannuksia, tietojen siirtämistä alueiden välillä.
    * **Anna tallennustilan säilö**. Voit luoda uuden säilön tai valitse aiemmin luotu säilö.

    Välitä nuolta.

5. Valitse **Sarjatoiminto asetukset** -sivun Sarjatoiminto muotoiluksi **CSV**-erotin- **välilehti**, kuin **UTF8**koodaus nimellä ja valitse sitten valintamerkki.

    ![Sarjatoiminto asetukset] (./media/data-lake-store-stream-analytics/create.input.4.png "Sarjatoiminto asetukset")

6. Kun olet valmis ohjatulla-blob-syöte lisätään **syötteiden** -välilehdessä ja **vianmäärityksen** sarakkeen pitäisi näkyä **OK**. Voit testata syötteen yhteys myös erikseen alareunassa **Testaa yhteys** -painikkeella.

## <a name="create-a-data-lake-store-output-for-the-job"></a>Tietosäilö järvi tulosteen, työn luominen

1. Avaa sivu Stream Analytics-työ, **tulostus** -välilehti ja valitse sitten **Lisää tulos** käynnistää ohjatun toiminnon.

2. **Lisää työsi tulos** -sivulla Valitse **Järvi tietovaraston**ja valitse sitten oikeaa nuolta.

    ![Lisää työsi tulos] (./media/data-lake-store-stream-analytics/create.output.1.png "Lisää työsi tulos")

3. **Hyväksy yhteyden** -sivulla, jos olet jo luonut järvi tietosäilö-tilin, valitse **Määritä nyt**. Muussa tapauksessa valitse **Rekisteröidy nyt** , jos haluat luoda uuden tilin. Tässä opetusohjelmassa uutiskirjeistä oletetaan, että sinulla on jo järvi tietosäilö-tili, luotu (kuten mainitaan täytä edellytyksiä). Sinun automaattisesti lupa, johon olet kirjautunut sisään Azure perinteinen Portal-tunnistetiedoilla.

    ![Määritä järvi tietosäilö] (./media/data-lake-store-stream-analytics/create.output.2.png "Määritä järvi tietosäilö")

4. Anna **Tiedot järvi kaupan asetukset** -sivun tiedot alla näyttökuvan esitetyllä tavalla.

    ![Määritä tiedot järvi kaupan asetukset] (./media/data-lake-store-stream-analytics/create.output.3.png "Määritä tiedot järvi kaupan asetukset")

    * **Enter tulostus-alias**. Tämä on yksilöllinen nimi antaisit työn tulostukseen.
    * **Määritä järvi tietosäilö-tili**. Olisi olet jo luonut, kuten edellä täytä edellytyksiä.
    * **Määritä polku etuliite-malli**. Tämä edellyttää tulosteen tiedostot, jotka on kirjoitettu järvi tietovaraston Stream Analytics projektin tunnistaa. Koska tulostaa työn kirjoittama otsikot ovat GUID-muoto, mukaan lukien etuliite auttaa tunnistaa kirjoitettujen tulos. Jos haluat jättää päivämäärän ja aikaleiman etuliite Varmista, että voit lisätä `{date}/{time}` etuliite rakenteessa. Jos lisäät, **Päivämäärämuoto **- ja **Aikamuoto** -kentät ovat käytössä, ja voit valita muodon valinta.

    Välitä nuolta.

5. Valitse **Sarjatoiminto asetukset** -sivun Sarjatoiminto muotoiluksi **CSV**-erotin- **välilehti**, kuin **UTF8**koodaus nimellä ja valitse sitten valintamerkkiä.

    ![Määritä muodossa] (./media/data-lake-store-stream-analytics/create.output.4.png "Määritä muodossa")

6. Kun olet saanut ohjatulla, järvi tietovaraston tulosteen lisätään **tulostus** -välilehdessä ja **vianmäärityksen** sarakkeen pitäisi näkyä **OK**. Voit myös erikseen Testaa yhteys tulostukseen alareunassa **Testaa yhteys** -painikkeella.

## <a name="run-the-stream-analytics-job"></a>Suorita Stream Analytics-työ

Virta Analytics työn suorittamista varten sinun on suoritettava kyselyn Query-välilehdessä. Tässä opetusohjelmassa, voit suorittaa otoksen kyselyn korvaamalla syötteen työn paikkamerkit ja tulosteen aliakset, alla näyttökuvan esitetyllä tavalla.

![Suorita kysely] (./media/data-lake-store-stream-analytics/run.query.png "Suorita kysely")

Valitse näytön alalaidasta **Tallenna** ja valitse sitten **Käynnistä**. Valitse **Mukautettu-aika**-valintaikkunassa ja valitse päivämäärän menneisyydessä, kuten **1/1/2016**. Napsauta aloittaa työn valintamerkkiä. Voi kestää muutaman minuutin aloittaa työn.

![Määritä työaika] (./media/data-lake-store-stream-analytics/run.query.2.png "Määritä työaika")

Kun projekti alkaa, valitse **Näyttö** -välilehdessä voit tarkastella tietoja käsittelyn.

![Näytön työ] (./media/data-lake-store-stream-analytics/run.query.3.png "Näytön työ")

Lopuksi voit avata järvi tietosäilö-tilisi ja tarkista, onko tiedot on kirjoitettu onnistuneesti tilin [Azure-portaalissa](https://portal.azure.com) .

![Tarkista tulos] (./media/data-lake-store-stream-analytics/run.query.4.png "Tarkista tulos")

Huomaa, että tulos on kirjoitettu kansioon määritetyllä tavalla järvi tietovaraston tulosteen Data Explorer-ruudun asetukset (`streamanalytics/job/output/{date}/{time}`).  

## <a name="see-also"></a>Katso myös

* [Luo HDInsight-klusterin käyttämään järvi tietosäilö](data-lake-store-hdinsight-hadoop-use-portal.md)
