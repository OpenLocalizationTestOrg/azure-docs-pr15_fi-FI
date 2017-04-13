<properties
 pageTitle="Esimerkki Apache myrsky topologioissa HDInsight | Microsoft Azure"
 description="Esimerkki myrsky topologioissa luettelo luodaan ja testattu Apache myrsky-HDInsight, mukaan lukien basic C#- ja Java topologioissa ja tapahtuman keskittimet käsitteleminen."
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

# <a name="example-storm-toplogies-and-components-for-apache-storm-on-hdinsight"></a>Esimerkki myrsky toplogies ja Apache myrsky HDInsight-osia

Seuraavassa on esimerkkejä luodaan ja ylläpidetään Microsoft käytettäväksi Apache myrsky HDInsight-luettelo. Näissä esimerkeissä kansilehden eri aiheista luomasta basic C#- ja Java topologioissa käsittelyn Azure palvelut, kuten tapahtuman keskittimet, DocumentDB, Power BI, SQL-tietokantaan, HBase HDInsight ja Azure-tallennustilan. Esimerkkejä myös osoittaa-Azure tai jopa muiden kuin Microsoft tekniikoita, kuten SignalR ja Socket.IO käsittelemisestä

| Kuvaus                                                                                             | Esittelee                                         | Kielen/Framework         |
|:--------------------------------------------------------------------------------------------------------|:-----------------------------------------------------|:---------------------------|
| [Kirjoittaa Apache myrsky Azure järvi tietosäilö](hdinsight-storm-write-data-lake-store.md) | Azure järvi tietovaraston kirjoittaminen | Java |
| [Tapahtuma-toiminnossa Spout ja lukko lähde](https://github.com/apache/storm/tree/master/external/storm-eventhubs) | Tapahtuman keskittimeen nokkaan ja lukko | Java |
| [Kehitä Java-pohjainen topologioissa Apache myrsky HDInsight-][5797064f]                                 | Maven-testi                                                | Java                       |
| [Kehitä C# topologioissa Apache myrsky käyttöön Visual Studiossa Hdinsightiin][16fce2d1]                     | HDInsight Tools for Visual Studio                    | C#-Java                   |
| [Luo useita tietojen virtaa C# myrsky topologian][ec5a4064]                                         | Useita virtaa                                     | C#                         |
| [Määrittää myrsky HDInsight-Twitter tykkäysten aiheita][3c86c7c8]                                   | Trident                                              | Java-Trident              |
| [Tapahtuman Azure HDInsight (C#)-myrsky lihavoituna tapahtumia prosessi][844d1d81]                                | Tapahtuman keskittimet                                           | C#- ja Java                |
| [Prosessin tapahtumia Azure tapahtuman lihavoituna myrsky-HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md) | Tapahtuman keskittimet | Java |
| [Myrsky topologian tietojen visualisointi Power Bi avulla][94d15238]                              | Power BI                                             | C#                         |
| [Myrsky ja HBase HDInsight tunnistimen tietojen analysoiminen][ab894747]                                     | Tapahtuman keskittimet, HBase, Socket.IO Web-koontinäyttö          | C#-Java-JavaScript-HTML |
| [Käsittele ajoneuvon tunnistimen tietojen tapahtuman keskittimet myrsky käyttäminen Hdinsightiin][246ee964]                        | Tapahtuman keskittimet, DocumentDb, tallennustilan Azure Blob (WASB)    | C#-Java                   |
| [Pura, muunna ja kuormituksen (ETL)-Azure tapahtuman keskittimet, HBase, myrsky käyttäminen Hdinsightiin][b4b68194] | Tapahtuman keskittimet HBase                                    | C#                         |
| [Mallin C# myrsky topologian projektin käsittelyyn Azure HDInsight-myrsky palvelut][ce0c02a2]  | Tapahtuman keskittimet, DocumentDb, SQL tietokanta, HBase, SignalR | C#-Java                   |
| [Skaalattavuus viitearvoja Azure tapahtuman keskittimet myrsky käyttäminen HDInsight-lukemista varten][d6c540e3]           | Viestin nopeus-tapahtuman keskittimet SQL-tietokantaan         | C#-Java                   |
| [Tapahtumien myrsky ja HBase käyttäminen HDInsight yhdistäminen](hdinsight-storm-correlation-topology.md) | HBase | C# |
| [Myrsky HDInsight-Python käyttäminen](hdinsight-storm-develop-python-topology.md) | Java- ja Clojure Python osia perusteella myrsky topologioissa | Python |

## <a name="next-steps"></a>Seuraavat vaiheet

* [Apache myrsky HDInsight-käytön aloittaminen][2b8c3488]

* [Opettele käyttöön ja hallita myrsky topologioissa myrsky HDInsight-kanssa][6eb0d3b8]

  [2b8c3488]: hdinsight-apache-storm-tutorial-get-started-linux.md "Opettele luominen HDInsight-klusterissa myrsky ja käyttöön Esimerkki topologioissa myrsky Raporttinäkymät-ikkunan avulla."
  [6eb0d3b8]: hdinsight-storm-deploy-monitor-topology.md "Lue, miten voit ottaa käyttöön ja hallita topologioissa verkkopohjaisia myrsky Raporttinäkymät-ikkunan ja myrsky Käyttöliittymän tai HDInsight-työkalujen avulla Visual Studio."
  [16fce2d1]: hdinsight-storm-develop-csharp-visual-studio-topology.md "Opettele luomaan C# myrsky topologioissa Visual Studio HDInsight-painikkeilla."
  [5797064f]: hdinsight-storm-develop-java-topology.md "Opi luomaan myrsky topologioissa Java-käyttämällä maven-testi, luomalla basic wordcount topologian."
  [94d15238]: hdinsight-storm-power-bi-topology.md "Käsittelee kirjoittaa tietoja Power BI C#-topologian ja valitse sitten Luo ja luominen Raporttinäkymät-ikkunan tiedoista."
  [ec5a4064]: https://github.com/Blackmist/csharp-storm-example "Esittelee basic myrsky topologian, joka suorittaa wordcount, toteutettu C#. Tämä myös esitellään, miten voit luoda useita tietojen virtaa C#-topologian kuluessa."
  [844d1d81]: hdinsight-storm-develop-csharp-event-hub-topology.md "Opettele lukea ja tietojen kirjoittaessaan HDInsight Azure tapahtuman myrsky lihavoituna."
  [ab894747]: hdinsight-storm-sensor-data-analysis.md "Opettele käyttämään HDInsight Apache myrsky käsitellä tunnistimen tietojen Azure tapahtuman keskittimet, visualisointi käyttämällä D3.js ja tallenna se (valinnainen), HBase."
  [3c86c7c8]: hdinsight-storm-twitter-trending.md "Opettele Trident avulla voit luoda myrsky topologian, joka määrittää tykkäysten aiheet (hashtags, joka perustuu) Twitter."
  [246ee964]: hdinsight-storm-iot-eventhub-documentdb.md "Opettele käyttämään myrsky topologian viestien lukeminen Azure tapahtuman keskittimet, asiakirjojen lukeminen Azure DocumentDB varten tietoihin viittaavien ja tallentaa tiedot Azure-tallennustilan."
  [d6c540e3]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/EventCountExample "Useita topologioissa osoittamaan siirtonopeuden, kun luettaessa Azure tapahtuman keskittimet ja tallentamista Apache myrsky käyttäminen HDInsight SQL-tietokantaan."
  [b4b68194]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample "Lue, miten voit lukea tietoja Azure tapahtuman keskittimet, kooste & transform tiedot ja tallentaa sen HBase HDInsight."
  [ce0c02a2]: https://github.com/hdinsight/hdinsight-storm-examples/tree/master/templates/HDInsightStormExamples "Tämä projekti sisältää malleja spouts, Pultit ja topologioissa vuorovaikutuksessa eri Azure palvelujen, kuten tapahtuman keskittimet, DocumentDB ja SQL-tietokantaan."
 
