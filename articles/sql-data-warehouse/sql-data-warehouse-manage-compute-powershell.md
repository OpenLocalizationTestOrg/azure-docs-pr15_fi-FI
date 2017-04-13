<properties
   pageTitle="Hallitse Laske virtaa Azure SQL-tietovarasto (PowerShell) | Microsoft Azure"
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
   ms.date="06/13/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-powershell"></a>Laske virtaa Azure SQL-tietovarasto (PowerShell) hallinta

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


## <a name="before-you-begin"></a>Ennen aloittamista

### <a name="install-the-latest-version-of-azure-powershell"></a>PowerShellin Azure uusimman version asentaminen

> [AZURE.NOTE]  Jos haluat käyttää PowerShellin Azure SQL-tietovarasto, sinun on PowerShellin Azure versio 1.0.3 tai suurempi.  Voit tarkistaa nykyisen version Suorita-komento **Get-moduuli - ListAvailable-nimen Azure**. Voit asentaa uusimman version [Microsoft WWW-ympäristö asennusohjelma][].  Katso lisätietoja, [miten voit asentaa ja määrittää PowerShellin Azure][].

### <a name="get-started-with-azure-powershell-cmdlets"></a>Aloita Azure PowerShellin cmdlet-komennot

Aloittaminen:

1. Avaa Azure PowerShell. 
2. PowerShell-kehotteessa Suorita nämä komennot kirjautua sisään Azure Resurssienhallinta ja valitse tilauksen.

    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Skaalaa Laske power

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Voit muuttaa DWUs [Määrittäminen AzureRmSqlDatabase][] PowerShell-cmdlet-komennolla. Seuraavassa esimerkissä määrittää palvelun tason tavoitteen DW1000 tietokannan MySQLDW, jossa omapalvelin-palvelimessa. 

```Powershell
Set-AzureRmSqlDatabase -DatabaseName "MySQLDW" -ServerName "MyServer" -RequestedServiceObjectiveName "DW1000"
```

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Keskeytä suorittaminen

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Tietokannan pysähtyvän [Keskeytys AzureRmSqlDatabase][] cmdlet-komennon avulla Seuraavassa esimerkissä keskeyttää tietokanta nimeltä Database02 nimeltä palvelin01 palvelimessa. Palvelin on nimetty ResourceGroup1 Azure resurssiryhmä. 

> [AZURE.NOTE] Huomaa, että jos palvelin on foo.database.windows.net, Määritä "tapahtuman nimenä" PowerShell-cmdlet--palvelin.

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
```
Muunnelma, seuraavan esimerkin hakee tietokannan $database objektiin. Se sitten piiput [Keskeytys AzureRmSqlDatabase][]objekti. Objektin resultDatabase on tallennettu tulokset. Lopulliseksi-komennolla näyttää tulokset.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Jatka suorittaminen

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Voit aloittaa tietokannan käyttämällä [Ansioluettelon AzureRmSqlDatabase][] cmdlet-komento. Seuraavassa esimerkissä käynnistyy tietokanta nimeltä Database02 nimeltä palvelin01 palvelimessa. Palvelin on nimetty ResourceGroup1 Azure resurssiryhmä. 

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" -DatabaseName "Database02"
```

Muunnelma, seuraavan esimerkin hakee tietokannan $database objektiin. [Jatka AzureRmSqlDatabase][] objektin piiput jälkeen ja tallentaa tulokset $resultDatabase. Lopulliseksi-komennolla näyttää tulokset.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
$resultDatabase
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Seuraavat vaiheet

Katso muita hallintatehtäviä [hallinnan yleiskatsaus][].

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Hallinnan yleiskatsaus]: ./sql-data-warehouse-overview-manage.md
[Asentaminen ja määrittäminen PowerShellin Azure]: ./powershell-install-configure.md
[Laske yleiskatsaus hallinta]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[Jatka AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
[Keskeyttää AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx
[Määritä AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx

<!--Other Web references-->
[Microsoftin WWW-ympäristö asennusohjelma]: https://aka.ms/webpi-azps
[Azure portal]: http://portal.azure.com/
