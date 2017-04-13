<properties
    pageTitle="Hallinta PowerShellin Azure SQL-tietokanta | Microsoft Azure"
    description="Azure SQL-tietokannan hallinta PowerShellin avulla."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="sstein"/>

# <a name="manage-azure-sql-database-with-powershell"></a>Hallinta PowerShellin Azure SQL-tietokanta


> [AZURE.SELECTOR]
- [Azure portal](sql-database-manage-portal.md)
- [Transact-SQL (SSMS)](sql-database-manage-azure-ssms.md)
- [PowerShellin](sql-database-manage-powershell.md)

Tässä ohjeaiheessa esitellään PowerShellin cmdlet-komennot, joita käytetään monta Azure SQL-tietokanta tehtävien suorittamiseen. Täydellinen luettelo on artikkelissa [Azure SQL-tietokannan cmdlet-komennot](https://msdn.microsoft.com/library/mt574084.aspx).


## <a name="create-a-resource-group"></a>Resurssiryhmän luominen

Luoda resurssiryhmä sekä SQL-tietokantaan ja liittyvät Azure resurssit ja sitten [Uusi AzureRmResourceGroup](https://msdn.microsoft.com/library/azure/mt759837.aspx) cmdlet-komennon.

```
$resourceGroupName = "resourcegroup1"
$resourceGroupLocation = "northcentralus"
New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
```

Lisätietoja on artikkelissa [Azure PowerShellin Azure resurssien hallinta](../powershell-azure-resource-manager.md).
Katso esimerkki-komentosarja [Luo SQL-tietokanta PowerShell-komentosarjaa](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).

## <a name="create-a-sql-database-server"></a>Luo SQL-tietokantapalvelinta

Luo [Uusi AzureRmSqlServer](https://msdn.microsoft.com/library/azure/mt603715.aspx) cmdlet-komento SQL-tietokantapalvelimeen. Korvaa *palvelin1* palvelimesi nimi. Palvelimen nimi on oltava yksilöllinen palvelimilta Azure SQL-tietokantaan. Jos palvelimen nimi on jo, järjestelmä ilmoittaa virheestä. Tämä komento voi kestää useita minuutteja. Tilauksesi on oltava olemassa resurssiryhmän.

```
$resourceGroupName = "resourcegroup1"

$sqlServerName = "server1"
$sqlServerVersion = "12.0"
$sqlServerLocation = "northcentralus"
$serverAdmin = "loginname"
$serverPassword = "password" 
$securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
$creds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword
    

$sqlServer = New-AzureRmSqlServer -ServerName $sqlServerName `
 -SqlAdministratorCredentials $creds -Location $sqlServerLocation `
 -ResourceGroupName $resourceGroupName -ServerVersion $sqlServerVersion
```

Lisätietoja on artikkelissa [Mikä on SQL-tietokantaan](sql-database-technical-overview.md). Katso esimerkki-komentosarja [Luo SQL-tietokanta PowerShell-komentosarjaa](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).


## <a name="create-a-sql-database-server-firewall-rule"></a>SQL-tietokanta palvelimen palomuurisäännön luominen

Voit käyttää [Uutta AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603860.aspx) cmdlet-komennon palvelimeen palomuurisäännön luominen Suorita seuraava komento, alkamis- ja IP-osoitteiden tilalle kelvollisia arvoja asiakkaan. Resurssiryhmä ja server on jo olemassa-tilaukseesi.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$firewallRuleName = "firewallrule1"
$firewallStartIp = "0.0.0.0"
$firewallEndIp = "255.255.255.255"

New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -FirewallRuleName $firewallRuleName `
 -StartIpAddress $firewallStartIp -EndIpAddress $firewallEndIp
```

Jos muut Azure palveluiden käyttöoikeus, palomuurisäännön luominen ja määrittäminen sekä `-StartIpAddress` ja `-EndIpAddress` **0.0.0.0**. Erityistä tämän palomuurisäännön avulla kaikki Azure liikenne yhteyden palvelimeen.

Lisätietoja on artikkelissa [Azure SQL-tietokantaan palomuurin](https://msdn.microsoft.com/library/azure/ee621782.aspx). Katso esimerkki-komentosarja [Luo SQL-tietokanta PowerShell-komentosarjaa](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).


## <a name="create-a-sql-database-blank"></a>Luo SQL-tietokanta (tyhjä)

Luo tietokanta ja [Uusi AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619339.aspx) cmdlet-komento. Resurssiryhmä ja server on jo olemassa-tilaukseesi. 

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$databaseName = "database1"
$databaseEdition = "Standard"
$databaseServiceLevel = "S0"

$currentDatabase = New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -DatabaseName $databaseName `
 -Edition $databaseEdition -RequestedServiceObjectiveName $databaseServiceLevel
```

Lisätietoja on artikkelissa [Mikä on SQL-tietokantaan](sql-database-technical-overview.md). Katso esimerkki-komentosarja [Luo SQL-tietokanta PowerShell-komentosarjaa](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).


## <a name="change-the-performance-level-of-a-sql-database"></a>SQL-tietokannan suorituskykyä tason muuttaminen

Skaalaa tietokannan ylös tai alas [Määrittäminen AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619433.aspx) cmdlet-komennon avulla. Resurssiryhmä-palvelin ja tietokanta on oltava olemassa-tilaukseesi. Määritä `-RequestedServiceObjectiveName` Basic taso riviväliksi (esimerkiksi seuraavat koodikatkelman). Määritä *S0*, *S1*, *P1*, *6*, jne., kuten edellisessä esimerkissä, muut tasoa.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$databaseName = "database1"
$databaseEdition = "Basic"
$databaseServiceLevel = " "

Set-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -DatabaseName $databaseName `
 -Edition $databaseEdition -RequestedServiceObjectiveName $databaseServiceLevel
```

Lisätietoja on artikkelissa [SQL-tietokanta-asetukset ja suorituskyky: ymmärtää, mikä on käytettävissä palvelun kunkin tason](sql-database-service-tiers.md). Esimerkki-komentosarja on [Esimerkki PowerShell-komentosarja SQL-tietokantaan palvelun taso ja suorituskyvyn tason muuttaminen](sql-database-scale-up-powershell.md#sample-powershell-script-to-change-the-service-tier-and-performance-level-of-your-sql-database).

## <a name="copy-a-sql-database-to-the-same-server"></a>Kopioi SQL-tietokanta-palvelimeen

Kopioi SQL-tietokanta-palvelimeen, [Uusi AzureRmSqlDatabaseCopy](https://msdn.microsoft.com/library/azure/mt603644.aspx) cmdlet-komennon avulla. Määritä `-CopyServerName` ja `-CopyResourceGroupName` , samat arvot kuin lähteen tietokannan palvelimen ja resurssin ryhmä.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"
$databaseName = "database1"

$copyDatabaseName = "database1_copy"
$copyServerName = $sqlServerName
$copyResourceGroupName = $resourceGroupName

New-AzureRmSqlDatabaseCopy -DatabaseName $databaseName `
 -ServerName $sqlServerName -ResourceGroupName $resourceGroupName `
 -CopyDatabaseName $copyDatabaseName -CopyServerName $sqlServerName `
 -CopyResourceGroupName $copyResourceGroupName
```

Lisätietoja on artikkelissa [kopioida Azure SQL-tietokantaan](sql-database-copy.md). Katso esimerkki-komentosarja [Kopioi SQL-tietokanta PowerShell-komentosarjaa](sql-database-copy-powershell.md#example-powershell-script).


## <a name="delete-a-sql-database"></a>SQL-tietokannan poistaminen

Poista SQL-tietokanta ja [Poista AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619368.aspx) cmdlet-komento. Resurssiryhmä-palvelin ja tietokanta on oltava olemassa-tilaukseesi.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"
$databaseName = "database1"

Remove-AzureRmSqlDatabase -DatabaseName $databaseName `
 -ServerName $sqlServerName -ResourceGroupName $resourceGroupName
```

## <a name="delete-a-sql-database-server"></a>Poista SQL-tietokantapalvelinta

Poista palvelimeen, jossa [Poista AzureRmSqlServer](https://msdn.microsoft.com/library/azure/mt603488.aspx) cmdlet-komento.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

Remove-AzureRmSqlServer -ServerName $sqlServerName -ResourceGroupName $resourceGroupName
```

## <a name="create-and-manage-elastic-database-pools-using-powershell"></a>Voit luoda ja hallita joustavasti tietokannan jakavat PowerShellin avulla

Lisätietoja PowerShellin joustavasti tietokannan jakavat luomisesta on artikkelissa [Luo uusi joustavasti tietokannan ryhmä PowerShellin avulla](sql-database-elastic-pool-create-powershell.md).

Lisätietoja hallitsemisesta joustavasti tietokannan jakavat PowerShellin avulla on artikkelissa [näyttö ja hallita joustavasti tietokanta-ryhmän PowerShellin](sql-database-elastic-pool-manage-powershell.md).



## <a name="related-information"></a>Muita aiheeseen liittyviä tietoja

- [Azure SQL-tietokannan cmdlet-komennot](https://msdn.microsoft.com/library/azure/mt574084.aspx)
- [Azure Cmdlet-viittaus](https://msdn.microsoft.com/library/azure/dn708514.aspx)
