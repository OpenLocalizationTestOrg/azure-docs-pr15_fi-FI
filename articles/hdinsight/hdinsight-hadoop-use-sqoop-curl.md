<properties
   pageTitle="Hadoop Sqoop käyttäminen HDInsight kääntö | Microsoft Azure"
   description="Lue, miten voit lähettää etäyhteyden Sqoop työt käyttämällä kääntö Hdinsightista."
   services="hdinsight"
   documentationCenter=""
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/21/2016"
   ms.author="jgao"/>

#<a name="run-sqoop-jobs-with-hadoop-in-hdinsight-with-curl"></a>Suorita Sqoop työt HDInsight kanssa kääntö Hadoop

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Opettele käyttää kääntö Hadoop-klusterin HDInsight-Sqoop työt.

Kääntö käytetään näytetään, miten voit käsitellä HDInsight käyttämällä raaka pyyntöjen suorittaminen, valvonta ja Hae Sqoop työt tulokset. Tämä toimii käyttämällä WebHCat REST API (aiemmalta nimeltään Templeton) HDInsight-klusterin myöntämä.

> [AZURE.NOTE] Jos ovat jo tuttuja Linux-pohjaiset Hadoop palvelinten käyttäminen, mutta ole aiemmin käyttänyt HDInsight, lue [käyttämisestä Linux Hdinsightista](hdinsight-hadoop-linux-information.md).

##<a name="prerequisites"></a>Edellytykset

Tämän artikkelin seuraavien vaiheiden suorittamiseen tarvitset seuraavat:

* Hadoop HDInsight-klusterissa (Linux tai Windows-pohjainen)

* [Kääntö](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a name="submit-sqoop-jobs-by-using-curl"></a>Lähettää Sqoop työt kääntö avulla

> [AZURE.NOTE] Kun käytät kääntö tai muu REST-yhteydenotto WebHCat kanssa, on todentaa pyynnöt antamalla HDInsight-klusterin järjestelmänvalvojan käyttäjänimi ja salasana. Sinun on käytettävä klusterinimeä myös, tunnus URI (Uniform Resource) lähettämiseen pyynnöt palvelimeen osana.
>
> Tässä osassa komentojen **käyttäjänimi** korvaaminen käyttäjä voi todentaa klusterin ja korvaa **SALASANAN** käyttäjätilin salasanan. Korvaa **CLUSTERNAME** yhteyttä klusterin nimen.
>
> REST API on suojattu [perustodentamista](http://en.wikipedia.org/wiki/Basic_access_authentication)kautta. Varmista aina pyynnöt Secure HTTP (HTTPS) avulla voit varmistaa, että tunnistetiedot lähetetään suojatusti palvelimeen.

1. Komentoriviltä Varmista, että voit muodostaa yhteyden HDInsight-klusterin seuraava komento avulla:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    Näyttöön tulee vastausta seuraavankaltaiselta:

        {"status":"ok","version":"v1"}

    Käyttää tätä komentoa parametrit ovat seuraavat:

    * **-u** - käyttäjänimen ja salasanan todennetaan pyynnön.
    * **G** - ilmaisee, että tämä on GET-pyynnössä.

    URL-Osoitteen **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**alkuun ovat samat kaikki palvelupyynnöt. Polun **/status/Status**tarkoittaa, että pyyntö palauttaa tilan WebHCat (tunnetaan myös nimellä Templeton) palvelimen. 

2. Voit lähettää sqoop työn käyttämällä seuraavaa:


        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d command="export --connect jdbc:sqlserver://SQLDATABASESERVERNAME.database.windows.net;user=USERNAME@SQLDATABASESERVERNAME;password=PASSWORD;database=SQLDATABASENAME --table log4jlogs --export-dir /tutorials/usesqoop/data --input-fields-terminated-by \0x20 -m 1" -d statusdir="wasbs:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/sqoop

    Käyttää tätä komentoa parametrit ovat seuraavat:

    * **-d** - jälkeen `-G` ei käytetä, pyyntö tulee oletusarvon mukaan POST-menetelmää. `-d`määrittää arvot, jotka lähetetään pyyntö kanssa.

        * **user.name** - käyttäjä, joka toimii komento.

        * **komento** - Sqoop-komennon suorittaminen.

        * **statusdir** - kansio, joka työn tila kirjoitetaan.

    Tämä komento tulee palauttaa työn ID, jolla voidaan tarkistaa työn tilan.

        {"id":"job_1415651640909_0026"}

3. Voit tarkistaa työn tilan, käytä seuraavaa komentoa. Korvaa **työn tunnus** edellisessä vaiheessa arvo. Jos palautusarvo esimerkiksi `{"id":"job_1415651640909_0026"}`, **työn tunnus** on `job_1415651640909_0026`.

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    Jos työ on valmis, tila ole **onnistui**.

    > [AZURE.NOTE] Kääntö pyyntö palauttaa JavaScript Object Notation (JSON) tiedoston tietoja työstä; jq käytetään hakemiseen tila-arvoa.

4. Kun työn tila on muuttunut **onnistui**, työn tulokset voit noutaa Azure-Blob-säiliö. `statusdir` Kyselyn kanssa välitetty parametri on kohdetiedosto; sijainti Tässä tapauksessa **wasbs: / / / esimerkki/kääntö**. Osoitteen tallentaa työn tulosteen HDInsight-klusterin käyttämä oletusarvo-tallennustilan säiliön **Esimerkki/kääntö** -kansio.

    Voit luettelo ja lataa tiedostot [Azure CLI](../xplat-cli-install.md)avulla. Esimerkiksi luettelon tiedostojen **Esimerkki/kääntö**Kirjoita seuraava komento:

        azure storage blob list <container-name> example/curl

    Jos haluat ladata tiedoston, toimimalla seuraavasti:

        azure storage blob download <container-name> <blob-name> <destination-file>

    > [AZURE.NOTE] Sinun on määritettävä tallennustilan tilin nimi, joka sisältää blob avulla `-a` ja `-k` parametreja tai joukko **AZURE\_TALLENNUSTILAN\_tilin** ja **AZURE\_TALLENNUSTILAN\_ACCESS\_avain** ympäristömuuttujat. Katso < Hyperlinkkiviittaus = "hdinsight-Lataa-data.md" target = "_blank" Lisätietoja.

##<a name="limitations"></a>Rajoitukset

* Vie - ja Linux-pohjaiset HDInsight, Microsoft SQL Server tai Azure SQL-tietokannan tietojen vieminen Sqoop liittimen joukkona ei tue joukkona Lisää tällä hetkellä.

* Jonottaminen - Linux-pohjaiset HDInsight kanssa käytettäessä `-batch` vaihtaminen Lisää suoritettaessa, Sqoop suorittaa useita Lisää sijaan jonottaminen Lisää toimintoja.

##<a name="summary"></a>Yhteenveto

Osoittanut tässä asiakirjassa, voit käyttää raaka HTTP-pyynnön suorittaminen, valvonta ja Sqoop työt tulosten tarkasteleminen HDInsight-klusterissa.

Katso lisätietoja tässä artikkelissa käytetyt REST-liittymän, <a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">Sqoop REST API-opas</a>.

##<a name="next-steps"></a>Seuraavat vaiheet

Lisätietoa rakenteen HDInsight kanssa:

* [Hadoop-HDInsight Sqoop käyttäminen](hdinsight-use-sqoop.md)

Lisätietoja muista tavoista voit käsitellä Hadoop-HDInsight:

* [Hadoop HDInsight-rakenteen käyttäminen](hdinsight-use-hive.md)

* [Possu käyttäminen Hadoop-Hdinsightiin](hdinsight-use-pig.md)

* [Hadoop-HDInsight MapReduce käyttäminen](hdinsight-use-mapreduce.md)

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md




[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


