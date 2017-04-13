<properties
   pageTitle="Dynaamisen SQL-SQL-tietovarasto | Microsoft Azure"
   description="Avulla dynaamisen SQL Azure SQL-tietovarasto for, ratkaisujen kehittämiseen liittyviä vinkkejä."
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

# <a name="dynamic-sql-in-sql-data-warehouse"></a>Dynaamisen SQL-SQL-tietovarasto
Kun Sovelluskoodin kehittäminen SQL-tietovarasto joudut ehkä kannattaa käyttää dynaamisen sql joustavia, Yleinen ja moduulit ratkaisuja. SQL Data Warehouse ei tue Blob-objektien tietotyypit tällä hetkellä. Tämä saattaa rajoittaa oman merkkijonot koon Blob-objektien tyypit kuuluttava varchar(max) ja nvarchar(max). Jos olet käyttänyt tällaisia sovelluksen koodissa luotaessa erittäin suuri merkkijonoja, sinun on Jaa koodin joukkojen ja johto lauseella sijaan.

Esimerkki:

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

Jos merkkijono on lyhyt voit [sp_executesql][] normaalisti.

> [AZURE.NOTE] Suoritetaan dynaamisen SQL lauseet on kaikki TSQL kelpoisuussääntöjen veloittaa.

## <a name="next-steps"></a>Seuraavat vaiheet
Artikkelissa kehitys Lisävihjeitä [kehittäminen yleiskatsaus][].

<!--Image references-->

<!--Article references-->
[kehittäminen yleiskatsaus]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[sp_executesql]: https://msdn.microsoft.com/library/ms188001.aspx

<!--Other Web references-->
