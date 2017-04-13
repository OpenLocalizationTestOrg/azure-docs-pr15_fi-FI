<properties
pageTitle="HDInsight porteista | Azure"
description="Hadoop palveluita HDInsight porteista luettelo."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="10/03/2016"
ms.author="larryfr"/>

# <a name="ports-and-uris-used-by-hdinsight"></a>Portit ja URI käyttämä Hdinsightiin

Tässä tiedostossa on Hadoop palveluita Linux-pohjaiset HDInsight klustereiden porteista luettelo. Se antaa tietoja myös käyttämällä SSH klusterin muodostamisessa käytettävä portit.

## <a name="public-ports-vs-non-public-ports"></a>Julkinen portit ja julkinen portit

Linux-pohjaiset HDInsight klustereiden paljastaa vain kolme portit julkisesti Internetissä. 22, 23 ja 443. Näiden avulla voit käyttää turvallisesti klusterin SSH ja Servicesin avulla tarjoamia suojatun HTTPS-protokollan kautta.

Sisäisesti Hdinsightista on toteutettu mukaan useita Azuren näennäiskoneiden (klusterin, sijaitsevien solmujen) Virtual Azure-verkossa. Valitse virtual verkoston voit käyttää portit eikä julkaista Internetissä. Esimerkiksi jos muodostat yhteyden jokin käyttämällä SSH pään solmut, pään solmun voit sitten suoraan käyttää palveluita klusterin solmuissa.

> [AZURE.IMPORTANT] Kun luot HDInsight-klusterin, jos et määritä Virtual Azure-verkon määritys-asetus, muistikirja luodaan; Et voi liittyä muiden laitteiden (esimerkiksi muiden Azuren näennäiskoneiden tai asiakas-kehitys machine) tätä VPN ohjelma luo automaattisesti. 

Liittäminen virtual verkon muihin tietokoneisiin, luoda virtuaalisen verkon ensin ja määritä se HDInsight-klusterin luotaessa. Lisätietoja [laajentaa HDInsight ominaisuuksia Virtual Azure-verkon avulla](hdinsight-extend-hadoop-virtual-network.md)

## <a name="public-ports"></a>Julkinen portit

Kaikki HDInsight-klusterin solmut sijaitsevat Azure Virtual verkossa ja ei voi käyttää suoraan Internetistä. Julkinen yhdyskäytävän sisältää seuraavat portit, jotka ovat yleisiä kaikki HDInsight-klusterin välillä internet-yhteyttä.

| Palvelun | Port (portti) | Protokolla | Kuvaus |
| ---- | ---------- | -------- | ----------- | ----------- |
| sshd | 22 | SSH | Valitse ensisijainen headnode sshd asiakkaiden muodostaa yhteyden. Tutustu [SSH Linux-pohjaiset HDInsight kanssa](hdinsight-hadoop-linux-use-ssh-windows.md) |
| sshd | 22 | SSH | Yhdistää asiakkaiden sshd reuna-solmun (vain HDInsight Premium). Katso [HDInsight R-palvelimen käyttäminen](hdinsight-hadoop-r-server-get-started.md) |
| sshd | 23 | SSH | Valitse Toissijainen headnode sshd asiakkaiden muodostaa yhteyden. Tutustu [SSH Linux-pohjaiset HDInsight kanssa](hdinsight-hadoop-linux-use-ssh-windows.md) |
| Ambari | 443 | HTTPS | Ambari web-Käyttöliittymä. Katso [hallinta HDInsight Ambari Web-Käyttöliittymä](hdinsight-hadoop-manage-ambari.md) |
| Ambari | 443 | HTTPS | Ambari REST-Ohjelmointirajapinnalla. Katso [hallinta HDInsight Ambari REST-Ohjelmointirajapinnalla](hdinsight-hadoop-manage-ambari-rest-api.md) |
| WebHCat | 443 | HTTPS | HCatalog REST-Ohjelmointirajapinnalla. Katso [käyttää rakenteen kääntö kanssa](hdinsight-hadoop-use-pig-curl.md), [Käytä kääntö Possu](hdinsight-hadoop-use-pig-curl.md), [MapReduce kääntö kanssa](hdinsight-hadoop-use-mapreduce-curl.md) |
| HiveServer2 | 443 | ODBC | Muodostaa yhteyden käyttämällä ODBC rakenne. Katso [Connect Excel to Microsoft ODBC-ohjaimella HDInsight Excelin](hdinsight-connect-excel-hive-odbc-driver.md). |
| HiveServer2 | 443 | JDBC | Muodostaa yhteyden käyttämällä JDBC rakenne. Artikkelissa [etäyhteyden muodostaminen rakenne JDBC ohjaimen HDInsight-rakenne](hdinsight-connect-hive-jdbc-driver.md) |

Seuraavat ovat käytettävissä tiettyyn klusteri:

| Palvelun | Port (portti) | Protokolla |Klusterin tyyppi | Kuvaus |
| ------------ | ---- |  ----------- | --- | ----------- |
| Stargate | 443 | HTTPS | HBase | HBase REST-Ohjelmointirajapinnalla. Katso [HBase käytön aloittaminen](hdinsight-hbase-tutorial-get-started-linux.md) |
| Livy | 443 | HTTPS |  Ohjattu | Ohjattu REST-Ohjelmointirajapinnalla. Katso [Lähetä ohjattu työt käyttämällä etäyhteyden Livy](hdinsight-apache-spark-livy-rest-interface.md) |
| Myrsky | 443 | HTTPS | Myrsky | Myrsky web-Käyttöliittymä. Katso [käyttöönotto ja hallita myrsky topologioissa HDInsight-](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="authentication"></a>Todennus

Kaikkien palveluiden julkisesti Internetissä olevat todennus on:

| Port (portti) | Tunnistetiedot |
| ---- | ----------- |
| 22 tai 23 | Klusterin luonnin aikana määritetyt SSH käyttäjän tunnistetiedot |
| 443 | Kirjautumisnimi (oletusarvo: järjestelmänvalvoja) ja salasana, jotka olivat valittuina klusterin luonnin aikana |

## <a name="non-public-ports"></a>Ei-julkinen portit

> [AZURE.NOTE] Jotkin palvelut ovat käytettävissä vain tiettyyn klusteri tyyppejä. Esimerkiksi HBase on käytettävissä vain HBase klusterin tyyppejä.

### <a name="hdfs-ports"></a>HDFS portit

| Palvelun | Solmut | Port (portti) | Protokolla | Kuvaus |
| ------- | ------- | ---- | -------- | ----------- | 
| NameNode web-Käyttöliittymä | Pää-solmut | 30070 | HTTPS | Web-Käyttöliittymä nykyisen tilan tarkasteleminen |
| NameNode metatietojen palvelu | pään solmujen | 8020 | IPC | Tiedoston järjestelmän metatiedot 
| DataNode | Kaikki työntekijä solmut | 30075 | HTTPS | Web Käyttöliittymän Näytä tila, lokit jne. |
| DataNode | Kaikki työntekijä solmut | 30010 | &nbsp; | Tietojen siirtäminen |
| DataNode | Kaikki työntekijä solmut | 30020 | IPC | Metatietojen toiminnot |
| Toissijainen NameNode | Pää-solmut | 50090 | HTTP | Tarkistuspiste NameNode metatiedot |

### <a name="yarn-ports"></a>KUITENKAAN portit

| Palvelun | Solmut | Port (portti) | Protokolla | Kuvaus |
| ------- | ------- | ---- | -------- | ----------- |
| Resurssien hallinnan web-Käyttöliittymä | Pää-solmut | 8088-suorittimen | HTTP | Web-Käyttöliittymä resurssien hallinta |
| Resurssien hallinnan web-Käyttöliittymä | Pää-solmut | 8090 | HTTPS | Web-Käyttöliittymä resurssien hallinta |
| Resurssienhallinta järjestelmänvalvojan käyttöliittymä | pään solmujen | 8141 | IPC | Sovelluksen lähetysten varten (rakenne-rakenne-palvelimeen, Possu, jne.) |
| Resurssienhallinta ajoitus | pään solmujen | 8030 | HTTP | Järjestelmänvalvojan käyttöliittymä |
| Resurssienhallinta sovelluksen käyttöliittymä | pään solmujen | 8050 | HTTP |Sovellusten hallinta-liittymän osoite |
| NodeManager | Kaikki työntekijä solmut | 30050 | &nbsp; | Säilön esimies osoite |
| NodeManager web-Käyttöliittymä | Kaikki työntekijä solmut | 30060 | HTTP | Resurssien hallinnan käyttöliittymä |
| Aikajana-osoite | Pää-solmut | 10200 | RPC | Aikajanan palvelun RPC-palvelun. |
| Aikajanan web-Käyttöliittymä | Pää-solmut | 8181 | HTTP | Aikajanan palvelun web-Käyttöliittymä |

### <a name="hive-ports"></a>Rakenne-portit

| Palvelun | Solmut | Port (portti) | Protokolla | Kuvaus |
| ------- | ------- | ---- | -------- | ----------- |
| HiveServer2 | Pää-solmut | 10001 | Thrift | Palvelun ohjelmallisesti muodostamisesta rakenne (Thrift/JDBC) |
| HiveServer | Pää-solmut | 10000 | Thrift | Palvelun ohjelmallisesti muodostamisesta rakenne (Thrift/JDBC) |
| Metastore rakenne | Pää-solmut | 9083 | Thrift | Palvelun ohjelmallisesti muodostamisesta rakenteen metatiedot (Thrift/JDBC) |

### <a name="webhcat-ports"></a>WebHCat portit

| Palvelun | Solmut | Port (portti) | Protokolla | Kuvaus |
| ------- | ------- | ---- | -------- | ----------- |
| WebHCat server | Pää-solmut | 30111 | HTTP | Verkko-Ohjelmointirajapinnan tasalla HCatalog ja muut Hadoop-palvelut |

### <a name="mapreduce-ports"></a>MapReduce portit

| Palvelun | Solmut | Port (portti) | Protokolla | Kuvaus |
| ------- | ------- | ---- | -------- | ----------- |
| JobHistory | Pää-solmut | 19888 | HTTP | MapReduce JobHistory web-Käyttöliittymä |
| JobHistory | Pää-solmut | 10020 | &nbsp; | MapReduce JobHistory server |
| ShuffleHandler | &nbsp; | 13562 | &nbsp; | Siirtää keskitason kartan tulostaa pyytää pienennyslaitteita varten |

### <a name="oozie"></a>Oozie

| Palvelun | Solmut | Port (portti) | Protokolla | Kuvaus |
| ------- | ------- | ---- | -------- | ----------- |
| Oozie server | Pää-solmut | 11000 | HTTP | Oozie palvelun URL-osoite |
| Oozie server | Pää-solmut | 11001 | HTTP | Portti Oozie järjestelmänvalvoja |

### <a name="ambari-metrics"></a>Ambari arvot

| Palvelun | Solmut | Port (portti) | Protokolla | Kuvaus |
| ------- | ------- | ---- | -------- | ----------- |
| Aikajana (sovelluksen historia) | Pää-solmut | 6188 | HTTP | Aikajanan palvelun web-Käyttöliittymä |
| Aikajana (sovelluksen historia) | Pää-solmut | 30200 | RPC | Aikajanan palvelun web-Käyttöliittymä |

### <a name="hbase-ports"></a>HBase portit

| Palvelun | Solmut | Port (portti) | Protokolla | Kuvaus |
| ------- | ------- | ---- | -------- | ----------- |
| HMaster | Pää-solmut | 16000 | &nbsp; | &nbsp; |
| HMaster tiedot Web-Käyttöliittymä | Pää-solmut | 16010 | HTTP | Portti HBase perustyyli-Web-Käyttöliittymä |
| Alue-palvelin | Kaikki työntekijä solmut | 16020 | &nbsp; | &nbsp; |
| &nbsp; | &nbsp; | 2181 | &nbsp; | Portti, asiakkaiden avulla voit muodostaa yhteyden ZooKeeper |

### <a name="kafka-ports"></a>Kafka portit

| Palvelun | Solmut | Port (portti) | Protokolla | Kuvaus |
| ------- | ------- | ---- | -------- | ----------- |
| Broker  | Työntekijän solmujen | 9092 | [Kafka protokolla](http://kafka.apache.org/protocol.html) | Käytettävän asiakkaan liikenne |
| &nbsp; | Zookeeper solmujen | 2181 | &nbsp; | Portti, asiakkaiden avulla voit muodostaa yhteyden Zookeeper |
