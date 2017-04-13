<properties
    pageTitle="Palauttaa Azure SQL-tietokantaan edellisessä kohdassa aika (PowerShell) | Microsoft Azure"
    description="Palauttaa Azure SQL-tietokantaan aiempaan ajankohtaan"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="NA"
    ms.date="07/17/2016"
    ms.author="sstein"/>

# <a name="restore-an-azure-sql-database-to-a-previous-point-in-time-with-powershell"></a>Palauttaa Azure SQL-tietokantaan aiempaan ajankohtaan PowerShellin avulla

> [AZURE.SELECTOR]
- [Yleiskatsaus](sql-database-recovery-using-backups.md)
- [Ajankohta palauttaminen: Azure portal](sql-database-point-in-time-restore-portal.md)

Tässä artikkelissa kerrotaan, miten voit palauttaa tietokannan aikaisempaan senhetkinen [SQL-tietokantaan automaattisen varmuuskopioinnin](sql-database-automated-backups.md). Voit tehdä tämän PowerShell-toiminnolla.

[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="restore-your-database-to-a-point-in-time-as-a-standalone-database"></a>Palauttaa tietokannan pisteeseen erillinen tietokantana samanaikaisesti

1. Pyydä tietokannan palautettavan avulla [Get-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt603648(v=azure.300\).aspx) cmdlet-komento.

        $Database = Get-AzureRmSqlDatabase -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Palauta tietokanta kohtaa samanaikaisesti käyttämällä [palauttaminen-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt693390(v=azure.300\).aspx) cmdlet-komento.

        Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime UTCDateTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.ServerName -TargetDatabaseName "RestoredDatabase" –ResourceId $Database.ResourceID -Edition "Standard" -ServiceObjectiveName "S2"


## <a name="restore-your-database-to-a-point-in-time-into-an-elastic-database-pool"></a>Palauttaa tietokannan pisteeseen senhetkinen joustavasti tietokannan resurssivarantoon

1. Pyydä tietokannan palautettavan avulla [Get-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt603648(v=azure.300\).aspx) cmdlet-komento.

        $Database = Get-AzureRmSqlDatabase -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Palauta tietokanta kohtaa samanaikaisesti käyttämällä [palauttaminen-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt693390(v=azure.300\).aspx) cmdlet-komento.

        Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime UTCDateTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.ServerName -TargetDatabaseName "RestoredDatabase" –ResourceId $Database.ResourceID –ElasticPoolName "elasticpool01"


## <a name="next-steps"></a>Seuraavat vaiheet

- Liiketoiminnan jatkuvuus-yleiskatsaus ja tilanteita, joissa on artikkelissa [liiketoiminnan jatkuvuuden yhteenveto](sql-database-business-continuity.md)
- Lisätietoja Azure SQL-tietokanta automaattisen varmuuskopioinnin on artikkelissa [SQL-tietokantaan automaattisen varmuuskopioinnin suunnittelu](sql-database-automated-backups.md)
- Lisätietoja palauttaminen automaattisen varmuuskopioinnin käyttämisestä on artikkelissa [tietokannan palvelun käynnistämä-varmuuskopioista palauttaminen](sql-database-recovery-using-backups.md)
- Lisätietoja nopeampi palautusasetukset on artikkelissa [Aktiivinen-Geo-replikointi](sql-database-geo-replication-overview.md)  
- Lisätietoja käyttämisestä automaattisen varmuuskopioinnin arkistointia varten, tutustu artikkeliin [tietokannan kopioiminen](sql-database-copy.md)
