<properties 
    pageTitle="Jupyter muistikirjan asentaminen tietokoneeseen ja yhdistä se ohjattu HDInsight-klusterin | Microsoft Azure" 
    description="Asentaa Jupyter muistikirjan paikallisesti tietokoneesta ja yhdistä se Apache Ohjattu-klusterin Azure HDInsight-tietoja." 
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
    ms.date="09/26/2016" 
    ms.author="nitinme"/>


# <a name="install-jupyter-notebook-on-your-computer-and-connect-to-apache-spark-cluster-on-hdinsight-linux"></a>Jupyter muistikirjan asentaminen tietokoneeseen ja Apache Ohjattu klusterin HDInsight Linux yhdistäminen

Tässä artikkelissa kerrotaan, asentaa Jupyter muistikirjan mukautetun PySpark (for Python) ja ohjattu (for Scala) ytimet ohjattu taikaa ja muistikirjan yhdistäminen HDInsight-klusterin. Voi olla useita syitä Jupyter asennetaan paikalliseen tietokoneeseen ja voi olla joitakin haasteita. Luettelo syistä ja haasteisiin on kohdassa [Miksi minun kannattaa asentaa Jupyter tietokoneessa](#why-should-i-install-jupyter-on-my-computer) on tämän artikkelin lopussa.

Yhdistävää Jupyter ja ohjattu tärkeä asentaminen tietokoneeseen on tärkeimmät kolme vaihetta.

* Asenna Jupyter muistikirjan
* Asenna PySpark ja ohjattu ytimet ohjattu taikaa
* Ohjattu tärkeä Ohjattu klusterin HDInsight-yhteyden määrittäminen

Saat lisätietoja mukautetun ytimet ja käytettävissä Jupyter muistikirjojen HDInsight-klusterin ohjattu tärkeä [ytimet käytettävissä olevien HDInsight varausyksiköiden Apache ohjattu Linux muistikirjojen Jupyter](hdinsight-apache-spark-jupyter-notebook-kernels.md).

##<a name="prerequisites"></a>Edellytykset

Tässä luettelossa edellytykset eivät ole Jupyter asentamiseen. Nämä ovat muodostamisesta HDInsight-klusterin Jupyter muistikirjan, kun muistikirja on asennettu.

- Azure tilaus. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Apache Ohjattu klusterin HDInsight Linux. Ohjeita on artikkelissa [Azure Hdinsightiin luominen Apache ohjattu varausyksiköt](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="install-jupyter-notebook-on-your-computer"></a>Jupyter muistikirjan asentaminen tietokoneeseen

Sinun on asennettava Python, ennen kuin asennat Jupyter muistikirjat. Python ja Jupyter ovat käytettävissä [Ananconda jakauman](https://www.continuum.io/downloads)osana. Kun olet asentanut Anaconda, asennat todella Python jakauman. Kun Anaconda on asennettu, voit lisätä Jupyter asennuksen suorittamalla komennon. Tässä osassa on ohjeet, joita on seurattava.

1. Lataa omaa ympäristöäsi [Anaconda installer](https://www.continuum.io/downloads) ja suorita asennus. Ohjatun määritystoiminnon esitettävän Varmista, että valitset Anaconda lisääminen LIIKERATA muuttuja.

2. Suorita seuraava komento asentaa Jupyter.

        conda install jupyter

    Saat lisätietoja installting Jupyter [Asentaminen Jupyter Anaconda avulla](http://jupyter.readthedocs.io/en/latest/install.html).

## <a name="install-the-kernels-and-spark-magic"></a>Asenna ytimet ja ohjattu tärkeä

Ohjeita siitä, miten voit asentaa ohjattu tärkeä PySpark ja ohjattu-ytimet artikkelissa GitHub [sparkmagic ohjeissa](https://github.com/jupyter-incubator/sparkmagic#installation) .

## <a name="configure-spark-magic-to-access-the-hdinsight-spark-cluster"></a>Ohjattu tärkeä on ohjattu HDInsight-klusterin määrittäminen

Tässä osassa voit määrittää, johon asensit aiemmin muodostaa Apache ohjattu-klusteriin, joka on olet jo luonut Azure Hdinsightiin ohjattu tärkeä.

1. Jupyter tiedot tallennetaan yleensä käyttäjät pääkansion. Etsi kaikki käyttöympäristö kotihakemistoosi, kirjoita seuraavat komennot.

    Aloita Python käyttöliittymän. Kirjoita komentorivi-ikkunaan seuraavat tiedot:

        python

    Kirjoita seuraavalla komennolla pääkansion selville Python-liittymän.

        import os
        print(os.path.expanduser('~'))

2. Siirry pääkansion ja luo kansio nimeltä **.sparkmagic** , jos se ei ole jo olemassa.

3. Kansiossa Luo tiedosto nimeltä **config.json** ja lisää seuraavat JSON koodikatkelman, sisällä.

        {
          "kernel_python_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          },
          "kernel_scala_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          }
        }

4. **{Käyttäjänimi}**, **{CLUSTERDNSNAME}**ja **{BASE64ENCODEDPASSWORD}** korvaa tarvittavat arvot. Apuohjelmien tuttuja ohjelmointikieli tai verkossa useita avulla voit luoda base64-koodattuja salasanan actualy salasana. Yksinkertainen Python koodikatkelman suorittaminen Komentorivi-ikkuna on seuraava:

        python -c "import base64; print(base64.b64encode('{YOURPASSWORD}'))"

5. Aloita Jupyter. Käytä seuraavaa komentoa komentoriviltä.

        jupyter notebook

6. Varmista, että voit muodostaa yhteyden käyttämällä Jupyter muistikirjan klusterin ja, joita voit käyttää käytettävissä ohjattu tärkeä ytimet kanssa. Suorita seuraavat vaiheet.

    1. Luo uusi muistikirja. Valitse oikeassa yläkulmassa **Uusi**. Raportissa pitäisi näkyä oletusarvon ydin **Python2** ja kahden uuden ytimet, jotka olet asentanut, **PySpark** ja **Ohjattu**.

        ![Luo uusi muistikirja Jupyter] (./media/hdinsight-apache-spark-jupyter-notebook-install-locally/jupyter-kernels.png "Luo uusi muistikirja Jupyter")

    
        Valitse **PySpark**.


    2. Suorita seuraavat koodikatkelman.

            %%sql
            SELECT * FROM hivesampletable LIMIT 5

        Jos haet voi onnistuneesti tulosteen, HDInsight-klusterin yhteys testataan.

    >[AZURE.TIP] Jos haluat päivittää muodostaa yhteyden eri klusterin muistikirjan määritykset, Päivitä config.json uusi laskettavat arvot, mukaisesti vaiheesta 3 yllä. 

## <a name="why-should-i-install-jupyter-on-my-computer"></a>Miksi minun kannattaa asentaa Jupyter tietokoneessani?

Voi olla useita syitä, miksi voit Jupyter asentaminen tietokoneeseen ja kytke se sitten ohjattu HDInsight-klusteriin.

* Vaikka Jupyter muistikirjat on jo käytössä Azure Hdinsightiin Ohjattu-klusterin, Jupyter asentaminen tietokoneeseen on luontivaihtoehto muistikirjojen paikallisesti testaat sovelluksesi käynnissä klusterin vastaan, ja lataa muistikirjat klusterin. Voit ladata klusterin muistikirjat, voit joko ladata ne Jupyter muistikirja, jossa on käytössä tai klusterin tai tallentaa tiedostoja tallennustilan klusterin liittyvää tiliä /HdiNotebooks-kansioon. Saat lisätietoja siitä, miten muistikirjat on tallennettu klusterin, [minne Jupyter muistikirjat tallennetaan](hdinsight-apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored)?
* Käytettävissä olevat työkirjoja paikallisesti, voit muodostaa yhteyden eri ohjattu klustereiden sovelluksen tarpeen mukaan.
* Voit käyttää GitHub tietolähteen hallinta-järjestelmä otetaan käyttöön ja muistikirjat Versionhallinta. Voit myös määrittää yhteiskäytön ympäristössä, jossa useat käyttäjät voivat käsitellä samaan muistikirjaan.
* Voit käyttää muistikirjoja paikallisesti ilman, että klusterin sinun. Tarvitset vain klusteriin ja testaa muistikirjat vastaan, ei voi hallita muistikirjojen tai kehitysympäristö manuaalisesti.
* Voi olla helpompi määrittää oman paikallisen kehitysympäristö suhteessa sen klusterin Jupyter asennuksen määrittämistä varten.  Hyödynnä koko ohjelmisto on asennettu paikallisesti ilman vähintään yhden remote klustereiden määrittämisestä.

>[AZURE.WARNING] Paikalliseen tietokoneeseen asennettu Jupyter useita käyttäjiä voit suorittaa saman muistikirjan saman Ohjattu klusterin samanaikaisesti. Tällaisessa luodaan useita Livy istuntoja. Jos ongelma voi ilmetä ja haluat korjata joka, se on seurata Livy istunto monimutkaista kuuluu käyttäjän.




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

* [Ulkoiset pakettien käyttäminen Jupyter muistikirjat](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

### <a name="manage-resources"></a>Resurssien hallinta

* [Resurssien Azure Hdinsightiin Apache Ohjattu-klusterin](hdinsight-apache-spark-resource-manager.md)

* [Raita- ja Apache ohjattu töitä klusterin Hdinsightiin](hdinsight-apache-spark-job-debugging.md)
