<properties
    pageTitle="Palauta poistetut Azure SQL-tietokanta (PowerShell) | Microsoft Azure"
    description="Palauta poistetut Azure SQL-tietokanta (PowerShell)."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/12/2016"
    ms.author="sstein"
    ms.workload="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="restore-a-deleted-azure-sql-database-by-using-powershell"></a>Palauta poistetut Azure SQL-tietokanta PowerShell-toiminnolla

> [AZURE.SELECTOR]
- [Yleiskatsaus](sql-database-recovery-using-backups.md)
- [Palauta poistettu DB: portaali](sql-database-restore-deleted-database-portal.md)
- [**Palauta poistettu DB: PowerShell**](sql-database-restore-deleted-database-powershell.md)

[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]


## <a name="get-a-list-of-deleted-databases"></a>Poistetun tietokantojen luettelo

```
$resourceGroupName = "resourcegroupname"
$sqlServerName = "servername"

$DeletedDatabases = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $resourceGroupName -ServerName $sqlServerName
```

## <a name="restore-your-deleted-database-into-a-standalone-database"></a>Palauttaa poistetun tietokannan erillinen-tietokantaan

Pyydä poistetun tietokannan varmuuskopiointi, jonka haluat palauttaa [Get-AzureRmSqlDeletedDatabaseBackup](https://msdn.microsoft.com/library/azure/mt693387(v=azure.300/).aspx) cmdlet-komennolla. Käynnistä sitten palauttaminen varmuuskopiosta poistetun tietokannan [Palauttaminen AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390(v=azure.300/).aspx) cmdlet-komennolla.

```
$resourceGroupName = "resourcegroupname"
$sqlServerName = "servername"
$databaseName = "deletedDbToRestore"

$DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $resourceGroupName -ServerName $sqlServerName -DatabaseName $databaseName

Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName "RestoredDatabase" –ResourceId $DeletedDatabase.ResourceID -Edition "Standard" -ServiceObjectiveName "S2"
```


## <a name="restore-your-deleted-database-into-an-elastic-database-pool"></a>Palauttaa poistetun tietokannan joustavasti tietokannan resurssivarantoon

Pyydä poistetun tietokannan varmuuskopiointi, jonka haluat palauttaa [Get-AzureRmSqlDeletedDatabaseBackup](https://msdn.microsoft.com/library/azure/mt693387(v=azure.300/).aspx) cmdlet-komennolla. Käynnistä sitten palauttaminen varmuuskopiosta poistetun tietokannan [Palauttaminen AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390(v=azure.300/).aspx) cmdlet-komennolla.

```
$resourceGroupName = "resourcegroupname"
$sqlServerName = "servername"
$databaseName = "deletedDbToRestore"

$DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $resourceGroupName -ServerName $sqlServerName -DatabaseName $databaseName

Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName "RestoredDatabase" –ResourceId $DeletedDatabase.ResourceID –ElasticPoolName "elasticpool01"
```


## <a name="next-steps"></a>Seuraavat vaiheet

- Liiketoiminnan jatkuvuus-yleiskatsaus ja tilanteita, joissa on artikkelissa [liiketoiminnan jatkuvuuden yhteenveto](sql-database-business-continuity.md)
- Lisätietoja Azure SQL-tietokanta automaattisen varmuuskopioinnin on artikkelissa [SQL-tietokantaan automaattisen varmuuskopioinnin suunnittelu](sql-database-automated-backups.md)
- Lisätietoja palauttaminen automaattisen varmuuskopioinnin käyttämisestä on artikkelissa [tietokannan palvelun käynnistämä-varmuuskopioista palauttaminen](sql-database-recovery-using-backups.md)
- Lisätietoja nopeampi palautusasetukset on artikkelissa [Aktiivinen-Geo-replikointi](sql-database-geo-replication-overview.md)  
- Lisätietoja käyttämisestä automaattisen varmuuskopioinnin arkistointia varten, tutustu artikkeliin [tietokannan kopioiminen](sql-database-copy.md)
