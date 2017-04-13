<properties
    pageTitle="Mikä on HBase HDInsight? | Microsoft Azure"
    description="Johdanto Apache HBase HDInsight-Hadoop NoSQL tietokannan luominen. Käytä tapauksissa tietoja ja vertaa HBase Hadoop muiden klustereihin."
    keywords="bigtable, nosql, mikä on hbase"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/14/2016"
    ms.author="jgao"/>



# <a name="what-is-hbase-in-hdinsight-a-nosql-database-that-provides-bigtable-like-capabilities-for-hadoop"></a>Mikä on HBase HDInsight: A NoSQL tietokannan, joka sisältää BigTable kaltaisten ominaisuuksien Hadoop

Apache HBase on avoimen lähdekoodin NoSQL tietokannan, joka perustuu Hadoop ja mallintaa Google BigTable jälkeen. HBase on RAM ja vahva yhdenmukaisuuden paljon rakenteeton ja semistructured sarakkeen perheille järjestetty schemaless tietokannan tietoja.

Tiedot tallennetaan taulukon rivit ja tiedot rivillä on ryhmitelty sarake perhe. HBase on schemaless tietokanta mielessä, että sarakkeiden eikä tallennettujen tietojen tyypin on määritettävä, ennen kuin voit käyttää niitä. Avaa lähdekoodin Skaalaa lineaarisesti käsittelemään petabytes tietojen näyttämisen solmujen tuhansia. Voit luottaa tietojen kopion, eräkäsittely ja muita ominaisuuksia, jotka toimittaa välimuisti Hadoop ekosysteemissä.

## <a name="how-is-hbase-implemented-in-azure-hdinsight"></a>HBase toteuttamistapaa: Azure Hdinsightiin

HDInsight HBase tarjota hallitun klusteriin, joka on integroitu Azure-ympäristöön. Varausyksiköt määritetään ja tallentaa tiedot suoraan Azure-Blob-säiliö, joka sisältää pienen viive ja parannettu joustavuus suorituskyvyn ja kustannusten vaihtoehdoissa. Näin asiakkaat voivat luoda vuorovaikutteisia sivustoja, jotka toimivat suuria tietojoukkoja luonnissa services, joka tallentaa päätepisteestä miljoonia tietojen sensor ja telemetriatietojen ja analysoida Hadoop työt. HBase ja Hadoop ovat käteviä, alkaen big datasta projektin pisteet Azure; erityisesti he voivat ottaa käyttöön reaaliaikaisten sovellusten on paljon käyttöä varten.

HDInsight-käyttöönoton hyödyntää HBase antamaan automaattinen sharding taulukoiden vahva yhdenmukaisuuden lukee kirjoituksia ja automaattinen automaattisesti arkkitehtuuri asteikko-kohtaa. Suorituskykyä on parannettu tallentamalla ladatun välimuistiin lukuja ja siirtonopeuden streaming kirjoituksia, suuri. VPN valmistelu on myös HDInsight HBase käytettävissä. Lisätietoja on artikkelissa [Azure Virtual verkossa klustereiden säännöstä HDInsight] [hbase-provision-vnet].

## <a name="how-is-data-managed-in-hdinsight-hbase"></a>Miten tietoja hallitaan HDInsight HBase?

Tietoja voidaan hallita HBase avulla `create`, `get`, `put`, ja `scan` HBase-liittymän komennoista. Tietoja kirjoitetaan tietokantaan käyttämällä `put` ja lukea käyttämällä `get`. `scan` Komentoa käytetään tietojen saamiseksi taulukon rivejä. Tietoja voidaan hallita myös käyttämällä HBase C# API, joka sisältää asiakkaan kirjaston HBase REST-Ohjelmointirajapinnalla päälle. HBase-tietokannan myös voidaan suorittaa kysely käyttämällä rakenne. Katso esittely näitä ohjelmoinnin malleja, [käyttäminen HBase kanssa Hadoop HDInsight][hbase-get-started]. Muiden suorittimien ovat käytettävissä myös, joka Salli tietojen käsittely solmut isännöivien tietokannan.


## <a name="scenarios-use-cases-for-hbase"></a>Skenaariot: Käyttötapauksiin HBase
Mitä BigTable (ja tunnisteesta HBase) canonical käyttötapaus on luotu on web-haku. Hakukoneet muodostaa indeksejä, jotka vastaavat ehtoja verkkosivuista, jotka sisältävät. Mutta on monia muita Käytä tapauksia, joka HBase soveltuu, joista osa on eritelty sisältö.

- Avain-arvo-säilö

    HBase voidaan käyttää avain-arvo-kaupan ja se sopii viestin järjestelmien hallinta. Facebook käyttää HBase niiden sähköpostijärjestelmän ja se sopii tallentamisesta ja Internet-viestinnän hallinta. WebTable käyttää HBase voi etsiä ja hallitse taulukoita, jotka ovat poimittujen verkkosivuille.

- Tunnistimen tiedot

    HBase on hyötyä tallentaa tiedot, jotka kerätään vaiheittainen eri lähteistä. Tämä sisältää sosiaalisen analytics-aikasarjalle vuorovaikutteiset raporttinäkymät pitäminen ajan tasalla kehityksen ja laskureita ja valvonta log järjestelmien hallinta. Esimerkkejä tällaisista tiedostoista ovat Bloomberg toimija pääte ja avaa aika sarjan tietokannan (OpenTSDB), joka tallentaa ja kerääminen palvelimen järjestelmien kuntoa arvot käyttö.

- Reaaliaikainen kysely

    [Pori](http://phoenix.apache.org/) on SQL-kyselyn engine Apache HBase varten. Se on käyttää JDBC ohjaimen ja kyselyt ja HBase taulukoiden hallinnan avulla SQL ottaa käyttöön.

- HBase ympäristö nimellä

    Sovellusten suorittamisen käyttämällä sitä datastore HBase päälle. Esimerkkejä tällaisista tiedostoista ovat Pori, OpenTSDB tai Kiji Titan. Sovellusten voidaan myös yhdistää HBase. Esimerkkejä tällaisista tiedostoista ovat rakenne, Possu, Solr, myrsky, Flume, Impala, ohjattu, hermosolmu ja Poraudu.


##<a name="next-steps"></a>Seuraavat vaiheet

- [Aloita käyttäminen Hadoop HDInsight HBase][hbase-get-started]
- [Valmistele HDInsight klustereiden Azure Virtual verkossa] [hbase-provision-vnet]
- [Määritä HBase replikoinnin Hdinsightiin](hdinsight-hbase-geo-replication.md)
- [Analysoi Twitter markkinatunnelma HBase HDInsight kanssa][hbase-twitter-sentiment]
- [Käytä maven-testi HBase käyttäminen HDInsight (Hadoop) Java-sovellusten rakentamiseen][hbase-build-java-maven]

##<a name="see-also"></a>Katso myös

- [Apache HBase](https://hbase.apache.org/)
- [Bigtable: Jäsenneltyjen tietojen hajautettu tallennustilan järjestelmä](http://research.google.com/archive/bigtable.html)




[hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md

[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hbase-build-java-maven]: hdinsight-hbase-build-java-maven.md

[hdinsight-use-hive]: hdinsight-use-hive.md

[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md

[hbase-get-started]: http://azure.microsoft.com/documentation/articles/hdinsight-hbase-get-started/

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: ../storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
