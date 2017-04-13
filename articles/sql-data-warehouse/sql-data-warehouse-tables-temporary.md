<properties
   pageTitle="SQL-tietovarasto tilapäinen taulukoiden | Microsoft Azure"
   description="Tilapäinen taulukot SQL Azure-tietovarasto käytön aloittaminen."
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
   ms.date="06/29/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="temporary-tables-in-sql-data-warehouse"></a>SQL-tietovarasto väliaikaiset taulukot

> [AZURE.SELECTOR]
- [Yleiskatsaus][]
- [Tietotyypit][]
- [Jaa][]
- [Indeksi][]
- [Osion][]
- [Tilastotiedot][]
- [Tilapäinen][]

Tilapäinen taulukot ovat erittäin hyödyllinen, kun tietojenkäsittely - erityisesti jos keskitason tulokset ovat lyhytkestoisia muunnoksen aikana. SQL-tietovarasto väliaikaisten taulukoiden olemassa istunnon tasolla.  Ne näkyvät vain istuntoon, johon ne on luotu ja ovat poistetaan automaattisesti, kun kyseisen istunnon kirjautuu sisään.  Väliaikaisten taulukoiden tarjouksen suorituskyvyn etu, koska kenttien tulosten kirjoitetaan paikallisiin sen sijaan, että remote tallennustilan.  Väliaikaiset taulukot ovat Azure SQL-tietovarasto hieman erilainen kuin Azure SQL-tietokanta niitä voi käyttää mitä tahansa istunnosta, mukaan lukien sekä sisäiset ja ulkopuoliset tallennetun toimintosarjan sisällä.

Tässä artikkelissa on olennainen väliaikaisten taulukoiden käyttöohjeet ja korostaa periaatteiden istunnon tason väliaikaisia taulukoita. Tämän artikkelin tiedot käyttämällä auttavat modularize koodisi hyötykäyttöä ja helpottaa ylläpitoa koodin parantamiseksi.

## <a name="create-a-temporary-table"></a>Väliaikaisen taulukon luominen

Väliaikaisten taulukoiden luotuja etuliitteiden vain oman taulukkonimi, jossa `#`.  Esimerkki:

```sql
CREATE TABLE #stats_ddl
(
    [schema_name]       NVARCHAR(128) NOT NULL
,   [table_name]            NVARCHAR(128) NOT NULL
,   [stats_name]            NVARCHAR(128) NOT NULL
,   [stats_is_filtered]     BIT           NOT NULL
,   [seq_nmbr]              BIGINT        NOT NULL
,   [two_part_name]         NVARCHAR(260) NOT NULL
,   [three_part_name]       NVARCHAR(400) NOT NULL
)
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,   HEAP
)
```

Väliaikaisia taulukoita voi luoda myös `CTAS` täsmälleen saman menetelmän avulla:

```sql
CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,   HEAP
)
AS
(
SELECT
        sm.[name]                                                               AS [schema_name]
,       tb.[name]                                                               AS [table_name]
,       st.[name]                                                               AS [stats_name]
,       st.[has_filter]                                                         AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects         AS ob
JOIN    sys.stats           AS st   ON  ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc   ON  st.[stats_id]       = sc.[stats_id]
                                    AND st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co   ON  sc.[column_id]      = co.[column_id]
                                    AND sc.[object_id]      = co.[object_id]
JOIN    sys.tables          AS tb   ON  co.[object_id]      = tb.[object_id]
JOIN    sys.schemas         AS sm   ON  tb.[schema_id]      = sm.[schema_id]
WHERE   1=1
AND     st.[user_created]   = 1
GROUP BY
        sm.[name]
,       tb.[name]
,       st.[name]
,       st.[filter_definition]
,       st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
``` 

>[AZURE.NOTE] `CTAS`on hyvin tehokas komento ja on lisätty etuna on erittäin tehokkaasti sen käyttö tapahtuman log tilaa. 


## <a name="dropping-temporary-tables"></a>Pudottaminen väliaikaisia taulukoita

Uuden istunnon luomisen jälkeen ei ole väliaikaisten taulukoiden pitäisi olla.  Kuitenkin jos soitat samaan tallennetun toimintosarjan, joka luo tilapäisen saman niminen, jos haluat varmistaa, että oman `CREATE TABLE` lauseet onnistuvat yksinkertainen vanhat olemassa-valintaruutu ja `DROP` voidaan kuin seuraavassa esimerkissä:

```sql
IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END
```

Coding yhdenmukaisuuden, se on hyvä käyttää tätä mallia taulukot ja väliaikaisia taulukoita.  On myös suositeltavaa käyttää `DROP TABLE` Poista väliaikaiset taulukot, kun olet tehnyt heidän kanssaan koodisi.  Tallennettu toimintosarja-kehitys on aivan yhteinen Nähdäksesi avattavan komentoja koottu menetelmä, jolla varmistetaan objektit lopussa leikkauskohdat Siivotaan.

```sql
DROP TABLE #stats_ddl
```

## <a name="modularizing-code"></a>Modularizing koodi

Koska väliaikaisia taulukoita voi nähdä missä tahansa käyttäjän istuntoon, tämä voidaan hyödyntää auttaa modularize sovelluksen-koodin.  Esimerkiksi alla tallennetun toimintosarjan monipuolisesti suositellut käytännöt edellä luomiseen DDL, jossa voit päivittää kaikki tilastotiedot tietokannan tilasto nimen mukaan.

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_update_stats]
(   @update_type    tinyint -- 1 default 2 fullscan 3 sample 4 resample
    ,@sample_pct     tinyint
)
AS

IF @update_type NOT IN (1,2,3,4)
BEGIN;
    THROW 151000,'Invalid value for @update_type parameter. Valid range 1 (default), 2 (fullscan), 3 (sample) or 4 (resample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END

CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
)
AS
(
SELECT
        sm.[name]                                                               AS [schema_name]
,       tb.[name]                                                               AS [table_name]
,       st.[name]                                                               AS [stats_name]
,       st.[has_filter]                                                         AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects         AS ob
JOIN    sys.stats           AS st   ON  ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc   ON  st.[stats_id]       = sc.[stats_id]
                                    AND st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co   ON  sc.[column_id]      = co.[column_id]
                                    AND sc.[object_id]      = co.[object_id]
JOIN    sys.tables          AS tb   ON  co.[object_id]      = tb.[object_id]
JOIN    sys.schemas         AS sm   ON  tb.[schema_id]      = sm.[schema_id]
WHERE   1=1
AND     st.[user_created]   = 1
GROUP BY
        sm.[name]
,       tb.[name]
,       st.[name]
,       st.[filter_definition]
,       st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
GO
```

Tässä vaiheessa vain toiminto on tapahtunut on tallennettu toimintosarja joka yksinkertaisesti luominen luodaan väliaikaisen taulukossa #stats_ddl kanssa DDL-lauseita.  Tämä tallennettu toimintosarja pudota #stats_ddl, jos se on jo varmistamiseksi ei onnistu, jos toimivat istunnon useita kertoja.  Koska ei `DROP TABLE` tallennetun toimintosarjan lopussa, kun tallennettu toimintosarja on valmis, se jättää luodun taulukon niin, että se voidaan lukea tallennetun toimintosarjan ulkopuolella.  SQL-tietovarasto toisin kuin muut SQL Server-tietokannat on mahdollista käyttää väliaikaisen taulukon ulkopuolella, se on luotu kuvatulla tavalla.  SQL-tietovarasto väliaikaisia taulukoita voi käyttää **mitä tahansa** istunnon sisällä. Tämä voi aiheuttaa enemmän kuin moduulit ja helpommin hallittaviin koodi seuraavassa esimerkissä:

```sql
EXEC [dbo].[prc_sqldw_update_stats] @update_type = 1, @sample_pct = NULL;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''

WHILE @i <= @t
BEGIN
    SET @s=(SELECT update_stats_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

## <a name="temporary-table-limitations"></a>Väliaikaisen taulukossa rajoitukset

SQL-tietovarasto määrätä muutama rajoitus, kun väliaikaisia taulukoita.  Tällä hetkellä vain istunnon kohdistetuissa väliaikaisia taulukoita tuetaan.  Yleinen väliaikaiset taulukot eivät ole tuettuja.  Lisäksi näkymiä ei voi luoda väliaikaisia taulukoita.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja on artikkeleissa [Taulukon yleistä][Yleiskatsaus], [Taulukon tietojen tietotyyppejä][Tietotyyppejä], [jakaminen taulukon][välit], [indeksoinnin taulukon][indeksin], [Taulukon jakaminen]-[osio] ja [Ylläpito taulukon tilastotiedot][tilastotiedot].  Lisätietoja parhaista käytännöistä on [SQL Data Warehouse parhaita käytäntöjä][].

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

<!--MSDN references-->

<!--Other Web references-->
