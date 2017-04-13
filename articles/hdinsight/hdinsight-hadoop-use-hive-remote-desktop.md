<properties
   pageTitle="Hadoop-rakenne ja Etätyöpöydän käyttäminen HDInsight | Microsoft Azure"
   description="Opettele muodostamaan HDInsight-klusterin Hadoop Etätyöpöydän avulla ja suorita sitten rakenne kyselyjen rakenne komentorivivalitsimet käyttöliittymän avulla."
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
   ms.date="09/06/2016"
   ms.author="larryfr"/>

# <a name="use-hive-with-hadoop-on-hdinsight-with-remote-desktop"></a>Rakenteen käyttäminen Hadoop-HDInsight etätyöpöydän kanssa

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Tässä artikkelissa opettele muodostamaan HDInsight-klusterin Etätyöpöydän avulla ja suorita rakenteen kyselyjen avulla rakenne käyttöliittymä (CLI).

> [AZURE.NOTE] Tässä asiakirjassa ei tarjoa HiveQL lausekkeita, jotka esimerkeissä käytetään mitä yksityiskohtainen kuvaus. Tietoja HiveQL, jota käytetään tässä esimerkissä on artikkelissa [Käytä rakenne ja Hadoop-Hdinsightista](hdinsight-use-hive.md).

##<a id="prereq"></a>Edellytykset

Tämän artikkelin seuraavien vaiheiden suorittamiseen tarvitset seuraavat:

* Windows-pohjaisesta HDInsight (Hadoop-HDInsight)-klusterin

* Asiakastietokone, Windows 10, ikkunan 8 tai Windows 7: ssä

##<a id="connect"></a>Etätyöpöytä yhteydessä

Etätyöpöytä käyttöön HDInsight-klusterin ja muodostaa siihen voidaan [muodostaa käyttämällä RDP HDInsight klustereihin](hdinsight-administer-use-management-portal.md#rdp)ohjeita noudattamalla.

##<a id="hive"></a>Rakenne-komennolla

Kun yhteys on HDInsight-klusterin työpöydälle, rakenteen käsitteleminen seuraavien vaiheiden avulla:

1. HDInsight työpöydältä aloittaa **Hadoop komentoriviltä käsin**.

2. Kirjoita Aloita rakenne-CLI seuraava komento:

        %hive_home%\bin\hive

    Kun CLI on alkanut, näyttöön tulee rakenne CLI kehote: `hive>`.

3. Käytä CLI, kirjoita seuraavista väittämistä voit luoda uuden taulukon **log4jLogs** mallitietojen avulla:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Alikyselyn tehdä seuraavat toimet:

    * **Taulukon**: poistaa taulukon ja datatiedosto, jos taulukko on jo olemassa.

    * **Luo ulkoinen taulukko**: Luo uusi "ulkoinen" taulukko rakenne. Ulkoiset taulukot tallentaa vain taulukkomäärityksen rakenne (tietojen jää alkuperäiseen sijaintiin).

        > [AZURE.NOTE] Ulkoiset taulukot on käytettävä, kun odotat pohjana olevia tietoja voi päivittää ulkoisesta tietolähteestä (esimerkiksi automaattinen tiedot-lataus) tai toinen MapReduce-toiminto, mutta haluat aina uusimmat tiedot käyttämällä kyselyjä rakenne.
        >
        > Onko pudottaminen ulkoisen taulukon **ei** Poista tiedot vain taulukkomäärityksen.

    * **RIVIN muoto**: kertoo rakenne tietojen muotoilun. Tällöin kunkin lokin kentät on erotettu toisistaan välilyönnillä.

    * **TALLENNETTU AS TEXTFILE sijainti**: kertoo rakenne, jossa tiedot on tallennettu (esimerkkitietoja /-hakemistosta) ja että se on tallennettu tekstinä.

    * **Valitse**: Valitsee kaikki rivit, joissa sarakkeen **t4** sisältää arvon **[virhe]**määrä. Tämä olisi palauttaa arvon **3** , koska tilikaudessa on kolme rivit, jotka sisältävät arvon.

    * **INPUT__FILE__NAME LIKE "%.log"** - kertoo rakenne, joka on palauttaa tietoja ainoastaan loppuosaksi tiedostoista. loki. Tämä rajoittaa haun sample.log tiedostoon, joka sisältää haluamasi tiedot ja säilyttää-palauttaminen tietoja muiden Esimerkki datatiedostot, jotka eivät vastaa määritimme rakenne.


4. Seuraavista väittämistä avulla voit luoda uuden "sisäinen" taulukon **errorLogs**:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Alikyselyn tehdä seuraavat toimet:

    * **Luo taulukko Jos ei ole olemassa**: Luo taulukon, jos se ei ole jo olemassa. **Ulkoinen** avainsana ei käytetä, koska tämä on sisäinen taulukko, joka on tallennettu rakenne-tietovarasto ja hallitsee kokonaan rakenne.

        > [AZURE.NOTE] Toisin kuin **Ulkoiset** taulukot pudottaminen sisäinen taulukko poistaa pohjana olevia tietoja.

    * **TALLENNETTU AS ORC**: tiedot tallennetaan optimoitu rivi (ORC) sarakemuodossa. Tämä on erittäin optimoitu ja tehokkaana muoto rakenteen tietojen tallentamista varten.

    * Lisää **Korvaa... Valitse**: valitsee **log4jLogs** -taulukosta rivit, jotka sisältävät **[virhe]**, lisää tiedot sitten **errorLogs** taulukkoon.

    Voit varmistaa, että vain rivit, jotka sisältävät **[virhe]** -sarakkeen t4 tallennettiin **errorLogs** taulukkoon, kaikki rivit tuotto **errorLogs**seuraavan lauseen avulla:

        SELECT * from errorLogs;

    Kolme riviä tietojen pitäisi voi palauttaa, kaikki sisältävän **[virhe]** -sarakkeen t4.

##<a id="summary"></a>Yhteenveto

Kuten näet, rakenne-komento on helppo tapa vuorovaikutteisesti rakenteen kyselyjen suorittaminen HDInsight-klusterissa, työ-tilaa ja hakea tulos.

##<a id="nextsteps"></a>Seuraavat vaiheet

Yleisiä tietoja HDInsight-rakenne:

* [Hadoop HDInsight-rakenteen käyttäminen](hdinsight-use-hive.md)

Lisätietoja muista tavoista voit käsitellä Hadoop-HDInsight:

* [Possu käyttäminen Hadoop-Hdinsightiin](hdinsight-use-pig.md)

* [Hadoop-HDInsight MapReduce käyttäminen](hdinsight-use-mapreduce.md)

Jos käytät Tez rakenteen kanssa, lue seuraavat tiedostot virheiden tiedot:

* [Käytä Windows-pohjaisesta HDInsight Tez-Käyttöliittymä](hdinsight-debug-tez-ui.md)

* [Linux-pohjaiset HDInsight Ambari Tez-näkymän käyttäminen](hdinsight-debug-ambari-tez-view.md)

[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

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


[Powershell-install-configure]: ../powershell-install-configure.md
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

