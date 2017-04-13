<properties 
    pageTitle="Ohjattu HDInsight-klusterin mukautettuja kirjastoja käyttäminen analysointiin sivuston lokit | Microsoft Azure" 
    description="Käytä mukautettuja kirjastojen ohjattu HDInsight-klusterin sivuston lokit analysointia varten" 
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

# <a name="analyze-website-logs-using-a-custom-library-with-apache-spark-cluster-on-hdinsight-linux"></a>Analysoi mukautetun kirjaston käyttäminen Apache Ohjattu klusterin HDInsight Linux sivuston lokit

Tämän muistikirjan esitellään, miten mukautetun kirjaston käyttäminen ohjattu HDInsight-lokitietojen analysointia varten. Käytämme mukautetun kirjasto on nimeltään **iislogparser.py**Python kirjastoon.

> [AZURE.TIP] Tässä opetusohjelmassa on myös käytettävissä Jupyter muistikirjan, jonka luot HDInsight klusterissa ohjattu (Linux). Muistikirjan kokemus voit suorittaa Python katkelmat muistikirjan itse. Suoritettava opetusohjelman muistikirjan luominen Ohjattu klusterin, Jupyter muistikirjan avaaminen (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), ja suorita sitten **Analysoi kirjautuu kanssa käyttämällä mukautettuja library.ipynb ohjattu** muistikirjan **PySpark** -kansiossa.

**Edellytykset:**

Tarvitset seuraavat:

- Azure tilaus. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Apache Ohjattu klusterin HDInsight Linux. Ohjeita on artikkelissa [Azure Hdinsightiin luominen Apache ohjattu varausyksiköt](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="save-raw-data-as-an-rdd"></a>Tietoja Tallenna nimellä RDD

Tässä osassa Käytämme liittyvät Apache Ohjattu-klusterin HDInsight- [Jupyter](https://jupyter.org) muistikirjan suorittamaan töitä, käsitellä raaka mallitiedot ja Tallenna rakenne-taulukkoon. Esimerkkitiedot on .csv-tiedosto (hvac.csv) käytettävissä kaikissa klustereiden oletusarvoisesti.

Kun tiedot on tallennettu rakenne-taulukkona, seuraavan osion olemme muodostaa yhteyden käyttämällä Liiketoimintatieto-työkaluista, kuten Power BI- ja Tableau rakennetaulukko.

1. [Azure-portaaliin](https://portal.azure.com/)ja valitse startboard Napsauta Ohjattu klusterin ruutua (jos se kiinnitettyinä startboard). Voit myös siirtyä yhteyttä klusterin-kohdassa **Selaa kaikki** > **HDInsight klustereiden**.   

2. Ohjattu klusterin-sivu **Klusterin Raporttinäkymät-ikkunan**ja valitse sitten **Jupyter muistikirja**. Kirjoita pyydettäessä klusterin järjestelmänvalvojan tunnistetietoja.

    > [AZURE.NOTE] Voi myös saavuttaa Jupyter muistikirjan yhteyttä klusterin avaamalla seuraava URL-osoite selaimen. Korvaa __CLUSTERNAME__ yhteyttä klusterin nimen:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Luo uusi muistikirja. Valitse **Uusi**ja valitse sitten **PySpark**.

    ![Luo uusi muistikirja Jupyter] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdispark.note.jupyter.createnotebook.png "Luo uusi muistikirja Jupyter")

3. Uusi muistikirja luodaan ja avataan nimellä Untitled.pynb. Valitse yläreunassa muistikirjan nimi ja kirjoita kutsumanimi.

    ![Anna muistikirjalle nimi] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdispark.note.jupyter.notebook.name.png "Anna muistikirjalle nimi")

4. Koska olet luonut muistikirjan PySpark ydin, sinun ei tarvitse luoda minkä tahansa kontekstit erikseen. Ohjattu ja rakenne-kontekstit luodaan automaattisesti, kun suoritat koodin ensimmäiseen soluun. Voit aloittaa tuomalla tiedostotyypit, joita tarvitaan Tämä skenaario. Liitä seuraava koodikatkelman tyhjään soluun ja paina näppäinyhdistelmää **VAIHTO + ENTER**.


        from pyspark.sql import Row
        from pyspark.sql.types import *


5. Luo jo käytettävissä otoksen lokitiedot käyttäminen klusterin RDD. Voit käyttää tallennustilan oletustilin klusterin osoitteessa **\HdiSamples\HdiSamples\WebsiteLogSampleData\SampleLog\909f2b.log**liittyvät tiedot.


        logs = sc.textFile('wasbs:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log')


6. Hakea asettaminen, varmista, että otoksen lokin edellisessä vaiheessa onnistui.

        logs.take(5)

    Näyttöön tulee tulos seuraavankaltaiselta:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [u'#Software: Microsoft Internet Information Services 8.0',
         u'#Fields: date time s-sitename cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step4.png X-ARR-LOG-ID=4bea5b3d-8ac9-46c9-9b8c-ec3e9500cbea 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 72177 871 47']

## <a name="analyze-log-data-using-a-custom-python-library"></a>Analysoi tietoja käyttämällä mukautettuja Python kirjastoon

7. Yllä tulosteesta muutaman ensimmäisen rivin ovat otsikkotiedot ja jäljellä olevan kunkin rivin vastaa kyseisen kuvattu rakenne. Jäsennyksen näiden lokeja voi olla monimutkaisia. Näin on Käytämme mukautetun Python kirjastoon (**iislogparser.py**), joka tekee jäsennyksen helpompaa tällaisten lokit. Oletusarvoisesti tämä kirjasto sisältyy oman Ohjattu klusterin HDInsight-osoitteessa **/HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py**.

    Kuitenkin tämä kirjasto ei ole tekstimuodossa `PYTHONPATH` niin, että Microsoft ei voi käyttää esimerkiksi tuonti-lauseen avulla `import iislogparser`. Tämän kirjaston käyttäminen on täytyy jakaa sen työntekijä solmujen. Suorita seuraavat koodikatkelman.


        sc.addPyFile('wasbs:///HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py')


9. `iislogparser`antaa `parse_log_line` , joka palauttaa `None` Jos lokin rivi on otsikkorivi ja palauttaa esiintymä `LogLine` luokan, jos se havaitsee log-rivi. Käytä `LogLine` luokan poimia vain log rivit RDD:

        def parse_line(l):
            import iislogparser
            return iislogparser.parse_log_line(l)
        logLines = logs.map(parse_line).filter(lambda p: p is not None).cache()


10. Noutaa pari poimitun log viivoja ja varmista, että onnistui vaihe.

        logLines.take(2)

    Tulosteen pitäisi näyttää seuraavankaltaiselta:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46,
        2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32]


11. `LogLine` Puolestaan on muutamia hyödyllisiä menetelmiä luokka, kuten `is_error()`, joka palauttaa onko lokitiedosto virhekoodin. Tämän toiminnon avulla voit laskea virheiden poimitun log rivien määrä ja kirjaudu sitten toiseen tiedostoon kaikki virheet.

        errors = logLines.filter(lambda p: p.is_error())
        numLines = logLines.count()
        numErrors = errors.count()
        print 'There are', numErrors, 'errors and', numLines, 'log entries'
        errors.map(lambda p: str(p)).saveAsTextFile('wasbs:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b-2.log')

    Pitäisi näkyä tulos seuraavalla tavalla:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        There are 30 errors and 646 log entries

12. Voit myös käyttää tietojen visualisoinnin **Matplotlib** . Esimerkiksi jos haluat erottaa pyynnöt, jotka suoritetaan pitkään syy, haluat ehkä löytää tiedostot, jotka eniten yhteyshenkilönä keskimäärin aikaa.
Alla olevassa koodikatkelman hakee yläreunan 25 resurssit, joiden noudatit useimmat aikaa yhteyshenkilönä pyynnön.

        def avgTimeTakenByKey(rdd):
            return rdd.combineByKey(lambda line: (line.time_taken, 1),
                                    lambda x, line: (x[0] + line.time_taken, x[1] + 1),
                                    lambda x, y: (x[0] + y[0], x[1] + y[1]))\
                      .map(lambda x: (x[0], float(x[1][0]) / float(x[1][1])))
            
        avgTimeTakenByKey(logLines.map(lambda p: (p.cs_uri_stem, p))).top(25, lambda x: x[1])

    Pitäisi näkyä tulos seuraavalla tavalla:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [(u'/blogposts/mvc4/step13.png', 197.5),
         (u'/blogposts/mvc2/step10.jpg', 179.5),
         (u'/blogposts/extractusercontrol/step5.png', 170.0),
         (u'/blogposts/mvc4/step8.png', 159.0),
         (u'/blogposts/mvcrouting/step22.jpg', 155.0),
         (u'/blogposts/mvcrouting/step3.jpg', 152.0),
         (u'/blogposts/linqsproc1/step16.jpg', 138.75),
         (u'/blogposts/linqsproc1/step26.jpg', 137.33333333333334),
         (u'/blogposts/vs2008javascript/step10.jpg', 127.0),
         (u'/blogposts/nested/step2.jpg', 126.0),
         (u'/blogposts/adminpack/step1.png', 124.0),
         (u'/BlogPosts/datalistpaging/step2.png', 118.0),
         (u'/blogposts/mvc4/step35.png', 117.0),
         (u'/blogposts/mvcrouting/step2.jpg', 116.5),
         (u'/blogposts/aboutme/basketball.jpg', 109.0),
         (u'/blogposts/anonymoustypes/step11.jpg', 109.0),
         (u'/blogposts/mvc4/step12.png', 106.0),
         (u'/blogposts/linq8/step0.jpg', 105.5),
         (u'/blogposts/mvc2/step18.jpg', 104.0),
         (u'/blogposts/mvc2/step11.jpg', 104.0),
         (u'/blogposts/mvcrouting/step1.jpg', 104.0),
         (u'/blogposts/extractusercontrol/step1.png', 103.0),
         (u'/blogposts/sqlvideos/sqlvideos.jpg', 102.0),
         (u'/blogposts/mvcrouting/step21.jpg', 101.0),
         (u'/blogposts/mvc4/step1.png', 98.0)]


13. Voit myös näyttää tiedot piirto-lomakkeessa. Luo piirto, uutiskirjeistä luo väliaikaisen taulukossa **AverageTime**ensimmäisessä vaiheessa. Taulukon ryhmittelee lokit voit tarkastella, jos käytettävissä on minkä tahansa epätavallisia viive piikkarit tietyn milloin tahansa.

        avgTimeTakenByMinute = avgTimeTakenByKey(logLines.map(lambda p: (p.datetime.minute, p))).sortByKey()
        schema = StructType([StructField('Minutes', IntegerType(), True),
                             StructField('Time', FloatType(), True)])
                             
        avgTimeTakenByMinuteDF = sqlContext.createDataFrame(avgTimeTakenByMinute, schema)
        avgTimeTakenByMinuteDF.registerTempTable('AverageTime')

14. Suorita seuraava SQL-kyselyn saat kaikki tietueet **AverageTime** taulukossa.

        %%sql -o averagetime
        SELECT * FROM AverageTime

    `%%sql` Tärkeä perään `-o averagetime` varmistaa, että kyselyn tulos on samanlainen paikallisesti Jupyter palvelimessa (yleensä klusterin headnode). Tulos on samanlainen kuin [Pandas](http://pandas.pydata.org/) -dataframe määritetyn nimen **averagetime**kanssa.

    Pitäisi näkyä tulos seuraavalla tavalla:

    ![SQL-kyselyn tuloksiin] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/sql.output.png "SQL-kyselyn tuloksiin")

    Jos haluat lisätietoja `%%sql` tärkeä sekä muita käytettävissä PySpark, ydin magics Katso [ytimet Jupyter muistikirjojen ohjattu HDInsight klustereiden käytettävissä](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels).

15. Voit nyt luoda joka Matplotlib muodostamiseen tietojen visualisoinnin kirjaston. Mitä on luotava-paikallisesti pysyviä **averagetime** dataframe, koska koodikatkelman alettava `%%local` tärkeä. Tällä varmistetaan, että koodin suorittaminen Jupyter palvelimessa paikallisesti.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt
        
        plt.plot(averagetime['Minutes'], averagetime['Time'], marker='o', linestyle='--')
        plt.xlabel('Time (min)')
        plt.ylabel('Average time taken for request (ms)')

    Pitäisi näkyä tulos seuraavalla tavalla:

    ![Matplotlib tulostus] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdi-apache-spark-web-log-analysis-plot.png "Matplotlib tulostus")

16. Sen jälkeen, kun olet sovelluksen käytössä, sinun tulee Sammuta muistikirjan Vapauta resurssit. Muistikirjan **Tiedosto** -valikon kiellä valitsemalla **Sulje ja Pysäytä**. Tämä on sulkeminen ja Sulje muistikirja.
    

## <a name="seealso"></a>Katso myös


* [Yleistä: Apache ohjattu-Azure Hdinsightiin](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Skenaariot

* [Ohjattu BI: vuorovaikutteinen tietojen analysoinnissa ohjattu käyttäminen HDInsight kanssa Liiketoimintatieto-työkaluista](hdinsight-apache-spark-use-bi-tools.md)

* [Ohjattu koneen Learning kanssa: Käytä ohjattu-HDInsight rakennuksen lämpötilan LVI tietojen analysointiin](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Ohjattu koneen Learning kanssa: Käytä ohjattu elintarvikkeiden tulokset ennustetaan HDInsight-](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Ohjattu virtautetun median: Käytä ohjattu HDInsight reaaliaikainen streaming sovellusten luomiseen:](hdinsight-apache-spark-eventhub-streaming.md)

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
