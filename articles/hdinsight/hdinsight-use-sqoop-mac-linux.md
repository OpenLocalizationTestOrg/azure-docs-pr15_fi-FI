<properties
    pageTitle="Hadoop Sqoop käyttäminen Linux-pohjaiset HDInsight | Microsoft Azure"
    description="Opettele suorittaminen Sqoop tuonti ja vienti Linux-pohjaiset Hadoop HDInsight-klusterissa ja Azure SQL-tietokantaan."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="larryfr"/>

#<a name="use-sqoop-with-hadoop-in-hdinsight-ssh"></a>Sqoop käyttäminen Hadoop-HDInsight (SSH)

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Opettele käyttämään Sqoop tuomiseen ja viemiseen Linux-pohjaiset HDInsight-klusterin ja Azure SQL-tietokanta tai SQL Server-tietokantaan.

> [AZURE.NOTE] Tässä artikkelissa kuvattuja Linux-pohjaiset HDInsight-klusterin muodostaa SSH avulla. Windows-asiakkaiden käyttää myös ja PowerShellin Azure Hdinsightiin .NET SDK toimimaan Linux-pohjaiset klustereiden Sqoop. Nämä artikkelit avaaminen sarkainvalitsinta avulla.

##<a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat Tässä opetusohjelmassa, sinun on oltava seuraavasti:

- **A Hadoop-klusterin HDInsight-** ja __Azure SQL-tietokanta__: Tässä asiakirjassa ohjeita perustuvat klusterin ja tietokanta on luotu [Luo klusterin ja SQL-tietokanta](hdinsight-use-sqoop.md#create-cluster-and-sql-database) -asiakirja. Jos sinulla on jo HDInsight-klusterin ja SQL-tietokantaan, voit korvata koskevat tässä asiakirjassa käytettävät arvot.
- **Työasema**: tietokoneen SSH avulla.

##<a name="install-freetds"></a>Asenna FreeTDS

1. SSH avulla voit muodostaa yhteyden Linux-pohjaiset HDInsight-klusterin. Käytettävä osoite muodostettaessa on `CLUSTERNAME-ssh.azurehdinsight.net` ja portti on `22`.

    Lisätietoja SSH avulla voit muodostaa yhteyden HDInsight-kohdassa seuraavat asiakirjat:

    * **Linux, Unix tai OS X-asiakkaat**: artikkelissa [etäyhteyden muodostaminen Linux-pohjaiset HDInsight-klusterin Linux, OS X tai Unix](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster)

    * **Windows-asiakkaita**: artikkelissa [etäyhteyden muodostaminen Linux-pohjaiset HDInsight-klusterin Windowsista](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster)

3. Käytä asentamiseksi FreeTDS seuraava komento:

        sudo apt-get --assume-yes install freetds-dev freetds-bin

    FreeTDS käytetään useita vaiheita muodostaa yhteyden SQL-tietokantaan.

##<a name="create-the-table-in-sql-database"></a>Luo taulukon SQL-tietokantaan

> [AZURE.IMPORTANT] Jos käytössäsi on HDInsight-klusterin ja SQL-tietokanta on luotu vaiheet ja [Luo klusterin SQL-tietokantaan](hdinsight-use-sqoop.md), Ohita tämän osan vaiheet, kuten asiakirjan ohjeita osana on luotu tietokanta ja taulukko.

1. SSH yhteyden muodostaminen Hdinsightiin Käytä seuraavaa komentoa muodostaa yhteyden SQL-tietokanta ja Kreetassa taulukon, jota käytetään loput seuraavasti:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Näyttöön tulee tulosteen seuraavankaltaiselta:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to sqooptest
        1>

5. Milloin `1>` Kysy, kirjoita seuraavat rivit:

        CREATE TABLE [dbo].[mobiledata](
        [clientid] [nvarchar](50),
        [querytime] [nvarchar](50),
        [market] [nvarchar](50),
        [deviceplatform] [nvarchar](50),
        [devicemake] [nvarchar](50),
        [devicemodel] [nvarchar](50),
        [state] [nvarchar](50),
        [country] [nvarchar](50),
        [querydwelltime] [float],
        [sessionid] [bigint],
        [sessionpagevieworder] [bigint])
        GO
        CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)
        GO

    Kun `GO` lauseen yksikkö-edellisen lauseet lasketaan. Ensin **mobiledata** taulukko luodaan ja valitse liitetty indeksi on lisätty siihen (vaatii SQL-tietokantaan.)

    Voit varmistaa, että taulukossa on luotu käyttämällä seuraavaa:

        SELECT * FROM information_schema.tables
        GO

    Näyttöön tulee tulosteen seuraavankaltaiselta:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        sqooptest       dbo     mobiledata      BASE TABLE

8. Kirjoita `exit` osoitteessa `1>` kehote Lopeta tsql-apuohjelma.

##<a name="sqoop-export"></a>Sqoop vienti

3. SSH yhteyden muodostaminen Hdinsightiin, Tu, varmista, että Sqoop voi nähdä SQL-tietokantaan seuraava komento:

        sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>

    Tämä palautetaan tietokannoista, mukaan lukien **sqooptest** tietokannan aiemmin luomasi luettelo.

4. Käytä seuraavaa komentoa tietojen vieminen **mobiledata** taulukon **hivesampletable** :

        sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --export-dir 'wasbs:///hive/warehouse/hivesampletable' --fields-terminated-by '\t' -m 1

    Tämän tiedon Sqoop **sqooptest** -tietokantayhteyden muodostamisessa SQL-tietokantaan ja viedä tietoja **wasbs: / / / rakenne ja varasto/hivesampletable** (fyysinen tiedostot *hivesampletable*) **mobiledata** -taulukkoon.

5. Komennon jälkeen muodostaa yhteyden tietokantaan käyttämällä TSQL käyttämällä seuraavaa:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Kun yhteys on muodostettu, varmista, että tiedot on viety **mobiledata** taulukkoon seuraavista väittämistä avulla:

        SELECT * FROM mobiledata
        GO

    Raportissa pitäisi näkyä luettelo-taulukon tiedot. Kirjoita `exit` Lopeta tsql-apuohjelma.

##<a name="sqoop-import"></a>Sqoop tuominen

1. Seuraavat avulla voit tuoda tietoja **mobiledata** taulukosta SQL-tietokantaan **wasbs: / / / opetusohjelmat/usesqoop/importeddata** HDInsight-kansio:

        sqoop import --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasbs:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1

    Tuotujen tietojen on kenttiä, jotka on erotettu toisistaan sarkainmerkin ja rivit se lopetetaan uusi rivi-merkin.

2. Kun tuonti on valmis, käytä seuraavaa komentoa luetteloon tiedot, uusi kansio:

        hadoop fs -text wasbs:///tutorials/usesqoop/importeddata/part-m-00000

##<a name="using-sql-server"></a>SQL Serverin avulla

Voit käyttää myös Sqoop ja viedä tietoja SQL Server data Centerissä tai ylläpidettävä Azure Virtual koneeseen. Erot käytettäessä SQL-tietokantaan ja SQL Server on:

* HDInsight ja SQL Server on oltava samassa Azure Virtual verkossa

    > [AZURE.NOTE] HDInsight tukee vain sijainti-virtual verkkojen ja sitä ei ole tällä hetkellä toimi affiniteetti ryhmän virtual verkkojen.

    Kun käytät SQL Server-että palvelinkeskukseen, sinun on määritettävä virtual verkon *sivusto sivusto* tai *pisteen sivuston*.

    > [AZURE.NOTE] **Pisteen sivuston** virtual verkoissa SQL Server on oltava käynnissä VPN-asiakasohjelman configuration-sovellus, joka on käytettävissä Azure virtual verkon asetusten **raporttinäkymät-ikkunan** .

    Katso lisätietoja Azure virtuaaliverkko- [Virtual yleistä](../virtual-network/virtual-networks-overview.md).

* SQL Server on määritetty sallimaan SQL-todennusta. Lisätietoja on artikkelissa [Valitse Office todennustila](https://msdn.microsoft.com/ms144284.aspx)

* Saatat joutua määrittämään SQL Serverin etäyhteyksien. Lisätietoja [vianmääritys yhteyden muodostaminen SQL Server-tietokantamoduuli](http://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx)

* Sinun on luotava **sqooptest** tietokannan SQL Server-apuohjelmalla, kuten **SQL Server Management Studiossa** tai **tsql** - käytettäessä Azure-CLI toimii vain Azure SQL-tietokantaan

    **Mobiledata** -taulukon luominen TSQL-lauseet ovat käytetyt SQL-tietokantaan, lukuun ottamatta luomisesta clusterd index - tätä ei tarvita SQL Serveriä varten:

        CREATE TABLE [dbo].[mobiledata](
        [clientid] [nvarchar](50),
        [querytime] [nvarchar](50),
        [market] [nvarchar](50),
        [deviceplatform] [nvarchar](50),
        [devicemake] [nvarchar](50),
        [devicemodel] [nvarchar](50),
        [state] [nvarchar](50),
        [country] [nvarchar](50),
        [querydwelltime] [float],
        [sessionid] [bigint],
        [sessionpagevieworder] [bigint])

* Kun muodostaa yhteyden SQL Server HDInsight, joudut ehkä käyttää SQL Serverin IP-osoitetta, paitsi jos olet määrittänyt DNS Domain Name System () nimien Azure Virtual verkossa. Esimerkki:

        sqoop import --connect 'jdbc:sqlserver://10.0.1.1:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasbs:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1

##<a name="limitations"></a>Rajoitukset

* Vie - ja Linux-pohjaiset HDInsight, Microsoft SQL Server tai Azure SQL-tietokannan tietojen vieminen Sqoop liittimen joukkona ei tue joukkona Lisää tällä hetkellä.

* Jonottaminen - Linux-pohjaiset HDInsight kanssa käytettäessä `-batch` vaihtaminen Lisää suoritettaessa, Sqoop suorittaa useita Lisää sijaan jonottaminen Lisää toimintoja.

##<a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet oppinut käyttämään Sqoop. Lisätietoja on artikkeleissa:

- [Oozie käyttäminen HDInsight][hdinsight-use-oozie]: Oozie-työnkulun käyttäminen Sqoop-toiminto.
- [Analysoi viive lentotietoihin käyttämällä HDInsight][hdinsight-analyze-flight-data]: Käytä rakenne analysointiin lento viivyttää tiedot ja vie Azure SQL-tietokantaan Sqoop avulla.
- [Tietojen lataaminen HDInsight][hdinsight-upload-data]: Etsi tietojen lataaminen HDInsight/Azure-Blob-säiliö muilla tavoilla.



[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database-create-configure.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
