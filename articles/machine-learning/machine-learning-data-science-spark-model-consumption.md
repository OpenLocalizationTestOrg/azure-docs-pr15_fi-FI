<properties
    pageTitle="Ohjattu laadittuihin koneen learning mallien sija | Microsoft Azure"
    description="Miten pistemäärän learning mallit, jotka on tallennettu Azure Blob Storage (WASB)."
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
    ms.date="10/07/2016"
    ms.author="deguhath;bradsev;gokuma" />

# <a name="score-spark-built-machine-learning-models"></a>Ohjattu laadittuihin koneen learning mallien pisteet 

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

Tässä ohjeaiheessa kerrotaan, miten voit ladata tietokoneen Koulujen (ML) malleja, jotka ohjattu MLlib avulla luotu ja tallennettu Azure Blob Storage (WASB) ja niiden sija ne, jotka on myös tallennettu WASB tietojoukkoja kanssa. Se näyttää kuinka esikäsittely syöttötiedot, muunna indeksoinnin ja koodaus-funktioiden käyttämisestä MLlib työkalujen ominaisuuksia ja nimetty pisteen tieto-objekti, jonka avulla voidaan syötteeksi näkyvissä pistemäärä kanssa ML-mallien luomisesta. Näkyvissä pistemäärä käytettävät mallit ovat lineaarisen regressiosuoran, logistista regressiota, satunnainen metsää mallit ja liukuvärin kehittämällä puun mallit.


## <a name="prerequisites"></a>Edellytykset

1. Tarvitset Azure-tili ja luodun HDInsight ohjattu voit tarpeen HDInsight 3.4 ohjattu-1.6 klusterin vaiheiden suorittamiseen. Katso ohjeet [Yleiskatsaus tietojen tiede käyttämällä Ohjattu Azure HDInsight-](machine-learning-data-science-spark-overview.md) täytä näitä vaatimuksia. Ohjeaiheen sisältää myös käytetyt tähän Kokousesitelmän 2013 taksin tiedot ja ohjeita siitä, miten voit suorittaa koodin Ohjattu klusterin Jupyter muistikirjan kuvaus. **PySpark-machine-learning-data-science-spark-model-consumption.ipynb** muistikirjan, joka sisältää tässä ohjeaiheessa MALLIKOODEJA sisältyy [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark).

2. Sinun on luotava konepohjaisten oppimistekniikoiden mallien tulos tähän suorittamalla [tietojen tarkasteluun ja arvioiminen ohjattu](machine-learning-data-science-spark-data-exploration-modeling.md) aiheen kautta.   


[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]
 

## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a>Määritys: tallennuspaikkojen, kirjastojen ja valmiiksi määritettyä ohjattu yhteydessä

Ohjattu pystyy lukemiseen ja kirjoittamiseen, Azure tallennustilan Blob (WASB). Jotta kaikki tiedot tallennetut siellä voidaan käsitellä ohjattu ja tallennettu uudelleen WASB tulokset.

Jos haluat tallentaa WASB mallien tai tiedostot, polku on määritetty oikein. Ohjattu klusterin liitetty oletusarvo-säilö voidaan viitata käyttämällä polku alkaen: *"wasb / /"*. Seuraava koodi esimerkki määrittää sijainnin tiedot voi lukea ja mallin tallennustilan kansio, johon mallin tulos on tallennettu polun. 


### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>Määrittää hakemiston polut tallennuspaikkojen WASB

Mallit tallennetaan: "wasb: / / / käyttäjä/remoteuser/NYCTaxi/mallit". Jos tämä polku ei ole määritetty oikein, malleja ei ladata, näkyvissä pistemäärä.

Scored tulokset on tallennettu: "wasb: / / / käyttäjä/remoteuser/NYCTaxi/ScoredResults". Jos kansion polku on virheellinen, tulokset eivät tallennu kyseiseen kansioon.   


>[AZURE.NOTE] Oletuskansiot polku voi kopioida ja liittää paikkamerkit koodi tulosteesta **machine-learning-data-science-spark-data-exploration-modeling.ipynb** muistikirjan viimeistä solua.   


Näin voit määrittää hakemiston polut koodi: 

    # LOCATION OF DATA TO BE SCORED (TEST DATA)
    taxi_test_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Test.tsv";
    
    # SET THE MODEL STORAGE DIRECTORY PATH 
    # NOTE THE LAST BACKSLASH IN THIS PATH IS NEEDED
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/" 
    
    # SET SCORDED RESULT DIRECTORY PATH
    # NOTE THE LAST BACKSLASH IN THIS PATH IS NEEDED
    scoredResultDir = "wasb:///user/remoteuser/NYCTaxi/ScoredResults/"; 
    
    # FILE LOCATIONS FOR THE MODELS TO BE SCORED
    logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-04-1817_40_35.796789"
    linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-04-1817_44_00.993832"
    randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-04-1817_42_58.899412"
    randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-04-1817_44_27.204734"
    BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-04-1817_43_16.354770"
    BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-04-1817_44_46.206262"

    # RECORD START TIME
    import datetime
    datetime.datetime.now()

**TULOS:**

DateTime.DATETIME (2016, 4, 25, 23, 56, 19, 229403)


### <a name="import-libraries"></a>Tuo-kirjastot

Aseta ohjattu konteksti ja tuoda tarvittavat kirjastojen seuraava koodi

    #IMPORT LIBRARIES
    import pyspark
    from pyspark import SparkConf
    from pyspark import SparkContext
    from pyspark.sql import SQLContext
    import matplotlib
    import matplotlib.pyplot as plt
    from pyspark.sql import Row
    from pyspark.sql.functions import UserDefinedFunction
    from pyspark.sql.types import *
    import atexit
    from numpy import array
    import numpy as np
    import datetime


### <a name="preset-spark-context-and-pyspark-magics"></a>Valmiiksi ohjattu konteksti- ja PySpark magics

Jupyter muistikirjat mukana toimitettuja PySpark ytimet on valmiiksi määritettyä kontekstissa. Sinun ei tarvitse määrittää ohjattu tai rakenteen kontekstit erikseen ennen kuin aloitat käsitteleminen sovelluksen kehittävät siten. Nämä ovat käytettävissä oletusarvoisesti. Nämä kontekstit ovat seuraavat:

- SC - ohjattu varten 
- sqlContext - rakennetta varten

PySpark ydin tarjoaa esimääritettyjä "magics", jotka ovat erityisiä komennot, jotka voivat soittaa kanssa %%. On kaksi tällaisten komennot, joita käytetään seuraavia MALLIKOODEJA.

- **%% paikallisen** Määrittää, että seuraaville riveille koodi suoritetaan paikallisesti. Koodi on oltava kelvollinen Python koodi.
- **%% sql -o<variable name>** 
- Suorittaa sqlContext rakenteen kysely. Jos + o-parametri on kulunut, kyselyn tulos on samanlainen %% paikallisen Python yhteydessä kuin Pandas dataframe.
 

Saat lisätietoja Jupyter muistikirjat ja valmiin ytimet "magics", niiden avulla, katso [ytimet käytettävissä olevien HDInsight HDInsight ohjattu Linux varausyksiköiden muistikirjojen Jupyter](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).


## <a name="ingest-data-and-create-a-cleaned-data-frame"></a>Tietoja ingest ja puhdistettava tietojen kehyksen luominen

Tässä osassa on koodin sarjalle vaadittavat tiedot annettava tulos ingest tehtävät. Lukea liitettyjen 0,1 otoksen taksin työmatkan ja maksun (tallennetun tiedoston .tsv-tiedostona)-muodossa tiedot ja luo sitten Puhdista tietojen kehykseksi.

Taksin työmatkan ja maksun tiedostoja on liitettyinä perustuvat määrätyllä tavalla: [ryhmän-tiede prosessin käytössä: käyttämällä HDInsight Hadoop klustereiden](machine-learning-data-science-process-hive-walkthrough.md) aihe.

    # INGEST DATA AND CREATE A CLEANED DATA FRAME

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # IMPORT FILE FROM PUBLIC BLOB
    taxi_test_file = sc.textFile(taxi_test_file_loc)
    
    # GET SCHEMA OF THE FILE FROM HEADER
    taxi_header = taxi_test_file.filter(lambda l: "medallion" in l)
    
    # PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
    taxi_temp = taxi_test_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
            .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
                            float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
                            float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))
        
    # GET SCHEMA OF THE FILE FROM HEADER
    schema_string = taxi_test_file.first()
    fields = [StructField(field_name, StringType(), True) for field_name in schema_string.split('\t')]
    fields[7].dataType = IntegerType() #Pickup hour
    fields[8].dataType = IntegerType() # Pickup week
    fields[9].dataType = IntegerType() # Weekday
    fields[10].dataType = IntegerType() # Passenger count
    fields[11].dataType = FloatType() # Trip time in secs
    fields[12].dataType = FloatType() # Trip distance
    fields[19].dataType = FloatType() # Fare amount
    fields[20].dataType = FloatType() # Surcharge
    fields[21].dataType = FloatType() # Mta_tax
    fields[22].dataType = FloatType() # Tip amount
    fields[23].dataType = FloatType() # Tolls amount
    fields[24].dataType = FloatType() # Total amount
    fields[25].dataType = IntegerType() # Tipped or not
    fields[26].dataType = IntegerType() # Tip class
    taxi_schema = StructType(fields)
    
    # CREATE DATA FRAME
    taxi_df_test = sqlContext.createDataFrame(taxi_temp, taxi_schema)
    
    # CREATE A CLEANED DATA-FRAME BY DROPPING SOME UN-NECESSARY COLUMNS & FILTERING FOR UNDESIRED VALUES OR OUTLIERS
    taxi_df_test_cleaned = taxi_df_test.drop('medallion').drop('hack_license').drop('store_and_fwd_flag').drop('pickup_datetime')\
        .drop('dropoff_datetime').drop('pickup_longitude').drop('pickup_latitude').drop('dropoff_latitude')\
        .drop('dropoff_longitude').drop('tip_class').drop('total_amount').drop('tolls_amount').drop('mta_tax')\
        .drop('direct_distance').drop('surcharge')\
        .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200" )
    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_test_cleaned.cache()
    taxi_df_test_cleaned.count()
    
    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_test_cleaned.registerTempTable("taxi_test")
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**TULOS:**

Solun yläpuolella olevan suorittamiseen aika: 46.37 sekuntia


## <a name="prepare-data-for-scoring-in-spark"></a>Valitse ohjattu näkyvissä pistemäärä tietojen valmisteleminen 

Tässä osassa esitellään indeksoida, koodata ja skaalata valmisteleminen valvottava MLlib learning algoritmit käyttää luokitukseen ja regression categorical ominaisuuksia.

### <a name="feature-transformation-index-and-encode-categorical-features-for-input-into-models-for-scoring"></a>Muunnoksen ominaisuus: indeksoida ja koodata näkyvissä pistemäärä mallit-syötteestä categorical ominaisuudet 

Tässä osassa esitetään, kuinka voit indeksoida categorical tietojen `StringIndexer` ja koodata ominaisuuksien `OneHotEncoder` input malleihin.

[StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) käyttää merkkijonon sarakkeen otsikoiden sarakkeen otsikon indeksien. Indeksien järjestetään mukaan otsikko tiheydet. 

[OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) yhdistää sarakkeen otsikon indeksien binaarinen vektorit arvolla enintään yhden yksi-yhteen sarakkeeseen. Algoritmeista, odottaa jatkuva tärkeät ominaisuudet, kuten logistista regressiota categorical ominaisuuksia käytettäväksi koodausta avulla.
    
    #INDEX AND ONE-HOT ENCODE CATEGORICAL FEATURES

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, VectorIndexer
    
    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night" 
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush" 
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_test 
    """
    taxi_df_test_with_newFeatures = sqlContext.sql(sqlStatement)
    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_test_with_newFeatures.cache()
    taxi_df_test_with_newFeatures.count()
    
    # INDEX AND ONE-HOT ENCODING
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_test_with_newFeatures) # Input data-frame is the cleaned one from above
    indexed = model.transform(taxi_df_test_with_newFeatures)
    encoder = OneHotEncoder(dropLast=False, inputCol="vendorIndex", outputCol="vendorVec")
    encoded1 = encoder.transform(indexed)
    
    # INDEX AND ENCODE RATE_CODE
    stringIndexer = StringIndexer(inputCol="rate_code", outputCol="rateIndex")
    model = stringIndexer.fit(encoded1)
    indexed = model.transform(encoded1)
    encoder = OneHotEncoder(dropLast=False, inputCol="rateIndex", outputCol="rateVec")
    encoded2 = encoder.transform(indexed)
    
    # INDEX AND ENCODE PAYMENT_TYPE
    stringIndexer = StringIndexer(inputCol="payment_type", outputCol="paymentIndex")
    model = stringIndexer.fit(encoded2)
    indexed = model.transform(encoded2)
    encoder = OneHotEncoder(dropLast=False, inputCol="paymentIndex", outputCol="paymentVec")
    encoded3 = encoder.transform(indexed)
    
    # INDEX AND ENCODE TRAFFIC TIME BINS
    stringIndexer = StringIndexer(inputCol="TrafficTimeBins", outputCol="TrafficTimeBinsIndex")
    model = stringIndexer.fit(encoded3)
    indexed = model.transform(encoded3)
    encoder = OneHotEncoder(dropLast=False, inputCol="TrafficTimeBinsIndex", outputCol="TrafficTimeBinsVec")
    encodedFinal = encoder.transform(indexed)
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**TULOS:**

Solun yläpuolella olevan suorittamiseen aika: 5.37 sekuntia


### <a name="create-rdd-objects-with-feature-arrays-for-input-into-models"></a>Luo RDD objektit ominaisuus matriisien syötteen malleihin kanssa

Tässä osassa on koodi, joka näyttää, miten yhden kuuman koodata se, jotta se voidaan testata MLlib logistista regressiota ja puun perustuva mallit ja kouluttaminen ja indeksoi categorical tekstitietoja RDD objektina. Indeksoidut tiedot tallennetaan [Joustavat Distributed tietojoukko (RDD)](http://spark.apache.org/docs/latest/api/java/org/apache/spark/rdd/RDD.html) -objekteja. Nämä ovat basic otetaan-ohjattu. Objektin RDD edustaa pysyvä, osioitu sivustokokoelman elementtejä, jotka voidaan käyttää Ohjattu kanssa samanaikaisesti.

Se sisältää myös koodi, joka näyttää, miten haluat skaalata tiedot ja `StandardScalar` myöntämä MLlib lineaarisen regressiosuoran kanssa Stochastic liukuvärin laskeutumisnopeus (SGD), Suositut algoritmin, koulutus koneen learning malleja monenlaisille käytettäviksi. [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) käytetään skaalata yksikön varianssin ominaisuuksia. Skaalaus ominaisuutta kutsutaan myös tietojen normalisointi vakuutusyhtiö, että ominaisuuksien laajasti maksanut arvot ovat ole antanut liiallinen arvioida objektiivisin-funktiossa. 


    # CREATE RDD OBJECTS WITH FEATURE ARRAYS FOR INPUT INTO MODELS

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT LIBRARIES
    from pyspark.mllib.linalg import Vectors
    from pyspark.mllib.feature import StandardScaler, StandardScalerModel
    from pyspark.mllib.util import MLUtils
    from numpy import array
    
    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex,
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        return  features
    
    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        return  features
    
    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingRegression(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex, 
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        return  features
    
    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO LINEAR REGRESSION MODELS
    def parseRowOneHotRegression(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        return  features

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTESTbinary = encodedFinal.map(parseRowIndexingBinary)
    oneHotTESTbinary = encodedFinal.map(parseRowOneHotBinary)
    
    # FOR REGRESSION CLASSIFICATION TRAINING AND TESTING
    indexedTESTreg = encodedFinal.map(parseRowIndexingRegression)
    oneHotTESTreg = encodedFinal.map(parseRowOneHotRegression)
    
    # SCALING FEATURES FOR LINEARREGRESSIONWITHSGD MODEL
    scaler = StandardScaler(withMean=False, withStd=True).fit(oneHotTESTreg)
    oneHotTESTregScaled = scaler.transform(oneHotTESTreg)
    
    # CACHE RDDS IN MEMORY
    indexedTESTbinary.cache();
    oneHotTESTbinary.cache();
    indexedTESTreg.cache();
    oneHotTESTreg.cache();
    oneHotTESTregScaled.cache();
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**TULOS:**

Solun yläpuolella olevan suorittamiseen aika: 11.72 sekuntia


## <a name="score-with-the-logistic-regression-model-and-save-output-to-blob"></a>Pisteet logistista regressiota malli ja Tallenna tulosteen blob

Koodin tässä osassa näytetään, miten lataamisen logistista Regression mallin, joka on tallennettu Azure-blob-säiliö ja ennustetaan riippumatta siitä, onko Vihje maksetaan taksin matkan, pisteet kanssa vakio luokituksen arvot, ja Tallenna ja piirtää Blob-objektien tallennustilaan tulokset. Scored tulokset tallennetaan RDD objekteja. 


    # SCORE AND EVALUATE LOGISTIC REGRESSION MODEL

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # IMPORT LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionModel
    
    ## LOAD SAVED MODEL
    savedModel = LogisticRegressionModel.load(sc, logisticRegFileLoc)
    predictions = oneHotTESTbinary.map(lambda features: (float(savedModel.predict(features))))
    
    ## SAVE SCORED RESULTS (RDD) TO BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp + ".txt";
    dirfilename = scoredResultDir + logisticregressionfilename;
    predictions.saveAsTextFile(dirfilename)
    
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**TULOS:**

Solun yläpuolella olevan suorittamiseen aika: 19.22 sekuntia


## <a name="score-a-linear-regression-model"></a>Lineaarisen regressiosuoran mallin pisteet

[LinearRegressionWithSGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) avulla kouluttaminen lineaarisen regressiosuoran mallin avulla Stochastic liukuvärin laskeutumisnopeus (SGD) optimointi ennustaa Vihje maksetun summan. 

Koodin tässä osassa näytetään, miten lineaarisen regressiosuoran mallin lataaminen Azure-blob-säiliö, pisteet skaalatun muuttujien käyttäminen ja tallenna sitten takaisin blob tulokset.

    #SCORE LINEAR REGRESSION MODEL

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    #LOAD LIBRARIES
    from pyspark.mllib.regression import LinearRegressionWithSGD, LinearRegressionModel
    
    # LOAD MODEL AND SCORE USING ** SCALED VARIABLES **
    savedModel = LinearRegressionModel.load(sc, linearRegFileLoc)
    predictions = oneHotTESTregScaled.map(lambda features: (float(savedModel.predict(features))))
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = scoredResultDir + linearregressionfilename;
    predictions.saveAsTextFile(dirfilename)
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**TULOS:**

Solun yläpuolella olevan suorittamiseen aika: 16.63 sekuntia


## <a name="score-classification-and-regression-random-forest-models"></a>Pisteet luokittelu ja regression satunnaisia metsää-mallit

Koodin tässä osassa kerrotaan, miten voit ladata tallennetun luokittelu ja regression satunnaisia metsää mallit tallennetaan Azure-blob-säiliö, pisteet niiden suorituskyvyn vakio valitsimen ja regression toimenpiteitä, ja valitse Tallenna tulokset takaisin Blob-objektien tallennustilaan.

[Satunnainen metsien](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) ovat yhdistelmäasut päätös puut.  Ne yhdistää useita päätös puita overfitting riskien. Satunnainen metsien voit käsitellä categorical ominaisuudet laajentaa multiclass luokitus-asetusta, eivät edellytä ominaisuus skaalaus ja voivat kerätä ei jonkin verran epälineaarisuutta ja ominaisuus vuorovaikutukset. Satunnainen metsien on yksi eniten onnistuneen konepohjaisten oppimistekniikoiden luokitukseen ja regression mallit.

[spark.mllib](http://spark.apache.org/mllib/) tukee satunnaisia metsien binaarinen ja multiclass luokittelu ja regression, sekä jatkuva categorical ominaisuuksilla. 

    # SCORE RANDOM FOREST MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES 
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    
    
    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB
    savedModel = RandomForestModel.load(sc, randomForestClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfclassificationfilename = "RandomForestClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfclassificationfilename;
    predictions.saveAsTextFile(dirfilename)
    

    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB
    savedModel = RandomForestModel.load(sc, randomForestRegFileLoc)
    predictions = savedModel.predict(indexedTESTreg)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfregressionfilename = "RandomForestRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfregressionfilename;
    predictions.saveAsTextFile(dirfilename)

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**TULOS:**

Solun yläpuolella olevan suorittamiseen aika: 31.07 sekuntia


## <a name="score-classification-and-regression-gradient-boosting-tree-models"></a>Pisteet luokittelu ja regression liukuvärin kehittämällä puun mallit

Tässä osassa koodi näkyy luokittelu ja regression liukuvärin kehittämällä puun mallien lataaminen Azure-blob-säiliö, sija niiden suorituskyvyn vakio valitsimen ja regression toimenpiteet ja tallenna sitten takaisin Blob-objektien tallennustilaan tulokset. 

**spark.mllib** tukee GBTs binaarinen luokittelu ja regression, sekä jatkuva categorical ominaisuuksilla. 

[Liukuvärin lisäyksen puut](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) ovat yhdistelmäasut päätös puut. GBTs kouluttaminen päätös puut muutosten pienentämiseksi tappio-funktiota. GBTs voit käsitellä categorical ominaisuuksia, eivät edellytä ominaisuus skaalaus ja voivat kerätä ei jonkin verran epälineaarisuutta ja vuorovaikutukset ominaisuus. Ne voidaan myös multiclass luokitus-asetusta.


    # SCORE GRADIENT BOOSTING TREE MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    
    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB

    #LOAD AND SCORE THE MODEL
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btclassificationfilename;
    predictions.saveAsTextFile(dirfilename)
    

    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB

    # LOAD AND SCORE MODEL 
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeRegressionFileLoc)
    predictions = savedModel.predict(indexedTESTreg)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btregressionfilename;
    predictions.saveAsTextFile(dirfilename)


    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 
    
**TULOS:**

Solun yläpuolella olevan suorittamiseen aika: 14.6 sekuntia


## <a name="clean-up-objects-from-memory-and-print-scored-file-locations"></a>Järjestä objektit muistiin ja tulostaa tulos oletuskansiot

    # UNPERSIST OBJECTS CACHED IN MEMORY
    taxi_df_test_cleaned.unpersist()
    indexedTESTbinary.unpersist();
    oneHotTESTbinary.unpersist();
    indexedTESTreg.unpersist();
    oneHotTESTreg.unpersist();
    oneHotTESTregScaled.unpersist();


    # PRINT OUT PATH TO SCORED OUTPUT FILES
    print "logisticRegFileLoc: " + logisticregressionfilename;
    print "linearRegFileLoc: " + linearregressionfilename;
    print "randomForestClassificationFileLoc: " + rfclassificationfilename;
    print "randomForestRegFileLoc: " + rfregressionfilename;
    print "BoostedTreeClassificationFileLoc: " + btclassificationfilename;
    print "BoostedTreeRegressionFileLoc: " + btregressionfilename;


**TULOS:**

logisticRegFileLoc: LogisticRegressionWithLBFGS_2016 05-0317_22_38.953814.txt

linearRegFileLoc: LinearRegressionWithSGD_2016 05-0317_22_58.878949

randomForestClassificationFileLoc: RandomForestClassification_2016 05-0317_23_15.939247.txt

randomForestRegFileLoc: RandomForestRegression_2016 05-0317_23_31.459140.txt

BoostedTreeClassificationFileLoc: GradientBoostingTreeClassification_2016-05-0317_23_49.648334.txt

BoostedTreeRegressionFileLoc: GradientBoostingTreeRegression_2016-05-0317_23_56.860740.txt



## <a name="consume-spark-models-through-a-web-interface"></a>Ohjattu mallien tarjoaman web-liittymän kautta

Ohjattu tarjoaa voit lähettää etäyhteyden kutsutaan Livy komponenttiin Ehdota tai vuorovaikutteinen kyselyjen REST-liittymän kautta. Livy on käytössä oletusarvoisesti ohjattu HDInsight-klusterin. Saat lisätietoja Livy: [Lähetä ohjattu työt käyttämällä etäyhteyden Livy](../hdinsight/hdinsight-apache-spark-livy-rest-interface.md). 

Voit käyttää Livy lähettää etänä työn, joka erän tulosten tiedosto, joka on tallennettu Azuren Blob-objektien ja kirjoittaa sitten toiseen blob tulokset. Voit tehdä tämän lataat Python-komentosarja  
[Github](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/Spark/Python/ConsumeGBNYCReg.py) Ohjattu klusterin Blob-objektien. Voit käyttää työkalua, kuten **Microsoft Azure-tallennustilan Explorer** tai **AzCopy** Kopioi komentosarja klusterin Blob-objektien. Tässä tapauksessa emme ladata komentosarja ***wasb:///example/python/ConsumeGBNYCReg.py***.   


>[AZURE.NOTE] Pikanäppäimet, että sinun tarvitsee löytyy tallennustilan Ohjattu klusterin liittyvää tiliä-portaalissa. 


Kun tähän sijaintiin ladattu palvelimeen, tämä komentosarja suoritetaan Ohjattu klusterin hajautettu kontekstissa. Se Lataa malli ja suorita ennusteiden mallin syötteen tiedostoja.  

Voit käynnistää tämän komentosarjan etäyhteyden tekemällä yksinkertaisia HTTPS/REST-pyyntö Livy.  Seuraavassa on HTTP-pyyntö käynnistää Python komentosarjan etäyhteyden muodostaminen kääntö komennon. Korvaa CLUSTERLOGIN, CLUSTERPASSWORD, CLUSTERNAME Ohjattu klusterin haluamasi arvot.


    # CURL COMMAND TO INVOKE PYTHON SCRIPT WITH HTTP REQUEST

    curl -k --user "CLUSTERLOGIN:CLUSTERPASSWORD" -X POST --data "{\"file\": \"wasb:///example/python/ConsumeGBNYCReg.py\"}" -H "Content-Type: application/json" https://CLUSTERNAME.azurehdinsight.net/livy/batches

Voit käyttää mitä tahansa kieltä Etäjärjestelmä käynnistää Ohjattu projektin kautta Livy tekemällä yksinkertaisia HTTPS puhelun Perustodentamista.   


>[AZURE.NOTE] Se on helppo tapa Python pyynnöt kirjaston tehtäessä HTTP-puhelu, mutta se ei ole asennettuna oletusarvoisesti Azure-funktiot. Jotta vanhempia HTTP-kirjastojen avulla sen sijaan.   


Näin HTTP puhelun Python koodi:

    #MAKE AN HTTPS CALL ON LIVY. 

    import os

    # OLDER HTTP LIBRARIES USED HERE INSTEAD OF THE REQUEST LIBRARY AS THEY ARE AVAILBLE BY DEFAULT
    import httplib, urllib, base64
    
    # REPLACE VALUE WITH ONES FOR YOUR SPARK CLUSTER
    host = '<spark cluster name>.azurehdinsight.net:443'
    username='<username>'
    password='<password>'
    
    #AUTHORIZATION
    conn = httplib.HTTPSConnection(host)
    auth = base64.encodestring('%s:%s' % (username, password)).replace('\n', '')
    headers = {'Content-Type': 'application/json', 'Authorization': 'Basic %s' % auth}
    
    # SPECIFY THE PYTHON SCRIPT TO RUN ON THE SPARK CLUSTER
    # IN THE FILE PARAMETER OF THE JSON POST REQUEST BODY
    r=conn.request("POST", '/livy/batches', '{"file": "wasb:///example/python/ConsumeGBNYCReg.py"}', headers )
    response = conn.getresponse().read()
    print(response)
    conn.close()


Voit lisätä Python koodi myös, että tulosten blob perusteella eri tapahtumia, kuten ajastin, luominen tai päivittäminen blob Ohjattu projektin lähetyksen käynnistettävän [Azure-Funktiot](https://azure.microsoft.com/documentation/services/functions/) . 

Jos käytät mieluummin koodin vapaa asiakas-versio, näkyvissä pistemäärä määrittämällä HTTP-toiminnon **Logiikan sovellusten suunnittelu** -ja sen parametrien ohjattu Siirtoerän käynnistäminen [Azure logiikan sovellusten](https://azure.microsoft.com/documentation/services/app-service/logic/) avulla. 

- Azure-portaalista, Luo uusi logiikan-sovellus valitsemalla **+ Uusi** -> **Web + Mobile** -> **Logiikan sovelluksen**. 
- Kun kosketat **Logiikan sovellusten suunnittelu**, kirjoita nimi logiikan sovelluksen ja sovelluksen palvelun suunnitteleminen.
- Valitse HTTP-toiminto ja tarvittavat parametrit seuraavassa kuvassa:

![](./media/machine-learning-data-science-spark-model-consumption/spark-logica-app-client.png)


## <a name="whats-next"></a>Mitä seuraavaksi? 

**Usean kelpoisuustarkistus ja hyperparameter sweeping**: miten malleja voi nähdä [Advanced tietojen tarkasteluun ja arvioiminen ohjattu](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) koulutus rajat kelpoisuustarkistus ja hyper-parametrin sweeping avulla.
