<properties
    pageTitle="Uudet SQL-tietokanta-asetukset PowerShellin | Microsoft Azure"
    description="Opi nyt SQL-tietokannan luominen PowerShellin avulla. Tietokannan asetukset perustehtäviä voit hallitaan PowerShellin cmdlet-komennot."
    keywords="Luo uusi tietokanta sql-tietokanta-asetukset"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="hero-article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="08/19/2016"
    ms.author="sstein"/>

# <a name="create-a-sql-database-and-perform-common-database-setup-tasks-with-powershell-cmdlets"></a>SQL-tietokannan luominen ja suorittaa yleisiä tietokannan suorittamalla PowerShell cmdlet-komennot


> [AZURE.SELECTOR]
- [Azure portal](sql-database-get-started.md)
- [PowerShellin](sql-database-get-started-powershell.md)
- [C#](sql-database-get-started-csharp.md)



Lue, miten voit luoda SQL-tietokanta käyttämällä PowerShell cmdlet-komennot. (Joustavasti tietokantojen luominen näkyvät [Luo uusi joustavasti tietokannan ryhmä PowerShellin avulla](sql-database-elastic-pool-create-powershell.md).)


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="database-setup-create-a-resource-group-server-and-firewall-rule"></a>Tietokanta-asetukset: resurssiryhmä, palvelimen ja palomuurisäännön luominen

Kun access suorittaa valitun Azure tilauksen vastaan cmdlet-komennot, seuraava vaihe on vahvistamiseksi resurssiryhmän, joka sisältää palvelin, johon tietokanta luodaan. Voit muokata käyttämään jostakin kelvollinen sijainti seuraava komento, voit valita. Suorita **(Hae AzureRmLocation | WHERE-objektin {$_. Toimittajat - eq "Microsoft.Sql"}). Sijainti** saat kelvollinen sijaintien luettelon.

Suorita seuraava komento luoda resurssiryhmän:

    New-AzureRmResourceGroup -Name "resourcegroupsqlgsps" -Location "westus"


### <a name="create-a-server"></a>Palvelimen luominen

SQL-tietokantoja luodaan sisällä Azure SQL-tietokanta-palvelimiin. Suorita [uusi-AzureRmSqlServer] (https://msdn.microsoft.com/library/azure/mt603715(v=azure.300\).aspx) luominen palvelimeen. Palvelimen nimen on oltava yksilöllinen kaikki Azure SQL-tietokanta-palvelimiin. Jos palvelimen nimi on jo, järjestelmä ilmoittaa virheestä. Myös nykyarvo katsomalla on tämä komento voi kestää useita minuutteja. Voit muokata komentoa käyttämään mikä tahansa kelvollinen sijainti, voit valita, mutta sinun on käytettävä edellisessä vaiheessa luotu resurssiryhmä käytettäviä samaan sijaintiin.

    New-AzureRmSqlServer -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -Location "westus" -ServerVersion "12.0"

Kun suoritat tämän komennon, ohjelma pyytää käyttäjänimeä ja salasanaa. Älä kirjoita Azure tunnistetiedot. Kirjoita käyttäjänimi ja salasana, palvelimen järjestelmänvalvojan luominen sen sijaan. Tässä artikkelissa alareunassa komentosarja näyttää palvelimen määrittämisestä koodi.

Palvelimen tiedot näkyviin, kun palvelin on luotu.

### <a name="configure-a-server-firewall-rule-to-allow-access-to-the-server"></a>Määritä palvelimen palomuurisääntö, joka sallii palvelimen käyttöoikeus

Palvelimen käyttämiseen, sinun täytyy vahvistaa palomuurisäännön. Suorita [uusi-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx)-komento, alkamis- ja IP-osoitteiden tilalle tietokoneen kelvollisia arvoja.

    New-AzureRmSqlServerFirewallRule -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -FirewallRuleName "rule1" -StartIpAddress "192.168.0.0" -EndIpAddress "192.168.0.0"

Palomuurin säännön tiedot näkyviin, kun sääntö on luotu.

Jos Azure muiden palvelinta, Lisää palomuurisääntö ja StartIpAddress ja EndIpAddress arvoksi 0.0.0.0. Tämä sääntö sallii Azure tietoliikenteen jokin Azure tilaus yhteyden palvelimeen.

Lisätietoja on artikkelissa [Azure SQL-tietokantaan palomuurin](sql-database-firewall-configure.md).


## <a name="create-a-sql-database"></a>SQL-tietokannan luominen

Nyt käytössäsi on resurssiryhmä palvelimeen ja määritetty, voit käyttää palvelimen palomuurisäännön.

Seuraavat [uusi AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt619339(v=azure.300\).aspx) komento luo vakiorivejä tason S1-suorituskyvyn viereiset (tyhjä) SQL-tietokantaan:


    New-AzureRmSqlDatabase -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -DatabaseName "database1" -Edition "Standard" -RequestedServiceObjectiveName "S1"


Tietokannan tiedot näkyviin, kun tietokanta on luotu.

## <a name="create-a-sql-database-powershell-script"></a>SQL-tietokannan PowerShell-komentosarjaa luominen

Seuraavaa PowerShell-komentosarjaa luo SQL-tietokantaan ja kaikki sen riippuvaiset resurssit. Korvaa kaikki `{variables}` arvoilla tilaus ja resurssit (Poista **{}** , kun määrität arvot).

    # Sign in to Azure and set the subscription to work with
    $SubscriptionId = "{subscription-id}"

    Add-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $SubscriptionId

    # CREATE A RESOURCE GROUP
    $resourceGroupName = "{group-name}"
    $rglocation = "{Azure-region}"
    
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $rglocation
    
    # CREATE A SERVER
    $serverName = "{server-name}"
    $serverVersion = "12.0"
    $serverLocation = "{Azure-region}"
    
    $serverAdmin = "{server-admin}"
    $serverPassword = "{server-password}" 
    $securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
    $serverCreds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword
    
    $sqlDbServer = New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $serverName -Location $serverLocation -ServerVersion $serverVersion -SqlAdministratorCredentials $serverCreds
    
    # CREATE A SERVER FIREWALL RULE
    $ip = (Test-Connection -ComputerName $env:COMPUTERNAME -Count 1 -Verbose).IPV4Address.IPAddressToString
    $firewallRuleName = '{rule-name}'
    $firewallStartIp = $ip
    $firewallEndIp = $ip
    
    $fireWallRule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $serverName -FirewallRuleName $firewallRuleName -StartIpAddress $firewallStartIp -EndIpAddress $firewallEndIp
    
    
    # CREATE A SQL DATABASE
    $databaseName = "{database-name}"
    $databaseEdition = "{Standard}"
    $databaseSlo = "{S0}"
    
    $sqlDatabase = New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -Edition $databaseEdition -RequestedServiceObjectiveName $databaseSlo
    
   
    # REMOVE ALL RESOURCES THE SCRIPT JUST CREATED
    #Remove-AzureRmResourceGroup -Name $resourceGroupName






## <a name="next-steps"></a>Seuraavat vaiheet
Kun olet luonut SQL-tietokantaan ja suorittaa perustiedot tietokannasta, voit ryhtyä seuraavat asiat:

- [SQL-tietokannan PowerShellin hallinta](sql-database-manage-powershell.md)
- [Yhteyden muodostaminen SQL-tietokantaa, jossa SQL Server Management Studiossa ja suorittaa otoksen T-SQL-kysely](sql-database-connect-query-ssms.md)


## <a name="additional-resources"></a>Lisäresursseja

- [Azure SQL-tietokannan cmdlet] (https://msdn.microsoft.com/library/azure/mt574084(v=azure.300\).aspx)
- [Azure SQL-tietokanta](https://azure.microsoft.com/documentation/services/sql-database/)
