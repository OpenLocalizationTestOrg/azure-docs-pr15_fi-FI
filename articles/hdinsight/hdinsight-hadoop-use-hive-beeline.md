<properties
   pageTitle="Beeline käyttäminen rakenne-HDInsight (Hadoop) | Microsoft Azure"
   description="Opettele SSH avulla voit muodostaa yhteyden Hadoop-klusterin HDInsight- ja vuorovaikutteisesti Lähetä rakenteen kyselyjen avulla Beeline. Beeline on apuohjelma käsittelyyn HiveServer2 JDBC päälle."
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
   ms.date="10/10/2016"
   ms.author="larryfr"/>

#<a name="use-hive-with-hadoop-in-hdinsight-with-beeline"></a>Rakenteen käyttäminen HDInsight kanssa Beeline Hadoop

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Tässä artikkelissa kerrotaan, miten suojattu runko (SSH) avulla voit muodostaa yhteyden Linux-pohjaiset HDInsight-klusterin ja vuorovaikutteisesti Lähetä rakenteen kyselyjen [Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) komentorivin-työkalun avulla.

> [AZURE.NOTE] Beeline käyttää JDBC rakennetiedoston yhdistäminen. Lisätietoja rakenteen JDBC käyttämisestä on artikkelissa [etäyhteyden muodostaminen rakenne-Azure Hdinsightiin rakenne JDBC ohjaimen avulla](hdinsight-connect-hive-jdbc-driver.md).

##<a id="prereq"></a>Edellytykset

Tämän artikkelin seuraavien vaiheiden suorittamiseen tarvitset seuraavat:

* Linux-pohjaiset Hadoop HDInsight-klusterissa.

* SSH-asiakas. Linux, Unix ja Mac OS noudetaan SSH avulla. Windows-käyttäjien on ladattava asiakas, kuten [painovärit, muste](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

##<a id="ssh"></a>Yhdistä SSH

Yhteyden muodostaminen täydellinen toimialuenimi (FQDN) HDInsight-klusterin SSH-komennon avulla. FQDN on annoit klusterin, valitse **. azurehdinsight.net**. Esimerkiksi seuraavat muodostamaan nimeltä **myhdinsight**klusterin:

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Jos antamasi varmenteen avaimen SSH todennusta varten** , kun olet luonut HDInsight-klusterin, joudut ehkä määrittää yksityinen avain sijainnin järjestelmästä asiakasohjelman:

    ssh admin@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

**Jos antamasi salasanan SSH todennustavaksi** HDInsight-klusterin luonnin yhteydessä, tarvitset Anna pyydettäessä salasana.

SSH käyttämisestä HDInsight, katso lisätietoja [Käytön SSH Linux-pohjaiset Hadoop HDInsight Linux, OS X-ja Unix-kanssa](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-based-clients"></a>Painovärit, muste (Windows-asiakkaat)

Windows ei tarjoa valmiin SSH asiakas. On suositeltavaa käyttää **painovärit, muste**, joka voi ladata [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Lisätietoja painovärit, muste on artikkelissa [Käyttäminen SSH kanssa Linux-pohjaiset Hadoop-HDInsight Windows ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="beeline"></a>Beeline-komennolla

1. Kun yhteys on muodostettu, voit käynnistää Beeline käyttämällä seuraavaa:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin

    Tämä käynnistää Beeline asiakas- ja muodostaa JDBC URL-osoitteeseen. Tässä kohdassa `localhost` käytetään, koska HiveServer2 toimii sekä pää-klusterin solmut ja että käytössäsi on Beeline suoraan Valitse ensisijainen headnode.
    
    Kun komento on valmis, sinun vuoroon `jdbc:hive2://localhost:10001/>` kehote.

3. Beeline komennot alkavat yleensä `!` merkki, kuten `!help` näyttää ohjeen. Kuitenkin, `!` usein jätetään pois. Esimerkiksi `help` se toimii myös.

    Jos tarkastelet Ohje, huomaat, että `!sql`, jota käytetään suorittamaan HiveQL lauseita. HiveQL niin usein käytetään kuitenkin, että jätät pois edeltävän `!sql`. Seuraavat kaksi lausetta on oltava täsmälleen samat tulokset; näyttää taulukoiden tällä hetkellä käytettävissä rakenteen kautta:
    
        !sql show tables;
        show tables;
    
    Valitse uuden klusterin vain yhden taulukon luettelossa: __hivesampletable__.

4. Rakenne hivesampletable näytettävä käyttämällä seuraavaa:

        describe hivesampletable;
        
    Tämä palauttavat seuraavat tiedot:
    
        +-----------------------+------------+----------+--+
        |       col_name        | data_type  | comment  |
        +-----------------------+------------+----------+--+
        | clientid              | string     |          |
        | querytime             | string     |          |
        | market                | string     |          |
        | deviceplatform        | string     |          |
        | devicemake            | string     |          |
        | devicemodel           | string     |          |
        | state                 | string     |          |
        | country               | string     |          |
        | querydwelltime        | double     |          |
        | sessionid             | bigint     |          |
        | sessionpagevieworder  | bigint     |          |
        +-----------------------+------------+----------+--+

    Sarakkeet näkyvät taulukossa. Vaikka emme voi suorittaa joitakin kyselyitä, jotka perustuvat tiedoista, sen sijaan luodaan uusi taulukko, miten voi ladata tietoja rakenne ja rakenteen käyttäminen.
    
5. Kirjoita seuraavista väittämistä avulla mukana HDInsight-klusterin mallitiedot **log4jLogs** uuden taulukon luominen:

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Alikyselyn tehdä seuraavat toimet:

    * **DROP TABLE** - poistaa taulukon ja datatiedostoa, siltä varalta, kun taulukko on jo olemassa.
    * **Luo ulkoinen taulukko** - Luo uusi "ulkoinen" taulukko rakenne. Ulkoiset taulukot tallentamaan taulukkomäärityksen rakenne. Tietoja jää alkuperäiseen sijaintiin.
    * **RIVIN muoto** - kertoo rakenne tietojen muotoilun. Tällöin kunkin lokin kentät on erotettu toisistaan välilyönnillä.
    * **TALLENNETTU AS TEXTFILE sijainti** - kertoo rakenne, jossa tiedot on tallennettu (esimerkkitietoja /-hakemistosta) ja että se on tallennettu tekstinä.
    * **Valitse** - valitsee kaikki rivit, joissa sarakkeen **t4** sisältää arvon **[virhe]**määrä. Tämä olisi palauttaa arvon **3** on kolme rivit, jotka sisältävät arvon.
    * **INPUT__FILE__NAME LIKE "%.log"** - kertoo rakenne, joka on palauttaa tietoja ainoastaan loppuosaksi tiedostoista. loki. Tavallisesti vain sinulla olisi tiedot samaan rakenteeseen ja samaan kansioon kun kysely rakenne, mutta tässä esimerkissä lokitiedosto on tallennettu tietoja muihin tapoihin.

    > [AZURE.NOTE] Ulkoiset taulukot on käytettävä, vaikka odotat pohjana olevia tietoja voi päivittää ulkoisesta tietolähteestä, kuten automaattinen tiedot-lataus tai toinen MapReduce-toiminto, mutta haluat aina uusimmat tiedot käyttämällä kyselyjä rakenne.
    >
    > Onko pudottaminen ulkoisen taulukon **ei** Poista tiedot vain taulukkomäärityksen.
    
    Tämä komento pitäisi näyttää seuraavankaltaiselta:
    
        INFO  : Tez session hasn't been created yet. Opening session
        INFO  :
        
        INFO  : Status: Running (Executing on YARN cluster with App id application_1443698635933_0001)
        
        INFO  : Map 1: -/-      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0(+1)/1
        INFO  : Map 1: 1/1      Reducer 2: 1/1
        +----------+--------+--+
        |   sev    | count  |
        +----------+--------+--+
        | [ERROR]  | 3      |
        +----------+--------+--+
        1 row selected (47.351 seconds)

4. Jos haluat poistua Beeline, käytä `!quit`.

##<a id="file"></a>Suorita HiveQL-tiedosto

Beeline myös voidaan suorittaa HiveQL lauseet sisältävän tiedoston. Seuraavien vaiheiden avulla voit luoda tiedoston ja suorita sen Beeline.

1. Seuraavalla komennolla voit luoda uuden tiedoston nimeltä __query.hql__:

        nano query.hql
        
2. Kun editorin avautuu, Määritä seuraavat tiedoston sisällön. Tämä kysely-toiminto luo uuden "sisäinen" taulukon **errorLogs**:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Alikyselyn tehdä seuraavat toimet:

    * **Luo taulukko IF NOT olemassa** - luo taulukon, jos se ei ole jo olemassa. **Ulkoinen** avainsana ei käytetä, koska tämä on sisäinen taulukko, joka on tallennettu rakenne-tietovarasto ja hallitsee kokonaan rakenne.
    * **TALLENNETTU AS ORC** - tallentaa tiedot optimoitu rivin Sarakemuoto (ORC)-muodossa. Tämä on erittäin optimoitu ja tehokkaana muoto rakenteen tietojen tallentamista varten.
    * Lisää **Korvaa... Valitse** - valitsee rivit, jotka sisältävät **[virhe]** **log4jLogs** taulukosta ja valitse Lisää tiedot taulukkoon **errorLogs** .
    
    > [AZURE.NOTE] Toisin kuin ulkoiset taulukot pudottaminen sisäinen taulukko poistetaan myös pohjana olevia tietoja.
    
3. Jos haluat tallentaa tiedoston, __painamalla näppäinyhdistelmää Ctrl__+ ___X__ja __Y__ja lopuksi __Enter__.

4. Seuraavat avulla voit suorittaa tiedoston Beeline. Vaihda __HOSTNAME__ saadun aiemmin pään solmu ja __PYYDÄ__ järjestelmänvalvojatilin salasanan nimi:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -i query.hql

    > [AZURE.NOTE] `-i` Parametrin Beeline käynnistyy, käytetään lauseet query.hql-tiedosto ja jää Beeline `jdbc:hive2://localhost:10001/>` kehote. Voit suorittaa myös tiedoston avulla `-f` parametri, joka palauttaa Bash, kun tiedosto on käsitelty.

5. Varmista, että **errorLogs** taulukko on luotu, kaikki rivit tuotto **errorLogs**seuraavan lauseen avulla:

        SELECT * from errorLogs;

    Kolme tietoriviä palautetaan, kaikki sisältävän **[virhe]** -sarakkeen t4:
    
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | errorlogs.t1  | errorlogs.t2  | errorlogs.t3  | errorlogs.t4  | errorlogs.t5  | errorlogs.t6  | errorlogs.t7  |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | 2012-02-03    | 18:35:34      | SampleClass0  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 18:55:54      | SampleClass1  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 19:25:27      | SampleClass4  | [ERROR]       | incorrect     | id            |               |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        3 rows selected (1.538 seconds)

## <a name="more-about-beeline-connectivity"></a>Lisätietoja Beeline yhteys

Ohjeita tämän asiakirjan käyttäminen `localhost` muodostaa HiveServer2 klusterin headnode käytössä. Voit myös käyttää isäntänimi tai täydellinen toimialuenimi headnode ne edellyttävät lisätoimia prosessin (ohjeiden avulla selvää isäntänimi tai FQDN). Käyttämällä `localhost` riittää käytettäessä Beeline headnode.

Jos yhteyttä klusterin reuna-solmu on asennettu Beeline kanssa, sinun on käytettävä isäntänimi tai täydellinen toimialuenimi headnode muodostaa.

Jos sinulla on asennettu asiakastietokoneeseen yhteyttä klusterin ulkopuolella Beeline, voit muodostaa yhteyden seuraava komento. Korvaa __CLUSTERNAME__ HDInsight-klusterin nimen. Korvaa admin (HTTP sisäänkirjautuminen)-tilin salasanan __salasana__ .

    beeline -u 'jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;ssl=true?hive.server2.transport.mode=http;hive.server2.thrift.http.path=hive2' -n admin -p PASSWORD

Huomaa, että parametrit/URI on erilainen kuin kun käynnissä suoraan headnode tai reuna-solmun klusterin kuluessa. Tämä johtuu siitä klusterin internet-yhteyden käyttää julkisen yhdyskäytävään, joka reitittää liikenteen porttiin 443 päälle. Myös useita muita palveluita niitä julkaista porttiin 443, julkisen yhdyskäytävän kautta, jotta URI on erilainen kuin kun yhdistetään suoraan. Kun muodostat internet-todennusta myös istunnon antamalla salasana.

##<a id="summary"></a><a id="nextsteps"></a>Seuraavat vaiheet

Kuten näet, Beeline-komento on helposti suorittaa vuorovaikutteisesti HDInsight-klusterissa rakenteen kyselyjä.

Lisätietoa HDInsight-rakenne:

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

[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

