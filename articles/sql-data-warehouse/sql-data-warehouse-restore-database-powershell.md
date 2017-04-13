<properties
   pageTitle="Palauttaa Azure SQL-tietovarasto (PowerShell) | Microsoft Azure"
   description="Azure SQL-tietovarasto palauttaminen PowerShell tehtävät."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="Lakshmi1812"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/21/2016"
   ms.author="lakshmir;barbkess;sonyama"/>

# <a name="restore-an-azure-sql-data-warehouse-powershell"></a>Palauttaa Azure SQL-tietovarasto (PowerShell)

> [AZURE.SELECTOR]
- [Yleiskatsaus][]
- [Portal][]
- [PowerShellin][]
- [MUILLE KÄYTTÄJILLE][]

Tässä artikkelissa kerrotaan palauttamisesta Azure SQL-tietovarasto PowerShellin avulla.

## <a name="before-you-begin"></a>Ennen aloittamista

**Tarkista DTU kapasiteetti.** SQL Server (kuten myserver.database.windows.net) on oletusarvoinen DTU kiintiön nykyisessä kunkin SQL-tietovarasto.  Ennen kuin voit palauttaa SQL-tietovarasto, varmista, että SQL server on tarpeeksi jäljellä olevan DTU kiintiön palautetaan parhaillaan tietokannan. Lisätietoja DTU tarpeen laskea tai pyydä lisätietoja DTU on artikkelissa [DTU kiintiön muutospyyntö][].

### <a name="install-powershell"></a>PowerShellin asentaminen

Jotta voit käyttää PowerShellin Azure SQL-tietovarasto, täytyy asentaa PowerShellin Azure 1.0 tai uudempi versio.  Voit tarkistaa versiosi suorittamalla **Get-moduuli - ListAvailable-nimen AzureRM**.  [Microsoftin WWW-ympäristö asennusohjelma][]voidaan asentaa uusin versio.  Katso lisätietoja uusimman version asentamisesta, [miten voit asentaa ja määrittää PowerShellin Azure][].

## <a name="restore-an-active-or-paused-database"></a>Aktiivinen tai keskeytetty tietokannan palauttaminen

Voit palauttaa tietokannan tilannevedoksen käyttämällä [Palauttaminen AzureRmSqlDatabase][] PowerShell-cmdlet-komennon.

1. Avaa Windows PowerShell.
2. Muodosta yhteys Azure-tili ja Luettele kaikki tilisi tilaukset.
3. Valitse tilaus, joka sisältää tietokannan palautetaan.
4. Luettelo tietokannan palauttaminen pisteet.
5. Valitse haluamasi palauttaminen-kohdan avulla RestorePointCreationDate.
6. Tietokannan palauttaminen haluamasi palauttaminen pisteeseen.
7. Tarkista, että palautettu tietokanta on online-tilassa.

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# List the last 10 database restore points
((Get-AzureRMSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName ($DatabaseName).RestorePointCreationDate)[-10 .. -1]

# Or list all restore points
Get-AzureRmSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Get the specific database to restore
$Database = Get-AzureRmSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Pick desired restore point using RestorePointCreationDate
$PointInTime="<RestorePointCreationDate>"  

# Restore database from a restore point
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.$ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Verify the status of restored database
$RestoredDatabase.status

```

>[AZURE.NOTE] Kun palautus on valmis, voit määrittää palautetun tietokannan mukaan seuraavat [tietokantasi palautuksen jälkeen asetuksia][].


## <a name="restore-a-deleted-database"></a>Poistetun tietokannan palauttaminen

Jos haluat palauttaa poistetun tietokannan, käytä [Palauttaminen AzureRmSqlDatabase][] cmdlet-komento.

1. Avaa Windows PowerShell.
2. Muodosta yhteys Azure-tili ja Luettele kaikki tilisi tilaukset.
3. Valitse tilaus, joka sisältää palauttaa poistetun tietokannan.
4. Pyydä tietokannan avaamiseksi Poistetut.
5. Palauttaa poistetun tietokannan.
6. Tarkista, että palautettu tietokanta on online-tilassa.

```Powershell
$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# Get the deleted database to restore
$DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $ResourceGroupNam -ServerName $ServerName -DatabaseName $DatabaseName

# Restore deleted database
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $DeletedDatabase.ResourceID

# Verify the status of restored database
$RestoredDatabase.status
```

>[AZURE.NOTE] Kun palautus on valmis, voit määrittää palautetun tietokannan mukaan seuraavat [tietokantasi palautuksen jälkeen asetuksia][].


## <a name="restore-from-an-azure-geographical-region"></a>Palauttaa Azure maantieteellinen alue

Palauttaa tietokannan [Palauttaminen AzureRmSqlDatabase][] cmdlet-komennon avulla

1. Avaa Windows PowerShell.
2. Muodosta yhteys Azure-tili ja Luettele kaikki tilisi tilaukset.
3. Valitse tilaus, joka sisältää tietokannan palautetaan.
4. Hae haluamasi tietokanta.
5. Tietokannan palauttaminen pyynnön luominen
6. Tarkista geo palauttaa tietokannan tila.

```Powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<Subscription_name>"

# Get the database you want to recover
$GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>"

# Recover database
$GeoRestoredDatabase = Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourTargetServer>" -TargetDatabaseName "<NewDatabaseName>" –ResourceId $GeoBackup.ResourceID

# Verify that the geo-restored database is online
$GeoRestoredDatabase.status
```

>[AZURE.NOTE] Voit määrittää tietokannan, kun palautus on valmis, katso [tietokantasi palautuksen jälkeen asetuksia][]. 


Palautetun tietokanta on TDE käyttävä Jos lähdetietokannan ei TDE käytössä.


## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja liiketoiminnan jatkuvuuden ominaisuuksista versioiden Azure SQL-tietokanta, lue [Azure SQL-tietokanta liiketoiminnan jatkuvuuden yhteenveto][].

<!--Image references-->

<!--Article references-->
[Azure SQL-tietokantaan liiketoiminnan jatkuvuuden yhteenveto]: sql-database-business-continuity.md
[Muutospyyntö DTU kiintiö]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Määritä tietokantasi palautuksen jälkeen]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery
[Asentaminen ja määrittäminen PowerShellin Azure]: powershell-install-configure.md
[Yleiskatsaus]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShellin]: ./sql-data-warehouse-restore-database-powershell.md
[MUILLE KÄYTTÄJILLE]: ./sql-data-warehouse-restore-database-rest-api.md
[Määritä tietokantasi palautuksen jälkeen]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery

<!--MSDN references-->
[Palauta AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoftin WWW-ympäristö asennusohjelma]: https://aka.ms/webpi-azps
