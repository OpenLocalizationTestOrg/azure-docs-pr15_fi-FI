<properties
   pageTitle="Hadoop rakenteen käyttäminen HDInsight kääntö | Microsoft Azure"
   description="Lue, miten voit lähettää etäyhteyden Possu työt käyttämällä kääntö Hdinsightista."
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
   ms.date="09/07/2016"
   ms.author="larryfr"/>

#<a name="run-hive-queries-with-hadoop-in-hdinsight-with-curl"></a>Näyttää rakenne kyselyjen HDInsight kanssa kääntö Hadoop

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Tässä asiakirjassa kerrotaan, voit käyttää kääntö Azure HDInsight-klusterissa Hadoop rakenteen kyselyt.

Kääntö käytetään näytetään, miten voit käsitellä HDInsight raaka pyyntöjen avulla voi suorittaa, valvoa ja hakea tulosten rakenne. Tämä toimii käyttämällä WebHCat REST API (aiemmalta nimeltään Templeton) HDInsight-klusterin myöntämä.

> [AZURE.NOTE] Jos ovat jo tuttuja Linux-pohjaiset Hadoop palvelinten käyttäminen, mutta ole aiemmin käyttänyt HDInsight, katso [huomioitavia seikkoja Hadoop-Linux-pohjaiset Hdinsightista](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Edellytykset

Tämän artikkelin seuraavien vaiheiden suorittamiseen tarvitset seuraavat:

* Hadoop HDInsight-klusterissa (Linux tai Windows-pohjainen)

* [Kääntö](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a id="curl"></a>Rakenne-kyselyjen suorittaminen kääntö avulla

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

    URL-Osoitteen **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**alkuun ovat samat kaikki palvelupyynnöt. Polun **/status/Status**tarkoittaa, että pyyntö palauttaa tilan WebHCat (tunnetaan myös nimellä Templeton) palvelimen. Voit myös pyytää rakenne-version avulla seuraava komento:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/version/hive

    Tämä tulee palauttaa vastauksen seuraavankaltaiselta:

        {"module":"hive","version":"0.13.0.2.1.6.0-2103"}

2. Voit luoda uuden taulukon **log4jLogs**käyttämällä seuraavaa:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;DROP+TABLE+log4jLogs;CREATE+EXTERNAL+TABLE+log4jLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+ROW+FORMAT+DELIMITED+FIELDS+TERMINATED+BY+' '+STORED+AS+TEXTFILE+LOCATION+'wasbs:///example/data/';SELECT+t4+AS+sev,COUNT(*)+AS+count+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log'+GROUP+BY+t4;" -d statusdir="wasbs:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive

    Käyttää tätä komentoa parametrit ovat seuraavat:

    * **-d** - jälkeen `-G` ei käytetä, pyyntö tulee oletusarvon mukaan POST-menetelmää. `-d`määrittää arvot, jotka lähetetään pyyntö kanssa.

        * **user.name** - käyttäjä, joka toimii komento.

        * **Suorita** - HiveQL lauseet suorittamiseen.

        * **statusdir** - kansio, joka työn tila kirjoitetaan.

    Alikyselyn tehdä seuraavat toimet:

    * **DROP TABLE** - poistaa taulukon ja datatiedostoa, jos taulukko on jo olemassa.

    * **Luo ulkoinen taulukko** - Luo uusi "ulkoinen" taulukko rakenne. Ulkoiset taulukot tallentaa vain taulukkomäärityksen rakenne. Tietoja jää alkuperäiseen sijaintiin.

        > [AZURE.NOTE] Ulkoiset taulukot on käytettävä, vaikka odotat pohjana olevia tietoja voi päivittää ulkoisesta tietolähteestä, kuten automaattinen tiedot-lataus tai toinen MapReduce-toiminto, mutta haluat aina uusimmat tiedot käyttämällä kyselyjä rakenne.
        >
        > Onko pudottaminen ulkoisen taulukon **ei** Poista tiedot vain taulukkomäärityksen.

    * **RIVIN muoto** - kertoo rakenne tietojen muotoilun. Tällöin kunkin lokin kentät on erotettu toisistaan välilyönnillä.

    * **TALLENNETTU AS TEXTFILE sijainti** - kertoo rakenne, jossa tiedot on tallennettu (esimerkkitietoja /-hakemistosta) ja että se on tallennettu tekstinä.

    * **Valitse** - valitsee kaikki rivit, joissa sarakkeen **t4** sisältää arvon **[virhe]**määrä. Tämä olisi palauttaa arvon **3** on kolme rivit, jotka sisältävät arvon.

    > [AZURE.NOTE] Huomaa, että HiveQL lauseet välien korvataan `+` merkki kääntö käytettäessä. Tarjottu arvot, jotka sisältävät välilyönnin erotin, kuten ei korvataan `+`.

    * **INPUT__FILE__NAME LIKE '% 25.log'** - hakua vain viimeiset, tiedostojen käyttäminen tämän rajoitukset. loki. Jos tämä ei ole määritetty, rakenne yrittää etsiä kaikki tiedostot tähän kansioon ja sen alihakemistoista, mukaan lukien tiedostot, jotka eivät vastaa määritetty tässä taulukossa sarakkeen rakenne.

    > [AZURE.NOTE] Huomaa, että % 25 on URL-osoite koodattu %, lomakkeen, jotta todellinen-ehto on `like '%.log'`. Prosenttia on on oltava URL-koodattava, sitä kohdellaan erikoismerkki URL-osoitteet.

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

6. Seuraavista väittämistä avulla voit luoda uuden "sisäinen" taulukon **errorLogs**:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;CREATE+TABLE+IF+NOT+EXISTS+errorLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+STORED+AS+ORC;INSERT+OVERWRITE+TABLE+errorLogs+SELECT+t1,t2,t3,t4,t5,t6,t7+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log';SELECT+*+from+errorLogs;" -d statusdir="wasbs:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive

    Alikyselyn tehdä seuraavat toimet:

    * **Luo taulukko IF NOT olemassa** - luo taulukon, jos se ei ole jo olemassa. **Ulkoinen** avainsana ei käytetä, koska tämä on sisäinen taulukko, joka on tallennettu rakenne-tietovarasto ja hallitsee kokonaan rakenne.

        > [AZURE.NOTE] Toisin kuin ulkoiset taulukot pudottaminen sisäinen taulukko poistetaan myös pohjana olevia tietoja.

    * **TALLENNETTU AS ORC** - tallentaa tiedot optimoitu rivin Sarakemuoto (ORC)-muodossa. Tämä on erittäin optimoitu ja tehokkaana muoto rakenteen tietojen tallentamista varten.
    * Lisää **Korvaa... Valitse** - valitsee rivit, jotka sisältävät **[virhe]** **log4jLogs** taulukosta ja valitse Lisää tiedot taulukkoon **errorLogs** .
    * **Valitse** - valitsee kaikki rivit taulukosta **errorLogs** .

7. Käytä palauttaa tilan tarkistaminen työ Työn tunnus. Kun onnistui, avulla Azure CLI Mac, Linux ja Windows kuvatulla aiemmin ladata ja katsella tulokset. Tulos on sisällettävä kolme rivit, jotka sisältävät **[virhe]**.


##<a id="summary"></a>Yhteenveto

Osoittanut tässä asiakirjassa, voit käyttää raaka HTTP-pyynnön suorittaminen, valvonta ja Tarkastele tuloksia rakennetta työpaikkojen HDInsight-klusterin.

Katso lisätietoja tässä artikkelissa käytetyt REST-liittymän, <a href="https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference" target="_blank">WebHCat viittaus</a>.

##<a id="nextsteps"></a>Seuraavat vaiheet

Lisätietoa rakenteen HDInsight kanssa:

* [Hadoop HDInsight-rakenteen käyttäminen](hdinsight-use-hive.md)

Lisätietoja muista tavoista voit käsitellä Hadoop-HDInsight:

* [Possu käyttäminen Hadoop-Hdinsightiin](hdinsight-use-pig.md)

* [Hadoop-HDInsight MapReduce käyttäminen](hdinsight-use-mapreduce.md)

Jos käytät Tez rakenteen kanssa, lue seuraavat tiedostot virheiden tiedot:

* [Käytä Windows-pohjaisesta HDInsight Tez-Käyttöliittymä](hdinsight-debug-tez-ui.md)

* [Linux-pohjaiset HDInsight Ambari Tez-näkymän käyttäminen](hdinsight-debug-ambari-tez-view.md)

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


