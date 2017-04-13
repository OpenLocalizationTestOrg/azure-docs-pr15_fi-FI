<properties
   pageTitle="Hadoop Possu käyttäminen HDInsight kääntö | Microsoft Azure"
   description="Opettele käyttää kääntö Hadoop-klusterin Azure HDInsight-Possu latinalainen työt."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/23/2016"
   ms.author="larryfr"/>

#<a name="run-pig-jobs-with-hadoop-on-hdinsight-by-using-curl"></a>Näyttää Possu työt Hadoop-HDInsight käyttämällä kääntö

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Tässä asiakirjassa kerrotaan, voit käyttää kääntö Azure HDInsight-klusterin Possu latinalainen työt. Possu latinalainen ohjelmointikieli voit kuvaavat muunnoksia, joita käytetään esittämään haluttu tulos syöttötiedot.

Kääntö käytetään näytetään, miten voit käsitellä HDInsight käyttämällä raaka pyyntöjen suorittaminen, valvonta ja Hae Possu työt tulokset. Tämä toimii WebHCat REST API (aiemmalta nimeltään Templeton), joka tarjoaa HDInsight-klusterin avulla.

> [AZURE.NOTE] Jos ovat jo tuttuja Linux-pohjaiset Hadoop palvelinten käyttäminen, mutta ole aiemmin käyttänyt HDInsight-kohdassa [Linux-pohjaiset HDInsight-vihjeitä](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Edellytykset

Tämän artikkelin seuraavien vaiheiden suorittamiseen tarvitset seuraavat:

* Azure Hdinsightiin (Hadoop-HDInsight)-klusterin (Linux-pohjaiset tai Windows-pohjainen)

* [Kääntö](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a id="curl"></a>Suorita Possu työt kääntö avulla

> [AZURE.NOTE] Kun käytät kääntö tai muu REST-yhteydenotto WebHCat kanssa, on todentaa pyynnöt antamalla HDInsight-klusterin järjestelmänvalvojan käyttäjänimi ja salasana. Sinun on käytettävä klusterinimeä myös, tunnus URI (Uniform Resource), jota käytetään lähettää pyynnöt palvelimeen osana.
>
> Tässä osassa komentojen **käyttäjänimi** korvaaminen käyttäjä voi todentaa klusterin ja korvaa **SALASANAN** käyttäjätilin salasanan. Korvaa **CLUSTERNAME** yhteyttä klusterin nimen.
>
> REST API on suojattu [basic access-todennus](http://en.wikipedia.org/wiki/Basic_access_authentication)kautta. Varmista aina pyynnöt Secure HTTP (HTTPS) avulla voit varmistaa, että tunnistetiedot lähetetään suojatusti palvelimeen.

1. Komentoriviltä Varmista, että voit muodostaa yhteyden HDInsight-klusterin seuraava komento avulla:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    Näyttöön tulee vastausta seuraavankaltaiselta:

        {"status":"ok","version":"v1"}

    Käyttää tätä komentoa parametrit ovat seuraavat:

    * **-u**: käyttäjänimi ja salasana todennetaan pyyntö
    * **G**: ilmaisee, että GET-pyynnössä

    URL-Osoitteen **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**alkuun ovat samat kaikki palvelupyynnöt. Polun **/status/Status**tarkoittaa, että pyyntö palauttaa WebHCat (tunnetaan myös nimellä Templeton) tilan palvelimen.

2. Seuraava koodi avulla voit lähettää klusterin Possu latinalainen työn:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="LOGS=LOAD+'wasbs:///example/data/sample.log';LEVELS=foreach+LOGS+generate+REGEX_EXTRACT($0,'(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)',1)+as+LOGLEVEL;FILTEREDLEVELS=FILTER+LEVELS+by+LOGLEVEL+is+not+null;GROUPEDLEVELS=GROUP+FILTEREDLEVELS+by+LOGLEVEL;FREQUENCIES=foreach+GROUPEDLEVELS+generate+group+as+LOGLEVEL,COUNT(FILTEREDLEVELS.LOGLEVEL)+as+count;RESULT=order+FREQUENCIES+by+COUNT+desc;DUMP+RESULT;" -d statusdir="wasbs:///example/pigcurl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/pig

    Käyttää tätä komentoa parametrit ovat seuraavat:

    * **-d**: koska `-G` ei käytetä, pyyntö tulee oletusarvon mukaan POST-menetelmää. `-d`määrittää arvot, jotka lähetetään pyyntö kanssa.

        * **User.name**: käyttäjä, joka toimii komento
        * **Suorita**: Possu latinalainen lauseet suorittaminen
        * **statusdir**: kansio, joka työn tila on kirjoitettu

    > [AZURE.NOTE] Huomaa, että Possu latinalainen lauseet välilyönnit korvataan `+` merkki kääntö käytettäessä.

    Tämä komento tulee palauttaa työn ID, jolla voidaan käyttää projektin etenemisen tarkistaminen esimerkiksi:

        {"id":"job_1415651640909_0026"}

3. Voit tarkistaa työn tilan, käytä seuraavaa komentoa. Korvaa **työn tunnus** edellisessä vaiheessa arvo. Jos palautusarvo esimerkiksi `{"id":"job_1415651640909_0026"}`, **työn tunnus** on `job_1415651640909_0026`.

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    Jos työ on valmis, tila ole **onnistui**.

    > [AZURE.NOTE] Kääntö pyyntö palauttaa JavaScript Object Notation (JSON) tiedoston tietoja työstä ja jq käytetään hakemiseen tila-arvoa.

##<a id="results"></a>Tulosten tarkasteleminen

Kun työn tila on muuttunut **onnistui**, työn tulokset voit noutaa Azure-Blob-säiliö. `statusdir` Kyselyn kanssa välitetty parametri on kohdetiedosto; sijainti Tässä tapauksessa **wasbs: / / / esimerkki/pigcurl**. Osoitteen tallentaa työn tulosteen HDInsight-klusterin käyttämä oletusarvo-tallennustilan säiliön **Esimerkki/pigcurl** hakemistossa.

Voit luettelo ja lataa tiedostot [Azure CLI](../xplat-cli-install.md)avulla. Esimerkiksi luettelon tiedostojen **Esimerkki/pigcurl**Kirjoita seuraava komento:

    azure storage blob list <container-name> example/pigcurl

Jos haluat ladata tiedoston, toimimalla seuraavasti:

    azure storage blob download <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] Tallennustilan tilin nimi, joka sisältää blob käyttämällä on määritettävä `-a` ja `-k` parametreja tai set **AZURE\_TALLENNUSTILAN\_tilin** ja **AZURE\_TALLENNUSTILAN\_ACCESS\_avain** ympäristömuuttujat.

##<a id="summary"></a>Yhteenveto

Osoittanut tässä asiakirjassa, voit käyttää raaka HTTP-pyynnön suorittaminen, valvonta ja Possu työt tulosten tarkasteleminen HDInsight-klusterissa.

Lisätietoja tässä artikkelissa käytetyt REST-liittymän Hae [WebHCat viittaus](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).

##<a id="nextsteps"></a>Seuraavat vaiheet

Lisätietoja Possu HDInsight:

* [Possu käyttäminen Hadoop-Hdinsightiin](hdinsight-use-pig.md)

Lisätietoja muista tavoista voit käsitellä Hadoop-HDInsight:

* [Hadoop HDInsight-rakenteen käyttäminen](hdinsight-use-hive.md)

* [Hadoop-HDInsight MapReduce käyttäminen](hdinsight-use-mapreduce.md)
