<properties
    pageTitle="Skaalattavissa tietojen tiede-Azure tietojen Lake: lopusta loppuun-ongelmatilanteita | Microsoft Azure"
    description="Vaiheittaiset ohjeet tietojen tarkasteluun ja binaarinen luokitus tehtävät-tietojoukko Azure tietojen järvi avulla."  
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="bradsev;weig"/>


# <a name="scalable-data-science-in-azure-data-lake-an-end-to-end-walkthrough"></a>Skaalattavissa tietojen tiede-Azure tietojen Lake: lopusta loppuun-ongelmatilanteita

Tätä vaiheittaista esitetään, kuinka voit tehdä tietojen tarkasteluun ja Kokousesitelmän taksin matkan otoksen tehtävien binaarinen luokittelu ja kuljetusmaksua tietojoukko ennustaa riippumatta siitä, onko Vihje kiinnitetään maksun Azure tietojen järvi avulla. [Ryhmän tietojen tiede prosessin](http://aka.ms/datascienceprocess)vaiheiden ohjaa-loppuun, hankintaa koulutus malliin ja sitten verkkopalveluun, joka julkaisee mallin käyttöönottoon.


### <a name="azure-data-lake-analytics"></a>Azure tietojen järvi Analytics

[Microsoft Azure tietojen järvi](https://azure.microsoft.com/solutions/data-lake/) on kaikki ominaisuuksia, joita tarvitaan helposti tietojen tutkijoiden ja tallentaa tiedot koon, muodon ja nopeuden ja tietojen käsittely, kehittyneen analyysin ja konepohjaisten oppimistekniikoiden mallinnus kanssa hyvin skaalattavuus edullinen tavalla.   Voit maksaa työn kohden-peruste vain, kun tietoja käsitellään todella. Azure tietojen järvi Analytics sisältää U-SQL-, kieli, joka yhdistää SQL määritettäviä laatu kanssa ilmeikäs potenssiin C# antamaan skaalattava jaettu kysely-ominaisuus. Sen avulla voit käsitellä erimuotoisia tietoja lisäämällä luku-rakenne, lisätä mukautetun logiikan ja käyttäjän määrittämät funktiot (UDF) ja sisältää laajennettavuus siitä, kuinka tasolla tietoa hakuindeksointi voidaan suojata hallintaa. Lisätietoja suunnittelumalli takana U-SQL-blogimerkinnässä [Visual Studio](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).

Tietoja järvi Analytics on myös Cortana Analytics-ohjelmistopaketti ja se toimii SQL Azure-tietovarasto, Power BI ja Data Factory avaimen osa. Kyseinen rivimäärä on valmis cloud big datasta ja kehittyneen analyysin ympäristössä.

Tätä vaiheittaista aloittaa edellytykset ja kanssa, jotka muodostavat tietojen tiede prosessi ja miten voit asentaa ne tietojen järvi Analytics tehtävien suorittamiseen tarvittavat resurssit. Sen avulla U SQL tietojen käsittely vaiheet ja tekee näyttämällä Python ja rakenteen käyttäminen Azure koneen Learning Studiossa voivat laatia ja kehittää ennakoivan mallit. 


### <a name="u-sql-and-visual-studio"></a>U-SQL- ja Visual Studio

Tätä vaiheittaista suosittelee Muokkaa U-SQL-komentosarjat käsittelemään dataset Visual Studion avulla. U-SQL-komentosarjat kuvatut ja annettu erilliseen tiedostoon. Sisältää ingesting ja tutkiminen näyte tiedot. Se näyttää myös suorittaminen työn komentosarjatoimintojen U-SQL Azure-portaalista. Rakenteen taulukot luodaan tietojen liittyvän HDInsight-klusterin rakennuksen ja Azure koneen Learning Studiossa binaarinen luokitus-mallin käyttöönoton helpottamiseksi.  


### <a name="python"></a>Python

Tämä vaiheittainen kuvaus sisältää myös osan, joka näyttää, miten muodosta ja ota käyttöön ennakoivan mallin avulla Python Azure koneen Learning Studiossa.  Annamme Jupyter muistikirjan seuraavasti tässä prosessissa Python komentosarjojen kanssa. Muistikirja on joitakin lisäominaisuus suunnitteluryhmät vaiheet ja mallien rakentaminen kuten multiclass luokittelu ja lisäksi binaarinen luokitus-malli napsauttamalla mallinnus regression koodi. Regressiosuoran tehtävä on ennustaa Vihje Vihje muiden ominaisuuksien perusteella määrää. 


### <a name="azure-machine-learning"></a>Azure koneen oppiminen
Azure koneen Learning Studio käytetään muodosta ja ota käyttöön ennakoivan mallit. Tämä on valmis käyttämällä kahdella tavalla: ensin Python komentosarjojen ja sitten HDInsight (Hadoop)-klusterin taulukoiden rakennetta.


### <a name="scripts"></a>Komentosarjojen

Vain tärkeimmät vaiheet kuvattuja tämän vaiheittaisen kuvauksen suorittamiseksi. Koko **U SQL-komentosarja** ja **Jupyter muistikirjan** voi ladata [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).


## <a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat näitä ohjeita, sinulla on oltava seuraavasti:

- Azure tilaus. Jos sinulla ei ole yhtä, katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- [Suositellaan] Visual Studio 2013 tai 2015. Jos sinulla ei ole mitään näistä asennettuna, voit ladata maksuttoman yhteisön edition [täältä](https://www.visualstudio.com/visual-studio-homepage-vs.aspx). Valitse Visual Studio-osassa **yhteisön 2015 Lataa** -painiketta. 

>[AZURE.NOTE] Visual Studio sijaan voit käyttää myös Azure-portaalin lähettää Azure tietojen järvi kyselyt. Microsoft toimittaa ohjeita siitä, miten voit tehdä sekä ja Visual Studio ja kohtaan **prosessin tiedot ja U-SQL**-portaalissa. 

- Kirjautuminen Azure tietojen järvi esikatselu

>[AZURE.NOTE] Sinun on hankittava hyväksynnän käyttämään Azure tietojen järvi Store (ADLS) ja Azure tietojen järvi Analytics (ADLA) palveluista ovat esikatselussa. Voit pyydetään, kun olet luonut ensimmäisen ADLS tai ADLA. Sigh ylöspäin, valitse **Rekisteröidy esikatselu**, lue ja valitse **OK**. Esimerkiksi näin ADLS rekisteröinti-sivulla:

 ![2](./media/machine-learning-data-science-process-data-lake-walkthrough/2-ADLA-preview-signup.PNG)


## <a name="prepare-data-science-environment-for-azure-data-lake"></a>Azure tietojen järvi tietojen tiede ympäristön valmisteleminen
Näiden vaiheiden tietojen tiede-ympäristön valmistelemisesta on luominen on seuraavissa resursseissa:

- Azure järvi tietovarasto (ADLS) 
- Azure tietojen järvi Analytics (ADLA)
- Azure Blob storage-tili
- Azure koneen Learning Studio-tili
- Azure tietojen järvi Tools for Visual Studio (suositus)

Tässä osassa on ohjeita siitä, miten voit luoda näitä resursseja. Jos rakenteen taulukoiden käyttäminen Azure koneen Learning sijaan Python, voit luoda mallin, myös tarvitset valmistelu (Hadoop) HDInsight-klusterin. Tämä vaihtoehtoinen kuvattua kuvattu ohjeita.
<br/>
>AZURE. Huomautus **Azure järvi tietovaraston** voidaan luoda joko erikseen tai kun luot **Azure tietojen järvi Analytics** kuin oletusarvo-tallennustilan. Ohjeita viitataan luomisen erikseen alla näitä resursseja, mutta tietojen järvi-tallennustilan tilin ei tarvitse luoda erikseen.
<br/>
### <a name="create-an-azure-data-lake-store"></a>Luo Azure tietojen järvi-säilö

Luo ADLS [Azure Portal](http://portal.azure.com). Lisätietoja on artikkelissa [Create HDInsight-klusterin tietojen järvi kaupan käyttämällä Azure-portaalissa](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md). Muista **Vaihtoehtoinen määritys** -sivu on kuvattu **tietolähde** -sivu-klusterin AAD-tunnistetietojen määrittäminen. 

 ![3](./media/machine-learning-data-science-process-data-lake-walkthrough/3-create-ADLS.PNG)


### <a name="create-an-azure-data-lake-analytics-account"></a>Azure tietojen järvi Analytics-tilin luominen
Luo ADLA tili [Azure-portaalissa](http://portal.azure.com). Lisätietoja on artikkelissa [Opetusohjelma: Azure tietojen järvi Analytics Azure-portaalissa aloittaminen](../data-lake-analytics/data-lake-analytics-get-started-portal.md). 

 ![4](./media/machine-learning-data-science-process-data-lake-walkthrough/4-create-ADLA-new.PNG)


### <a name="create-an-azure-blob-storage-account"></a>Luo tili Azure-Blob-objektien tallennustilaan
Luo Azure Blob storage tilin [Azure-portaalissa](http://portal.azure.com). Lisätietoja on artikkelissa Create tallennustilan asiakas-osio [tietoja Azure](../storage/storage-create-storage-account.md)-tallennustilan tilin.
    
 ![5](./media/machine-learning-data-science-process-data-lake-walkthrough/5-Create-Azure-Blob.PNG)


### <a name="set-up-an-azure-machine-learning-studio-account"></a>Azure koneen Learning Studio-tilin määrittäminen
Kirjaudu ylös/kyselyjä Azure koneen Learning Studio [Azure koneen oppiminen](https://azure.microsoft.com/services/machine-learning/) -sivulta. Napsauta **Aloita** -painiketta ja valitse sitten "Vapaa työtilan" tai "Vakio työtilan". Tämän jälkeen osaat luoda kokeissa Azure ML Studiossa.  

### <a name="install-azure-data-lake-tools-recommended"></a>Asenna Azure järvi Datatyökalut [suositellaan]
Asenna järvi Azure Datatyökalut Visual Studio [Azure tietojen järvi Tools for Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504)-versiosi.

 ![6](./media/machine-learning-data-science-process-data-lake-walkthrough/6-install-ADL-tools-VS.PNG)

Kun asennus on suoritettu onnistuneesti, Avaa Visual Studio. Raportissa pitäisi näkyä tietojen järvi välilehden yläreunassa olevaa valikkoa. Azure resurssien näkyy vasemmassa ruudussa kirjauduttaessa Azure-tili.

 ![7](./media/machine-learning-data-science-process-data-lake-walkthrough/7-install-ADL-tools-VS-done.PNG)


## <a name="the-nyc-taxi-trips-dataset"></a>Kokousesitelmän taksin Trips-tietojoukko
Tietojoukon on käytetty tähän on yleisesti saatavilla tietojoukko-- [Kokousesitelmän taksin Trips tietojoukko](http://www.andresmh.com/nyctaxitrips/). Kokousesitelmän taksin työmatkan tiedot koostuu noin 20 gt: n pakattu CSV-tiedostoja (noin 48 gt puretaan), jokaisen matkan tallenteen 173 miljoonaa yksittäisiä trips ja kuljetusmaksut maksettu. Työmatkan kunkin tietueen sisältää nouto ja latauskirjaston sijainnit ja ajat, anonymized hakkeroida (ohjaimen) käyttöoikeuden numero ja medallion (taksin on yksilöllinen tunnus)-numero. Tietoja kattaa kaikki trips vuoden 2013 ja annetaan seuraavat kaksi tietojoukkoja jokaiselle kuukaudelle:

 - 'trip_data' CSV sisältää työmatkan tiedot, kuten matkustajien, nouto ja dropoff pistettä, työmatkan keston ja työmatkan pituus määrän. Tässä on muutama Esimerkki tietue:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count, trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

 - 'trip_fare' CSV on maksettu kunkin työmatkan, kuten maksutyyppi, maksun summa, Lisämaksu ja verot, vihjeet ja tietullit, maksun ja yhteensä maksetun summan tiedot. Tässä on muutama Esimerkki tietue:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Yksilöivä liittymään työmatkan\_tiedot ja työmatkan\_maksun koostuu seuraavat kolme kenttää: medallion, hakkeroida\_käyttöoikeus- ja nouto\_datetime. Raaka CSV-tiedostoja voi käyttää julkisen Azure tallennustilan Blob-objektien. Tämä liitoksen U-SQL-komentosarja on [Liity työmatkan ja maksun taulukot](#join) -osassa.

## <a name="process-data-with-u-sql"></a>Prosessin U-SQL-tietojasi

Esitelty tämän osan tietojen käsittely tehtävät ovat ingesting, laadun tarkistaminen, tutustuminen ja näytteiden tiedot. Olemme näyttää liittyminen työmatkan ja maksun taulukot. Viimeisessä osassa näkyy Suorita U SQL Azure-portaalista komentosarjaan määritetty työn. Seuraavassa on linkkejä kunkin tästä:

- [Tietoja nieltynä: julkisen Blob-objektien tietojen lukeminen](#ingest)
- [Tietojen laadun tarkistukset](#quality)
- [Tietojen tarkasteluun](#explore)
- [Matkan ja maksun taulukoiden](#join)
- [Tiedot-esimerkkejä](#sample)
- [Suorita U-SQL-työt](#run)

U-SQL-komentosarjat kuvatut ja annettu erilliseen tiedostoon. Voit ladata koko **U-SQL-komentosarjat** [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).

Suorittaa U-SQL-Avaa Visual Studiossa, valitse **tiedoston--> Uusi--> projektin**, valitse **U-SQL-projektin**nimi ja tallenna se kansioon.

![8](./media/machine-learning-data-science-process-data-lake-walkthrough/8-create-USQL-project.PNG)

>[AZURE.NOTE] On mahdollista suorittaa sen sijaan, että Visual Studio U-SQL Azure-portaalin avulla. Voit Siirry Azure tietojen järvi Analytics resurssin portaalissa ja Lähetä kyselyt suoraan nimellä esitelty seuraavan kuvan mukaisesti.

![9](./media/machine-learning-data-science-process-data-lake-walkthrough/9-portal-submit-job.PNG)

### <a name="ingest"></a>Tietojen nieltynä: Tietojen suodattaminen julkisen blob luku

Azure-blob-tietojen sijainti on viitattu nimellä **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name** ja voidaan uuttaa **Extractors.Csv()**. Korvaa oman säilön nimi ja tallennustilaa tilin nimen seuraavat komentosarjat container_name@blob_storage_account_name wasb-osoitteen. Tiedostojen nimet ovat samassa muodossa, koska Microsoft käyttää **työmatkan\_data_ {\*\}.csv** lukea kaikki 12 työmatkan tiedostot. 

    ///Read in Trip data
    @trip0 =
        EXTRACT 
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count string,
        trip_time_in_secs string,
        trip_distance string,
        pickup_longitude string,
        pickup_latitude string,
        dropoff_longitude string,
        dropoff_latitude string
    // This is reading 12 trip data from blob
    FROM "wasb://container_name@blob_storage_account_name.blob.core.windows.net/nyctaxitrip/trip_data_{*}.csv"
    USING Extractors.Csv();

Koska ensimmäisellä rivillä on otsikoita, annettava otsikot ja muuta saraketyypit tarvittavat niistä. Emme voi joko Tallenna käsitellyt tiedot käyttämällä **swebhdfs://data_lake_storage_name.azuredatalakestorage.net/folder_name/file_name**_ Azure tietojen järvi tallentaminen tai Azure-Blob-säiliö tilin käyttämällä **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name**. 

    // change data types
    @trip =
        SELECT 
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        DateTime.Parse(pickup_datetime) AS pickup_datetime,
        DateTime.Parse(dropoff_datetime) AS dropoff_datetime,
        Int32.Parse(passenger_count) AS passenger_count,
        Double.Parse(trip_time_in_secs) AS trip_time_in_secs,
        Double.Parse(trip_distance) AS trip_distance,
        (pickup_longitude==string.Empty ? 0: float.Parse(pickup_longitude)) AS pickup_longitude,
        (pickup_latitude==string.Empty ? 0: float.Parse(pickup_latitude)) AS pickup_latitude,
        (dropoff_longitude==string.Empty ? 0: float.Parse(dropoff_longitude)) AS dropoff_longitude,
        (dropoff_latitude==string.Empty ? 0: float.Parse(dropoff_latitude)) AS dropoff_latitude
    FROM @trip0
    WHERE medallion != "medallion";

    ////output data to ADL
    OUTPUT @trip   
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_trip.csv"
    USING Outputters.Csv(); 

    ////Output data to blob
    OUTPUT @trip   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_trip.csv"
    USING Outputters.Csv();  

Lisää vastaavasti emme voi lukea maksun pisteparin. Napsauta hiiren kakkospainikkeella Azure järvi tietovaraston ja voit tarkastella **Azure Portal--> Data Explorer** tai **Resurssienhallinta** Visual Studion tietojen. 

 ![10](./media/machine-learning-data-science-process-data-lake-walkthrough/10-data-in-ADL-VS.PNG)

 ![11](./media/machine-learning-data-science-process-data-lake-walkthrough/11-data-in-ADL.PNG)


### <a name="quality"></a>Tietojen laadun tarkistukset

Kun työmatkan ja maksun taulukot on luettu, tietojen laadun tarkistukset voidaan toteuttaa alla kuvatulla tavalla. Tuloksena oleva CSV-tiedostoja voidaan siirtää Azure-Blob-säiliö tai Azure Lake Tietosäilölle. 

Etsi medallions yksilöllinen numero ja medallions määrä:

    ///check the number of medallions and unique number of medallions
    @trip2 =
        SELECT
        medallion,
        vendor_id,
        pickup_datetime.Month AS pickup_month
        FROM @trip;
    
    @ex_1 =
        SELECT
        pickup_month, 
        COUNT(medallion) AS cnt_medallion,
        COUNT(DISTINCT(medallion)) AS unique_medallion
        FROM @trip2
        GROUP BY pickup_month;
        OUTPUT @ex_1   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_1.csv"
    USING Outputters.Csv(); 

Etsi ne, jotka ovat yli 100 trips medallions:

    ///find those medallions that had more than 100 trips
    @ex_2 =
        SELECT medallion,
               COUNT(medallion) AS cnt_medallion
        FROM @trip2
        //where pickup_datetime >= "2013-01-01t00:00:00.0000000" and pickup_datetime <= "2013-04-01t00:00:00.0000000"
        GROUP BY medallion
        HAVING COUNT(medallion) > 100;
        OUTPUT @ex_2   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_2.csv"
    USING Outputters.Csv(); 

Etsi virheellinen tietueet kannalta pickup_longitude:

    ///find those invalid records in terms of pickup_longitude
    @ex_3 =
        SELECT COUNT(medallion) AS cnt_invalid_pickup_longitude
        FROM @trip
        WHERE
        pickup_longitude <- 90 OR pickup_longitude > 90;
        OUTPUT @ex_3   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_3.csv"
    USING Outputters.Csv(); 

Puuttuvien arvojen etsiminen joitakin muuttujat:

    //check missing values
    @res =
        SELECT *,
               (medallion == null? 1 : 0) AS missing_medallion
        FROM @trip;
    
    @trip_summary6 =
        SELECT 
            vendor_id,
        SUM(missing_medallion) AS medallion_empty, 
        COUNT(medallion) AS medallion_total,
        COUNT(DISTINCT(medallion)) AS medallion_total_unique  
        FROM @res
        GROUP BY vendor_id;
    OUTPUT @trip_summary6
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_16.csv"
    USING Outputters.Csv();



### <a name="explore"></a>Tietojen tarkasteluun

Emme voi tehdä joidenkin tietojen tarkasteluun, saat paremman käsityksen siitä, tiedot.

Etsi Kallistettu ja Kallistettu trips jakautumisen:

    ///tipped vs. not tipped distribution
    @tip_or_not =
        SELECT *,
               (tip_amount > 0 ? 1: 0) AS tipped
        FROM @fare;
    
    @ex_4 =
        SELECT tipped,
               COUNT(*) AS tip_freq
        FROM @tip_or_not
        GROUP BY tipped;
        OUTPUT @ex_4   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_4.csv"
    USING Outputters.Csv(); 

Etsi Vihje summa leikkaus arvoilla jakautumisen: 0,5,10 ja 20 dollaria.

    //tip class/range distribution
    @tip_class =
        SELECT *,
               (tip_amount >20? 4: (tip_amount >10? 3:(tip_amount >5 ? 2:(tip_amount > 0 ? 1: 0)))) AS tip_class
        FROM @fare;
    @ex_5 =
        SELECT tip_class,
               COUNT(*) AS tip_freq
        FROM @tip_class
        GROUP BY tip_class;
        OUTPUT @ex_5   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_5.csv"
    USING Outputters.Csv(); 

Etsi työmatkan etäisyys basic tilastotietoja:

    // find basic statistics for trip_distance
    @trip_summary4 =
        SELECT 
            vendor_id,
            COUNT(*) AS cnt_row,
            MIN(trip_distance) AS min_trip_distance,
            MAX(trip_distance) AS max_trip_distance,
            AVG(trip_distance) AS avg_trip_distance 
        FROM @trip
        GROUP BY vendor_id;
    OUTPUT @trip_summary4
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_14.csv"
    USING Outputters.Csv();

Etsi työmatkan etäisyys arvona:

    // find percentiles of trip_distance
    @trip_summary3 =
        SELECT DISTINCT vendor_id AS vendor,
                        PERCENTILE_DISC(0.25) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc,
                        PERCENTILE_DISC(0.5) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc,
                        PERCENTILE_DISC(0.75) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc
        FROM @trip;
       // group by vendor_id;
    OUTPUT @trip_summary3
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_13.csv"
    USING Outputters.Csv(); 


### <a name="join"></a>Matkan ja maksun taulukoiden

Matkan ja maksun taulukot on liitetty medallion, hack_license ja pickup_time.

    //join trip and fare table

    @model_data_full =
    SELECT t.*, 
    f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
    (f.tip_amount > 0 ? 1: 0) AS tipped,
    (f.tip_amount >20? 4: (f.tip_amount >10? 3:(f.tip_amount >5 ? 2:(f.tip_amount > 0 ? 1: 0)))) AS tip_class
    FROM @trip AS t JOIN  @fare AS f
    ON   (t.medallion == f.medallion AND t.hack_license == f.hack_license AND t.pickup_datetime == f.pickup_datetime)
    WHERE   (pickup_longitude != 0 AND dropoff_longitude != 0 );

    //// output to blob
    OUTPUT @model_data_full   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_7_full_data.csv"
    USING Outputters.Csv(); 

    ////output data to ADL
    OUTPUT @model_data_full   
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_ex_7_full_data.csv"
    USING Outputters.Csv(); 


Kunkin tason matkustaja Laske-tietueita, keskiarvo Vihje summa, hajonnan Vihje summa, prosenttiosuus Kallistettu trips määrän laskeminen.

    // contigency table
    @trip_summary8 =
        SELECT passenger_count,
               COUNT(*) AS cnt,
               AVG(tip_amount) AS avg_tip_amount,
               VAR(tip_amount) AS var_tip_amount,
               SUM(tipped) AS cnt_tipped,
               (float)SUM(tipped)/COUNT(*) AS pct_tipped
        FROM @model_data_full
        GROUP BY passenger_count;
        OUTPUT @trip_summary8
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_17.csv"
    USING Outputters.Csv();


### <a name="sample"></a>Tiedot-esimerkkejä

Olemme satunnaisesti Valitse ensin 0,1 % tietojen liitetyn taulukon:

    //random select 1/1000 data for modeling purpose
    @addrownumberres_randomsample =
    SELECT *,
            ROW_NUMBER() OVER() AS rownum
    FROM @model_data_full;
    
    @model_data_random_sample_1_1000 =
    SELECT *
    FROM @addrownumberres_randomsample
    WHERE rownum % 1000 == 0;
    
    OUTPUT @model_data_random_sample_1_1000   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_7_random_1_1000.csv"
    USING Outputters.Csv(); 

Olemme Tee käsittelemätöntä esimerkkejä binaarinen muuttujan tip_class mukaan:

    //stratified random select 1/1000 data for modeling purpose
    @addrownumberres_stratifiedsample =
    SELECT *,
            ROW_NUMBER() OVER(PARTITION BY tip_class) AS rownum
    FROM @model_data_full;
    
    @model_data_stratified_sample_1_1000 =
    SELECT *
    FROM @addrownumberres_stratifiedsample
    WHERE rownum % 1000 == 0;
    //// output to blob
    OUTPUT @model_data_stratified_sample_1_1000   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_9_stratified_1_1000.csv"
    USING Outputters.Csv(); 
    ////output data to ADL
    OUTPUT @model_data_stratified_sample_1_1000   
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_ex_9_stratified_1_1000.csv"
    USING Outputters.Csv(); 


### <a name="run"></a>Suorita U-SQL-työt

Kun olet lopettanut muokkaamisen U-SQL-komentosarjat, voit lähettää ne palvelimeen Azure tietojen järvi Analytics-tilin avulla. Valitse **Tietojen järvi**, **Lähetä työ**, valitse **Analytics-tilin**, valitse **rinnakkaisuus**ja **Lähetä** -painiketta.  

 ![12](./media/machine-learning-data-science-process-data-lake-walkthrough/12-submit-USQL.PNG)

Kun työ noudatetaan onnistuneesti, työtäsi tila näkyvät Visual Studiossa seurantaa varten. Kun työ on päättynyt, voit myös toista työ prosessi ja Etsi pullonkaula toimet työn tehokkuuden parantamiseksi. Voit myös siirtyä Azure-portaaliin tarkistamaan U-SQL-töiden tilan.

 ![13](./media/machine-learning-data-science-process-data-lake-walkthrough/13-USQL-running-v2.PNG)


 ![14](./media/machine-learning-data-science-process-data-lake-walkthrough/14-USQL-jobs-portal.PNG)


Voit tarkistaa Azure-Blob-säiliö tai Azure-portaalin tulosteen tiedostot. Käytämme käsittelemätöntä mallitiedot Microsoftin mallinnus seuraavassa vaiheessa.

 ![15](./media/machine-learning-data-science-process-data-lake-walkthrough/15-U-SQL-output-csv.PNG)

 ![16](./media/machine-learning-data-science-process-data-lake-walkthrough/16-U-SQL-output-csv-portal.PNG)


## <a name="build-and-deploy-models-in-azure-machine-learning"></a>Muodosta ja ota käyttöön Azure koneen Learning mallit

Olemme osoittaa kahden ominaisuuksia, joilla voit tukitietojen Azure koneen Learning luonnissa ja 

- Valitse ensimmäinen vaihtoehto käytetään näytteeksi tiedot, jotka on kirjoitettu Azure-Blob (- **tietojen esimerkkejä** yllä olevassa vaiheessa) ja Python voivat laatia ja kehittää Azure koneen Learning mallit. 
- Toinen vaihtoehto testaat kyselyllä rakenteen suoraan Azure tietojen järvi tiedot. Tämä vaihtoehto edellyttää, että uuden HDInsight-klusterin luominen tai Käytä aiemmin luodun HDInsight-klusterin missä rakenne-taulukoiden osoittamalla Azure järvi tietosäilö NY taksin tiedot.  Käsiteltävät aiheet sekä näistä vaihtoehdoista. 

## <a name="option-1-use-python-to-build-and-deploy-machine-learning-models"></a>Vaihtoehto 1: Käytä Python voivat laatia ja kehittää koneen learning mallit

Muodosta ja ota käyttöön tietokoneen learning mallien avulla Python, voit luoda Jupyter muistikirjan paikallisessa tietokoneessa tai Azure Konepohjaisten Oppimistekniikoiden Studiossa. Annettujen [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough) Jupyter muistikirja on tarkasteleminen, visualisointi tiedot, toiminto tekniikka, mallinnus ja käyttöönottoa koko koodi. Tässä artikkelissa on näkyy vain mallinnus ja käyttöönotto. 

### <a name="import-python-libraries"></a>Tuo Python kirjastot

Jotta voit suorittaa otosten Jupyter muistikirjan tai Python komentosarja tiedosto-pakettien tarvitaan seuraavia Python. Jos käytössäsi on AzureML muistikirjan service-pakkauksissa on asennettu valmiiksi.

    import pandas as pd
    from pandas import Series, DataFrame
    import numpy as np
    import matplotlib.pyplot as plt
    from time import time
    import pyodbc
    import os
    from azure.storage.blob import BlobService
    import tables
    import time
    import zipfile
    import random
    import sklearn
    from sklearn.linear_model import LogisticRegression
    from sklearn.cross_validation import train_test_split
    from sklearn import metrics
    from __future__ import division
    from sklearn import linear_model
    from azureml import services


### <a name="read-in-the-data-from-blob"></a>Lue Blob-objektien tiedot

- Yhteysmerkkijono   

        CONTAINERNAME = 'test1'
        STORAGEACCOUNTNAME = 'XXXXXXXXX'
        STORAGEACCOUNTKEY = 'YYYYYYYYYYYYYYYYYYYYYYYYYYYY'
        BLOBNAME = 'demo_ex_9_stratified_1_1000_copy.csv'
        blob_service = BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
    
- Lue tekstinä

        t1 = time.time()
        data = blob_service.get_blob_to_text(CONTAINERNAME,BLOBNAME).split("\n")
        t2 = time.time()
        print(("It takes %s seconds to read in "+BLOBNAME) % (t2 - t1))

 ![17](./media/machine-learning-data-science-process-data-lake-walkthrough/17-python_readin_csv.PNG)    
 
- Lisää sarakkeiden nimet ja erottaa sarakkeet

        colnames = ['medallion','hack_license','vendor_id','rate_code','store_and_fwd_flag','pickup_datetime','dropoff_datetime',
        'passenger_count','trip_time_in_secs','trip_distance','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'payment_type', 'fare_amount', 'surcharge', 'mta_tax', 'tolls_amount',  'total_amount', 'tip_amount', 'tipped', 'tip_class', 'rownum']
        df1 = pd.DataFrame([sub.split(",") for sub in data], columns = colnames)
    


- Muuttaa joitakin sarakkeita numeerinen

        cols_2_float = ['trip_time_in_secs','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'fare_amount', 'surcharge','mta_tax','tolls_amount','total_amount','tip_amount', 'passenger_count','trip_distance'
        ,'tipped','tip_class','rownum']
        for col in cols_2_float:
            df1[col] = df1[col].astype(float)

### <a name="build-machine-learning-models"></a>Koneen learning mallien rakentaminen

Seuraavassa on luoda binaarinen luokitus mallin ennustaa onko matkan Kallistettu vai ei. Jupyter muistikirjan löydät kaksi muiden mallien: multiclass luokittelu ja regression mallit.

- Ensin annettava tyhjä muuttujat, joilla voidaan luoda scikit – Katso mallit

        df1_payment_type_dummy = pd.get_dummies(df1['payment_type'], prefix='payment_type_dummy')
        df1_vendor_id_dummy = pd.get_dummies(df1['vendor_id'], prefix='vendor_id_dummy')

- Luo tietojen kehys mallinnus

        cols_to_keep = ['tipped', 'trip_distance', 'passenger_count']
        data = df1[cols_to_keep].join([df1_payment_type_dummy,df1_vendor_id_dummy])
        
        X = data.iloc[:,1:]
        Y = data.tipped

- Koulutus ja 60 – 40 Jaa testaaminen

        X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.4, random_state=0)

- Logistista regressiota koulutus määrittäminen

        model = LogisticRegression()
        logit_fit = model.fit(X_train, Y_train)
        print ('Coefficients: \n', logit_fit.coef_)
        Y_train_pred = logit_fit.predict(X_train)

       ![c1](./media/machine-learning-data-science-process-data-lake-walkthrough/c1-py-logit-coefficient.PNG)

- Pisteet testauksen tietojoukon

        Y_test_pred = logit_fit.predict(X_test)

- Laske arvioinnin arvot

        fpr_train, tpr_train, thresholds_train = metrics.roc_curve(Y_train, Y_train_pred)
        print fpr_train, tpr_train, thresholds_train
        
        fpr_test, tpr_test, thresholds_test = metrics.roc_curve(Y_test, Y_test_pred) 
        print fpr_test, tpr_test, thresholds_test
        
        #AUC
        print metrics.auc(fpr_train,tpr_train)
        print metrics.auc(fpr_test,tpr_test)
        
        #Confusion Matrix
        print metrics.confusion_matrix(Y_train,Y_train_pred)
        print metrics.confusion_matrix(Y_test,Y_test_pred)

       ![c2](./media/machine-learning-data-science-process-data-lake-walkthrough/c2-py-logit-evaluation.PNG)


 
### <a name="build-web-service-api-and-consume-it-in-python"></a>Muodosta Web Service API ja käyttää sen Python

Haluat operationalize konepohjaisten oppimistekniikoiden mallin, kun se on muodostettu. Käytämme binaarinen logistista mallin tässä esimerkkinä. Varmista, scikit – Tutustu Paikallinen kone-versio on 0.15.1. Sinun ei tarvitse olla huolissaan siitä, tämä, jos käytät Azure ML studio palvelu.

- Etsi työtilan tunnistetiedot Azure ML studio asetuksista. Valitse Azure koneen Learning Studio **asetukset** --> **nimi** --> **Luvan tunnusten**. 

    ![C3](./media/machine-learning-data-science-process-data-lake-walkthrough/c3-workspace-id.PNG)


        workspaceid = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx'
        auth_token = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx'

- Luo Web-palvelu

        @services.publish(workspaceid, auth_token) 
        @services.types(trip_distance = float, passenger_count = float, payment_type_dummy_CRD = float, payment_type_dummy_CSH=float, payment_type_dummy_DIS = float, payment_type_dummy_NOC = float, payment_type_dummy_UNK = float, vendor_id_dummy_CMT = float, vendor_id_dummy_VTS = float)
        @services.returns(int) #0, or 1
        def predictNYCTAXI(trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH,payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS ):
            inputArray = [trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH, payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS]
            return logit_fit.predict(inputArray)

- Web-palvelu valtuudet

        url = predictNYCTAXI.service.url
        api_key =  predictNYCTAXI.service.api_key
        
        print url
        print api_key

        @services.service(url, api_key)
        @services.types(trip_distance = float, passenger_count = float, payment_type_dummy_CRD = float, payment_type_dummy_CSH=float,payment_type_dummy_DIS = float, payment_type_dummy_NOC = float, payment_type_dummy_UNK = float, vendor_id_dummy_CMT = float, vendor_id_dummy_VTS = float)
        @services.returns(float)
        def NYCTAXIPredictor(trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH,payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS ):
            pass

- Soita Web service API. Voit joutua odottamaan edellisessä vaiheessa 5-10 sekunnin.

        NYCTAXIPredictor(1,2,1,0,0,0,0,0,1)

       ![c4](./media/machine-learning-data-science-process-data-lake-walkthrough/c4-call-API.PNG)


## <a name="option-2-create-and-deploy-models-directly-in-azure-machine-learning"></a>Vaihtoehto 2: Luo ja ota käyttöön mallit suoraan Azure koneen oppiminen

Azure koneen Learning Studio voit lukea tietoja suoraan Azure järvi tietovaraston ja sitten voidaan luoda ja ottaa käyttöön mallit. Tätä tapaa kannattaa käyttää rakennetaulukko, joka osoittaa Azure tietojen järvi liikkeestä. Tämä edellyttää, että erillisessä Azure HDInsight-klusterin valmisteltava, jonka rakenne-taulukko luodaan. Seuraavissa osissa näyttää, miten voit tehdä tämän. 

### <a name="create-an-hdinsight-linux-cluster"></a>Luo HDInsight-Linux klusteri

Luo [Azure Portal](http://portal.azure.com)HDInsight-klusterin (Linux). Lisätietoja on kohdassa **Luo HDInsight-klusterin Azure Lake Tietosäilölle käyttöönsä** [Luo HDInsight-klusterin tietojen järvi kaupan käyttämällä Azure-portaalissa](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

 ![18](./media/machine-learning-data-science-process-data-lake-walkthrough/18-create_HDI_cluster.PNG)

### <a name="create-hive-table-in-hdinsight"></a>Rakenne-taulukon luominen Hdinsightiin

Nyt luoda rakenne taulukot, jota käytetään Azure koneen Learning Studiossa HDInsight-klusterin käyttämällä Azure järvi tietovaraston edellisessä vaiheessa tallennettuja tietoja. Siirry juuri luomasi HDInsight-klusterin. Valitse **asetukset** --> **Ominaisuudet** --> **Klusterin AAD tunnistetietojen** --> **ADLS Access**, varmista, että Azure järvi tietosäilö-tili on lisätty ja Lue luettelo-, kirjoittaa ja suorita oikeudet. 

 ![19](./media/machine-learning-data-science-process-data-lake-walkthrough/19-HDI-cluster-add-ADLS.PNG)


Valitse **asetukset** -painikkeen vieressä **raporttinäkymät-ikkunan** ja näyttöön ponnahdusikkuna. Valitse **Rakenne Näytä** ylä- ja sivun oikeassa yläkulmassa **Kyselyeditori**tulee näkyviin.

 ![20](./media/machine-learning-data-science-process-data-lake-walkthrough/20-HDI-dashboard.PNG)

 ![21](./media/machine-learning-data-science-process-data-lake-walkthrough/21-Hive-Query-Editor-v2.PNG)


Liitä seuraava rakenne komentosarjoja, voit luoda taulukon. Tietolähteen sijainti on Azure järvi tietovaraston viittauksen tällä tavalla: **adl://data_lake_store_name.azuredatalakestore.net:443/kansion_nimi/tiedostonimi**.

    CREATE EXTERNAL TABLE nyc_stratified_sample
    (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count string,
        trip_time_in_secs string,
        trip_distance string,
        pickup_longitude string,
        pickup_latitude string,
        dropoff_longitude string,
        dropoff_latitude string,
      payment_type string,
      fare_amount string,
      surcharge string,
      mta_tax string,
      tolls_amount string,
      total_amount string,
      tip_amount string,
      tipped string,
      tip_class string,
      rownum string
      )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    LOCATION 'adl://data_lake_storage_name.azuredatalakestore.net:443/nyctaxi_folder/demo_ex_9_stratified_1_1000_copy.csv';


Kun kysely on päättynyt, näet tulokset tältä:

 ![22](./media/machine-learning-data-science-process-data-lake-walkthrough/22-Hive-Query-results.PNG)



### <a name="build-and-deploy-models-in-azure-machine-learning-studio"></a>Muodosta ja ota käyttöön Azure koneen Learning Studiossa mallit

On nyt muodosta ja ota käyttöön mallin, joka ennustaa riippumatta siitä, onko Vihje maksetaan Azure koneen Learning. Käsittelemätöntä esimerkkitiedot on valmis käytettäväksi binaarinen luokitus (Vihje tai ei) ongelma. Ennakoivan mallien multiclass luokitus (tip_class) ja regression (tip_amount) avulla voit myös sisäisten ja Azure koneen Learning Studiossa käyttöön, mutta seuraavassa on vain näkyvät käsittelemisestä binaarinen luokitus-mallin tapauksessa.

1. Tietojen noutaminen Azure ml **Tietojen tuominen** -moduulin, käytettävissä olevat **tiedot tarkistavan ja tulostus** -osassa. Lisätietoja [tietojen tuominen-moduulin](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) viittaus-sivu.
2. Valitse **Kyselyn rakenne** **tietolähteen** **Ominaisuudet** -ruudussa.
3. Liitä seuraava rakenne **tietokantakyselyn rakenne** -editorissa

        select * from nyc_stratified_sample;

4. Kirjoita URI HDInsight-klusterin (tämä löytyy Azure-portaalissa), Hadoop-tunnistetiedot, ja siirtää tietoja ja Azure-tallennustilan tilin nimi/avaimen/säilö nimi.

 ![23](./media/machine-learning-data-science-process-data-lake-walkthrough/23-reader-module-v3.PNG)  

Esimerkki tietojen lukeminen rakennetaulukko binaarinen luokitus-kokeen näkyy alla olevassa kuvassa.

 ![24](./media/machine-learning-data-science-process-data-lake-walkthrough/24-AML-exp.PNG)

Kun kokeilla on luotu, valitse **Määritä Web-palvelu** --> **Ennakoivan Web-palvelu**

 ![25](./media/machine-learning-data-science-process-data-lake-walkthrough/25-AML-exp-deploy.PNG)

Suorita automaattisesti luotu näkyvissä pistemäärä koe, kun se on valmis, valitse **Ota käyttöön Web-palvelu**

 ![26](./media/machine-learning-data-science-process-data-lake-walkthrough/26-AML-exp-deploy-web.PNG)

WWW-palvelun koontinäyttö näkyvät pian:

 ![27](./media/machine-learning-data-science-process-data-lake-walkthrough/27-AML-web-api.PNG)


## <a name="summary"></a>Yhteenveto

Noudata tämän vaiheittaisen kuvauksen suorittamiseksi luomasi tietojen tiede-ympäristön etsimisen skaalattava Azure tietojen järvi lopusta loppuun-ratkaisuja. Tässä ympäristössä käytettiin analysoida suuria julkisen tietojoukko, ottaen tiede prosessi, hankintaa mallin koulutus – ja mallin käyttöönottoon web-palvelu-canonical vaiheet. U SQL käytettiin käsitellä, tutki ja mallitiedot. Python ja rakenteen vieläkin Azure koneen Learning Studiossa muodosta ja ota käyttöön ennakoivan mallit.

## <a name="whats-next"></a>Mitä seuraavaksi?

[Ryhmän tietojen tiede prosessi (TDSP)](http://aka.ms/datascienceprocess) oppimispolkuun on linkit ohjeaiheisiin kuvaava vaiheittaiset ohjeet kehittyneen analyysin prosessi. On vaiheittaiset ohjeet eritelty [ryhmän tietojen tiede prosessin vaiheittaiset ohjeet](data-science-process-walkthroughs.md) -sivulle, joka korostaa resurssit ja palvelut käyttämisestä eri ennakoivan analytics tilanteissa sarja:

- [Ryhmän tietojen tiede prosessin käytössä: SQL-tietovarasto käyttäminen](machine-learning-data-science-process-sqldw-walkthrough.md)
- [Ryhmän tietojen tiede prosessin käytössä: HDInsight Hadoop klustereiden käyttäminen](machine-learning-data-science-process-hive-walkthrough.md)
- [Ryhmän tietojen tiede prosessi: SQL Serverin avulla](machine-learning-data-science-process-sql-walkthrough.md)
- [Tietoja tiede prosessin ohjattu käyttäminen Azure Hdinsightiin](machine-learning-data-science-spark-overview.md)

