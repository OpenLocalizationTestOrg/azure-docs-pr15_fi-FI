<properties 
    pageTitle="Kopioi PowerShellin Azure SQL-tietokantaan | Microsoft Azure" 
    description="Luo PowerShellin Azure SQL-tietokanta" 
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/08/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="copy-an-azure-sql-database-using-powershell"></a>Kopioi PowerShellin Azure SQL-tietokantaan


> [AZURE.SELECTOR]
- [Yleiskatsaus](sql-database-copy.md)
- [Azure portal](sql-database-copy-portal.md)
- [PowerShellin](sql-database-copy-powershell.md)
- [T-SQL](sql-database-copy-transact-sql.md)

Tässä artikkelissa kerrotaan PowerShellin avulla on SQL-tietokanta kopioiminen samaan palvelimeen toiseen palvelimeen, ja kopioi tietokannan [joustavasti tietokannan resurssivarantoon](sql-database-elastic-pool.md). Kopioi tietokantatoiminnon käyttää [Uutta AzureRmSqlDatabaseCopy](https://msdn.microsoft.com/library/mt603644.aspx) cmdlet-komento. 


Tässä artikkelissa suorittamiseen tarvitset seuraavat:

- Azure SQL-tietokantaan (tietokanta, kopioi). Jos sinulla ei ole SQL-tietokantaan, luo yhden noudattamalla tämän artikkelin: [ensimmäisen Azure SQL-tietokannan luominen](sql-database-get-started.md).
- PowerShellin Azure uusimman version. Lisätietoja on artikkelissa [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md).


SQL-tietokannan monia uusia ominaisuuksia tuetaan vain, kun käytät [Azure resurssien hallinnan käyttöönottomalli](../azure-resource-manager/resource-group-overview.md)niin esimerkeissä [Azure SQL-tietokannan PowerShellin cmdlet-komennot](https://msdn.microsoft.com/library/azure/mt574084.aspx) resurssien hallinta. Perinteinen käyttöönotto aiemmin mallin [Azure SQL-tietokanta (perinteinen) cmdlet-komennot](https://msdn.microsoft.com/library/azure/dn546723.aspx) ovat tuettuja yhteensopivuus aiempien versioiden kanssa, mutta suosittelemme käyttämään resurssien hallinnan Cmdlet-komentoja.


>[AZURE.NOTE] Tietokannan koon mukaan kopiointi saattaa kestää jonkin aikaa suorittamiseen.


## <a name="copy-a-sql-database-to-the-same-server"></a>Kopioi SQL-tietokanta-palvelimeen

Jos haluat luoda kopion samaan palvelimeen, jätetään pois `-CopyServerName` parametrin (tai määrittää palvelimeen).

    New-AzureRmSqlDatabaseCopy -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -CopyDatabaseName "database1_copy"

## <a name="copy-a-sql-database-to-a-different-server"></a>Kopioi SQL-tietokannan eri palvelimeen

Jos haluat luoda kopion eri palvelimessa, Sisällytä `-CopyServerName` parametri ja määritä se toiseen palvelimeen. *Kopioi* palvelin on jo olemassa. Jos se on eri resurssiryhmä ja valitse on myös lisättävä `-CopyResourceGroupName` parametria.

    New-AzureRmSqlDatabaseCopy -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -CopyServerName "server2" -CopyDatabaseName "database1_copy"


## <a name="copy-a-sql-database-into-an-elastic-database-pool"></a>Kopioi SQL-tietokantaan joustavasti tietokannan resurssivarantoon

SQL-tietokannan kopion luominen resurssivarantoon, Määritä `-ElasticPoolName` parametrin aiemmin luotuun ryhmään.

    New-AzureRmSqlDatabaseCopy -ResourceGroupName "resourcegoup1" -ServerName "server1" -DatabaseName "database1" -CopyResourceGroupName "poolResourceGroup" -CopyServerName "poolServer1" -CopyDatabaseName "database1_copy" -ElasticPoolName "poolName"


## <a name="resolve-logins"></a>Ratkaise kirjautumiset

Voit ratkaista kirjautumiset kopiointi päätyttyä, katso [ratkaista kirjautumiset](sql-database-copy-transact-sql.md#resolve-logins-after-the-copy-operation-completes)


## <a name="example-powershell-script"></a>Esimerkki PowerShell-komentosarjaa

Seuraavaa komentosarjaa oletetaan, että kaikki resurssiryhmät-palvelimet- ja varannon on jo olemassa (korvaa aiemmin luotu resurssien muuttujien arvoihin). Kaikki tiedot on oltava, lukuun ottamatta tietokannan kopion.

    # Sign in to Azure and set the subscription to work with
    # ------------------------------------------------------
    $SubscriptionId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    Add-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $SubscriptionId
    
    
    # SQL database source (the existing database to copy)
    # ---------------------------------------------------
    $sourceDbName = "db1"
    $sourceDbServerName = "server1"
    $sourceDbResourceGroupName = "rg1"
    
    # SQL database copy (the new db to be created)
    # --------------------------------------------
    $copyDbName = "db1_copy"
    $copyDbServerName = "server2"
    $copyDbResourceGroupName = "rg2"
    
    # Copy a database to the same server
    # ----------------------------------
    New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyDatabaseName $copyDbName
    
    # Copy a database to a different server
    # -------------------------------------
    New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyResourceGroupName $copyDbResourceGroupName -CopyServerName $copyDbServerName -CopyDatabaseName $copyDbName
    
    # Copy a database into an elastic database pool
    # ---------------------------------------------
    $poolName = "pool1"
    
    New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyResourceGroupName $copyDbResourceGroupName -CopyServerName $copyDbServerName -ElasticPoolName $poolName -CopyDatabaseName $copyDbName



    

## <a name="next-steps"></a>Seuraavat vaiheet

- Saat yleiskuvan kopioiminen Azure SQL-tietokantaan [kopioida Azure SQL-tietokantaan](sql-database-copy.md) .
- Katso Azure-portaalissa tietokannan kopioiminen [Kopioi Azure-portaalissa Azure SQL-tietokantaan](sql-database-copy-portal.md) .
- Kohdassa käyttämällä Transact-SQL-tietokannan kopioiminen [Kopioi Azure SQL-tietokantaan käyttämällä T-SQL](sql-database-copy-transact-sql.md) .
- Katso lisätietoja käyttäjien ja kirjautumiset hallinta, kun tietokannan kopioiminen toiseen looginen palvelimeen [Azure SQL-tietokannan suojauksen jälkeen palauttaminen hallintaa](sql-database-geo-replication-security-config.md) .


## <a name="additional-resources"></a>Lisäresursseja

- [Uusi AzureRmSqlDatabase](https://msdn.microsoft.com/library/mt603644.aspx)
- [Hae AzureRmSqlDatabaseActivity](https://msdn.microsoft.com/library/mt603687.aspx)
- [Kirjautumiset hallinta](sql-database-manage-logins.md)
- [Yhteyden muodostaminen SQL-tietokantaa, jossa SQL Server Management Studiossa ja suorittaa otoksen T-SQL-kysely](sql-database-connect-query-ssms.md)
- [Vie tietokantaan BACPAC](sql-database-export.md)
- [Liiketoiminnan jatkuvuus yhteenveto](sql-database-business-continuity.md)
- [SQL-tietokanta-asiakirjat](https://azure.microsoft.com/documentation/services/sql-database/)
- [Azure SQL tietokanta PowerShell Cmdlet-viittaus](https://msdn.microsoft.com/library/mt574084.aspx)
