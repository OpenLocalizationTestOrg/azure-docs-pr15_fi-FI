<properties
    pageTitle="Tietojen tarkasteluun ja arvioiminen ohjattu | Microsoft Azure"
    description="Esitellään ohjattu MLlib työkalujen tietojen tarkasteluun ja mallinnus ominaisuuksia."
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

# <a name="data-exploration-and-modeling-with-spark"></a>Tietojen tarkasteluun ja ohjattu arvioiminen

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

Tätä vaiheittaista käyttää HDInsight ohjattu voit tehdä tietojen tarkasteluun ja binaarinen luokittelu ja mallinnus tehtävät-Kokousesitelmän näyte regression Taksiliikennepalvelut työmatkan ja kuljetusmaksua 2013 tietojoukko.  Se opastaa [Tietojen tiede prosessin](http://aka.ms/datascienceprocess)vaiheet, lopusta loppuun-käyttämällä HDInsight-Ohjattu klusterin käsittelyä varten ja Azure-BLOB tiedot ja mallien tallentamiseen. Prosessin käsitellään ja valitseminen puolestaan Visualisoi Azure-tallennustilan Blob-tuonut tiedot ja valmistelee ennakoivan mallien rakentamiseen tiedot. Nämä ovat muodosta tehtävät binaarinen luokittelu ja regression mallinnus ohjattu MLlib-työkalujen avulla.

- **Binaarinen luokitus** tehtävä on ennustaa riippumatta siitä, onko Vihje maksettu matkan. 
- **Regressiosuoran** tehtävä on ennustaa Vihje Vihje muiden ominaisuuksien perusteella määrää. 

Käytämme mallit ovat logistista ja lineaarisen regressiosuoran, satunnainen metsien ja liukuvärin tehosti puita:

- [Lineaarisen regressiosuoran SGD kanssa](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) on Stochastic liukuvärin laskeutumisnopeus (SGD) menetelmän käyttävän lineaarisen regressiosuoran mallin ja optimointiin ja ominaisuus skaalaus ennustetaan Vihje summat maksettu. 
- [Logistista regressiota LBFGS kanssa](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) tai "logit" regression on regression malli, jonka avulla voidaan muuttujan ollessa categorical voit tehdä tietojen luokittelu. LBFGS on Newtonin optimointi algoritmin, joka on lähellä käyttämällä muistin rajoitetun Broyden – Fletcher – Goldfarb – Shanno (BFGS) algoritmin ja jota laajasti käytetään tietokoneen learning.
- [Satunnainen metsien](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) ovat yhdistelmäasut päätös puut.  Ne yhdistää useita päätös puita overfitting riskien. Satunnainen metsien käytetään regression ja luokittelu ja voit käsitellä categorical ominaisuudet ja laajennettavissa multiclass luokitus-asetus. Ne eivät edellytä ominaisuus skaalaus ja voivat kerätä ei jonkin verran epälineaarisuutta ja vuorovaikutukset ominaisuus. Satunnainen metsien on yksi eniten onnistuneen konepohjaisten oppimistekniikoiden luokitukseen ja regression mallit.
- [Liukuvärin tehosti puita](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) ovat yhdistelmäasut päätös puut. GBTs kouluttaminen päätös puut muutosten pienentämiseksi tappio-funktiota. GBTs käytetään regression ja luokittelu ja voit käsitellä categorical ominaisuuksia, eivät edellytä ominaisuus skaalaus ja voivat kerätä ei jonkin verran epälineaarisuutta ja ominaisuus vuorovaikutukset. Ne voidaan myös multiclass luokitus-asetusta.

Mallinnus vaiheet sisältää myös esittää, kuinka kouluttaminen ja arvioi Tallenna kukin mallin koodi. Python on käytetty koodi ratkaisun ja Näytä asiaa joudutaan.   


>[AZURE.NOTE] Vaikka ohjattu MLlib työkalujen on suunniteltu toimimaan on paljon, suhteellisen pieni otoksen (noin 30 megatavua käyttämällä 170K rivien noin 0,1 prosenttia alkuperäisen Kokousesitelmän tietojoukko) käytetään tässä helpottamiseksi. Tässä määritetyn Harjoitus toimii tehokkaasti (noin 10 minuuttia) HDInsight-klusterin 2 työntekijä solmujen kanssa. Sama koodi, pieniä muutoksia voidaan käsitellä suurempi-tietosarjoja tarvittavat tiedot muistiin välimuistiin ja muuttamiseen klusteri muutoksia.

## <a name="prerequisites"></a>Edellytykset

Tarvitset Azure-tili ja luodun HDInsight ohjattu voit tarpeen HDInsight 3.4 ohjattu-1.6 klusterin vaiheiden suorittamiseen. Katso ohjeet [Yleiskatsaus tietojen tiede käyttämällä Ohjattu Azure HDInsight-](machine-learning-data-science-spark-overview.md) täytä näitä vaatimuksia. Ohjeaiheen sisältää myös käytetyt tähän Kokousesitelmän 2013 taksin tiedot ja ohjeita siitä, miten voit suorittaa koodin Ohjattu klusterin Jupyter muistikirjan kuvaus. **PySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb** muistikirjan, joka sisältää tässä ohjeaiheessa MALLIKOODEJA sisältyy [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark). 


[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a>Määritys: tallennuspaikkojen, kirjastojen ja valmiiksi määritettyä ohjattu yhteydessä

Ohjattu voi lukea ja kirjoittaa Azure tallennustilan Blob (tunnetaan myös nimellä WASB). Jotta kaikki tiedot tallennetut siellä voidaan käsitellä ohjattu ja tallennettu uudelleen WASB tulokset.

Jos haluat tallentaa WASB mallien tai tiedostot, polku on määritetty oikein. Ohjattu klusterin liitetty oletusarvon säilö voidaan viitata käyttämällä polku alkaa sanalla: "wasb: / / /". Muista sijainneista, joihin viitataan "wasb: / /".


### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>Määrittää hakemiston polut tallennuspaikkojen WASB

Seuraava koodi malli määrittää niitä voi lukea ja mallin tallennustilan kansio, johon mallin tulos on tallennettu polun sijainti:


    # SET PATHS TO FILE LOCATIONS: DATA AND MODEL STORAGE

    # LOCATION OF TRAINING DATA
    taxi_train_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv";

    # SET THE MODEL STORAGE DIRECTORY PATH 
    # NOTE THAT THE FINAL BACKSLASH IN THE PATH IS NEEDED.
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/" 


### <a name="import-libraries"></a>Tuo-kirjastot

Määrittäminen edellyttää myös tuominen tarvittavat kirjastoihin. Määritä ohjattu kontekstin ja tuoda tarvittavat kirjastojen seuraava koodi:


    # IMPORT LIBRARIES
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

Jupyter muistikirjat mukana toimitettuja PySpark ytimet on valmiiksi määritettyä kontekstissa. Sinun ei tarvitse määrittää ohjattu tai rakenteen kontekstit erikseen ennen kuin aloitat käsitteleminen sovelluksen kehittävät siten. Nämä kontekstit ovat käytettävissä oletusarvoisesti. Nämä kontekstit ovat seuraavat:

- SC - ohjattu varten 
- sqlContext - rakennetta varten

PySpark ydin tarjoaa esimääritettyjä "magics", jotka ovat erityisiä komennot, jotka voivat soittaa kanssa %%. On kaksi tällaisten komennot, joita käytetään seuraavia MALLIKOODEJA.

- **%% paikallisen** Määrittää, että seuraaville riveille koodi on suoritettava paikallisesti. Koodi on oltava kelvollinen Python koodi.
- **%%sql -o <variable name>** Suorittaa sqlContext rakenteen kysely. Jos -o-parametri on kulunut, kyselyn tulos on samanlainen %% paikallisen Python yhteydessä kuin Pandas DataFrame.
 

Saat lisätietoja Jupyter muistikirjat ja valmiin ytimet "magics", niiden avulla, katso [ytimet käytettävissä olevien HDInsight HDInsight ohjattu Linux varausyksiköiden muistikirjojen Jupyter](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).
 

## <a name="data-ingestion-from-public-blob"></a>Tietoja nieltynä julkisen Blob-objektien poistaminen

Tietoja tiede prosessin ensimmäiseksi on ingest lähteistä analysoitavien tietojen kohtaa, johon on tietojen tarkasteluun ja mallinnus ympäristölle sijaitsee. Ympäristön on ohjattu tämän vaiheittaisen kuvauksen suorittamiseksi. Tässä osassa on koodin sarjan tehtävien suorittamiseen:

- tietoja näyte mallintaa ingest
- Lue syötteen tietojoukossa (tallennettu .tsv-tiedostona)
- Muotoile ja poista tiedot
- Voit luoda ja tallentaa objektit (RDDs tai tietojen kehykset) muisti
- Rekisteröi temp-taulukkona SQL-kontekstissa.

Seuraavassa on tietoja erilaisille koodi.

    # INGEST DATA

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT FILE FROM PUBLIC BLOB
    taxi_train_file = sc.textFile(taxi_train_file_loc)
    
    # GET SCHEMA OF THE FILE FROM HEADER
    schema_string = taxi_train_file.first()
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
    
    # PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
    taxi_header = taxi_train_file.filter(lambda l: "medallion" in l)
    taxi_temp = taxi_train_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
            .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
                            float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
                            float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))
    
    
    # CREATE DATA FRAME
    taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)
    
    # CREATE A CLEANED DATA-FRAME BY DROPPING SOME UN-NECESSARY COLUMNS & FILTERING FOR UNDESIRED VALUES OR OUTLIERS
    taxi_df_train_cleaned = taxi_train_df.drop('medallion').drop('hack_license').drop('store_and_fwd_flag').drop('pickup_datetime')\
        .drop('dropoff_datetime').drop('pickup_longitude').drop('pickup_latitude').drop('dropoff_latitude')\
        .drop('dropoff_longitude').drop('tip_class').drop('total_amount').drop('tolls_amount').drop('mta_tax')\
        .drop('direct_distance').drop('surcharge')\
        .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200" )

    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()
    
    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**TULOS:**

Solun yläpuolella olevan suorittamiseen aika: 51.72 sekuntia


## <a name="data-exploration--visualization"></a>Tietojen tarkasteluun ja visualisointi

Kun tiedot on tuotu ohjattu, tietojen tiede prosessin seuraava vaihe on saada tarkempaa tietoja tietojen tarkasteluun ja visualisoinnin kautta. Tässä osassa tutkia taksin tiedot käyttämällä SQL-kyselyjä ja piirtää kohde muuttujat ja silmämääräisesti mahdollisen ominaisuuksia. Tarkemmin sanottuna on piirretään taajuus matkustaja laskee taksin trips, Vihje summat korkojakso ja miten vihjeitä vaihtelevat maksusumma ja tyypin mukaan.

### <a name="plot-a-histogram-of-passenger-count-frequencies-in-the-sample-of-taxi-trips"></a>Histogrammin matkustaja Laske taajuuksien taksin trips otoksessa

Tämä koodi ja myöhempiä katkelmat kyselyn otoksen ja paikallisen tärkeä tietojen SQL tärkeä avulla.

- **SQL tärkeä (`%%sql`)** HDInsight PySpark ydin tukee helposti tekstiin HiveQL kyselyt sqlContext. (-O muuttujan_nimi) argumentin jatkuu SQL-kyselyn tulos kuin Pandas-DataFrame Jupyter-palvelimeen. Tämä tarkoittaa on käytettävissä paikallisten-tilassa.
- ** `%%local` Tärkeä** voidaan suorittaa koodin paikallisesti Jupyter palvelimessa, joka on HDInsight-klusterin headnode. Yleensä käytetään `%%local` magic yhdessä `%%sql` magic -o käynnistysparametrilla. + O-parametrin jatkuvat paikallisesti SQL-kyselyn tulos ja valitse sitten %% paikallisen tärkeä käynnistää koodikatkelman suorittaa paikallisesti vastaan SQL-kyselyjä, joka on samanlainen paikallisesti tulosteen seuraava määrittäminen

Tulos on automaattisesti näyttää, kun suoritat koodi.

Tämä kysely palauttaa trips matkustaja määrän mukaan. 

    # PLOT FREQUENCY OF PASSENGER COUNTS IN TAXI TRIPS

    # HIVEQL QUERY AGAINST THE sqlContext
    %%sql -q -o sqlResults
    SELECT passenger_count, COUNT(*) as trip_counts 
    FROM taxi_train 
    WHERE passenger_count > 0 and passenger_count < 7 
    GROUP BY passenger_count 

Koodi luo paikalliset tiedot-kehyksen kyselyn tuloksesta ja esittää tietoja. `%%local` Tärkeä Luo paikalliset tiedot-kehys, `sqlResults`, jota voidaan käyttää tulostamisessa matplotlib kanssa. 

>[AZURE.NOTE] Tämä PySpark tärkeä käytetään useita kertoja tämän vaiheittaisen kuvauksen suorittamiseksi. Jos tietojen määrää on suuri, on esimerkki tiedot-kehys, joka mahtuu luominen paikalliseen muistiin.

    #CREATE LOCAL DATA-FRAME AND USE FOR MATPLOTLIB PLOTTING

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local
    
    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES. 
    # CLICK ON THE TYPE OF PLOT TO BE GENERATED (E.G. LINE, AREA, BAR ETC.)
    sqlResults

Näin koodin esitystapa trips matkustaja laskee mukaan

    # PLOT PASSENGER NUMBER VS. TRIP COUNTS
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline
    
    x_labels = sqlResults['passenger_count'].values
    fig = sqlResults[['trip_counts']].plot(kind='bar', facecolor='lightblue')
    fig.set_xticklabels(x_labels)
    fig.set_title('Counts of trips by passenger count')
    fig.set_xlabel('Passenger count in trips')
    fig.set_ylabel('Trip counts')
    plt.show()

**TULOS:**

![Matkan korkojakso matkustaja määrän mukaan](./media/machine-learning-data-science-spark-data-exploration-modeling/trip-freqency-by-passenger-count.png)

Voit valita useita erilaisia visualisointeja (taulukko, ympyräkaavio, rivin, alue tai palkin) välillä käyttämällä **tyyppi** -valikkopainikkeet muistikirjan. Palkin piirron on mukana.
    
### <a name="plot-a-histogram-of-tip-amounts-and-how-tip-amount-varies-by-passenger-count-and-fare-amounts"></a>Lajiteltu histogrammi Vihje summat ja miten Vihje määrä vaihtelee matkustaja Laske- ja maksun luvuilla.

Käytä SQL-kyselyn mallitiedot.

    #PLOT HISTOGRAM OF TIP AMOUNTS AND VARIATION BY PASSENGER COUNT AND PAYMENT TYPE
    
    # HIVEQL QUERY AGAINST THE sqlContext
    %%sql -q -o sqlResults
    SELECT fare_amount, passenger_count, tip_amount, tipped 
    FROM taxi_train 
    WHERE passenger_count > 0 
    AND passenger_count < 7 
    AND fare_amount > 0 
    AND fare_amount < 200 
    AND payment_type in ('CSH', 'CRD') 
    AND tip_amount > 0 
    AND tip_amount < 25


Koodin solun käyttää SQL-kyselyn luomiseen kolme joudutaan tiedot.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local
    
    # HISTOGRAM OF TIP AMOUNTS AND PASSENGER COUNT
    ax1 = sqlResults[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()
    
    # TIP BY PASSENGER COUNT
    ax2 = sqlResults.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()
    
    # TIP AMOUNT BY FARE AMOUNT, POINTS ARE SCALED BY PASSENGER COUNT
    ax = sqlResults.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(sqlResults.passenger_count))
    ax.set_title('Tip amount by Fare amount')
    ax.set_xlabel('Fare Amount ($)')
    ax.set_ylabel('Tip Amount ($)')
    plt.axis([-2, 100, -2, 20])
    plt.show()


**TULOS:** 

![Vihje summan jakauma](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-distribution.png)

![Vihje summa matkustaja määrän mukaan](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-by-passenger-count.png)

![Vihje määrä, maksun summa](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-by-fare-amount.png)


## <a name="feature-engineering-transformation-and-data-preparation-for-modeling"></a>Tekniset Funktiot, muunnos ja tietojen mallinnus valmistelu ominaisuus
Tässä osassa kuvataan sekä annetaan koodi, joita käytetään tietojen valmisteleminen ML mallinnus käytettäviksi. Se näyttää, miten voit tehdä seuraavat toimet:

- Luo uusi ominaisuus binning tuntia liikenne aika uudelleenkäytettävät
- Indeksi ja koodata categorical ominaisuudet
- Luo nimetty pisteen objektit ML Funktiot-syötteestä
- Luo satunnaisen aliraportti esimerkkejä tietojen ja jaa se koulutus ja määrittää testaaminen
- Ominaisuuden skaalaus
- Muistin välimuistin objektit


### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a>Luo uusi ominaisuus binning tuntia liikenne aika uudelleenkäytettävät

Tämä koodi näkyy Luo uuden toiminnon liikenne aika uudelleenkäytettävät binning tuntia ja tallentaa tuloksena olevat tiedot kehys, muistiin. Jos joustavat Distributed tietojoukkoja (RDDs) ja tietoja-kehysten käytetään toistuvasti, välimuistiin johtaa parannettu suorittamisen kertaa. Näin ollen on välimuistin RDDs ja tietojen kehyksiä ongelmatilanteita useita vaiheissa. 

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night" 
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush" 
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_train 
    """
    taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)
    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    # THE .COUNT() GOES THROUGH THE ENTIRE DATA-FRAME,
    # MATERIALIZES IT IN MEMORY, AND GIVES THE COUNT OF ROWS.
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()

**TULOS:** 

126050

### <a name="index-and-encode-categorical-features-for-input-into-modeling-functions"></a>Indeksi ja koodata mallinnus Funktiot-syötteestä categorical ominaisuudet

Tässä osassa esitellään indeksoida ja koodata mallinnus-Funktiot-syötteestä categorical ominaisuuksia. Mallinnus ja ennustaa MLlib toiminnot vaativat ominaisuuksien categorical syöttötiedot indeksoitu tai koodattu ennen käyttöä. Mallista riippuen sinun on indeksoida tai koodata niitä eri tavoilla:  

- **Puun perustuva mallinnus** edellyttää luokat koodattava numeeriset arvot (esimerkiksi ominaisuuden, jossa on kolme luokkaa saattaa olla koodattu 0; 1; 2). Tämä on annettu MLlib's [StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) -funktio. Tämä funktio käyttää merkkijonon sarakkeen otsikoiden sarakkeen otsikon indeksien, jotka on tilattu otsikko tiheydet mukaan. Vaikka indeksoitu numeeriset arvot syöte ja tietojen käsittelyä varten, puun perustuvat algoritmit voidaan määrittää käsitellä niitä oikein luokat. 

- **Logistic ja lineaarisen regressiosuoran mallit** edellyttävät yksi kuuman koodaus, jossa, esimerkiksi ominaisuuden, jossa on kolme luokkaa laajennettu kolmeen toiminto-sarakkeeseen, kunkin sisältävän 0 tai 1 Huomautus luokan mukaan. MLlib avulla voit tehdä yhden kuuman koodaus [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) -funktiota. Tämä encoder yhdistää sarakkeen otsikon indeksien binaarinen vektorit arvolla enintään yhden yksi-yhteen sarakkeeseen. Algoritmeista, odottaa numeerisen arvon ominaisuuksia, kuten logistista regressiota categorical ominaisuuksia voi käyttää tätä koodausta avulla.

Näin koodin indeksoida ja koodata categorical ominaisuuksia:


    # INDEX AND ENCODE CATEGORICAL FEATURES

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES    
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, VectorIndexer
    
    # INDEX AND ENCODE VENDOR_ID
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_train_with_newFeatures) # Input data-frame is the cleaned one from above
    indexed = model.transform(taxi_df_train_with_newFeatures)
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
    
    # INDEX AND TRAFFIC TIME BINS
    stringIndexer = StringIndexer(inputCol="TrafficTimeBins", outputCol="TrafficTimeBinsIndex")
    model = stringIndexer.fit(encoded3)
    indexed = model.transform(encoded3)
    encoder = OneHotEncoder(dropLast=False, inputCol="TrafficTimeBinsIndex", outputCol="TrafficTimeBinsVec")
    encodedFinal = encoder.transform(indexed)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**TULOS:**

Solun yläpuolella olevan suorittamiseen aika: 1.28 sekuntia

### <a name="create-labeled-point-objects-for-input-into-ml-functions"></a>Luo nimetty pisteen objektit ML Funktiot-syötteestä

Tässä osassa on koodi, joka näyttää, miten indeksoida categorical tekstitietoja nimetty piste-tietotyypiksi ja koodata sen niin, että se voidaan testata MLlib logistista regressiota ja muiden luokitus-mallien ja kouluttaminen. Nimetty pisteen objektit ovat joustavat Distributed tietojoukkoja (RDD) muotoiltu siten, että tarvitaan kuin syöttötiedot ML algoritmit MLlib kaiken irti. [Piste, jossa](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) on paikallinen vector – tiivis tai lyhyet, liittyvä otsikko ja vastaus.  

Tässä osassa on koodi, joka näyttää, miten indeksoida categorical tekstitietoja [kohta, jossa](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) -tietotyypiksi ja koodata sen niin, että se voidaan testata MLlib logistista regressiota ja muiden luokitus-mallien ja kouluttaminen. Nimetty pisteen objektit ovat joustavat Distributed tietojoukkoja (RDD) koostuva otsikko (kohde tai vastauksen muuttuva) ja toiminnon vektorista. Tässä muodossa tarvita syötteeksi monta ML algoritmit MLlib.

Seuraavassa on indeksoida ja koodata tekstin muotoiluominaisuuksien binaarinen luokituksen koodi.

    # FUNCTIONS FOR BINARY CLASSIFICATION

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from numpy import array

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex,
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        labPt = LabeledPoint(line.tipped, features)
        return  labPt
    
    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tipped, features)
        return  labPt


Seuraavassa on koodin koodata ja indeksoi categorical tekstin muotoiluominaisuuksien lineaarisen regressiosuoran analyysia varten.

    # FUNCTIONS FOR REGRESSION WITH TIP AMOUNT AS TARGET VARIABLE

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingRegression(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex, 
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])

        labPt = LabeledPoint(line.tip_amount, features)
        return  labPt
    
    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO LINEAR REGRESSION MODELS
    def parseRowOneHotRegression(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tip_amount, features)
        return  labPt


### <a name="create-a-random-sub-sampling-of-the-data-and-split-it-into-training-and-testing-sets"></a>Luo satunnaisen aliraportti esimerkkejä tietojen ja jaa se koulutus ja määrittää testaaminen

Tämä koodi luo satunnaisen esimerkkejä (25 % käytetään tässä) tiedot. Vaikka se ei edellytä tässä esimerkissä koon vuoksi DataSet, on osoitettava siitä, miten voit voit Esimerkki tähän niin tiedät, miten sitä käytetään oman ongelman tarpeen mukaan. Kun objektit on suuri, tämä tallentaa merkittäviä kellonajan, kun koulutus mallit. Seuraavaksi on jaettu otosten luokittelu ja regression mallinnus koulutus-osa (tähän 75 %) ja testauksen osa (tähän 25 %).


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.sql.functions import rand

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    samplingFraction = 0.25;
    trainingFraction = 0.75; testingFraction = (1-trainingFraction);
    seed = 1234;
    encodedFinalSampled = encodedFinal.sample(False, samplingFraction, seed=seed)
    
    # SPLIT SAMPLED DATA-FRAME INTO TRAIN/TEST
    # INCLUDE RAND COLUMN FOR CREATING CROSS-VALIDATION FOLDS (FOR USE LATER IN AN ADVANCED TOPIC)
    dfTmpRand = encodedFinalSampled.select("*", rand(0).alias("rand"));
    trainData, testData = dfTmpRand.randomSplit([trainingFraction, testingFraction], seed=seed);
    
    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary = trainData.map(parseRowIndexingBinary)
    indexedTESTbinary = testData.map(parseRowIndexingBinary)
    oneHotTRAINbinary = trainData.map(parseRowOneHotBinary)
    oneHotTESTbinary = testData.map(parseRowOneHotBinary)
    
    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg = trainData.map(parseRowIndexingRegression)
    indexedTESTreg = testData.map(parseRowIndexingRegression)
    oneHotTRAINreg = trainData.map(parseRowOneHotRegression)
    oneHotTESTreg = testData.map(parseRowOneHotRegression)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**TULOS:**

Solun yläpuolella olevan suorittamiseen aika: 0,24 sekuntia


### <a name="feature-scaling"></a>Ominaisuuden skaalaus

Skaalaus ominaisuutta kutsutaan myös tietojen normalisointi vakuutusyhtiö, että ominaisuuksien laajasti maksanut arvot ovat ole antanut liiallinen arvioida objektiivisin-funktiossa. Ominaisuuden skaalauksen koodi käyttää [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) skaalata yksikön varianssin ominaisuuksia. Se tarjoaa MLlib käytettäviksi lineaarisen regressiosuoran kanssa Stochastic liukuvärin laskeutumisnopeus (SGD), Suositut algoritmin, koulutus monien muiden konepohjaisten oppimistekniikoiden malleja, kuten haltijan muutoksista tai tuki vector koneet (SVM).

>[AZURE.NOTE] LinearRegressionWithSGD algoritmin on luottamuksellisia voit korostaa skaalaus löytänyt.

Seuraavassa on asteikko muuttujat regularized lineaarinen SGD algoritmin käytettäväksi koodi.

    # FEATURE SCALING

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from pyspark.mllib.linalg import Vectors
    from pyspark.mllib.feature import StandardScaler, StandardScalerModel
    from pyspark.mllib.util import MLUtils
    
    # SCALE VARIABLES FOR REGULARIZED LINEAR SGD ALGORITHM
    label = oneHotTRAINreg.map(lambda x: x.label)
    features = oneHotTRAINreg.map(lambda x: x.features)
    scaler = StandardScaler(withMean=False, withStd=True).fit(features)
    dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
    oneHotTRAINregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))
    
    label = oneHotTESTreg.map(lambda x: x.label)
    features = oneHotTESTreg.map(lambda x: x.features)
    scaler = StandardScaler(withMean=False, withStd=True).fit(features)
    dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
    oneHotTESTregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**TULOS:**

Solun yläpuolella olevan suorittamiseen aika: 13.17 sekuntia


### <a name="cache-objects-in-memory"></a>Muistin välimuistin objektit

Aika, koulutusta ja ML algoritmeista testaaminen voidaan vähentää välimuistiin syöttötiedot kehyksen objektien käytettäviä luokitus-regression, ja skaalatun ominaisuuksia.

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary.cache()
    indexedTESTbinary.cache()
    oneHotTRAINbinary.cache()
    oneHotTESTbinary.cache()
    
    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg.cache()
    indexedTESTreg.cache()
    oneHotTRAINreg.cache()
    oneHotTESTreg.cache()
    
    # SCALED FEATURES
    oneHotTRAINregScaled.cache()
    oneHotTESTregScaled.cache()
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**TULOS:** 

Solun yläpuolella olevan suorittamiseen aika: 0,15 sekuntia


## <a name="predict-whether-or-not-a-tip-is-paid-with-binary-classification-models"></a>Ennustaa riippumatta siitä, onko Vihje maksetaan binaarinen luokitus-mallit

Tässä osassa esitellään, miten Käytä kolme mallit, korrelaatio binaarinen luokitus-tehtävän Vihje maksetaan taksin matkan vai ei. Esittää mallit ovat:

- Haltijan logistista regressiota 
- Satunnainen metsää malli
- Liukuvärin lisäyksen puut

Mallien rakentaminen koodiosassa jakautuu vaiheet: 

1. Parametrin omaavien tietojen **malli-koulutus**
2. Arvot ja testaa tietojoukon **mallin arviointi**
3. **Mallin tallentaminen** myöhempää kulutus blob

### <a name="classification-using-logistic-regression"></a>Perusyksikkönä logistista regressiota

Koodin tässä osassa näytetään, miten kouluttaminen ja arvioi logistista regressiota mallin säästäminen [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) , riippumatta siitä, onko Vihje maksettu Kokousesitelmän taksin työmatkan ja maksun tietojoukko matkan ennustaa.

**KV ja hyperparameter sweeping logistista regressiota mallin kouluttaminen**

    # LOGISTIC REGRESSION CLASSIFICATION WITH CV AND HYPERPARAMETER SWEEPING

    # GET ACCURACY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionWithLBFGS 
    from sklearn.metrics import roc_curve,auc
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics
    
    
    # CREATE MODEL WITH ONE SET OF PARAMETERS
    logitModel = LogisticRegressionWithLBFGS.train(oneHotTRAINbinary, iterations=20, initialWeights=None, 
                                                   regParam=0.01, regType='l2', intercept=True, corrections=10, 
                                                   tolerance=0.0001, validateData=True, numClasses=2)
    
    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(logitModel.weights))
    print("Intercept: " + str(logitModel.intercept))
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**TULOS:** 

Kerroin: [0.0082065285375-0.0223675576104-0.0183812028036, - 3.48124578069e-05-0.00247646947233-0.00165897881503, 0.0675394837328,-0.111823113101,-0.324609912762,-0.204549780032,-1.36499216354, 0.591088507921,-0.664263411392,-1.00439726852, 3.46567827545,-3.51025855172,-0.0471341112232,-0.043521833294, 0.000243375810385, 0.054518719222]

Leikkauspiste:-0.0111216486893

Solun yläpuolella olevan suorittamiseen aika: 14.43 sekuntia

**Vakio arvot kanssa binaarinen luokitus-mallin arvioiminen**

    #EVALUATE LOGISTIC REGRESSION MODEL WITH LBFGS

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # PREDICT ON TEST DATA WITH MODEL
    predictionAndLabels = oneHotTESTbinary.map(lambda lp: (float(logitModel.predict(lp.features)), lp.label))
    
    # INSTANTIATE METRICS OBJECT
    metrics = BinaryClassificationMetrics(predictionAndLabels)

    # AREA UNDER PRECISION-RECALL CURVE
    print("Area under PR = %s" % metrics.areaUnderPR)

    # AREA UNDER ROC CURVE
    print("Area under ROC = %s" % metrics.areaUnderROC)
    metrics = MulticlassMetrics(predictionAndLabels)

    # OVERALL STATISTICS
    precision = metrics.precision()
    recall = metrics.recall()
    f1Score = metrics.fMeasure()
    print("Summary Stats")
    print("Precision = %s" % precision)
    print("Recall = %s" % recall)
    print("F1 Score = %s" % f1Score)


    ## SAVE MODEL WITH DATE-STAMP
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp;
    dirfilename = modelDir + logisticregressionfilename;
    logitModel.save(sc, dirfilename);
    
    # OUTPUT PROBABILITIES AND REGISTER TEMP TABLE
    logitModel.clearThreshold(); # This clears threshold for classification (0.5) and outputs probabilities
    predictionAndLabelsDF = predictionAndLabels.toDF()
    predictionAndLabelsDF.registerTempTable("tmp_results");
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**TULOS:** 

Hinta pinta = 0.985297691373

OHJETIEDOSTO pinta = 0.983714670256

Yhteenveto tilasto

Tarkkuus = 0.984304060189

Peruuttaa = 0.984304060189

F1 Sija = 0.984304060189

Solun yläpuolella olevan suorittamiseen aika: 57.61 sekuntia

**Piirrä OHJETIEDOSTO käyrän.**

*PredictionAndLabelsDF* on rekisteröity taulukon *tmp_results*edelliseen soluun. *tmp_results* voidaan tehdä kyselyjä ja tulosta tiedot sqlResults kehykseen tulostamisessa. Seuraavassa on koodi.


    # QUERY RESULTS                              
    %%sql -q -o sqlResults
    SELECT * from tmp_results


Seuraavassa on julkaista ennusteiden ja piirtää OHJETIEDOSTO-käyrän koodi.

    # MAKE PREDICTIONS AND PLOT ROC-CURVE

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc

    # MAKE PREDICTIONS
    predictions_pddf = test_predictions.rename(columns={'_1': 'probability', '_2': 'label'})
    prob = predictions_pddf["probability"] 
    fpr, tpr, thresholds = roc_curve(predictions_pddf['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT ROC CURVE
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
    

**TULOS:**

![Logistista regressiota OHJETIEDOSTO curve.png](./media/machine-learning-data-science-spark-data-exploration-modeling/logistic-regression-roc-curve.png)


### <a name="random-forest-classification"></a>Satunnainen metsää luokittelu

Koodin tässä osassa näytetään, miten kouluttaminen ja arvioi Tallenna satunnaisia metsää mallin, joka ennustaa riippumatta siitä, onko Vihje maksettu Kokousesitelmän taksin työmatkan ja maksun tietojoukko matkan.
    
    #PREDICT WHETHER A TIP IS PAID OR NOT USING RANDOM FOREST

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics
    
    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    
    # TRAIN RANDOMFOREST MODEL
    rfModel = RandomForest.trainClassifier(indexedTRAINbinary, numClasses=2, 
                                           categoricalFeaturesInfo=categoricalFeaturesInfo,
                                           numTrees=25, featureSubsetStrategy="auto",
                                           impurity='gini', maxDepth=5, maxBins=32)
    ## UN-COMMENT IF YOU WANT TO PRINT TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())
    
    # PREDICT ON TEST DATA AND EVALUATE
    predictions = rfModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)
    
    # AREA UNDER ROC CURVE
    metrics = BinaryClassificationMetrics(predictionAndLabels)
    print("Area under ROC = %s" % metrics.areaUnderROC)
    
    # PERSIST MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfclassificationfilename = "RandomForestClassification_" + datestamp;
    dirfilename = modelDir + rfclassificationfilename;
    
    rfModel.save(sc, dirfilename);
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**TULOS:**

OHJETIEDOSTO pinta = 0.985297691373

Solun yläpuolella olevan suorittamiseen aika: 31.09 sekuntia


### <a name="gradient-boosting-trees-classification"></a>Liukuvärin lisäyksen puiden luokittelu

Koodin tässä osassa näytetään, miten kouluttaminen, käsitellä, ja Tallenna liukuvärin lisäyksen puut mallin, joka ennustaa riippumatta siitä, onko Vihje maksettu Kokousesitelmän taksin työmatkan matkan ja kuljetusmaksua tietojoukko.

    #PREDICT WHETHER A TIP IS PAID OR NOT USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    
    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    
    gbtModel = GradientBoostedTrees.trainClassifier(indexedTRAINbinary, categoricalFeaturesInfo=categoricalFeaturesInfo, numIterations=5)
    ## UNCOMMENT IF YOU WANT TO PRINT TREE DETAILS
    #print('Learned classification GBT model:')
    #print(bgtModel.toDebugString())
    
    # PREDICT ON TEST DATA AND EVALUATE
    predictions = gbtModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)
    
    # AREA UNDER ROC CURVE
    metrics = BinaryClassificationMetrics(predictionAndLabels)
    print("Area under ROC = %s" % metrics.areaUnderROC)
    
    # PERSIST MODEL IN A BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp;
    dirfilename = modelDir + btclassificationfilename;
    
    gbtModel.save(sc, dirfilename)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**TULOS:**

OHJETIEDOSTO pinta = 0.985297691373

Solun yläpuolella olevan suorittamiseen aika: 19.76 sekuntia


## <a name="predict-tip-amounts-for-taxi-trips-with-regression-models"></a>Vihje lukumäärät taksin trips regression mallit ja ennustaa

Tässä osassa esitellään, miten käyttää kolmen tietomallin regression tehtävän, korrelaatio Vihje summa maksettu taksin matkan Vihje muiden ominaisuuksien perusteella. Esittää mallit ovat:

- Haltijan muuttujan lineaarinen regressio
- Satunnainen metsää
- Liukuvärin lisäyksen puut

Näitä malleja on kuvattu käyttöönotto. Mallien rakentaminen koodiosassa jakautuu vaiheet: 

1. Parametrin omaavien tietojen **malli-koulutus**
2. Arvot ja testaa tietojoukon **mallin arviointi**
3. **Mallin tallentaminen** myöhempää kulutus blob

### <a name="linear-regression-with-sgd"></a>Lineaarisen regressiosuoran SGD kanssa 

Tässä osassa koodi näkyy kouluttaminen lineaarisen regressiosuoran, joka käyttää stochastic liukuvärin laskeutumisnopeus (SGD) optimointi skaalatun ominaisuuksien avulla ja pisteet, laskeminen ja Tallenna malli Azure Blob Storage (WASB).

>[AZURE.TIP] Valitse Microsoftin kokemus voi olla ongelmia sekä LinearRegressionWithSGD mallit ja parametrit on oltava muuttaa/optimoitu huolellisesti saamiseksi kelvollinen malli. Lähentää helpottaa skaalaus muuttujien merkittävästi. 


    #PREDICT TIP AMOUNTS USING LINEAR REGRESSION WITH SGD

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel
    from pyspark.mllib.evaluation import RegressionMetrics
    from scipy import stats
    
    # USE SCALED FEATURES TO TRAIN MODEL
    linearModel = LinearRegressionWithSGD.train(oneHotTRAINregScaled, iterations=100, step = 0.1, regType='l2', regParam=0.1, intercept = True)

    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(linearModel.weights))
    print("Intercept: " + str(linearModel.intercept))
    
    # SCORE ON SCALED TEST DATA-SET & EVALUATE
    predictionAndLabels = oneHotTESTregScaled.map(lambda lp: (float(linearModel.predict(lp.features)), lp.label))
    testMetrics = RegressionMetrics(predictionAndLabels)
    
    # PRINT TEST METRICS
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # SAVE MODEL WITH DATE-STAMP IN THE DEFAULT BLOB FOR THE CLUSTER
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = modelDir + linearregressionfilename;
    
    linearModel.save(sc, dirfilename)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**TULOS:**

Kerroin: [0.00457675809917,-0.0226314167349,-0.0191910355236, 0.246793409578, 0.312047890459, 0.359634405999, 0.00928692253981,-0.000987181489428,-0.0888306617845, 0.0569376211553, 0.115519551711, 0.149250164995,-0.00990211159703,-0.00637410344522, 0.545083566179,-0.536756072402, 0.0105762393099,-0.0130117577055, 0.0129304737772,-0.00171065945959]

Leikkauspiste-: 0.853872718283

RMSE = 1.24190115863

R sqr = 0.608017146081

Solun yläpuolella olevan suorittamiseen aika: 58.42 sekuntia


### <a name="random-forest-regression"></a>Satunnainen metsää regressio

Koodin tässä osassa näytetään, miten kouluttaminen ja arvioi olevan satunnaisen metsää regression ennustaa Vihje summa Kokousesitelmän taksin työmatkan tietojen tallentaminen.


    #PREDICT TIP AMOUNTS USING RANDOM FOREST

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics
    
    
    ## TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=25, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=10, maxBins=32)
    ## UN-COMMENT IF YOU WANT TO PRING TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())
    
    ## PREDICT AND EVALUATE ON TEST DATA-SET
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = oneHotTESTreg.map(lambda lp: lp.label).zip(predictions)

    # TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfregressionfilename = "RandomForestRegression_" + datestamp;
    dirfilename = modelDir + rfregressionfilename;
    
    rfModel.save(sc, dirfilename);
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**TULOS:**

RMSE = 0.891209218139

R sqr = 0.759661334921

Solun yläpuolella olevan suorittamiseen aika: 49.21 sekuntia


### <a name="gradient-boosting-trees-regression"></a>Liukuvärin lisäyksen puut regressio

Koodin tässä osassa näytetään, miten kouluttaminen ja arvioi liukuvärin lisäyksen puut mallin, joka ennustaa Vihje summa Kokousesitelmän taksin työmatkan tietojen tallentaminen.

**Kouluttaminen ja arvioiminen**

    #PREDICT TIP AMOUNTS USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart= datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    from pyspark.mllib.util import MLUtils
    
    ## TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    gbtModel = GradientBoostedTrees.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo, 
                                                    numIterations=10, maxBins=32, maxDepth = 4, learningRate=0.1)
    
    ## EVALUATE A TEST DATA-SET
    predictions = gbtModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)

    # TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp;
    dirfilename = modelDir + btregressionfilename;
    gbtModel.save(sc, dirfilename)
    
    # CONVER RESULTS TO DF AND REGISER TEMP TABLE
    test_predictions = sqlContext.createDataFrame(predictionAndLabels)
    test_predictions.registerTempTable("tmp_results");
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**TULOS:**

RMSE = 0.908473148639

R sqr = 0.753835096681

Solun yläpuolella olevan suorittamiseen aika: 34.52 sekuntia

**Esitystapa.**

*tmp_results* on rekisteröity rakennetaulukko edellisen solun. Taulukon tulokset ovat tulosteen *sqlResults* tiedot-kehykseen tulostamisessa. Näin koodi

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT * from tmp_results

Seuraavassa on koodin piirtämiseen Jupyter palvelimen avulla.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    import numpy as np

    # PLOT 
    ax = test_predictions_pddf.plot(kind='scatter', figsize = (6,6), x='_1', y='_2', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(test_predictions_pddf['_1'], test_predictions_pddf['_2'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    ax.plot(test_predictions_pddf['_1'], fit[0] * test_predictions_pddf['_1'] + fit[1], color='magenta')
    plt.axis([-1, 20, -1, 20])
    plt.show(ax)
    

**TULOS:**

![Todellinen-ja-ennustettujen-Vihje-summat](./media/machine-learning-data-science-spark-data-exploration-modeling/actual-vs-predicted-tips.png)

    
## <a name="clean-up-objects-from-memory"></a>Järjestä objektit muistia

Käytä `unpersist()` Poista välimuistissa objektit.
        
    # REMOVE ORIGINAL DFs
    taxi_df_train_cleaned.unpersist()
    taxi_df_train_with_newFeatures.unpersist()
    
    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary.unpersist()
    indexedTESTbinary.unpersist()
    oneHotTRAINbinary.unpersist()
    oneHotTESTbinary.unpersist()
    
    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg.unpersist()
    indexedTESTreg.unpersist()
    oneHotTRAINreg.unpersist()
    oneHotTESTreg.unpersist()
    
    # SCALED FEATURES
    oneHotTRAINregScaled.unpersist()
    oneHotTESTregScaled.unpersist()


## <a name="record-storage-locations-of-the-models-for-consumption-and-scoring"></a>Tietueen tallennuspaikkojen mallit kulutus ja näkyvissä pistemäärä

Kuluttavat ja sija riippumaton tietojoukko, joka on kuvattu [pisteet ja arvioi ohjattu laadittuihin koneen learning mallit](machine-learning-data-science-spark-model-consumption.md) ohjeaiheessa haluat kopioida ja liittää näitä nimiä, jotka sisältävät luoda tähän kulutus Jupyter muistikirjaan tallennettujen mallien. Seuraavassa on koodin tulostettavaksi polut mallitiedostot, sinun on olemassa.

    # MODEL FILE LOCATIONS FOR CONSUMPTION
    print "logisticRegFileLoc = modelDir + \"" + logisticregressionfilename + "\"";
    print "linearRegFileLoc = modelDir + \"" + linearregressionfilename + "\"";
    print "randomForestClassificationFileLoc = modelDir + \"" + rfclassificationfilename + "\"";
    print "randomForestRegFileLoc = modelDir + \"" + rfregressionfilename + "\"";
    print "BoostedTreeClassificationFileLoc = modelDir + \"" + btclassificationfilename + "\"";
    print "BoostedTreeRegressionFileLoc = modelDir + \"" + btregressionfilename + "\"";


**TULOS**

logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-05-0317_03_23.516568"

linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-05-0317_05_21.577773"

randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-05-0317_04_11.950206"

randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-05-0317_06_08.723736"

BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-05-0317_04_36.346583"

BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-05-0317_06_51.737282"


## <a name="whats-next"></a>Mitä seuraavaksi?

Nyt kun olet luonut regression ja luokittelu mallien ohjattu MlLib, olet valmis lisätietoja pisteet ja arvioida näitä malleja. Lisäasetukset tietojen tarkasteluun ja mallinnus muistikirjan tarkempaa virtualisointiratkaisuihin rajat vahvistusta, hyper-parametrin sweeping, mukaan lukien ja mallin arviointi. 

**Malli kulutus:** Lisätietoja pisteet ja arvioi luokittelu ja regression mallit luodaan tämän artikkelin on artikkelissa [pisteet ja arvioi ohjattu laadittuihin koneen learning mallien](machine-learning-data-science-spark-model-consumption.md).

**Usean kelpoisuustarkistus ja hyperparameter sweeping**: miten malleja voi nähdä [Advanced tietojen tarkasteluun ja arvioiminen ohjattu](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) koulutus rajat kelpoisuustarkistus ja hyper-parametrin sweeping avulla



