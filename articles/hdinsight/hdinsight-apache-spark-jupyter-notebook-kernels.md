<properties 
    pageTitle="Käytettävissä Jupyter muistikirjoja HDInsight ohjattu ytimet klusterit Linux | Microsoft Azure" 
    description="Lisätietoja muita Jupyter muistikirjan ytimet Ohjattu klusterin HDInsight Linux käytettävissä." 
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


# <a name="kernels-available-for-jupyter-notebooks-with-apache-spark-clusters-on-hdinsight-linux"></a>Käytettävissä Jupyter muistikirjojen Apache ohjattu klustereiden HDInsight Linux ytimet

HDInsight (Linux)-klusterin Apache ohjattu sisältää Jupyter muistikirjoja, joita voit testata sovelluksia. Ydin on ohjelma, joka suoritetaan ja tulkitsee koodisi. HDInsight ohjattu klustereiden on kaksi ytimet, joiden avulla voit Jupyter muistikirjan. Nämä ovat:

1. **PySpark** (koskee sovelluksia kirjoitettu Python)
2. **Ohjattu** (koskee sovelluksia kirjoitettu Scala)

Tässä artikkelissa tietoja näiden ytimet käyttämisestä ja mitä hyötyä sinulla on käyttänyt niitä.

**Edellytykset:**

Tarvitset seuraavat:

- Azure tilaus. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Apache Ohjattu klusterin HDInsight Linux. Ohjeita on artikkelissa [Azure Hdinsightiin luominen Apache ohjattu varausyksiköt](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="how-do-i-use-the-kernels"></a>Miten käytän ytimet? 

1. [Azure-portaaliin](https://portal.azure.com/)ja valitse startboard Napsauta Ohjattu klusterin ruutua (jos se kiinnitettyinä startboard). Voit myös siirtyä yhteyttä klusterin-kohdassa **Selaa kaikki** > **HDInsight klustereiden**.   

2. Ohjattu klusterin-sivu **Klusterin Raporttinäkymät-ikkunan**ja valitse sitten **Jupyter muistikirja**. Kirjoita pyydettäessä klusterin järjestelmänvalvojan tunnistetietoja.

    > [AZURE.NOTE] Voi myös saavuttaa Jupyter muistikirjan yhteyttä klusterin avaamalla seuraava URL-osoite selaimen. Korvaa __CLUSTERNAME__ yhteyttä klusterin nimen:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Luoda uuden muistikirjan uusi ytimet. Valitse **Uusi**ja valitse sitten **Pyspark** tai **Ohjattu**. Käytettävä ohjattu ydin Scala sovellusten ja PySpark ydin Python sovellukset.

    ![Luo uusi muistikirja Jupyter] (./media/hdinsight-apache-spark-jupyter-notebook-kernels/jupyter-kernels.png "Luo uusi muistikirja Jupyter") 

3. Tämä on Avaa uuden muistikirjan valitsit ydin.

## <a name="why-should-i-use-the-pyspark-or-spark-kernels"></a>Miksi PySpark tai ohjattu ytimet kannattaa käyttää?

Seuraavassa on muutamia uusia ytimet eduista.

1. **Ennalta määritetty kontekstit**. **PySpark** tai **Ohjattu** ytimet Jupyter muistikirjat mukana toimitettuja sinun ei tarvitse määrittää ohjattu tai rakenne-kontekstit erikseen, ennen kuin käynnistät sovelluksen, voit kehittää; käsitteleminen Nämä ovat käytettävissä oletusarvoisesti. Nämä kontekstit ovat seuraavat:

    * **sc** - ohjattu kontekstille
    * **sqlContext** - rakenne kontekstille


    Näin on sinun ei tarvitse Suorita lauseet voit määrittää kontekstit seuraavalta:

        ###################################################
        # YOU DO NOT NEED TO RUN THIS WITH THE NEW KERNELS
        ###################################################
        sc = SparkContext('yarn-client')
        sqlContext = HiveContext(sc)

    Voit sen sijaan suoraan käyttää valmiiksi määritettyä kontekstit sovelluksessa.
    
2. **Solun magics**. PySpark ydin tarjoaa esimääritettyjä "magics", jotka ovat erityisiä komennot, jotka voivat soittaa kanssa `%%` (kuten `%%MAGIC` <args>). Magic komento on koodin solun ensimmäistä sanaa ja Salli useita tekstirivejä-sisältöä. Magic word on oltava ensimmäistä sanaa solussa. Lisääminen mitään tärkeä, vaikka kommentit ennen aiheuttaa virheen.   Katso lisätietoja magics [tähän](http://ipython.readthedocs.org/en/stable/interactive/magics.html).

    Seuraavassa taulukossa on lueteltu kautta ytimet eri magics.

  	| Tärkeä     | Esimerkki                         | Kuvaus  |
  	|-----------|---------------------------------|--------------|
  	| Apua      | `%%help`                            | Luo taulukkoon käytettävissä magics Esimerkki ja kuvaus |
  	| tiedot      | `%%info`                          | Tulostaa nykyisen Livy päätepisteen istunnon tiedot |
  	| asetusten määrittäminen | `%%configure -f`<br>`{"executorMemory": "1000M"`,<br>`"executorCores": 4`} | Määrittää istunnon luominen parametrit. Voimassa merkintä (-f) on pakollinen, jos istunnon on jo luotu ja istunnon kohteet poistetaan, ja uudelleen. Tarkista [Livy's kirjaa /sessions pyytää leipätekstin](https://github.com/cloudera/livy#request-body) kelvollisia parametreja luettelo. Parametrit on siirrettävä JSON merkkijonona, ja se on oltava seuraavalle riville jälkeen tärkeä, kuten esimerkiksi-sarakkeen. |
  	| SQL       |  `%%sql -o <variable name>`<br> `SHOW TABLES`    | Suorittaa sqlContext rakenteen kysely. Jos `-o` välitetty parametri, kyselyn tulos on samanlainen %% paikallisen Python yhteydessä kuin [Pandas](http://pandas.pydata.org/) dataframe.   |
  	| Paikallinen     |     `%%local`<br>`a=1`              | Kaikki seuraaville riveille koodi suoritetaan paikallisesti. Koodi on oltava kelvollinen Python koodi. |
  	| lokit      | `%%logs`                        | Tulostaa nykyisen Livy istunnon lokit.  |
  	| Poista    | `%%delete -f -s <session number>` | Poistaa tietyn istunnon nykyinen Livy päätepisteen. Huomaa, että et voi poistaa, jotka on aloitettu istuntoa varten ydin itse. |
  	| uudelleenjärjestäminen   | `%%cleanup -f`                    | Poistaa kaikki nykyisen Livy päätepisteen, mukaan lukien tämän muistikirjan istunnon istunnot. Voimassa lippu -f on pakollinen.  |

    >[AZURE.NOTE] Lisäksi magics on lisätty PySpark ydin, voit käyttää myös [valmiin IPython magics](https://ipython.org/ipython-doc/3/interactive/magics.html#cell-magics), mukaan lukien `%%sh`. Voit käyttää `%%sh` suorittaa komentosarjat ja koodin lohko-klusterin headnode tärkeä. 

3. **Automaattinen visualisointi**. **Pyspark** ydin automaattisesti valitseminen puolestaan Visualisoi tulosteen rakenne ja SQL-kyselyjä. Voit halutessasi valita useita erilaisia visualisointeja, kuten taulukon-, ympyrä-, viiva-, alue-, palkki.

## <a name="parameters-supported-with-the-sql-magic"></a>Tueta parametrit %% sql tärkeä

%% Sql tärkeä tukee parametrien, joiden avulla voit hallita tulosteen, joka tulee näyttöön, kun suoritat kyselyitä. Seuraavassa taulukossa on tulos.

| Parametri     | Esimerkki                         | Kuvaus  |
|-----------|---------------------------------|--------------|
| + o      | `-o <VARIABLE NAME>`                          | Pysyvä kyselyn tuloksen parametrin avulla %% paikallisen Python konteksti- [Pandas](http://pandas.pydata.org/) dataframe nimellä. Dataframe muuttujan nimi on määrittämäsi muuttujan nimi. |
| -q      | `-q`                          | Käytä tätä visualisoinnit solun käytöstä. Jos et halua visualisointi solun sisältö ja haluat siepata dataframe nimellä ja valitse Käytä `-q -o <VARIABLE>`. Haluatko käytöstä visualisointeja ei ole määrittänyt tulokset (esimerkiksi SQL-kyselyn käytön puoli tehosteita, kuten `CREATE TABLE` lauseen), käyttää `-q` määrittämättä `-o` argumentti. |
| -m       |  `-m <METHOD>`    | Missä **tapa** **ottaa** tai **otoksen** (oletusarvo on **otettava**). Jos menetelmä on **otettava**, ydin valitsee osat (kuvattu jäljempänä tässä taulukossa) MAXROWS määrittämää tulosjoukon tiedot yläreunasta. Jos menetelmä on **otoksen**, ydin satunnaisesti Esimerkki tietojoukon seuraavien osien `-r` parametri, tässä taulukossa on kuvattu Seuraava.   |
| -r     |     `-r <FRACTION>`            | Näin **NIMITTÄJÄ** liukuluku 0.0 ja 1.0 välillä. Jos SQL-kyselyn Esimerkki laskentatapaa on `sample`, valitse ydin esimerkit satunnaisesti; tulosjoukon osista määritetyn nimittäjä Esimerkiksi jos suoritat SQL-kyselyn argumenttien `-m sample -r 0.01`, valitse 1 % tuloksen rivit näyte satunnaisesti. |
| -n      | `-n <MAXROWS>`                        | **MAXROWS** on kokonaisluku. Ydin rajoittaa **MAXROWS**tulosteen rivien määrä. Jos **MAXROWS** on negatiivinen luku, kuten **luku -1**, tulosjoukon rivien määrä ei rajoitettu. |

**Esimerkki:**

    %%sql -q -m sample -r 0.1 -n 500 -o query2 
    SELECT * FROM hivesampletable

Yllä lause tekee seuraavat toimet:

* Valitsee kaikki tietueet **hivesampletable**.
* Koska Käytämme - q, poistaa käytöstä automaattinen visualisointi.
* Koska Käytämme `-m sample -r 0.1 -n 500` se satunnaisesti esimerkit 10 % hivesampletable riveillä ja rajoittaa tulosjoukon rivien 500 kokoa.
* Lopuksi, koska on käytetty `-o query2` myös tallentaa tulosteen dataframe, jota kutsutaan **kysely2**kyselyjä.
    

## <a name="considerations-while-using-the-new-kernels"></a>Huomioon otettavia seikkoja uusi ytimet käytettäessä

Kumpi ydin käytät (PySpark tai ohjattu), jätä käynnissä muistikirjat käyttää klusteriresursseja.  Nämä ytimet kanssa koska kontekstit valmiiksi, riittää, että poistut muistikirjat ei lopettaa yhteydessä ja näin ollen klusteriresursseja on edelleen käytössä. Hyvä, PySpark ja ohjattu ytimet olisi muistikirjan **Tiedosto** -valikosta **Sulje ja pysäyttäminen** -vaihtoehtoa. Tämä keskeyttää yhteydessä ja lopettaa muistikirjan.    


## <a name="show-me-some-examples"></a>Näytä esimerkkejä

Kun avaat Jupyter muistikirjan, näet kaksi kansioiden päätasolle.

* **PySpark** -kansio on esimerkki muistikirjat, jotka käyttävät uusia **Python** ydin.
* **Scala** -kansio on esimerkki muistikirjat, jotka käyttävät uusia **Ohjattu** ydin.

Voit avata muistikirjan **00 – [Lue Minut ensin] Ohjattu tärkeä ydin ominaisuudet** käytettävissä eri magics lisätietoja **PySpark** tai **Ohjattu** kansiosta. Voit käyttää myös muita otoksen muistikirjat käytettävissä kaksi kansioiden lisätietoja eri skenaarioissa Jupyter muistikirjojen käyttäminen HDInsight ohjattu klustereiden saavuttamiseksi.

## <a name="where-are-the-notebooks-stored"></a>Minne muistikirjat tallennetaan?

Jupyter muistikirjat tallennetaan tallennustilan tiliin liittyvät klusterin **/HdiNotebooks** -kansiossa.  Muistikirjojen, tekstitiedostoista ja kansiot, jotka luot Jupyter on käytettävissä olevat WASB.  Jos käytät Jupyter voit luoda kansion **siihen luotavat** ja muistikirjan **myfolder/mynotebook.ipynb**, voit esimerkiksi käyttää kyseisen muistikirjan osoitteessa `wasbs:///HdiNotebooks/myfolder/mynotebook.ipynb`.  Vastaavasti on myös TOSI, toisin sanoen jos lataat muistikirjan suoraan tallennustilan-tilille osoitteessa `/HdiNotebooks/mynotebook1.ipynb`, muistikirjan voi tarkastella Jupyter sekä.  Muistikirjojen säilyy tallennustilan tilin klusterin poistamisen jälkeen.

Muistikirjat on tallennettu tallennustilan tilin poistetaan ÄÄN yhteensopivaan. Näin, jos olet SSH kyselyjä klusterin, voit käyttää tiedoston hallintakomentoja, kuten seuraavassa:

    hdfs dfs -ls /HdiNotebooks                            # List everything at the root directory – everything in this directory is visible to Jupyter from the home page
    hdfs dfs –copyToLocal /HdiNotebooks                 # Download the contents of the HdiNotebooks folder
    hdfs dfs –copyFromLocal example.ipynb /HdiNotebooks   # Upload a notebook example.ipynb to the root folder so it’s visible from Jupyter


Siltä varalta, että on ongelmia yhteyden klusterin tallennustilan-tiliin, muistikirjat on tallennettu myös headnode `/var/lib/jupyter`.

## <a name="supported-browser"></a>Tuettua selainta
Jupyter muistikirjat suorittaminen vasten HDInsight ohjattu klustereiden tuetaan vain Google Chrome.

## <a name="feedback"></a>Palaute

Uusi ytimet ovat kehittyville vaiheen ja erääntyneiden ajan kuluessa. Tämä myös tarkoittaa, että ohjelmointirajapinnan saattaa muuttua, kun nämä ytimet käyttöaika. Arvostamme palautetta, joihin sinulla on nämä uusi ytimet-ohjelmalla. Tämä on erittäin hyödyllinen muokkaavat nämä ytimet n lopullinen versio julkaistaan. Voit jättää kommentteja ja palautetta tämän artikkelin alareunassa **Kommentit** -osassa.


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

* [Ulkoiset pakettien käyttäminen Jupyter muistikirjat](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Asenna tietokoneeseen Jupyter ja muodosta yhteys ohjattu HDInsight-klusterin](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Resurssien hallinta

* [Resurssien Azure Hdinsightiin Apache Ohjattu-klusterin](hdinsight-apache-spark-resource-manager.md)

* [Raita- ja Apache ohjattu töitä klusterin Hdinsightiin](hdinsight-apache-spark-job-debugging.md)
