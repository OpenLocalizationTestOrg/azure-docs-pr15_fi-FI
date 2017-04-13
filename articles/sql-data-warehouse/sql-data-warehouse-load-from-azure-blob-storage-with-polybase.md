<properties
   pageTitle="Tietojen lataaminen Azure-blob-säiliö SQL-tietovarasto (PolyBase) | Microsoft Azure"
   description="Opettele käyttämään PolyBase lataamaan tietojen Azure-blob-säiliö SQL-tietovarasto. Lataa joitakin taulukoita julkisten tietojen Contoso jälleenmyynti tietovarasto rakenne."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="load-data-from-azure-blob-storage-into-sql-data-warehouse-polybase"></a>Tietojen lataaminen Azure-blob-säiliö SQL-tietovarasto (PolyBase)

> [AZURE.SELECTOR]
- [Tietoja Factory](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md)
- [PolyBase](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)

Tietojen lataaminen Azure-blob-säiliö Azure SQL-tietovarasto PolyBase ja T-SQL-komentoja avulla. 

Tässä opetusohjelmassa ladataan, Pyri yksinkertaisuuteen kahden taulukon julkisen Azure-tallennustilan Blob-Contoso jälleenmyynti tietovarasto rakenne. Lataa koko tietojoukko, suorita [ladata koko Contoso jälleenmyynti tietovarasto][] Esimerkki Microsoft SQL Server näytteiden säilöstä.

Tässä opetusohjelmassa menettelet seuraavasti:

1. Määritä PolyBase Lataa Azure-blob-säiliö
2. Julkisten tietojen lataaminen tietokantaan
3. Suorita optimointi, kun lataus on valmis.


## <a name="before-you-begin"></a>Ennen aloittamista
Jos haluat suorittaa tässä opetusohjelmassa, sinun on Azure-tili, jolla on jo tietovarasto SQL-tietokanta. Jos sinulla ei vielä ole tämä, katso [luominen SQL-tietovarasto][].

## <a name="1-configure-the-data-source"></a>1. Määritä tietolähde

PolyBase määrittää ulkoisen T-SQL-objektien sijainti ja määritteet ulkoisten tietojen avulla. Ulkoisen objektimääritykset on tallennettu SQL-tietovarasto. Itse tiedot tallennetaan ulkoisesti.

### <a name="11-create-a-credential"></a>1.1. Luo olevan tunnistetiedon

Jos haluat ladata Contoso julkisten tietojen **Ohita tämä vaihe** . Sinun ei tarvitse julkisten tietojen suojattu käyttö, koska se on jo kaikkien käyttäjien käytettävissä.

**Älä ohita tämä vaihe** , jos käytät Tässä opetusohjelmassa mallina lataamisen omilla tiedoillasi. Tietojen käyttämistä varten kautta olevan tunnistetiedon seuraavaa komentosarjaa avulla voit luoda tietokantaan kohdistettu tunnistetiedon ja käyttää sitä, kun määrität tietolähteen sijainti.


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
```

Siirry vaiheeseen 2.

### <a name="12-create-the-external-data-source"></a>1.2. Luo ulkoinen tietolähde

[Luo ULKOISEEN TIETOLÄHTEESEEN][] tällä komennolla voit tallentaa tiedot ja tietojen tyypin sijainti. 

```sql
CREATE EXTERNAL DATA SOURCE AzureStorage_west_public
WITH 
(  
    TYPE = Hadoop 
,   LOCATION = 'wasbs://contosoretaildw-tables@contosoretaildw.blob.core.windows.net/'
); 
```

> [AZURE.IMPORTANT] Jos valitset julkisiksi azure-blob tallennustilan säiliöiden, muista, että tietojen omistajana voit veloitetaanko tietojen juniin kulujen kun tietojen poistuu tietokeskuksen. 

## <a name="2-configure-data-format"></a>2. Määritä tiedot-muoto

Tiedot on tallennettu tekstitiedostojen Azure-blob-säiliö ja kunkin kentän toisistaan erottimen. Suorita tämä [Luo ULKOISEN TIEDOSTOMUOTO][] -komento on määritettävä tiedot muodossa tekstitiedostoista. Contoso-tiedot ovat pakkaamattomat ja putkien erotettu.

```sql
CREATE EXTERNAL FILE FORMAT TextFileFormat 
WITH 
(   FORMAT_TYPE = DELIMITEDTEXT
,   FORMAT_OPTIONS  (   FIELD_TERMINATOR = '|'
                    ,   STRING_DELIMITER = ''
                    ,   DATE_FORMAT      = 'yyyy-MM-dd HH:mm:ss.fff'
                    ,   USE_TYPE_DEFAULT = FALSE 
                    )
);
``` 

## <a name="3-create-the-external-tables"></a>3. ulkoisen taulukoiden luominen

Nyt kun olet määrittänyt tietojen lähteen ja tiedostomuoto, olet valmis luomaan ulkoiset taulukot. 

### <a name="31-create-a-schema-for-the-data"></a>3.1. Tietojen rakenteen luomista. 

Voit luoda Contoso-tiedot tallennetaan tietokannan sijainti, rakenteen luomista.

```sql
CREATE SCHEMA [asb]
GO
```

### <a name="32-create-the-external-tables"></a>3.2. Luo ulkoinen taulukot. 

Suorittamalla tämän DimProduct ja FactOnlineSales ulkoisen taulukoiden luominen. Kaikki Microsoft tekevät tähän on sarakkeiden nimet ja tietotyyppien määrittäminen ja ne sitominen sijainti ja Azure-blob-tiedostojen tallentaminen muodossa. Määritys on tallennettu SQL-tietovarasto ja tiedot ovat silti Azure Blob-objektien tallennustilaan.

**Sijainti** -parametri on kansion pääkansion tallennustilan Azure-Blob-kohdassa. Jokaisessa taulukossa on eri kansioon.


```sql

--DimProduct
CREATE EXTERNAL TABLE [asb].DimProduct (
    [ProductKey] [int] NOT NULL,
    [ProductLabel] [nvarchar](255) NULL,
    [ProductName] [nvarchar](500) NULL,
    [ProductDescription] [nvarchar](400) NULL,
    [ProductSubcategoryKey] [int] NULL,
    [Manufacturer] [nvarchar](50) NULL,
    [BrandName] [nvarchar](50) NULL,
    [ClassID] [nvarchar](10) NULL,
    [ClassName] [nvarchar](20) NULL,
    [StyleID] [nvarchar](10) NULL,
    [StyleName] [nvarchar](20) NULL,
    [ColorID] [nvarchar](10) NULL,
    [ColorName] [nvarchar](20) NOT NULL,
    [Size] [nvarchar](50) NULL,
    [SizeRange] [nvarchar](50) NULL,
    [SizeUnitMeasureID] [nvarchar](20) NULL,
    [Weight] [float] NULL,
    [WeightUnitMeasureID] [nvarchar](20) NULL,
    [UnitOfMeasureID] [nvarchar](10) NULL,
    [UnitOfMeasureName] [nvarchar](40) NULL,
    [StockTypeID] [nvarchar](10) NULL,
    [StockTypeName] [nvarchar](40) NULL,
    [UnitCost] [money] NULL,
    [UnitPrice] [money] NULL,
    [AvailableForSaleDate] [datetime] NULL,
    [StopSaleDate] [datetime] NULL,
    [Status] [nvarchar](7) NULL,
    [ImageURL] [nvarchar](150) NULL,
    [ProductURL] [nvarchar](150) NULL,
    [ETLLoadID] [int] NULL,
    [LoadDate] [datetime] NULL,
    [UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/DimProduct/' 
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;
 
--FactOnlineSales
CREATE EXTERNAL TABLE [asb].FactOnlineSales 
(
    [OnlineSalesKey] [int]  NOT NULL,
    [DateKey] [datetime] NOT NULL,
    [StoreKey] [int] NOT NULL,
    [ProductKey] [int] NOT NULL,
    [PromotionKey] [int] NOT NULL,
    [CurrencyKey] [int] NOT NULL,
    [CustomerKey] [int] NOT NULL,
    [SalesOrderNumber] [nvarchar](20) NOT NULL,
    [SalesOrderLineNumber] [int] NULL,
    [SalesQuantity] [int] NOT NULL,
    [SalesAmount] [money] NOT NULL,
    [ReturnQuantity] [int] NOT NULL,
    [ReturnAmount] [money] NULL,
    [DiscountQuantity] [int] NULL,
    [DiscountAmount] [money] NULL,
    [TotalCost] [money] NOT NULL,
    [UnitCost] [money] NULL,
    [UnitPrice] [money] NULL,
    [ETLLoadID] [int] NULL,
    [LoadDate] [datetime] NULL,
    [UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/FactOnlineSales/' 
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;
```

## <a name="4-load-the-data"></a>4. Lataa tiedot
Ei voi käyttää ulkoisia tietoja eri tavoilla.  Voit kyselyn suoraan ulkoisen taulukon tietoihin, lataa uuden tietokannan taulukoiden tiedot tai Lisää ulkoisia tietoja aiemmin tietokantataulukot.  


### <a name="41-create-a-new-schema"></a>4.1. Luo uusi rakenne

CTAS Luo uuden taulukon, joka sisältää tiedot.  Luo contoso-tietojen rakennetta.

```sql
CREATE SCHEMA [cso]
GO
```

### <a name="42-load-the-data-into-new-tables"></a>4.2. Lataa tiedot uusien taulukoiden

Tietojen lataaminen Azure-blob-säiliö ja tallenna se sisältyy tietokannan taulukkoon, lauseella [Luo taulukko liitetään Valitse (Transact-SQL)][] . Lataamisen CTAS hyödyntää juuri luomasi erittäin kirjoitetun ulkoiset taulukot. Lataa tiedot uuteen taulukkoon, käytä yhtä taulukkoa kohden [CTAS][] lausetta. 

CTAS Luo uuden taulukon ja lisää se select-lauseen tulokset. CTAS määrittää uuden taulukon on samat sarakkeet ja tietotyypit select-lauseen tulokset. Jos olet valinnut ulkoisen taulukon kaikki sarakkeet, uusi taulukko on käyttävien ulkoisen taulukon tietotyypit ja sarakkeet.

Tässä esimerkissä on luoda dimensio ja hash hajautettu taulukoiden faktataulukon. 


```sql
SELECT GETDATE();
GO

CREATE TABLE [cso].[DimProduct]            WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[DimProduct]             OPTION (LABEL = 'CTAS : Load [cso].[DimProduct]             ');
CREATE TABLE [cso].[FactOnlineSales]       WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[FactOnlineSales]        OPTION (LABEL = 'CTAS : Load [cso].[FactOnlineSales]        ');
```

### <a name="43-track-the-load-progress"></a>4.3 kuormituksen-edistymisen seuraaminen

Voit seurata edistymistä että latautuvat dynaaminen hallinta näkymissä (DMVs). 

```sql
-- To see all requests
SELECT * FROM sys.dm_pdw_exec_requests;

-- To see a particular request identified by its label
SELECT * FROM sys.dm_pdw_exec_requests as r;
WHERE r.[label] = 'CTAS : Load [cso].[DimProduct]             '
      OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
;

-- To track bytes and files
SELECT
    r.command,
    s.request_id,
    r.status,
    count(distinct input_name) as nbr_files, 
    sum(s.bytes_processed)/1024/1024 as gb_processed
FROM
    sys.dm_pdw_exec_requests r
    inner join sys.dm_pdw_dms_external_work s
        on r.request_id = s.request_id
WHERE 
    r.[label] = 'CTAS : Load [cso].[DimProduct]             '
    OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
GROUP BY
    r.command,
    s.request_id,
    r.status
ORDER BY
    nbr_files desc,
    gb_processed desc;
```

## <a name="5-optimize-columnstore-compression"></a>5. optimointi columnstore pakkaus

SQL-tietovarasto tallentaa oletusarvon mukaan taulukon indeksi on liitetty columnstore. Kuormituksen päätyttyä joitakin tietorivit ehkä ei pakata columnstore kyselyjä.  On useita syitä, miksi näin voi käydä. Lisätietoja on artikkelissa [Hallitse columnstore indeksejä][].

Jos haluat optimoida kyselyn suorituskykyä ja columnstore pakkauksen jälkeen kuormituksen, muodosta uudelleen taulukon Pakota columnstore indeksin pakata kaikki rivit. 

```sql
SELECT GETDATE();
GO

ALTER INDEX ALL ON [cso].[DimProduct]               REBUILD;
ALTER INDEX ALL ON [cso].[FactOnlineSales]          REBUILD;
```

Lisätietoja ylläpito columnstore indeksit on artikkelissa [Hallitse columnstore indeksejä][] .

## <a name="6-optimize-statistics"></a>6. tilasto optimointi

Kannattaa luoda yksisarakkeisen tilastotiedot heti, kun kuormituksen. On tehdäänkö tilastoja varten. Esimerkiksi jos luot yksisarakkeisen tilastotiedot jokaisessa sarakkeessa voi kestää kauan uudelleen kaikki tilastotiedot. Jos tiedät, että tietyt sarakkeet, jotka eivät mene olevan kyselyn predikaatit, voit ohittaa niiden sarakkeiden luominen tilastotiedot.

Jos Luo yksisarakkeisen tilastotiedot jokaisessa sarakkeessa jokaisen taulukon, voit käyttää tallennetun toimintosarjan koodin otosten `prc_sqldw_create_stats` [tilastotiedoista][] on artikkelissa.

Seuraavassa esimerkissä on hyvä aloittaa luomisen tilastotiedot. Se luo yksisarakkeisen tilastotiedot kunkin sarakkeen dimensio-taulukossa ja liittävä sarakkeiden kertoma-taulukoissa. Voit aina lisätä yhden tai usean sarakkeen tilastotiedot muiden kertoma taulukkosarakkeiden myöhemmin.


```sql
CREATE STATISTICS [stat_cso_DimProduct_AvailableForSaleDate] ON [cso].[DimProduct]([AvailableForSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_BrandName] ON [cso].[DimProduct]([BrandName]);
CREATE STATISTICS [stat_cso_DimProduct_ClassID] ON [cso].[DimProduct]([ClassID]);
CREATE STATISTICS [stat_cso_DimProduct_ClassName] ON [cso].[DimProduct]([ClassName]);
CREATE STATISTICS [stat_cso_DimProduct_ColorID] ON [cso].[DimProduct]([ColorID]);
CREATE STATISTICS [stat_cso_DimProduct_ColorName] ON [cso].[DimProduct]([ColorName]);
CREATE STATISTICS [stat_cso_DimProduct_ETLLoadID] ON [cso].[DimProduct]([ETLLoadID]);
CREATE STATISTICS [stat_cso_DimProduct_ImageURL] ON [cso].[DimProduct]([ImageURL]);
CREATE STATISTICS [stat_cso_DimProduct_LoadDate] ON [cso].[DimProduct]([LoadDate]);
CREATE STATISTICS [stat_cso_DimProduct_Manufacturer] ON [cso].[DimProduct]([Manufacturer]);
CREATE STATISTICS [stat_cso_DimProduct_ProductDescription] ON [cso].[DimProduct]([ProductDescription]);
CREATE STATISTICS [stat_cso_DimProduct_ProductKey] ON [cso].[DimProduct]([ProductKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductLabel] ON [cso].[DimProduct]([ProductLabel]);
CREATE STATISTICS [stat_cso_DimProduct_ProductName] ON [cso].[DimProduct]([ProductName]);
CREATE STATISTICS [stat_cso_DimProduct_ProductSubcategoryKey] ON [cso].[DimProduct]([ProductSubcategoryKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductURL] ON [cso].[DimProduct]([ProductURL]);
CREATE STATISTICS [stat_cso_DimProduct_Size] ON [cso].[DimProduct]([Size]);
CREATE STATISTICS [stat_cso_DimProduct_SizeRange] ON [cso].[DimProduct]([SizeRange]);
CREATE STATISTICS [stat_cso_DimProduct_SizeUnitMeasureID] ON [cso].[DimProduct]([SizeUnitMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_Status] ON [cso].[DimProduct]([Status]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeID] ON [cso].[DimProduct]([StockTypeID]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeName] ON [cso].[DimProduct]([StockTypeName]);
CREATE STATISTICS [stat_cso_DimProduct_StopSaleDate] ON [cso].[DimProduct]([StopSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_StyleID] ON [cso].[DimProduct]([StyleID]);
CREATE STATISTICS [stat_cso_DimProduct_StyleName] ON [cso].[DimProduct]([StyleName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitCost] ON [cso].[DimProduct]([UnitCost]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureID] ON [cso].[DimProduct]([UnitOfMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureName] ON [cso].[DimProduct]([UnitOfMeasureName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitPrice] ON [cso].[DimProduct]([UnitPrice]);
CREATE STATISTICS [stat_cso_DimProduct_UpdateDate] ON [cso].[DimProduct]([UpdateDate]);
CREATE STATISTICS [stat_cso_DimProduct_Weight] ON [cso].[DimProduct]([Weight]);
CREATE STATISTICS [stat_cso_DimProduct_WeightUnitMeasureID] ON [cso].[DimProduct]([WeightUnitMeasureID]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CurrencyKey] ON [cso].[FactOnlineSales]([CurrencyKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CustomerKey] ON [cso].[FactOnlineSales]([CustomerKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_DateKey] ON [cso].[FactOnlineSales]([DateKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_OnlineSalesKey] ON [cso].[FactOnlineSales]([OnlineSalesKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_ProductKey] ON [cso].[FactOnlineSales]([ProductKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_PromotionKey] ON [cso].[FactOnlineSales]([PromotionKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_StoreKey] ON [cso].[FactOnlineSales]([StoreKey]);
```

## <a name="achievement-unlocked"></a>Tavoitteiden lukitus!

Julkisten tietojen on onnistuneesti ladattu Azure SQL-tietovarasto. Hyvää työtä!

Voit nyt aloittaa kyselyt taulukoiden käyttäminen, kuten jompikumpi seuraavista:

```sql
SELECT  SUM(f.[SalesAmount]) AS [sales_by_brand_amount]
,       p.[BrandName]
FROM    [cso].[FactOnlineSales] AS f
JOIN    [cso].[DimProduct]      AS p ON f.[ProductKey] = p.[ProductKey]
GROUP BY p.[BrandName]
```

## <a name="next-steps"></a>Seuraavat vaiheet
Lataa koko Contoso jälleenmyynti tietovarasto tiedot, käytä komentosarja kehittäminen Lisävihjeitä varten on artikkelissa [SQL-tietovarasto kehittäminen yleiskatsaus][].

<!--Image references-->

<!--Article references-->
[SQL-tietovarasto luominen]: sql-data-warehouse-get-started-provision.md
[Load data into SQL Data Warehouse]: sql-data-warehouse-overview-load.md
[SQL-tietovarasto kehittäminen yleiskatsaus]: sql-data-warehouse-overview-develop.md
[Hallitse columnstore indeksejä]: sql-data-warehouse-tables-index.md
[Tilastotiedot]: sql-data-warehouse-tables-statistics.md
[CTAS]: sql-data-warehouse-develop-ctas.md
[label]: sql-data-warehouse-develop-label.md

<!--MSDN references-->
[LUO ULKOINEN TIETOLÄHDE]: https://msdn.microsoft.com/en-us/library/dn935022.aspx
[LUO ULKOINEN TIEDOSTOMUOTO]: https://msdn.microsoft.com/en-us/library/dn935026.aspx
[Luo taulukon AS SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[sys.dm_pdw_exec_requests]: https://msdn.microsoft.com/library/mt203887.aspx
[REBUILD]: https://msdn.microsoft.com/library/ms188388.aspx

<!--Other Web references-->
[Microsoft Download Center]: http://www.microsoft.com/download/details.aspx?id=36433
[Lataa koko Contoso jälleenmyynti-tietovarasto]: https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/contoso-data-warehouse/readme.md
