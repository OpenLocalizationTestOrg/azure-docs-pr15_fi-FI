<properties
   pageTitle="Rakenne-liittymän käyttäminen HDInsight (Hadoop) | Microsoft Azure"
   description="Opettele käyttämään rakenne-liittymän Linux-pohjaiset HDInsight-klusterin kanssa. Opettele muodostamaan käyttämällä SSh HDInsight-klusterin ja valitse rakenne-liittymän avulla voit suorittaa vuorovaikutteisesti kyselyjä."
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
   ms.date="10/04/2016"
   ms.author="larryfr"/>

# <a name="use-hive-with-hadoop-in-hdinsight-with-ssh"></a>Rakenteen käyttäminen HDInsight kanssa SSH Hadoop

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Tässä artikkelissa kerrotaan käyttämisestä suojattu runko (SSH) Hadoop Azure HDInsight-klusterissa yhdistäminen ja vuorovaikutteisesti lähettää rakenteen kyselyjen rakenteen komentorivivalitsimet käyttöliittymän (CLI) avulla.

> [AZURE.IMPORTANT] Kun rakenne-komento on käytettävissä Linux-pohjaiset HDInsight klustereiden, kannattaa harkita Beeline. Beeline on uudempaa asiakasohjelmaa käsittelyyn rakenne ja HDInsight-klusterin mukana. Saat lisätietoja käytössä [Käytä rakenne ja HDInsight kanssa Beeline Hadoop](hdinsight-hadoop-use-hive-beeline.md).

##<a id="prereq"></a>Edellytykset

Tämän artikkelin seuraavien vaiheiden suorittamiseen tarvitset seuraavat:

* Linux-pohjaiset Hadoop HDInsight-klusterissa.

* SSH-asiakas. Linux, Unix ja Mac OS noudetaan SSH avulla. Windows-käyttäjien on ladattava asiakas, kuten [painovärit, muste](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

##<a id="ssh"></a>Yhdistä SSH

Yhteyden muodostaminen täydellinen toimialuenimi (FQDN) HDInsight-klusterin SSH-komennon avulla. FQDN on annoit klusterin, valitse **. azurehdinsight.net**. Esimerkiksi seuraavat muodostamaan nimeltä **myhdinsight**klusterin:

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Jos antamasi varmenteen avaimen SSH todennusta varten** , kun olet luonut HDInsight-klusterin, joudut ehkä määrittää yksityinen avain sijainnin järjestelmästä asiakasohjelman:

    ssh -i ~/mykey.key admin@myhdinsight-ssh.azurehdinsight.net

**Jos antamasi salasanan SSH todennustavaksi** HDInsight-klusterin luonnin yhteydessä, tarvitset Anna pyydettäessä salasana.

SSH käyttämisestä HDInsight, katso lisätietoja [Käytön SSH Linux-pohjaiset Hadoop HDInsight Linux, OS X-ja Unix-kanssa](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-based-clients"></a>Painovärit, muste (Windows-asiakkaat)

Windows ei tarjoa valmiin SSH asiakas. On suositeltavaa käyttää **painovärit, muste**, joka voi ladata [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Lisätietoja painovärit, muste on artikkelissa [Käyttäminen SSH kanssa Linux-pohjaiset Hadoop-HDInsight Windows ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="hive"></a>Rakenne-komennolla

2. Kun yhteys on muodostettu, Käynnistä rakenne-CLI käyttämällä seuraava komento:

        hive

3. Käytä CLI, kirjoita seuraavista väittämistä mallitietoja käyttämällä **log4jLogs** uuden taulukon luominen:

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
    * **INPUT__FILE__NAME LIKE "%.log"** - kertoo rakenne, joka on palauttaa tietoja ainoastaan loppuosaksi tiedostoista. loki. Tämä rajoittaa haun sample.log tiedostoon, joka sisältää haluamasi tiedot ja säilyttää-palauttaminen tietoja muiden Esimerkki datatiedostot, jotka eivät vastaa määritimme rakenne.

    > [AZURE.NOTE] Ulkoiset taulukot on käytettävä, vaikka odotat pohjana olevia tietoja voi päivittää ulkoisesta tietolähteestä, kuten automaattinen tiedot-lataus tai toinen MapReduce-toiminto, mutta haluat aina uusimmat tiedot käyttämällä kyselyjä rakenne.
    >
    > Onko pudottaminen ulkoisen taulukon **ei** Poista tiedot vain taulukkomäärityksen.

4. Seuraavista väittämistä avulla voit luoda uuden "sisäinen" taulukon **errorLogs**:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Alikyselyn tehdä seuraavat toimet:

    * **Luo taulukko IF NOT olemassa** - luo taulukon, jos se ei ole jo olemassa. **Ulkoinen** avainsana ei käytetä, koska tämä on sisäinen taulukko, joka on tallennettu rakenne-tietovarasto ja hallitsee kokonaan rakenne.
    * **TALLENNETTU AS ORC** - tallentaa tiedot optimoitu rivin Sarakemuoto (ORC)-muodossa. Tämä on erittäin optimoitu ja tehokkaana muoto rakenteen tietojen tallentamista varten.
    * Lisää **Korvaa... Valitse** - valitsee rivit, jotka sisältävät **[virhe]** **log4jLogs** taulukosta ja valitse Lisää tiedot taulukkoon **errorLogs** .

    Voit varmistaa, että vain ne rivit, joka sisältää **[virhe]** -sarakkeen t4 tallennettiin **errorLogs** taulukkoon, käytä palauttaa kaikki rivit **errorLogs**seuraavan lauseen:

        SELECT * from errorLogs;

    Kolme riviä tietojen pitäisi voi palauttaa, kaikki sisältävän **[virhe]** -sarakkeen t4.

    > [AZURE.NOTE] Toisin kuin ulkoiset taulukot pudottaminen sisäinen taulukko poistetaan myös pohjana olevia tietoja.

##<a id="summary"></a>Yhteenveto

Kuten näet, rakenne-komento on vuorovaikutteisesti rakenteen kyselyjen suorittaminen HDInsight-klusterissa, työ-tilaa ja Nouda tulosteen helposti.

##<a id="nextsteps"></a>Seuraavat vaiheet

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


[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png

