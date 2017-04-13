<properties
   pageTitle="Kysely, joka sisältää Hadoop-työkalujen rakenne Visual Studio | Microsoft Azure"
   description="Opettele rakenteen käyttäminen Visual Studio Hadoop työkaluilla HDInsight Hadoop."
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

#<a name="run-hive-queries-using-the-hdinsight-tools-for-visual-studio"></a>Visual Studio HDInsight-työkaluilla rakenne-kyselyt

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Tässä artikkelissa kerrotaan lähettäminen Visual Studio HDInsight-työkaluilla HDInsight-klusterin kyselyjä rakenne.

> [AZURE.NOTE] Tässä asiakirjassa ei tarjoa esimerkeissä käytetään HiveQL lauseet mitä yksityiskohtainen kuvaus. Tietoja HiveQL, jota käytetään tässä esimerkissä on artikkelissa [Käytä rakenne ja Hadoop-Hdinsightista](hdinsight-use-hive.md).

##<a id="prereq"></a>Edellytykset

Tämän artikkelin seuraavien vaiheiden suorittamiseen tarvitset seuraavasti.

* Azure Hdinsightiin (Hadoop-HDInsight)-klusterin (Linux tai Windows-pohjainen)

* Visual Studio (yksi seuraavista versioista):

    Visual Studio 2013 yhteisön/Professional ja Premium/Ultimate ja [Päivitä 4](https://www.microsoft.com/download/details.aspx?id=44921)

    Visual Studio 2015 (yhteisön/Enterprise)

- Visual studio HDInsight työkaluja. Artikkelissa on tietoja asennuksesta ja määrityksestä työkalujen [käyttäminen Visual Studio Hadoop työkaluja Hdinsightista](hdinsight-hadoop-visual-studio-tools-get-started.md) .

##<a id="run"></a>Visual Studio HDInsight-työkaluilla rakenne-kyselyt

1. Avaa **Visual Studio** ja valitse **Uusi** > **projektin** > **HDInsight** > **Rakenne sovelluksen**. Anna nimi tälle projektille.

2. Avaa **Script.hql** tiedosto, joka on luotu projekti ja liitä seuraavat HiveQL lauseet:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Alikyselyn tehdä seuraavat toimet:

    * **Taulukon**: poistaa taulukon ja datatiedosto, jos taulukko on jo olemassa.
    * **Luo ulkoinen taulukko**: Luo uusi "ulkoinen" taulukko rakenne. Ulkoiset taulukot tallentamaan taulukkomäärityksen rakenne (tietojen jää alkuperäiseen sijaintiin).

        > [AZURE.NOTE] Ulkoiset taulukot on käytettävä, kun odotat pohjana olevia tietoja voi päivittää ulkoisesta tietolähteestä (esimerkiksi automaattinen tiedot-lataus) tai toinen MapReduce-toiminto, mutta haluat aina uusimmat tiedot käyttämällä kyselyjä rakenne.
        >
        > Onko pudottaminen ulkoisen taulukon **ei** Poista tiedot vain taulukkomäärityksen.

    * **RIVIN muoto**: kertoo rakenne tietojen muotoilun. Tällöin kunkin lokin kentät on erotettu toisistaan välilyönnillä.
    * **TALLENNETTU AS TEXTFILE sijainti**: kertoo rakenne, jossa tiedot on tallennettu (esimerkkitietoja /-hakemistosta) ja että se on tallennettu tekstinä.
    * **Valitse**: Valitse kaikki rivit, joissa sarakkeen **t4** sisältävät arvon **[virhe]**määrä. Tämä olisi palauttaa arvon **3** , koska tilikaudessa on kolme rivit, jotka sisältävät arvon.
    * **INPUT__FILE__NAME LIKE "%.log"** - kertoo rakenne, joka on palauttaa tietoja ainoastaan viimeiset, tiedostoista. loki. Tämä rajoittaa haun sample.log tiedostoon, joka sisältää haluamasi tiedot ja säilyttää-palauttaminen tietoja muiden Esimerkki datatiedostot, jotka eivät vastaa määritimme rakenne.

3. Työkalurivin Valitse **HDInsight-klusterin** , jota haluat käyttää tätä kyselyä ja valitse sitten **Lähetä WebHCat** suorittaa rakenteen työn WebHCat lauseet. Voit myös lähettää työ käyttämällä __kautta HiveServer2 Suorita__ -painiketta, jos HiveServer2 ei käytettävissä klusterin-versiosi. **Rakenne yhteenveto** tulee näkyviin, ja näyttöön tulee käynnissä olevan projektin tietoja. **Päivitä** -linkin avulla voit päivittää työn, tiedot, ennen kuin **Työn tilaksi** muuttuu **Valmis**.

4. **Työn tulostus** -linkin avulla voit tarkastella työn tulos. Pitäisi näkyä `[ERROR] 3`, joka on arvo, SELECT-lauseessa.

5. Voit suorittaa rakenteen kyselyjen myös ilman, että luot projektin. **Palvelimen Explorer**Laajenna **Azure** > **HDInsight**HDInsight-palvelimen hiiren kakkospainikkeella ja valitse sitten **kirjoittaa kyselyn rakenne**.

6. **Temp.hql** asiakirja, joka tulee näkyviin lisää seuraavat HiveQL lauseet:

        set hive.execution.engine=tez;
        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Alikyselyn tehdä seuraavat toimet:

    * **Luo taulukko Jos ei ole olemassa**: Luo taulukon, jos se ei ole jo olemassa. **Ulkoinen** avainsana ei käytetä, koska tämä on sisäinen taulukko, joka on tallennettu rakenne-tietovarasto ja hallitsee kokonaan rakenne.

        > [AZURE.NOTE] Toisin kuin **Ulkoiset** taulukot pudottaminen sisäinen taulukko poistaa pohjana olevia tietoja.

    * **TALLENNETTU AS ORC**: tiedot tallennetaan optimoitu rivi (ORC) sarakemuodossa. Tämä on erittäin optimoitu ja tehokkaana muoto rakenteen tietojen tallentamista varten.
    * Lisää **Korvaa... Valitse**: valitsee **log4jLogs** -taulukosta rivit, jotka sisältävät **[virhe]**, lisää tiedot sitten **errorLogs** taulukkoon.

7. Valitse työkaluriviltä **Lähetä** työn suorittamista varten. **Tilan** avulla voit määrittää, että työ on suoritettu onnistuneesti.

8. Voit varmistaa, että projektin Vastatut ja luoda uuden taulukon, **Palvelimen** Resurssienhallinnassa ja laajenna **Azure** > **HDInsight** > HDInsight-klusterin > **Rakenne tietokantojen** > ja **Oletusarvo**. Pitäisi näkyä **errorLogs** ja **log4jLogs** -taulukon.

##<a id="summary"></a>Yhteenveto

Kuten näet, Visual Studio HDInsight-työkalut tarjoavat rakenteen kyselyjen suorittaminen HDInsight-klusterissa, työ-tilaa ja Nouda tulosteen helposti.

##<a id="nextsteps"></a>Seuraavat vaiheet

Yleisiä tietoja HDInsight-rakenne:

* [Hadoop HDInsight-rakenteen käyttäminen](hdinsight-use-hive.md)

Lisätietoja muista tavoista voit käsitellä Hadoop-HDInsight:

* [Possu käyttäminen Hadoop-Hdinsightiin](hdinsight-use-pig.md)

* [Hadoop-HDInsight MapReduce käyttäminen](hdinsight-use-mapreduce.md)

Jos haluat lisätietoja Visual Studio HDInsight-työkalut:

* [Visual Studio HDInsight Työkalut aloittaminen](../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md)


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



[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png
