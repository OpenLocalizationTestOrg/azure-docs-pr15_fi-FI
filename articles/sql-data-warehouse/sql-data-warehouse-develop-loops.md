<properties
   pageTitle="SQL-tietovarasto silmukoita | Microsoft Azure"
   description="Transact-SQL-silmukoita ja korvaaminen kohdistimet Azure SQL-tietovarasto, ratkaisujen kehittämiseen liittyviä vinkkejä."
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

# <a name="loops-in-sql-data-warehouse"></a>SQL-tietovarasto silmukat
SQL-tietovarasto tukee [aikana][] tapahtuu suoritetaan toistuvasti lauseen lohkot. Tämä säilyvät, kun määritetyt ehdot ovat tosia, tai kunnes koodin lopettaa erityisesti silmukan avulla `BREAK` avainsana. Silmukat ovat erityisen hyödyllisiä korvaaminen kohdistimet määritetty SQL-koodi. Lähettäjä-lähes kaikki kohdistimet, joka on kirjoitettu SQL-koodi on nopea eteenpäin, lue vain eri. Tästä syystä [samalla, kun] silmukoita ovat hyvä vaihtoehto, jos tiedä tarvitse korvata jonkin.

## <a name="leveraging-loops-and-replacing-cursors-in-sql-data-warehouse"></a>Hyödyntäminen silmukoita ja SQL-tietovarasto kohdistimet korvaaminen
Kuitenkin ennen Sukeltava muistissaan ensin joita kannattaa miettiä seuraavia kysymyksiä: "Voitu tämä kohdistin uudelleen kirjoittaa käyttämään perustuvan joukon toimintoja?". Monissa tapauksissa vastaus on Kyllä ja on usein paras tapa. Määritä mukaan-toimintoa usein suorittaa huomattavasti nopeammin iteratiivinen, riveittäin toimintatavan.

Kelaa eteenpäin vain luku-kohdistimet voit helposti korvaa looping rakennetta. Alla on esimerkki. Tämä esimerkki päivittää jokaisen taulukon tietokannan tilastotiedot. Merkkijonoavaimen päälle silmukan taulukot emme pysty suorittamaan jokaisen komennon järjestyksessä.

Luo väliaikainen taulukko, joka sisältää tunnistetaan yksittäisten lauseiden yksilöllinen rivinumeron:

```
CREATE TABLE #tbl
WITH
( DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS Sequence
,       [name]
,       'UPDATE STATISTICS '+QUOTENAME([name]) AS sql_code
FROM    sys.tables
;
```

Se alusta muuttujat silmukan suorittamiseen:

```
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

Nyt silmukka päälle lauseet suoritetaan yksi kerrallaan:

```
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

Pudota lopuksi väliaikainen ensimmäisessä vaiheessa luotu taulukko

```
DROP TABLE #tbl;
```


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>Seuraavat vaiheet
Artikkelissa kehitys Lisävihjeitä [kehittäminen yleiskatsaus][].

<!--Image references-->

<!--Article references-->
[kehittäminen yleiskatsaus]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[AIKAA]: https://msdn.microsoft.com/library/ms178642.aspx


<!--Other Web references-->
