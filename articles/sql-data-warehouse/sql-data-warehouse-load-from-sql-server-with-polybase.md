<properties
   pageTitle="Tietojen lataaminen SQL Server Azure SQL-tietovarasto (PolyBase) | Microsoft Azure"
   description="Tietojen vieminen tasainen tiedostoja, voit tuoda tietoja Azure-blob-säiliö AZCopy ja PolyBase ingest tietojen tuominen SQL Azure-tietovarasto SQL Server käyttää bcp."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/30/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="load-data-with-polybase-in-sql-data-warehouse"></a>SQL-tietovarasto PolyBase tietojen lataaminen

> [AZURE.SELECTOR]
- [SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
- [PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md)
- [BCP](sql-data-warehouse-load-from-sql-server-with-bcp.md)

Tässä opetusohjelmassa näytetään, miten voit ladata tietoja SQL-tietovarasto AzCopy ja PolyBase avulla. Kun olet valmis, saat tietää, miten voit:

- Tietojen kopioiminen Azure-blob-säiliö AzCopy avulla
- Voit määrittää tietojen tietokantaobjektien luominen
- Suorita T-SQL-kysely lataa tiedot

>[AZURE.VIDEO loading-data-with-polybase-in-azure-sql-data-warehouse]

## <a name="prerequisites"></a>Edellytykset

Jos haluat käydä läpi opetusohjelman,

- SQL-tietovarasto tietokanta.
- Azure-tallennustilan tilin tyyppiä vakio paikallisesti tarpeettomat Storage (vakio-LRS) Vakio Geo Redundant Storage (vakio-GRS) tai vakio-ja lukuoikeudet Geo Redundant tallennustilan (vakio-RAGRS).
- Komentorivin AzCopy-apuohjelma. Lataa ja asenna [uusin versio AzCopy][] , joka on asennettu Microsoft Azure-tallennustilan työkalut.

    ![Azure-tallennustilan Työkalut](./media/sql-data-warehouse-get-started-load-with-polybase/install-azcopy.png)


## <a name="step-1-add-sample-data-to-azure-blob-storage"></a>Vaihe 1: Lisää mallitiedot Azure-blob-säiliö

Jotta voit ladata tietoja annettava sijoittaa Azure-blob-säiliö mallitietoja. Tässä vaiheessa on lisätä tallennustilaa Azure-blob mallitiedot. Käytämme myöhemmin, Lataa tämä mallitiedot SQL-tietovarasto tietokannan PolyBase.

### <a name="a-prepare-a-sample-text-file"></a>A. Valmistele tekstin mallitiedosto

Valmistele tekstin mallitiedosto:

1. Avaa Muistio ja kopioi seuraavat rivit tiedot uuteen tiedostoon. Tallentaa tämä paikallisen temp-kansioon % temp%\DimDate2.txt.

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

### <a name="b-find-your-blob-service-endpoint"></a>B. Etsi blob-palvelupäätepiste

Voit etsiä blob-Palvelupäätepisteen seuraavasti:

1. Azure-portaalista valitsemalla **Selaa** > **Tallennustilan tilit**.
2. Valitse tallennustilan-tili, jota haluat käyttää.
3. Valitse BLOB Storage tili-sivu

    ![Valitse BLOB-objektit](./media/sql-data-warehouse-get-started-load-with-polybase/click-blobs.png)

1. Tallenna blob palvelun päätepisteen URL-osoite myöhempää käyttöä varten.

    ![BLOB-palvelupäätepiste](./media/sql-data-warehouse-get-started-load-with-polybase/blob-service.png)

### <a name="c-find-your-azure-storage-key"></a>C-NÄPPÄINYHDISTELMÄÄ. Azure-tallennustilan key-tuotetunnuksen etsiminen

Azure-tallennustilan key-tuotetunnuksen etsiminen:

1. Azure-portaalista, valitse **Selaa** > **Tallennustilan tilit**.
2. Valitse tallennustilan-tili, jota haluat käyttää.
3. Valitse **kaikki asetukset** > **pikanäppäimet**.
4. Napsauta kopioi-ruutuun jokin access-näppäimiä kopioiminen Leikepöydälle.

    ![Kopioi Azure tallennustilan avain](./media/sql-data-warehouse-get-started-load-with-polybase/access-key.png)

### <a name="d-copy-the-sample-file-to-azure-blob-storage"></a>D. Kopioi mallitiedostossa Azure-blob-säiliö

Tietojen kopioiminen Azure-blob-säiliö:

1. Avaa komentorivi-ikkuna ja muuta kansioiden AzCopy asennus-kansioon. Tämä komento muuttuu oletuskansio asennuksen 64-bittinen Windows-asiakasohjelmassa.

    ```
    cd /d "%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy"
    ```

1. Suorita seuraava komento lataa tiedosto. Määritä blob palvelun päätepisteen URL, <blob service endpoint URL> ja Azure tallennustilan tiliavain < azure_storage_account_key >.

    ```
    .\AzCopy.exe /Source:C:\Temp\ /Dest:<blob service endpoint URL> /datacontainer/datedimension/ /DestKey:<azure_storage_account_key> /Pattern:DimDate2.txt
    ```

Katso myös [Aloittaminen AzCopy komentorivivalitsimet-apuohjelman][uusin versio AzCopy].

### <a name="e-explore-your-blob-storage-container"></a>E. Tutustu blob-tallennustilan säilö

Jos haluat nähdä tiedoston ladattujen Blob-objektien tallennustilaan:

1. Siirry Blob service-sivu.
2. Kaksoisnapsauta säilöt- **datacontainer**.
3. Tutustu polku tietoihin, valitse kansio **datedimension** ja näet ladatun tiedoston **DimDate2.txt**.
4. Voit tarkastella ominaisuudet valitsemalla **DimDate2.txt**.
5. Huomaa, että Blob-objektien ominaisuudet-sivu, voit ladata tai poistaa tiedoston.

    ![Näytä tallennustilan Azure-blob](./media/sql-data-warehouse-get-started-load-with-polybase/view-blob.png)


## <a name="step-2-create-an-external-table-for-the-sample-data"></a>Vaihe 2: Luo ulkoisen taulukon mallitietoja varten

Tässä osassa on luoda ulkoisen taulukon, joka määrittää mallitietoja.

PolyBase käyttää ulkoisia taulukoita käyttää Azure Blob-objektien tallennustilaan tallennettuja tietoja. Koska SQL Data Warehouse ei tallennettuna tiedot, PolyBase käsittelee todennus ulkoisiin tietoihin käyttämällä tietokannan kohdistettu tunnistetiedon.

Tässä esimerkissä käytetään näitä Transact-SQL-lauseita luomaan ulkoisen taulukon.

- [Luo Master Key (Transact-SQL)][] voit salata tietokannan toiminta määritetty tunnistetiedon.
- Voit määrittää todennustiedot Azure-tallennustilan tilin [Luominen tietokannan suodatetut tunnistetiedon (Transact-SQL)][] .
- [Luo ulkoinen tietolähde (Transact-SQL)][] Määritä Azure-blob-säiliö sijainti.
- [Luo ulkoinen tiedostomuodossa (Transact-SQL)][] Määritä muodon tiedot.
- [Luo ulkoinen taulukko (Transact-SQL)][] ja määritä taulukkomääritys ja tietojen sijainti.

Suorita kysely SQL-tietovarasto tietokannalle. Se Luo nimetty DimDate2External dbo rakennetta, joka osoittaa Azure-blob-säiliö DimDate2.txt esimerkkitiedot ulkoisen taulukon.


```sql
-- A: Create a master key.
-- Only necessary if one does not already exist.
-- Required to encrypt the credential secret in the next step.

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- IDENTITY: Provide any string, it is not used for authentication to Azure storage.
-- SECRET: Provide your Azure storage account key.


CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
WITH
    IDENTITY = 'user',
    SECRET = '<azure_storage_account_key>'
;


-- C: Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure blob storage.
-- LOCATION: Provide Azure storage account name and blob container name.
-- CREDENTIAL: Provide the credential created in the previous step.

CREATE EXTERNAL DATA SOURCE AzureStorage
WITH (
    TYPE = HADOOP,
    LOCATION = 'wasbs://<blob_container_name>@<azure_storage_account_name>.blob.core.windows.net',
    CREDENTIAL = AzureStorageCredential
);


-- D: Create an external file format
-- FORMAT_TYPE: Type of file format in Azure storage (supported: DELIMITEDTEXT, RCFILE, ORC, PARQUET).
-- FORMAT_OPTIONS: Specify field terminator, string delimiter, date format etc. for delimited text files.
-- Specify DATA_COMPRESSION method if data is compressed.

CREATE EXTERNAL FILE FORMAT TextFile
WITH (
    FORMAT_TYPE = DelimitedText,
    FORMAT_OPTIONS (FIELD_TERMINATOR = ',')
);


-- E: Create the external table
-- Specify column names and data types. This needs to match the data in the sample file.
-- LOCATION: Specify path to file or directory that contains the data (relative to the blob container).
-- To point to all files under the blob container, use LOCATION='.'

CREATE EXTERNAL TABLE dbo.DimDate2External (
    DateId INT NOT NULL,
    CalendarQuarter TINYINT NOT NULL,
    FiscalQuarter TINYINT NOT NULL
)
WITH (
    LOCATION='/datedimension/',
    DATA_SOURCE=AzureStorage,
    FILE_FORMAT=TextFile
);


-- Run a query on the external table

SELECT count(*) FROM dbo.DimDate2External;

```


SQL Server Object Explorerissa Visual Studiossa näet ulkoisessa tiedostomuodossa, ulkoista tietolähdettä ja DimDate2External-taulukon.

![Näytä ulkoisen taulukon](./media/sql-data-warehouse-get-started-load-with-polybase/external-table.png)

## <a name="step-3-load-data-into-sql-data-warehouse"></a>Vaihe 3: Lataa tiedot SQL-tietovarasto

Kun ulkoisen taulukon on luotu, voit ladata tietoja uuteen taulukkoon tai lisääminen aiemmin luotuun taulukkoon.

- Lataa tiedot uuteen taulukkoon, suorita [Luo taulukko liitetään Valitse (Transact-SQL)][] -lause. Uusi taulukko on nimetty kysely sarakkeet. Sarakkeiden tietotyyppien vastaavat ulkoisen taulukkomääritys-tietotyypit.
- Jos haluat ladata tiedot aiemmin luotuun taulukkoon, käytä [Lisää... Valitse (Transact-SQL)][] lause.

```sql
-- Load the data from Azure blob storage to SQL Data Warehouse

CREATE TABLE dbo.DimDate2
WITH
(   
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT * FROM [dbo].[DimDate2External];
```

## <a name="step-4-create-statistics-on-your-newly-loaded-data"></a>Vaihe 4: Luo tilastotiedot ladatut tiedot

SQL-tietovarasto ei luominen automaattisesti tai automaattinen päivitys tilastotiedot. Vuoksi saavuttamiseksi hyvin kyselyn suorituskykyä, on tärkeää tilastotiedot luomisesta kunkin sarakkeen kunkin taulukon ensimmäinen kuormituksen. Kannattaa myös päivittää merkittävät muutokset tietojen jälkeen tilastotiedot.

Tässä esimerkissä Luo yksisarakkeisen tilastotiedot DimDate2 uuteen taulukkoon.

```sql
CREATE STATISTICS [DateId] on [DimDate2] ([DateId]);
CREATE STATISTICS [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
CREATE STATISTICS [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
```

Lisätietoja on artikkelissa [tilastotiedot][].  


## <a name="next-steps"></a>Seuraavat vaiheet
Saat lisätietoja sinun tulee tietää, kun kehität ratkaisuun, joka käyttää PolyBase [PolyBase opas][] .

<!--Image references-->


<!--Article references-->
[PolyBase in SQL Data Warehouse Tutorial]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Tilastotiedot]: ./sql-data-warehouse-tables-statistics.md
[PolyBase opas]: ./sql-data-warehouse-load-polybase-guide.md
[AzCopy uusin versio]: ../storage/storage-use-azcopy.md

<!--External references-->
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server destination adapter]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx


[Luo ulkoinen TIETOLÄHDE (Transact-SQL)]:https://msdn.microsoft.com/library/dn935022.aspx
[Luo ulkoinen TIEDOSTOMUODOSSA (Transact-SQL)]:https://msdn.microsoft.com/library/dn935026.aspx
[Luo ulkoinen taulukko (Transact-SQL)]:https://msdn.microsoft.com/library/dn935021.aspx

[DROP EXTERNAL DATA SOURCE (Transact-SQL)]:https://msdn.microsoft.com/library/mt146367.aspx
[DROP EXTERNAL FILE FORMAT (Transact-SQL)]:https://msdn.microsoft.com/library/mt146379.aspx
[DROP EXTERNAL TABLE (Transact-SQL)]:https://msdn.microsoft.com/library/mt130698.aspx

[Luo taulukon AS SELECT (Transact-SQL)]:https://msdn.microsoft.com/library/mt204041.aspx
[LISÄÄ... Valitse (Transact-SQL)]:https://msdn.microsoft.com/library/ms174335.aspx
[Luo MASTER KEY (Transact-SQL)]:https://msdn.microsoft.com/library/ms174382.aspx
[CREATE CREDENTIAL (Transact-SQL)]:https://msdn.microsoft.com/library/ms189522.aspx
[Luo tietokanta SUODATETUT TUNNISTETIEDON (Transact-SQL)]:https://msdn.microsoft.com/library/mt270260.aspx
[DROP CREDENTIAL (Transact-SQL)]:https://msdn.microsoft.com/library/ms189450.aspx
