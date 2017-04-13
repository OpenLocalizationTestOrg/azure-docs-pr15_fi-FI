<properties
    pageTitle="Yhteyden muodostaminen SQL-tietokantaan käyttämällä .NET (C#) | Microsoft Azure"
    description="Käytä oleva Tämä nopea Käynnistä muodostaa uusi sovellus, jolla C# ja palautettua mukaan tehokkaita relaatiotietokannasta pilveen Azure SQL-tietokantaan."
    services="sql-database"
    documentationCenter=""
    authors="tobbox"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/16/2016"
    ms.author="tobiast"/>

# <a name="connect-to-sql-database-by-using-net-c"></a>Yhteyden muodostaminen SQL-tietokantaan käyttämällä .NET (C#)

[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 

## <a name="step-1--configure-development-environment"></a>Vaihe 1: Määritä kehitysympäristö

[Määritä ADO.NET kehittäminen kehitysympäristö](https://msdn.microsoft.com/library/mt718321.aspx)

## <a name="step-2-create-a-sql-database"></a>Vaihe 2: SQL-tietokannan luominen

Katso [aloittaminen-sivu](sql-database-get-started.md) luominen mallitietokanta.  On tärkeää noudatat luominen **AdventureWorks-tietokantamallin**opas. Alla on esitetty esimerkkejä toimivat vain **AdventureWorks rakenne**.  

## <a name="step-3--get-connection-string"></a>Vaihe 3: Hae yhteysmerkkijonon

[AZURE.INCLUDE [sql-database-include-connection-string-dotnet-20-portalshots](../../includes/sql-database-include-connection-string-dotnet-20-portalshots.md)]

## <a name="step-4-run-sample-code"></a>Vaihe 4: Suorita otoksen koodi

* [Yhteyden muodostaminen SQL käyttämällä ADO.NET käsite kuitti](https://msdn.microsoft.com/library/mt718320.aspx)
* [SQL ADO.NET resiliently yhdistäminen](https://msdn.microsoft.com/library/mt703195.aspx)

## <a name="next-steps"></a>Seuraavat vaiheet

* [ASP.NET-MVC-sovelluksen luominen auth ja SQL DB ja ota käyttöön App Azure-palvelu]( ../app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md)
* Tarkista [SQL-tietokannan kehittäminen yleiskatsaus](sql-database-develop-overview.md)
* Lisätietoja [Microsoft SQL Server ADO.Net-ohjain](https://msdn.microsoft.com/library/mt657768.aspx)

## <a name="additional-resources"></a>Lisäresursseja 

* [Rakenne-kuvioiden määrittäminen usean vuokraajan SaaS sovellusten Azure SQL-tietokanta](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Selaa kaikkia [ominaisuuksia SQL-tietokantaan](https://azure.microsoft.com/services/sql-database/)





