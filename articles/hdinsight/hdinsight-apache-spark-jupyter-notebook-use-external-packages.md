<properties 
    pageTitle="Ulkoiset pakettien käyttäminen Jupyter muistikirjat Apache ohjattu klustereissa HDInsight | Azure"
    description="Vaiheittaiset ohjeet ovat määrittäminen Jupyter muistikirjat käytettävissä HDInsight ohjattu klustereiden käyttämään ulkoisen ohjattu paketit." 
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
    ms.date="10/28/2016" 
    ms.author="nitinme"/>


# <a name="use-external-packages-with-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight-linux"></a>Käytä ulkoisia pakettien Jupyter työkirjoja Apache ohjattu klustereissa HDInsight Linux

>[AZURE.NOTE] Tässä artikkelissa on ohjattu 1.5.2 HDInsight 3.3-ja valitse HDInsight 3.4 1.6.2 ohjattu. 

Opettele määrittämään Jupyter muistikirjan Apache Ohjattu-klusterin HDInsight (Linux), käytä ulkoista, yhteisön lähettänyt paketteja, jotka eivät sisälly ulos,-valmiilla klusterin. 

Voit hakea [maven-testi säilöön](http://search.maven.org/) luettelo kaikista paketteja, jotka ovat käytettävissä. Saat luettelon käytettävissä pakettien myös muista lähteistä. Esimerkiksi yhteisön lähettänyt pakettien täydellinen luettelo on osoitteessa [Ohjattu paketit](http://spark-packages.org/).

Tässä artikkelissa kerrotaan, [Ohjattu csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) -paketin käyttäminen Jupyter muistikirja.

##<a name="prerequisites"></a>Edellytykset

Tarvitset seuraavat:

- Azure tilaus. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Apache Ohjattu klusterin HDInsight Linux. Ohjeita on artikkelissa [Azure Hdinsightiin luominen Apache ohjattu varausyksiköt](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="use-external-packages-with-jupyter-notebooks"></a>Ulkoiset pakettien käyttäminen Jupyter muistikirjat 

1. [Azure-portaaliin](https://portal.azure.com/)ja valitse startboard Napsauta Ohjattu klusterin ruutua (jos se kiinnitettyinä startboard). Voit myös siirtyä yhteyttä klusterin-kohdassa **Selaa kaikki** > **HDInsight klustereiden**.   

2. Ohjattu klusterin-sivu valitsemalla **Pikalinkit**, ja valitse sitten **Jupyter muistikirjan**- **Klusterin Dashboard** -sivu. Kirjoita pyydettäessä klusterin järjestelmänvalvojan tunnistetietoja.

    > [AZURE.NOTE] Voi myös saavuttaa Jupyter muistikirjan yhteyttä klusterin avaamalla seuraava URL-osoite selaimen. Korvaa __CLUSTERNAME__ yhteyttä klusterin nimen:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Luo uusi muistikirja. Valitse **Uusi**ja valitse sitten **Ohjattu**.

    ![Luo uusi muistikirja Jupyter] (./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdispark.note.jupyter.createnotebook.png "Luo uusi muistikirja Jupyter")

3. Uusi muistikirja luodaan ja avataan nimellä Untitled.pynb. Valitse yläreunassa muistikirjan nimi ja kirjoita kutsumanimi.

    ![Anna muistikirjalle nimi] (./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdispark.note.jupyter.notebook.name.png "Anna muistikirjalle nimi")

4. Voit käyttää `%%configure` tärkeä määrittäminen käyttämään ulkoisen paketin muistikirjan. Muistikirjat, jotka käyttävät ulkoisen paketteja, varmista, että soitat `%%configure` magic koodin ensimmäiseen soluun. Näin varmistat, että ydin on määritetty käyttämään paketin, ennen kuin käynnistyy.

        %%configure
        { "packages":["com.databricks:spark-csv_2.10:1.4.0"] }


    >[AZURE.IMPORTANT] Jos unohdat määrittäminen ydin ensimmäistä solua, voit käyttää `%%configure` kanssa `-f` parametri, mutta, joka käynnistää istunnon ja kaikki käynnissä menetetään.

5. Valitse yllä koodikatkelman `packages` odottaa maven-testi koordinaattien maven-testi keskitettyyn säilöön luettelo. Valitse tämä koodikatkelman `com.databricks:spark-csv_2.10:1.4.0` on **Ohjattu csv** -paketin maven-testi-koordinaatin. Näin miten paketin koordinaatteja muodosta.

    a. Etsi paketti maven-testi-säilössä. Tässä opetusohjelmassa Käytämme [Ohjattu csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).
    
    b. Kerää säilöstä, arvot **Ryhmätunnus**, **ArtifactId**ja **versio**.

    ![Käytä ulkoisia pakettien Jupyter muistikirjan] (./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/use-external-packages-with-jupyter.png "Käytä ulkoisia pakettien Jupyter muistikirjan")

    c-näppäinyhdistelmää. Yhdistää kolme arvoa, kaksoispisteellä (****:).

        com.databricks:spark-csv_2.10:1.4.0

6. Suorittaa koodin solu, jossa `%%configure` tärkeä. Tämä määritetään pohjana Livy istunnon antamasi pakettia. Muistikirjan seuraavien soluissa voit käyttää paketti nyt alla kuvatulla tavalla.

        val df = sqlContext.read.format("com.databricks.spark.csv").
        option("header", "true").
        option("inferSchema", "true").
        load("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

7. Voit suorittaa katkelmat, kuten alla näkyvän edellisessä vaiheessa luomasi dataframe tiedot.

        df.show()

        df.select("Time").count()


## <a name="seealso"></a>Katso myös


* [Yleistä: Apache ohjattu-Azure Hdinsightiin](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Skenaariot

* [Ohjattu BI: vuorovaikutteinen tietojen analysoinnissa ohjattu käyttäminen HDInsight kanssa Liiketoimintatieto-työkaluista](hdinsight-apache-spark-use-bi-tools.md)

* [Ohjattu koneen Learning kanssa: Käytä ohjattu-HDInsight rakennuksen lämpötilan LVI tietojen analysointiin](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

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

* [Asenna tietokoneeseen Jupyter ja muodosta yhteys ohjattu HDInsight-klusterin](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Resurssien hallinta

* [Resurssien Azure Hdinsightiin Apache Ohjattu-klusterin](hdinsight-apache-spark-resource-manager.md)

* [Raita- ja Apache ohjattu töitä klusterin Hdinsightiin](hdinsight-apache-spark-job-debugging.md)
