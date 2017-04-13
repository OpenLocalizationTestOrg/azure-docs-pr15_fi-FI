<properties
   pageTitle="Käytä MapReduce ja kääntö kanssa Hadoop HDInsight | Microsoft Azure"
   description="Opettele etäyhteyden suorittaa MapReduce työt Hadoop kanssa käyttämällä kääntö Hdinsightista."
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
   ms.date="09/27/2016"
   ms.author="larryfr"/>

#<a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-curl"></a>Suorittaa MapReduce työt Hadoop kanssa käyttämällä kääntö Hdinsightiin

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Tämän asiakirjan kerrotaan, miten voit käyttää kääntö Hadoop HDInsight-klusterissa MapReduce työt.

Kääntö käytetään näytetään, miten voit käsitellä HDInsight MapReduce töiden raaka pyyntöjen avulla. Tämä toimii käyttämällä WebHCat REST API (aiemmalta nimeltään Templeton) HDInsight-klusterin myöntämä.

> [AZURE.NOTE] Jos olet jo aiemmin Linux-pohjaiset Hadoop palvelinten käyttäminen, mutta ole aiemmin käyttänyt HDInsight-kohdassa [huomioitavia seikkoja Linux-pohjaiset Hadoop-Hdinsightista](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Edellytykset

Tämän artikkelin seuraavien vaiheiden suorittamiseen tarvitset seuraavat:

* Hadoop HDInsight-klusterissa (Linux tai Windows-pohjainen)

* [Kääntö](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a id="curl"></a>Suorita MapReduce työt käyttämällä kääntö

> [AZURE.NOTE] Kun käytät kääntö tai muu REST-yhteydenotto WebHCat, on todentaa pyynnöt antamalla HDInsight-klusterin järjestelmänvalvojan käyttäjänimi ja salasana. Sinun on myös käytettävä klusterinimeä, jota käytetään pyynnöt lähettäminen palvelimen URI osana.
>
> Tässä osassa komentojen korvaa **käyttäjänimi** käyttäjä voi todentaa salasanalla käyttäjätilin klusterin ja **salasana** . Korvaa **CLUSTERNAME** yhteyttä klusterin nimen.
>
> REST API on suojattu [basic access-todennuksen](http://en.wikipedia.org/wiki/Basic_access_authentication)avulla. Tee pyynnöt aina HTTPS avulla voit varmistaa, että tunnistetiedot lähetetään suojatusti palvelimeen.

1. Komentoriviltä Käytä seuraavaa komentoa varmistaaksesi, että voit muodostaa yhteyden HDInsight-klusterin:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    Näyttöön tulee vastausta seuraavankaltaiselta:

        {"status":"ok","version":"v1"}

    Käyttää tätä komentoa parametrit ovat seuraavat:

    * **-u**: käyttäjänimi ja salasana todennetaan pyyntö osoittaa
    * **G**: ilmaisee, että GET-pyynnössä

    URI- **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**alussa on sama kaikki palvelupyynnöt.

2. Lähetä MapReduce työn, käytä seuraavaa komentoa:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d jar=wasbs:///example/jars/hadoop-mapreduce-examples.jar -d class=wordcount -d arg=wasbs:///example/data/gutenberg/davinci.txt -d arg=wasbs:///example/data/CurlOut https://CLUSTERNAME.azurehdinsight.net/templeton/v1/mapreduce/jar

    URI (/ mapreduce/purkki) loppuun kertoo, että pyyntö alkaa MapReduce työn luokan purkki tiedostossa WebHCat. Käyttää tätä komentoa parametrit ovat seuraavat:

    * **-d**: `-G` ei käytetä, joten pyynnön oletusarvo on POST-menetelmää. `-d`määrittää arvot, jotka lähetetään pyyntö kanssa.

        * **User.name**: käyttäjä, joka toimii komento
        * **JAR**: purkki tiedosto, joka sisältää luokan sijainnin suoritettiin
        * **luokka**: luokka, joka sisältää MapReduce logiikka
        * **argumentti**: MapReduce; työhön välitettävien argumenttien Tässä tapauksessa, Lisää teksti-tiedosto ja kansio, joita käytetään tulos

    Tämä komento tulee palauttaa työn ID, jolla voidaan työn tilan tarkistaminen:

        {"id":"job_1415651640909_0026"}

3. Voit tarkistaa työn tilan, käytä seuraavaa komentoa. Korvaa **työn tunnus** edellisessä vaiheessa arvo. Jos palautusarvo esimerkiksi `{"id":"job_1415651640909_0026"}`, ja valitse työn tunnus on `job_1415651640909_0026`.

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    Jos työ on valmis, tila olla "onnistui".

    > [AZURE.NOTE] Kääntö pyyntö palauttaa JSON tiedoston tietoja työstä; jq käytetään hakemiseen tila-arvoa.

4. Kun työn tila on muuttunut **onnistui**, työn tulokset voit noutaa Azure-Blob-säiliö. `statusdir` , Jotka välitetään kyselyn parametri sisältää kohdetiedosto; sijainti Tässä tapauksessa **wasbs: / / / esimerkki/kääntö**. Osoitteen tallentaa työn tulosteen HDInsight-klusterin käyttämä oletusarvo-tallennustilan säiliön **Esimerkki/kääntö** hakemistossa.

Voit luettelo ja lataa tiedostot [Azure CLI](../xplat-cli-install.md)avulla. Esimerkiksi **Esimerkki/kääntö**tiedostojen luettelon seuraavalla komennolla:

    azure storage blob list <container-name> example/curl

Jos haluat ladata tiedoston, toimimalla seuraavasti:

    azure storage blob download <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] Tallennustilan tilin nimi, joka sisältää blob käyttämällä on määritettävä `-a` ja `-k` parametreja tai set **AZURE\_TALLENNUSTILAN\_tilin** ja **AZURE\_TALLENNUSTILAN\_ACCESS\_avain** ympäristömuuttujat. Katso lisätietoja [lataaminen HDInsight tiedot](hdinsight-upload-data.md) .

##<a id="summary"></a>Yhteenveto

Osoittanut tässä asiakirjassa, voit käyttää raaka HTTP-pyynnön suorittaminen, valvonta ja tarkasteleminen rakennetta työpaikkojen HDInsight-klusterin.

Lisätietoja REST-liittymän, jota käytetään tämän artikkelin kohdassa [WebHCat viittaus](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).

##<a id="nextsteps"></a>Seuraavat vaiheet

Yleisiä tietoja MapReduce työt HDInsight:

* [Hadoop-HDInsight MapReduce käyttäminen](hdinsight-use-mapreduce.md)

Lisätietoja muista tavoista voit käsitellä Hadoop-HDInsight:

* [Hadoop HDInsight-rakenteen käyttäminen](hdinsight-use-hive.md)

* [Possu käyttäminen Hadoop-Hdinsightiin](hdinsight-use-pig.md)
