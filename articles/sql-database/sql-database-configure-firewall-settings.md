<properties
    pageTitle="Määritä SQL-tietokanta-palvelintason palomuurisäännön | Microsoft Azure"
    description="Opettele määrittämään palomuurin IP-osoitteet, jotka käyttävät Azure SQL server."
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
    ms.author="rickbyh;carlrab"/>


# <a name="configure-an-azure-sql-database-server-level-firewall-rule-using-the-azure-portal"></a>Määritä saapuvan Azure SQL-tietokanta palvelintason palomuurisääntö Azure-portaalissa


> [AZURE.SELECTOR]
- [Yleiskatsaus](sql-database-firewall-configure.md)
- [Azure Portal](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShellin](sql-database-configure-firewall-settings-powershell.md)
- [REST-OHJELMOINTIRAJAPINNALLA](sql-database-configure-firewall-settings-rest.md)

Azure SQL server käyttää palomuurisäännöt sallivat palvelimia ja tietokantoja. Voit määrittää palvelintason ja tietokannan tason palomuuriasetukset perusmuodon tai käyttäjän tietokannan Azure SQL Serverin palvelimen sallimaan valikoivasti pääsy tietokantaan looginen. Tässä ohjeaiheessa kerrotaan palvelintason palomuurisäännöt.

> [AZURE.IMPORTANT] Azure yhteydet on otettava käyttöön Salli sovellusten azuren Azure SQL-palvelimeen. Miten palomuurin säännöt työ on artikkelissa [Azure SQL-palvelimeen palomuurin määrittämisestä \- yleiskatsaus](sql-database-firewall-configure.md). Jos teet Azure cloud reunan sisällä yhteyksiä, saatat joutua avaamaan joitakin muita TCP-portit. Lisätietoja on artikkelissa **Vipuventtiili V12 SQL-tietokantaan: ulkopuolella ja** [portit lisäksi 1433 ADO.NET 4.5 ja SQL-tietokannan Vipuventtiili V12](sql-database-develop-direct-route-ports-adonet-v12.md) -osa

**Suositus:** Käytä palvelintason palomuurisäännöt järjestelmänvalvojille ja, kun sinulla on useita tietokantoja, joissa on samat access-vaatimukset ja joita et halua käyttää aikaa määrittäminen tietokantojen yksitellen. Microsoft suosittelee käyttämällä tietokannan tason palomuurisäännöt aina, kun se on mahdollista, tietoturvan ja tehdä tietokannan paremmin siirrettäviä.

[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a>Hallitse olemassa palvelintason palomuurisäännöt palvelun Azure-portaalissa

Toista vaiheet hallittavan palvelintason palomuurisäännöt.

- Jos haluat lisätä nykyiseen tietokoneeseen, valitse Lisää asiakkaan IP.
- Jos haluat lisätä muita IP-osoitteet, kirjoita säännön nimi, Käynnistä IP-osoite ja Lopeta IP-osoite.
- Jos haluat muokata aiemmin luotua sääntöä, valitse säännön kenttiä ja Muokkaa.
- Jos haluat poistaa aiemmin luotua sääntöä, Vie osoitin säännön X näkyy rivin lopussa. Valitse Poista sääntö X.

Valitse Tallenna muutokset **Tallenna** .

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja artikkelissa käyttämisestä Transact-SQL server tason ja tietokannan tason palomuurisäännöt luomisesta on ohjeaiheessa [määrittäminen Azure SQL-tietokanta palvelintason ja tietokannan tason palomuurisäännöt käyttämällä T-SQL](sql-database-configure-firewall-settings-tsql.md). 

Miten artikkeleihin luomisesta palvelintason palomuurisäännöt muiden kautta, on artikkelissa: 

- [Määritä Azure SQL-tietokanta palvelintason palomuurisäännöt PowerShellin avulla](sql-database-configure-firewall-settings-powershell.md)
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

 
