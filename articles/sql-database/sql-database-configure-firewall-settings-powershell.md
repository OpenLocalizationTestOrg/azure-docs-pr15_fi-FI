<properties
    pageTitle="Määritä Azure SQL-tietokanta palvelintason palomuurisäännöt PowerShellin avulla | Microsoft Azure"
    description="Opettele määrittämään palomuurin IP-osoitteet, jotka käyttävät Azure SQL-tietokannat."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/09/2016"
    ms.author="sstein"/>


# <a name="configure-azure-sql-database-server-level-firewall-rules-by-using-powershell"></a>Määritä Azure SQL-tietokanta palvelintason palomuurisäännöt PowerShell-toiminnolla


> [AZURE.SELECTOR]
- [Yleiskatsaus](sql-database-firewall-configure.md)
- [Azure portal](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShellin](sql-database-configure-firewall-settings-powershell.md)
- [REST-OHJELMOINTIRAJAPINNALLA](sql-database-configure-firewall-settings-rest.md)


Azure SQL-tietokanta käyttämällä palomuurisäännöt sallivat palvelimia ja tietokantoja. Voit määrittää palvelintason ja tietokannan tason palomuuriasetukset perusmuodon tietokannan tai käyttäjän tietokannan SQL-tietokanta palvelimen sallimaan valikoivasti access-tietokantaan.

> [AZURE.IMPORTANT] Azure yhteydet on otettava käyttöön Salli sovellusten Azure-tietokannan palvelimeen. Saat lisätietoja palomuurisäännöt ja Azure ottaminen käyttöön yhteydet [Azure SQL-tietokantaan palomuurin](sql-database-firewall-configure.md). Jos teet Azure cloud reunan sisällä yhteyksiä, saatat joutua avaamaan joitakin muita TCP-portit. Lisätietoja on artikkelissa "Vipuventtiili V12 SQL-tietokannan: ulkopuolella ja" [portit lisäksi 1433 ADO.NET 4.5 ja SQL-tietokannan Vipuventtiili V12](sql-database-develop-direct-route-ports-adonet-v12.md)-osassa.


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="create-server-firewall-rules"></a>Luo palvelimen palomuurisäännöt

Palvelintason palomuurin säännöt voidaan luoda, päivittää ja poistaa Azure PowerShell-toiminnolla.

Palvelintason uuden palomuurisäännön luominen, suorittaminen [uusi-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx) cmdlet-komento. Seuraavassa esimerkissä mahdollistaa alueen IP-osoitteiden Contoso-palvelimeen.

    New-AzureRmSqlServerFirewallRule -ResourceGroupName 'resourcegroup1' -ServerName 'Contoso' -FirewallRuleName "ContosoFirewallRule" -StartIpAddress '192.168.1.1' -EndIpAddress '192.168.1.10'       

Voit muokata aiemmin luotua palvelintason palomuurisääntöä, suorittaminen [Set-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603789(v=azure.300\).aspx) cmdlet-komento. Seuraavassa esimerkissä muuttuu hyväksyttävä IP-osoitteiden nimeltä ContosoFirewallRule säännön solualue.

    Set-AzureRmSqlServerFirewallRule -ResourceGroupName 'resourcegroup1' –StartIPAddress 192.168.1.4 –EndIPAddress 192.168.1.10 –RuleName 'ContosoFirewallRule' –ServerName 'Contoso'

Jos haluat poistaa nykyisen palvelintason palomuurisäännön, suorittaminen [Poista-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603588(v=azure.300\).aspx) cmdlet-komento. Seuraavassa esimerkissä poistaa säännön nimeltä ContosoFirewallRule.

    Remove-AzureRmSqlServerFirewallRule –RuleName 'ContosoFirewallRule' –ServerName 'Contoso'


## <a name="manage-firewall-rules-by-using-powershell"></a>Palomuurisäännöt hallinta PowerShellin avulla

PowerShellin avulla voit hallita palomuurisäännöt. Lisätietoja on seuraavissa artikkeleissa:

* [Uusi AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx)
* [AzureRmSqlServerFirewallRule Poista] (https://msdn.microsoft.com/library/azure/mt603588(v=azure.300\).aspx)
* [AzureRmSqlServerFirewallRule määrittäminen] (https://msdn.microsoft.com/library/azure/mt603789(v=azure.300\).aspx)
* [Get-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603586(v=azure.300\).aspx)


## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja käyttämisestä Transact-SQL luotava palvelintason ja tietokannan tason palomuurisäännöt artikkelissa [määrittäminen Azure SQL-tietokanta palvelintason ja tietokannan tason palomuurisäännöt käyttämällä T-SQL](sql-database-configure-firewall-settings-tsql.md).

Lisätietoja palvelintason palomuurisäännöt jollakin toisella tavalla luomisesta on artikkelissa:

- [Määritä Azure SQL-tietokanta palvelintason palomuurisäännöt Azure-portaalissa](sql-database-configure-firewall-settings.md)
- [Määritä Azure SQL-tietokanta palvelintason palomuurisäännöt REST-Ohjelmointirajapinnan käyttäminen](sql-database-configure-firewall-settings-rest.md)

Opetusohjelmaan-tietokannan luominen-kohdassa [luominen Azure-portaalissa minuutteina SQL-tietokantaan](sql-database-get-started.md).
Lisätietoja yhteyden muodostamisesta Azure SQL-tietokanta Avaa lähde- tai kolmansien osapuolien sovellukset on [asiakkaan Pika-aloitus MALLIKOODEJA SQL-tietokantaan](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Siirtyminen tietokantoja on artikkelissa [tietokannan käytön ja kirjaudu sisään tietoturvan hallintaa](https://msdn.microsoft.com/library/azure/ee336235.aspx).


## <a name="additional-resources"></a>Lisäresursseja

- [Tietokannan suojaaminen](sql-database-security.md)
- [Tietoturvakeskus SQL Server-tietokantamoduuli ja Azure SQL-tietokanta](https://msdn.microsoft.com/library/bb510589)


<!--Image references-->
[1]: ./media/sql-database-configure-firewall-settings/AzurePortalBrowseForFirewall.png
[2]: ./media/sql-database-configure-firewall-settings/AzurePortalFirewallSettings.png
<!--anchors-->
