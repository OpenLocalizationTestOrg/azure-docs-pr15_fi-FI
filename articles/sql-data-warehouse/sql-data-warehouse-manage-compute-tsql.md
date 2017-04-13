<properties
   pageTitle="Hallitse Laske power Azure SQL Data Warehouse (REST) | Microsoft Azure"
   description="Transact-SQL (T-SQL) tehtävät muuttamalla DWUs suorituskyvyn asteikko-kohtaa. Voit tallentaa kustannukset skaalaus takaisin huippu esiintymistä."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/08/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-t-sql"></a>Hallitse Laske virtaa Azure SQL-tietovarasto (T-SQL)

> [AZURE.SELECTOR]
- [Yleiskatsaus](sql-data-warehouse-manage-compute-overview.md)
- [Portal](sql-data-warehouse-manage-compute-portal.md)
- [PowerShellin](sql-data-warehouse-manage-compute-powershell.md)
- [MUILLE KÄYTTÄJILLE](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Skaalaa suorituskyvyn käyttämällä laajentaminen Laske resurssit ja muistin täyttämään havainnollistamiseen muuttaminen tarpeisiin. Tallenna kustannukset skaalauksen takaisin resurssien mukaan huippu aikoja tai keskeyttäminen Laske aikana kokonaan. 

Tämä joukko tehtäviä käyttää T-SQL avulla:

- Nykyinen DWU näyttöasetukset
- Muuta Laske resurssien säätämällä DWUs

Keskeyttäminen ja jatkaminen tietokannan, jokin muu vaihtoehto ympäristö ohjeaiheen yläreunassa.

Lisätietoja tästä on artikkelissa [Hallitse Laske power yleiskatsaus][].

<a name="current-dwu-bk"></a>

## <a name="view-current-dwu-settings"></a>Nykyisen DWU näyttöasetukset

Voit tarkastella tietokantoja nykyiset DWU asetukset seuraavasti:

1. Avaa Visual Studio 2015 SQL Server Object Explorerissa.
2. Muodosta yhteys loogisen SQL-tietokantapalvelinta liittyvät perustyylin tietokantaan.
2. Valitse sys.database_service_objectives dynaaminen hallinta-näkymässä. Tässä on esimerkki: 

```
SELECT
 db.name [Database],
 ds.edition [Edition],
 ds.service_objective [Service Objective]
FROM
 sys.database_service_objectives ds
 JOIN sys.databases db ON ds.database_id = db.database_id
```

<a name="scale-dwu-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute"></a>Skaalaa suorittaminen

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Voit muuttaa DWUs seuraavasti:


1. Yhdistäminen perustyylin tietokantaa, joka liittyy looginen SQL-tietokanta-palvelimen kanssa.
2. Käytä [Muuta TIETOKANTAA][] TSQL lausetta. Seuraava esimerkki määrittää palvelun tason tavoitteen DW1000 tietokannan MySQLDW. 

```Sql
ALTER DATABASE MySQLDW
MODIFY (SERVICE_OBJECTIVE = 'DW1000')
;
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Seuraavat vaiheet

Katso muita hallintatehtäviä [hallinnan yleiskatsaus][].

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Hallinnan yleiskatsaus]: ./sql-data-warehouse-overview-manage.md
[Laske power yleiskatsaus hallinta]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->

[MUUTTAA TIETOKANTAA]: https://msdn.microsoft.com/library/mt204042.aspx


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
