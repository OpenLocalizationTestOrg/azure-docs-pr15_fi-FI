<properties
    pageTitle="Laajennettu tietojen tarkasteluun ja arvioiminen ohjattu | Microsoft Azure"
    description="HDInsight ohjattu avulla voit tehdä tietojen tarkasteluun ja kouluttaminen binaarinen luokittelu ja regression käyttämällä rajat-kelpoisuustarkistus ja hyperparameter optimointi mallit."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/07/2016"
    ms.author="deguhath;bradsev;gokuma" />

# <a name="advanced-data-exploration-and-modeling-with-spark"></a>Lisäasetukset tietojen tarkasteluun ja ohjattu arvioiminen 

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

Tätä vaiheittaista käyttää HDInsight ohjattu tietojen tarkasteluun ja kouluttaminen binaarinen luokittelu ja regression mallien avulla rajat kelpoisuustarkistus ja hyperparameter optimointia Kokousesitelmän otoksen Taksiliikennepalvelut työmatkan ja kuljetusmaksua 2013 tietojoukko. Se opastaa [Tietojen tiede prosessin](http://aka.ms/datascienceprocess)vaiheet, lopusta loppuun-käyttämällä HDInsight-Ohjattu klusterin käsittelyä varten ja Azure-BLOB tiedot ja mallien tallentamiseen. Prosessin käsitellään ja valitseminen puolestaan Visualisoi Azure-tallennustilan Blob-tuonut tiedot ja valmistelee ennakoivan mallien rakentamiseen tiedot. Python on käytetty koodi ratkaisun ja Näytä asiaa joudutaan. Nämä ovat muodosta tehtävät binaarinen luokittelu ja regression mallinnus ohjattu MLlib-työkalujen avulla. 

- **Binaarinen luokitus** tehtävä on ennustaa riippumatta siitä, onko Vihje maksettu matkan. 
- **Regressiosuoran** tehtävä on ennustaa Vihje Vihje muiden ominaisuuksien perusteella määrää. 

Mallinnus vaiheet sisältää myös esittää, kuinka kouluttaminen ja arvioi Tallenna kukin mallin koodi. Aiheen käsitellään joitakin saman pohjan [tietojen tarkasteluun ja arvioiminen ohjattu](machine-learning-data-science-spark-data-exploration-modeling.md) aiheeksi. Mutta se on "monimutkaisempi", se myös käyttävä rajat vahvistus yhdessä hyperparameter sweeping kouluttaminen optimaalisesti tarkempien luokittelu ja regression mallien avulla. 

**Rajat-vahvistus (KV)** on tekniikka, jonka sopimuksen merkityksellisiin kuinka hyvin koulutus-tunnetut tietojoukolle mallin generalizes ennakoimista, se ei ole harjoitettu tietojoukkoja ominaisuuksia. Yleiset verrata tätä tapaa takana on, että malli on koulutus-tietojoukko tunnetut tietojen näyttämisen ja sitten riippumaton tietojoukko verrataan sen ennusteiden tarkkuutta. Yleisiä käyttöönoton, joka käyttää on jakaa tietojoukon K taitosten tekeminen ja kouluttaminen mallin pyöreän pöydän tavalla kaikissa yhtä taitosten tekeminen lukuun ottamatta. 

**Hyperparameter optimointi** on ongelma valitsemalla learning-algoritmin hyperparameters joukko yleensä yhteydessä optimointi algoritmin suorituskyvyn riippumaton tietojoukon mitta. **Hyperparameters** ovat arvoja, jotka on määritettävä mallin koulutus-toimintosarjan ulkopuolella. Nämä arvot oletuksia saattaa olla vaikutusta joustavuutta ja mallien tarkkuudella. Päätös puut on hyperparameters, esimerkiksi esimerkiksi haluamasi syvyyttä ja puun lehtiä määrä. Tuen Vector koneet (SVMs) edellyttävät määrittäminen luokitteluvirhe seuraamusta termin. 

Yleisiä voi suorittaa hyperparameter optimointi käyttää tässä on ruudukon haun tai **parametrin Puhdista**. Tämä koostuu learning algoritmin hyperparameter tilaa määritettyyn alijoukkoa monipuolisen haku, arvot läpi suorittamiseen. Cross kelpoisuustarkistus voi toimittaa suorituskyvyn mittarin lajittelemiseen ulos optimaalisen ruudukon haun algoritmin tuottamat tulokset. KV käyttää hyperparameter sweeping auttaa raja-ongelmien, kuten overfitting koulutus tiedot mallin niin, että mallin säilyttää resursseja koskevat yleiset tietojoukolle, joista koulutus tiedot on purettu.

Käytämme mallit ovat logistista ja lineaarisen regressiosuoran, satunnainen metsien ja liukuvärin tehosti puita:

- [Lineaarisen regressiosuoran SGD kanssa](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) on Stochastic liukuvärin laskeutumisnopeus (SGD) menetelmän käyttävän lineaarisen regressiosuoran mallin ja optimointiin ja ominaisuus skaalaus ennustaa Vihje summat maksettu. 
- [Logistista regressiota LBFGS kanssa](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) tai "logit" regression on regression malli, jonka avulla voidaan muuttujan ollessa categorical voit tehdä tietojen luokittelu. LBFGS on Newtonin optimointi algoritmin, joka on lähellä käyttämällä muistin rajoitetun Broyden – Fletcher – Goldfarb – Shanno (BFGS) algoritmin ja jota laajasti käytetään tietokoneen learning.
- [Satunnainen metsien](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) ovat yhdistelmäasut päätös puut.  Ne yhdistää useita päätös puut overfitting riskejä voidaan minimoida. Satunnainen metsien käytetään regression ja luokittelu ja voit käsitellä categorical ominaisuudet ja laajennettavissa multiclass luokitus-asetus. Ne eivät edellytä ominaisuus skaalaus ja voivat kerätä ei jonkin verran epälineaarisuutta ja vuorovaikutukset ominaisuus. Satunnainen metsien on yksi eniten onnistuneen konepohjaisten oppimistekniikoiden luokitukseen ja regression mallit.
- [Liukuvärin tehosti puita](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) ovat yhdistelmäasut päätös puut. GBTs kouluttaminen päätös puut muutosten pienentämiseksi tappio-funktiota. GBTs käytetään regression ja luokittelu ja voit käsitellä categorical ominaisuuksia, eivät edellytä ominaisuus skaalaus ja voivat kerätä ei jonkin verran epälineaarisuutta ja ominaisuus vuorovaikutukset. Ne voidaan myös multiclass luokitus-asetusta.

Esimerkkejä KV ja Hyperparameter mallinnus Puhdista näkyvät binaarinen luokitus ongelman. Yksinkertainen esimerkkejä (ilman parametrin objektipinoja) esitetään tärkeimmät artikkelissa regression tehtävät. Mutta lisäyksen vahvistus parametrin Puhdista käyttämällä satunnaisia metsää regression lineaarisen regressiosuoran ja KV joustavasti verkon käyttäminen esitetään myös. **Joustavasti verkon** on haltijan regression menetelmä sovittamiseen lineaarisen regressiosuoran mallit, joka yhdistää lineaarisesti seuraamuksina [vapaa](https://en.wikipedia.org/wiki/Lasso%20%28statistics%29) ja [reunuksen](https://en.wikipedia.org/wiki/Tikhonov_regularization) menetelmistä tason 1 ja tason 2-arvot.   



>[AZURE.NOTE] Vaikka ohjattu MLlib työkalujen on suunniteltu toimimaan on paljon, suhteellisen pieni otoksen (noin 30 megatavua käyttämällä 170K rivien noin 0,1 prosenttia alkuperäisen Kokousesitelmän tietojoukko) käytetään tässä helpottamiseksi. Tässä määritetyn Harjoitus toimii tehokkaasti (noin 10 minuuttia) HDInsight-klusterin 2 työntekijä solmujen kanssa. Sama koodi, pieniä muutoksia voidaan käsitellä suurempi-tietosarjoja tarvittavat tiedot muistiin välimuistiin ja muuttamiseen klusteri muutoksia.


## <a name="prerequisites"></a>Edellytykset

Tarvitset Azure-tili ja luodun HDInsight ohjattu voit tarpeen HDInsight 3.4 ohjattu-1.6 klusterin vaiheiden suorittamiseen. Katso ohjeet [Yleiskatsaus tietojen tiede käyttämällä Ohjattu Azure HDInsight-](machine-learning-data-science-spark-overview.md) täytä näitä vaatimuksia. Ohjeaiheen sisältää myös käytetyt tähän Kokousesitelmän 2013 taksin tiedot ja ohjeita siitä, miten voit suorittaa koodin Ohjattu klusterin Jupyter muistikirjan kuvaus. **PySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb** muistikirjan, joka sisältää tässä ohjeaiheessa MALLIKOODEJA sisältyy [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark).


[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a>Määritys: tallennuspaikkojen, kirjastojen ja valmiiksi määritettyä ohjattu yhteydessä

Ohjattu voi lukea ja kirjoittaa Azure tallennustilan Blob (tunnetaan myös nimellä WASB). Jotta kaikki tiedot tallennetut siellä voidaan käsitellä ohjattu ja tallennettu uudelleen WASB tulokset.

Jos haluat tallentaa WASB mallien tai tiedostot, polku on määritetty oikein. Ohjattu klusterin liitetty oletusarvon säilö voidaan viitata käyttämällä polku alkaa sanalla: "wasb: / / /". Muista sijainneista, joihin viitataan "wasb: / /".

### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>Määrittää hakemiston polut tallennuspaikkojen WASB

Seuraava koodi esimerkki määrittää tietojen voi lukea ja mallin tallennustilan kansio, johon mallin tulos on tallennettu polun sijainnin:

    # SET PATHS TO FILE LOCATIONS: DATA AND MODEL STORAGE

    # LOCATION OF TRAINING DATA
    taxi_train_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv";

    
    # SET THE MODEL STORAGE DIRECTORY PATH 
    # NOTE THAT THE FINAL BACKSLASH IN THE PATH IS NEEDED.
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";

    # PRINT START TIME
    import datetime
    datetime.datetime.now()

**TULOS**

DateTime.DATETIME (2016, 4, 18, 17, 36, 27, 832799)


### <a name="import-libraries"></a>Tuo-kirjastot

Tuoda tarvittavat kirjastojen seuraava koodi:

    # LOAD PYSPARK LIBRARIES
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
- **%%sql -o <variable name>** Suorittaa sqlContext rakenteen kysely. Jos + o-parametri on kulunut, kyselyn tulos on samanlainen %% paikallisen Python yhteydessä kuin Pandas DataFrame.
 

Saat lisätietoja Jupyter muistikirjat ja valmiin ytimet "magics", niiden avulla, katso [ytimet käytettävissä olevien HDInsight HDInsight ohjattu Linux varausyksiköiden muistikirjojen Jupyter](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).


## <a name="data-ingestion-from-public-blob"></a>Tietoja nieltynä julkisen Blob-objektien poistaminen: 

Tietoja tiede ensimmäinen vaihe on ingest lähteistä, jossa se sijaitsee tietojen tarkasteluun ja mallinnusympäristön analysoidaan tietoja. Tässä ympäristössä on ohjattu tämän vaiheittaisen kuvauksen suorittamiseksi. Tässä osassa on koodin sarjan tehtävien suorittamiseen:

- tietoja näyte mallintaa ingest
- Lue syötteen tietojoukossa (tallennettu .tsv-tiedostona)
- Muotoile ja poista tiedot
- Voit luoda ja tallentaa objektit (RDDs tai tietojen kehykset) muisti
- Rekisteröi temp-taulukkona SQL-kontekstissa.

Seuraavassa on tietoja erilaisille koodi.


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
    
    # CACHE & MATERIALIZE DATA-FRAME IN MEMORY. GOING THROUGH AND COUNTING NUMBER OF ROWS MATERIALIZES THE DATA-FRAME IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()
    
    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**TULOS**

Solun yläpuolella olevan suorittamiseen aika: 276.62 sekuntia


## <a name="data-exploration--visualization"></a>Tietojen tarkasteluun ja visualisointi 

Kun tiedot on tuotu ohjattu, tietojen tiede prosessin seuraava vaihe on saada tarkempaa tietoja tietojen tarkasteluun ja visualisoinnin kautta. Tässä osassa tutkia taksin tiedot käyttämällä SQL-kyselyjä ja piirtää kohde muuttujat ja silmämääräisesti mahdollisen ominaisuuksia. Tarkemmin sanottuna on piirretään taajuus matkustaja laskee taksin trips, Vihje summat korkojakso ja miten vihjeitä vaihtelevat maksusumma ja tyypin mukaan.

### <a name="plot-a-histogram-of-passenger-count-frequencies-in-the-sample-of-taxi-trips"></a>Histogrammin matkustaja Laske taajuuksien taksin trips otoksessa

Tämä koodi ja myöhempiä katkelmat kyselyn otoksen ja paikallisen tärkeä tietojen SQL tärkeä avulla.

- **SQL tärkeä (`%%sql`)** HDInsight PySpark ydin tukee helposti tekstiin HiveQL kyselyt sqlContext. (-O muuttujan_nimi) argumentin jatkuu SQL-kyselyn tulos kuin Pandas-DataFrame Jupyter-palvelimeen. Tämä tarkoittaa on käytettävissä paikallisten-tilassa.
- ** `%%local` Tärkeä** voidaan suorittaa koodin paikallisesti Jupyter palvelimessa, joka on HDInsight-klusterin headnode. Yleensä käytetään `%%local` magic yhdessä `%%sql` magic -o käynnistysparametrilla. + O-parametrin jatkuvat paikallisesti SQL-kyselyn tulos ja valitse sitten %% paikallisen tärkeä käynnistää koodikatkelman suorittaa paikallisesti vastaan SQL-kyselyjä, joka on samanlainen paikallisesti tulosteen seuraava määrittäminen

Tulos on automaattisesti näyttää, kun suoritat koodi.

Tämä kysely palauttaa trips matkustaja määrän mukaan. 

    # PLOT FREQUENCY OF PASSENGER COUNTS IN TAXI TRIPS

    # SQL QUERY
    %%sql -q -o sqlResults
    SELECT passenger_count, COUNT(*) as trip_counts FROM taxi_train WHERE passenger_count > 0 and passenger_count < 7 GROUP BY passenger_count


Koodi luo paikalliset tiedot-kehyksen kyselyn tuloksesta ja esittää tietoja. `%%local` Tärkeä Luo paikalliset tiedot-kehys, `sqlResults`, jota voidaan käyttää tulostamisessa matplotlib kanssa. 

>[AZURE.NOTE] Tämä PySpark tärkeä käytetään useita kertoja tämän vaiheittaisen kuvauksen suorittamiseksi. Jos tietojen määrää on suuri, on esimerkki tiedot-kehys, joka mahtuu luominen paikalliseen muistiin.


    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local
    
    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES. 
    # CLICK ON THE TYPE OF PLOT TO BE GENERATED (E.G. LINE, AREA, BAR ETC.)
    sqlResults

Näin koodin esitystapa trips matkustaja laskee mukaan

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline
    
    # PLOT PASSENGER NUMBER VS TRIP COUNTS
    x_labels = sqlResults['passenger_count'].values
    fig = sqlResults[['trip_counts']].plot(kind='bar', facecolor='lightblue')
    fig.set_xticklabels(x_labels)
    fig.set_title('Counts of trips by passenger count')
    fig.set_xlabel('Passenger count in trips')
    fig.set_ylabel('Trip counts')
    plt.show()

**TULOS**

![Taajuus trips matkustaja määrän mukaan](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/frequency-of-trips-by-passenger-count.png)

Voit valita useita erilaisia visualisointeja (taulukko, ympyräkaavio, rivin, alue tai palkin) välillä käyttämällä **tyyppi** -valikkopainikkeet muistikirjan. Palkin piirron on mukana.


### <a name="plot-a-histogram-of-tip-amounts-and-how-tip-amount-varies-by-passenger-count-and-fare-amounts"></a>Lajiteltu histogrammi Vihje summat ja miten Vihje määrä vaihtelee matkustaja Laske- ja maksun luvuilla.

Käytä SQL-kyselyn mallitiedot...
    
    # SQL SQUERY
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

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    
    # TIP BY PAYMENT TYPE AND PASSENGER COUNT
    ax1 = resultsPDDF[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()
    
    # TIP BY PASSENGER COUNT
    ax2 = resultsPDDF.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount ($) by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()
    
    # TIP AMOUNT BY FARE AMOUNT, POINTS ARE SCALED BY PASSENGER COUNT
    ax = resultsPDDF.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(resultsPDDF.passenger_count))
    ax.set_title('Tip amount by Fare amount ($)')
    ax.set_xlabel('Fare Amount')
    ax.set_ylabel('Tip Amount')
    plt.axis([-2, 120, -2, 30])
    plt.show()
    

**TULOS:** 

![Vihje summan jakauma](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-distribution.png)

![Vihje summa matkustaja määrän mukaan](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-by-passenger-count.png)

![Vihje summan mukaan maksun summa](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-by-fare-amount.png)


## <a name="feature-engineering-transformation-and-data-preparation-for-modeling"></a>Tekniset Funktiot, muunnos ja tietojen mallinnus valmistelu ominaisuus

Tässä osassa kuvataan sekä annetaan koodi, joita käytetään tietojen valmisteleminen ML mallinnus käytettäviksi. Se näyttää, miten voit tehdä seuraavat toimet:

- Luo uusi ominaisuus binning tuntia liikenne aika uudelleenkäytettävät
- Indeksi ja valitse kuuman koodata categorical ominaisuudet
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

**TULOS**

126050


### <a name="index-and-one-hot-encode-categorical-features"></a>Indeksi ja yksi kuuman koodata categorical ominaisuudet

Tässä osassa esitellään indeksoida ja koodata mallinnus-Funktiot-syötteestä categorical ominaisuuksia. Mallinnus ja ennustaa MLlib funktiot edellyttävät ominaisuuksien categorical syöttötiedot indeksoitu tai koodattu ennen käyttöä. 

Mallista riippuen sinun täytyy indeksoida tai koodata niitä eri tavoilla. Esimerkiksi Logistic ja lineaarisen regressiosuoran mallit edellyttävät yksi kuuman koodaus, jossa, esimerkiksi ominaisuuden, jossa on kolme luokkaa laajennettu kolmeen toiminto-sarakkeeseen, kunkin sisältävän 0 tai 1 Huomautus luokan mukaan. MLlib avulla voit tehdä yhden kuuman koodaus [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) -funktiota. Tämä encoder yhdistää sarakkeen otsikon indeksien binaarinen vektorit arvolla enintään yhden yksi-yhteen sarakkeeseen. Algoritmeista, odottaa numeerisen arvon ominaisuuksia, kuten logistista regressiota categorical ominaisuuksia voi käyttää tätä koodausta avulla.

Näin koodin indeksoida ja koodata categorical ominaisuuksia:


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, OneHotEncoder, VectorIndexer
    
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


**TULOS**

Solun yläpuolella olevan suorittamiseen aika: 3.14 sekuntia


### <a name="create-labeled-point-objects-for-input-into-ml-functions"></a>Luo nimetty pisteen objektit ML Funktiot-syötteestä

Tässä osassa on koodi, joka näyttää, miten indeksoida categorical tekstitietoja nimetty piste-tietotyypiksi ja koodata sen niin, että se voidaan testata MLlib logistista regressiota ja muiden luokitus-mallien ja kouluttaminen. Nimetty pisteen objektit ovat joustavat Distributed tietojoukkoja (RDD) muotoiltu siten, että tarvitaan kuin syöttötiedot ML algoritmit MLlib kaiken irti. [Piste, jossa](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) on paikallinen vector – tiivis tai lyhyet, liittyvä otsikko ja vastaus.

Seuraavassa on indeksoida ja koodata tekstin muotoiluominaisuuksien binaarinen luokituksen koodi.

    # FUNCTIONS FOR BINARY CLASSIFICATION

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from numpy import array

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.pickup_hour, line.weekday,
                             line.passenger_count, line.trip_time_in_secs, line.trip_distance, line.fare_amount])
        labPt = LabeledPoint(line.tipped, features)
        return  labPt
    
    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                   line.vendorVec.toArray(), line.rateVec.toArray(), line.paymentVec.toArray()), axis=0)
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
    
    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    from pyspark.sql.functions import rand
    
    samplingFraction = 0.25;
    trainingFraction = 0.75; testingFraction = (1-trainingFraction);
    seed = 1234;
    encodedFinalSampled = encodedFinal.sample(False, samplingFraction, seed=seed)
    
    # SPLIT SAMPLED DATA-FRAME INTO TRAIN/TEST, WITH A RANDOM COLUMN ADDED FOR DOING CV (SHOWN LATER)
    # INCLUDE RAND COLUMN FOR CREATING CROSS-VALIDATION FOLDS
    dfTmpRand = encodedFinalSampled.select("*", rand(0).alias("rand"));
    trainData, testData = dfTmpRand.randomSplit([trainingFraction, testingFraction], seed=seed);
    
    # CACHE TRAIN AND TEST DATA
    trainData.cache()
    testData.cache()
    
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

**TULOS**

Solun yläpuolella olevan suorittamiseen aika: 0.31 sekuntia


### <a name="feature-scaling"></a>Ominaisuuden skaalaus

Skaalaus ominaisuutta kutsutaan myös tietojen normalisointi vakuutusyhtiö, että ominaisuuksien laajasti maksanut arvot ovat ole antanut liiallinen arvioida objektiivisin-funktiossa. Ominaisuuden skaalauksen koodi käyttää [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) skaalata yksikön varianssin ominaisuuksia. Se tarjoaa MLlib käytettäviksi lineaarisen regressiosuoran kanssa Stochastic liukuvärin laskeutumisnopeus (SGD), Suositut algoritmin, koulutus monien muiden konepohjaisten oppimistekniikoiden malleja, kuten haltijan muutoksista tai tuki vector koneet (SVM).   

>[AZURE.TIP] LinearRegressionWithSGD algoritmin on luottamuksellisia voit korostaa skaalaus löytänyt.   

Seuraavassa on asteikko muuttujat regularized lineaarinen SGD algoritmin käytettäväksi koodi.

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

**TULOS**

Solun yläpuolella olevan suorittamiseen aika: 11.67 sekuntia


### <a name="cache-objects-in-memory"></a>Muistin välimuistin objektit

Aika, koulutusta ja ML algoritmeista testaaminen voidaan vähentää välimuistiin syöttötiedot kehyksen objektien luokitus-regression käytettäviä ja skaalatun ominaisuuksia.

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

**TULOS** 

Solun yläpuolella olevan suorittamiseen aika: 0,13 sekuntia


## <a name="predict-whether-or-not-a-tip-is-paid-with-binary-classification-models"></a>Ennustaa riippumatta siitä, onko Vihje maksetaan binaarinen luokitus-mallit

Tässä osassa esitellään, miten Käytä kolme mallit, korrelaatio binaarinen luokitus-tehtävän Vihje maksetaan taksin matkan vai ei. Esittää mallit ovat:

- Logistista regressiota 
- Satunnainen metsää
- Liukuvärin lisäyksen puut

Mallien rakentaminen koodiosassa jakautuu vaiheet: 

1. Parametrin omaavien tietojen **malli-koulutus**
2. Arvot ja testaa tietojoukon **mallin arviointi**
3. **Mallin tallentaminen** myöhempää kulutus blob

Olemme näyttää, miten rajat-tarkistuksen (KV)-parametrin sweeping kahdella tavalla:

1. Määrittää algoritmin käyttäen **Yleinen** mukautettua koodia, joita voidaan käyttää minkä tahansa MLlib-algoritmin ja kaikki parametrit. 
1. Voit käyttää **pySpark CrossValidator myyntijakso-funktiota**. Huomaa, että vaikka helposti, tutustu kokemuksen perusteella CrossValidator on ohjattu 1.5.0 koskevat seuraavat rajoitukset: 

    - Myyntijakso malleja ei voi tallentaa/samanlainen tulevat käyttöön.
    - Ei voi käyttää mallissa jokaisen parametrin.
    - Jokaisen MLlib algoritmin ei voi käyttää.


### <a name="generic-cross-validation-and-hyperparameter-sweeping-used-with-the-logistic-regression-algorithm-for-binary-classification"></a>Yleinen kelpoisuustarkistus ja hyperparameter sweeping logistista regressiota algoritmin kanssa käytettävä binaarinen luokittelu

Koodin tässä osassa näytetään, miten kouluttaminen ja arvioi logistista regressiota mallin säästäminen [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) , riippumatta siitä, onko Vihje maksettu Kokousesitelmän taksin työmatkan ja maksun tietojoukko matkan ennustaa. Malli on koulutus vahvistus (KV) ja hyperparameter sweeping toteutettu mukautettua koodia, joka voidaan ottaa käyttöön jonkin MLlib learning algoritmit avulla.   

>[AZURE.NOTE] Tämän mukautetun KV-koodin suorittaminen voi kestää useita minuutteja.

**KV ja hyperparameter sweeping logistista regressiota mallin kouluttaminen**

    # LOGISTIC REGRESSION CLASSIFICATION WITH CV AND HYPERPARAMETER SWEEPING

    # GET ACCURACY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionWithLBFGS 
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    
    # CREATE PARAMETER GRID FOR LOGISTIC REGRESSION PARAMETER SWEEP
    from sklearn.grid_search import ParameterGrid
    grid = [{'regParam': [0.01, 0.1], 'iterations': [5, 10], 'regType': ["l1", "l2"], 'tolerance': [1e-3, 1e-4]}]
    paramGrid = list(ParameterGrid(grid))
    numModels = len(paramGrid)
    
    # SET NUM FOLDS AND NUM PARAMETER SETS TO SWEEP ON
    nFolds = 3;
    h = 1.0 / nFolds;
    metricSum = np.zeros(numModels);
    
    # BEGIN CV WITH PARAMETER SWEEP
    for i in range(nFolds):
        # Create training and x-validation sets
        validateLB = i * h
        validateUB = (i + 1) * h
        condition = (trainData["rand"] >= validateLB) & (trainData["rand"] < validateUB)
        validation = trainData.filter(condition)
        # Create LabeledPoints from data-frames
        if i > 0:
            trainCVLabPt.unpersist()
            validationLabPt.unpersist()
        trainCV = trainData.filter(~condition)
        trainCVLabPt = trainCV.map(parseRowOneHotBinary)
        trainCVLabPt.cache()
        validationLabPt = validation.map(parseRowOneHotBinary)
        validationLabPt.cache()
        # For parameter sets compute metrics from x-validation
        for j in range(numModels):
            regt = paramGrid[j]['regType']
            regp = paramGrid[j]['regParam']
            iters = paramGrid[j]['iterations']
            tol = paramGrid[j]['tolerance']
            # Train logistic regression model with hypermarameter set
            model = LogisticRegressionWithLBFGS.train(trainCVLabPt, regType=regt, iterations=iters,  
                                                      regParam=regp, tolerance = tol, intercept=True)
            predictionAndLabels = validationLabPt.map(lambda lp: (float(model.predict(lp.features)), lp.label))
            # Use ROC-AUC as accuracy metrics
            validMetrics = BinaryClassificationMetrics(predictionAndLabels)
            metric = validMetrics.areaUnderROC
            metricSum[j] += metric
    
    avgAcc = metricSum / nFolds;
    bestParam = paramGrid[np.argmax(avgAcc)];
    
    # UNPERSIST OBJECTS
    trainCVLabPt.unpersist()
    validationLabPt.unpersist()
        
    # TRAIN ON FULL TRAIING SET USING BEST PARAMETERS FROM CV/PARAMETER SWEEP
    logitBest = LogisticRegressionWithLBFGS.train(oneHotTRAINbinary, regType=bestParam['regType'], 
                                                  iterations=bestParam['iterations'], 
                                                  regParam=bestParam['regParam'], tolerance = bestParam['tolerance'], 
                                                  intercept=True)
    
    
    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(logitBest.weights))
    print("Intercept: " + str(logitBest.intercept))
    
    # PRINT ELAPSED TIME    
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**TULOS**

Kerroin: [0.0082065285375-0.0223675576104-0.0183812028036, - 3.48124578069e-05-0.00247646947233-0.00165897881503, 0.0675394837328,-0.111823113101,-0.324609912762,-0.204549780032,-1.36499216354, 0.591088507921,-0.664263411392,-1.00439726852, 3.46567827545,-3.51025855172,-0.0471341112232,-0.043521833294, 0.000243375810385, 0.054518719222]

Leikkauspiste:-0.0111216486893

Solun yläpuolella olevan suorittamiseen aika: 14.43 sekuntia


**Vakio arvot kanssa binaarinen luokitus-mallin arvioiminen**

Tässä osassa koodi esitetään, kuinka voit arvioida logistista regressiota mallin vastaan testi-tietojoukon, mukaan lukien piirron OHJETIEDOSTO käyrän.


    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT LIBRARIES
    from sklearn.metrics import roc_curve,auc
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics
    
    # PREDICT ON TEST DATA WITH BEST/FINAL MODEL
    predictionAndLabels = oneHotTESTbinary.map(lambda lp: (float(logitBest.predict(lp.features)), lp.label))
    
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
    
    # OUTPUT PROBABILITIES AND REGISTER TEMP TABLE
    logitBest.clearThreshold(); # This clears threshold for classification (0.5) and outputs probabilities
    predictionAndLabelsDF = predictionAndLabels.toDF()
    predictionAndLabelsDF.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME    
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**TULOS**

Hinta pinta = 0.985336538462

OHJETIEDOSTO pinta = 0.983383274312

Yhteenveto tilasto

Tarkkuus = 0.984174341679

Peruuttaa = 0.984174341679

F1 Sija = 0.984174341679

Solun yläpuolella olevan suorittamiseen aika: 2.67 sekuntia


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
    
    #PREDICTIONS
    predictions_pddf = sqlResults.rename(columns={'_1': 'probability', '_2': 'label'})
    prob = predictions_pddf["probability"] 
    fpr, tpr, thresholds = roc_curve(predictions_pddf['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)
    
    # PLOT ROC CURVES
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
    

**TULOS**

![Yleinen lähestymistapa logistista regressiota OHJETIEDOSTO käyrä](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/logistic-regression-roc-curve.png)


**Pysyvä tulevien kulutus blob-malli**

Tässä osassa koodi näkyy kulutus logistista regressiota mallin tallentaminen.

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionModel
    
    # PERSIST MODEL
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp;
    dirfilename = modelDir + logisticregressionfilename;
    
    logitBest.save(sc, dirfilename);
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";


**TULOS**

Solun yläpuolella olevan suorittamiseen aika: 34.57 sekuntia


### <a name="use-mllibs-crossvalidator-pipeline-function-with-logistic-regression-elastic-regression-model"></a>MLlib's CrossValidator myyntijakso-funktiota käytetään logistista regressiota (joustavasti regression)-malli

Koodin tässä osassa näytetään, miten kouluttaminen ja arvioi logistista regressiota mallin säästäminen [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) , riippumatta siitä, onko Vihje maksettu Kokousesitelmän taksin työmatkan ja maksun tietojoukko matkan ennustaa. Malli on koulutus käyttämällä vahvistus (KV) ja hyperparameter sweeping toteutettu MLlib CrossValidator myyntijakso-funktiolla parametrin Puhdista KV varten.   


>[AZURE.NOTE] Tämä MLlib KV-koodin suorittaminen voi kestää useita minuutteja.


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.classification import LogisticRegression
    from pyspark.ml import Pipeline
    from pyspark.ml.evaluation import BinaryClassificationEvaluator
    from pyspark.ml.tuning import CrossValidator, ParamGridBuilder
    from sklearn.metrics import roc_curve,auc
    
    # DEFINE ALGORITHM / MODEL
    lr = LogisticRegression()
    
    # DEFINE GRID PARAMETERS
    paramGrid = ParamGridBuilder().addGrid(lr.regParam, (0.01, 0.1))\
                                  .addGrid(lr.maxIter, (5, 10))\
                                  .addGrid(lr.tol, (1e-4, 1e-5))\
                                  .addGrid(lr.elasticNetParam, (0.25,0.75))\
                                  .build()
    
    # DEFINE CV WITH PARAMETER SWEEP
    cv = CrossValidator(estimator= lr,
                        estimatorParamMaps=paramGrid,
                        evaluator=BinaryClassificationEvaluator(),
                        numFolds=3)
    
    # CONVERT TO DATA-FRAME: THIS DOES NOT RUN ON RDDs
    trainDataFrame = sqlContext.createDataFrame(oneHotTRAINbinary, ["features", "label"])
    
    # TRAIN WITH CROSS-VALIDATION
    cv_model = cv.fit(trainDataFrame)
    

    ## PREDICT AND EVALUATE ON TEST DATA-SET

    # USE TEST DATASET FOR PREDICTION
    testDataFrame = sqlContext.createDataFrame(oneHotTESTbinary, ["features", "label"])
    test_predictions = cv_model.transform(testDataFrame)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**TULOS**

Solun yläpuolella olevan suorittamiseen aika: 107.98 sekuntia


**Piirrä OHJETIEDOSTO käyrän.**

*PredictionAndLabelsDF* on rekisteröity taulukon *tmp_results*edelliseen soluun. *tmp_results* voidaan tehdä kyselyjä ja tulosta tiedot sqlResults kehykseen tulostamisessa. Seuraavassa on koodi.


    # QUERY RESULTS
    %%sql -q -o sqlResults
    SELECT label, prediction, probability from tmp_results

Seuraavassa on esitystapa OHJETIEDOSTO käyrän koodi.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES 
    %%local
    from sklearn.metrics import roc_curve,auc
    
    # ROC CURVE
    prob = [x["values"][1] for x in sqlResults["probability"]]
    fpr, tpr, thresholds = roc_curve(sqlResults['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)
    
    #PLOT
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


**TULOS**

![Käyttämällä MLlib's CrossValidator logistista regressiota OHJETIEDOSTO käyrä](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/mllib-crossvalidator-roc-curve.png)



### <a name="random-forest-classification"></a>Satunnainen metsää luokittelu

Koodin tässä osassa näytetään, miten kouluttaminen ja arvioi olevan satunnaisen metsää regression ennustaa riippumatta siitä, onko Vihje maksettu Kokousesitelmän taksin työmatkan ja maksun tietojoukko matkan Tallenna.


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
    ## UN-COMMENT IF YOU WANT TO PRING TREES
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


**TULOS**

OHJETIEDOSTO pinta = 0.985336538462

Solun yläpuolella olevan suorittamiseen aika: 26.72 sekuntia


### <a name="gradient-boosting-trees-classification"></a>Liukuvärin lisäyksen puiden luokittelu

Koodin tässä osassa näytetään, miten kouluttaminen, käsitellä, ja Tallenna liukuvärin lisäyksen puut mallin, joka ennustaa riippumatta siitä, onko Vihje maksettu Kokousesitelmän taksin työmatkan matkan ja kuljetusmaksua tietojoukko.


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    
    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    
    gbtModel = GradientBoostedTrees.trainClassifier(indexedTRAINbinary, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                    numIterations=10)
    ## UNCOMMENT IF YOU WANT TO PRINT TREE DETAILS
    #print('Learned classification GBT model:')
    #print(bgtModel.toDebugString())
    
    # PREDICT ON TEST DATA AND EVALUATE
    predictions = gbtModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)
    
    # Area under ROC curve
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

**TULOS**

OHJETIEDOSTO pinta = 0.985336538462

Solun yläpuolella olevan suorittamiseen aika: 28.13 sekuntia


## <a name="predict-tip-amount-with-regression-models-not-using-cv"></a>Vihje summa ennustetaan regression mallit (Käytä KV) kanssa

Tässä osassa esitellään, miten käyttää kolmen tietomallin regression tehtävän, korrelaatio Vihje summa maksettu taksin matkan Vihje muiden ominaisuuksien perusteella. Esittää mallit ovat:

- Haltijan muuttujan lineaarinen regressio
- Satunnainen metsää
- Liukuvärin lisäyksen puut

Näitä malleja on kuvattu käyttöönotto. Mallien rakentaminen koodiosassa jakautuu vaiheet: 

1. Parametrin omaavien tietojen **malli-koulutus**
2. Arvot ja testaa tietojoukon **mallin arviointi**
3. **Mallin tallentaminen** myöhempää kulutus blob   


>AZURE Huomautus: Rajat vahvistus käytetään ei kanssa kolme regression mallien sisältö, koska tämä näkyy yksityiskohtaisesti logistista regressiota mallien. Esimerkki, jossa näkyy voit käyttää KV joustavasti nettonykyarvon lineaarisen regressiosuoran varten on annettu sisältö lisäyksen.


>AZURE Huomautus: Tutustu kokemus voi olla ongelmat lähentää LinearRegressionWithSGD mallit ja parametrit on oltava muuttaa/optimoitu huolellisesti saamiseksi kelvollinen malli. Lähentää helpottaa skaalaus muuttujien merkittävästi. Tässä ohjeaiheessa lisäyksen näkyvät nettonykyarvon joustavasti regression voidaan myös LinearRegressionWithSGD sijaan.


### <a name="linear-regression-with-sgd"></a>Lineaarisen regressiosuoran SGD kanssa

Tässä osassa koodi näkyy kouluttaminen lineaarisen regressiosuoran, joka käyttää stochastic liukuvärin laskeutumisnopeus (SGD) optimointi skaalatun ominaisuuksien avulla ja pisteet, laskeminen ja Tallenna malli Azure Blob Storage (WASB).

>[AZURE.TIP] Valitse Microsoftin kokemus voi olla ongelmia sekä LinearRegressionWithSGD mallit ja parametrit on oltava muuttaa/optimoitu huolellisesti saamiseksi kelvollinen malli. Lähentää helpottaa skaalaus muuttujien merkittävästi.


    # LINEAR REGRESSION WITH SGD 

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
    
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = modelDir + linearregressionfilename;
    
    linearModel.save(sc, dirfilename)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**TULOS**

Kerroin: [0.0141707753435,-0.0252930927087,-0.0231442517137, 0.247070902996, 0.312544147152, 0.360296120645, 0.0122079566092,-0.00456498588241,-0.0898228505177, 0.0714046248793, 0.102171263868, 0.100022455632,-0.00289545676449,-0.00791124681938, 0.54396316518,-0.536293513569, 0.0119076553369,-0.0173039244582, 0.0119632796147, 0.00146764882502]

Leikkauspiste-: 0.854507624459

RMSE = 1.23485131376

R sqr = 0.597963951127

Solun yläpuolella olevan suorittamiseen aika: 38.62 sekuntia


### <a name="random-forest-regression"></a>Satunnainen metsää regressio

Koodin tässä osassa näytetään, miten kouluttaminen ja arvioi satunnaisia metsää mallin, joka ennustaa Vihje summa Kokousesitelmän taksin työmatkan tietojen tallentaminen.   

>[AZURE.NOTE] Parametrin sweeping käyttäen mukautettua koodia rajat-tarkistus on annettu lisäyksen.


    #PREDICT TIP AMOUNTS USING RANDOM FOREST

    # RECORD START TIME
    timestart= datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics
    
    
    # TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=25, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=10, maxBins=32)
    # UN-COMMENT IF YOU WANT TO PRING TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())
    
    # PREDICT AND EVALUATE ON TEST DATA-SET
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = oneHotTESTreg.map(lambda lp: lp.label).zip(predictions)
    
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

**TULOS**

RMSE = 0.931981967875

R sqr = 0.733445485802

Solun yläpuolella olevan suorittamiseen aika: 25.98 sekuntia


### <a name="gradient-boosting-trees-regression"></a>Liukuvärin lisäyksen puut regressio

Koodin tässä osassa näytetään, miten kouluttaminen ja arvioi liukuvärin lisäyksen puut mallin, joka ennustaa Vihje summa Kokousesitelmän taksin työmatkan tietojen tallentaminen.

**Kouluttaminen ja arvioiminen**

    #PREDICT TIP AMOUNTS USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart= datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    from pyspark.mllib.util import MLUtils
    
    # TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    gbtModel = GradientBoostedTrees.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo, 
                                                    numIterations=10, maxBins=32, maxDepth = 4, learningRate=0.1)
    
    # EVALUATE A TEST DATA-SET
    predictions = gbtModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)
    
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES
    test_predictions= sqlContext.createDataFrame(predictionAndLabels)
    test_predictions_pddf = test_predictions.toPandas()
    
    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp;
    dirfilename = modelDir + btregressionfilename;
    gbtModel.save(sc, dirfilename)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**TULOS**

RMSE = 0.928172197114

R sqr = 0.732680354389

Solun yläpuolella olevan suorittamiseen aika: 20.9 sekuntia


**Esitystapa.**
    
*tmp_results* on rekisteröity rakennetaulukko edellisen solun. Taulukon tulokset ovat tulosteen *sqlResults* tiedot-kehykseen tulostamisessa. Näin koodi

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT * from tmp_results


Seuraavassa on koodin piirtämiseen Jupyter palvelimen avulla.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import numpy as np
    
    # PLOT
    ax = sqlResults.plot(kind='scatter', figsize = (6,6), x='_1', y='_2', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(sqlResults['_1'], sqlResults['_2'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    ax.plot(sqlResults['_1'], fit[0] * sqlResults['_1'] + fit[1], color='magenta')
    plt.axis([-1, 15, -1, 15])
    plt.show(ax)

![Todellinen-ja-ennustettujen-Vihje-summat](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/actual-vs-predicted-tips.png)


## <a name="appendix-additional-regression-tasks-using-cross-validation-with-parameter-sweeps"></a>Lisäys: Regressiota tehtävien välisen vahvistus käyttäminen parametrin sweeps

Tämän lisäyksen on KV joustavasti verkon käytön lineaarisen regressiosuoran hallinnasta ja miten KV tehdään parametrin Puhdista mukautetun koodin käytön satunnaisia metsää regression koodi.


### <a name="cross-validation-using-elastic-net-for-linear-regression"></a>Toimintojen välinen kelpoisuuden käyttämällä Lisää joustavasti nettonykyarvon muuttujan lineaarinen regressio

Koodin tässä osassa näytetään, miten ristiksi joustavasti verkon käytön lineaarisen regressiosuoran kelpoisuustarkistus ja arvioimiseen vastaan testitiedot mallin.

    ###  CV USING ELASTIC NET FOR LINEAR REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.regression import LinearRegression
    from pyspark.ml import Pipeline
    from pyspark.ml.evaluation import RegressionEvaluator
    from pyspark.ml.tuning import CrossValidator, ParamGridBuilder
    
    # DEFINE ALGORITHM/MODEL
    lr = LinearRegression()
    
    # DEFINE GRID PARAMETERS
    paramGrid = ParamGridBuilder().addGrid(lr.regParam, (0.01, 0.1))\
                                  .addGrid(lr.maxIter, (5, 10))\
                                  .addGrid(lr.tol, (1e-4, 1e-5))\
                                  .addGrid(lr.elasticNetParam, (0.25,0.75))\
                                  .build() 
    
    # DEFINE PIPELINE 
    # SIMPLY THE MODEL HERE, WITHOUT TRANSFORMATIONS
    pipeline = Pipeline(stages=[lr])
    
    # DEFINE CV WITH PARAMETER SWEEP
    cv = CrossValidator(estimator= lr,
                        estimatorParamMaps=paramGrid,
                        evaluator=RegressionEvaluator(),
                        numFolds=3)
    
    # CONVERT TO DATA FRAME, AS CROSSVALIDATOR WON'T RUN ON RDDS
    trainDataFrame = sqlContext.createDataFrame(oneHotTRAINreg, ["features", "label"])
    
    # TRAIN WITH CROSS-VALIDATION
    cv_model = cv.fit(trainDataFrame)
    

    # EVALUATE MODEL ON TEST SET
    testDataFrame = sqlContext.createDataFrame(oneHotTESTreg, ["features", "label"])
    
    # MAKE PREDICTIONS ON TEST DOCUMENTS
    # cvModel uses the best model found (lrModel).
    predictionAndLabels = cv_model.transform(testDataFrame)
    
    # CONVERT TO DF AND SAVE REGISER DF AS TABLE
    predictionAndLabels.registerTempTable("tmp_results");
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**TULOS**

Solun yläpuolella olevan suorittamiseen aika: 161.21 sekuntia

**Arvioi R SQR metrijärjestelmä kanssa**

*tmp_results* on rekisteröity rakennetaulukko edellisen solun. Taulukon tulokset ovat tulosteen *sqlResults* tiedot-kehykseen tulostamisessa. Näin koodi

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT label,prediction from tmp_results


Seuraavassa on koodin R sqr laskemiseen.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    from scipy import stats
    
    #R-SQR TEST METRIC
    corstats = stats.linregress(sqlResults['label'],sqlResults['prediction'])
    r2 = (corstats[2]*corstats[2])
    print("R-sqr = %s" % r2)


**TULOS**

R sqr = 0.619184907088


### <a name="cross-validation-with-parameter-sweep-using-custom-code-for-random-forest-regression"></a>Toimintojen vahvistus parametrin Puhdista mukautetun koodin käytön satunnaisia metsää regression kanssa

Tässä osassa koodi näkyy miten vahvistus toimintojen kanssa käyttämällä mukautettua koodia satunnaisia metsää regression parametrin Puhdista ja mallin vastaan testitiedot arvioiminen.


    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    # GET ACCURARY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics
    from sklearn.grid_search import ParameterGrid
    
    ## CREATE PARAMETER GRID
    grid = [{'maxDepth': [5,10], 'numTrees': [25,50]}]
    paramGrid = list(ParameterGrid(grid))
    
    ## SPECIFY LEVELS OF CATEGORICAL VARIBLES
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    
    # SPECIFY NUMFOLDS AND ARRAY TO HOLD METRICS
    nFolds = 3;
    numModels = len(paramGrid)
    h = 1.0 / nFolds;
    metricSum = np.zeros(numModels);
    
    for i in range(nFolds):
        # Create training and x-validation sets
        validateLB = i * h
        validateUB = (i + 1) * h
        condition = (trainData["rand"] >= validateLB) & (trainData["rand"] < validateUB)
        validation = trainData.filter(condition)
        # Create labeled points from data-frames
        if i > 0:
            trainCVLabPt.unpersist()
            validationLabPt.unpersist()
        trainCV = trainData.filter(~condition)
        trainCVLabPt = trainCV.map(parseRowIndexingRegression)
        trainCVLabPt.cache()
        validationLabPt = validation.map(parseRowIndexingRegression)
        validationLabPt.cache()
        # For parameter sets compute metrics from x-validation
        for j in range(numModels):
            maxD = paramGrid[j]['maxDepth']
            numT = paramGrid[j]['numTrees']
            # Train logistic regression model with hypermarameter set
            rfModel = RandomForest.trainRegressor(trainCVLabPt, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=numT, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=maxD, maxBins=32)
            predictions = rfModel.predict(validationLabPt.map(lambda x: x.features))
            predictionAndLabels = validationLabPt.map(lambda lp: lp.label).zip(predictions)
            # Use ROC-AUC as accuracy metrics
            validMetrics = RegressionMetrics(predictionAndLabels)
            metric = validMetrics.rootMeanSquaredError
            metricSum[j] += metric
    
    avgAcc = metricSum/nFolds;
    bestParam = paramGrid[np.argmin(avgAcc)];
    
    # UNPERSIST OBJECTS
    trainCVLabPt.unpersist()
    validationLabPt.unpersist()
            
    ## TRAIN FINAL MODL WIHT BEST PARAMETERS
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=bestParam['numTrees'], featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=bestParam['maxDepth'], maxBins=32)

    # EVALUATE MODEL ON TEST DATA
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)
    
    #PRINT TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**TULOS**

RMSE = 0.906972198262

R sqr = 0.740751197012

Solun yläpuolella olevan suorittamiseen aika: 69.17 sekuntia


### <a name="clean-up-objects-from-memory-and-print-model-locations"></a>Järjestä objektit muistin tai Tulosta mallin sijainnista

Käytä `unpersist()` Poista välimuistissa objektit.

    # UNPERSIST OBJECTS CACHED IN MEMORY

    # REMOVE ORIGINAL DFs
    taxi_df_train_cleaned.unpersist()
    taxi_df_train_with_newFeatures.unpersist()
    trainData.unpersist()
    trainData.unpersist()
    
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


**TULOS**

PythonRDD [122] osoitteessa RDD osoitteessa PythonRDD.scala: 43


**Tulosteen polku, jota käytetään kulutus muistikirjan mallitiedostot.** Kuluttavat ja sija riippumaton tiedot – Määritä, tarvitset kopioiminen ja liittäminen näitä tiedostonimiä "kulutus muistikirjan".


    # PRINT MODEL FILE LOCATIONS FOR CONSUMPTION
    print "logisticRegFileLoc = modelDir + \"" + logisticregressionfilename + "\"";
    print "linearRegFileLoc = modelDir + \"" + linearregressionfilename + "\"";
    print "randomForestClassificationFileLoc = modelDir + \"" + rfclassificationfilename + "\"";
    print "randomForestRegFileLoc = modelDir + \"" + rfregressionfilename + "\"";
    print "BoostedTreeClassificationFileLoc = modelDir + \"" + btclassificationfilename + "\"";
    print "BoostedTreeRegressionFileLoc = modelDir + \"" + btregressionfilename + "\"";


**TULOS**

logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-05-0316_47_30.096528"

linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-05-0316_51_28.433670"

randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-05-0316_50_17.454440"

randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-05-0316_51_57.331730"

BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-05-0316_50_40.138809"

BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-05-0316_52_18.827237"

## <a name="whats-next"></a>Mitä seuraavaksi?

Nyt kun olet luonut regression ja luokittelu mallien ohjattu MlLib, olet valmis lisätietoja pisteet ja arvioida näitä malleja.

**Malli kulutus:** Lisätietoja pisteet ja arvioi luokittelu ja regression mallit luodaan tämän artikkelin on artikkelissa [pisteet ja arvioi ohjattu laadittuihin koneen learning mallien](machine-learning-data-science-spark-model-consumption.md).
