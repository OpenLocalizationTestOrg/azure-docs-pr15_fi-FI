<properties
    pageTitle="SQL-tietokanta-opetusohjelma: suojauksen käytön aloittaminen"
    description="Lue, miten voit luoda käyttäjätilejä, jos haluat käyttää ja hallita tietokannan."
    keywords=""
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/17/2016"
    ms.author="carlrab"/>

# <a name="sql-database-tutorial-create-sql-database-user-accounts-to-access-and-manage-a-database"></a>SQL-tietokanta-opetusohjelma: käyttäjätilien hallita tietokannan ja käyttää SQL-tietokannan luominen


> [AZURE.SELECTOR]
- [Hae aloittaminen-opetusohjelma](sql-database-get-started-security.md)
- [Käyttöoikeuksien myöntäminen](sql-database-manage-logins.md)

Tässä opetusohjelmassa kerrotaan käyttämisestä SQL Server Management Studiossa (SSMS) avulla:

- Kirjaudu sisään käyttämällä palvelintason pääasiallista kirjautuminen SQL-tietokantaan.
- SQL-tietokanta-käyttäjätilin luominen
- SQL-tietokannan käyttäjien [db_owner käyttöoikeuksien](https://msdn.microsoft.com/library/ms189121.aspx#Anchor_0)myöntäminen
- Yhteyden muodostaminen SQL-tietokantaan, joka ei ole palvelintason lyhennys käyttäjätiliin.

[AZURE.INCLUDE [Login](../../includes/azure-getting-started-portal-login.md)]


[AZURE.INCLUDE [Create SQL Database logical server](../../includes/sql-database-sql-server-management-studio-connect-server-principal.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-database-user.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-grant-database-user-dbo-permissions.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-sql-server-management-studio-connect-user.md)]


## <a name="next-steps"></a>Seuraavat vaiheet
Nyt kun olet valmis SQL-tietokantaan Tässä opetusohjelmassa ja luonut käyttäjätilin ja myöntää käyttäjän tilin dbo käyttöoikeudet, olet valmis lisätietoja [SQL-tietokannan suojaus](sql-database-manage-logins.md).


