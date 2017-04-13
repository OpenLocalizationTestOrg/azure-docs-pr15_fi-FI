<properties
   pageTitle="Tietotyyppien SQL-tietovarasto taulukoiden | Microsoft Azure"
   description="Tietotyyppien Azure SQL-tietovarasto taulukoiden käytön aloittaminen."
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

# <a name="data-types-for-tables-in-sql-data-warehouse"></a>Taulukot SQL-tietovarasto tietotyypit

> [AZURE.SELECTOR]
- [Yleiskatsaus][]
- [Tietotyypit][]
- [Jaa][]
- [Indeksi][]
- [Osion][]
- [Tilastotiedot][]
- [Tilapäinen][]

SQL-tietovarasto tukee useimmin käytetyt tietotyypit.  Alla on luettelo SQL-tietovarasto tukemat tietotyypit.  Lisätietoja tietotyyppien tuesta on kohdassa [taulukon luominen][].

|**Tuetut tietotyypit**|||
|---|---|---|
[bigint][]|[desimaaliluku][]|[smallint][]|
[binaaritiedosto][]|[liukuluku][]|[Smallmoney][]|
[Bit][]|[kokonaisluku][]|[Sysname][]|
[merkki][]|[rahaa][]|[aika][]|
[päivämäärä][]|[nchar][]|[tinyint][]|
[Päivämäärä ja aika][]|[nvarchar][]|[Uniqueidentifier][]|
[datetime2][]|[Todellinen][]|[Varbinary][]|
[DateTimeOffset-arvoa][]|[smalldatetime][]|[varchar][]|


## <a name="data-type-best-practices"></a>Tietotyyppi parhaat käytännöt

 Sarakkeen tyyppi määritettäessä käyttämällä pienin tietotyyppi, joka tue tietojen parantaa kyselyn suorituskykyä. Tämä on erityisen tärkeää merkki- ja VARCHAR sarakkeet. Jos sarakkeen pisimmän arvo on 25 merkkiä, Määritä sarakkeen sitten VARCHAR(25). Vältä määrittäminen kaikki merkin sarakkeet suuri oletusarvon pituus. Määritä lisäksi sarakkeita kuin VARCHAR, kun, joka on kaikki haluamasi tarvita sen sijaan, että Käytä [NVARCHAR][].  Käytä NVARCHAR(4000) tai VARCHAR(8000), kun se on mahdollista NVARCHAR(MAX) tai VARCHAR(MAX) sijaan.

## <a name="polybase-limitation"></a>Polybase rajoitus

Jos käytät Polybase lataamaan taulukoiden, määrittää taulukoiden niin, että suurin mahdollinen rivin koon, pituus saraketta leveä enintään 32 767 tavua.  Kun määrität rivi, jonka pituus tietoja, joita voi ylittää leveyden ja ladata BCP sisältävien rivien, voit voi Polybase avulla voit ladata nämä tiedot.  Leveä rivien Polybase tuki lisätään pian.

## <a name="unsupported-data-types"></a>Ei-tuetut tietotyypit

Jos olet siirtymässä tietokannan toisessa SQL-ympäristössä, kuten Azure SQL-tietokantaan, kun siirrät, voit kohdata joitakin tietotyyppejä, jotka eivät ole tuettuja SQL-tietovarasto.  Seuraavassa on ei-tuetut tietotyypit sekä muita vaihtoehtoja, voit käyttää tekstin näyttäminen ei-tuetut tietotyypit.

|Tietotyyppi|Vaihtoehtoinen menetelmä|
|---|---|
|[geometria][]|[Varbinary][]|
|[Geography][]|[Varbinary][]|
|[HierarchyId][]|[nvarchar][] (4000)|
|[Kuva][ntext,text,image]|[Varbinary][]|
|[teksti][ntext,text,image]|[varchar][]|
|[ntext][ntext,text,image]|[nvarchar][]|
|[sql_variant][]|Jaa sarake osiin useita erittäin kirjoitetun sarakkeisiin.|
|[taulukko][]|Muuntaa väliaikaisia taulukoita.|
|[aikaleima][]|Uudista koodi, jota käytetään [datetime2][] ja `CURRENT_TIMESTAMP` funktio.  Tuetaan vain vakiot oletusarvoisiksi vuoksi current_timestamp ei voi määrittää oletusarvon rajoitus. Jos haluat siirtää rivin version arvot aikaleima kirjoitetun sarakkeen sitten Käytä [BINAARILUVUN][](8)- tai [VARBINARY][BINAARILUVUN](8) ei ole NULL tai tyhjän rivin version arvot.|
|[XML][]|[varchar][]|
|[käyttäjän määrittämät tyypit][]|muuntaa takaisin niiden alkuperäiset tyypit, jos mahdollista|
|oletusarvot|oletusarvojen tukevat literaalien ja vain vakioita.  Muut deterministic lausekkeita tai -funktioita, kuten `GETDATE()` tai `CURRENT_TIMESTAMP`, ei tueta.|

Alapuolella SQL voidaan suorittaa nykyinen tunnistavan sarakkeet, jotka ovat SQL-tietokanta ei tue Azure SQL-tietovarasto:

```sql
SELECT  t.[name], c.[name], c.[system_type_id], c.[user_type_id], y.[is_user_defined], y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN ('geography','geometry','hierarchyid','image','text','ntext','sql_variant','timestamp','xml')
 AND  y.[is_user_defined] = 1;
```

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja on artikkeleissa [Taulukon yleistä][Yleiskatsaus], [jakaminen taulukon][välit], [indeksoinnin taulukon][indeksin], [Taulukon jakaminen]-[osio], [Ylläpito taulukon tilastotiedot][tilastoja] ja [Väliaikaisia taulukoita][tilapäinen].  Lisätietoja parhaista käytännöistä on [SQL Data Warehouse parhaita käytäntöjä][].

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
[taulukon luominen]: https://msdn.microsoft.com/library/mt203953.aspx
[bigint]: https://msdn.microsoft.com/library/ms187745.aspx
[binaaritiedosto]: https://msdn.microsoft.com/library/ms188362.aspx
[Bit]: https://msdn.microsoft.com/library/ms177603.aspx
[merkki]: https://msdn.microsoft.com/library/ms176089.aspx
[päivämäärä]: https://msdn.microsoft.com/library/bb630352.aspx
[Päivämäärä ja aika]: https://msdn.microsoft.com/library/ms187819.aspx
[datetime2]: https://msdn.microsoft.com/library/bb677335.aspx
[DateTimeOffset-arvoa]: https://msdn.microsoft.com/library/bb630289.aspx
[desimaaliluku]: https://msdn.microsoft.com/library/ms187746.aspx
[liukuluku]: https://msdn.microsoft.com/library/ms173773.aspx
[geometria]: https://msdn.microsoft.com/library/cc280487.aspx
[Geography]: https://msdn.microsoft.com/library/cc280766.aspx
[HierarchyId]: https://msdn.microsoft.com/library/bb677290.aspx
[kokonaisluku]: https://msdn.microsoft.com/library/ms187745.aspx
[rahaa]: https://msdn.microsoft.com/library/ms179882.aspx
[nchar]: https://msdn.microsoft.com/library/ms186939.aspx
[nvarchar]: https://msdn.microsoft.com/library/ms186939.aspx
[ntext,text,image]: https://msdn.microsoft.com/library/ms187993.aspx
[Todellinen]: https://msdn.microsoft.com/library/ms173773.aspx
[smalldatetime]: https://msdn.microsoft.com/library/ms182418.aspx
[smallint]: https://msdn.microsoft.com/library/ms187745.aspx
[Smallmoney]: https://msdn.microsoft.com/library/ms179882.aspx
[sql_variant]: https://msdn.microsoft.com/library/ms173829.aspx
[Sysname]: https://msdn.microsoft.com/library/ms186939.aspx
[taulukko]: https://msdn.microsoft.com/library/ms175010.aspx
[aika]: https://msdn.microsoft.com/library/bb677243.aspx
[aikaleima]: https://msdn.microsoft.com/library/ms182776.aspx
[tinyint]: https://msdn.microsoft.com/library/ms187745.aspx
[Uniqueidentifier]: https://msdn.microsoft.com/library/ms187942.aspx
[Varbinary]: https://msdn.microsoft.com/library/ms188362.aspx
[varchar]: https://msdn.microsoft.com/library/ms186939.aspx
[XML]: https://msdn.microsoft.com/library/ms187339.aspx
[käyttäjän määrittämät tyypit]: https://msdn.microsoft.com/library/ms131694.aspx
