<properties
    pageTitle="Katso, mitä rakenne ja Opi käyttämään HiveQL | Microsoft Azure"
    description="Tietoja Apache rakenne ja miten sitä käytetään Hadoop HDInsight. Valitse rakenne-suoritetaan ja HiveQL avulla voit analysoida Apache log4j mallitiedosto."
    keywords="hiveql, mikä on rakenne"
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
    ms.date="09/19/2016"
    ms.author="larryfr"/>

# <a name="use-hive-and-hiveql-with-hadoop-in-hdinsight-to-analyze-a-sample-apache-log4j-file"></a>Käyttäminen Hadoop HDInsight-rakenne ja HiveQL analysointiin Apache log4j mallitiedosto

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]


Tässä opetusohjelmassa Opettele käyttämään HDInsight Apache rakenne Hadoop ja valitse rakenne-projektin suorittaminen. Opit myös HiveQL ja analysoida Apache log4j mallitiedosto.

##<a id="why"></a>Mikä on rakenne ja miksi sitä käytetään?
[Apache rakenne](http://hive.apache.org/) on varaston tietojärjestelmä Hadoop, joka mahdollistaa tietojen yhteenvedon, kyselyt ja tietojen analysointi HiveQL (kyselykieli samalla tavalla kuin SQL) avulla. Rakenteen voidaan tutkia tietoja vuorovaikutteisesti tai luoda Uudelleenkäytettävän erä töitä.

Rakenne mahdollistaa projektirakenne suurelta erimuotoisia tietoja. Kun olet määrittänyt rakennetta, voit käyttää rakenteen kyselyn tiedot ilman Java tai MapReduce. **HiveQL** Voit kirjoittaa lausekkeita, jotka muistuttavat T-SQL-kyselyitä (rakenne-kyselykielen).

Rakenteen ymmärtää käsittelemisestä rakenteisia ja puolirakenteisia tietoja, kuten tekstitiedostoista, jossa kentät on erotettu toisistaan tiettyjä merkkejä. Rakenteen tukee myös monimutkaisia tai epäsäännöllisen Jäsennettyjen tietojen mukautettu **Sarjatoiminto/deserializers (SerDe)** . Lisätietoja on artikkelissa [käyttäminen mukautettujen JSON-SerDe HDInsight kanssa](http://blogs.msdn.com/b/bigdatasupport/archive/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight.aspx).

## <a name="user-defined-functions-udf"></a>Käyttäjän määrittämät funktiot (UDF)

Rakenteen voi myös laajentaa **käyttäjän määrittämiä funktioita (UDF)**kautta. UDF voit toteuttaa toimintoja tai logiikka, joka ei ole helposti mallintaa HiveQL. Esimerkki UDF käytön rakenne on seuraavissa artikkeleissa:

* [Käytä Java käyttäjän määrittämät-funktio ja rakenne](hdinsight-hadoop-hive-java-udf.md)

* [Rakenne ja HDInsight Possu Python käyttäminen](hdinsight-python.md)

* [Käyttää C# rakenne ja HDInsight Possu](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

* [Mukautettu rakenne UDF lisääminen Hdinsightiin](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

* [Mukautettu rakenne UDF Esimerkki päivämäärän ja kellonajan muodosta muunnettava rakenteen aikaleima](https://github.com/Azure-Samples/hdinsight-java-hive-udf)

## <a name="hive-internal-tables-vs-external-tables"></a>Sisäisiä taulukoita ja ulkoiset taulukot rakenne

Tarvitset lisätietoja sisäinen taulukko rakenne ja ulkoisen taulukon muutamia asioita:

- **Luo taulukko** -komento luo sisäinen taulukko. Tiedosto on sijaittava oletusarvo-säilössä.
- **Luo taulukko** -komento siirtyy datatiedoston /hive/varasto/<TableName> kansio.
- **Luo ulkoinen taulukko** -komento luo ulkoisen taulukon. Datatiedosto voi sijaita oletusarvo-säilö ulkopuolella.
- **Luo ulkoinen taulukko** -komento ei siirry datatiedoston.
- **Luo ulkoinen taulukko** -komento ei salli kaikki kansiot sijainnissa. Tämä on syytä, miksi opetusohjelman tekee sample.log-tiedoston kopio.

Lisätietoja on artikkelissa [HDInsight: rakenne sisäisten ja ulkoisten taulukoiden johdanto][cindygross-hive-tables].


##<a id="data"></a>Esimerkki tiedoista, äänitiedoston Apache log4j

Tässä esimerkissä käytetään *log4j* mallitiedosto, jotka on tallennettu **/example/data/sample.log** -blob-tallennustilan säilö. Kunkin lokin tiedoston sisälle koostuu rivi-kentät, joka sisältää `[LOG LEVEL]` -kenttä näyttää tyyppi ja vakavuus, esimerkiksi:

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

Edellisen esimerkin lokin taso on virhe.

> [AZURE.NOTE] Voit myös luoda log4j tiedoston [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) kirjaaminen-työkalun avulla ja lataa tiedosto blob-säilö. Saat ohjeet [HDInsight lataa tiedot](hdinsight-upload-data.md) . Saat lisätietoja siitä, miten HDInsight käyttämisestä Azure-Blob-säiliö [Käytä Azure-Blob-säiliö HDInsight kanssa](hdinsight-hadoop-use-blob-storage.md).

Esimerkkitiedot tallennetaan Azure-Blob-säiliö, jota HDInsight käyttää käyttöjärjestelmän. HDInsight käyttää BLOB **wasb** etuliitettä käyttämällä tallennettuja tiedostoja. Voit käyttää sample.log-tiedosto, käytä seuraavaa syntaksia:

    wasbs:///example/data/sample.log

Koska Azure-Blob-säiliö tallennetaan oletusarvoisesti HDInsight, voit käyttää tiedoston **/example/data/sample.log** -HiveQL avulla.

> [AZURE.NOTE] Syntaksi, **wasbs: / / /**, käytetään oletusarvon tallennustilan säiliön HDInsight-klusterin tallennettuja tiedostoja. Jos olet määrittänyt lisätallennustilaa tilit, kun yhteyttä klusterin valmisteltu ja haluat käyttää näissä tileissä tallennettuja tiedostoja, voit käyttää tietoja määrittämällä säilö nimi ja tallennustilaa tilin osoite, esimerkiksi **wasbs://mycontainer@mystorage.blob.core.windows.net/example/data/sample.log**.

##<a id="job"></a>Esimerkki projektin: projektin sarakkeiden erotinmerkkejä tietojen sivulle

HiveQL seuraavista väittämistä projektin sarakkeiden sivulle erotinmerkkejä tiedot, jotka on tallennettu **wasbs: / / / / esimerkkitietoja** directory:

    set hive.execution.engine=tez;
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

Edellisen esimerkin HiveQL lauseet tehdä seuraavat toimet:

* __Määritä hive.execution.engine=tez;__:, suorittamisen moduuli käyttämään Tez. Käytä Tez sijasta MapReduce tarjota kyselyn suorituskykyä kasvaa. Lisätietoja Tez on kohdassa [Käyttö Apache Tez parannettu suorituskyky](#usetez) .

    > [AZURE.NOTE] Tämä lause on vain tarvitaan Windows-pohjaisesta HDInsight-klusterin; käytettäessä Tez on oletusarvo-suorittamisen ohjelma Linux-pohjaiset Hdinsightista.

* **Taulukon**: poistaa taulukon ja datatiedosto, jos taulukko on jo olemassa.
* **Luo ulkoinen taulukko**: Luo uuden **ulkoisen** taulukon rakennetta. Ulkoiset taulukot tallentaa vain taulukkomäärityksen rakenne; tiedot on vasen alkuperäiseen sijaintiin ja alkuperäisessä muodossaan.
* **RIVIN muoto**: kertoo rakenne tietojen muotoilun. Tällöin kunkin lokin kentät on erotettu toisistaan välilyönnillä.
* **TALLENNETTU AS TEXTFILE sijainti**: kertoo rakenne, jossa tiedot on tallennettu (esimerkkitietoja /-hakemistosta) ja että se on tallennettu tekstinä. Tiedot voidaan yhdessä tiedostossa tai eri useita tiedostot kansiossa.
* **Valitse**: Valitsee kaikki rivit, joissa sarakkeen **t4** sisältää arvon **[virhe]**määrä. Tämä olisi palauttaa arvon **3** , koska tilikaudessa on kolme rivit, jotka sisältävät arvon.
* **INPUT__FILE__NAME LIKE "%.log"** - kertoo rakenne, joka on palauttaa tietoja ainoastaan loppuosaksi tiedostoista. loki. Tämä rajoittaa haun sample.log tiedostoon, joka sisältää haluamasi tiedot ja säilyttää-palauttaminen tietoja muiden Esimerkki datatiedostot, jotka eivät vastaa määritimme rakenne.

> [AZURE.NOTE] Ulkoiset taulukot käytetään, kun odotat pohjana olevia tietoja voi päivittää ulkoisesta tietolähteestä, kuten automaattinen tietojen lataus tai toinen MapReduce-toiminto ja haluat aina uusimmat tiedot käyttämällä kyselyjä rakenne.
>
> Onko pudottaminen ulkoisen taulukon **ei** Poista tiedot, se poistaa taulukkomääritys.

Kun olet luonut ulkoisen taulukon, seuraavista väittämistä käytetään Luo **sisäinen** taulukko.

    set hive.execution.engine=tez;
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs
    SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';

Alikyselyn tehdä seuraavat toimet:

* **Luo taulukko Jos ei ole olemassa**: Luo taulukon, jos se ei ole jo olemassa. **Ulkoinen** avainsana ei käytetä, koska tämä on sisäinen taulukko, joka on tallennettu rakenne-tietovarasto ja hallitsee kokonaan rakenne.
* **TALLENNETTU AS ORC**: tallentaa tiedot optimoitu rivin Sarakemuoto (ORC)-muodossa. Tämä on erittäin optimoitu ja tehokkaana muoto rakenteen tietojen tallentamista varten.
* Lisää **Korvaa... Valitse**: valitsee rivien **log4jLogs** taulukon, joka sisältää **[virheen]**ja lisää sitten **errorLogs** taulukkoon tiedot.

> [AZURE.NOTE] Toisin kuin ulkoiset taulukot pudottaminen sisäinen taulukko poistaa pohjana olevia tietoja.

##<a id="usetez"></a>Käytä Apache Tez suorituskyvyn paraneminen

[Apache Tez](http://tez.apache.org) on kehys, joka mahdollistaa tietojen paljon käyttävät sovellukset, kuten rakennetta, voit suorittaa tehokkaammin asteikko. HDInsight uusimman version rakenne tukee Tez käytössä. Tez on käytössä oletusarvoisesti Linux-pohjaiset HDInsight klustereiden.

> [AZURE.NOTE] Tez ei ole käytössä oletusarvoisesti Windows-pohjaisesta HDInsight klustereiden ja se on otettava käyttöön. Kyselyn rakenne on määritettävä hyödyntää Tez seuraava arvo:
>
> ```set hive.execution.engine=tez;```
>
>Tämä voi lähettää kyselyn kohti välein asettamalla kyselyn alussa. Voit myös määrittää tämä on oletusarvoisesti klusterissa määrittämällä määritys-arvo, kun luot klusterin. Löydät lisätietoja [valmistelu HDInsight](hdinsight-provision-clusters.md)klustereissa.

[Rakenne Tez rakenne tiedostojen](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez) sisältävät tietoja käyttöönoton vaihtoehtoja ja säätäminen käyttömahdollisuudet määrän.

Helpota virheenkorjaus työt suoritettiin käyttämällä Tez, Hdinsightista on seuraavassa verkossa, jotta voit tarkastella Tez töiden käyttöliittymät:

* [Käytä Windows-pohjaisesta HDInsight Tez-Käyttöliittymä](hdinsight-debug-tez-ui.md)

* [Linux-pohjaiset HDInsight Ambari Tez-näkymän käyttäminen](hdinsight-debug-ambari-tez-view.md)

##<a id="run"></a>Valitse, miten voit suorittaa HiveQL työn

HDInsight suorittamisen HiveQL työt käyttämällä usealla eri tavalla. Seuraavan taulukon avulla voit päättää, mitä menetelmä sopii sinulle ja noudata vaiheittainen linkkiä.

| **Käytä tätä** , jos haluat...                                                     | .. .an **Vuorovaikutteinen** käyttöliittymä | ... **Eräkäsittely** | .. .korvaava **klusterin käyttöjärjestelmä** | .. .from **Asiakkaan käyttöjärjestelmä** |
|:--------------------------------------------------------------------------------|:---------------------------:|:-----------------------:|:------------------------------------------|:-----------------------------------------|
| [Rakenne-näkymä](hdinsight-hadoop-use-hive-ambari-view.md) | ✔ | ✔ | Linux | Kaikki (selainpohjainen) |
| [Beeline-komentoa (SSH-istunto)](hdinsight-hadoop-use-hive-beeline.md)                                         |              ✔              |            ✔            | Linux                                     | Linux, Unix, Mac OS x: ssä tai Windows        |
| [Rakenne-komentoa (SSH-istunto)](hdinsight-hadoop-use-hive-ssh.md)                                         |              ✔              |            ✔            | Linux                                     | Linux, Unix, Mac OS x: ssä tai Windows        |
| [Kääntö](hdinsight-hadoop-use-hive-curl.md)                                       |           &nbsp;            |            ✔            | Linux- tai Windows                          | Linux, Unix, Mac OS x: ssä tai Windows        |
| [Kyselyn konsoli](hdinsight-hadoop-use-hive-query-console.md)                     |           &nbsp;            |            ✔            | Windows                                   | Kaikki (selainpohjainen)                            |
| [HDInsight tools for Visual Studio](hdinsight-hadoop-use-hive-visual-studio.md) |           &nbsp;            |            ✔            | Linux- tai Windows                          | Windows                                  |
| [Windows PowerShellin](hdinsight-hadoop-use-hive-powershell.md)                   |           &nbsp;            |            ✔            | Linux- tai Windows                          | Windows                                  |
| [Etätyöpöytä](hdinsight-hadoop-use-hive-remote-desktop.md)                   |              ✔              |            ✔            | Windows                                   | Windows                                  |

## <a name="running-hive-jobs-on-azure-hdinsight-using-on-premises-sql-server-integration-services"></a>Azure Hdinsightiin rakenteen työt käyttämällä paikallisen SQL Server-Integrointipalveluilla käynnissä

Voit myös rakenteen suoritetaan SQL Server Integration Services (SSIS). Azure Feature Packin SSIS sisältää seuraavat osat, jotka rakenteen työt käyttäminen Hdinsightista.


- [Tehtävän Azure HDInsight-rakenne][hivetask]
- [Azure tilauksen Yhteyksienhallinnan][connectionmanager]


Lue lisää Azure Feature Packin SSIS [tähän][ssispack].


##<a id="nextsteps"></a>Seuraavat vaiheet

Nyt kun olet tutustunut rakenne on ja miten sitä käytetään Hadoop HDInsight, seuraavat linkkien avulla voit tutkia muita esimerkkejä Azure Hdinsightiin.


- [Tietojen lataaminen Hdinsightiin][hdinsight-upload-data]
- [Possu käyttäminen Hdinsightiin][hdinsight-use-pig]
- [Sqoop käyttäminen Hdinsightiin](hdinsight-use-sqoop.md)
- [Oozie käyttäminen Hdinsightiin](hdinsight-use-oozie.md)
- [MapReduce työt käyttäminen Hdinsightiin][hdinsight-use-mapreduce]

[check]: ./media/hdinsight-use-hive/hdi.checkmark.png

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/
[hivetask]: http://msdn.microsoft.com/library/mt146771(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx

[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md


[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-get-started.md

[Powershell-install-configure]: ../powershell-install-configure.md
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png


[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx
