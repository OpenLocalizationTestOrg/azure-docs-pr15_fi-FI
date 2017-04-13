<properties 
   pageTitle="Käytä Apache Pori ja SQuirreL HDInsight | Microsoft Azure" 
   description="Katso, miten Apache Pori käyttäminen HDInsight ja asentaminen ja työaseman muodostaa HBase-klusterin HDInsight-SQuirreL määrittämistä." 
   services="hdinsight" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/02/2016"
   ms.author="jgao"/>

# <a name="use-apache-phoenix-with-linux-based-hbase-clusters-in-hdinsight"></a>Apache Pori käyttäminen Linux-pohjaiset HBase varausyksiköt Hdinsightiin  

Lue [Apache Pori](http://phoenix.apache.org/) käyttämisestä HDInsight ja Opi käyttämään SQLLine. Saat lisätietoja Pori [Pori 15 minuutin tai vähemmän](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html). Pori kielioppi-kohdassa [Pori kieliopin](http://phoenix.apache.org/language/index.html).

>[AZURE.NOTE] Katso HDInsight Pori, versiotiedot [uudet myöntämä HDInsight Hadoop-klusterin versioissa?] [hdinsight-versions].

##<a name="use-sqlline"></a>Käytä SQLLine
[SQLLine](http://sqlline.sourceforge.net/) on SQL suorittamiseen komentoriviltä apuohjelma. 

###<a name="prerequisites"></a>Edellytykset
Ennen kuin voit käyttää SQLLine, sinulla on oltava seuraavasti:

- **A HBase klusterin Hdinsightista**. Lisätietoja säännöstä HBase klusterin on artikkelissa [aloittaminen Apache HBase HDInsight-][hdinsight-hbase-get-started].
- **Yhteyden muodostaminen HBase klusterin remote desktop-protokollan kautta**. Katso ohjeet [HDInsight käyttämällä perinteinen Azure-portaalin hallinta Hadoop varausyksiköt][hdinsight-manage-portal].


Kun muodostat yhteyden HBase-klusterin, sinun on Muodosta yhteys Zookeepers. Kunkin HDInsight-klusterin on 3 Zookeepers. 

**Saat lisätietoja Zookeeper isäntänimi**

1. Avaa Ambari Selaa **https://<ClusterName>. azurehdinsight.net**.
2. Kirjoita login HTTP (klusterin) käyttäjänimi ja salasana.
3. Valitse vasemmasta valikosta **ZooKeeper** . Näet on 3 **ZooKeeper palvelimen** luettelossa.
4. Valitse jokin luettelossa **ZooKeeper palvelimeen** . Etsi **Hostname**yhteenveto-ruudussa. On samanlainen kuin *zk1 jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*.

**Jos haluat käyttää SQLLine**

1. Yhdistä käyttäen SSH klusterin. Katso ohjeet [Käyttäminen SSH kanssa Linux-pohjaiset Hadoop HDInsight Linux, Unix-tai OS X-](hdinsight-hadoop-linux-use-ssh-unix.md) tai [Käytä SSH kanssa Linux-pohjaiset Hadoop-HDInsight Windows](hdinsight-hadoop-linux-use-ssh-windows.md) asiakastietokoneen käyttöjärjestelmän mukaan.

2. Suorita SSH, suorita SQLLine seuraavista komennoista:

        cd /usr/hdp/2.2.9.1-7/phoenix/bin
        ./sqlline.py <ClusterName>:2181:/hbase-unsecure

2. Suorita seuraavat komennot HBase taulukon luominen ja lisää tietoja:

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));
    
        !tables
        
        UPSERT INTO Company VALUES(1, 'Microsoft');
        
        SELECT * FROM Company;
        
        !quit

Lisätietoja on artikkelissa [SQLLine manuaalisen](http://sqlline.sourceforge.net/#manual) ja [Pori kieliopin](http://phoenix.apache.org/language/index.html).


 
##<a name="next-steps"></a>Seuraavat vaiheet
Tässä artikkelissa on opit Apache Pori käyttämisestä Hdinsightista.  Lisätietoja on artikkelissa

- [HDInsight HBase yleiskatsaus][hdinsight-hbase-overview]: HBase on rakennettu Hadoop, joka tarjoaa RAM ja vahva yhdenmukaisuuden rakenteeton ja semistructured suurista tietomääristä Apache, Avaa-lähde-NoSQL tietokanta.
- [Valmistele HBase klustereiden Azure Virtual verkossa][hdinsight-hbase-provision-vnet]: VPN-integroinnin HBase klustereiden voidaan ottaa käyttöön virtual samassa verkossa kuin sovelluksia niin, että sovellukset voivat viestiä HBase suoraan.
- [Määritä HBase replikointi HDInsight](hdinsight-hbase-geo-replication.md): Opettele määrittämään HBase replikoinnin yli kaksi Azure palvelinkeskusten. 
- [Analysoi Twitter markkinatunnelma kanssa HBase HDInsight][hbase-twitter-sentiment]: Opi laskemaan reaaliaikainen [markkinatunnelma analyysi](http://en.wikipedia.org/wiki/Sentiment_analysis) big datasta käyttämällä HBase HDInsight-Hadoop-klusterin.

[azure-portal]: https://portal.azure.com
[vnet-point-to-site-connectivity]: https://msdn.microsoft.com/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETPT

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hdinsight-hbase-phoenix-sqlline]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-phoenix-sqlline.png
[img-certificate]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vpn-certificate.png
[img-vnet-diagram]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vnet-point-to-site.png
[img-squirrel-driver]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-driver.png
[img-squirrel-alias]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-alias.png
[img-squirrel]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel.png
[img-squirrel-sql]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-sql.png


 
