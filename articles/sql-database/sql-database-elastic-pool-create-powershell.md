<properties
    pageTitle="Luo uusi joustavasti tietokanta-varanto PowerShellin | Microsoft Azure"
    description="Opettele käyttämään PowerShell asteikko-kohtaa Azure SQL-tietokanta luomalla hallittavan useiden tietokantojen skaalattava joustavasti tietokannan varannon resursseja."
    services="sql-database"
    documentationCenter=""
    authors="srinia"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="05/27/2016"
    ms.author="srinia"/>

# <a name="create-a-new-elastic-database-pool-with-powershell"></a>Luo uusi ryhmä joustavasti tietokannan PowerShellin avulla

> [AZURE.SELECTOR]
- [Azure portal](sql-database-elastic-pool-create-portal.md)
- [PowerShellin](sql-database-elastic-pool-create-powershell.md)
- [C#](sql-database-elastic-pool-create-csharp.md)


Opettele luomaan [joustavasti tietokannan resurssivarantoon](sql-database-elastic-pool.md) käyttämällä PowerShell cmdlet-komennot. 

Katso yleisiä virhekoodit [SQL-virhekoodit SQL-tietokantaan asiakassovellukset: tietokannan yhteysvirhe ja muiden ongelmien](sql-database-develop-error-messages.md).

> [AZURE.NOTE] Joustavasti jakavat on yleisesti saatavilla (GA) kaikki Azure alueilla Pohjois keskitetyn US ja missä se on käytössä esikatselu Länsi Intia lukuun ottamatta.  Näillä alueilla joustavasti jakavat GA toimitetaan mahdollisimman pian. Lisäksi joustavasti jakavat tällä hetkellä tue tietokantoja, joissa [ladatun OLTP tai ladatun analytics](sql-database-in-memory.md).


Sinun on oltava vähintään Azure PowerShell 1.0. Lisätietoja on artikkelissa [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md).

## <a name="create-a-new-pool"></a>Luo uusi ryhmä

[Uusi AzureRmSqlElasticPool](https://msdn.microsoft.com/library/azure/mt619378.aspx) cmdlet-komento luo uusi ryhmä. Palvelun taso-arvon (basic, standard tai premium) on rajoitettu eDTU resurssivarantoon, min ja max Dtus arvot. Katso [eDTU ja tallennustilaa tiedostokokorajoituksista joustavasti jakavat ja joustavasti tietokannat](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases).

    New-AzureRmSqlElasticPool -ResourceGroupName "resourcegroup1" -ServerName "server1" -ElasticPoolName "elasticpool1" -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100


## <a name="create-a-new-elastic-database-in-a-pool"></a>Luo uusi joustavasti tietokanta resurssivarantoon

[Uusi AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619339.aspx) -cmdlet-komennolla ja **ElasticPoolName** -parametrin arvoksi kohde-ryhmän. Siirtää aiemmin luodun tietokannan resurssivarantoon, on artikkelissa [siirtää joustavasti resurssivarantoon tietokannasta](sql-database-elastic-pool-manage-powershell.md#Move-a-database-into-an-elastic-pool).

    New-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## <a name="create-a-pool-and-populate-it-with-multiple-new-databases"></a>Luo resurssivarantoon ja lisää useiden uusien tietokantojen 

Suuri määrä resurssivarantoon tietokantojen luominen voi kestää aikaa, kun valmis käyttämällä portal tai PowerShellin cmdlet-komennot, jotka luovat kerralla vain yhden tietokannan. Uusi ryhmä kyselyjä luonnin automatisoida, on artikkelissa [CreateOrUpdateElasticPoolAndPopulate ](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).   

## <a name="example-create-a-pool-using-powershell"></a>Esimerkki: Luo resurssivarantoon PowerShellin avulla 

Tämä komentosarja luo uusi Azure resurssiryhmä ja uuteen palvelimeen. Pyydettäessä annettava järjestelmänvalvojan käyttäjänimi ja uusi (ei Azure tunnistetietojen)-palvelimen salasana.

    $subscriptionId = '<your Azure subscription id>'
    $resourceGroupName = '<resource group name>'
    $location = '<datacenter location>'
    $serverName = '<server name>'
    $poolName = '<pool name>'
    $databaseName = '<database name>'

    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId

    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $serverName -Location $location -ServerVersion "12.0"
    New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $serverName -FirewallRuleName "rule1" -StartIpAddress "192.168.0.198" -EndIpAddress "192.168.0.199"

    New-AzureRmSqlElasticPool -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100

    New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -ElasticPoolName $poolName -MaxSizeBytes 10GB



## <a name="next-steps"></a>Seuraavat vaiheet

- [Oman ryhmän hallinta](sql-database-elastic-pool-manage-powershell.md)
- [Luo joustavasti töitä](sql-database-elastic-jobs-overview.md) Joustavasti työt avulla voit suorittaa T-SQL-komentosarjat altaan tietokantojen määrää.
- [Skaalaa ulos kanssa Azure SQL-tietokanta](sql-database-elastic-scale-introduction.md): Käytä joustavasti Tietokantatyökalut asteikko-kohtaa.

