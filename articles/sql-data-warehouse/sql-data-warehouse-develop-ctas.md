<properties
   pageTitle="Luo taulukko, valitse (CTAS) SQL-tietovarasto | Microsoft Azure"
   description="Koodaus ja Luo taulukko nimellä select Azure SQL-tietovarasto (CTAS)-lauseen, ratkaisujen kehittämiseen liittyviä vinkkejä."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/14/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="create-table-as-select-ctas-in-sql-data-warehouse"></a>Luo taulukko valitsemalla (CTAS) kuin SQL-tietovarasto
Luo taulukko valitsemalla nimellä tai `CTAS` on yksi tärkeimmistä T-SQL-ominaisuuksista on käytettävissä. On täysin parallelized toiminto, joka luo uuden taulukon perusteella SELECT-lauseen tulos. `CTAS`on helpoin ja nopein tapa luoda kopion taulukon. Määrittää ahdetut versio, harkitse `SELECT..INTO` halutessasi. Tässä tiedostossa on esimerkkejä ja parhaita käytäntöjä `CTAS`.

## <a name="using-ctas-to-copy-a-table"></a>Taulukon kopioiminen CTAS avulla

Esimerkiksi jokin yleisimpiä käyttötavat `CTAS` luodaan taulukon kopio niin, että voit muuttaa DDL. Jos esimerkiksi alun perin luotu kuin taulukon `ROUND_ROBIN` ja, kun haluat vaihtaa sen jakaa sarakkeen, taulukon `CTAS` voit muuttaa jakauman-sarake. `CTAS`voi käyttää myös voit muuttaa jakaminen, indeksointi tai sarakkeen tyyppiä.

Oletetaan, että olet luonut taulukon oletusarvon jakauman tyyppi `ROUND_ROBIN` jaetaan, koska ei jakauman sarake on määritetty `CREATE TABLE`.

```sql
CREATE TABLE FactInternetSales
(
    ProductKey int NOT NULL,
    OrderDateKey int NOT NULL,
    DueDateKey int NOT NULL,
    ShipDateKey int NOT NULL,
    CustomerKey int NOT NULL,
    PromotionKey int NOT NULL,
    CurrencyKey int NOT NULL,
    SalesTerritoryKey int NOT NULL,
    SalesOrderNumber nvarchar(20) NOT NULL,
    SalesOrderLineNumber tinyint NOT NULL,
    RevisionNumber tinyint NOT NULL,
    OrderQuantity smallint NOT NULL,
    UnitPrice money NOT NULL,
    ExtendedAmount money NOT NULL,
    UnitPriceDiscountPct float NOT NULL,
    DiscountAmount float NOT NULL,
    ProductStandardCost money NOT NULL,
    TotalProductCost money NOT NULL,
    SalesAmount money NOT NULL,
    TaxAmt money NOT NULL,
    Freight money NOT NULL,
    CarrierTrackingNumber nvarchar(25),
    CustomerPONumber nvarchar(25)
);
```

Nyt haluat luoda uuden taulukon indeksin Columnstore niin, että voit hyödyntää liitetty Columnstore taulukoiden suorituskyvyn. Myös jaettava tämän taulukon ProductKey, koska ovat eri liitokset tämän sarakkeen mukaan ja haluat estää tietojen siirto-ProductKey liitokset aikana. Lisäksi myös lisättävän jakaminen-OrderDateKey niin, että voit nopeasti poistaa vanhoja tietoja pudottamalla vanhat osiot. Seuraavassa on CTAS-lause, jonka kopioida vanhan taulukon uuteen taulukkoon.

```sql
CREATE TABLE FactInternetSales_new
WITH
(
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = HASH(ProductKey),
    PARTITION
    (
        OrderDateKey RANGE RIGHT FOR VALUES
        (
        20000101,20010101,20020101,20030101,20040101,20050101,20060101,20070101,20080101,20090101,
        20100101,20110101,20120101,20130101,20140101,20150101,20160101,20170101,20180101,20190101,
        20200101,20210101,20220101,20230101,20240101,20250101,20260101,20270101,20280101,20290101
        )
    )
)
AS SELECT * FROM FactInternetSales;
```

Lopuksi voit nimetä uudelleen, Vaihda uuteen taulukkoon ja pudota vanhan taulukon taulukoissa.

```sql
RENAME OBJECT FactInternetSales TO FactInternetSales_old;
RENAME OBJECT FactInternetSales_new TO FactInternetSales;

DROP TABLE FactInternetSales_old;
```

> [AZURE.NOTE] Azure SQL-tietovarasto ei vielä tuki automaattinen luominen tai automaattisen päivityksen tilastotiedot.  Jotta saat parhaan suorituskyvyn-kyselyissä, on tärkeää, että tilastotiedot luodaan kaikkien taulukoiden kaikkien sarakkeiden jälkeen ensimmäisen kuormituksen tai merkittäviä muutoksia esiintyvät tiedot.  Yksityiskohtainen kuvaus tilasto on aiheessa [tilastotiedot][] aiheista kehittäminen-ryhmässä.

## <a name="using-ctas-to-work-around-unsupported-features"></a>Kiertää ominaisuuksia CTAS avulla

`CTAS`voi käyttää myös voidaan ratkaista useita alla ei-tuettuja ominaisuuksia. Tämä usein voit todistaa voidaan voitto/win tilanteen, paitsi koodi on yhteensopiva, mutta se suoritetaan usein nopeampaa-SQL-tietovarasto. Tämä on täysin parallelized rakenteineen tuloksena. Tilanteita, joissa voi olla käyttänyt ympärille CTAS ovat seuraavat:

- VALITSE... TUOMINEN
- ANSI-LIITOKSET päivitykset
- ANSI-poistaa liitokset
- Yhdistä lause

> [AZURE.NOTE] Kokeile huomioon otettavia "CTAS ensimmäisen". Jos arvelet, että voit ratkaista ongelman käyttämällä `CTAS` sitten, joka on yleensä paras esitellä se – myös silloin, kun kirjoitat enemmän tietoja tuloksena.
>

## <a name="selectinto"></a>VALITSE... TUOMINEN
Saatat löytää `SELECT..INTO` näkyy ratkaisu paikoille määrä.

Alla on esimerkki `SELECT..INTO` lause:

```sql
SELECT *
INTO    #tmp_fct
FROM    [dbo].[FactInternetSales]
```

Voit muuntaa edellä, `CTAS` on aivan vakava Välitä:

```sql
CREATE TABLE #tmp_fct
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
;
```

> [AZURE.NOTE] CTAS edellyttää tällä hetkellä jakauman sarakkeen erikseen.  Jos ei tarkoituksella yrität muuttaa jakauman-sarakkeen lisääminen `CTAS` suorittaa nopeimmin Jos valitset jakauman sarakkeessa, joka on sama kuin pohjana olevassa taulukossa, kun tämä strategia vältetään tietojen siirto.  Jos olet luomassa pienen taulukon, josta suorituskyvyn ei ole kerroin, ja voit määrittää `ROUND_ROBIN` Vältä päättävän jakauman sarakkeen.

## <a name="ansi-join-replacement-for-update-statements"></a>ANSI liity korvaa päivityksen lauseet

Saatat löytää monimutkaisia päivitys, joka yhdistää useampaa kuin kahta taulukkoa ehtorivien ANSI liittyminen syntaksi suorittamiseen päivittäminen tai poistaminen.

Kuvitellaan, on päivitettävä tässä taulukossa:

```sql
CREATE TABLE [dbo].[AnnualCategorySales]
(   [EnglishProductCategoryName]    NVARCHAR(50)    NOT NULL
,   [CalendarYear]                  SMALLINT        NOT NULL
,   [TotalSalesAmount]              MONEY           NOT NULL
)
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
;
```

Alkuperäinen kysely saattaa olla etsitään seuraavankaltaiselta:

```sql
UPDATE  acs
SET     [TotalSalesAmount] = [fis].[TotalSalesAmount]
FROM    [dbo].[AnnualCategorySales]     AS acs
JOIN    (
        SELECT  [EnglishProductCategoryName]
        ,       [CalendarYear]
        ,       SUM([SalesAmount])              AS [TotalSalesAmount]
        FROM    [dbo].[FactInternetSales]       AS s
        JOIN    [dbo].[DimDate]                 AS d    ON s.[OrderDateKey]             = d.[DateKey]
        JOIN    [dbo].[DimProduct]              AS p    ON s.[ProductKey]               = p.[ProductKey]
        JOIN    [dbo].[DimProductSubCategory]   AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
        JOIN    [dbo].[DimProductCategory]      AS c    ON u.[ProductCategoryKey]       = c.[ProductCategoryKey]
        WHERE   [CalendarYear] = 2004
        GROUP BY
                [EnglishProductCategoryName]
        ,       [CalendarYear]
        ) AS fis
ON  [acs].[EnglishProductCategoryName]  = [fis].[EnglishProductCategoryName]
AND [acs].[CalendarYear]                = [fis].[CalendarYear]
;
```

Koska SQL Data Warehouse ei tue ANSI liitokset `FROM` lauseke `UPDATE` -lauseessa, et voi kopioida muuttamatta sen hieman koodi päälle.

Voit käyttää yhdistelmän `CTAS` ja implisiittinen liity, voit korvata koodi:

```sql
-- Create an interim table
CREATE TABLE CTAS_acs
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT  ISNULL(CAST([EnglishProductCategoryName] AS NVARCHAR(50)),0)    AS [EnglishProductCategoryName]
,       ISNULL(CAST([CalendarYear] AS SMALLINT),0)                      AS [CalendarYear]
,       ISNULL(CAST(SUM([SalesAmount]) AS MONEY),0)                     AS [TotalSalesAmount]
FROM    [dbo].[FactInternetSales]       AS s
JOIN    [dbo].[DimDate]                 AS d    ON s.[OrderDateKey]             = d.[DateKey]
JOIN    [dbo].[DimProduct]              AS p    ON s.[ProductKey]               = p.[ProductKey]
JOIN    [dbo].[DimProductSubCategory]   AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
JOIN    [dbo].[DimProductCategory]      AS c    ON u.[ProductCategoryKey]       = c.[ProductCategoryKey]
WHERE   [CalendarYear] = 2004
GROUP BY
        [EnglishProductCategoryName]
,       [CalendarYear]
;

-- Use an implicit join to perform the update
UPDATE  AnnualCategorySales
SET     AnnualCategorySales.TotalSalesAmount = CTAS_ACS.TotalSalesAmount
FROM    CTAS_acs
WHERE   CTAS_acs.[EnglishProductCategoryName] = AnnualCategorySales.[EnglishProductCategoryName]
AND     CTAS_acs.[CalendarYear]               = AnnualCategorySales.[CalendarYear]
;

--Drop the interim table
DROP TABLE CTAS_acs
;
```

## <a name="ansi-join-replacement-for-delete-statements"></a>ANSI liity korvaa poistaminen lauseet
Joskus paras tapa tietojen poistamista varten on käytettävä `CTAS`. Sen sijaan, että poistamisen yksinkertaisesti Valitse tiedot, jotka haluat säilyttää. Tämä varsinkin varten `DELETE` raportit, joissa käytetään liittävä syntaksi, koska SQL Data Warehouse ei tue ANSI liitokset ansi `FROM` lauseke `DELETE` lause.

Esimerkki muunnettu DELETE-lauseessa on käytettävissä alla:

```sql
CREATE TABLE dbo.DimProduct_upsert
WITH
(   Distribution=HASH(ProductKey)
,   CLUSTERED INDEX (ProductKey)
)
AS -- Select Data you wish to keep
SELECT     p.ProductKey
,          p.EnglishProductName
,          p.Color
FROM       dbo.DimProduct p
RIGHT JOIN dbo.stg_DimProduct s
ON         p.ProductKey = s.ProductKey
;

RENAME OBJECT dbo.DimProduct        TO DimProduct_old;
RENAME OBJECT dbo.DimProduct_upsert TO DimProduct;
```

## <a name="replace-merge-statements"></a>Korvaa yhdistämisen lauseet
Yhdistä lauseet voidaan korvata, vähintään-osassa, käyttämällä `CTAS`. Voit yhdistää `INSERT` ja `UPDATE` siirtäminen yhden lauseen. Tietueet on suljettava käytöstä toisessa asiakirjassa.

Esimerkki `UPSERT` on käytettävissä alla:

```sql
CREATE TABLE dbo.[DimProduct_upsert]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED INDEX ([ProductKey])
)
AS
-- New rows and new versions of rows
SELECT      s.[ProductKey]
,           s.[EnglishProductName]
,           s.[Color]
FROM      dbo.[stg_DimProduct] AS s
UNION ALL  
-- Keep rows that are not being touched
SELECT      p.[ProductKey]
,           p.[EnglishProductName]
,           p.[Color]
FROM      dbo.[DimProduct] AS p
WHERE NOT EXISTS
(   SELECT  *
    FROM    [dbo].[stg_DimProduct] s
    WHERE   s.[ProductKey] = p.[ProductKey]
)
;

RENAME OBJECT dbo.[DimProduct]          TO [DimProduct_old];
RENAME OBJECT dbo.[DimpProduct_upsert]  TO [DimProduct];

```

## <a name="ctas-recommendation-explicitly-state-data-type-and-nullability-of-output"></a>CTAS suositus: Postitoimipaikka erikseen tietotyyppi ja Null-arvoisuutta tulosteen

Koodin siirrettäessä voi olla yli tällaista coding kuvion suorittaa:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE result
(result DECIMAL(7,2) NOT NULL
)
WITH (DISTRIBUTION = ROUND_ROBIN)

INSERT INTO result
SELECT @d*@f
;
```

Ehkä vaistomaisesti Mieti siirtäminen CTAS koodi ja sinun on oikein. On kuitenkin piilotetut ongelman aiheuttavat.

Saman tuloksen ei tuota seuraava koodi:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455
;

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT @d*@f as result
;
```

Huomaa, että sarakkeen "tulos" suorittaa eteenpäin tietojen tyypin ja Null-arvoisuutta lausekkeen arvot. Tämä voi aiheuttaa muotoiltu varianssit arvot, jos et ole varovainen.

Kokeile esimerkkinä seuraavasti:

```sql
SELECT result,result*@d
from result
;

SELECT result,result*@d
from ctas_r
;
```

Tallennettu tulos arvo ei ole. Pysyvän tulos-sarakkeen arvo käytetään muiden lausekkeiden virhe tulee vieläkin merkittäviä.

![][1]

Tämä on erityisen tärkeitä tietoja-siirtojen. Vaikka toinen kysely on arguably tarkempi on ongelma. Tietoja olisi eri verrattuna lähdejärjestelmän ja, joka johtaa eheys siirtämistä koskevia kysymyksiä. Tämä on jokin näistä harvinaisissa tapauksia, joissa "Virhe" vastaus on todella oikeaa tiliä!

Näemme, että tämä tilanne kahden tuloksen välinen syynä on implisiittisesti tyyppi liittyvät alaspäin. Ensimmäinen taulukko määrittää sarakkeen määritys. Implisiittinen tyyppi-muunnos ilmenee, kun rivi on lisätty. Toinen esimerkki ei ole implisiittinen muuntamisen, kun lauseke määrittää sarakkeen tietotyypin. Huomaa myös, että toinen Esimerkki sarakkeen määritetty NULLable sarakkeena ensimmäisen esimerkissä se on ei. Kun taulukko on luotu esimerkiksi ensimmäisen sarakkeen Null-arvoisuutta määritettiin erikseen. Toinen esimerkki se vain jätettiin lauseke ja oletusarvoisesti tämä johtaa NULL-määritys.  

Näiden ongelmien ratkaisemiseen on määritettävä Null-arvoisuutta- ja tietotyyppien muuntamisesta `SELECT` osa `CTAS` lause. Et voi määrittää nämä ominaisuudet Luo taulukko-osaan.

Seuraavassa esimerkissä kerrotaan, miten voit korjata koodi:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT ISNULL(CAST(@d*@f AS DECIMAL(7,2)),0) as result
```

Ota seuraavat seikat huomioon:
- CAST tai Muunna voitu on käytetty
- ISNULL käytetään Null-arvoisuutta ei yhdistetyn
- ISNULL on uloimmat-funktio
- ISNULL toinen osa on vakio eli 0

> [AZURE.NOTE] Null-arvoisuutta on oikein asetettu on käyttää `ISNULL` eikä `COALESCE`. `COALESCE`ei ole deterministic-funktio ja siten lausekkeen tulos on aina näytettäviä. `ISNULL`ei ole samanlainen. On deterministic. Tämän vuoksi kun toinen osa `ISNULL` -funktio on vakio tai literaalimerkit sitten tuloksena oleva arvo on ei NULL.

Vihje Tämä ei ole vain hyötyä varmistamisesta laskelmien eheys. On myös tärkeää taulukon osion siirtymistä. Kuvitellaan, sinulla on tässä taulukossa, että kertoma on määritelty:

```sql
CREATE TABLE [dbo].[Sales]
(
    [date]      INT     NOT NULL
,   [product]   INT     NOT NULL
,   [store]     INT     NOT NULL
,   [quantity]  INT     NOT NULL
,   [price]     MONEY   NOT NULL
,   [amount]    MONEY   NOT NULL
)
WITH
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

Arvo-kenttä on kuitenkin laskettu lauseke, joka ei kuulu lähdetietoihin.

Osioitu tietojoukon luominen haluat ehkä seuraavasti:

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   [quantity]*[price]  AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create')
;
```

Kysely suoritetaan täysin kunnossa. Ongelma on, kun yrität suorittaa osio-valitsin. Taulukon määrityksiä ei vastaa. Jotta taulukon määritelmiä vastaa CTAS on muokattava.

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   ISNULL(CAST([quantity]*[price] AS MONEY),0) AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create');
```

Näet sen vuoksi tyyppi yhdenmukaisuutta ja ylläpito Null-arvoisuutta ominaisuuksista CTAS on hyvä suunnitteluryhmät paras käytäntö. Sen avulla voidaan ylläpitää laskelmien eheys ja myös varmistaa, että osion vaihtaminen on mahdollista.

Tutustu MSDN lisätietoja [CTAS][]avulla. Se on yksi Azure SQL-tietovarasto tärkeimmät lauseet. Varmista, että sen perusteellisesti ymmärtämistä.

## <a name="next-steps"></a>Seuraavat vaiheet
Artikkelissa kehitys Lisävihjeitä [kehittäminen yleiskatsaus][].

<!--Image references-->
[1]: media/sql-data-warehouse-develop-ctas/ctas-results.png

<!--Article references-->
[kehittäminen yleiskatsaus]: sql-data-warehouse-overview-develop.md
[Tilastotiedot]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[CTAS]: https://msdn.microsoft.com/library/mt204041.aspx

<!--Other Web references-->
