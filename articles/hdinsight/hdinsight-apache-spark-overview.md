<properties 
    pageTitle="Yleiskatsaus Apache ohjattu HDInsight | Microsoft Azure" 
    description="Johdanto Apache ohjattu HDInsight-ja tilanteet, joissa ohjattu käyttämisestä HDInsight-sovellukset." 
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
    ms.date="08/25/2016" 
    ms.author="nitinme"/>

# <a name="overview-apache-spark-on-hdinsight-linux"></a>Yleistä: Apache ohjattu HDInsight Linux
 
<a href="http://spark.apache.org/" target="_blank">Apache ohjattu</a> on käsittelyn framework, joka tukee ladatun käsittelyn edistää big datasta analyyttisten sovellusten suorituskykyä Avaa lähde-rinnakkain. Ohjattu käsittely ohjelma on luotu nopeus-käyttöä ja yksityiskohtaisen analysoinnin helpottamiseksi. Ohjattu päivän ladatun laskenta ominaisuuksia Varmista on hyvä iteratiivinen algoritmien koneen opiskelu- ja kaavion funktiolauseita. Ohjattu on myös yhteensopiva Azure-Blob-säiliö (WASB), joten tiedot tallennetaan Azure voidaan käsitellä helposti ohjattu kautta.

Kun luot Ohjattu klusterin HDInsight, luot Azure Laske resurssien ohjattu asentanut ja määrittänyt. Kestää noin kymmenen minuuttia vain Ohjattu-klusterin luominen Hdinsightista. Käsitellään tiedot tallennetaan Azure-Blob-objektien tallennustilaan. Katso [Käytä Azure-Blob-säiliö kanssa HDInsight][hdinsight-storage].

![Apache ohjattu-Azure Hdinsightiin] (./media/hdinsight-apache-spark-overview/hdispark.architecture.png  "Apache ohjattu-Azure Hdinsightiin")


**Jos haluat Apache ohjattu Azure HDInsight-käytön aloittaminen** Katso [pikaopas: Luo Ohjattu klusterin HDInsight Linux ja suorita esimerkkisovellusten Jupyter](hdinsight-apache-spark-jupyter-spark-sql.md).

>[AZURE.NOTE] Katso luettelo tunnettuja ongelmia ja rajoituksia nykyinen versio julkaistaan [tunnetut ongelmat ja Apache ohjattu-Azure Hdinsightiin (Linux)](hdinsight-apache-spark-jupyter-spark-sql.md).


## <a name="why-use-spark-on-azure-hdinsight"></a>Miksi käyttää Ohjattu Azure Hdinsightiin? 

Azure Hdinsightiin on täysin hallittujen ohjattu-palvelu. Ohjattu eduista Hdinsightista ovat seuraavat:

| Toiminto                             | Kuvaus       |
|-------------------------------------|-------------------|
| Helppokäyttötoiminnot klustereiden luominen            | Voit luoda uuden Ohjattu klusterin HDInsight käyttämällä Azure hallinta-portaalin ja PowerShellin Azure Hdinsightiin .NET SDK tunnissa. Katso [Ohjattu klusterin HDInsight käytön aloittaminen](hdinsight-apache-spark-jupyter-spark-sql.md) |
| Käytön helppous                     | Ohjattu HDInsight klustereissa sisältää ennalta määritetty Jupyter muistikirjat. Voit käyttää näitä vuorovaikutteinen tietojen käsittely ja visualisoinnin. URL-osoitteet on https://CLUSTERNAME.azurehdinsight.net/jupyter. Korvaa __CLUSTERNAME__ ohjattu HDInsight-klusterin nimen.|
| REST API                       | Ohjattu HDInsight-sisältää [Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server), REST-Ohjelmointirajapinta-pohjainen Ohjattu projektin palvelin ja lähettämiseen etäyhteyden valvoa käynnissä olevat työt. |
| Azure järvi tietovaraston tuki | Ohjattu HDInsight-voidaan määrittää käyttämään Azure järvi tietovaraston Lisää tallennustilaa. Lisätietoja järvi tietosäilö on artikkelissa [Yleiskatsaus, Azure järvi tietosäilö](../data-lake-store/data-lake-store-overview.md).
| Azure palvelujen integrointi | Ohjattu HDInsight-sisältyy yhdistimen Azure tapahtuman porttiin. Asiakkaiden kehittää streaming sovellusten tapahtuma-keskittimet lisäksi [Kafka](http://kafka.apache.org/), joka on jo ohjattu osana. |
| R Server-tuki  | Voit määrittää HDInsight Ohjattu klusterin R-palvelimelle hajautettu R funktiolauseita toimimaan nopeuksia luvattu Ohjattu klusterin kanssa. Lisätietoja on artikkelissa [käyttäminen HDInsight R-palvelimeen](hdinsight-hadoop-r-server-get-started.md).   |
| Integrointi IntelliJ IDEA  | Voit luoda ja lähettää ohjattu HDInsight-klusterin sovellusten IntelliJ HDInsight ‑laajennuksen. Katso lisätietoja [Käytä HDInsight Työkalut ‑laajennuksen IntelliJ KANNATTAA luoda ohjattu HDInsight ohjattu Linux klusterin sovelluksia](hdinsight-apache-spark-intellij-tool-plugin.md). |
| Samanaikainen kyselyt              | HDInsight ohjattu tukee samanaikainen kyselyt. Näin useiden kyselyjen yhden käyttäjän tai useiden kyselyjen eri käyttäjien ja sovellusten jakaminen samoja klusteriresursseja. |
| Välimuistiin tallentamisen SSD                 | Voit valita välimuistin tietoja muistiin tai SSD klusterisolmut liitetään. Välimuistiin tallentamisen muistin tarjoaa kyselyn parhaan suorituskyvyn, mutta voi olla kallista; välimuistiin tallentamisen SSD on hyvä vaihtoehto parantaa kyselyn suorituskykyä ilman ei tarvitse luoda klusterin koon, joita tarvitaan koko tietojoukkoa mahdu muistiin.|
| Integrointi Liiketoimintatieto-työkaluista       | HDInsight ohjattu tarjoaa yhdistimet Liiketoimintatieto-työkaluista, kuten [Power BI](http://www.powerbi.com/) - ja [Tableau](http://www.tableau.com/products/desktop) tietojen analysoinnissa.|
| Valmiiksi ladattua Anaconda kirjastojen        | Ohjattu olevien HDInsight varausyksiköiden sisältyvät Anaconda kirjastojen valmiiksi asennettuna. [Anaconda](http://docs.continuum.io/anaconda/) on lähellä 200 kirjastojen koneen learning, tietojen analysointi, visualisoinnin.|
| Skaalattavuus                     | Vaikka voit määrittää näkyvien solmujen määrän yhteyttä klusterin luonnin aikana, haluat ehkä Suurenna tai Pienennä klusterin vastaamaan työmäärää. Kaikki HDInsight klustereiden Salli klusterin solmujen määrän. Myös Ohjattu klustereiden voi vetää ja pudottaa ilman yksityiskohtien tietoja, koska kaikki tiedot on tallennettu Azure-Blob-objektien tallennustilaan. |
| Tuen 24/7                    | Ohjattu HDInsight-sisältyy yritystason 24/7 tuki- ja SLA 99,9 % ylös-aika.|



## <a name="what-are-the-use-cases-for-spark-on-hdinsight"></a>Mitkä ovat ohjattu HDInsight-käyttö laatikkomäärät?

Apache ohjattu HDInsight-mahdollistaa seuraavat keskeiset skenaariot.

### <a name="interactive-data-analysis-and-bi"></a>Vuorovaikutteinen tietojen analysointi ja BI

[Tarkista opetusohjelman](hdinsight-apache-spark-use-bi-tools.md)

Apache ohjattu HDInsight-tiedot tallennetaan Azure-BLOB-objektit. Yrityksen asiantuntijoilta ja avaimen päätösten tekijät voit analysoida ja luo raportteja tietojen päälle ja vuorovaikutteisten raporttien analysoitujen tietojen Microsoft Power BI avulla. Analyytikot aloittaa rakenteeton tai luovuttaa Jäsennettyjen tietojen Azuren tallennustilaan, Määritä tiedot käyttämällä muistikirjat rakenne ja luominen käyttämällä Microsoft Power BI. HDInsight ohjattu tukee myös kolmannen osapuolen BI-työkaluja, kuten Tableau, Qlikview ja SAP-Lumira, jolloin ihanteellinen ympäristössä tietojen analyytikot, yrityksen asiantuntijoilta ja avaimen päätösten tekijät.

### <a name="iterative-machine-learning"></a>Iteratiivinen koneen oppiminen

[Tarkista opetusohjelman: ennustaa rakentaminen lämpötilat uisng LVI-tiedot](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

[Tarkista opetusohjelman: elintarvikkeiden tulokset ennustetaan](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

Apache ohjattu sisältyy [MLlib](http://spark.apache.org/mllib/), konepohjaisten oppimistekniikoiden kirjaston laadittuihin ohjattu päälle. Ohjattu HDInsight-sisältää, Anaconda-koneen learning pakettien erilaisia Python jaettavaksi. Tapahtumaominaisuuksia tämä valmiin tukeen Jupyter muistikirjoja ja sinulla on ylä-ja--rivi-ympäristön tietokoneen learning sovellusten luomisen.  

### <a name="streaming-and-real-time-data-analysis"></a>Streaming ja reaaliaikaisten tietojen analysointi

[Tarkista opetusohjelman](hdinsight-apache-spark-eventhub-streaming.md)

Reaaliaikaisen tiedon analysointi käytetään vakiovalikoimasta ajan vähentäminen tietojen tietoja käsittelemällä tiedot, kun se viedään maihin, TOSI, streaming ratkaisu rakentamiseen skenaarioita. HDInsight ohjattu on monipuolisia tuki reaaliaikainen analytics ratkaisujen. Ohjattu ei jo ingest tietoja useista lähteistä, kuten Kafka, Flume, Twitterissä, ZeroMQ tai TCP sockets yhdistimiä, HDInsight ohjattu Lisää pääosin tuen ingesting Azure tapahtuman keskittimet tiedot. Tapahtuman keskittimet ovat yleisimmät jonotuspalvelu Azure. Ottaa itsestään-valmiin tuki tapahtuman keskittimien helpottaa tiedostojen HDInsight-etsimisen reaaliaikainen analytics myyntijakso ihanteellinen ympäristössä.

##<a name="next-steps"></a>Mitä osat ovat Ohjattu klusterin sisältää?

Ohjattu HDInsight-sisältää seuraavat osat, jotka ovat käytettävissä klustereiden oletusarvoisesti.

- [Ohjattu Core](https://spark.apache.org/docs/1.5.1/). Sisältää ohjattu Core, ohjattu SQL, ohjattu streaming API, GraphX ja MLlib.
- [Anaconda](http://docs.continuum.io/anaconda/)
- [Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server)
- [Jupyter muistikirjan](https://jupyter.org)

Ohjattu HDInsight-sisältää myös [ODBC-ohjain](http://go.microsoft.com/fwlink/?LinkId=616229) connectivity ohjattu klustereihin HDInsight BI-työkaluista, kuten Microsoft Power BI- ja Tableau.

## <a name="where-do-i-start"></a>Jos aloitetaan?

Aloita Ohjattu-klusterin luominen HDInsight Linux. Katso [pikaopas: Luo Ohjattu klusterin HDInsight Linux ja suorita esimerkkisovellusten Jupyter](hdinsight-apache-spark-jupyter-spark-sql.md). 

## <a name="next-steps"></a>Seuraavat vaiheet

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

* [Ulkoiset pakettien käyttäminen Jupyter muistikirjat](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Asenna tietokoneeseen Jupyter ja muodosta yhteys ohjattu HDInsight-klusterin](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Resurssien hallinta

* [Resurssien Azure Hdinsightiin Apache Ohjattu-klusterin](hdinsight-apache-spark-resource-manager.md)

* [Raita- ja Apache ohjattu töitä klusterin Hdinsightiin](hdinsight-apache-spark-job-debugging.md)


[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
