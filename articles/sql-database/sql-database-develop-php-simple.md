<properties
    pageTitle="Muodosta yhteys SQL-tietokantaan käyttämällä Windows PHP | Microsoft Azure"
    description="Esittää otoksen PHP ohjelmaan, joka yhdistää asiakaskoneesta Windows Azure SQL-tietokanta ja linkkejä asiakkaan tarvitsemia tarvittavat ohjelmisto-osiin."
    services="sql-database"
    documentationCenter=""
    authors="meet-bhagdev"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="php"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="meetb"/>


# <a name="connect-to-sql-database-by-using-php-on-windows"></a>Yhteyden muodostaminen SQL-tietokantaan PHP avulla Windowsiin


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 


Tässä ohjeaiheessa kuvataan siitä, miten voit muodostaa yhteyden Azure SQL-tietokanta-asiakassovellus kirjoitettu PHP, joka suoritetaan Windows.

## <a name="step-1--configure-development-environment"></a>Vaihe 1: Määritä kehitysympäristö

[Määritä PHP kehittäminen kehitysympäristö](https://msdn.microsoft.com/library/mt720663.aspx)

## <a name="step-2-create-a-sql-database"></a>Vaihe 2: SQL-tietokannan luominen

Katso [aloittaminen-sivu](sql-database-get-started.md) luominen mallitietokanta.  On tärkeää noudatat luominen **AdventureWorks-tietokantamallin**opas. Alla on esitetty esimerkkejä toimivat vain **AdventureWorks rakenne**.


## <a name="step-3-get-connection-details"></a>Vaihe 3: Hae yhteystiedot

[AZURE.INCLUDE [sql-database-include-connection-string-details-20-portalshots](../../includes/sql-database-include-connection-string-details-20-portalshots.md)]


## <a name="step-4-run-sample-code"></a>Vaihe 4: Suorita otoksen koodi

* [Yhteyden muodostaminen SQL käyttämällä PHP käsite kuitti](https://msdn.microsoft.com/library/mt720665.aspx)
* [Resiliently muodostaa yhteyden SQL PHP](https://msdn.microsoft.com/library/mt720667.aspx)


## <a name="next-steps"></a>Seuraavat vaiheet

* Tarkista [SQL-tietokannan kehittäminen yleiskatsaus](sql-database-develop-overview.md)
* Lisätietoja [Microsoft SQL Server PHP-ohjain](https://msdn.microsoft.com/library/dn865013.aspx)
* Liittyen PHP asennus- ja käyttöä, katso lisätietoja [SQL Server-tietokannoista käyttäminen PHP](http://social.technet.microsoft.com/wiki/contents/articles/1258.accessing-sql-server-databases-from-php.aspx).

## <a name="additional-resources"></a>Lisäresursseja 

* [Rakenne-kuvioiden määrittäminen usean vuokraajan SaaS sovellusten Azure SQL-tietokanta](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Selaa kaikkia [ominaisuuksia SQL-tietokantaan](https://azure.microsoft.com/services/sql-database/)
