<properties
    pageTitle="Ohjattu klusterin luominen HDInsight Linux ja käyttäminen ohjattu SQL-Jupyter vuorovaikutteisen analyysin | Microsoft Azure"
    description="Vaiheittaiset ohjeet siitä, miten voit luoda nopeasti Apache ohjattu HDInsight-klusterin ja käytä sitten ohjattu SQL Jupyter muistikirjoista vuorovaikutteinen kyselyt."
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
    ms.topic="get-started-article"
    ms.date="10/28/2016"
    ms.author="nitinme"/>


# <a name="get-started-create-apache-spark-cluster-on-hdinsight-linux-and-run-interactive-queries-using-spark-sql"></a>Aloittaminen: Apache Ohjattu klusterin luominen HDInsight Linux ja vuorovaikutteinen kyselyjen ohjattu SQL suorittaminen

Lue, miten voit luoda Apache Ohjattu-klusterin HDInsight ja suorita Ohjattu klusterin vuorovaikutteinen ohjattu SQL-kyselyjä [Jupyter](https://jupyter.org) muistikirjan avulla.

   ![Apache ohjattu HDInsight-käytön aloittaminen] (./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.getstartedflow.png  "Käyttäminen Apache ohjattu HDInsight opetusohjelmassa. Esitelty vaiheet: tallennustilan; tilin luominen Luo klusteri; Suorita ohjattu SQL-lauseet")

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a>Edellytykset

- **Azure-tilaus**. Ennen kuin aloitat Tässä opetusohjelmassa, sinun on oltava Azure tilaus. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **A suojattu runko (SSH)-asiakasohjelman**: Linux, Unix ja OS X järjestelmien provied SSH asiakasohjelman kautta `ssh` komento. Windows-järjestelmiä Suosittelemme [painovärit, muste](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).
    
- **Suojattu runko (SSH) näppäimet (valinnainen)**: Voit suojata SSH-tili, jota käytetään muodostaa yhteyttä klusterin salasanan tai julkinen avain. Salasanalla kaavan nopeasti alkuun ja Käytä asetusta voit nopeasti luoda klusterin ja Suorita testi joitakin työt. Avaimen avulla on turvallisempi, mutta se vaatii lisäasetuksia. Haluat ehkä käyttää tätä tapaa tuotannon klusterin luotaessa. Tässä artikkelissa Käytämme salasanan menetelmän. Ohjeet luominen ja käyttäminen HDInsight SSH näppäimet viitata on seuraavissa artikkeleissa:

    -  Tietokoneesta Linux - [Käyttö SSH Linux-pohjaiset HDInsight (Hadoop) Linux, Unix-tai OS X kanssa](hdinsight-hadoop-linux-use-ssh-unix.md).
    
    -  Windows-tietokoneesta - [Käyttö SSH Linux-pohjaiset HDInsight (Hadoop) Windows kanssa](hdinsight-hadoop-linux-use-ssh-windows.md).

>[AZURE.NOTE] Tässä artikkelissa käytetään luomaan ohjattu klusteriin, joka käyttää [Azure-tallennustilan BLOB kuin klusterin tallennustilan](hdinsight-hadoop-use-blob-storage.md)Azure resurssi-hallinnan malli. Voit myös luoda ohjattu klusteriin, joka käyttää [Azure järvi tietovaraston](../data-lake-store/data-lake-store-overview.md) Lisää tallennustilaa lisäksi Azure-tallennustilan BLOB kuin oletusarvo-tallennustilan. Katso ohjeet [Luo HDInsight-klusterin tietojen järvi kaupan](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

### <a name="access-control-requirements"></a>Access-ohjausobjektin vaatimukset

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-spark-cluster"></a>Ohjattu klusterin luominen

Tässä osassa voit luoda HDInsight versio 3.4 klusterin (ohjattu versio 1.6.1) Azure resurssi-hallinta-mallin avulla. Katso tietoja HDInsight-versioista ja niiden palvelutasosopimuksia [HDInsight osan versiotiedot](hdinsight-component-versioning.md). Katso muiden klusterin luominen kautta, [Luo HDInsight klustereiden](hdinsight-hadoop-provision-linux-clusters.md).

1. Valitse Avaa malli Azure-portaalissa seuraavassa kuvassa.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-spark-cluster-in-hdinsight.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    Malli sijaitsee julkisen blob säilön, *https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-spark-cluster-in-hdinsight.json*. 
   
2. Valitse parametrit-sivu Anna seuraavat tiedot:

    - **ClusterName**: nimi, jonka aiot luoda Hadoop-klusterin.
    - **Klusterin käyttäjätunnus ja salasana**: Kirjautuminen oletusnimi on järjestelmänvalvoja.
    - **SSH käyttäjänimi ja salasana**.
    
    Kirjoita muistiin nämä arvot.  Tarvitset niitä myöhemmin-opetusohjelman.

    > [AZURE.NOTE] SSH käytetään etäyhteyden käyttämällä komentoriviä HDInsight-klusterin. Käyttäjänimi ja salasana, joita käytät tätä käytetään muodostettaessa yhteyttä klusterin SSH kautta. Lisäksi SSH käyttäjänimen on oltava yksilöiviä, luodessaan käyttäjätilille HDInsight klusterin solmuissa kaikki. Seuraavassa on kuvattu joitakin varattu services klusterin tilitiedot ja ei voi käyttää SSH käyttäjänimi:
    >
    > pääkansio, hdiuser, myrsky, hbase, ubuntu, zookeeper, hdfs, kuitenkaan, mapred, hbase, rakenne, oozie, falcon, sqoop, järjestelmänvalvoja, tez, hcat, hdinsight zookeeper.

    > Lisätietoja SSH käyttämisestä Hdinsightista on seuraavissa artikkeleissa:

    > * [Linux-pohjaiset Hadoop HDInsight Linux, Unix tai OS X-SSH käyttäminen](hdinsight-hadoop-linux-use-ssh-unix.md)
    > * [SSH käyttäminen Linux-pohjaiset Hadoop-HDInsight Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

    
3 valitsemalla **OK** voit tallentaa parametrit.

4- **Mukautettu käyttöönotto** -sivu **resurssiryhmä** avattava luetteloruutu-ruutu ja valitse sitten Luo uusi resurssiryhmä **Uusi** . Resurssiryhmä on säilö, joka ryhmittelee klusterin, riippuvaiset tallennustilan tilin ja muut linkitetyn resurssi.

5 Valitse **ehdot**ja valitse sitten **Luo**.

6 valitsemalla **Luo**. Näkyviin tulee uusi ruutu liittyvä Submitting käyttöönoton mallin käyttöönottoa varten. Kestää noin 20 minuutin klusterin ja SQL-tietokannan luomiseen.



## <a name="run-spark-sql-queries-using-a-jupyter-notebook"></a>Suorita ohjattu SQL-kyselyjä käytettäessä Jupyter muistikirjaa

Tässä osassa käytetään Jupyter muistikirjan suorittaa Ohjattu klusterin ohjattu SQL-kyselyjä. HDInsight ohjattu klustereiden on kaksi ytimet, joiden avulla voit Jupyter muistikirjan. Nämä ovat:

* **PySpark** (koskee sovelluksia kirjoitettu Python)
* **Ohjattu** (koskee sovelluksia kirjoitettu Scala)

Tässä artikkelissa käytetään PySpark ydin. Artikkelin [ytimet käytettävissä Jupyter muistikirjojen ohjattu HDInsight klustereiden](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels) voi lukea tietoja PySpark ydin eduista. Muutamaan avaimen PySpark ydin eduista on kuitenkin

* Sinun ei tarvitse määrittää kontekstit ohjattu ja rakenne. Nämä määritetään automaattisesti puolestasi.
* Voit käyttää solun magics, kuten `%%sql`, suorita suoraan SQL- tai rakenne-kyselyjen ilman mitään edellisen koodikatkelmat.
* SQL- tai rakenne-kyselyjen tulosteen automaattisesti näyttää.

### <a name="create-jupyter-notebook-with-pyspark-kernel"></a>Luomalla Jupyter muistikirja PySpark ydin 

1. [Azure-portaaliin](https://portal.azure.com/)ja valitse startboard Napsauta Ohjattu klusterin ruutua (jos se kiinnitettyinä startboard). Voit myös siirtyä yhteyttä klusterin-kohdassa **Selaa kaikki** > **HDInsight klustereiden**.   

2. Ohjattu klusterin-sivu **Klusterin Raporttinäkymät-ikkunan**ja valitse sitten **Jupyter muistikirja**. Kirjoita pyydettäessä klusterin järjestelmänvalvojan tunnistetietoja.

    > [AZURE.NOTE] Voi myös saavuttaa Jupyter muistikirjan yhteyttä klusterin avaamalla seuraava URL-osoite selaimen. Korvaa __CLUSTERNAME__ yhteyttä klusterin nimen:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Luo uusi muistikirja. Valitse **Uusi**ja valitse sitten **PySpark**.

    ![Luo uusi muistikirja Jupyter] (./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.note.jupyter.createnotebook.png "Luo uusi muistikirja Jupyter")

3. Uusi muistikirja luodaan ja avataan nimellä Untitled.pynb. Valitse yläreunassa muistikirjan nimi ja kirjoita kutsumanimi.

    ![Anna muistikirjalle nimi] (./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.note.jupyter.notebook.name.png "Anna muistikirjalle nimi")

4. Koska olet luonut muistikirjan PySpark ydin, sinun ei tarvitse luoda minkä tahansa kontekstit erikseen. Ohjattu ja rakenne-kontekstit luodaan automaattisesti, kun suoritat koodin ensimmäiseen soluun. Voit aloittaa tuomalla, joita tarvitaan Tämä skenaario. Voit tehdä seuraavat koodikatkelman Liitä solun ja painamalla **VAIHTO + ENTER**.

        from pyspark.sql.types import *
        
    Aina, kun suoritat työn Jupyter, sitten web selaimen ikkunan otsikko näkyy **(varattu)** -tila sekä muistion otsikko. Näet myös yhtenäinen ympyrä oikeassa alakulmassa **PySpark** tekstin viereen. Kun työ on valmis, tämä muuttaa tyhjä ympyrä.

     ![Jupyter muistikirjan työn tila] (./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.jupyter.job.status.png "Jupyter muistikirjan työn tila")

4. Lataa mallitiedot väliaikaisen taulukossa. Luodessasi Ohjattu klusterin HDInsight mallitiedosto, **hvac.csv**kopioidaan kohdassa **\HdiSamples\HdiSamples\SensorSampleData\hvac**liittyvän tallennustilan-tilille.

    Liitä Seuraava koodiesimerkki on tyhjä solu ja painamalla **VAIHTO + ENTER**. Koodin tässä esimerkissä Rekisteröi tiedot **LVI**tilapäinen taulukkoon.

        # Load the data
        hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
        
        # Create the schema
        hvacSchema = StructType([StructField("date", StringType(), False),StructField("time", StringType(), False),StructField("targettemp", IntegerType(), False),StructField("actualtemp", IntegerType(), False),StructField("buildingID", StringType(), False)])
        
        # Parse the data in hvacText
        hvac = hvacText.map(lambda s: s.split(",")).filter(lambda s: s[0] != "Date").map(lambda s:(str(s[0]), str(s[1]), int(s[2]), int(s[3]), str(s[6]) ))
        
        # Create a data frame
        hvacdf = sqlContext.createDataFrame(hvac,hvacSchema)
        
        # Register the data fram as a table to run queries against
        hvacdf.registerTempTable("hvac")

5. Kun käytät PySpark ydin, voit nyt suoraan suorittaa SQL-kyselyn väliaikaisen taulukossa **LVI** juuri luomasi avulla `%%sql` tärkeä. Jos haluat lisätietoja `%%sql` tärkeä sekä muita käytettävissä PySpark, ydin magics Katso [ytimet Jupyter muistikirjojen ohjattu HDInsight klustereiden käytettävissä](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels).
        
        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

5. Kun työ on suoritettu onnistuneesti, seuraava taulukkomuotoinen tulos näkyy oletusarvoisesti.

    ![Taulukon tulosteen kyselyn tulos] (./media/hdinsight-apache-spark-jupyter-spark-sql/tabular.output.png "Taulukon tulosteen kyselyn tulos")

    Voit myös tarkastella muiden visualisointien tuloksia. Esimerkiksi alue-kaaviota, saat saman tuloksen näyttäisi samanlaiselta kuin seuraavassa.

    ![Kyselyn tulos kaavion pinta] (./media/hdinsight-apache-spark-jupyter-spark-sql/area.output.png "Kyselyn tulos kaavion pinta")


6. Sen jälkeen, kun olet sovelluksen käytössä, sinun tulee Sammuta muistikirjan Vapauta resurssit. Muistikirjan **Tiedosto** -valikon kiellä valitsemalla **Sulje ja Pysäytä**. Tämä on Sammuta ja Sulje muistikirja.

##<a name="delete-the-cluster"></a>Klusterin poistaminen

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="see-also"></a>Katso myös


* [Yleistä: Apache ohjattu-Azure Hdinsightiin](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Skenaariot

* [Ohjattu BI: vuorovaikutteinen tietojen analysoinnissa ohjattu käyttäminen HDInsight kanssa Liiketoimintatieto-työkaluista](hdinsight-apache-spark-use-bi-tools.md)

* [Ohjattu koneen Learning kanssa: Käytä ohjattu-HDInsight rakennuksen lämpötilan LVI tietojen analysointiin](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Ohjattu koneen Learning kanssa: Käytä ohjattu elintarvikkeiden tulokset ennustetaan HDInsight-](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Ohjattu virtautetun median: Käytä ohjattu HDInsight reaaliaikainen streaming sovellusten luomiseen:](hdinsight-apache-spark-eventhub-streaming.md)

* [Sivuston log analyysi ohjattu käyttäminen Hdinsightiin](hdinsight-apache-spark-custom-library-website-log-analysis.md)

* [Sovelluksen tietoja telemetriatietojen tietojen analysointi ohjattu käyttäminen Hdinsightiin](hdinsight-spark-analyze-application-insight-logs.md)

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

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md
