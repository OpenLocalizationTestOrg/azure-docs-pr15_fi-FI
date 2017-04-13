<properties
    pageTitle="Azure SQL-tietokantaan palvelintason ja tietokannan tason palomuurisäännöt käyttämällä T-SQL | Microsoft Azure"
    description="Opettele määrittämään palomuurin IP-osoitteet, jotka käyttävät Azure SQL-tietokannat."
    services="sql-database"
    documentationCenter=""
    authors="BYHAM"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article" 
    ms.date="08/30/2016"
    ms.author="rickbyh"/>


# <a name="configure-azure-sql-database-server-level-and-database-level-firewall-rules-using-t-sql"></a>Määritä käyttämällä T-SQL Azure SQL-tietokanta palvelintason ja tietokannan tason palomuurisäännöt


> [AZURE.SELECTOR]
- [Yleiskatsaus](sql-database-firewall-configure.md)
- [Azure Portal](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShellin](sql-database-configure-firewall-settings-powershell.md)
- [REST-OHJELMOINTIRAJAPINNALLA](sql-database-configure-firewall-settings-rest.md)


Microsoft Azure SQL-tietokanta käyttämällä palomuurisäännöt sallivat palvelimia ja tietokantoja. Voit määrittää palvelintason ja tietokannan tason palomuuriasetukset perusmuodon tai käyttäjän tietokannan Azure SQL-tietokanta palvelimen sallimaan valikoivasti access-tietokantaan.

> [AZURE.IMPORTANT] Azure yhteydet on otettava käyttöön Salli sovellusten Azure-tietokannan palvelimeen. Saat lisätietoja palomuurisäännöt ja Azure ottaminen käyttöön yhteydet [Azure SQL-tietokantaan palomuurin](sql-database-firewall-configure.md). Jos teet Azure cloud reunan sisällä yhteyksiä, saatat joutua avaamaan joitakin muita TCP-portit. Lisätietoja on artikkelissa **Vipuventtiili V12 SQL-tietokantaan: ulkopuolella ja** [portit lisäksi 1433 ADO.NET 4.5 ja SQL-tietokannan Vipuventtiili V12](sql-database-develop-direct-route-ports-adonet-v12.md) -osa


## <a name="server-level-firewall-rules"></a>Palvelintason palomuurisäännöt

Vain palvelintason pääasiallista kirjautumistunnus tai Azure Active Directory-järjestelmänvalvojan voit luoda palvelintason palomuurisäännön avulla Transact-SQL.

1. Käynnistä kysely-ikkunassa ja virtual master-tietokantayhteyden muodostamisessa SQL Server Management Studiossa avulla.
2. Palvelintason palomuurisäännöt olla valittuna, luoda, päivittää tai kysely-ikkunassa poistetaan.
3. Luo tai päivitä palvelintason palomuurisäännöt, suorita `sp_set_firewall_rule` tallennetun toimintosarjan. Seuraavassa esimerkissä mahdollistaa alueen IP-osoitteiden Contoso-palvelimeen.<br/>Aloita tekemällä näe, mitä sääntöjä jo olemassa.

        SELECT * FROM sys.firewall_rules ORDER BY name;

    Lisää palomuurisäännön.

        EXECUTE sp_set_firewall_rule @name = N'ContosoFirewallRule',
            @start_ip_address = '192.168.1.1', @end_ip_address = '192.168.1.10'

    Voit poistaa palvelintason palomuurisäännön, suorita sp_delete_firewall_rule tallennetun toimintosarjan. Seuraavassa esimerkissä poistaa säännön nimeltä ContosoFirewallRule.
 
        EXECUTE sp_delete_firewall_rule @name = N'ContosoFirewallRule'
 
 Saat lisätietoja näiden tallennettujen toimintosarjojen [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) ja [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx).

## <a name="database-level-firewall-rules"></a>Tietokannan tason palomuurisäännöt

Vain tietokannan käyttäjä, jolla on **oikeudet** -käyttöoikeuksia tietokannan (kuten tietokannan omistaja) voit luoda tietokantaan tason palomuurisäännön.

1. Kun olet luonut palvelintason palomuuri IP-osoite, Käynnistä kyselyikkunan Classic-portaalin tai SQL Server Management Studiossa kautta.
2. Muodosta yhteys tietokantaan, jonka haluat luoda palomuurisäännön, tietokannan tason.

    Voit luoda uuden tai päivittää nykyisen tietokannan tason palomuurisäännön, suorita `sp_set_database_firewall_rule` tallennettu toimintosarja. Seuraavassa esimerkissä luodaan uusi palomuurisääntö nimeltä ContosoFirewallRule.
 
        EXEC sp_set_database_firewall_rule @name = N'ContosoFirewallRule', 
            @start_ip_address = '192.168.1.11', @end_ip_address = '192.168.1.11'
 
    Jos haluat poistaa nykyisen tietokannan tason palomuurisäännön, suorita `sp_delete_database_firewall_rule` tallennetun toimintosarjan. Seuraavassa esimerkissä poistaa säännön nimeltä ContosoFirewallRule.
`
   SUORITUS sp_delete_database_firewall_rule @name = N'ContosoFirewallRule'

Saat lisätietoja näiden tallennettujen toimintosarjojen [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) ja [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx).

## <a name="next-steps"></a>Seuraavat vaiheet

Miten artikkeleihin luomisesta palvelintason palomuurisäännöt muiden kautta, on artikkelissa: 

- [Määritä Azure SQL-tietokanta palvelintason palomuurisäännöt Azure-portaalissa](sql-database-configure-firewall-settings.md)
- [Määritä Azure SQL-tietokanta palvelintason palomuurisäännöt PowerShellin avulla](sql-database-configure-firewall-settings-powershell.md)
- [Määritä Azure SQL-tietokanta palvelintason palomuurisäännöt REST-Ohjelmointirajapinnan käyttäminen](sql-database-configure-firewall-settings-rest.md)

Opetusohjelmaan-tietokannan luominen-kohdassa [luominen Azure-portaalissa minuutteina SQL-tietokantaan](sql-database-get-started.md).
Lisätietoja yhteyden muodostamisesta Azure SQL-tietokanta Avaa lähde- tai kolmansien osapuolien sovellukset on [asiakkaan Pika-aloitus MALLIKOODEJA SQL-tietokantaan](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Siirtyminen tietokantoja on artikkelissa [tietokannan käytön ja kirjaudu sisään tietoturvan hallintaa](https://msdn.microsoft.com/library/azure/ee336235.aspx).


## <a name="additional-resources"></a>Lisäresursseja

- [Tietokannan suojaaminen](sql-database-security.md)
- [Tietoturvakeskus SQL Server-tietokantamoduuli ja Azure SQL-tietokanta](https://msdn.microsoft.com/library/bb510589)
