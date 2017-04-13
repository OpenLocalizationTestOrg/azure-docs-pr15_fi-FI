<properties
    pageTitle="Markkinatunnelma analyysi käyttämällä Azure Stream analyysin ja Azure koneen Learning | Microsoft Azure"
    description="Käyttäjän määrittämien funktioiden ja tietokoneen oppiminen käyttämisestä Stream Analytics-työ"
    keywords=""
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
    ms.date="10/04/2016" 
    ms.author="jeffstok"
/>

# <a name="sentiment-analysis-by-using-azure-stream-analytics-and-azure-machine-learning"></a>Markkinatunnelma analyysi käyttämällä Azure Stream Analytics ja Azure koneen oppiminen #

Tässä artikkelissa on suunniteltu avulla voit nopeasti määrittää yksinkertaisen Azure Stream Analytics-työ, Azure koneen Learning integrointi. Microsoft käyttää markkinatunnelma analytics koneen Learning mallin Cortana Liiketoimintatieto-valikoimasta streaming tekstitietojen analysointia varten, ja määrittää markkinatunnelma kirjainarvosanan reaaliajassa. Tämän artikkelin tiedot auttavat ymmärtämään skenaariot, kuten reaaliaikainen markkinatunnelma analytics-tietovirta Twitter-tiedot, analysoida asiakkaan keskustelut ja tukihenkilökunta tietueita ja arvioida kommenttien keskustelupalsoille, blogeihin ja videot ja muut reaaliaikaiset, ennakoivan tulosmalli skenaarioita lisäksi.

Tässä artikkelissa on CSV‑mallitiedostoa tekstillä syötteeksi Azure-Blob-säiliö seuraavan kuvan mukaisesti. Työn sovelletaan markkinatunnelma analytics-mallin käyttäjän määrittämien funktioiden (UDF) tekstin mallitiedot-blob-kaupasta. Lopputulos sijoitetaan samassa blob säilössä, toiseen CSV-tiedostoon. 

![Virta Analytics koneen oppiminen](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-2.png)  

Seuraavassa kuvassa näytetään määritysten. Realistisesta skenaarion streaming Azure tapahtuman keskittimet-syötteen tietoja Twitter-Blob-säiliö tilalle. Voit lisäksi luoda [Microsoft Power BI](https://powerbi.microsoft.com/) reaaliaikainen visualisoinnin, kooste markkinatunnelma.    

![Virta Analytics koneen oppiminen](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-1.png)  

## <a name="prerequisites"></a>Edellytykset

Edellytykset, joka on tässä artikkelissa esitellään tehtävien suorittamisesta ovat seuraavat:

-   Aktiivinen Azure-tilaus.
-   CSV-tiedosto, joka ei tietoja. Voit ladata tiedoston näkyvissä kuvassa 1 [GitHub](https://github.com/Azure/azure-stream-analytics/blob/master/Sample Data/sampleinput.csv)tai voit luoda oman tiedoston. Tässä artikkelissa oletetaan, että käytät määrätty GitHub lataaminen.

Korkean tason tässä artikkelissa esitellään tehtävän Tee näin:

1.  Syötteen CSV-tiedoston lataaminen Azure-Blob-säiliö.
2.  Lisää markkinatunnelma analytics mallin Cortana liiketoimintatietojen valikoimasta Azure koneen Learning lisääminen työtilaan.
3.  Tämän mallin käyttöön tietokoneen Learning työtilan WWW-palvelulle.
4.  Luo Stream Analytics-työ, joka kutsuu verkkosovelluksen funktiona, voit määrittää, tekstille, joka on syötetty markkinatunnelma.
5.  Aloita Stream Analytics-työ ja noudata tulos.

## <a name="upload-the-csv-input-file-to-blob-storage"></a>Syötteen CSV-tiedoston lataaminen-Blob-säiliö

Tässä vaiheessa voit käyttää CSV-tiedostoon, kuten kuin ladattavissa GitHub on jo määritetty. Voit käyttää [Azure-tallennustilan Explorer](http://storageexplorer.com/) tai Visual Studio ensin lataamaan tiedosto tai voit käyttää mukautettua koodia. Käytämme esimerkkejä Visual Studio perusteella.

1.  Valitse Visual Studion **Azure** > **tallennustilan** > **Liittää ulkoisen tallennustilan**. Kirjoita **tilin nimi** ja **tili-näppäintä**.  

    ![Virta Analytics koneen oppimista palvelimen Explorer](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-server-explorer.png)  

2.  Laajenna vaiheessa 1 liitetty tallennustilan, valitse **Luo Blob-säilö**ja kirjoita looginen nimi. Kun olet luonut säilö, avata sen ja tarkastella sen sisältöä. (Se on tyhjä tässä vaiheessa).  

    ![Virtauttaa Analytics koneen Learning, Luo blob](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-create-blob.png)  

3.  CSV-tiedoston lataaminen **Lataa Blob-objektien**ja valitse sitten **paikallisen tiedoston**.  

## <a name="add-the-sentiment-analytics-model-from-the-cortana-intelligence-gallery"></a>Lisää markkinatunnelma analytics-malli Cortana Liiketoimintatieto-valikoimasta

1.  Lataa [ennakoivan markkinatunnelma analytics mallin](https://gallery.cortanaintelligence.com/Experiment/Predictive-Mini-Twitter-sentiment-analysis-Experiment-1) Cortana Liiketoimintatieto-valikoimasta.  
2.  Koneen Learning Studiossa Valitse **Avaa Studiossa**.  

    ![Virtauttaa Analytics koneen Learning, Avaa tietokoneen Learning Studio](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-open-ml-studio.png)  

3.  Siirry työtilaan Kirjaudu sisään. Valitse sijainti, joka vastaa parhaiten oman sijainnin.
4.  Valitse sivun alareunassa **Suorita** .  
5.  Kun prosessi on suoritettu onnistuneesti, valitse **WWW-palvelun käyttöön**.
6.  Markkinatunnelma analytics-malli on valmis käytettäväksi. Vahvista- **testi** -painiketta ja antaa tekstiä, kuten "Minusta Microsoft." Testi tulee palauttaa tuloksen seuraavankaltaiselta:

`'Predictive Mini Twitter sentiment analysis Experiment' test returned ["4","0.715057671070099"]...`  

![Virta Analytics koneen Learning analysis-tietoja](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-analysis-data.png)  

Valitse **sovellukset** -sarakkeessa **Excel 2010: n tai aiemman työkirjan** Ohjelmointirajapinnan avain ja URL-osoite, jota tarvitaan myöhemmin määrittämään Stream Analytics-työ linkkiä. (Tämä vaihe on pakollinen vain, jos haluat käyttää tietokoneen Learning mallin työtiloissa Azure-tili. Tässä artikkelissa oletetaan vaatii sähköpostiosoite skenaarion.)  

Huomautus verkkopalvelun URL-osoite ja Accessin avaimeen Excel ladatusta tiedostosta, alla kuvatulla tavalla:  

![Virta Analytics koneen Learning quick glance](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-quick-glance.png)  

## <a name="create-a-stream-analytics-job-that-uses-the-machine-learning-model"></a>Virta Analytics työn, joka käyttää tietokoneen Learning-mallin luominen

1.  Siirry [Azure portal](https://manage.windowsazure.com).  
2.  Valitse **Uusi** > **tietopalvelujen** > **Stream Analytics** > **nopea luominen**. Työtäsi nimi Kirjoita **Projektin nimi**, anna haluamasi alue työn **alueella**ja valitse sitten **Aluekohtaiset seuranta tallennustilan**tilin tili.    
3.  Kun työ on luotu, **syötteiden** -välilehdessä valitsemalla **Lisää syöte**.  

    ![Virtauttaa Analytics koneen Learning, lisää tietokoneen Learning syöte](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-add-input-screen.png)  

4.  **Lisää** ohjattu ensimmäisellä sivulla **tietovirta**ja valitse sitten **Seuraava**. Valitse seuraavalla sivulla **Blob-objektien tallennustilaan** syötteenä ja valitse sitten **Seuraava**.  
5.  Ohjatun toiminnon **Blob Storage asetukset** -sivulla on tallennustilan tilin blob säilön nimi määrittämäsi aiemmin kun tiedot on ladattu palvelimeen. Valitse **Seuraava**. **Tapahtuman Sarjatoiminto Muotoile**-Valitse **CSV**. Hyväksy muiden **Sarjatoiminto asetukset** -sivun oletusarvot. Valitse **OK**.  
6.  **Tulostus** -välilehdessä **Lisää tulos**.  

    ![Lisää tulosteen, virtauttaa Analytics koneen oppiminen](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-add-output-screen.png)  

7.  Valitse **Blob-säiliö**ja kirjoita sitten samat parametrit säilö lukuun ottamatta. **Input** -arvo on määritetty lukea "testi" säilö, jossa **CSV** -tiedoston lataaminen. Kirjoita "testoutput"- **tuloste**. Säilön nimien on oltava eri. Varmista, että tämä säilö.     
8.  Valitse **Seuraava** tuloste **Sarjatoiminto asetusten**määrittäminen. **Syöte**-ja **CSV**ja valitse sitten **OK** -painiketta.
9.  **Toiminnot** -välilehdessä **Lisää tietokoneen Learning-funktio**.  

    ![Virtauttaa Analytics koneen Learning, lisää tietokoneen Learning-funktio](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-add-ml-function.png)  

10. **Koneen Learning WWW-Palveluasetukset** -sivulla Etsi tietokoneen Learning-työtila, web-palvelu ja oletusarvoinen päätepiste. Tämän artikkelin koskevat asetukset manuaalisesti ja myönnä tuntemusta määrittäminen työtila-web-palvelu, kunhan tiedät sen URL-osoite ja että sinulla API-näppäintä. Kirjoita päätepisteen **URL-osoite** ja **Ohjelmointirajapinnan avain**. Valitse **OK**.    

    ![Virta Analytics koneen Learning koneen Learning web-palvelu](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-ml-web-service.png)    

11. Valitse **kysely** -välilehdessä Muokkaa kyselyä seuraavasti:    

 ```
    WITH subquery AS (  
        SELECT text, sentiment(text) as result from input  
    )  
 
    Select text, result.[Scored Labels]  
    Into output  
    From subquery  
 ```    
12. Valitse Tallenna kysely **Tallenna** .

## <a name="start-the-stream-analytics-job-and-observe-the-output"></a>Aloita Stream Analytics-työ ja noudata tulos

1.  Valitse **Käynnistä** työ-sivun alareunassa.
2.  **Käynnistä kysely-valintaikkunassa** **Mukautettu aika**ja valitsemalla sitten ennen aika, kun olet ladannut CSV-Blob-säiliö. Valitse **OK**.  

    ![Virta Analytics koneen Learning, mukautetut aika](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-custom-time.png)  

3.  Siirry Blob-säiliö käytit lataaminen CSV-tiedoston, esimerkiksi Visual Studio-työkalun avulla.
4.  Muutama minuutti, kun työ on alkanut, tulostus-säilö luodaan ja CSV-tiedosto on ladattu siihen.  
5.  Avaa tiedosto oletusarvon CSV-editorissa. Suurin piirtein seuraavanlaisen seuraavassa näytetään:  

    ![Virta Analytics koneen Learning CSV-näkymä](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-csv-view.png)  

## <a name="conclusion"></a>Tekemistä

Tässä artikkelissa kerrotaan, miten Luo Stream Analytics-työ, joka lukee streaming tekstitietoja ja koskee markkinatunnelma analytics reaaliaikaisia tietoja. Voit tehdä nämä eikä hänen nimeään tarvitse olla huolissaan siitä, intricacies muodostamisen markkinatunnelma analytics-malli. Tämä on yksi Cortana Liiketoimintatieto-ohjelmiston eduista.

Voit myös tarkastella Azure koneen Learning funktion liittyvät arvot. Toiminto, valitse **Näyttö** -välilehti. Kolme funktion liittyvät arvot ovat näkyvissä.  

- **Funktion pyynnöt** ilmaisee tietokoneen Learning verkkopalvelun lähetetään pyyntöjen määrä.  
- **Funktion tapahtumien** osoittaa pyynnön tapahtumien määrä. Oletusarvon mukaan sivupyynnön koneen Learning web-palvelu sisältää enintään 1 000 tapahtumat.  
- **Funktion pyynnöt epäonnistui** ilmaisee numeron epäonnistuneiden pyyntöjen koneen Learning web-palveluun.  

    ![Virta Analytics koneen Learning koneen Learning valvonta-näkymään](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-ml-monitor-view.png)  
