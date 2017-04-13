<properties 
    pageTitle="Tunnetut ongelmat ja Apache ohjattu HDInsight | Microsoft Azure" 
    description="Apache ohjattu HDInsight-tunnetut ongelmat." 
    services="hdinsight" 
    documentationCenter="" 
    authors="mumian" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/25/2016" 
    ms.author="nitinme"/>

# <a name="known-issues-for-apache-spark-cluster-on-hdinsight-linux"></a>Apache Ohjattu klusterin HDInsight Linux tunnetut ongelmat

Tämän asiakirjan pitää kirjaa siitä HDInsight ohjattu public preview kaikista tunnetuista ongelmista.  

##<a name="livy-leaks-interactive-session"></a>Livy vuodot vuorovaikutteisen istunnon
 
Kun Livy käynnistetään vuorovaikutteisen istunnon (Ambari tai headnode 0 virtuaalikoneen uudelleenkäynnistyksen vuoksi) edelleen toiminnassa, vuorovaikutteinen työ-istunnon voidaan vuotamaan. Tästä syystä uusia töitä voit jumissa hyväksytty-tilassa ja ei voi aloittaa.

**Rajoituksen:**

Noudattamalla seuraavia vaiheita voit kiertää ongelman:

1. Ssh headnode kyselyjä. 
2. Suorita seuraava komento sovelluksen tunnukset vuorovaikutteinen töiden Livy kautta. 

        yarn application –list

    Nimet on määritetty Livy Jos työt on aloittaminen Livy vuorovaikutteisen istunnon ilman eksplisiittinen nimiä, Livy istunnon aloittaminen Jupyter muistikirjan oletusarvon projektin työn nimi alkaa remotesparkmagics_ *. 

3. Suorita seuraava komento lopettaa kyseisiä töitä. 

        yarn application –kill <Application ID>

Uusia töitä alkaa käynnissä. 

##<a name="spark-history-server-not-started"></a>Ohjattu historia-palvelin ei ole aloitettu 

Ohjattu historia-palvelinta ei ole käynnistetty automaattisesti klusterin luomisen jälkeen.  

**Rajoituksen:** 

Käynnistä manuaalisesti Ambari historia-palvelimeen.

## <a name="permission-issue-in-spark-log-directory"></a>Ohjattu log Directory ongelman käyttöoikeudet 

Kun hdiuser lähettää työn spark-submit kanssa, käytössäsi on virhe java.io.FileNotFoundException: /var/log/spark/sparkdriver_hdiuser.log (estetty) ja ohjaimen lokiin ei ole kirjoitettu. 

**Rajoituksen:**
 
1. Lisää hdiuser Hadoop-ryhmään. 
2. Anna 777 käyttöoikeudet /var/log/spark klusterin luonnin jälkeen. 
3. Päivitä Ambari avulla voit olla 777 oikeuksilla hakemisto ohjattu lokitiedostojen sijainti.  
4. Suorita ohjattu-Lähetä sudo nimellä.  

## <a name="issues-related-to-jupyter-notebooks"></a>Jos haluat Jupyter muistikirjat

Seuraavassa on joitakin Jupyter muistikirjat liittyvistä tunnetuista ongelmista.


### <a name="notebooks-with-non-ascii-characters-in-filenames"></a>Muistikirjojen-ASCII merkkejä tiedostonimissä

Jupyter muistikirjoja, joita voidaan käyttää Ohjattu HDInsight klustereissa ei saa olla-ASCII merkkejä tiedostonimissä. Jos yrität ladata tiedoston-ASCII tiedostonimi on Jupyter Käyttöliittymän kautta, se epäonnistuu äänettömästi (eli Jupyter ei anna Lataa tiedosto, mutta se ei palauttaa näkyvissä virhe joko). 

### <a name="error-while-loading-notebooks-of-larger-sizes"></a>Virhe ladattaessa suurempi kokoisille muistikirjat

Jotkut virheen **`Error loading notebook`** kun lataat muistikirjat, jotka ovat suurempia kokoinen.  

**Rajoituksen:**

Jos saat virheilmoituksen, se ei tarkoita tiedoissa on vaurioitunut tai kadonneen.  Muistikirjat näkyvät edelleen levyn `/var/lib/jupyter`, ja voit käyttää niitä klusterin SSH. Voit kopioida yhteyttä klusterin muistikirjojen (joko SCP tai WinSCP) paikalliseksi varmuuskopioina muistikirjan tärkeitä tietoja katoamisen estämiseksi. Voit sitten oman headnode osoitteessa portin 8001 käyttämään Jupyter avaamatta yhdyskäytävän SSH tunnelin.  Sieltä voit poistaa muistikirjan tulosteen ja tallenna se muistikirjan koon pienentämiseksi uudelleen.

Jos haluat estää tämän virheen tulevaisuudessa, sinun on noudatettava parhaista käytännöistä:

* On tärkeää säilyttää muistikirjan koon pieni. Muistikirjan, joka lähetetään takaisin Jupyter ohjattu työt tulosteen on samanlainen.  On parhaita käytäntöjä, joiden Jupyter yleinen välttää `.collect()` käytössä suuren RDD's tai dataframes; sen sijaan, jos haluat vilkaista RDD sisältöä, harkitse käytön `.take()` tai `.sample()` niin, että jaettavaan päädy liian suuri.
* Myös, kun muistikirjan, poista kaikki tulosteen solujen koon pienentämiseksi.

### <a name="notebook-initial-startup-takes-longer-than-expected"></a>Muistikirjan ensimmäisen käynnistyksen kestää odotettua kauemmin 

Ensimmäinen tärkeä ohjattu muistikirjan Jupyter koodi-lause voi kestää yli minuuttiin.  

**SELITYS:**
 
Tämä johtuu siitä ensimmäiseen koodin suoritettaessa. Taustalla Tämä käynnistää istunnon määritys ja ohjattu, SQL ja rakenteen kontekstit on määritetty. Kun nämä kontekstit on määritetty, ensimmäinen lause suoritetaan ja näin vaikutelman, lause kulunut kauan loppuun.

### <a name="jupyter-notebook-timeout-in-creating-the-session"></a>Jupyter muistikirjan luominen istunnon aikakatkaisu

Kun Ohjattu klusterin on resursseja, ohjattu ja Pyspark ytimet Jupyter muistikirjassa on yritit luoda istunnon aikakatkaisu. 

**Ongelman pienentämistavat:** 

1. Vapauta resurssien Ohjattu-klusterin mukaan:

    - Ohjattu muut muistikirjat pysäytetään valikosta Sulje ja pysäyttäminen tai valitsemalla Sammuta muistikirjan Resurssienhallinnassa.
    - Pysäytetään kuitenkaan ohjattu muista sovelluksista.

2. Yritit käynnistäminen muistikirja uudelleen. Riitä resurssien pitäisi olla käytettävissä, voit luoda istunnon nyt.

##<a name="see-also"></a>Katso myös

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

* [Ulkoiset pakettien käyttäminen Jupyter muistikirjat](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Asenna tietokoneeseen Jupyter ja muodosta yhteys ohjattu HDInsight-klusterin](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Resurssien hallinta

* [Resurssien Azure Hdinsightiin Apache Ohjattu-klusterin](hdinsight-apache-spark-resource-manager.md)

* [Raita- ja Apache ohjattu töitä klusterin Hdinsightiin](hdinsight-apache-spark-job-debugging.md)
