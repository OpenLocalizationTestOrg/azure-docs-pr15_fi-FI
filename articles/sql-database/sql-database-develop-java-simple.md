<properties
    pageTitle="Yhteyden muodostaminen SQL-tietokanta käyttämällä Java JDBC Windows | Microsoft Azure"
    description="Esittää Java koodin otoksen avulla voit muodostaa yhteyden Azure SQL-tietokantaan. Esimerkissä JDBC, ja se suoritetaan Windows asiakastietokoneessa."
    services="sql-database"
    documentationCenter=""
    authors="LuisBosquez"
    manager="jhubbard"
    editor="genemi"/>


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="lbosq"/>


# <a name="connect-to-sql-database-by-using-java-with-jdbc-on-windows"></a>Yhteyden muodostaminen SQL-tietokanta käyttämällä Java JDBC Windows


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 


Tässä ohjeartikkelissa esitellään Java koodin otoksen, joiden avulla voit muodostaa yhteyden Azure SQL-tietokantaan. Java-Esimerkki perustuu-Java Development Kit (JDK) versio 1.8. Otosten yhdistää käyttämällä JDBC driver Azure SQL-tietokantaan.

## <a name="step-1--configure-development-environment"></a>Vaihe 1: Määritä kehitysympäristö

[Määritä Java kehittäminen kehitysympäristö](https://msdn.microsoft.com/library/mt720658.aspx)

## <a name="step-2-create-a-sql-database"></a>Vaihe 2: SQL-tietokannan luominen

Katso [aloittaminen-sivu](sql-database-get-started.md) tietokannan luominen.  

## <a name="step-3-get-connection-string"></a>Vaihe 3: Hae yhteysmerkkijonon

[AZURE.INCLUDE [sql-database-include-connection-string-jdbc-20-portalshots](../../includes/sql-database-include-connection-string-jdbc-20-portalshots.md)]

> [AZURE.NOTE] Jos käytät JTDS JDBC ohjainta ja valitse sinun on lisättävä "ssl = edellyttävät" yhteys URL-osoitteen merkkijono ja tarvitset seuraavat asetuksen määrittäminen JVM "-Djsse.enableCBCProtection=false". JVM tämä asetus poistaa käytöstä suojausheikkous korjaus, joten varmista, tiedät, mitä riskejä liittyy ennen tämä asetus.

## <a name="step-4-run-sample-code"></a>Vaihe 4: Suorita otoksen koodi

* [Yhteyden muodostaminen SQL käyttämällä Java käsite kuitti](https://msdn.microsoft.com/library/mt720656.aspx)

## <a name="next-steps"></a>Seuraavat vaiheet

* Käy [Java Developer Center](/develop/java/).
* Tarkista [SQL-tietokannan kehittäminen yleiskatsaus](sql-database-develop-overview.md)
* Lisätietoja [Microsoft SQL Server JDBC-ohjain](https://msdn.microsoft.com/library/mt484311.aspx)

## <a name="additional-resources"></a>Lisäresursseja 

* [Rakenne-kuvioiden määrittäminen usean vuokraajan SaaS sovellusten Azure SQL-tietokanta](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Selaa kaikkia [ominaisuuksia SQL-tietokantaan](https://azure.microsoft.com/services/sql-database/)
