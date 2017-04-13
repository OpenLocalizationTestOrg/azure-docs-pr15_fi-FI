<properties 
    pageTitle="Ohjattu klusterin HDInsight Linux Zeppelin muistikirjojen käyttäminen | Microsoft Azure" 
    description="Vaiheittaiset ohjeet Zeppelin muistikirjojen käyttäminen ohjattu klustereiden HDInsight Linux." 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/05/2016" 
    ms.author="nitinme"/>


# <a name="use-zeppelin-notebooks-with-apache-spark-cluster-on-hdinsight-linux"></a>Apache Ohjattu klusterin HDInsight Linux Zeppelin muistikirjojen käyttäminen

HDInsight ohjattu klustereiden sisältävät Zeppelin muistikirjoja, joita voit suorittaa ohjattu töitä. Tässä artikkelissa kerrotaan, Zeppelin muistikirjan käyttämisestä HDInsight-klusterin.


**Edellytykset:**

* Azure tilaus. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* Ohjattu Apache-klusterin. Ohjeita on artikkelissa [Azure Hdinsightiin luominen Apache ohjattu varausyksiköt](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="launch-a-zeppelin-notebook"></a>Zeppelin muistikirjan avaaminen

1. Ohjattu klusterin-sivu **Klusterin Raporttinäkymät-ikkunan**ja valitse sitten **Zeppelin muistikirja**. Kirjoita pyydettäessä klusterin järjestelmänvalvojan tunnistetietoja.

    > [AZURE.NOTE] Voi myös saavuttaa Zeppelin muistikirjan yhteyttä klusterin avaamalla seuraava URL-osoite selaimen. Korvaa __CLUSTERNAME__ yhteyttä klusterin nimen:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/zeppelin`

2. Luo uusi muistikirja. Otsikko-ruudusta **muistikirja**ja valitse sitten **Luo uusi muistiinpano**.

    ![Luo uusi muistikirja Zeppelin] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.createnewnote.png "Luo uusi muistikirja Zeppelin")

    Kirjoita muistikirjan nimi ja valitse sitten **Luo huomautus**.

3. Varmista myös, että muistion otsikko näkyy yhdistetyn tila. Se on merkitty vihreä piste oikeassa alakulmassa.

    ![Zeppelin muistikirjan tila] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.newnote.connected.png "Zeppelin muistikirjan tila")

4. Lataa mallitiedot väliaikaisen taulukossa. Luodessasi Ohjattu klusterin HDInsight mallitiedosto, **hvac.csv**kopioidaan kohdassa **\HdiSamples\SensorSampleData\hvac**liittyvän tallennustilan-tilille.

    Liitä tyhjän kappaleen, joka luodaan uusi muistikirja oletusarvoisesti seuraavat koodikatkelman.

        %livy.spark
        //The above magic instructs Zeppelin to use the Livy Scala interpreter

        // Create an RDD using the default Spark context, sc
        val hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
        
        // Define a schema
        case class Hvac(date: String, time: String, targettemp: Integer, actualtemp: Integer, buildingID: String)
        
        // Map the values in the .csv file to the schema
        val hvac = hvacText.map(s => s.split(",")).filter(s => s(0) != "Date").map(
            s => Hvac(s(0), 
                    s(1),
                    s(2).toInt,
                    s(3).toInt,
                    s(6)
            )
        ).toDF()
        
        // Register as a temporary table called "hvac"
        hvac.registerTempTable("hvac")
        
    Paina **VAIHTO + ENTER** tai napsauta Suorita koodikatkelman kappaleen **Toista** -painiketta. Tilan kappaleen oikeassa olisi edistyminen-valmis odottavat suorittaminen valmiiksi. Tulos näyttää sama kappale alareunassa. Näyttökuvan näyttää seuraavalta:

    ![Tietoja väliaikaisen taulukon luominen] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.note.loaddDataintotable.png "Tietoja väliaikaisen taulukon luominen")

    Voit myös kirjoittaa kunkin kappaleen otsikon. Valitse oikeassa yläkulmassa **asetukset** -kuvaketta ja valitse sitten **Näytä otsikko**.

5. Voit nyt suorittaa ohjattu SQL-lauseet **LVI** -taulukossa. Liitä seuraava kysely uuden kappaleen. Kysely palauttaa rakennuksen tunniste ja kohde ja todellinen lämpötilat etsimisen kunkin tiettynä päivänä. Paina **VAIHTO + ENTER**.

        %sql
        select buildingID, (targettemp - actualtemp) as temp_diff, date from hvac where date = "6/1/13" 

    **% Sql** -lauseen alussa kertoo Livy Scala kääntäjän käyttämään muistikirjaa.

    Seuraavassa näyttökuvassa näkyy tulos.

    ![Suorita ohjattu SQL-lause muistikirja] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.note.sparksqlquery1.png "Suorita ohjattu SQL-lause muistikirja")

     Valitse vaihtaa saman tuloksen erilaisia-esityksiä (korostettuna suorakulmio) näyttöasetukset. Valitse **asetukset** valitsemaan mitä consitutes avain ja arvot tulos. Näyttökuvan yläpuolella käyttää **rakennuksen tunniste** arvona avain ja **temp_diff** keskiarvon.

    
6. Voit lähettää arviointisivustopyynnön myös suorittamalla ohjattu SQL-lauseet muuttujien käyttäminen kyselyssä. Seuraava koodikatkelman näytetään, miten voit määrittää muuttujaa **Temp**-kysely, jossa on mahdolliset arvot haluat kyselyn, jossa. Kun suoritat kyselyn, avattavan luettelon täytetään automaattisesti muuttujan annetuilla arvoilla.

        %sql
        select buildingID, date, targettemp, (targettemp - actualtemp) as temp_diff from hvac where targettemp > "${Temp = 65,65|75|85}" 

    Liitä tämä koodikatkelman uuden kappaleen ja painamalla **VAIHTO + ENTER**. Seuraavassa näyttökuvassa näkyy tulos.

    ![Suorita ohjattu SQL-lause muistikirja] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.note.sparksqlquery2.png "Suorita ohjattu SQL-lause muistikirja")

    Seuraavien kyselyjen voit valita uuden arvon avattavasta luettelosta ja suorita kysely uudelleen. Valitse **asetukset** valitsemaan mitä consitutes avain ja arvot tulos. Yllä näyttökuvan käyttää **rakennuksen tunniste** avaimeksi **temp_diff** arvona ja **targettemp** ryhmäksi keskiarvo.

7. Käynnistä Livy kääntäjän Lopeta sovellus uudelleen. Tee Avaa kääntäjän asetukset valitsemalla kirjautuneena oikeassa yläkulmassa käyttäjänimi ja valitse sitten **kääntäjän**.

    ![Käynnistä kääntäjän] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Rakenne tulostus")

2. Selaa kohtaan Livy kääntäjän asetukset ja valitse sitten **uudelleen**.

    ![Käynnistä Livy intepreter] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.zeppelin.restart.interpreter.png "Käynnistä Zeppelin intepreter")

## <a name="how-do-i-use-external-packages-with-the-notebook"></a>Miten ulkoisen pakettien käyttäminen muistikirjan?

Voit määrittää Zeppelin muistikirjan Apache Ohjattu-klusterin HDInsight (Linux) käyttämään ulkoinen, yhteisön lähettänyt paketteja, jotka eivät sisälly ulos,-valmiilla klusterin. Voit hakea [maven-testi säilöön](http://search.maven.org/) paketteja, jotka ovat käytettävissä täydellinen luettelo. Saat luettelon käytettävissä pakettien myös muista lähteistä. Esimerkiksi yhteisön lähettänyt pakettien täydellinen luettelo on osoitteessa [Ohjattu paketit](http://spark-packages.org/).

Tässä artikkelissa tulee näkyviin [Ohjattu csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) -paketin käyttäminen Jupyter muistikirja.

1. Avaa kääntäjän asetukset. Valitse oikeassa yläkulmassa käyttäjänimi kirjautuneena ja valitse **kääntäjän**.

    ![Käynnistä kääntäjän] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Rakenne tulostus")

2. Siirry Livy kääntäjän asetukset ja valitse sitten **Muokkaa**.

    ![Kääntäjän asetusten muuttaminen] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-1.png "Kääntäjän asetusten muuttaminen")

3. Lisää uuden tunnuksen, jota kutsutaan **livy.spark.jars.packages** ja aseta sen arvoksi muodossa `group:id:version`. Näin on, jos haluat käyttää [Ohjattu csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) -paketti, sinun on määritettävä salaisuus arvo `com.databricks:spark-csv_2.10:1.4.0`.

    ![Kääntäjän asetusten muuttaminen] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-2.png "Kääntäjän asetusten muuttaminen")

    Valitse **Tallenna** ja Käynnistä Livy kääntäjän.

4. **Vihje**: Jos haluat, kuinka vuoroon edellä annettu avaimen arvo, näin miten.

    a. Etsi paketti maven-testi-säilössä. Tässä opetusohjelmassa on käytettävä [Ohjattu csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).
    
    b. Kerää säilöstä, arvot **Ryhmätunnus**, **ArtifactId**ja **versio**.

    ![Käytä ulkoisia pakettien Jupyter muistikirjan] (./media/hdinsight-apache-spark-zeppelin-notebook/use-external-packages-with-jupyter.png "Käytä ulkoisia pakettien Jupyter muistikirjan")

    c-näppäinyhdistelmää. Yhdistää kolme arvoa, kaksoispisteellä (****:).

        com.databricks:spark-csv_2.10:1.4.0

## <a name="where-are-the-zeppelin-notebooks-saved"></a>Mihin Zeppelin muistikirjat tallennetaan?

Zeppelin muistikirjat tallennetaan klusterin headnodes. Jotta Jos poistat klusterin, muistikirjat poistetaan myös. Jos haluat säilyttää muistikirjat-toisten klustereiden myöhempää käyttöä varten, sinun on vietävä ne sen jälkeen, kun olet tehnyt töitä. Vie muistikirjan, valitsemalla **Vie** kuvaketta alla olevassa kuvassa esitetyllä tavalla.

![Lataa muistikirja] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-download-notebook.png "Lataa muistikirja")

Tämä tallentaa muistikirjan JSON tiedoston lataaminen-sijainti.

## <a name="livy-session-management"></a>Livy istuntojen hallinnan

Kun suoritat koodin ensimmäisen kappaleen Zeppelin muistikirjan, Livy uuden istunnon luodaan ohjattu HDInsight-klusterin. Tässä istunnossa jaetaan kaikki Zeppelin muistikirjat, jotka luot myöhemmin. Jos jostain syystä Livy istunto on lopetettu (klusterin uudelleenkäynnistyksen jne.), voit voi suorittaa töitä Zeppelin muistikirjasta.

Tässä tapauksessa on suoritettava seuraavat toimet ennen kuin aloitat töitä Zeppelin muistikirjan. 

1. Käynnistä Livy-kääntäjän Zeppelin muistikirjasta. Tee Avaa kääntäjän asetukset valitsemalla kirjautuneena oikeassa yläkulmassa käyttäjänimi ja valitse sitten **kääntäjän**.

    ![Käynnistä kääntäjän] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Rakenne tulostus")

2. Selaa kohtaan Livy kääntäjän asetukset ja valitse sitten **uudelleen**.

    ![Käynnistä Livy intepreter] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.zeppelin.restart.interpreter.png "Käynnistä Zeppelin intepreter")

3. Suorittaa koodin solun Zeppelin muistikirjan. Tämä luo uuden istunnon Livy HDInsight-klusterin.

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







