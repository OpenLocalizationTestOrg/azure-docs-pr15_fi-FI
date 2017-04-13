<properties
   pageTitle="SQL-tietovarasto asetukset Ryhmittelyperuste | Microsoft Azure"
   description="Vihjeitä ryhmän Azure SQL-tietovarasto asetukset, ratkaisujen kehittämiseen."
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

# <a name="group-by-options-in-sql-data-warehouse"></a>Ryhmittelyperuste SQL-tietovarasto asetukset

[GROUP BY][] -lauseeseen käytetään kooste tietojen yhteenveto rivisarjaa. Siinä on myös on useita vaihtoehtoja, jotka ulottuvat on toimintoja, jotka on käyttänyt ympärille, kun ne ovat suoraan tue Azure SQL-tietovarasto.

Nämä asetukset ovat
- RYHMITTELE ja ROLLUP
- RYHMITTELY JOUKOT
- GROUP BY kuutiolla

## <a name="rollup-and-grouping-sets-options"></a>Rollup ja ryhmittely joukot-vaihtoehdot
Yksinkertaisin vaihtoehto on käyttää `UNION ALL` sijaan suorittamiseen koonnin allekirjoitat eksplisiittinen syntaksia. Tulos on täysin sama

Alla on esimerkki ryhmän lausekkeen avulla `ROLLUP` vaihtoehto:

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount)             AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t       ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY ROLLUP (
                        [SalesTerritoryCountry]
                ,       [SalesTerritoryRegion]
                )
;
```

ROLLUP käyttämällä Microsoft on pyydetty seuraavat koosteet:
- Maiden ja alueiden
- Maa
- Kokonaissumma

Voit korvata tämän sinun on käytettävä `UNION ALL`; koosteet määrittäminen pakollinen erikseen palauttaa saman tuloksen:

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
UNION ALL
SELECT [SalesTerritoryCountry]
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
UNION ALL
SELECT NULL
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey;
```

Ryhmittely joukkojen kaikki olemme on suoritettava on toteuttaa saman lainan mutta UNION ALL osien kooste tasoissa haluat nähdä vain luominen

## <a name="cube-options"></a>Kuutioasetukset
Ei luoda ryhmän mukaan kanssa DATAKUUTION UNION ALL menetelmän avulla. Ongelma ilmenee, että koodin nopeasti voi olla hankalaa ja suureksi. Pienentämään Näin voit käyttää tätä kehittyneempiä lähestymistapa.

Yllä olevassa esimerkissä käytetään.

Ensimmäiseksi on määritellä 'kuution", joka määrittää koosteen on tarkoitus luoda kaikki tasot. On tärkeää huomioi, RISTILIITOS johdetun kahdesta taulukosta. Tämä luo kaikki tasot, us. Loput koodi on todella olemassa muotoilu.

```sql
CREATE TABLE #Cube
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
AS
WITH GrpCube AS
(SELECT    CAST(ISNULL(Country,'NULL')+','+ISNULL(Region,'NULL') AS NVARCHAR(50)) as 'Cols'
,          CAST(ISNULL(Country+',','')+ISNULL(Region,'') AS NVARCHAR(50))  as 'GroupBy'
,          ROW_NUMBER() OVER (ORDER BY Country) as 'Seq'
FROM       ( SELECT 'SalesTerritoryCountry' as Country
             UNION ALL
             SELECT NULL
           ) c
CROSS JOIN ( SELECT 'SalesTerritoryRegion' as Region
             UNION ALL
             SELECT NULL
           ) r
)
SELECT Cols
,      CASE WHEN SUBSTRING(GroupBy,LEN(GroupBy),1) = ','
            THEN SUBSTRING(GroupBy,1,LEN(GroupBy)-1)
            ELSE GroupBy
       END AS GroupBy  --Remove Trailing Comma
,Seq
FROM GrpCube;
```

CTAS tulokset näkyvät alla:

![][1]

Voit määrittää kohdetaulukkoon väliaikaisen tulosten tallentaminen on toinen vaihe:

```sql
DECLARE
 @SQL NVARCHAR(4000)
,@Columns NVARCHAR(4000)
,@GroupBy NVARCHAR(4000)
,@i INT = 1
,@nbr INT = 0
;
CREATE TABLE #Results
(
 [SalesTerritoryCountry] NVARCHAR(50)
,[SalesTerritoryRegion]  NVARCHAR(50)
,[TotalSalesAmount]      MONEY
)
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
;
```

Kolmas vaihe on Microsoftin kuution sarakkeiden suorittamiseen koosteen ympäri. Kyselyn suorittaa kerran #Cube väliaikaisen taulukon kunkin rivin ja tulosten tallentaminen #Results temp-taulukossa

```sql
SET @nbr =(SELECT MAX(Seq) FROM #Cube);

WHILE @i<=@nbr
BEGIN
    SET @Columns = (SELECT Cols    FROM #Cube where seq = @i);
    SET @GroupBy = (SELECT GroupBy FROM #Cube where seq = @i);

    SET @SQL ='INSERT INTO #Results
              SELECT '+@Columns+'
              ,      SUM(SalesAmount) AS TotalSalesAmount
              FROM  dbo.factInternetSales s
              JOIN  dbo.DimSalesTerritory t  
              ON s.SalesTerritoryKey = t.SalesTerritoryKey
              '+CASE WHEN @GroupBy <>''
                     THEN 'GROUP BY '+@GroupBy ELSE '' END

    EXEC sp_executesql @SQL;
    SET @i +=1;
END
```

Olemme lopuksi palauttamaan tulokset lukemalla yksinkertaisesti #Results tilapäisestä taulukosta

```sql
SELECT *
FROM #Results
ORDER BY 1,2,3
;
```

Purkaa koodin osiin ja luomalla looping rakennetta koodin muuttuu entistä helpommin hallittaviin ja kannattava.


## <a name="next-steps"></a>Seuraavat vaiheet
Artikkelissa kehitys Lisävihjeitä [kehittäminen yleiskatsaus][].

<!--Image references-->
[1]: media/sql-data-warehouse-develop-group-by-options/sql-data-warehouse-develop-group-by-cube.png

<!--Article references-->
[kehittäminen yleiskatsaus]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[RYHMITTELYPERUSTE]: https://msdn.microsoft.com/library/ms177673.aspx


<!--Other Web references-->
