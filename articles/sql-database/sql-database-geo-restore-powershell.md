<properties
    pageTitle="Palauttaa Azure SQL-tietokantaan geo tarpeettomat varmuuskopiosta (PowerShell) | Microsoft Azure"
    description="Palauttaa Azure SQL-tietokantaan uuden palvelimen geo tarpeettomat varmuuskopiosta"
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

# <a name="restore-an-azure-sql-database-from-a-geo-redundant-backup-by-using-powershell"></a>Palauttaa Azure SQL-tietokantaan geo tarpeettomat varmuuskopiosta PowerShell-toiminnolla


> [AZURE.SELECTOR]
- [Yleiskatsaus](sql-database-recovery-using-backups.md)
- [GEO-Palauta: Azure Portal](sql-database-geo-restore-portal.md)

Tässä artikkelissa kerrotaan, miten palauttaa tietokannan siirtäminen uuteen palvelimeen käyttämällä geo palauttaminen. Tämä voidaan toteuttaa PowerShellin kautta.

[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="geo-restore-your-database-into-a-standalone-database"></a>GEO-Palauta tietokanta erikseen tietokantaan

1. Hae geo tarpeettomat varmuuskopion tietokannasta, jonka haluat palauttaa avulla [Get-AzureRmSqlDatabaseGeoBackup] (https://msdn.microsoft.com/library/azure/mt693388(v=azure.300\).aspx) cmdlet-komento.

        $GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Aloittaa geo tarpeettomat varmuuskopiosta palauttaminen avulla [palauttaminen-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt693390(v=azure.300\).aspx) cmdlet-komento.

        Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "TargetResourceGroup" -ServerName "TargetServer" -TargetDatabaseName "RestoredDatabase" –ResourceId $GeoBackup.ResourceID -Edition "Standard" -RequestedServiceObjectiveName "S2"


## <a name="geo-restore-your-database-into-an-elastic-database-pool"></a>Tietokannan tuominen joustavasti tietokannan resurssivarantoon GEO-Palauta

1. Hae geo tarpeettomat varmuuskopion tietokannasta, jonka haluat palauttaa avulla [Get-AzureRmSqlDatabaseGeoBackup] (https://msdn.microsoft.com/library/azure/mt693388(v=azure.300\).aspx) cmdlet-komento.

        $GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Aloittaa geo tarpeettomat varmuuskopiosta palauttaminen avulla [palauttaminen-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt693390(v=azure.300\).aspx) cmdlet-komento. Määritä ryhmän nimi, jonka haluat palauttaa tietokannan kyselyjä.

        Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "TargetResourceGroup" -ServerName "TargetServer" -TargetDatabaseName "RestoredDatabase" –ResourceId $GeoBackup.ResourceID –ElasticPoolName "elasticpool01"  


## <a name="next-steps"></a>Seuraavat vaiheet

- Liiketoiminnan jatkuvuus-yleiskatsaus ja tilanteita, joissa on artikkelissa [liiketoiminnan jatkuvuuden yhteenveto](sql-database-business-continuity.md).
- Lisätietoja Azure SQL-tietokanta automaattisen varmuuskopioinnin on artikkelissa [SQL-tietokantaan automaattisen varmuuskopioinnin](sql-database-automated-backups.md).
- Lisätietoja automaattisen varmuuskopioinnin käyttämisestä palauttamista varten on artikkelissa [tietokannan palvelun käynnistämä-varmuuskopioista palauttaminen](sql-database-recovery-using-backups.md).
- Lisätietoja nopeampi palautusasetukset on artikkelissa [Aktiivinen-Geo-replikoinnin](sql-database-geo-replication-overview.md).  
- Lisätietoja käyttämisestä automaattisen varmuuskopioinnin arkistointia varten on artikkelissa [tietokannan kopiota](sql-database-copy.md).
