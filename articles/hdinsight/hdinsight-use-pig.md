<properties
   pageTitle="Käytä Hadoop Possu HDInsight | Microsoft Azure"
   description="Opettele Possu käyttäminen Hadoop-Hdinsightista."
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
   ms.date="09/14/2016"
   ms.author="larryfr"/>

# <a name="use-pig-with-hadoop-on-hdinsight"></a>Possu käyttäminen Hadoop-Hdinsightiin

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

[Apache Possu](http://pig.apache.org/) on luomisen Hadoop ohjelmien avulla ohjeita nimeltään *Possu latinalaisen*kielen. Possu on vaihtoehtoinen Java *MapReduce* ratkaisujen luomiseen ja se sisältyy Azure Hdinsightiin.

Tässä artikkelissa opit, miten voit käyttää Possu Hdinsightista.

##<a id="why"></a>Possu käyttötarkoitus

Yksi haasteiden tietojen käsittelemistä käyttämällä MapReduce Hadoop on käyttöönoton käsittely logiikan vain kartta ja Vähennä-toimintoa käyttämällä. Monimutkainen käsittelyä varten sinun on usein Jaa käsittely useita MapReduce toiminnoista, joita ovat ketjutettu yhdessä halutun tuloksen saavuttamiseksi.

Sen sijaan, että pakottaminen voit käyttää vain kartta ja vähentää Funktiot Possu avulla voit määrittää käsittely muunnoksia, joka tuottaa haluttu tulos kulkee tiedot joukkona.

Possu latinalaisen kielen voit tiedonkulun raaka syöttö-– vähintään yksi muunnoksia tuottamaan haluttu tulos-kuvaa. Possu latinalainen ohjelmat toimi tätä yleisen mallia:

- **Lataa**: tietojen toiminnat tiedostojärjestelmästä lukeminen
- **Muunna**: tietojen muokkaamiseen
- **Dump tai store**: Tulosta näytön tietojen tai tallentaa sen käsittelyä varten

Possu latinalainen tukee myös käyttäjän määrittämiä funktioita (UDF), jonka avulla voit kutsua ulkoisia osat, jotka toteuttavat logiikka, joka on vaikea Possu latinalainen mallin.

Saat lisätietoja Possu latinalainen [Possu latinalainen viittaus manuaalinen 1](http://pig.apache.org/docs/r0.7.0/piglatin_ref1.html) ja [Possu latinalainen viittaus manuaalinen 2](http://pig.apache.org/docs/r0.7.0/piglatin_ref2.html).

Esimerkki UDF käyttäminen Possu Katso seuraavat asiakirjat:

* [Käytä DataFu HDInsight Possu kanssa](hdinsight-hadoop-use-pig-datafu-udf.md) - DataFu on hyötyä UDF Apache ylläpitämä kokoelma

* [Python käyttäminen Possu ja HDInsight-rakenne](hdinsight-python.md)

* [Käyttää C# rakenne ja HDInsight Possu](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

##<a id="data"></a>Esimerkki tiedoista

Tässä esimerkissä käytetään *log4j* mallitiedosto, jotka on tallennettu **/example/data/sample.log** -blob-tallennustilan säilö. Kunkin lokin tiedoston sisälle koostuu rivi-kentät, joka sisältää `[LOG LEVEL]` -kenttä näyttää tyyppi ja vakavuus, esimerkiksi:

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

Edellisen esimerkin lokin taso on virhe.

> [AZURE.NOTE] Voit myös luoda log4j tiedoston [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) kirjaaminen-työkalun avulla ja oman blob Lataa tiedosto. Saat ohjeet [HDInsight lataa tiedot](hdinsight-upload-data.md) . Lisätietoja siitä, miten BLOB Azuren tallennustilaan käytetään HDInsight-kohdassa [Käytä Azure-Blob-säiliö HDInsight kanssa](hdinsight-hadoop-use-blob-storage.md).

Esimerkkitiedot tallennetaan Azure-Blob-säiliö, joka HDInsight käyttää käyttöjärjestelmän Hadoop klustereiden. HDInsight käyttää BLOB **wasb** etuliitettä käyttämällä tallennettuja tiedostoja. Voit käyttää sample.log-tiedosto, käytä seuraavaa syntaksia:

    wasbs:///example/data/sample.log

Koska WASB tallennetaan oletusarvoisesti HDInsight, voit käyttää tiedoston **/example/data/sample.log** Possu latinalainen-avulla.

> [AZURE.NOTE] Syntaksi, **wasbs: / / /**, käytetään oletusarvon tallennustilan säiliön HDInsight-klusterin tallennettuja tiedostoja. Jos olet määrittänyt lisätallennustilaa tilit, kun yhteyttä klusterin valmisteltu ja haluat käyttää näissä tileissä tallennettuja tiedostoja, voit käyttää tietoja määrittämällä säilö nimi ja tallennustilaa tilin osoite, esimerkiksi: **wasbs://mycontainer@mystorage.blob.core.windows.net/example/data/sample.log**.


##<a id="job"></a>Tietoja työn Esimerkki

Seuraava Possu latinalainen työ Lataa **sample.log** -tiedoston HDInsight-klusterin oletusarvo-tallennustilan. Valitse suorittaa muunnoksia, joka johtaa määrä, kuinka monta kertaa kunkin tason tapahtui KESKIPOIKKEAMA sarjaa. Tulokset ovat polkumyynnillä STDOUT kyselyjä.

    LOGS = LOAD 'wasbs:///example/data/sample.log';
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
    RESULT = order FREQUENCIES by COUNT desc;
    DUMP RESULT;

Seuraava kuva esittää hajautuksen jokaisen muunnoksen mitä tietoihin.

![Muunnokset graafinen esitys][image-hdi-pig-data-transformation]

##<a id="run"></a>Possu latinalainen suoritetaan

HDInsight voit suorittaa Possu latinalainen työt usealla eri tavalla. Seuraavan taulukon avulla voit päättää, mitä menetelmä sopii sinulle ja noudata vaiheittainen linkkiä.

| **Käytä tätä** , jos haluat...                                   | .. .an **Vuorovaikutteinen** käyttöliittymä | ... **Eräkäsittely** | .. .korvaava **klusterin käyttöjärjestelmä** | .. .from **Asiakkaan käyttöjärjestelmä** |
|:--------------------------------------------------------------|:---------------------------:|:-----------------------:|:------------------------------------------|:-----------------------------------------|
| [SSH](hdinsight-hadoop-use-pig-ssh.md)                        |              ✔              |            ✔            | Linux                                     | Linux, Unix, Mac OS x: ssä tai Windows        |
| [Kääntö](hdinsight-hadoop-use-pig-curl.md)                      |           &nbsp;            |            ✔            | Linux- tai Windows                          | Linux, Unix, Mac OS x: ssä tai Windows        |
| [.NET SDK Hadoop](hdinsight-hadoop-use-pig-dotnet-sdk.md) |           &nbsp;            |            ✔            | Linux- tai Windows                          | Windows (toistaiseksi)                        |
| [Windows PowerShellin](hdinsight-hadoop-use-pig-powershell.md)  |           &nbsp;            |            ✔            | Linux- tai Windows                          | Windows                                  |
| [Etätyöpöytä](hdinsight-hadoop-use-pig-remote-desktop.md)  |              ✔              |            ✔            | Windows                                   | Windows                                  |


## <a name="running-pig-jobs-on-azure-hdinsight-using-on-premises-sql-server-integration-services"></a>Käynnissä Possu työt Azure Hdinsightiin paikallisen SQL Server-Integrointipalveluilla käyttäminen

Voit myös Possu suoritetaan SQL Server Integration Services (SSIS). Azure Feature Packin SSIS sisältää seuraavat osat, jotka toimivat Possu työt-Hdinsightista.


- [Azure Hdinsightiin Possu tehtävä][pigtask]
- [Azure tilauksen Yhteyksienhallinnan][connectionmanager]


Lue lisää Azure Feature Packin SSIS [tähän][ssispack].


##<a id="nextsteps"></a>Seuraavat vaiheet

Nyt oppinut voit Possu käyttäminen HDInsight-seuraavat linkkien avulla voit tutkia muita esimerkkejä Azure Hdinsightiin.

* [Tietojen lataaminen Hdinsightiin][hdinsight-upload-data]
* [Rakenteen käyttäminen Hdinsightiin][hdinsight-use-hive]
* [Sqoop käyttäminen Hdinsightiin](hdinsight-use-sqoop.md)
* [Oozie käyttäminen Hdinsightiin](hdinsight-use-oozie.md)
* [MapReduce työt käyttäminen Hdinsightiin][hdinsight-use-mapreduce]

[check]: ./media/hdinsight-use-pig/hdi.checkmark.png

[apachepig-home]: http://pig.apache.org/
[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html
[curl]: http://curl.haxx.se/
[pigtask]: http://msdn.microsoft.com/library/mt146781(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx

[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: ../hdinsight-get-started.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md#mapreduce-sdk

[Powershell-install-configure]: ../powershell-install-configure.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx

[image-hdi-log4j-sample]: ./media/hdinsight-use-pig/HDI.wholesamplefile.png
[image-hdi-pig-data-transformation]: ./media/hdinsight-use-pig/HDI.DataTransformation.gif
[image-hdi-pig-powershell]: ./media/hdinsight-use-pig/hdi.pig.powershell.png
[image-hdi-pig-architecture]: ./media/hdinsight-use-pig/HDI.Pig.Architecture.png
