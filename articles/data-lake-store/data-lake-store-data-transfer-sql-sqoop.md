<properties 
   pageTitle="Kopioi järvi tietovaraston ja Azure SQL-tietokantaan käyttämällä Sqoop tietojen | Microsoft Azure"
   description="Kopioi Azure SQL-tietokanta ja järvi tietovaraston tietojen Sqoop avulla" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="copy-data-between-data-lake-store-and-azure-sql-database-using-sqoop"></a>Tietosäilö järvi ja Azure SQL-tietokantaan käyttämällä Sqoop tietojen kopioiminen

Opettele käyttämään Apache Sqoop tuoda ja viedä tietoja Azure SQL-tietokanta ja järvi tietovaraston välillä.
 

## <a name="what-is-sqoop"></a>Mikä on Sqoop?

Big datasta sovellukset ovat luonnollinen valinta käsittelyyn rakenteeton ja puolirakenteisia tietoja, kuten lokit ja tiedostoja. Kuitenkin myös ehkä tarvitse käsitellä rakenteellisia tietoja, jotka on tallennettu relaatiotietokannasta.

[Apache Sqoop](https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html) on suunniteltu tietojen siirtäminen relaatiotietokannat ja big datasta säilöön, kuten järvi tietovaraston välillä. Sen avulla voidaan tietojen tuominen relaatiotietokannasta hallintajärjestelmän (RDBMS) kuten Azure SQL-tietokanta järvi säilöön. Voit muuntaa ja analysoida tietoja käyttämällä big datasta toiminnoista ja vie tiedot sitten takaisin RDBMS. Tässä opetusohjelmassa käytetään Azure SQL-tietokantaan suhteellisen tietokannan, valitse Tuo/Vie.
 

## <a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat tämän artikkelin, sinulla on oltava seuraavasti:

- **Azure-tilaus**. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/).
- **Azure-tilauksen käyttöön** järvi tietovaraston public preview-version. Katso [ohjeet](data-lake-store-get-started-portal.md#signup). 
- **Azure HDInsight-klusterin** järvi tietovaraston tilin käyttöoikeus. Lisätietoja on kohdassa [Create HDInsight-klusterin tietojen järvi kaupan](data-lake-store-hdinsight-hadoop-use-portal.md). Tässä artikkelissa oletetaan, että HDInsight Linux-klusterin järvi tietovaraston Accessissa.
- **Azure SQL-tietokanta**. Ohjeita siitä, miten voit luoda, on artikkelissa [Azure SQL-tietokannan luominen](../sql-database/sql-database-get-started.md)

## <a name="do-you-learn-fast-with-videos"></a>Opit nopeasti ja ladattua?

[Tässä videossa](https://mix.office.com/watch/1butcdjxmu114) Azure tallennustilan BLOB-objektit ja järvi tietovaraston tietojen kopioiminen DistCp avulla.

## <a name="create-sample-tables-in-the-azure-sql-database"></a>Esimerkki taulukoiden luominen Azure SQL-tietokantaan

1. Luo aluksi kahden otoksen taulukon Azure SQL-tietokantaan. Yhteyden muodostaminen Azure SQL-tietokantaan ja suorita sitten seuraavat kyselyt [SQL Server Management Studiossa](../sql-database/sql-database-connect-query-ssms.md) tai Visual Studio avulla.

    **Taulukko1 luominen**

        CREATE TABLE [dbo].[Table1]( 
        [ID] [int] NOT NULL, 
        [FName] [nvarchar](50) NOT NULL, 
        [LName] [nvarchar](50) NOT NULL, 
        CONSTRAINT [PK_Table_4] PRIMARY KEY CLUSTERED 
            ( 
                [ID] ASC 
            ) 
        ) ON [PRIMARY] 
        GO

    **Luo taulukko2**

        CREATE TABLE [dbo].[Table2]( 
        [ID] [int] NOT NULL, 
        [FName] [nvarchar](50) NOT NULL, 
        [LName] [nvarchar](50) NOT NULL, 
        CONSTRAINT [PK_Table_4] PRIMARY KEY CLUSTERED 
            ( 
                [ID] ASC 
            ) 
        ) ON [PRIMARY] 
        GO

2. Lisää **Taulukko1**mallitietoja. Jätä **taulukko2** tyhjäksi. Olemme tuo tiedot **table1** järvi säilöön. Tämän jälkeen on Vie tietoja järvi tietovaraston **taulukko2**. Suorita seuraavat koodikatkelman.

         
        INSERT INTO [dbo].[Table1] VALUES (1,'Neal','Kell'), (2,'Lila','Fulton'), (3, 'Erna','Myers'), (4,'Annette','Simpson'); 
  

## <a name="use-sqoop-from-an-hdinsight-cluster-with-access-to-data-lake-store"></a>Sqoop avulla järvi tietovaraston HDInsight-klusterin Accessissa

HDInsight-klusterin jo käytettävissä Sqoop paketit. Jos olet määrittänyt HDInsight-klusterin käytettävä järvi tietovaraston Lisää tallennustilaa, voit tehdä Sqoop (ilman muutoksia määritys) Tuo/Vie tiedot välillä (Tässä esimerkissä Azure SQL-tietokanta) relaatio tietokannan ja järvi tietovaraston tilin. 

1. Tässä opetusohjelmassa oletetaan luomasi Linux-klusterin niin muodostaa yhteyttä klusterin SSH avulla. Artikkelissa [etäyhteyden muodostaminen Linux-pohjaiset HDInsight-klusterin](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster).

2. Tarkista, onko on saatavana järvi tietovaraston tilin klusterista. Suorita seuraava komento SSH seuraavasti:

        
        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net/

    Tämä on tarjottava tiedostot tai kansiot järvi tietovaraston tilin luettelo.

### <a name="import-data-from-azure-sql-database-into-data-lake-store"></a>Tietojen tuominen järvi tietovaraston Azure SQL-tietokanta

3. Siirry kansioon, johon Sqoop paketit ovat käytettävissä. Tavallisesti tämä on osoitteessa `/usr/hdp/<version>/sqoop/bin`. 

4. Tuo tiedot **Taulukko1** järvi tietosäilö-tilille. Käytä seuraavaa syntaksia:

        
        sqoop-import --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table1 --target-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1

    Huomaa, että **sql-tietokanta-palvelin-name** paikkamerkki jossa Azure SQL-tietokanta on käynnissä palvelimen nimi. **SQL-tietokanta-nimen** paikkamerkki todellinen tietokannan nimi.

    Esimerkiksi

        
        sqoop-import --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table1 --target-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1

5. Varmista, että tiedot on siirretty järvi tietosäilö-tilille. Suorita seuraava komento:

        
        hdfs dfs -ls adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/

    Raportissa pitäisi näkyä seuraava tulos.

        
        -rwxrwxrwx   0 sshuser hdfs          0 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/_SUCCESS
        -rwxrwxrwx   0 sshuser hdfs         12 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00000
        -rwxrwxrwx   0 sshuser hdfs         14 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00001
        -rwxrwxrwx   0 sshuser hdfs         13 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00002
        -rwxrwxrwx   0 sshuser hdfs         18 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00003

    Kunkin **osa-m -** * -tiedosto vastaa yhtä riville lähdetaulukon, * *Taulukko1**. Voit tarkastella osan sisältöä-m -* vahvistamiseksi tiedostot.


### <a name="export-data-from-data-lake-store-into-azure-sql-database"></a>Tietojen tuominen Azure SQL-tietokanta järvi tietosäilö

6. Vie tiedot järvi tietovaraston tililtä tyhjän taulukon **taulukko2**Azure SQL-tietokanta. Käytä seuraavaa syntaksia.

        
        sqoop-export --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table2 --export-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

    Esimerkiksi

        
        sqoop-export --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table2 --export-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

6. Varmista, että tiedot on ladattu SQL-tietokannan taulukkoon. Käytä [SQL Server Management Studiossa](../sql-database/sql-database-connect-query-ssms.md) tai Visual Studio suorittamalla seuraava kysely ja Azure SQL-tietokannan yhdistäminen.

        
        SELECT * FROM TABLE2

    Tämä on seuraava tulos.

        ID  FName   LName
        ------------------
        1   Neal    Kell
        2   Lila    Fulton
        3   Erna    Myers
        4   Annette Simpson

## <a name="see-also"></a>Katso myös

- [Tietojen kopioiminen Azure-tallennustilan BLOB järvi tietosäilö](data-lake-store-copy-data-azure-storage-blob.md)
- [Tietojen suojaaminen järvi säilössä](data-lake-store-secure-data.md)
- [Azure tietojen järvi Analytics käyttäminen järvi tietosäilö](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure Hdinsightiin käyttäminen järvi tietosäilö](data-lake-store-hdinsight-hadoop-use-portal.md)
