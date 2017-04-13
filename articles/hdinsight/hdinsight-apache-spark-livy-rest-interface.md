<properties
    pageTitle="Lähetä ohjattu työt käyttämällä etäyhteyden Livy | Microsoft Azure"
    description="Opettele lähettää ohjattu työt etäyhteyden HDInsight klustereiden Livy avulla."
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


# <a name="submit-spark-jobs-remotely-to-an-apache-spark-cluster-on-hdinsight-linux-using-livy"></a>Lähettää ohjattu työt etäyhteyden Apache Ohjattu-klusterin HDInsight Linux Livy käyttäminen

Azure HDInsight-klusterin Apache ohjattu sisältää Livy REST-liittymän työt etäyhteyden Ohjattu klusterin lähettämistä varten. Saat yksityiskohtaiset ohjeet [Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server).

Voit suorittaa vuorovaikutteinen ohjattu liittymät tai lähettää erätyöt voidaan suorittaa ohjattu Livy. Tässä artikkelissa on lyhyt Livy avulla voit lähettää Ehdota. Alla olevassa syntaksi käyttää kääntö REST-puheluissa Livy päätepisteelle.

**Edellytykset:**

Tarvitset seuraavat:

- Azure tilaus. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Apache Ohjattu klusterin HDInsight Linux. Ohjeita on artikkelissa [Azure Hdinsightiin luominen Apache ohjattu varausyksiköt](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="submit-a-batch-job-the-cluster"></a>Lähetät erän työn klusterin

Ennen kuin voit lähettää erän työn, täytyy ladata sovelluksen purkkiin klusterin liittyvän klusterin säilössä. [**AzCopy**](../storage/storage-use-azcopy.md), komentorivi-apuohjelman avulla voit tehdä. Liittyy monia muita asiakkaiden, voit ladata tiedostoja. Voit etsiä lisää niistä Lisää osoitteessa [Lataa tiedot Hadoop projekteille Hdinsightista](hdinsight-upload-data.md).

    curl -k --user "<hdinsight user>:<user password>" -v -H <content-type> -X POST -d '{ "file":"<path to application jar>", "className":"<classname in jar>" }' 'https://<spark_cluster_name>.azurehdinsight.net/livy/batches'

**Esimerkkejä**:

* Jos purkki tiedosto on klusterin storage (WASB)

        curl -k --user "admin:mypassword1!" -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasbs://mycontainer@mystorageaccount.blob.core.windows.net/data/SparkSimpleTest.jar", "className":"com.microsoft.spark.test.SimpleFile" }' "https://mysparkcluster.azurehdinsight.net/livy/batches"

* Jos, jota haluat siirtää purkki tiedostonimi ja LuokanNimi syötteen tiedoston osana (Tässä esimerkissä input.txt)

        curl -k  --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

## <a name="get-information-on-batches-running-on-the-cluster"></a>Tiedoston tietojen erissä klusterin käytössä

    curl -k --user "<hdinsight user>:<user password>" -v -X GET "https://<spark_cluster_name>.azurehdinsight.net/livy/batches"

**Esimerkkejä**:

* Jos haluat noutaa kaikki käynnissä olevat klusterin erissä:

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"

* Jos haluat hakea tiettyjä erän annetun batchId

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"


## <a name="delete-a-batch-job"></a>Työn Siirtoerän poistaminen

    curl -k --user "<hdinsight user>:<user password>" -v -X DELETE "https://<spark_cluster_name>.azurehdinsight.net/livy/batches/{batchId}"

**Esimerkki**:

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="livy-and-high-availability"></a>Livy ja suuren käytettävyyden

Livy tarjoaa suuren käytettävyyden Ohjattu klusterin töitä. Seuraavassa on muutamia esimerkkejä.

* Jos Livy-palvelun menee, kun olet lähettänyt työn etäyhteyden Ohjattu klusterin, työn säilyy käynnissä taustalla. Livy ollessa Varmuuskopioi se palauttaa työn ja raporttien se takaisin.

* Livy muistiin Jupyter muistikirjaa HDInsight-taustaan. Jos muistikirja on käynnissä Ohjattu projektin ja Livy palvelun saa käynnistetään uudelleen, muistikirjan säilyvät suorittaa koodin solut. 

## <a name="show-me-an-example"></a>Näytä esimerkki

Tässä osassa on on esitetty esimerkkejä siitä, miten voit käyttää Livy esittää ohjattu, sovelluksen edistymistä ja poista sitten työ. Tässä esimerkissä Käytämme sovellus on yksi kehittänyt on artikkelissa [Luo erillisen Scala sovelluksen ja suorittaa HDInsight Ohjattu klusterin](hdinsight-apache-spark-create-standalone-application.md). Seuraavia ohjeita oletetaan seuraavasti:

* Olet jo kopioinut sovelluksen purkkiin päälle tallennustilan klusterin liittyvää tiliä.
* Sinulla on asennettu tietokoneeseen, jossa seuraavasti rooliin kääntö.

Suorita seuraavat vaiheet.

1. Anna meidän Tarkista ensin, että Livy suoritetaan klusterin. Emme voi tehdä käytön luettelo käynnissä erissä. Jos käytössäsi on työn Livy ensimmäistä kertaa, tämä tulee palauttaa nolla.

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"

    Tulos pitäisi saada seuraavankaltaiselta:

        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:47:53 GMT
        < Content-Length: 34
        <
        {"from":0,"total":0,"sessions":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    Huomaa, miten tulosteen viimeisen rivin lukee **yhteensä: 0**, joka ei ole käynnissä erissä ehdottaa.

2. Anna meidän lähettää erän työn. Alla koodikatkelman käyttää parametreiksi välittää purkki nimi jonkin lähdetiedoston (input.txt) ja luokkanimi. Tämä on suositellaan, jos käytät poistaminen Windows-tietokoneessa seuraavat toimet.

        curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

    Tiedoston **input.txt** parametrit määritellään seuraavasti:

        { "file":"wasbs:///example/jars/SparkSimpleApp.jar", "className":"com.microsoft.spark.example.WasbIOTest" }

    Näyttöön tulee tulos seuraavankaltaiselta:

        < HTTP/1.1 201 Created
        < Content-Type: application/json; charset=UTF-8
        < Location: /0
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:51:30 GMT
        < Content-Length: 36
        <
        {"id":0,"state":"starting","log":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    Ilmoitus siitä, miten tulosteen viimeisellä rivillä lukee **tila: käynnistäminen**. Myös lukee, **id: 0**. Tämä on erän ID-tunnuksellasi.

3. Voit nyt noutaa tilan tietyn erän käyttämällä erä ID-tunnuksellasi.

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/0"

    Näyttöön tulee tulos seuraavankaltaiselta:

        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:54:42 GMT
        < Content-Length: 509
        <
        {"id":0,"state":"success","log":["\t diagnostics: N/A","\t ApplicationMaster host: 10.0.0.4","\t ApplicationMaster RPC port: 0","\t queue: default","\t start time: 1448063505350","\t final status: SUCCEEDED","\t tracking URL: http://hn0-myspar.lpel1gnnvxne3gwzqkfq5u5uzh.jx.internal.cloudapp.net:8088/proxy/application_1447984474852_0002/","\t user: root","15/11/20 23:52:47 INFO Utils: Shutdown hook called","15/11/20 23:52:47 INFO Utils: Deleting directory /tmp/spark-b72cd2bf-280b-4c57-8ceb-9e3e69ac7d0c"]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    Tulos näkyy nyt **tila: onnistui**, jossa ehdotetaan työn onnistui.

4. Jos haluat, voit poistaa erän.

        curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/0"

    Näyttöön tulee tulos seuraavankaltaiselta:

        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Sat, 21 Nov 2015 18:51:54 GMT
        < Content-Length: 17
        <
        {"msg":"deleted"}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    Rivin tulos näyttää erän onnistui. Jos poistat työ on käynnissä, työn olennaisesti lopettaa. Jos poistat työstä valmiina-onnistuneesti tai muussa tapauksessa se poistaa työn tiedot kokonaan.

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
