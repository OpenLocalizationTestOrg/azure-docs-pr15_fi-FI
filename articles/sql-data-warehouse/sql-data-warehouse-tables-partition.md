<properties
   pageTitle="SQL-tietovarasto taulukoiden jakaminen | Microsoft Azure"
   description="Taulukon jakaminen Azure SQL-tietovarasto käytön aloittaminen."
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
   ms.date="07/18/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="partitioning-tables-in-sql-data-warehouse"></a>SQL-tietovarasto taulukoiden jakaminen

> [AZURE.SELECTOR]
- [Yleiskatsaus][]
- [Tietotyypit][]
- [Jaa][]
- [Indeksi][]
- [Osion][]
- [Tilastotiedot][]
- [Tilapäinen][]

Jakaminen on tuettu SQL-tietovarasto-taulukon kaikentyyppisillä; esimerkiksi columnstore liitetty liitetty indeksi ja keon.  Jakaminen tuetaan myös kaikki jakauman tyypit mukaan lukien hash tai PYÖRISTÄ-funktiota Mikko jaetaan.  Jakaminen avulla voit jakaa tietojen jakaminen pienemmissä ryhmissä tietojen ja useimmissa tapauksissa on tehty date-sarake.

## <a name="benefits-of-partitioning"></a>Jakaminen edut

Jakaminen hyötyä tietojen ylläpito ja kyselyn suorituskykyä.  Onko se hyötyä molemmat tai vain yhden on riippuvainen siitä, miten tiedot on ladattu ja onko samassa sarakkeessa voidaan käyttää sekä tarkoituksiin, koska jakaminen voi tehdä vain yhden sarakkeen perusteella.

### <a name="benefits-to-loads"></a>Lataa edut

Tärkein etu SQL-tietovarasto jakaminen on tehostaa suorituskykyä tietojen käyttö, osion poistaminen ladataan, miten ja yhdistäminen.  Useimmissa tapauksissa tiedot osioidaan päivämäärä-sarakkeen, joka on sidottu tiiviisti järjestys, jossa tiedot on ladattu tietokantaan.  Yksi eniten hyötyä käyttämisestä osioiden pitämään tiedot sen tapahtuman kirjaaminen välttäminen.  Vain lisääminen, päivittäminen tai poistamasta tietoja voi eniten yksinkertaista lähestymistapa hieman haluat näyttää ja työmäärään, käyttämällä jakaminen kuormituksen prosessin aikana voi merkittävästi parantaa suorituskykyä.

Osion vaihtaminen voidaan poistaa tai korvaa taulukon osan nopeasti.  Esimerkiksi myynnin faktataulukon saattaa sisältää vain tietoja 36 edellisen kuukauden aikana.  Kuukausittain lopussa vanhimmasta kuukauden myyntitietojen poistetaan taulukosta.  Nämä tiedot on voitu poistaa poistaa tietoja vanhimmasta kuukauden delete-lauseen avulla.  Kuitenkin poistaminen suuria määriä tietoja rivi riviltä-sisältävä delete-lause voi kestää kauan sekä luoda suuria tapahtumat, jotka voi kestää kauan peruutus, jos järjestelmässä vikaan riskin.  Lisää paras tapa on pudota riittää, että vanhin osion tiedot.  Jos yksittäisten rivien poistaminen saattaa kestää tuntia, koko osion poistamista voi kestää sekuntia.

### <a name="benefits-to-queries"></a>Kyselyiden edut

Jakaminen, myös voidaan parantaa kyselyn suorituskykyä.  Jos kyselyn suodattimen osioitua saraketta, tämä rajoittaa vain ehdot täyttävän osioita, joka voi olla paljon pienemmän osan tiedoista välttäminen koko taulukon tarkistuksen.  Liitetty columnstore indeksit myötä predikaatin korjauksen suorituskyvyn etuja on vähemmän, mutta joskus voi olla hyötyä kyselyt.  Esimerkiksi jos myynnin faktataulukon on osioitu 36 kuukausiksi käyttämällä myyntipäivä kenttää ja valitse kyselyt suodatin myyntiä, jona ohittaa hakeminen osioita, jotka eivät vastaa suodatin.

## <a name="partition-sizing-guidance"></a>Osion koon muuttaminen ohjeita

Kun jakaminen voidaan parantaa suorituskykyä joissakin tilanteissa, **liian monta** osiot sisältävän taulukon luominen voi hidastaa järjestelmän toimintaa joissakin tilanteissa.  Näiden täyty erityisesti liitetty columnstore taulukoille.  Jakaminen on hyötyä, se on tärkeää ymmärtää, milloin kannattaa käyttää, jakaminen ja luoda osioiden määrää.  Ei ole vaikeaa nopea säännön siitä, kuinka monta osiot ovat liian monta, se on riippuvainen tiedot ja kuinka monta osioiden ovat lataaminen samanaikaisesti.  Mutta kuin Yleiset säännön of napin ajatella, lisäämällä 10s 100s osioita, ei 1000s.

**Liitetty columnstore** taulukkojen jakaminen luotaessa on tärkeä ottaa huomioon, kuinka monta riviä Power View kunkin osion.  Paras mahdollinen pakkaus ja liitetty columnstore taulukoiden suorituskyvyn tarvitaan vähintään 1 miljoonaa riviä kohden jakelu ja osio.  Ennen kuin osiot luodaan, SQL-tietovarasto jakaa jo kunkin taulukon 60 hajautettu tietokannat.  Taulukko lisätään jakaminen on luotu taustatietoja jaot lisäksi.  Tämän esimerkin avulla, jos myynnin faktataulukon sisälsi 36 kuukausittain osiot ja koska, SQL-tietovarasto on 60 jaot, valitse myynnin faktataulukon pitäisi näkyä 60 miljoonaa rivien kuukaudessa tai 2.1 miljardin rivien kun kuukauden täytetään.  Jos taulukko sisältää riviä kohden osion suositellut vähimmäismäärä merkittävästi vähemmän rivejä, kannattaa käyttää vähemmän osioiden Siirrä Suurenna kohti osion rivien määrä.  Myös on artikkelissa [indeksointi][indeksin] mukaan lukien kyselyt, jotka voidaan suorittaa SQL-tietovarasto arvioida klusterin columnstore indeksit laatuun.

## <a name="syntax-difference-from-sql-server"></a>SQL Server syntaksi ero

SQL-tietovarasto esitellään osioiden yksinkertaistettu määritys on hieman erilainen SQL Serveristä.  Osioinnin Funktiot ja mallit ei ole käytetty SQL-tietovarasto kuin SQL Server.  Sen sijaan sinun tarvitsee on tunnistaa osioitua saraketta ja reunan kohdeosoite.  Syntaksi jakaminen saattaa olla hieman erilainen kuin SQL Server-peruskäsitteet ovat samat.  SQL Server- ja SQL-tietovarasto tukevat yhdelle osion sarakkeelle taulukko, joka voi olla vaihtelivat osio.  Lisätietoja jakaminen on artikkelissa [osioitu taulukot ja indeksit][].

Seuraavassa esimerkissä SQL-tietovarasto osioitu [Luo taulukko][] -lauseen, osioiden FactInternetSales taulukon OrderDateKey saraketta:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

## <a name="migrating-partitioning-from-sql-server"></a>Siirtyminen SQL Serveristä jakaminen

Siirtää SQL Server-osion määritykset SQL-tietovarasto yksinkertaisesti:

- Poista SQL Server- [osion värimallin][].
- Luo taulukkoon lisättävän [partition-funktio][] -määritys.

Jos sähköpostit siirretään osioitua taulukon SQL Server-esiintymän alapuolella SQL voi auttaa menettelyn kunkin osion olevien rivien määrän.  Ota huomioon, että jos saman osioinnin rakeisuuden käytetään SQL-tietovarasto, kohti osion rivien määrä vähenee 60 kerrointa.  

```sql
-- Partition information for a SQL Server Database
SELECT      s.[name]                        AS      [schema_name]
,           t.[name]                        AS      [table_name]
,           i.[name]                        AS      [index_name]
,           p.[partition_number]            AS      [partition_number]
,           SUM(a.[used_pages]*8.0)         AS      [partition_size_kb]
,           SUM(a.[used_pages]*8.0)/1024    AS      [partition_size_mb]
,           SUM(a.[used_pages]*8.0)/1048576 AS      [partition_size_gb]
,           p.[rows]                        AS      [partition_row_count]
,           rv.[value]                      AS      [partition_boundary_value]
,           p.[data_compression_desc]       AS      [partition_compression_desc]
FROM        sys.schemas s
JOIN        sys.tables t                    ON      t.[schema_id]         = s.[schema_id]
JOIN        sys.partitions p                ON      p.[object_id]         = t.[object_id]
JOIN        sys.allocation_units a          ON      a.[container_id]      = p.[partition_id]
JOIN        sys.indexes i                   ON      i.[object_id]         = p.[object_id]
                                            AND     i.[index_id]          = p.[index_id]
JOIN        sys.data_spaces ds              ON      ds.[data_space_id]    = i.[data_space_id]
LEFT JOIN   sys.partition_schemes ps        ON      ps.[data_space_id]    = ds.[data_space_id]
LEFT JOIN   sys.partition_functions pf      ON      pf.[function_id]      = ps.[function_id]
LEFT JOIN   sys.partition_range_values rv   ON      rv.[function_id]      = pf.[function_id]
                                            AND     rv.[boundary_id]      = p.[partition_number]
WHERE       p.[index_id] <=1
GROUP BY    s.[name]
,           t.[name]
,           i.[name]
,           p.[partition_number]
,           p.[rows]
,           rv.[value]
,           p.[data_compression_desc]
;
```

## <a name="workload-management"></a>Kuormituksen hallinta

Yksi lopullinen kappaleen huomioon taulukon osion päätökseen ottaa huomioon on [kuormituksen hallinta][].  SQL-tietovarasto kuormituksen hallinta on ensisijaisesti muistia sekä samanaikainen hallinta.  SQL-tietovarasto kohdistettu kyselyn suorituksen aikana kunkin jakauman suurimman muistin on säännelty resurssin luokat.  Ihannetapauksessa osioiden voi muuttaa tekojen muut tekijät, kuten muistin tarpeisiin liitetty columnstore indeksien luomiseen.  Liitetty columnstore indeksit etu huomattavasti muistia jaettaessa.  Tämän vuoksi haluat varmistaa, että ilman ravintoa osion indeksi-muodosta ei muistin. Kyselyn käytettävissä olevan muistin määrän lisääminen saavutetaan siirtymällä oletusarvo-roolista smallrc, kuten largerc rooleihin.

Jakauman muistin jakamisesta tiedot ovat käytettävissä by säädin dynaaminen hallinta resurssinäkymät kyselyt. Reality muistin myöntäminen on pienempi kuin alla luvut. Tämä on kuitenkin ohjeet, joiden avulla voit data management toimille osioiden muutettaessa taso.  Yritä välttää lisäksi erittäin suuri resurssiluokkaa myöntämä muistin myöntäminen osioiden koon. Jos osioiden kasvaa lisäksi Tässä kuvassa suorittaa muistinkäyttöön, mikä puolestaan johtaa vähemmän optimaalisen pakkaamisen riskiä.

```sql
SELECT  rp.[name]                               AS [pool_name]
,       rp.[max_memory_kb]                      AS [max_memory_kb]
,       rp.[max_memory_kb]/1024                 AS [max_memory_mb]
,       rp.[max_memory_kb]/1048576              AS [mex_memory_gb]
,       rp.[max_memory_percent]                 AS [max_memory_percent]
,       wg.[name]                               AS [group_name]
,       wg.[importance]                         AS [group_importance]
,       wg.[request_max_memory_grant_percent]   AS [request_max_memory_grant_percent]
FROM    sys.dm_pdw_nodes_resource_governor_workload_groups  wg
JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools   rp ON wg.[pool_id] = rp.[pool_id]
WHERE   wg.[name] like 'SloDWGroup%'
AND     rp.[name]    = 'SloDWPool'
;
```

## <a name="partition-switching"></a>Osion vaihtaminen

SQL-tietovarasto tukee osion jakaminen, yhdistäminen ja siirtyminen. Nämä funktiot on excuted käyttämällä [ALTER TABLE][] -lause.

Siirtyä osioiden kahden taulukon välisen sinun on varmistettava, että osiot tasata niiden vastaaviin rajat ja taulukon määritelmiä vastaa. Tarkistusrajoitukset eivät ole käytettävissä arvoalue taulukossa ottamaan lähdetaulukon on oltava sama osion rajat, kuin kohdetaulukkoon. Jos tämä ei ole, osion Vaihda epäonnistuu, kun osion metatiedot ei synkronoida.

### <a name="how-to-split-a-partition-that-contains-data"></a>Voit jakaa tietoja sisältävän osion

Voit jakaa osio, joka sisältää jo tietoja tehokkaasti tapa on `CTAS` lause. Jos osioitua taulukko on liitetty columnstore sitten taulukko-osio on oltava tyhjä ennen kuin sitä voidaan jakaa.

Alla on esimerkki osioitua columnstore taulukko, joka sisältää kunkin osion yhden rivin:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
        [ProductKey]            int          NOT NULL
    ,   [OrderDateKey]          int          NOT NULL
    ,   [CustomerKey]           int          NOT NULL
    ,   [PromotionKey]          int          NOT NULL
    ,   [SalesOrderNumber]      nvarchar(20) NOT NULL
    ,   [OrderQuantity]         smallint     NOT NULL
    ,   [UnitPrice]             money        NOT NULL
    ,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101
                    )
                )
)
;

INSERT INTO dbo.FactInternetSales
VALUES (1,19990101,1,1,1,1,1,1);
INSERT INTO dbo.FactInternetSales
VALUES (1,20000101,1,1,1,1,1,1);


CREATE STATISTICS Stat_dbo_FactInternetSales_OrderDateKey ON dbo.FactInternetSales(OrderDateKey);
```

> [AZURE.NOTE] Luomalla tilasto-objektin varmistamme kyseisen taulukon metatietoja on tarkempi. Jos olemme pois luominen tilastotiedot, SQL-tietovarasto käyttävät oletusarvoja. Lisätietoja tilastoista Tarkista [tilastotiedot][].

Emme voi tehdä kyselyjä rivien määrä käytettäessä `sys.partitions` luettelon näkymän:

```sql
SELECT  QUOTENAME(s.[name])+'.'+QUOTENAME(t.[name]) as Table_name
,       i.[name] as Index_name
,       p.partition_number as Partition_nmbr
,       p.[rows] as Row_count
,       p.[data_compression_desc] as Data_Compression_desc
FROM    sys.partitions p
JOIN    sys.tables     t    ON    p.[object_id]   = t.[object_id]
JOIN    sys.schemas    s    ON    t.[schema_id]   = s.[schema_id]
JOIN    sys.indexes    i    ON    p.[object_id]   = i.[object_Id]
                            AND   p.[index_Id]    = i.[index_Id]
WHERE t.[name] = 'FactInternetSales'
;
```

Jos Yritämme Jaa tämä taulukko, emme tulee seuraava virhesanoma:

```sql
ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Virhesanoma 35346, tason 15 tila 1, muuta OSION lauseen rivin 44 Jaa lauseen epäonnistui, koska osio ei ole tyhjä.  Vain tyhjään osioita voidaan jakaa kun columnstore indeksi on olemassa taulukon. Harkitse columnstore hakemiston todistuksen muuta OSIOTA ja valitse columnstore indeksi muodostetaan uudelleen, muuta OSION jälkeen ennen poistamista käytöstä.

On kuitenkin käyttää `CTAS` pitoon tietojamme uuden taulukon luominen.

```sql
CREATE TABLE dbo.FactInternetSales_20000101
    WITH    (   DISTRIBUTION = HASH(ProductKey)
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101
                                )
                            )
            )
AS
SELECT *
FROM    FactInternetSales
WHERE   1=2
;
```

Kun osio rajat tasataan valitsin on sallittu. Tämä jättää lähdetaulukon tyhjän osion, jotka on jaettu myöhemmin.

```sql
ALTER TABLE FactInternetSales SWITCH PARTITION 2 TO  FactInternetSales_20000101 PARTITION 2;

ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Kaikki haluamasi hahmottaa on uusi osio rajat käyttämällä Microsoftin tietojen tasaaminen `CTAS` ja päätaulukko takaisin tietojamme valitsin

```sql
CREATE TABLE [dbo].[FactInternetSales_20000101_20010101]
    WITH    (   DISTRIBUTION = HASH([ProductKey])
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101,20010101
                                )
                            )
            )
AS
SELECT  *
FROM    [dbo].[FactInternetSales_20000101]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

ALTER TABLE dbo.FactInternetSales_20000101_20010101 SWITCH PARTITION 2 TO dbo.FactInternetSales PARTITION 2;
```

Kun olet ratkaissut tietojen siirto on hyvä Päivitä, jotta ne täsmällisemmin uusi jakautumisen niiden vastaaviin osioiden tiedot kohdetaulukkoon tilasto:

```sql
UPDATE STATISTICS [dbo].[FactInternetSales];
```

### <a name="table-partitioning-source-control"></a>Taulukon ja tietolähteen ohjausobjektin jakaminen

Sinun kannattaa harkita seuraavia lähestymistapa taulukon-määritys- **ruoste** välttämiseksi järjestelmästä Ohjausobjektin lähde:

1. Taulukon luominen osioitua taulukkona mutta osion arvoja

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    ()
                )
)
;
```

2. `SPLIT`taulukon käyttöönoton yhteydessä:

```sql
-- Create a table containing the partition boundaries

CREATE TABLE #partitions
WITH
(
    LOCATION = USER_DB
,   DISTRIBUTION = HASH(ptn_no)
)
AS
SELECT  ptn_no
,       ROW_NUMBER() OVER (ORDER BY (ptn_no)) as seq_no
FROM    (
        SELECT CAST(20000101 AS INT) ptn_no
        UNION ALL
        SELECT CAST(20010101 AS INT)
        UNION ALL
        SELECT CAST(20020101 AS INT)
        UNION ALL
        SELECT CAST(20030101 AS INT)
        UNION ALL
        SELECT CAST(20040101 AS INT)
        ) a
;

-- Iterate over the partition boundaries and split the table

DECLARE @c INT = (SELECT COUNT(*) FROM #partitions)
,       @i INT = 1                                 --iterator for while loop
,       @q NVARCHAR(4000)                          --query
,       @p NVARCHAR(20)     = N''                  --partition_number
,       @s NVARCHAR(128)    = N'dbo'               --schema
,       @t NVARCHAR(128)    = N'FactInternetSales' --table
;

WHILE @i <= @c
BEGIN
    SET @p = (SELECT ptn_no FROM #partitions WHERE seq_no = @i);
    SET @q = (SELECT N'ALTER TABLE '+@s+N'.'+@t+N' SPLIT RANGE ('+@p+N');');

    -- PRINT @q;
    EXECUTE sp_executesql @q;

    SET @i+=1;
END

-- Code clean-up

DROP TABLE #partitions;
```

Tämän menetelmän kanssa ja tietolähteen ohjausobjektin koodi pysyy staattinen ja osioinnin raja-arvot voivat olla dynaaminen; kehittyville varastoon ajan kuluessa.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja on artikkeleissa [Taulukon yleistä][Yleiskatsaus], [Taulukon tietojen tietotyyppejä][Tietotyypit], [jakaminen taulukon][välit], [indeksoinnin taulukon][indeksin], [Ylläpito taulukon tilastotiedot][tilastoja] ja [Väliaikaisten taulukoiden][tilapäinen].  Lisätietoja parhaista käytännöistä on [SQL Data Warehouse parhaita käytäntöjä][].

<!--Image references-->

<!--Article references-->
[Yleiskatsaus]: ./sql-data-warehouse-tables-overview.md
[Tietotyypit]: ./sql-data-warehouse-tables-data-types.md
[Jaa]: ./sql-data-warehouse-tables-distribute.md
[Indeksi]: ./sql-data-warehouse-tables-index.md
[Osion]: ./sql-data-warehouse-tables-partition.md
[Tilastotiedot]: ./sql-data-warehouse-tables-statistics.md
[Tilapäinen]: ./sql-data-warehouse-tables-temporary.md
[kuormituksen hallinta]: ./sql-data-warehouse-develop-concurrency.md
[SQL Data Warehouse parhaat käytännöt]: ./sql-data-warehouse-best-practices.md

<!-- MSDN Articles -->
[Osioitu taulukot ja indeksit]: https://msdn.microsoft.com/library/ms190787.aspx
[MUUTA TAULUKKO]: https://msdn.microsoft.com/en-us/library/ms190273.aspx
[TAULUKON LUOMINEN]: https://msdn.microsoft.com/library/mt203953.aspx
[Partition-funktio]: https://msdn.microsoft.com/library/ms187802.aspx
[osion malli]: https://msdn.microsoft.com/library/ms179854.aspx


<!-- Other web references -->
