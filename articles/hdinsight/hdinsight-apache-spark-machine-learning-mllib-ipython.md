<properties 
    pageTitle="Koneen learning sovellusten HDInsight luonnissa käytettävien Apache ohjattu | Microsoft Azure" 
    description="Vaiheittaiset ohjeet muistikirjojen käyttäminen Apache ohjattu luonnissa koneen learning sovellukset" 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/05/2016" 
    ms.author="nitinme"/>


# <a name="machine-learning-predictive-analysis-on-food-inspection-data-using-mllib-with-apache-spark-cluster-on-hdinsight-linux"></a>Koneen learning: tietojen MLlib käyttäminen Apache Ohjattu klusterin HDInsight Linux elintarvikkeiden tarkastus ennakoivan analyysi

> [AZURE.TIP] Tässä opetusohjelmassa on myös käytettävissä Jupyter muistikirjan, jonka luot HDInsight klusterissa ohjattu (Linux). Muistikirjan kokemus voit suorittaa Python katkelmat muistikirjan itse. Suoritettava opetusohjelman muistikirjan luominen Ohjattu klusterin, Jupyter muistikirjan avaaminen (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), ja suorita sitten **Ohjattu koneen oppiminen: ennakoivan analyyseistä käsitellessäsi elintarvikkeiden tarkastus tiedot käyttämällä MLLib.ipynb** muistikirjan **Python** -kansiossa.


Tässä artikkelissa kerrotaan, miten **MLLib**, ohjattu 's valmiin konepohjaisten oppimistekniikoiden kirjastojen avulla voit suorittaa yksinkertaisen ennakoivan analyysin Avaa tietojoukko. MLLib on core ohjattu kirjastoon, joka sisältää useita apuohjelmia, jotka ovat käteviä, kun tietokoneen learning tehtäviä, kuten apuohjelmia, jotka soveltuvat:

* Luokitus

* LOGREGR-funktio

* Klusterointi

* Aiheen mallinnus

* Yksittäinen arvo hajotus (SVD) ja lainan osa analysointi (KYS)

* Hypoteesien testaaminen ja otoksen arvojen laskeminen

Tässä artikkelissa esitellään yksinkertainen lähestymistapa *luokitus* logistista regressiota kautta.

## <a name="what-are-classification-and-logistic-regression"></a>Mitä luokittelu ja logistista regressiota?

*Luokitus*, hyvin yleisiä konepohjaisten oppimistekniikoiden tehtävän on lajitteleminen syöttötiedot luokkiin. Työn luokitus-algoritmin määrittäminen "otsikot" syöttämään tietoja, jotka saadaan laskemisesta on. Esimerkiksi voi ajatella koneen learning-algoritmin, joka hyväksyy pörssikaavioiden tietojen syötteenä ja jakaa osakkeen kahteen luokkaan: varastoja, joiden pitäisi myydä ja varastoja, joiden tulisi säilyttää.

Logistista regressiota on algoritmin, jota käytät luokitus. Ohjattu 's logistista regressiota Ohjelmointirajapinnan on hyötyä *binaarinen luokitus*tai luokittelussa syöttötiedot yhteen kaksi ryhmää. Saat lisätietoja logistista muutoksista [Wikipedia](https://en.wikipedia.org/wiki/Logistic_regression).

Yhteenveto-prosessi, jossa logistista regressiota tuottaa *logistista funktion* , joka voidaan ennustaa todennäköisyyden sille, että syötteen vector kuuluu ryhmä tai toinen.  

## <a name="what-are-we-trying-to-accomplish-in-this-article"></a>Mitä, emme yrität suorittaa tämän artikkelin?

Ohjattu käytetään joissakin ennakoivan analysoida elintarvikkeiden tarkastus tietojen (**Food_Inspections1.csv**), joka on hankittu [Chicago Kaupunki tietojen portaalissa](https://data.cityofchicago.org/)kautta. Tämä tietojoukko sisältää tietoja ruoka-tarkastukset, jotka on suoritettava Chicago, mukaan lukien kunkin ruoka perustaminen, joka on tarkastettu tietoja, virheitä, löytyi (jos saatavilla) ja tarkastuksen tulokset. CSV-datatiedosto on jo käytettävissä osoitteessa **/HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv**klusterin liittyvää tallennustilan tiliä.

Alla olevia ohjeita voit kehittää näet, mitä kestää niin välittää tai epäonnistua elintarvikkeiden tarkastus malli. 

## <a name="start-building-a-machine-learning-application-using-spark-mllib"></a>Aloittaa muodostamisen tietokoneen learning-sovellus, joka käyttää Ohjattu MLlib

1. [Azure-portaaliin](https://portal.azure.com/)ja valitse startboard Napsauta Ohjattu klusterin ruutua (jos se kiinnitettyinä startboard). Voit myös siirtyä yhteyttä klusterin-kohdassa **Selaa kaikki** > **HDInsight klustereiden**.   

2. Ohjattu klusterin-sivu **Klusterin Raporttinäkymät-ikkunan**ja valitse sitten **Jupyter muistikirja**. Kirjoita pyydettäessä klusterin järjestelmänvalvojan tunnistetietoja.

    > [AZURE.NOTE] Voi myös saavuttaa Jupyter muistikirjan yhteyttä klusterin avaamalla seuraava URL-osoite selaimen. Korvaa __CLUSTERNAME__ yhteyttä klusterin nimen:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Luo uusi muistikirja. Valitse **Uusi**ja valitse sitten **PySpark**.

    ![Luo uusi muistikirja Jupyter] (./media/hdinsight-apache-spark-machine-learning-mllib-ipython/hdispark.note.jupyter.createnotebook.png "Luo uusi muistikirja Jupyter")

3. Uusi muistikirja luodaan ja avataan nimellä Untitled.pynb. Valitse yläreunassa muistikirjan nimi ja kirjoita kutsumanimi.

    ![Anna muistikirjalle nimi] (./media/hdinsight-apache-spark-machine-learning-mllib-ipython/hdispark.note.jupyter.notebook.name.png "Anna muistikirjalle nimi")

3. Koska olet luonut muistikirjan PySpark ydin, sinun ei tarvitse luoda minkä tahansa kontekstit erikseen. Ohjattu ja rakenne-kontekstit luodaan automaattisesti, kun suoritat koodin ensimmäiseen soluun. Voit aloittaa muodostaminen tietokoneen Koulujen sovelluksen tuomalla, joita tarvitaan Tämä skenaario. Kiellä, Aseta kohdistin soluun ja paina näppäinyhdistelmää **VAIHTO + ENTER**.


        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
        from pyspark.sql.functions import UserDefinedFunction
        from pyspark.sql.types import *

## <a name="construct-an-input-dataframe"></a>Käyttää syötteen dataframe

Microsoft voi käyttää `sqlContext` tekemässä muunnoksia jäsenneltyjen tietojen. Ensimmäinen tehtävä on Lataa ohjattu SQL- *dataframe*mallitiedot ((**Food_Inspections1.csv**)). 

1. Koska perustietoja on CSV-muodossa, annettava ohjattu yhteydessä avulla voit jakaa tiedoston jokaisen rivin muistiin rakenteeton tekstinä Tämän jälkeen käytät Python's CSV-kirjaston jäsentää yksitellen kullekin riville. 


        def csvParse(s):
            import csv
            from StringIO import StringIO
            sio = StringIO(s)
            value = csv.reader(sio).next()
            sio.close()
            return value
        
        inspections = sc.textFile('wasbs:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv')\
                        .map(csvParse)


2. CSV-tiedosto on nyt kuin RDD. Anna meidän noutaa yhden rivin RDD ymmärtää tietojen rakenne.


        inspections.take(1)


    Pitäisi näkyä tulos seuraavalla tavalla:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [['413707',
          'LUNA PARK INC',
          'LUNA PARK  DAY CARE',
          '2049789',
          "Children's Services Facility",
          'Risk 1 (High)',
          '3250 W FOSTER AVE ',
          'CHICAGO',
          'IL',
          '60625',
          '09/21/2010',
          'License-Task Force',
          'Fail',
          '24. DISH WASHING FACILITIES: PROPERLY DESIGNED, CONSTRUCTED, MAINTAINED, INSTALLED, LOCATED AND OPERATED - Comments: All dishwashing machines must be of a type that complies with all requirements of the plumbing section of the Municipal Code of Chicago and Rules and Regulation of the Board of Health. OBSEVERD THE 3 COMPARTMENT SINK BACKING UP INTO THE 1ST AND 2ND COMPARTMENT WITH CLEAR WATER AND SLOWLY DRAINING OUT. INST NEED HAVE IT REPAIR. CITATION ISSUED, SERIOUS VIOLATION 7-38-030 H000062369-10 COURT DATE 10-28-10 TIME 1 P.M. ROOM 107 400 W. SURPERIOR. | 36. LIGHTING: REQUIRED MINIMUM FOOT-CANDLES OF LIGHT PROVIDED, FIXTURES SHIELDED - Comments: Shielding to protect against broken glass falling into food shall be provided for all artificial lighting sources in preparation, service, and display facilities. LIGHT SHIELD ARE MISSING UNDER HOOD OF  COOKING EQUIPMENT AND NEED TO REPLACE LIGHT UNDER UNIT. 4 LIGHTS ARE OUT IN THE REAR CHILDREN AREA,IN THE KINDERGARDEN CLASS ROOM. 2 LIGHT ARE OUT EAST REAR, LIGHT FRONT WEST ROOM. NEED TO REPLACE ALL LIGHT THAT ARE NOT WORKING. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: The walls and ceilings shall be in good repair and easily cleaned. MISSING CEILING TILES WITH STAINS IN WEST,EAST, IN FRONT AREA WEST, AND BY THE 15MOS AREA. NEED TO BE REPLACED. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair. SPLASH GUARDED ARE NEEDED BY THE EXPOSED HAND SINK IN THE KITCHEN AREA | 34. FLOORS: CONSTRUCTED PER CODE, CLEANED, GOOD REPAIR, COVING INSTALLED, DUST-LESS CLEANING METHODS USED - Comments: The floors shall be constructed per code, be smooth and easily cleaned, and be kept clean and in good repair. INST NEED TO ELEVATE ALL FOOD ITEMS 6INCH OFF THE FLOOR 6 INCH AWAY FORM WALL.  ',
          '41.97583445690982',
          '-87.7107455232781',
          '(41.97583445690982, -87.7107455232781)']]


3. Yllä tulosteen antaa us verrata lähdetiedoston; rakenne tiedosto sisältää nimen jokaisen toimipaikan, perustaminen, osoite, tarkastukset ja sijainnin, muun muassa tietojen tyypin. Valitse muutama sarake, joka käyttää Microsoftin ennakoivan analyysia varten ja ryhmitellä tulokset dataframe, joita on käyttää väliaikaisen taulukon luominen.


        schema = StructType([
        StructField("id", IntegerType(), False), 
        StructField("name", StringType(), False), 
        StructField("results", StringType(), False), 
        StructField("violations", StringType(), True)])

        df = sqlContext.createDataFrame(inspections.map(lambda l: (int(l[0]), l[1], l[12], l[13])) , schema)
        df.registerTempTable('CountResults')

4. Nyt on *dataframe* `df` , emme voi suorittaa sekä analyysi. On myös soittaa **CountResults**väliaikaisen taulukon. Olemme olet kirjoittanut 4 saraketta dataframe halutut: **tunnus**, **nimi**, **tulokset**ja **virheitä**. 
    
    Aloitetaan pieni otoksen tiedot:

        df.show(5)

    Pitäisi näkyä tulos seuraavalla tavalla:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        +------+--------------------+-------+--------------------+
        |    id|                name|results|          violations|
        +------+--------------------+-------+--------------------+
        |413707|       LUNA PARK INC|   Fail|24. DISH WASHING ...|
        |391234|       CAFE SELMARIE|   Fail|2. FACILITIES TO ...|
        |413751|          MANCHU WOK|   Pass|33. FOOD AND NON-...|
        |413708|BENCHMARK HOSPITA...|   Pass|                    |
        |413722|           JJ BURGER|   Pass|                    |
        +------+--------------------+-------+--------------------+

## <a name="understand-the-data"></a>Tietoja tiedot

1. Aloitetaan käsityksen siitä, tutustu tietojoukko sisältää. Mitkä ovat esimerkiksi **tulokset** -sarakkeen eri arvojen?


        df.select('results').distinct().show()

    
    Pitäisi näkyä tulos seuraavalla tavalla:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
    
        +--------------------+
        |             results|
        +--------------------+
        |                Fail|
        |Business Not Located|
        |                Pass|
        |  Pass w/ Conditions|
        |     Out of Business|
        +--------------------+
    
2. Nopea visualisoinnin Auta meitä syy näiden tulosten jakautumisen tietoja. Microsoft on jo väliaikaisen taulukossa **CountResults**tiedot. Voit suorittaa SQL-kyselyn saat paremman käsityksen siitä, miten tulokset jaetaan, taulukossa.

        %%sql -o countResultsdf
        SELECT results, COUNT(results) AS cnt FROM CountResults GROUP BY results

    `%%sql` Tärkeä perään `-o countResultsdf` varmistaa, että kyselyn tulos on samanlainen paikallisesti Jupyter palvelimessa (yleensä klusterin headnode). Tulos on samanlainen kuin [Pandas](http://pandas.pydata.org/) -dataframe määritetyn nimen **countResultsdf**kanssa.
    
    Pitäisi näkyä tulos seuraavalla tavalla:
    
    ![SQL-kyselyn tuloksiin] (./media/hdinsight-apache-spark-machine-learning-mllib-ipython/query.output.png "SQL-kyselyn tuloksiin")

    Jos haluat lisätietoja `%%sql` tärkeä sekä muita käytettävissä PySpark, ydin magics Katso [ytimet Jupyter muistikirjojen ohjattu HDInsight klustereiden käytettävissä](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels).

3. Voit käyttää myös Matplotlib-kirjaston muodostamiseen tietojen visualisoinnin luomiseen piirto. Mitä on luotava-paikallisesti pysyviä **countResultsdf** dataframe, koska koodikatkelman alettava `%%local` tärkeä. Tällä varmistetaan, että koodin suorittaminen Jupyter palvelimessa paikallisesti.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt
        
        
        labels = countResultsdf['results']
        sizes = countResultsdf['cnt']
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')

    Pitäisi näkyä tulos seuraavalla tavalla:

    ![Tulos tulostus](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/output_13_1.png)


4. Näet, ettei tietoalueessa 5 että tarkastuksen voi olla eri tulokset:
    
    * Ei toimipaikka 
    * Epäonnistuu
    * Välittää
    * PSS ja ehdot, ja
    * Liiketoiminnan ulos 

    Anna meidän kehittää mallin, jotka voivat arvata ruoka-tarkastuksen tulokset annettu virheitä. Koska logistista regressiota binaarinen luokitus-menetelmää, se on järkevää ryhmittämään tietojamme kahteen luokkaan: **epäonnistua** ja **välittää**. "Välittää ja ehdot" on edelleen kerralla, niin kun olemme kouluttaminen mallin, emme olettaa, että kaksi tulokset vastaavat. Tietoja muiden tulokset ("Business ei löydy", "poissa Business") eivät ole hyödyllisiä, joten on poistaa ne koulutus määrittämällä. Tämän pitäisi olla haittaa, koska nämä kahteen luokkaan muodostavat erittäin pienen prosenttiosuuden tulokset silti.

5. Siirry eteenpäin ja muunna Microsoftin aiemmin dataframe uutiskirjeistä (`df`) uusi dataframe, johon jokainen tarkastus esitetään lukuna otsikko virheitä pari kyselyjä. Tässä tapauksessa nimi, `0.0` edustaa epäonnistui, nimi, `1.0` : onnistumisen ja nimi, `-1.0` joitakin tuloksia lisäksi nämä kaksi kuvaa. Olemme uuden tietojen kehyksen laskettaessa suodattaa muut tulokset pois.


        def labelForResults(s):
            if s == 'Fail':
                return 0.0
            elif s == 'Pass w/ Conditions' or s == 'Pass':
                return 1.0
            else:
                return -1.0
        label = UserDefinedFunction(labelForResults, DoubleType())
        labeledData = df.select(label(df.results).alias('label'), df.violations).where('label >= 0')


    Noutaa yhden rivin oletetaan, että näet, miltä se näyttää nimetty tiedot.


        labeledData.take(1)


    Pitäisi näkyä tulos seuraavalla tavalla:
    
        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
    
        [Row(label=0.0, violations=u"41. PREMISES MAINTAINED FREE OF LITTER, UNNECESSARY ARTICLES, CLEANING  EQUIPMENT PROPERLY STORED - Comments: All parts of the food establishment and all parts of the property used in connection with the operation of the establishment shall be kept neat and clean and should not produce any offensive odors.  REMOVE MATTRESS FROM SMALL DUMPSTER. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: The walls and ceilings shall be in good repair and easily cleaned.  REPAIR MISALIGNED DOORS AND DOOR NEAR ELEVATOR.  DETAIL CLEAN BLACK MOLD LIKE SUBSTANCE FROM WALLS BY BOTH DISH MACHINES.  REPAIR OR REMOVE BASEBOARD UNDER DISH MACHINE (LEFT REAR KITCHEN). SEAL ALL GAPS.  REPLACE MILK CRATES USED IN WALK IN COOLERS AND STORAGE AREAS WITH PROPER SHELVING AT LEAST 6' OFF THE FLOOR.  | 38. VENTILATION: ROOMS AND EQUIPMENT VENTED AS REQUIRED: PLUMBING: INSTALLED AND MAINTAINED - Comments: The flow of air discharged from kitchen fans shall always be through a duct to a point above the roofline.  REPAIR BROKEN VENTILATION IN MEN'S AND WOMEN'S WASHROOMS NEXT TO DINING AREA. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair.  REPAIR DAMAGED PLUG ON LEFT SIDE OF 2 COMPARTMENT SINK.  REPAIR SELF CLOSER ON BOTTOM LEFT DOOR OF 4 DOOR PREP UNIT NEXT TO OFFICE.")]


## <a name="create-a-logistic-regression-model-from-the-input-dataframe"></a>Syötteen dataframe logistista regressiota mallin luominen

Tutustu lopullinen tehtävä on nimetty tietojen muuntaminen muotoa, jota voidaan analysoida logistista regressioanalyysi. Logistista regressiota algoritmin syöte on oltava joukko *Otsikko-ominaisuus vector paria*, missä "ominaisuus vektorin" vektorista luvuista, joka edustaa syötteen pistettä jollakin tavalla. Annettava siis voi muuntaa "virheitä"-sarake, jossa on puolirakenteisia ja sisältää paljon tekstimuotoinen, matriisiin reaalilukujen, koneen voi helposti ymmärtää kommentit. 

Yksi vakio konepohjaisten oppimistekniikoiden lähestymistavan käsittelyyn luonnollisen kielen on määrittää eri sanan "hakemisto" ja välittää sitten vektorista Koulujen algoritmin siten, että kunkin indeksin arvo sisältää merkkijonon sanan suhteelliset korkojakso tietokoneeseen. 

MLLib on helppo tapa tehtävän suorittamiseen. Olemme "tokenize" virheitä merkkijonot saat yksittäisiksi sanoiksi kukin merkkijono ensin ja sitten Käytämme `HashingTF` muuntaa kukin ehtojoukko tunnuksia, joka voi välittää sitten logistista regressiota algoritmin muodostaminen mallin ominaisuus vektorista. Olemme Järjestä kaikki nämä vaiheet järjestyksessä "putkijohto".


    tokenizer = Tokenizer(inputCol="violations", outputCol="words")
    hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
    lr = LogisticRegression(maxIter=10, regParam=0.01)
    pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])
    
    model = pipeline.fit(labeledData)


## <a name="evaluate-the-model-on-a-separate-test-dataset"></a>Erillinen testi dataset-mallin arvioiminen

Käytämme on aiemmin luotu *ennustetaan* tietoja tulosten mallin uusi tarkastukset on virheitä, jotka on laskettu perusteella. Olemme koulutus dataset **Food_Inspections1.csv**-malli. Käytä uutiskirjeistä toinen tietojoukko, **Food_Inspections2.csv** *arvioi* voimakkuuden tämän mallin, uudet tiedot. Tämä toinen arvojoukko (**Food_Inspections2.csv**) pitäisi olla jo klusterin liittyvät oletusarvo-tallennustilan säilö.

1. Alla olevassa koodikatkelman Luo uusi dataframe **predictionsDf** , joka sisältää mallin luoma tekstinsyöttö. Koodikatkelman luo myös väliaikaisen taulukon **ennusteiden** perusteella dataframe.


        testData = sc.textFile('wasbs:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections2.csv')\
                 .map(csvParse) \
                 .map(lambda l: (int(l[0]), l[1], l[12], l[13]))
        testDf = sqlContext.createDataFrame(testData, schema).where("results = 'Fail' OR results = 'Pass' OR results = 'Pass w/ Conditions'")
        predictionsDf = model.transform(testDf)
        predictionsDf.registerTempTable('Predictions')
        predictionsDf.columns


    Pitäisi näkyä tulos seuraavalla tavalla:
    
        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
        
        ['id',
         'name',
         'results',
         'violations',
         'words',
         'features',
         'rawPrediction',
         'probability',
         'prediction']

2. Etsi jokin ennusteiden. Suorita tämä koodikatkelman:

        predictionsDf.take(1)

    Näet tekstinsyöttö testi tietojoukon ensimmäisen tietueen.

3. `model.transform()` Menetelmä käyttää samaa muunnos uudet tiedot kanssa samaan rakenteeseen ja siitä, miten voit luokitella tietoja tekstinsyöttö vuoroon. Emme voi tehdä joitakin yksinkertaisia tilastojen käsityksen siitä, miten tarkkoja Microsoftin ennusteiden on:


        numSuccesses = predictionsDf.where("""(prediction = 0 AND results = 'Fail') OR 
                                              (prediction = 1 AND (results = 'Pass' OR 
                                                                   results = 'Pass w/ Conditions'))""").count()
        numInspections = predictionsDf.count()
        
        print "There were", numInspections, "inspections and there were", numSuccesses, "successful predictions"
        print "This is a", str((float(numSuccesses) / float(numInspections)) * 100) + "%", "success rate"

    Tulos näyttää seuraavalta:
    
        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
    
        There were 9315 inspections and there were 8087 successful predictions
        This is a 86.8169618894% success rate


    Ohjattu logistista regressiota käyttäminen antaa us tarkka mallin virheitä kuvaukset englanniksi ja onko annetun business välittää tai epäonnistua elintarvikkeiden tarkastus välisen suhteen. 

## <a name="create-a-visual-representation-of-the-prediction"></a>Luo tekstinsyöttö visuaalisessa muodossa

Olemme muodosta lopullinen visualisoinnin Auta meitä nyt syy tämän testin tulokset. 

1. Aluksi purkamalla eri ennusteiden ja tulokset aiemmin luotu **ennusteiden** tilapäisestä taulukosta. Seuraavat kyselyt erota *true_positive*, *false_positive*, *true_negative*ja *false_negative*tulos. Alla kyselyissä on ottaa visualisoinnin `-q` ja myös tallentaa tulos (käyttämällä `-o`) kuin dataframes, jonka avulla voidaan sitten kanssa `%%local` tärkeä. 

        %%sql -q -o true_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND results = 'Fail'

        %%sql -q -o false_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND (results = 'Pass' OR results = 'Pass w/ Conditions')

        %%sql -q -o true_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND results = 'Fail'

        %%sql -q -o false_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND (results = 'Pass' OR results = 'Pass w/ Conditions') 

2. Lopuksi Luo käyttämällä **Matplotlib**piirron seuraavat koodikatkelman avulla.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt
        
        labels = ['True positive', 'False positive', 'True negative', 'False negative']
        sizes = [true_positive['cnt'], false_positive['cnt'], false_negative['cnt'], true_negative['cnt']]
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')
    
    Raportissa pitäisi näkyä seuraava tulos.
    
    ![Ennakoiva tekstinsyöttö tulostus](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/output_26_1.png)


    Tässä kaaviossa tulos on "positiivinen" viittaa epäonnistui ruoka-tarkastuksen samalla, kun välitetty tarkastus viittaa negatiivinen tulos.

## <a name="shut-down-the-notebook"></a>Sulje muistikirja

Sen jälkeen, kun olet sovelluksen käytössä, sinun tulee Sammuta muistikirjan Vapauta resurssit. Muistikirjan **Tiedosto** -valikon kiellä valitsemalla **Sulje ja Pysäytä**. Tämä on Sammuta ja Sulje muistikirja.


## <a name="seealso"></a>Katso myös


* [Yleistä: Apache ohjattu-Azure Hdinsightiin](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Skenaariot

* [Ohjattu BI: vuorovaikutteinen tietojen analysoinnissa ohjattu käyttäminen HDInsight kanssa Liiketoimintatieto-työkaluista](hdinsight-apache-spark-use-bi-tools.md)

* [Ohjattu koneen Learning kanssa: Käytä ohjattu-HDInsight rakennuksen lämpötilan LVI tietojen analysointiin](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Ohjattu virtautetun median: Käytä ohjattu HDInsight reaaliaikainen streaming sovellusten luomiseen:](hdinsight-apache-spark-eventhub-streaming.md)

* [Sivuston log analyysi ohjattu käyttäminen Hdinsightiin](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Luominen ja suorittaminen sovellukset

* [Luo erillisen-sovelluksen käyttäminen Scala](hdinsight-apache-spark-create-standalone-application.md)

* [Suorita työt etäyhteyden käyttämällä Livy ohjattu klusterissa](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Työkalut ja laajennukset

* [HDInsight Työkalut ‑laajennuksen IntelliJ VERRATA avulla voit luoda ja lähettää ohjattu Scala sovelluksia](hdinsight-apache-spark-intellij-tool-plugin.md)

* [Ohjattu sovellusten virheenkorjaus etäyhteyden HDInsight Työkalut ‑laajennuksen IntelliJ VERRATA avulla](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [Ohjattu klusterin HDInsight-Zeppelin muistikirjojen käyttäminen](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Ytimet käytettävissä Jupyter muistikirjan Ohjattu-klusterin Hdinsightiin](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Ulkoiset pakettien käyttäminen Jupyter muistikirjat](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Asenna tietokoneeseen Jupyter ja muodosta yhteys ohjattu HDInsight-klusterin](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Resurssien hallinta

* [Resurssien Azure Hdinsightiin Apache Ohjattu-klusterin](hdinsight-apache-spark-resource-manager.md)

* [Raita- ja Apache ohjattu töitä klusterin Hdinsightiin](hdinsight-apache-spark-job-debugging.md)
