<properties
    pageTitle="Azure SQL-tietokantaan palvelintason palomuurisäännöt REST-Ohjelmointirajapinnan käyttäminen | Microsoft Azure"
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


#  <a name="configure-azure-sql-database-server-level-firewall-rules-using-the-rest-api"></a>Määritä Azure SQL-tietokanta palvelintason palomuurisäännöt REST-Ohjelmointirajapinnan käyttäminen


> [AZURE.SELECTOR]
- [Yleiskatsaus](sql-database-firewall-configure.md)
- [Azure portal](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShellin](sql-database-configure-firewall-settings-powershell.md)
- [REST-OHJELMOINTIRAJAPINNALLA](sql-database-configure-firewall-settings-rest.md)


Microsoft Azure SQL-tietokanta käyttämällä palomuurisäännöt sallivat palvelimia ja tietokantoja. Voit määrittää palvelintason ja tietokannan tason palomuuriasetukset perusmuodon tai käyttäjän tietokannan Azure SQL-tietokanta palvelimen sallimaan valikoivasti access-tietokantaan.

> [AZURE.IMPORTANT] Azure yhteydet on otettava käyttöön Salli sovellusten Azure-tietokannan palvelimeen. Saat lisätietoja palomuurisäännöt ja Azure ottaminen käyttöön yhteydet [Azure SQL-tietokantaan palomuurin](sql-database-firewall-configure.md). Jos teet Azure cloud reunan sisällä yhteyksiä, saatat joutua avaamaan joitakin muita TCP-portit. Lisätietoja on artikkelissa **Vipuventtiili V12 SQL-tietokantaan: ulkopuolella ja** [portit lisäksi 1433 ADO.NET 4.5 ja SQL-tietokannan Vipuventtiili V12](sql-database-develop-direct-route-ports-adonet-v12.md) -osa


## <a name="manage-server-level-firewall-rules-through-rest-api"></a>Palvelintason läpi REST API Palomuurisääntöjen hallinta
1. Palomuurisäännöt kautta REST API hallinta täytyy todentaa. Lisätietoja on artikkelissa [Kehittäjän ohje luvan Azure Resurssienhallinta-Ohjelmointirajapinnan kanssa](../resource-manager-api-authentication.md).
2. Palvelintason säännöt voidaan luoda, päivitetyn tai poistetut REST-Ohjelmointirajapinnan käyttäminen

    Luo tai päivitä palvelintason palomuurisäännön, suorita HYLLYTETTY menetelmän avulla seuraavasti:
 
        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/firewallRules/{rule-name}?api-version={api-version}
    
    Pyydä tekstissä

        {
         "properties": { 
            "startIpAddress": "{start-ip-address}", 
            "endIpAddress": "{end-ip-address}
            }
        } 
 

    Jos haluat poistaa nykyisen palvelintason palomuurisäännön, suorita DELETE-menetelmä avulla seuraavasti:
     
        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/firewallRules/{rule-name}?api-version={api-version}


## <a name="manage-firewall-rules-using-the-rest-api"></a>Hallitse palomuurisäännöt REST-Ohjelmointirajapinnan käyttäminen

* [Luo tai päivitä palomuurisääntö](https://msdn.microsoft.com/library/azure/mt445501.aspx)
* [Palomuurisäännön poistaminen](https://msdn.microsoft.com/library/azure/mt445502.aspx)
* [Hae palomuurisääntö](https://msdn.microsoft.com/library/azure/mt445503.aspx)
* [Luettele kaikki palomuurisäännöt](https://msdn.microsoft.com/library/azure/mt604478.aspx)
 
## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja artikkelissa käyttämisestä Transact-SQL server tason ja tietokannan tason palomuurisäännöt luomisesta on ohjeaiheessa [määrittäminen Azure SQL-tietokanta palvelintason ja tietokannan tason palomuurisäännöt käyttämällä T-SQL](sql-database-configure-firewall-settings-tsql.md). 

Miten artikkeleihin luomisesta palvelintason palomuurisäännöt muiden kautta, on artikkelissa: 

- [Määritä Azure SQL-tietokanta palvelintason palomuurisäännöt Azure-portaalissa](sql-database-configure-firewall-settings.md)
- [Määritä Azure SQL-tietokanta palvelintason palomuurisäännöt PowerShellin avulla](sql-database-configure-firewall-settings-powershell.md)

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

 
