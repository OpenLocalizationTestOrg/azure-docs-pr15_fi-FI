<properties
   pageTitle="Hallitse Laske power Azure SQL Data Warehouse (REST) | Microsoft Azure"
   description="Voit hallita tehtäviä PowerShell Laske power. Skaalaa Laske resurssien säätämällä DWUs. Tai keskeyttäminen ja jatkaminen Laske resurssien kustannusten tallentamiseen."
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

# <a name="manage-compute-power-in-azure-sql-data-warehouse-rest"></a>Laske power Azure SQL Data Warehouse (REST) hallinta

> [AZURE.SELECTOR]
- [Yleiskatsaus](sql-data-warehouse-manage-compute-overview.md)
- [Portal](sql-data-warehouse-manage-compute-portal.md)
- [PowerShellin](sql-data-warehouse-manage-compute-powershell.md)
- [MUILLE KÄYTTÄJILLE](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Skaalaa suorituskyvyn käyttämällä laajentaminen Laske resurssit ja muistin täyttämään havainnollistamiseen muuttaminen tarpeisiin. Tallenna kustannukset skaalauksen takaisin resurssien mukaan huippu aikoja tai keskeyttäminen Laske aikana kokonaan. 

Tämä joukko tehtäviä käyttää Azure-portaalissa:

- Skaalaa suorittaminen
- Keskeytä suorittaminen
- Jatka suorittaminen

Lisätietoja tästä on artikkelissa [Hallitse Laske yleiskatsaus][].

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Skaalaa Laske power

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Voit muuttaa DWUs avulla [Luo tai Päivitä tietokannan][] REST-Ohjelmointirajapinnalla. Seuraavassa esimerkissä määrittää palvelun tason tavoitteen DW1000 tietokannan MySQLDW, jossa omapalvelin-palvelimessa. Palvelin on nimetty ResourceGroup1 Azure resurssiryhmä.

```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/MyServer/databases/MySQLDW?api-version=2014-04-01-preview HTTP/1.1
Content-Type: application/json; charset=UTF-8

{
    "properties": {
        "requestedServiceObjectiveName": DW1000
    }
}
```

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Keskeytä suorittaminen

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Pysähtyvän tietokannan käyttäminen [Keskeytä tietokannan][] REST-Ohjelmointirajapinnalla. Seuraavassa esimerkissä keskeyttää tietokanta nimeltä Database02 nimeltä palvelin01 palvelimessa. Palvelin on nimetty ResourceGroup1 Azure resurssiryhmä.

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/Server01/databases/Database02/pause?api-version=2014-04-01-preview HTTP/1.1
```

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Jatka suorittaminen

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Voit aloittaa tietokannan käyttämällä [Ansioluettelon tietokannan][] REST-Ohjelmointirajapinnalla. Seuraavassa esimerkissä käynnistyy tietokanta nimeltä Database02 nimeltä palvelin01 palvelimessa. Palvelin on nimetty ResourceGroup1 Azure resurssiryhmä. 

```
POST https://management.azure.com/subscriptions{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/Server01/databases/Database02/resume?api-version=2014-04-01-preview HTTP/1.1
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Seuraavat vaiheet

Katso muita hallintatehtäviä [hallinnan yleiskatsaus][].

<!--Image references-->

<!--Article references-->
[Hallinnan yleiskatsaus]: ./sql-data-warehouse-overview-manage.md
[Laske yleiskatsaus hallinta]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[Keskeytä-tietokanta]: https://msdn.microsoft.com/library/azure/mt718817.aspx
[Jatka tietokannan]: https://msdn.microsoft.com/library/azure/mt718820.aspx
[Luo tai Päivitä tietokannan]: https://msdn.microsoft.com/library/azure/mt163685.aspx

<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
