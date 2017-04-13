<properties
    pageTitle="Tietoja tiede Scala ja ohjattu käyttäminen Azure | Microsoft Azure"
    description="Opi käyttämään Scala valvonnan alaisena koneen learning tehtävien ohjattu skaalattava MLlib ja ohjattu ML paketit Azure Hdinsightiin ohjattu-klusterissa kanssa."  
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
    ms.date="08/01/2016"
    ms.author="bradsev;deguhath"/>


# <a name="data-science-using-scala-and-spark-on-azure"></a>Tietoja tiede Azure Scala ja ohjattu käyttäminen

Tässä artikkelissa kerrotaan, miten Scala käytettävät valvonnan alaisena koneen learning tehtävien ohjattu skaalattava MLlib ja ohjattu ML paketit Azure Hdinsightiin ohjattu-klusterissa. Se esitellään tehtävät, jotka muodostavat [tietojen tiede prosessi](http://aka.ms/datascienceprocess): tietojen nieltynä ja tarkasteluun, visualisointiin, toiminto-tekniikka, mallinnus ja mallin kulutus. Mallien on artikkelissa ovat logistista ja lineaarisen regressiosuoran, satunnainen metsien ja liukuvärin tehosti puut (GBTs) lisäksi kaksi Yleiset valvonnan alaisena koneen learning toiminnot:

- Regressiosuoran ongelma: tekstinsyöttö taksin matkan Vihje summan ($)
- Binaarinen luokitus: tekstinsyöttö Vihje tai taksin matkan ei Vihje (1/0)

Mallinnus prosessi edellyttää koulutuksen ja arvioinnin testi tietojoukon ja haluamasi tarkkuus arvot. Tässä artikkelissa kerrotaan tallentaminen näitä malleja Azuren Blob-objektien tallennustilaan sekä pisteet ja niiden ennakoivan suorituskyvyn arvioiminen. Tässä artikkelissa käsitellään myös optimoiminen mallien avulla rajat kelpoisuustarkistus ja hyper-parametrin sweeping monimutkaisemman aiheet. Käytettävät tiedot on otoksen 2013 Kokousesitelmän taksin työmatkan ja maksun tietojoukon GitHub käytettävissä.

[Scala](http://www.scala-lang.org/), Java-virtuaalikoneen kielen integroi objektipohjaisten ja toiminnallisia kielen käsitteitä. Se on skaalattava kieli, joka sopii hyvin hyödyntää hajautettu käsittely pilveen ja Azure ohjattu klustereiden suoritetaan.

[Ohjattu](http://spark.apache.org/) on Avaa lähde rinnakkain käsittely framework, joka tukee ladatun käsittelyn edistää big datasta analytics-sovellusten suorituskykyä. Ohjattu käsittely-ohjelma on luotu nopeus-käyttöä ja yksityiskohtaisen analysoinnin helpottamiseksi. Ohjattu päivän ladatun hajautettu laskenta ominaisuuksia Varmista on hyvä iteratiivinen algoritmien koneen opiskelu- ja kaavion funktiolauseita. [Spark.ml](http://spark.apache.org/docs/latest/ml-guide.html) -paketti sisältää yhdenmukaisen korkean tason API sisäisten tietojen kehyksiä, joiden avulla voit luoda ja paranna käytännön konepohjaisten oppimistekniikoiden putkistot päälle. [MLlib](http://spark.apache.org/mllib/) on ohjattu 's skaalattava koneen learning kirjastoon, joka tuo mallinnus ominaisuuksia tässä jaetussa ympäristössä.

[Ohjattu Hdinsightista](../hdinsight/hdinsight-apache-spark-overview.md) on Avaa lähde ohjattu Azure isännöimä tarjoaminen. Se myös tukee Ohjattu klusterin Jupyter Scala muistikirjoja ja vuorovaikutteinen ohjattu SQL-kyselyjä transform, suodattaa ja visualisointi Azure-Blob-objektien tallennustilaan tallennettuja tietoja voidaan suorittaa. Scala koodikatkelmat tämän artikkelin, joiden ratkaisuja ja Näytä ja Visualisoi tietoja asiaa joudutaan Suorita ohjattu klustereiden asennettuihin Jupyter muistikirjoissa. Mallinnus ohjeita seuraavissa artikkeleissa on koodi, joka näytetään, miten kouluttaminen, arvioi, tallentaa ja tarjoaman kunkin mallin tyyppi.

Määrityksen vaiheet ja tämän artikkelin koodi ovat Azure Hdinsightiin 3.4 ohjattu 1.6. Kuitenkin koodin tämän artikkelin ja [Scala Jupyter muistikirjan](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration%20Modeling%20and%20Scoring%20using%20Scala.ipynb) ovat yleisiä ja pitäisi toimia Ohjattu klusterin. Klusterin määritys ja hallinnan vaiheet voivat erota hieman eri tavalla kuin mitä näkyy tämän artikkelin, jos et käytä HDInsight ohjattu.

> [AZURE.NOTE] Katso artikkelista, johon avulla voit suorittaa tehtäviä lopusta loppuun-tietojen tiede prosessin Python Scala sijaan avulla, [Käyttämällä Ohjattu Azure HDInsight-tietojen tiede](machine-learning-data-science-spark-overview.md).


## <a name="prerequisites"></a>Edellytykset

-   Sinulla on oltava Azure tilaus. Jos sinulla ei ole yhtä [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

-   Tarvitset Azure Hdinsightiin 3.4 ohjattu 1.6-klusterin voit suorittaa seuraavat toiminnot. Klusterin luomisesta on artikkelissa ohjeita [aloittaminen: Luo Apache ohjattu Azure Hdinsightiin](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md). Määritä klusterin tyyppi ja versio **Valitse klusterin tyyppi** -valikosta.

![HDInsight klusterin tyypin määrittäminen](./media/machine-learning-data-science-process-scala-walkthrough/spark-cluster-on-portal.png)


>[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


Kuvaus Kokousesitelmän taksin työmatkan tietoja ja suorittaa koodin Ohjattu klusterin Jupyter muistikirjan ohjeita on kohdassa [Yleistä tietojen tiede käyttämällä Ohjattu Azure HDInsight-](machine-learning-data-science-spark-overview.md)haluamasi osat.  


## <a name="execute-scala-code-from-a-jupyter-notebook-on-the-spark-cluster"></a>Suorita Ohjattu klusterin Jupyter muistikirjan Scala koodi

Voit käynnistää Jupyter muistikirjan Azure-portaalista. Etsi raporttinäkymän Ohjattu-klusterin ja napsauttamalla sitä ja kirjoita hallinta-sivun yhteyttä klusterin. Valitse **Klusterin raporttinäkymien**ja valitse **Jupyter muistikirjan** , Avaa Ohjattu klusterin liittyvät muistikirja.

![Klusterin Raporttinäkymät-ikkunan ja Jupyter muistikirjat](./media/machine-learning-data-science-process-scala-walkthrough/spark-jupyter-on-portal.png)

Voit käyttää myös Jupyter muistikirjoja osoitteessa https://&lt;clustername&gt;.azurehdinsight.net/jupyter. Korvaa *clustername* yhteyttä klusterin nimen. Tarvitset Siirry Jupyter muistikirjoihin järjestelmänvalvojan tilin salasana.

![Siirry Jupyter muistikirjat käyttämällä klusterinimeä](./media/machine-learning-data-science-process-scala-walkthrough/spark-jupyter-notebook.png)

Valitse kansio, joka on muutamia esimerkkejä valmiiksi pakattu muistikirjat, jotka käyttävät PySpark Ohjelmointirajapinnan Nähdäksesi **Scala** . Tarkasteluun mallinnus ja käyttämällä Scala.ipynb muistikirjan, joka sisältää tässä Suite ohjattu aiheista MALLIKOODEJA pistelasku on käytettävissä [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/Scala).


Voit ladata muistikirjaa suoraan GitHub Jupyter muistikirjan palvelimeen ohjattu klusterissa. Jupyter aloitus-sivulla Napsauta **Lataa** -painiketta. Resurssienhallinnassa Liitä Scala muistikirjan GitHub (raaka-sisältö) URL-osoite ja valitse sitten **Avaa**. Scala muistikirja on käytettävissä seuraavassa URL-osoitteessa:

[Exploration-Modeling-and-Scoring-using-Scala.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration-Modeling-and-Scoring-using-Scala.ipynb)

## <a name="setup-preset-spark-and-hive-contexts-spark-magics-and-spark-libraries"></a>Määritys: Esiasetus ohjattu ja rakenteen kontekstit, ohjattu magics ja ohjattu kirjastot

### <a name="preset-spark-and-hive-contexts"></a>Ennalta määritetty ohjattu ja rakenteen kontekstit

    # SET THE START TIME
    import java.util.Calendar
    val beginningTime = Calendar.getInstance().getTime()


Ohjattu ytimet Jupyter muistikirjat mukana toimitettuja on valmiiksi kontekstit. Sinun ei tarvitse määrittää ohjattu tai rakenteen kontekstit ennen kuin aloitat käsitteleminen sovelluksen kehittävät. Valmiiksi määritetyt kontekstit ovat seuraavat:

- `sc`SparkContext varten
- `sqlContext`HiveContext varten


### <a name="spark-magics"></a>Ohjattu magics

Ohjattu ydin tarjoaa esimääritettyjä "magics", jotka ovat erityisiä komennot, jotka voivat soittaa kanssa `%%`. Kaksi näitä komentoja voi käyttää seuraavia MALLIKOODEJA.

- `%%local`määrittää, että seuraaville riveille koodi suoritetaan paikallisesti. Koodi on oltava kelvollinen Scala koodi.
- `%%sql -o <variable name>`Rakenne-kysely suoritetaan `sqlContext`. Jos `-o` välitetty parametri, kyselyn tulos on samanlainen `%%local` kuvan ohjattu käyttäminen tietojen Scala kontekstissa.

Katso lisätietoja Jupyter muistikirjat ja niiden valmiin ytimet "magics", puhelu ja `%%` (esimerkiksi `%%local`), katso [ytimet käytettävissä olevien HDInsight HDInsight ohjattu Linux varausyksiköiden muistikirjojen Jupyter](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).


### <a name="import-libraries"></a>Tuo-kirjastot

Tuo ohjattu MLlib ja muita kirjastoja, sinun on seuraava koodi avulla.

    # IMPORT SPARK AND JAVA LIBRARIES
    import org.apache.spark.sql.SQLContext
    import org.apache.spark.sql.functions._
    import java.text.SimpleDateFormat
    import java.util.Calendar
    import sqlContext.implicits._
    import org.apache.spark.sql.Row

    # IMPORT SPARK SQL FUNCTIONS
    import org.apache.spark.sql.types.{StructType, StructField, StringType, IntegerType, FloatType, DoubleType}
    import org.apache.spark.sql.functions.rand

    # IMPORT SPARK ML FUNCTIONS
    import org.apache.spark.ml.Pipeline
    import org.apache.spark.ml.feature.{StringIndexer, VectorAssembler, OneHotEncoder, VectorIndexer, Binarizer}
    import org.apache.spark.ml.tuning.{ParamGridBuilder, TrainValidationSplit, CrossValidator}
    import org.apache.spark.ml.regression.{LinearRegression, LinearRegressionModel, RandomForestRegressor, RandomForestRegressionModel, GBTRegressor, GBTRegressionModel}
    import org.apache.spark.ml.classification.{LogisticRegression, LogisticRegressionModel, RandomForestClassifier, RandomForestClassificationModel, GBTClassifier, GBTClassificationModel}
    import org.apache.spark.ml.evaluation.{BinaryClassificationEvaluator, RegressionEvaluator, MulticlassClassificationEvaluator}

    # IMPORT SPARK MLLIB FUNCTIONS
    import org.apache.spark.mllib.linalg.{Vector, Vectors}
    import org.apache.spark.mllib.util.MLUtils
    import org.apache.spark.mllib.classification.{LogisticRegressionWithLBFGS, LogisticRegressionModel}
    import org.apache.spark.mllib.regression.{LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel}
    import org.apache.spark.mllib.tree.{GradientBoostedTrees, RandomForest}
    import org.apache.spark.mllib.tree.configuration.BoostingStrategy
    import org.apache.spark.mllib.tree.model.{GradientBoostedTreesModel, RandomForestModel, Predict}
    import org.apache.spark.mllib.evaluation.{BinaryClassificationMetrics, MulticlassMetrics, RegressionMetrics}

    # SPECIFY SQLCONTEXT
    val sqlContext = new SQLContext(sc)


## <a name="data-ingestion"></a>Tietoja nieltynä

Tietoja tiede prosessin ensimmäiseksi on ingest tiedot, jotka haluat analysoida. Voit noutaa tietoja ulkoisista lähteistä tai järjestelmistä, jossa se sijaitsee tietojen tarkasteluun ja mallinnus ympäristölle. Tässä artikkelissa voit ingest tiedot ovat liitettyjen 0,1 otoksen taksin työmatkan ja maksun (tallennettu .tsv-tiedostona)-tiedoston. Tietojen tarkasteluun ja mallinnus-ympäristö on ohjattu. Tässä osassa käsitellään seuraavia sarjan tehtävien suorittamiseen koodi:

1. Määritä kansiopolut tiedot ja malli-tallennustilan.
2. Lue syötteen tietojoukon (tallennettu .tsv-tiedostona).
3. Määritä tiedot rakenne ja Puhdista tiedot.
4. Luo puhdistettava tietojen kehyksen ja välimuistiin muistiin.
5. Rekisteröi SQLContext väliaikaisen taulukon tiedot.
6. Taulukon kysely ja tuo tulokset tietojen kehys.


### <a name="set-directory-paths-for-storage-locations-in-azure-blob-storage"></a>Määrittää hakemiston polut tallennuspaikkojen Azure-Blob-säiliö

Ohjattu sähköpostiviestien lukeminen ja kirjoittaminen Azure-Blob-säiliö. Voit ohjattu avulla voit käsitellä aiemmin luotuja tietoja ja tallentaa tulokset uudelleen Blob-objektien tallennustilaan.

Jos haluat tallennetaan Blob-objektien tallennustilaan mallit tai tiedostot, Määritä oikein polku. Viittaus liitetty Ohjattu klusterin käyttämällä polkua, joka alkaa oletusarvon säilön `wasb:///`. Viitata muihin sijainteihin käyttämällä `wasb://`.

Seuraava koodi esimerkki määrittää sijainnin syöttötiedot voi lukea ja Blob-objektien tallennustilaan, joka on liitetty Ohjattu klusterin polku kohtaa, johon haluat tallentaa mallin.

    # SET PATHS TO DATA AND MODEL FILE LOCATIONS
    # INGEST DATA AND SPECIFY HEADERS FOR COLUMNS
    val taxi_train_file = sc.textFile("wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv")
    val header = taxi_train_file.first;

    # SET THE MODEL STORAGE DIRECTORY PATH
    # NOTE THAT THE FINAL BACKSLASH IN THE PATH IS REQUIRED.
    val modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";


### <a name="import-data-create-an-rdd-and-define-a-data-frame-according-to-the-schema"></a>Tuoda tietoja, luoda RDD ja määrittää tietojen kehyksen rakenteen mukaan

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE SCHEMA BASED ON THE HEADER OF THE FILE
    val sqlContext = new SQLContext(sc)
    val taxi_schema = StructType(
        Array(
            StructField("medallion", StringType, true),
            StructField("hack_license", StringType, true),
            StructField("vendor_id", StringType, true),
            StructField("rate_code", DoubleType, true),
            StructField("store_and_fwd_flag", StringType, true),
            StructField("pickup_datetime", StringType, true),
            StructField("dropoff_datetime", StringType, true),
            StructField("pickup_hour", DoubleType, true),
            StructField("pickup_week", DoubleType, true),
            StructField("weekday", DoubleType, true),
            StructField("passenger_count", DoubleType, true),
            StructField("trip_time_in_secs", DoubleType, true),
            StructField("trip_distance", DoubleType, true),
            StructField("pickup_longitude", DoubleType, true),
            StructField("pickup_latitude", DoubleType, true),
            StructField("dropoff_longitude", DoubleType, true),
            StructField("dropoff_latitude", DoubleType, true),
            StructField("direct_distance", StringType, true),
            StructField("payment_type", StringType, true),
            StructField("fare_amount", DoubleType, true),
            StructField("surcharge", DoubleType, true),
            StructField("mta_tax", DoubleType, true),
            StructField("tip_amount", DoubleType, true),
            StructField("tolls_amount", DoubleType, true),
            StructField("total_amount", DoubleType, true),
            StructField("tipped", DoubleType, true),
            StructField("tip_class", DoubleType, true)
            )
        )

    # CAST VARIABLES ACCORDING TO THE SCHEMA
    val taxi_temp = (taxi_train_file.map(_.split("\t"))
                            .filter((r) => r(0) != "medallion")
                            .map(p => Row(p(0), p(1), p(2),
                                p(3).toDouble, p(4), p(5), p(6), p(7).toDouble, p(8).toDouble, p(9).toDouble, p(10).toDouble,
                                p(11).toDouble, p(12).toDouble, p(13).toDouble, p(14).toDouble, p(15).toDouble, p(16).toDouble,
                                p(17), p(18), p(19).toDouble, p(20).toDouble, p(21).toDouble, p(22).toDouble,
                                p(23).toDouble, p(24).toDouble, p(25).toDouble, p(26).toDouble)))


    # CREATE AN INITIAL DATA FRAME AND DROP COLUMNS, AND THEN CREATE A CLEANED DATA FRAME BY FILTERING FOR UNWANTED VALUES OR OUTLIERS
    val taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)

    val taxi_df_train_cleaned = (taxi_train_df.drop(taxi_train_df.col("medallion"))
            .drop(taxi_train_df.col("hack_license")).drop(taxi_train_df.col("store_and_fwd_flag"))
            .drop(taxi_train_df.col("pickup_datetime")).drop(taxi_train_df.col("dropoff_datetime"))
            .drop(taxi_train_df.col("pickup_longitude")).drop(taxi_train_df.col("pickup_latitude"))
            .drop(taxi_train_df.col("dropoff_longitude")).drop(taxi_train_df.col("dropoff_latitude"))
            .drop(taxi_train_df.col("surcharge")).drop(taxi_train_df.col("mta_tax"))
            .drop(taxi_train_df.col("direct_distance")).drop(taxi_train_df.col("tolls_amount"))
            .drop(taxi_train_df.col("total_amount")).drop(taxi_train_df.col("tip_class"))
            .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200"));

    # CACHE AND MATERIALIZE THE CLEANED DATA FRAME IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()

    # REGISTER THE DATA FRAME AS A TEMPORARY TABLE IN SQLCONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Tulos:**

Solun aikaa: 8 sekuntia.


### <a name="query-the-table-and-import-results-in-a-data-frame"></a>Taulukon kysely ja tuo tulokset tietojen kehyksessä

Kyselyn seuraavaksi taulukon maksun, matkustaja ja Vihje tiedot. suodattaminen vahingoittunut ja analysoida tietoja. ja tulostaa useita rivejä.

    # QUERY THE DATA
    val sqlStatement = """
        SELECT fare_amount, passenger_count, tip_amount, tipped
        FROM taxi_train
        WHERE passenger_count > 0 AND passenger_count < 7
        AND fare_amount > 0 AND fare_amount < 200
        AND payment_type in ('CSH', 'CRD')
        AND tip_amount > 0 AND tip_amount < 25
    """
    val sqlResultsDF = sqlContext.sql(sqlStatement)

    # SHOW ONLY THE TOP THREE ROWS
    sqlResultsDF.show(3)

**Tulos:**

fare_amount|passenger_count|tip_amount|Kallistettu
-----------|---------------|----------|------
       13.5|            1.0|       2.9|   1.0
       16,0|            2.0|       3.4|   1.0
       10.5|            2.0|       1.0|   1.0


## <a name="data-exploration-and-visualization"></a>Tietojen tarkasteluun ja visualisointi

Jälkeen voit tuoda tietoja Ohjattu, tietojen tiede prosessin seuraava vaihe on saada tarkempaa tietoja tietojen tarkasteluun ja visualisoinnin kautta. Tässä osassa Tarkastele taksin tiedot käyttämällä SQL-kyselyjä. Tuomalla tulosten esitystapa kohde muuttujat ja mahdollisen ominaisuuksia silmämääräisesti Jupyter automaattinen visualisointi-toiminnolla tiedot kehys.

### <a name="use-local-and-sql-magic-to-plot-data"></a>Piirrä tiedot paikallisen ja SQL-tärkeä avulla

Oletusarvon mukaan kaikki koodikatkelman, kun suoritat Jupyter muistikirjan tulosteen on käytettävissä, joka on samanlainen työntekijä solmuissa istunnon kontekstissa. Jos haluat tallentaa matkan jokaisen laskenta työntekijä solmut, ja jos kaikki tarve että laskenta tiedot ovat käytettävissä paikallisesti Jupyter solmu (joka on pään solmu), voit käyttää `%%local` tärkeä Jupyter palvelimessa suoritettavat koodikatkelman.

- **SQL-tärkeä** (`%%sql`). HDInsight ohjattu ydin tukee helposti tekstiin HiveQL kyselyitä, jotka perustuvat SQLContext. (`-o VARIABLE_NAME`) Argumentin jatkuu SQL-kyselyn tulos Pandas tietojen kuvan käyttäminen Jupyter-palvelimeen. Tämä tarkoittaa, että se on käytettävissä paikallisten-tilassa.
- `%%local`**tärkeä**. `%%local` Tärkeä suorittaa koodin paikallisesti Jupyter palvelimessa, joka on HDInsight-klusterin pään solmu. Yleensä käytetään `%%local` magic yhdessä `%%sql` magic kanssa `-o` parametria. `-o` Parametrin jatkuvat paikallisesti SQL-kyselyn tulos ja valitse sitten `%%local` tärkeä käynnistää koodikatkelman suorittaa paikallisesti vastaan SQL-kyselyjä, joka on samanlainen paikallisesti tulosteen Seuraava joukko.

### <a name="query-the-data-by-using-sql"></a>Kyselyn tiedot käyttämällä SQL
Tämä kysely palauttaa taksin-trips maksun summa, matkustaja määrä ja Vihje summa.

    # RUN THE SQL QUERY
    %%sql -q -o sqlResults
    SELECT fare_amount, passenger_count, tip_amount, tipped FROM taxi_train WHERE passenger_count > 0 AND passenger_count < 7 AND fare_amount > 0 AND fare_amount < 200 AND payment_type in ('CSH', 'CRD') AND tip_amount > 0 AND tip_amount < 25

Valitse seuraava koodi `%%local` tärkeä Luo paikalliset tiedot kehys sqlResults. Voit piirtää avulla matplotlib sqlResults.

> [AZURE.TIP] Paikallinen tärkeä käytetään useita kertoja tässä artikkelissa. Jos tietojoukko on suuri, esimerkki Ota kehykseksi tiedot, jotka mahtuu luominen paikalliseen muistiin.

### <a name="plot-the-data"></a>Piirrä tiedot

Voit piirtää Python-koodin avulla, kun tietojen kehys on paikallinen kontekstissa Pandas tietojen kuvan käyttäminen.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES.
    # CLICK THE TYPE OF PLOT TO GENERATE (LINE, AREA, BAR, ETC.)
    sqlResults


 Ohjattu ydin automaattisesti valitseminen puolestaan Visualisoi (HiveQL) SQL-kyselyjä tulosteen koodin suorittamisen jälkeen. Voit valita useita erilaisia visualisointeja:
 
- Taulukko
- Ympyräkaavio
- Rivi
- Alue
- Palkkikaavio

Näin tietojen koodi:

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline

    # PLOT TIP BY PAYMENT TYPE AND PASSENGER COUNT
    ax1 = sqlResults[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()

    # PLOT TIP BY PASSENGER COUNT
    ax2 = sqlResults.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()

    # PLOT TIP AMOUNT BY FARE AMOUNT; SCALE POINTS BY PASSENGER COUNT
    ax = sqlResults.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(sqlResults.passenger_count))
    ax.set_title('Tip amount by Fare amount')
    ax.set_xlabel('Fare Amount ($)')
    ax.set_ylabel('Tip Amount ($)')
    plt.axis([-2, 80, -2, 20])
    plt.show()


**Tulos:**

![Vihje summa Histogrammi](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-histogram.png)

![Vihje summa matkustaja määrän mukaan](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-by-passenger-count.png)

![Vihje määrä, maksun summa](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-by-fare-amount.png)


## <a name="create-features-and-transform-features-and-then-prep-data-for-input-into-modeling-functions"></a>Luo ominaisuudet ja muunna ominaisuudet ja valmistelu tietojen mallinnus Funktiot-syötteestä

Puun perustuva mallinnus toimintojen ohjattu ML ja MLlib sinun on valmisteleminen käyttämällä erilaisia tekniikoita, kuten binning, indeksoinnin, yksi kuuman koodaus ja vectorization kohde ja toiminnot. Seuraavassa on tämän osan ohjeita:

1. Luo uusi ominaisuus liikenne aika uudelleenkäytettävät **binning** tuntia.
2. **Indeksoinnin ja yksi kuuman koodaus** koske categorical ominaisuuksia.
3. **Esimerkki ja jaa tietojoukon** koulutusta ja testaa murtolukuja.
4. **Määritä koulutus muuttujan ja toiminnot**, ja luo sitten indeksoiduista tai yksi kuuman koodattu koulutus testaaminen syötteen nimetty pisteen joustavat distributed tietojoukkoja (RDDs) tai kehykset.
5. Automaattisesti **luokitella ja vectorize ominaisuudet ja kohteiden** syötteiden käytettävän tietokoneen learning mallit.


### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a>Luo uusi ominaisuus binning tuntia liikenne aika uudelleenkäytettävät

Tämä koodi näkyy poistamisesta Luo uuden toiminnon liikenne aika uudelleenkäytettävät binning tuntia ja välimuistiin tuloksena olevat tiedot kehys, muistiin. Parannettu suorittamisen kertaa liidejä käytettyjen RDDs ja tietojen kehykset toistuvasti välimuistiin. Näin ollen välimuistiin RDDs ja kehykset useita vaiheissa seuraavasti.

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    val sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night"
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush"
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_train
    """
    val taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)

    # CACHE THE DATA FRAME IN MEMORY AND MATERIALIZE THE DATA FRAME IN MEMORY
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()


### <a name="indexing-and-one-hot-encoding-of-categorical-features"></a>Indeksoinnin ja yksi kuuman koodauksen categorical ominaisuudet

Mallinnus ja ennustaa MLlib toiminnot vaativat ominaisuuksien categorical syöttötiedot indeksoitu tai koodattu ennen käyttöä. Tässä osassa näytetään, miten indeksoida tai koodata mallinnus-Funktiot-syötteestä categorical ominaisuuksia.

Sinun täytyy indeksoida tai koodata eri tavoin sen mukaan, mallin mallit. Esimerkiksi logistista ja lineaarisen regressiosuoran mallit edellyttävät yksi kuuman koodaus. Esimerkiksi ominaisuuden, jossa on kolme luokkaa voidaan laajentaa kolmeen toiminto-sarakkeeseen. Kukin sarake sisältää 0 tai 1 Huomautus luokan mukaan. MLlib on yksi kuuman koodaus [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) -funktiota. Tämä encoder yhdistää sarakkeen otsikon indeksien binaarinen vektorit arvolla enintään yhden yksi-yhteen sarakkeeseen. Koodausta, algoritmeista, odottaa numeerisen arvon ominaisuuksia, kuten logistista regressiota voi suojata categorical ominaisuuksia.

Tässä voit muuntaa vain neljä muuttujat näyttää esimerkkejä, jotka ovat merkkijonoja. Voit myös indeksoida muiden muuttujien viikonpäivä, kuten categorical muuttujat numeeriset arvot, joita edustaa.

Indeksoinnin, käytä `StringIndexer()`, ja yhden kuuman koodaus, käytä `OneHotEncoder()` funktioiden MLlib. Näin koodin indeksoida ja koodata categorical ominaisuuksia:


    # CREATE INDEXES AND ONE-HOT ENCODED VECTORS FOR SEVERAL CATEGORICAL FEATURES

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # INDEX AND ENCODE VENDOR_ID
    val stringIndexer = new StringIndexer().setInputCol("vendor_id").setOutputCol("vendorIndex").fit(taxi_df_train_with_newFeatures)
    val indexed = stringIndexer.transform(taxi_df_train_with_newFeatures)
    val encoder = new OneHotEncoder().setInputCol("vendorIndex").setOutputCol("vendorVec")
    val encoded1 = encoder.transform(indexed)

    # INDEX AND ENCODE RATE_CODE
    val stringIndexer = new StringIndexer().setInputCol("rate_code").setOutputCol("rateIndex").fit(encoded1)
    val indexed = stringIndexer.transform(encoded1)
    val encoder = new OneHotEncoder().setInputCol("rateIndex").setOutputCol("rateVec")
    val encoded2 = encoder.transform(indexed)

    # INDEX AND ENCODE PAYMENT_TYPE
    val stringIndexer = new StringIndexer().setInputCol("payment_type").setOutputCol("paymentIndex").fit(encoded2)
    val indexed = stringIndexer.transform(encoded2)
    val encoder = new OneHotEncoder().setInputCol("paymentIndex").setOutputCol("paymentVec")
    val encoded3 = encoder.transform(indexed)

    # INDEX AND TRAFFIC TIME BINS
    val stringIndexer = new StringIndexer().setInputCol("TrafficTimeBins").setOutputCol("TrafficTimeBinsIndex").fit(encoded3)
    val indexed = stringIndexer.transform(encoded3)
    val encoder = new OneHotEncoder().setInputCol("TrafficTimeBinsIndex").setOutputCol("TrafficTimeBinsVec")
    val encodedFinal = encoder.transform(indexed)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Tulos:**

Solun aikaa: 4 sekuntia.



### <a name="sample-and-split-the-data-set-into-training-and-test-fractions"></a>Esimerkki ja jaa tietojoukon kyselyjä koulutus ja testi

Tämä koodi luo satunnaisen esimerkkejä tiedot (Tässä esimerkissä 25 %). Vaikka esimerkkejä ei edellytä tässä esimerkissä koon vuoksi tietojoukon, on artikkelissa kerrotaan siitä, miten voit Esimerkki niin, että tiedät, miten sitä käytetään omia ongelmia tarvittaessa. Kun objektit on suuri, tämä säästää aikaa merkittäviä samalla, kun kouluttaminen mallit. Seuraavaksi Jaa otosten koulutus-osa (Tässä esimerkissä 75 %) ja testauksen osa (Tässä esimerkissä 25 %) luokittelu ja regression mallinnus.

Lisää kullekin riville ("SATUNNAISLUKU"-sarake), jonka avulla voidaan valita rajat vahvistus taitosten tekeminen aikana koulutus satunnaisen numeron (0 – 1).


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    val samplingFraction = 0.25;
    val trainingFraction = 0.75;
    val testingFraction = (1-trainingFraction);
    val seed = 1234;
    val encodedFinalSampledTmp = encodedFinal.sample(withReplacement = false, fraction = samplingFraction, seed = seed)
    val sampledDFcount = encodedFinalSampledTmp.count().toInt

    val generateRandomDouble = udf(() => {
        scala.util.Random.nextDouble
    })

    # ADD A RANDOM NUMBER FOR CROSS-VALIDATION
    val encodedFinalSampled = encodedFinalSampledTmp.withColumn("rand", generateRandomDouble());

    # SPLIT THE SAMPLED DATA FRAME INTO TRAIN AND TEST, WITH A RANDOM COLUMN ADDED FOR DOING CROSS-VALIDATION (SHOWN LATER)
    # INCLUDE A RANDOM COLUMN FOR CREATING CROSS-VALIDATION FOLDS
    val splits = encodedFinalSampled.randomSplit(Array(trainingFraction, testingFraction), seed = seed)
    val trainData = splits(0)
    val testData = splits(1)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Tulos:**

Solun aikaa: 2 sekunnin ajan.


### <a name="specify-training-variable-and-features-and-then-create-indexed-or-one-hot-encoded-training-and-testing-input-labeled-point-rdds-or-data-frames"></a>Määritä koulutus muuttujan ja ominaisuudet ja luo sitten indeksoiduista tai yksi kuuman koodattu koulutus ja testaus syöte, jossa kohta RDDs tai tietojen kehykset

Tässä osassa on koodi, joka näytetään, miten voit indeksoida categorical tekstitietoja nimetty piste-tietotyypiksi ja koodata se, jotta voit käyttää sitä kouluttaminen ja MLlib logistista regressiota ja luokittelu muiden mallien testaaminen. Nimetty pisteen objekteja, jotka on muotoiltu siten, että tarvitaan kuin syöttötiedot konepohjaisten oppimistekniikoiden MLlib algoritmit useimmat RDDs. [Piste, jossa](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) on paikallinen vector – tiivis tai lyhyet, liittyvä otsikko ja vastaus.

Koodi Määritä target (riippuvaiset) muuttujan ja -toiminnot kouluttaminen mallien avulla. Sitten voit luoda indeksoiduista tai yksi kuuman koodattu koulutus ja testaus, jossa kohta RDDs tai tietojen kehykset syöte.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # MAP NAMES OF FEATURES AND TARGETS FOR CLASSIFICATION AND REGRESSION PROBLEMS
    val featuresIndOneHot = List("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))
    val featuresIndIndex = List("paymentIndex", "vendorIndex", "rateIndex", "TrafficTimeBinsIndex", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))

    # SPECIFY THE TARGET FOR CLASSIFICATION ('tipped') AND REGRESSION ('tip_amount') PROBLEMS
    val targetIndBinary = List("tipped").map(encodedFinalSampled.columns.indexOf(_))
    val targetIndRegression = List("tip_amount").map(encodedFinalSampled.columns.indexOf(_))

    # CREATE INDEXED LABELED POINT RDD OBJECTS
    val indexedTRAINbinary = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTbinary = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTRAINreg = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTreg = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))

    # CREATE INDEXED DATA FRAMES THAT YOU CAN USE TO TRAIN BY USING SPARK ML FUNCTIONS
    val indexedTRAINbinaryDF = indexedTRAINbinary.toDF()
    val indexedTESTbinaryDF = indexedTESTbinary.toDF()
    val indexedTRAINregDF = indexedTRAINreg.toDF()
    val indexedTESTregDF = indexedTESTreg.toDF()

    # CREATE ONE-HOT ENCODED (VECTORIZED) DATA FRAMES THAT YOU CAN USE TO TRAIN BY USING SPARK ML FUNCTIONS
    val assemblerOneHot = new VectorAssembler().setInputCols(Array("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount")).setOutputCol("features")
    val OneHotTRAIN = assemblerOneHot.transform(trainData)
    val OneHotTEST = assemblerOneHot.transform(testData)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Tulos:**

Solun aikaa: 4 sekuntia.


### <a name="automatically-categorize-and-vectorize-features-and-targets-to-use-as-inputs-for-machine-learning-models"></a>Luokittele ja vectorize ominaisuudet ja konepohjaisten oppimistekniikoiden mallien syötteiden käytettävän kohteet automaattisesti

Ohjattu ML avulla voit luokitella kohde ja toiminnot puun perustuva mallinnus-funktioiden käyttäminen. Koodin suorittaa kaksi tehtävää:

-   Luo luokitus binaarinen kohteen määrittämällä arvo on 0 tai 1 kunkin arvopisteen väliltä 0 – 1 raja-arvo 0,5 avulla.
- Luokittelee automaattisesti ominaisuuksia. Jos mikä tahansa ominaisuus eri numeeristen arvojen määrä on pienempi kuin 32-toiminto on luokiteltu.

Seuraavassa on nämä kaksi tehtävää koodi.

    # CATEGORIZE FEATURES AND BINARIZE THE TARGET FOR THE BINARY CLASSIFICATION PROBLEM

    # TRAIN DATA
    val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
    val indexerModel = indexer.fit(indexedTRAINbinaryDF)
    val indexedTrainwithCatFeat = indexerModel.transform(indexedTRAINbinaryDF)
    val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
    val indexedTRAINwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)

    # TEST DATA
    val indexerModel = indexer.fit(indexedTESTbinaryDF)
    val indexedTrainwithCatFeat = indexerModel.transform(indexedTESTbinaryDF)
    val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
    val indexedTESTwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)

    # CATEGORIZE FEATURES FOR THE REGRESSION PROBLEM
    # CREATE PROPERLY INDEXED AND CATEGORIZED DATA FRAMES FOR TREE-BASED MODELS

    # TRAIN DATA
    val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
    val indexerModel = indexer.fit(indexedTRAINregDF)
    val indexedTRAINwithCatFeat = indexerModel.transform(indexedTRAINregDF)

    # TEST DATA
    val indexerModel = indexer.fit(indexedTESTbinaryDF)
    val indexedTESTwithCatFeat = indexerModel.transform(indexedTESTregDF)



## <a name="binary-classification-model-predict-whether-a-tip-should-be-paid"></a>Binaarinen luokitus mallin: ennustaa onko Vihje maksettu

Tässä osassa voit luoda binaarinen luokitus mallien ennustaa riippumatta siitä, onko Vihje maksettu kolmenlaisia:

- **Logistista regressiota mallin** avulla ohjattu ML `LogisticRegression()` funktio
- **Satunnainen metsää luokitus mallin** avulla ohjattu ML `RandomForestClassifier()` funktio
- **Liukuvärin lisäyksen puun luokitus-mallin** avulla MLlib `GradientBoostedTrees()` funktio

### <a name="create-a-logistic-regression-model"></a>Logistista regressiota mallin luominen

Seuraavaksi logistista regressiota mallin luominen käyttämällä Ohjattu ML `LogisticRegression()` funktio. Voit luoda mallin luomisesta tietyt vaiheet-koodi:

1. **Junassa mallin** tietojasi yhden parametrin määrittäminen.
2. **Arvioi mallin** testi tietojen joukossa, jossa arvot.
3. **Tallenna malli** Blob-objektien tallennustilaan tulevat käyttöön.
4. Testitiedot vastaan **sija malli** .
5. **Piirrä tulokset** kanssa toimivien ominaisuus (OHJETIEDOSTO) kaaria vastaanottaja.

Näin nämä toimintosarjat koodi:

    # CREATE A LOGISTIC REGRESSION MODEL
    val lr = new LogisticRegression().setLabelCol("tipped").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)
    val lrModel = lr.fit(OneHotTRAIN)

    # PREDICT ON THE TEST DATA SET
    val predictions = lrModel.transform(OneHotTEST)

    # SELECT `BinaryClassificationEvaluator()` TO COMPUTE THE TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)

    # SAVE THE MODEL
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LogisticRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

Lataa, pisteet ja tulosten tallentaminen.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD THE SAVED MODEL AND SCORE THE TEST DATA SET
    val savedModel = org.apache.spark.ml.classification.LogisticRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE THE MODEL ON THE TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tipped","probability","rawPrediction")
    predictions.registerTempTable("testResults")

    # SELECT `BinaryClassificationEvaluator()` TO COMPUTE THE TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE ROC RESULTS
    println("ROC on test data = " + ROC)


**Tulos:**

Valitse testitiedot OHJETIEDOSTO = 0.9827381497557599


Paikallinen Pandas kehykset Python avulla voit piirtää OHJETIEDOSTO käyrän.


    # QUERY THE RESULTS
    %%sql -q -o sqlResults
    SELECT tipped, probability from testResults


    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc

    sqlResults['probFloat'] = sqlResults.apply(lambda row: row['probability'].values()[0][1], axis=1)
    predictions_pddf = sqlResults[["tipped","probFloat"]]

    # PREDICT THE ROC CURVE
    # predictions_pddf = sqlResults.rename(columns={'_1': 'probability', 'tipped': 'label'})
    prob = predictions_pddf["probFloat"]
    fpr, tpr, thresholds = roc_curve(predictions_pddf['tipped'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT THE ROC CURVE
    plt.figure(figsize=(5,5))
    plt.plot(fpr, tpr, label='ROC curve (area = %0.2f)' % roc_auc)
    plt.plot([0, 1], [0, 1], 'k--')
    plt.xlim([0.0, 1.0])
    plt.ylim([0.0, 1.05])
    plt.xlabel('False Positive Rate')
    plt.ylabel('True Positive Rate')
    plt.title('ROC Curve')
    plt.legend(loc="lower right")
    plt.show()


**Tulos:**

![Vihje tai ei ole Vihje OHJETIEDOSTO käyrä](./media/machine-learning-data-science-process-scala-walkthrough/plot-roc-curve-tip-or-not.png)


### <a name="create-a-random-forest-classification-model"></a>Satunnainen metsää luokitus mallin luominen

Seuraavaksi satunnaisia metsää luokitus mallin luominen käyttämällä Ohjattu ML `RandomForestClassifier()` toimi ja arvioi testitiedot malli.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE THE RANDOM FOREST CLASSIFIER MODEL
    val rf = new RandomForestClassifier().setLabelCol("labelBin").setFeaturesCol("featuresCat").setNumTrees(10).setSeed(1234)

    # FIT THE MODEL
    val rfModel = rf.fit(indexedTRAINwithCatFeatBinTarget)
    val predictions = rfModel.transform(indexedTESTwithCatFeatBinTarget)

    # EVALUATE THE MODEL
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(predictions)
    println("F1 score on test data: " + Test_f1Score);

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # CALCULATE BINARY CLASSIFICATION EVALUATION METRICS
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("label").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)


**Tulos:**

Valitse testitiedot OHJETIEDOSTO = 0.9847103571552683


### <a name="create-a-gbt-classification-model"></a>GBT luokitus mallin luominen

Seuraavaksi GBT luokitus mallin luominen käyttämällä MLlib's `GradientBoostedTrees()` toimi ja arvioi testitiedot malli.


    # TRAIN A GBT CLASSIFICATION MODEL BY USING MLLIB AND A LABELED POINT

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE GBT CLASSIFICATION MODEL
    val boostingStrategy = BoostingStrategy.defaultParams("Classification")
    boostingStrategy.numIterations = 20
    boostingStrategy.treeStrategy.numClasses = 2
    boostingStrategy.treeStrategy.maxDepth = 5
    boostingStrategy.treeStrategy.categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))

    # TRAIN THE MODEL
    val gbtModel = GradientBoostedTrees.train(indexedTRAINbinary, boostingStrategy)

    # SAVE THE MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "GBT_Classification__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    gbtModel.save(sc, filename);

    # EVALUATE THE MODEL ON TEST INSTANCES AND THE COMPUTE TEST ERROR
    val labelAndPreds = indexedTESTbinary.map { point =>
      val prediction = gbtModel.predict(point.features)
      (point.label, prediction)
    }
    val testErr = labelAndPreds.filter(r => r._1 != r._2).count.toDouble / indexedTRAINbinary.count()
    //println("Learned classification GBT model:\n" + gbtModel.toDebugString)
    println("Test Error = " + testErr)

    # USE BINARY AND MULTICLASS METRICS TO EVALUATE THE MODEL ON THE TEST DATA
    val metrics = new MulticlassMetrics(labelAndPreds)
    println(s"Precision: ${metrics.precision}")
    println(s"Recall: ${metrics.recall}")
    println(s"F1 Score: ${metrics.fMeasure}")

    val metrics = new BinaryClassificationMetrics(labelAndPreds)
    println(s"Area under PR curve: ${metrics.areaUnderPR}")
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE ROC METRIC
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")


**Tulos:**

OHJETIEDOSTO käyrän pinta: 0.9846895479241554


## <a name="regression-model-predict-tip-amount"></a>Regressiosuoran mallin: ennustaa Vihje summa

Tässä osassa voit luoda kahdentyyppisiä regression mallien ennustaa Vihje määrä:

- **Haltijan lineaarisen regressiosuoran mallin** avulla ohjattu ML `LinearRegression()` funktio. Säästää malli ja valitse testitiedot mallin arvioiminen.
- **Liukuvärin kehittämällä puun regression mallin** avulla ohjattu ML `GBTRegressor()` funktio.


### <a name="create-a-regularized-linear-regression-model"></a>Haltijan lineaarisen regressiosuoran mallin luominen

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE A REGULARIZED LINEAR REGRESSION MODEL BY USING THE SPARK ML FUNCTION AND DATA FRAMES
    val lr = new LinearRegression().setLabelCol("tip_amount").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)

    # FIT THE MODEL BY USING DATA FRAMES
    val lrModel = lr.fit(OneHotTRAIN)
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SUMMARIZE THE MODEL OVER THE TRAINING SET AND PRINT METRICS
    val trainingSummary = lrModel.summary
    println(s"numIterations: ${trainingSummary.totalIterations}")
    println(s"objectiveHistory: ${trainingSummary.objectiveHistory.toList}")
    trainingSummary.residuals.show()
    println(s"RMSE: ${trainingSummary.rootMeanSquaredError}")
    println(s"r2: ${trainingSummary.r2}")

    # SAVE THE MODEL IN AZURE BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LinearRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

    # PRINT THE COEFFICIENTS
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SCORE THE MODEL ON TEST DATA
    val predictions = lrModel.transform(OneHotTEST)

    # EVALUATE THE MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Tulos:**

Solun aikaa: 13 sekuntia.


    # LOAD A SAVED LINEAR REGRESSION MODEL FROM BLOB STORAGE AND SCORE A TEST DATA SET

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD A SAVED LINEAR REGRESSION MODEL FROM AZURE BLOB STORAGE
    val savedModel = org.apache.spark.ml.regression.LinearRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE THE MODEL ON TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tip_amount","prediction")
    predictions.registerTempTable("testResults")

    # EVALUATE THE MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE RESULTS
    println("R-sqr on test data = " + r2)


**Tulos:**

Testitiedot, R-sqr = 0.5960320470835743


Seuraavaksi kyselyn tulokset kuvan käyttäminen tietojen ja esittää sen AutoVizWidget ja matplotlib avulla.


    # RUN A SQL QUERY
    %%sql -q -o sqlResults
    select * from testResults

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES
    # CLICK THE TYPE OF PLOT TO GENERATE (LINE, AREA, BAR, AND SO ON)
    sqlResults

Koodin Luo paikalliset tiedot kehyksen kyselyn tuloksesta ja esittää tietoja. `%%local` Tärkeä Luo paikalliset tiedot kehys `sqlResults`, jonka avulla voit piirtää matplotlib kanssa.

>[AZURE.NOTE] Tämä ohjattu tärkeä käytetään useita kertoja tässä artikkelissa. Jos tietojen määrää on suuri, on esimerkki kehykseksi tiedot, jotka mahtuu luominen paikalliseen muistiin.

Luo joudutaan Python matplotlib avulla.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    sqlResults
    %matplotlib inline
    import numpy as np

    # PLOT THE RESULTS
    ax = sqlResults.plot(kind='scatter', figsize = (6,6), x='tip_amount', y='prediction', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(sqlResults['tip_amount'], sqlResults['prediction'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    #ax.plot(sqlResults['tip_amount'], fit[0] * sqlResults['prediction'] + fit[1], color='magenta')
    plt.axis([-1, 15, -1, 8])
    plt.show(ax)

**Tulos:**

![Vihje summa: todelliset ja budjetoidut](./media/machine-learning-data-science-process-scala-walkthrough/plot-actual-vs-predicted-tip-amount.png)


### <a name="create-a-gbt-regression-model"></a>GBT regression mallin luominen

GBT regression mallin luominen käyttämällä Ohjattu ML `GBTRegressor()` toimi ja arvioi testitiedot malli.

[Liukuvärin tehosti puita](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) ovat yhdistelmäasut päätös puut. GBTs kouluttaminen päätös puut muutosten pienentämiseksi tappio-funktiota. Voit käyttää GBTs regression ja luokitus. Ne voivat käsitellä categorical ominaisuuksia, eivät edellytä ominaisuus skaalaus ja voit siepata nonlinearities ja ominaisuus vuorovaikutukset. Voit myös käyttää niitä multiclass luokitus-asetusta.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # TRAIN A GBT REGRESSION MODEL
    val gbt = new GBTRegressor().setLabelCol("label").setFeaturesCol("featuresCat").setMaxIter(10)
    val gbtModel = gbt.fit(indexedTRAINwithCatFeat)

    # MAKE PREDICTIONS
    val predictions = gbtModel.transform(indexedTESTwithCatFeat)

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(predictions)


    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE RESULTS
    println("Test R-sqr is: " + Test_R2);


**Tulos:**

Testaa R-sqr on: 0.7655383534596654



## <a name="advanced-modeling-utilities-for-optimization"></a>Laajennettu mallinnus apuohjelmien optimointi

Tässä osassa voit käyttää tietokoneen learning apuohjelmat, jotka kehittäjät usein käyttää mallin optimointi. Tarkemmin sanottuna voi optimoida tietokoneen learning mallien kolmella eri tavalla käyttämällä parametrin sweeping ja rajat vahvistus:

-   Tietojen jakaminen junassa ja kelpoisuuden, optimointi mallin käyttämällä hyper-parametrin sweeping koulutus määrittäminen ja arvioida vahvistus tietylle (lineaarisen regressiosuoran)
-   Optimoi mallin avulla rajat kelpoisuustarkistus ja hyper-parametri sweeping käyttämällä Ohjattu ML CrossValidator-funktiota (binaarinen luokitus)
-   Optimoi mallin avulla mukautetun rajat-kelpoisuustarkistus ja parametrin sweeping koodin käyttää minkä tahansa konepohjaisten oppimistekniikoiden-funktio ja parametrin määrittäminen (lineaarisen regressiosuoran)


**Rajat vahvistus** on tekniikka, jonka sopimuksen merkityksellisiin kuinka hyvin koulutus-tunnetut tietojoukolle mallin generalize ennustaa tietojoukot, se ei ole harjoitettu ominaisuuksia. Yleiset verrata tätä tapaa takana on, että malli on koulutus-tietojoukon tunnetut tietojen ja sitten riippumaton tietojoukon verrataan sen ennusteiden tarkkuutta. Yleisiä käyttöönoton voidaan jakaa tietojoukon *k*-taitosten tekeminen ja kouluttaminen mallin pyöreän pöydän tavalla kaikissa yhtä taitosten tekeminen lukuun ottamatta.

**Hyper-parametrin optimointi** on ongelma valitsemalla learning-algoritmin hyper-parametrit joukko yleensä yhteydessä optimointi algoritmin suorituskyvyn riippumaton tietojoukon mitta. Hyper-parametri on arvo, joka on määritettävä mallin koulutus-toimintosarjan ulkopuolelta. Oletuksia hyper parametrien arvot voivat vaikuttaa joustavuutta ja mallin tarkkuudella. Päätös puut on hyper-parametrit, esimerkiksi esimerkiksi haluamasi syvyyttä ja puun lehtiä määrä. Sinun on määritettävä luokitteluvirhe seuraamusta termin tuki vector tietokoneelle (SVM).

Yleisiä voi suorittaa hyper-parametrin optimointi on käytettävä ruudukon haun, kutsutaan myös **parametrin Puhdista**. Ruudukon hakutoiminnossa monipuolisen haku tehdään learning algoritmin hyper-parametrin tilaa määritettyyn alijoukkoa arvot. Usean kelpoisuustarkistus voi toimittaa suorituskyvyn mittarin lajittelemiseen ulos optimaalisen ruudukon haun algoritmin tuottamat tulokset. Jos käytät rajat vahvistus hyper-parametrin sweeping, auttaa raja-ongelmien, kuten overfitting mallin koulutus tietoihin. Tällä tavalla mallin säilyttää resursseja koskevat yleiset tietojoukolle, joista koulutus tiedot on purettu.

### <a name="optimize-a-linear-regression-model-with-hyper-parameter-sweeping"></a>Lineaarisen regressiosuoran malli, joka sisältää hyper-parametrin sweeping optimointi

Jaa seuraavaksi tietojen junassa ja kelpoisuuden joukot, käytä hyper-parametrin sweeping koulutus tietylle optimointi mallin ja arvioida vahvistus tietylle (lineaarisen regressiosuoran).

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # RENAME `tip_amount` AS A LABEL
    val OneHotTRAINLabeled = OneHotTRAIN.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    val OneHotTESTLabeled = OneHotTEST.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    OneHotTRAINLabeled.cache()
    OneHotTESTLabeled.cache()

    # DEFINE THE ESTIMATOR FUNCTION: `THE LinearRegression()` FUNCTION
    val lr = new LinearRegression().setLabelCol("label").setFeaturesCol("features").setMaxIter(10)

    # DEFINE THE PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(lr.regParam, Array(0.1, 0.01, 0.001)).addGrid(lr.fitIntercept).addGrid(lr.elasticNetParam, Array(0.1, 0.5, 0.9)).build()

    # DEFINE THE PIPELINE WITH A TRAIN/TEST VALIDATION SPLIT (75% IN THE TRAINING SET), AND THEN THE SPECIFY ESTIMATOR, EVALUATOR, AND PARAMETER GRID
    val trainPct = 0.75
    val trainValidationSplit = new TrainValidationSplit().setEstimator(lr).setEvaluator(new RegressionEvaluator).setEstimatorParamMaps(paramGrid).setTrainRatio(trainPct)

    # RUN THE TRAIN VALIDATION SPLIT AND CHOOSE THE BEST SET OF PARAMETERS
    val model = trainValidationSplit.fit(OneHotTRAINLabeled)

    # MAKE PREDICTIONS ON THE TEST DATA BY USING THE MODEL WITH THE COMBINATION OF PARAMETERS THAT PERFORMS THE BEST
    val testResults = model.transform(OneHotTESTLabeled).select("label", "prediction")

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(testResults)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    println("Test R-sqr is: " + Test_R2);


**Tulos:**

Testaa R-sqr on: 0.6226484708501209


### <a name="optimize-the-binary-classification-model-by-using-cross-validation-and-hyper-parameter-sweeping"></a>Optimoi binaarinen luokitus-mallin avulla rajat kelpoisuustarkistus ja hyper-parametrin sweeping

Tässä osiossa kerrotaan optimoiminen binaarinen luokitus-mallin avulla rajat kelpoisuustarkistus ja hyper-parametrin sweeping. Tämä käyttää Ohjattu ML `CrossValidator` funktio.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE DATA FRAMES WITH PROPERLY LABELED COLUMNS TO USE WITH THE TRAIN AND TEST SPLIT
    val indexedTRAINwithCatFeatBinTargetRF = indexedTRAINwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    val indexedTESTwithCatFeatBinTargetRF = indexedTESTwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    indexedTRAINwithCatFeatBinTargetRF.cache()
    indexedTESTwithCatFeatBinTargetRF.cache()

    # DEFINE THE ESTIMATOR FUNCTION
    val rf = new RandomForestClassifier().setLabelCol("label").setFeaturesCol("features").setImpurity("gini").setSeed(1234).setFeatureSubsetStrategy("auto").setMaxBins(32)

    # DEFINE THE PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(4,8)).addGrid(rf.numTrees, Array(5,10)).addGrid(rf.minInstancesPerNode, Array(100,300)).build()

    # SPECIFY THE NUMBER OF FOLDS
    val numFolds = 3

    # DEFINE THE TRAIN/TEST VALIDATION SPLIT (75% IN THE TRAINING SET)
    val CrossValidator = new CrossValidator().setEstimator(rf).setEvaluator(new BinaryClassificationEvaluator).setEstimatorParamMaps(paramGrid).setNumFolds(numFolds)

    # RUN THE TRAIN VALIDATION SPLIT AND CHOOSE THE BEST SET OF PARAMETERS
    val model = CrossValidator.fit(indexedTRAINwithCatFeatBinTargetRF)

    # MAKE PREDICTIONS ON THE TEST DATA BY USING THE MODEL WITH THE COMBINATION OF PARAMETERS THAT PERFORMS THE BEST
    val testResults = model.transform(indexedTESTwithCatFeatBinTargetRF).select("label", "prediction")

    # COMPUTE THE TEST F1 SCORE
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(testResults)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Tulos:**

Solun aikaa: 33 sekuntia.


### <a name="optimize-the-linear-regression-model-by-using-custom-cross-validation-and-parameter-sweeping-code"></a>Optimoi lineaarisen regressiosuoran mallin mukautettujen rajat-kelpoisuustarkistus ja parametrin sweeping-koodin avulla

Optimoi mallin käyttäen mukautettua koodia ja tunnistaa parhaat mallin parametrit suurin tarkkuutta ehdon avulla. Luo sitten lopullinen mallin, valitse testitiedot mallin arvioiminen ja malli tallennetaan Blob-objektien tallennustilaan. Lopuksi ladata mallin, pisteet testitiedot ja arvioida tarkkuutta.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE PARAMETER GRID AND THE NUMBER OF FOLDS
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(5,10)).addGrid(rf.numTrees, Array(10,25,50)).build()

    val nFolds = 3
    val numModels = paramGrid.size
    val numParamsinGrid = 2

    # SPECIFY THE NUMBER OF CATEGORIES FOR CATEGORICAL VARIABLES
    val categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))

    var maxDepth = -1
    var numTrees = -1
    var param = ""
    var paramval = -1
    var validateLB = -1.0
    var validateUB = -1.0
    val h = 1.0 / nFolds;
    val RMSE  = Array.fill(numModels)(0.0)

    # CREATE K-FOLDS
    val splits = MLUtils.kFold(indexedTRAINbinary, numFolds = nFolds, seed=1234)


    # LOOP THROUGH K-FOLDS AND THE PARAMETER GRID TO GET AND IDENTIFY THE BEST PARAMETER SET BY LEVEL OF ACCURACY
    for (i <- 0 to (nFolds-1)) {
        validateLB = i * h
        validateUB = (i + 1) * h
        val validationCV = trainData.filter($"rand" >= validateLB  && $"rand" < validateUB)
        val trainCV = trainData.filter($"rand" < validateLB  || $"rand" >= validateUB)
        val validationLabPt = validationCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        val trainCVLabPt = trainCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        validationLabPt.cache()
        trainCVLabPt.cache()

        for (nParamSets <- 0 to (numModels-1)) {
            for (nParams <- 0 to (numParamsinGrid-1)) {
                param = paramGrid(nParamSets).toSeq(nParams).param.toString.split("__")(1)
                paramval = paramGrid(nParamSets).toSeq(nParams).value.toString.toInt
                if (param == "maxDepth") {maxDepth = paramval}
                if (param == "numTrees") {numTrees = paramval}
            }
            val rfModel = RandomForest.trainRegressor(trainCVLabPt, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                      numTrees=numTrees, maxDepth=maxDepth,
                                                      featureSubsetStrategy="auto",impurity="variance", maxBins=32)
            val labelAndPreds = validationLabPt.map { point =>
                                                     val prediction = rfModel.predict(point.features)
                                                     ( prediction, point.label )
                                                    }
            val validMetrics = new RegressionMetrics(labelAndPreds)
            val rmse = validMetrics.rootMeanSquaredError
            RMSE(nParamSets) += rmse
        }
        validationLabPt.unpersist();
        trainCVLabPt.unpersist();
    }
    val minRMSEindex = RMSE.indexOf(RMSE.min)

    # GET THE BEST PARAMETERS FROM A CROSS-VALIDATION AND PARAMETER SWEEP
    var best_maxDepth = -1
    var best_numTrees = -1
    for (nParams <- 0 to (numParamsinGrid-1)) {
        param = paramGrid(minRMSEindex).toSeq(nParams).param.toString.split("__")(1)
        paramval = paramGrid(minRMSEindex).toSeq(nParams).value.toString.toInt
        if (param == "maxDepth") {best_maxDepth = paramval}
        if (param == "numTrees") {best_numTrees = paramval}
    }

    # CREATE THE BEST MODEL WITH THE BEST PARAMETERS AND A FULL TRAINING DATA SET
    val best_rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                      numTrees=best_numTrees, maxDepth=best_maxDepth,
                                                      featureSubsetStrategy="auto",impurity="variance", maxBins=32)

    # SAVE THE BEST RANDOM FOREST MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "BestCV_RF_Regression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    best_rfModel.save(sc, filename);

    # PREDICT ON THE TRAINING SET WITH THE BEST MODEL AND THEN EVALUATE
    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = best_rfModel.predict(point.features)
                                            ( prediction, point.label )
                                           }

    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


    # LOAD THE MODEL
    val savedRFModel = RandomForestModel.load(sc, filename)

    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = savedRFModel.predict(point.features)
                                            ( prediction, point.label )
                                           }
    # TEST THE MODEL
    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2


**Tulos:**

Solun aikaa: 61 sekuntia.

## <a name="consume-spark-built-machine-learning-models-automatically-with-scala"></a>Ohjattu laadittuihin koneen learning malleja automaattisesti Scala tarjoaman

Katso yleiskuvaus aiheisiin, joissa opastusta tehtävät, jotka muodostavat Azure tietojen tiede-prosessi- [Ryhmän tietojen tiede prosessi](http://aka.ms/datascienceprocess).

[Ryhmän tietojen tiede prosessin vaiheittaisissa ohjeissa](data-science-process-walkthroughs.md) kerrotaan muiden lopusta loppuun-vaihe vaiheelta, jotka osoittavat tietyissä skenaarioissa ryhmän tietojen tiede-vaiheet. Vaiheittaiset ohjeet osoittavat myös voit yhdistää cloud ja paikallisen työkalujen ja palvelujen työnkulun tai myyntijakso älykkäät-sovelluksen luominen.

[Tulos ohjattu laadittuihin konepohjaisten oppimistekniikoiden mallien](machine-learning-data-science-spark-model-consumption.md) avulla voit ladata ja sija uudet tietojoukot kanssa koneen learning mallit: n ohjattu valmista ja Azure-Blob-säiliö tallennetaan automaattisesti Scala-koodin avulla. Voit noudattamalla näyttöön tulevia ohjeita siellä ja korvaa yksinkertaisesti Scala koodilla automaattinen kulutus tämän artikkelin Python koodi.
