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


# <a name="build-machine-learning-applications-to-run-on-apache-spark-clusters-on-hdinsight-linux"></a>Koneen Learning sovelluksia voidaan suorittaa Apache ohjattu klustereiden HDInsight Linux

Tietoja muodosta konepohjaisten oppimistekniikoiden-sovelluksesta valitsemalla Apache Ohjattu-klusterin Hdinsightista. Tässä artikkelissa kerrotaan käytettävissä Jupyter muistikirjan käyttäminen klusterin voivat laatia ja testaa tämän sovelluksen. Sovellus käyttää HVAC.csv esimerkkitiedot, joka on käytettävissä kaikkien klustereiden oletusarvoisesti.

**Edellytykset:**

Tarvitset seuraavat:

- Azure tilaus. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Apache Ohjattu klusterin HDInsight Linux. Ohjeita on artikkelissa [Azure Hdinsightiin luominen Apache ohjattu varausyksiköt](hdinsight-apache-spark-jupyter-spark-sql.md). 

##<a name="data"></a>Näytä tiedot

Ennen kuin aluksi sovelluksen luomiseen, uutiskirjeistä ymmärtää tiedot ja laji on hyötyä tiedoista analyysin rakenteen. 

Tässä artikkelissa Käytämme otoksen **HVAC.csv** tiedosto, joka on käytettävissä, HDInsight-klusterin liittyvät Azure-tallennustilan tilin. Tallennustilan tilin tiedosto on **\HdiSamples\HdiSamples\SensorSampleData\hvac**. Lataa ja avaa saat tilannevedoksen tiedot CSV-tiedosto.  

![LVI-tietojen tilannevedos] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.ml.show.data.png "LVI-tietojen tilannevedos")

Tiedot näkyvät kohde-lämpötilan ja rakennuksen, joka on asennettu LVI-järjestelmiä todellinen lämpötilan. Oletetaan, että **Järjestelmä** -sarakkeen edustaa Järjestelmätunnus ja **SystemAge** sarakkeen LVI-järjestelmä on nyt rakennuksen tapahtuu vuosien määrä.

Käytämme näitä tietoja ennustaa onko rakennuksen on hotter tai colder perustuvat kohde-lämpötilan määritetyn Järjestelmätunnus ja järjestelmän ikä.

##<a name="app"></a>Kirjoita käyttämällä Ohjattu MLlib machine learning-sovellus

Tämän sovelluksen Käytämme ohjattu ML putkijohto suorittamiseen asiakirjan luokitus. Putkijohto on jaettu asiakirja sanoja, sanoja muuntaa numeeriset ominaisuus vektorista sekä luomalla lopuksi tekstinsyöttö mallin ominaisuus vektorit ja otsikot. Seuraavien toimien sovelluksen luomiseen.

1. [Azure-portaaliin](https://portal.azure.com/)ja valitse startboard Napsauta Ohjattu klusterin ruutua (jos se kiinnitettyinä startboard). Voit myös siirtyä yhteyttä klusterin-kohdassa **Selaa kaikki** > **HDInsight klustereiden**.   

2. Ohjattu klusterin-sivu **Klusterin Raporttinäkymät-ikkunan**ja valitse sitten **Jupyter muistikirja**. Kirjoita pyydettäessä klusterin järjestelmänvalvojan tunnistetietoja.

    > [AZURE.NOTE] Voi myös saavuttaa Jupyter muistikirjan yhteyttä klusterin avaamalla seuraava URL-osoite selaimen. Korvaa __CLUSTERNAME__ yhteyttä klusterin nimen:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Luo uusi muistikirja. Valitse **Uusi**ja valitse sitten **PySpark**.

    ![Luo uusi muistikirja Jupyter] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.note.jupyter.createnotebook.png "Luo uusi muistikirja Jupyter")

3. Uusi muistikirja luodaan ja avataan nimellä Untitled.pynb. Valitse yläreunassa muistikirjan nimi ja kirjoita kutsumanimi.

    ![Anna muistikirjalle nimi] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.note.jupyter.notebook.name.png "Anna muistikirjalle nimi")

3. Koska olet luonut muistikirjan PySpark ydin, sinun ei tarvitse luoda minkä tahansa kontekstit erikseen. Ohjattu ja rakenne-kontekstit luodaan automaattisesti, kun suoritat koodin ensimmäiseen soluun. Voit aloittaa tuomalla tiedostotyypit, joita tarvitaan Tämä skenaario. Liitä seuraava koodikatkelman tyhjään soluun ja paina näppäinyhdistelmää **VAIHTO + ENTER**. 

        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
        
        import os
        import sys
        from pyspark.sql.types import *
        
        from pyspark.mllib.classification import LogisticRegressionWithSGD
        from pyspark.mllib.regression import LabeledPoint
        from numpy import array
        
        
     
4. On nyt ladata tietoja (hvac.csv), jäsentämään sen ja kouluttaminen mallin sen avulla. Tämän ominaisuuden voit määrittää funktion, joka tarkistaa, onko todellinen lämpötila rakennuksen suurempi kuin kohde-lämpötilan. Jos toteutunut lämpötila on suurempi, rakennus on kuuma, merkitty arvo **1.0**. Jos toteutunut lämpötila on vähemmän, rakennus on kylmän, merkitty arvon **0,0**. 

    Liitä seuraava koodikatkelman tyhjään soluun ja paina **VAIHTO + ENTER**.

        
        # List the structure of data for better understanding. Becuase the data will be
        # loaded as an array, this structure makes it easy to understand what each element
        # in the array corresponds to

        # 0 Date
        # 1 Time
        # 2 TargetTemp
        # 3 ActualTemp
        # 4 System
        # 5 SystemAge
        # 6 BuildingID

        LabeledDocument = Row("BuildingID", "SystemInfo", "label")

        # Define a function that parses the raw CSV file and returns an object of type LabeledDocument
        
        def parseDocument(line):
            values = [str(x) for x in line.split(',')]
            if (values[3] > values[2]):
                hot = 1.0
            else:
                hot = 0.0        
    
            textValue = str(values[4]) + " " + str(values[5])
    
            return LabeledDocument((values[6]), textValue, hot)

        # Load the raw HVAC.csv file, parse it using the function
        data = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        documents = data.filter(lambda s: "Date" not in s).map(parseDocument)
        training = documents.toDF()


5. Määritä ohjattu koneen learning putkijohto, jossa on kolme vaihetta: tokenizer, hashingTF ja lr. Saat lisätietoja siitä, mikä on putkijohto ja miten se toimii <a href="http://spark.apache.org/docs/latest/ml-guide.html#how-it-works" target="_blank">Ohjattu konepohjaisten oppimistekniikoiden myyntijakso</a>.

    Liitä seuraava koodikatkelman tyhjään soluun ja paina **VAIHTO + ENTER**.

        tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
        hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
        lr = LogisticRegression(maxIter=10, regParam=0.01)
        pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])

6. Sovita putkijohto koulutus-tiedostoon. Liitä seuraava koodikatkelman tyhjään soluun ja paina **VAIHTO + ENTER**.

        model = pipeline.fit(training)

7. Tarkista tarkistuspiste asiakirjaa koulutus edistymistä-sovelluksella. Liitä seuraava koodikatkelman tyhjään soluun ja paina **VAIHTO + ENTER**.

        training.show()

    Tämä on annettava tulosteen seuraavankaltaiselta:

        +----------+----------+-----+
        |BuildingID|SystemInfo|label|
        +----------+----------+-----+
        |         4|     13 20|  0.0|
        |        17|      3 20|  0.0|
        |        18|     17 20|  1.0|
        |        15|      2 23|  0.0|
        |         3|      16 9|  1.0|
        |         4|     13 28|  0.0|
        |         2|     12 24|  0.0|
        |        16|     20 26|  1.0|
        |         9|      16 9|  1.0|
        |        12|       6 5|  0.0|
        |        15|     10 17|  1.0|
        |         7|      2 11|  0.0|
        |        15|      14 2|  1.0|
        |         6|       3 2|  0.0|
        |        20|     19 22|  0.0|
        |         8|     19 11|  0.0|
        |         6|      15 7|  0.0|
        |        13|      12 5|  0.0|
        |         4|      8 22|  0.0|
        |         7|      17 5|  0.0|
        +----------+----------+-----+


    Siirry takaisin ja tarkista tulos vastaan raaka CSV-tiedostoon. Esimerkiksi CSV-tiedoston ensimmäisen rivin on nämä tiedot:

    ![LVI-tietojen tilannevedos] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.ml.show.data.first.row.png "LVI-tietojen tilannevedos")

    Huomaa, miten todellinen lämpötila on pienempi kuin kohde-lämpötilan-ehdottaminen, rakennus on kylmän. Näin ollen koulutus-tulostus ensimmäisen rivin **otsikko** arvo on **0,0**, mikä tarkoittaa sitä, rakennus ei ole kuuma.

8.  Valmistele tietojoukon suorittaa koulutetun mallin vastaan. Voit tehdä on hyväksytty Järjestelmätunnus ja järjestelmän ikä (merkitty kuin **SystemInfo** koulutus tulosteesta) ja mallin ennustaa onko kyseisen Järjestelmätunnus ja järjestelmän ikä rakennuksen olisi hotter (merkitty 1.0) tai jäähdytin (merkitty 0,0).

    Liitä seuraava koodikatkelman tyhjään soluun ja paina **VAIHTO + ENTER**.
        
        # SystemInfo here is a combination of system ID followed by system age
        Document = Row("id", "SystemInfo")
        test = sc.parallelize([(1L, "20 25"),
                      (2L, "4 15"),
                      (3L, "16 9"),
                      (4L, "9 22"),
                      (5L, "17 10"),
                      (6L, "7 22")]) \
            .map(lambda x: Document(*x)).toDF() 

9. Lopuksi voit tehdä ennusteiden testi-tietoihin. Liitä seuraava koodikatkelman tyhjään soluun ja paina **VAIHTO + ENTER**.

        # Make predictions on test documents and print columns of interest
        prediction = model.transform(test)
        selected = prediction.select("SystemInfo", "prediction", "probability")
        for row in selected.collect():
            print row

10. Näyttöön tulee tulos seuraavankaltaiselta:

        Row(SystemInfo=u'20 25', prediction=1.0, probability=DenseVector([0.4999, 0.5001]))
        Row(SystemInfo=u'4 15', prediction=0.0, probability=DenseVector([0.5016, 0.4984]))
        Row(SystemInfo=u'16 9', prediction=1.0, probability=DenseVector([0.4785, 0.5215]))
        Row(SystemInfo=u'9 22', prediction=1.0, probability=DenseVector([0.4549, 0.5451]))
        Row(SystemInfo=u'17 10', prediction=1.0, probability=DenseVector([0.4925, 0.5075]))
        Row(SystemInfo=u'7 22', prediction=0.0, probability=DenseVector([0.5015, 0.4985]))

    Tekstinsyöttö ensimmäisen rivin, näet tunnus 20 ja 25 vuotta järjestelmän iän LVI-järjestelmä, rakennus olevan pikanäppäimen (**tekstinsyöttö = 1.0**). DenseVector (0.49999) ensimmäinen arvo vastaa tekstinsyöttö 0,0 ja toisen arvon (0.5001) vastaa tekstinsyöttö 1.0. Tuloksessa, vaikka toinen arvo on vain erosi suurempi malli näkyy **tekstinsyöttö = 1.0**.

11. Sen jälkeen, kun olet sovelluksen käytössä, sinun tulee Sammuta muistikirjan Vapauta resurssit. Muistikirjan **Tiedosto** -valikon kiellä valitsemalla **Sulje ja Pysäytä**. Tämä on sulkeminen ja Sulje muistikirja.
           

##<a name="anaconda"></a>Käytä Anaconda scikit-kirjaston tietoja tietokoneen Learning varten

Apache ohjattu klustereiden HDInsight-sisältävät Anaconda kirjastoihin. Tämä sisältää myös **scikit-tietoja** tietokoneen learning kirjasto. Kirjaston myös eri tietojoukkojen, joiden avulla voit luoda otoksen sovelluksia suoraan Jupyter muistikirjan. Esimerkkejä käyttämisestä scikit-kirjaston tietoja, katso [http://scikit-learn.org/stable/auto_examples/index.html](http://scikit-learn.org/stable/auto_examples/index.html).

##<a name="seealso"></a>Katso myös

* [Yleistä: Apache ohjattu-Azure Hdinsightiin](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Skenaariot

* [Ohjattu BI: vuorovaikutteinen tietojen analysoinnissa ohjattu käyttäminen HDInsight kanssa Liiketoimintatieto-työkaluista](hdinsight-apache-spark-use-bi-tools.md)

* [Ohjattu koneen Learning kanssa: Käytä ohjattu elintarvikkeiden tulokset ennustetaan HDInsight-](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

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


[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-weblogs-sample]: hdinsight-hive-analyze-website-log.md
[hdinsight-sensor-data-sample]: hdinsight-hive-analyze-sensor-data.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md
