<properties
   pageTitle="Yleiskatsaus SQL-tietovarasto taulukoiden | Microsoft Azure"
   description="SQL Azure varaston arvotaulukot käytön aloittaminen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/04/2016"
   ms.author="sonyama;barbkess;jrj"/>

# <a name="overview-of-tables-in-sql-data-warehouse"></a>Yleiskatsaus SQL-tietovarasto taulukot

> [AZURE.SELECTOR]
- [Yleiskatsaus][]
- [Tietotyypit][]
- [Jaa][]
- [Indeksi][]
- [Osion][]
- [Tilastotiedot][]
- [Tilapäinen][]

Taulukkojen luontityökalut SQL-tietovarasto käytön aloittaminen on helppoa.  Perustiedot [CREATE TABLE][] -syntaksin seuraa yhteistä syntaksia todennäköisesti jo tuttuja käyttäminen muut tietokannat.  Luo taulukko, riittää, että haluat taulukon nimeä, nimeä sarakkeet ja määritä kunkin sarakkeen tietotyypin määrittäminen.  Jos olet luoda taulukoita muiden tietokannoissa, tämä pitäisi näyttää varmasti tutulta sinulle.

```sql  
CREATE TABLE Customers (FirstName VARCHAR(25), LastName VARCHAR(25))
 ``` 

Edellä olevassa esimerkissä Luo ja kaksi saraketta, Etunimi ja Sukunimi Asiakkaat-taulukon.  Kunkin sarakkeen tietotyyppi on VARCHAR(25), joka rajoittaa 25 merkkiä tiedot on määritetty.  Seuraavia keskeisiä määritteiden taulukon sekä muiden ovat useimmiten sama kuin muut tietokannat.  Tietotyypit on määritetty kunkin sarakkeen ja tärkeimpiä tietojen eheyttä.  Indeksit voidaan lisätä vähentämällä i/o suorituskyvyn parantamiseksi.  Jakaminen voidaan lisätä suorituskyvyssä, kun haluat muokata tietoja.

[Nimeäminen uudelleen] [ RENAME] SQL-tietovarasto taulukko näyttää tältä:

```sql  
RENAME OBJECT Customer TO CustomerOrig; 
 ```

## <a name="distributed-tables"></a>Hajautettu taulukot

Hajautettu järjestelmien, kuten SQL-tietovarasto käyttöön uuden ratkaisevan määritteen on **jakauman sarake**.  Jakauman sarake on erittäin paljon mitä se kuulostaa kuten.  On sarake, joka määrittää, miten voit jakaa tai jakamalla tiedot taustalla.  Kun luot taulukon määrittämättä jakauman sarakkeen, taulukon jaetaan automaattisesti käyttämällä **PYÖRISTÄ-funktiota Mikko**.  PYÖRISTÄ-funktiota Mikko taulukoita voi joissakin tilanteissa riittävästi, jakauman sarakkeiden määrittäminen huomattavasti pienentää tietojen siirto kyselyiden, joten optimoinnista aikana.  Katso [jakaminen taulukon][välit] lisätietoja jakauman sarakkeen valitseminen.

## <a name="indexing-and-partitioning-tables"></a>Indeksoinnin ja taulukoiden jakaminen

Muuttuvat kehittyneempiä käyttäen SQL-tietovarasto ja haluat optimoida suorituskyvyn, haluat lisätietoja taulukon rakennenäkymä.  Lisätietoja on artikkeleissa [Taulukon tietotyyppien][Tietotyyppejä], [jakaminen taulukon][välit], [indeksointi taulukon][indeksi] ja [Taulukon jakaminen]-[osio].

## <a name="table-statistics"></a>Taulukon tilastotiedot

Tilastot on hyvin tärkeä pääset parhaan suorituskyvyn SQL-tietovarasto ulos.  Koska SQL Data Warehouse ei ole vielä luo automaattisesti ja päivittää tilastotiedot, esimerkiksi siirryt on sitten toiminta Azure SQL-tietokantaan, tämän artikkelin [tilastoista][] lukeminen voi olla jokin tärkeimmät artikkelit luet varmistaa parhaan suorituskyvyn noutaminen kyselyjen.

## <a name="temporary-tables"></a>Väliaikaiset taulukot

Tilapäinen taulukot ovat joka vain olemassa toistaminen käyttäjätunnuksellasi ja ei voi nähdä muiden käyttäjien.  Väliaikaisia taulukoita voi estää muita näkemästä tilapäinen tulokset ja vähentää myös uudelleenjärjestäminen edellyttää erinomainen tapa.  Väliaikaisten taulukoiden hyödyntää myös paikallisen säilön, koska ne tarjoavat nopeampaa suorituskyvyn joitakin toimintoja.  Artikkeleissa [Väliaikaisen taulukossa][tilapäinen] lisätietoja väliaikaisia taulukoita.

## <a name="external-tables"></a>Ulkoiset taulukot

Ulkoiset taulukot, eli Polybase taulukot ovat taulukot, jotka voidaan suorittaa kysely SQL-tietovarasto, mutta piste-SQL-tietovarasto ulkoiset tiedot.  Voit esimerkiksi luoda ulkoisen taulukon mitkä pisteet Azure-Blob-säiliö olevia tiedostoja.  Saat lisätietoja siitä, miten voit luoda ja ulkoisen taulukon kyselyn [kuormituksen tietojen Polybase][].  

## <a name="unsupported-table-features"></a>Taulukkotoimintoja ei tueta

Kun SQL-tietovarasto sisältää monia muita tietokantoja tarjoamia saman taulukon ominaisuuksia, on joitakin ominaisuuksia, jossa ei vielä tueta.  Alla on luettelo joistain taulukkotoimintoja, jossa ei vielä tueta.

| Ei-tuettuja ominaisuuksia |
| --- |
|[Identity-ominaisuutta][] (katso [Määrittäminen korvaavan avaimen vaihtoehtoinen][])|
|Perusavaimen, viiteavaimien yksilölliset arvot ja tarkista [Taulukon rajoitukset][]|
|[Yksilöivät indeksit][]|
|[Lasketut sarakkeet][]|
|[Lyhyet sarakkeet][]|
|[Käyttäjän määrittämät tyypit][]|
|[Järjestys][]|
|[Käynnistimien][]|
|[Indeksoidut näkymät][]|
|[Synonyymit][]|

## <a name="table-size-queries"></a>Taulukon koon kyselyistä

Yksi helppo tunnistaa tilaa ja kulutettu kussakin 60 jaot taulukon rivit on käytettävä [DBCC PDW_SHOWSPACEUSED][].

```sql
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Kuitenkin DBCC komennoilla voit olla aivan rajoittaminen.  Dynaaminen hallinta näkymät (DMVs) ansiosta voit nähdä paljon yksityiskohtia sekä antavat paljon suurempi päättää kyselyn tuloksiin.  Aloita luomalla tämä näkymä, jossa useiden tämän ja muiden Microsoftin esimerkeissä viitataan.

```sql
CREATE VIEW dbo.vTableSizes
AS
WITH base
AS
(
SELECT 
 GETDATE()                                                             AS  [execution_time]
, DB_NAME()                                                            AS  [database_name]
, s.name                                                               AS  [schema_name]
, t.name                                                               AS  [table_name]
, QUOTENAME(s.name)+'.'+QUOTENAME(t.name)                              AS  [two_part_name]
, nt.[name]                                                            AS  [node_table_name]
, ROW_NUMBER() OVER(PARTITION BY nt.[name] ORDER BY (SELECT NULL))     AS  [node_table_name_seq]
, tp.[distribution_policy_desc]                                        AS  [distribution_policy_name]
, c.[name]                                                             AS  [distribution_column]
, nt.[distribution_id]                                                 AS  [distribution_id]
, i.[type]                                                             AS  [index_type]
, i.[type_desc]                                                        AS  [index_type_desc]
, nt.[pdw_node_id]                                                     AS  [pdw_node_id]
, pn.[type]                                                            AS  [pdw_node_type]
, pn.[name]                                                            AS  [pdw_node_name]
, di.name                                                              AS  [dist_name]
, di.position                                                          AS  [dist_position]
, nps.[partition_number]                                               AS  [partition_nmbr]
, nps.[reserved_page_count]                                            AS  [reserved_space_page_count]
, nps.[reserved_page_count] - nps.[used_page_count]                    AS  [unused_space_page_count]
, nps.[in_row_data_page_count] 
    + nps.[row_overflow_used_page_count] 
    + nps.[lob_used_page_count]                                        AS  [data_space_page_count]
, nps.[reserved_page_count] 
 - (nps.[reserved_page_count] - nps.[used_page_count]) 
 - ([in_row_data_page_count] 
         + [row_overflow_used_page_count]+[lob_used_page_count])       AS  [index_space_page_count]
, nps.[row_count]                                                      AS  [row_count]
from 
    sys.schemas s
INNER JOIN sys.tables t
    ON s.[schema_id] = t.[schema_id]
INNER JOIN sys.indexes i
    ON  t.[object_id] = i.[object_id]
    AND i.[index_id] <= 1
INNER JOIN sys.pdw_table_distribution_properties tp
    ON t.[object_id] = tp.[object_id]
INNER JOIN sys.pdw_table_mappings tm
    ON t.[object_id] = tm.[object_id]
INNER JOIN sys.pdw_nodes_tables nt
    ON tm.[physical_name] = nt.[name]
INNER JOIN sys.dm_pdw_nodes pn
    ON  nt.[pdw_node_id] = pn.[pdw_node_id]
INNER JOIN sys.pdw_distributions di
    ON  nt.[distribution_id] = di.[distribution_id]
INNER JOIN sys.dm_pdw_nodes_db_partition_stats nps
    ON nt.[object_id] = nps.[object_id]
    AND nt.[pdw_node_id] = nps.[pdw_node_id]
    AND nt.[distribution_id] = nps.[distribution_id]
LEFT OUTER JOIN (select * from sys.pdw_column_distribution_properties where distribution_ordinal = 1) cdp
    ON t.[object_id] = cdp.[object_id]
LEFT OUTER JOIN sys.columns c
    ON cdp.[object_id] = c.[object_id]
    AND cdp.[column_id] = c.[column_id]
)
, size
AS
(
SELECT
   [execution_time]
,  [database_name]
,  [schema_name]
,  [table_name]
,  [two_part_name]
,  [node_table_name]
,  [node_table_name_seq]
,  [distribution_policy_name]
,  [distribution_column]
,  [distribution_id]
,  [index_type]
,  [index_type_desc]
,  [pdw_node_id]
,  [pdw_node_type]
,  [pdw_node_name]
,  [dist_name]
,  [dist_position]
,  [partition_nmbr]
,  [reserved_space_page_count]
,  [unused_space_page_count]
,  [data_space_page_count]
,  [index_space_page_count]
,  [row_count]
,  ([reserved_space_page_count] * 8.0)                                 AS [reserved_space_KB]
,  ([reserved_space_page_count] * 8.0)/1000                            AS [reserved_space_MB]
,  ([reserved_space_page_count] * 8.0)/1000000                         AS [reserved_space_GB]
,  ([reserved_space_page_count] * 8.0)/1000000000                      AS [reserved_space_TB]
,  ([unused_space_page_count]   * 8.0)                                 AS [unused_space_KB]
,  ([unused_space_page_count]   * 8.0)/1000                            AS [unused_space_MB]
,  ([unused_space_page_count]   * 8.0)/1000000                         AS [unused_space_GB]
,  ([unused_space_page_count]   * 8.0)/1000000000                      AS [unused_space_TB]
,  ([data_space_page_count]     * 8.0)                                 AS [data_space_KB]
,  ([data_space_page_count]     * 8.0)/1000                            AS [data_space_MB]
,  ([data_space_page_count]     * 8.0)/1000000                         AS [data_space_GB]
,  ([data_space_page_count]     * 8.0)/1000000000                      AS [data_space_TB]
,  ([index_space_page_count]  * 8.0)                                   AS [index_space_KB]
,  ([index_space_page_count]  * 8.0)/1000                              AS [index_space_MB]
,  ([index_space_page_count]  * 8.0)/1000000                           AS [index_space_GB]
,  ([index_space_page_count]  * 8.0)/1000000000                        AS [index_space_TB]
FROM base
)
SELECT * 
FROM size
;
```

### <a name="table-space-summary"></a>Taulukkotilaa yhteenveto

Tämä kysely palauttaa rivit ja tilaa-taulukko.  Se on hyvä kysely nähdäksesi, mitkä taulukot ovat suurin taulukoita ja ovatko ne PYÖRISTÄ-funktiota Mikko vai hajautettu hash.  Jaettu hash taulukoiden se näyttää myös jakauman-sarake.  Useimmissa tapauksissa suurin taulukoissa pitäisi olla mukana indeksi on liitetty columnstore hash.

```sql
SELECT 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,     distribution_column
,    index_type_desc
,    COUNT(distinct partition_nmbr) as nbr_partitions
,    SUM(row_count)                 as table_row_count
,    SUM(reserved_space_GB)         as table_reserved_space_GB
,    SUM(data_space_GB)             as table_data_space_GB
,    SUM(index_space_GB)            as table_index_space_GB
,    SUM(unused_space_GB)           as table_unused_space_GB
FROM 
    dbo.vTableSizes
GROUP BY 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,     distribution_column
,    index_type_desc
ORDER BY
    table_reserved_space_GB desc
;
```

### <a name="table-space-by-distribution-type"></a>Taulukkotilaa jakauman tyypin mukaan

```sql
SELECT 
     distribution_policy_name
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY distribution_policy_name
;
```

### <a name="table-space-by-index-type"></a>Taulukkotilaa indeksi tyypin mukaan

```sql
SELECT 
     index_type_desc
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY index_type_desc
;
```

### <a name="distribution-space-summary"></a>Jakauman tilaa yhteenveto

```sql
SELECT 
    distribution_id
,    SUM(row_count)                as total_node_distribution_row_count
,    SUM(reserved_space_MB)        as total_node_distribution_reserved_space_MB
,    SUM(data_space_MB)            as total_node_distribution_data_space_MB
,    SUM(index_space_MB)           as total_node_distribution_index_space_MB
,    SUM(unused_space_MB)          as total_node_distribution_unused_space_MB
FROM dbo.vTableSizes
GROUP BY     distribution_id
ORDER BY    distribution_id
;
```

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja on artikkeleissa [Taulukon tietojen tietotyyppejä][Tietotyypit], [jakaminen taulukon][välit], [indeksoinnin taulukon][indeksin], [Taulukon jakaminen]-[osio], [Ylläpito taulukon tilastotiedot][tilastoja] ja [Väliaikaisia taulukoita][tilapäinen].  Lisätietoja parhaista käytännöistä on [SQL Data Warehouse parhaita käytäntöjä][].

<!--Image references-->

<!--Article references-->
[Yleiskatsaus]: ./sql-data-warehouse-tables-overview.md
[Tietotyypit]: ./sql-data-warehouse-tables-data-types.md
[Jaa]: ./sql-data-warehouse-tables-distribute.md
[Indeksi]: ./sql-data-warehouse-tables-index.md
[Osion]: ./sql-data-warehouse-tables-partition.md
[Tilastotiedot]: ./sql-data-warehouse-tables-statistics.md
[Tilapäinen]: ./sql-data-warehouse-tables-temporary.md
[SQL Data Warehouse parhaat käytännöt]: ./sql-data-warehouse-best-practices.md
[Lataa tiedot ja Polybase]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md

<!--MSDN references-->
[TAULUKON LUOMINEN]: https://msdn.microsoft.com/library/mt203953.aspx
[RENAME]: https://msdn.microsoft.com/library/mt631611.aspx
[DBCC PDW_SHOWSPACEUSED]: https://msdn.microsoft.com/library/mt204028.aspx
[Identity-ominaisuutta]: https://msdn.microsoft.com/library/ms186775.aspx
[Määrittää korvaavan vaihtoehtoinen]: https://blogs.msdn.microsoft.com/sqlcat/2016/02/18/assigning-surrogate-key-to-dimension-tables-in-sql-dw-and-aps/
[Taulukon rajoitukset]: https://msdn.microsoft.com/library/ms188066.aspx
[Lasketut sarakkeet]: https://msdn.microsoft.com/library/ms186241.aspx
[Lyhyet sarakkeet]: https://msdn.microsoft.com/library/cc280604.aspx
[Käyttäjän määrittämät tyypit]: https://msdn.microsoft.com/library/ms131694.aspx
[Järjestys]: https://msdn.microsoft.com/library/ff878091.aspx
[Käynnistimien]: https://msdn.microsoft.com/library/ms189799.aspx
[Indeksoidut näkymät]: https://msdn.microsoft.com/library/ms191432.aspx
[Synonyymit]: https://msdn.microsoft.com/library/ms177544.aspx
[Yksilöivät indeksit]: https://msdn.microsoft.com/library/ms188783.aspx

<!--Other Web references-->
