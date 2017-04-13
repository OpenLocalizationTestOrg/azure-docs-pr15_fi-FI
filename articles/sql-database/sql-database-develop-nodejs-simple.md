<properties
    pageTitle="Yhteyden muodostaminen SQL-tietokantaan Node.js avulla | Microsoft Azure"
    description="Esittää Node.js koodin otoksen avulla voit muodostaa yhteyden Azure SQL-tietokantaan."
    services="sql-database"
    documentationCenter=""
    authors="meet-bhagdev"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="meetb"/>

# <a name="connect-to-sql-database-by-using-nodejs"></a>Yhteyden muodostaminen SQL-tietokantaan Node.js avulla

[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 

Tässä ohjeaiheessa esitellään, miten yhteyden ja kyselyjen avulla Node.js Azure SQL-tietokanta. Voit suorittaa tässä esimerkissä Windows-, Ubuntu Linux- tai Mac-ympäristöissä.

## <a name="step-1-configure-development-environment"></a>Vaihe 1: Määritä kehitysympäristö

[Edellytyksistä vaivalloista Node.js ohjaimen avulla SQL Server](https://msdn.microsoft.com/library/mt652094.aspx)

## <a name="step-2-create-a-sql-database"></a>Vaihe 2: SQL-tietokannan luominen

Katso [aloittaminen-sivu](sql-database-get-started.md) luominen mallitietokanta.  On tärkeää noudatat luominen **AdventureWorks-tietokantamallin**opas. Alla on esitetty esimerkkejä toimivat vain **AdventureWorks rakenne**.

## <a name="step-3-get-connection-details"></a>Vaihe 3: Hae yhteystiedot

[AZURE.INCLUDE [sql-database-include-connection-string-details-20-portalshots](../../includes/sql-database-include-connection-string-details-20-portalshots.md)]

## <a name="step-4-run-sample-code"></a>Vaihe 4: Suorita otoksen koodi

[Yhteyden muodostaminen SQL käyttämällä Node.js käsite kuitti](https://msdn.microsoft.com/library/mt715784.aspx)

## <a name="next-steps"></a>Seuraavat vaiheet

* Tarkista [SQL-tietokannan kehittäminen yleiskatsaus](sql-database-develop-overview.md)
* Lisätietoja [Microsoft SQL Server Node.js-ohjain](https://msdn.microsoft.com/library/mt652093.aspx)

## <a name="additional-resources"></a>Lisäresursseja 

* [Rakenne-kuvioiden määrittäminen usean vuokraajan SaaS sovellusten Azure SQL-tietokanta](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Selaa kaikkia [ominaisuuksia SQL-tietokantaan](https://azure.microsoft.com/services/sql-database/)
