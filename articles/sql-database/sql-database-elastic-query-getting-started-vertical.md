<properties
    pageTitle="Aloita rajat-tietokantakyselyt (pystysuora jakaminen) | Microsoft Azure"   
    description="Voit käyttää joustavasti tietokantakyselyn pystysuunnassa osioitua tietokannat"
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="torsteng"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/23/2016"
    ms.author="torsteng" />

# <a name="get-started-with-cross-database-queries-vertical-partitioning-preview"></a>Aloita rajat-tietokantakyselyt (pystysuora jakaminen) (ennakkoversio)

Voit suorittaa T-SQL-kyselyjä, jotka ulottuvat useita tietokantoja, joissa yksi yhteyspisteen Azure SQL-tietokanta joustavasti tietokantakyselyn (ennakkoversio). Tämä ohjeaihe koskee [osioitu pystysuunnassa tietokannat](sql-database-elastic-query-vertical-partitioning.md).  

Kun olet valmis, se: Lue, miten voit määrittää ja Azure SQL-tietokannan avulla voit suorittaa kyselyjä, jotka ulottuvat useita Aiheeseen liittyviä tietokantoja. 

Saat lisätietoja joustavasti tietokannan kysely-toiminnon [Azure SQL-tietokanta joustavasti tietokannan kyselyn yleiskatsaus](sql-database-elastic-query-overview.md). 

## <a name="create-the-sample-databases"></a>Luo mallitietokantoja

Aluksi annettava **Asiakkaat** - ja **tilaukset**-joko samassa tai eri looginen palvelimet tietokantojen luominen.   

Suorita seuraavat kyselyt **OrderInformation** -taulukon luominen ja kirjoita esimerkkitiedot **tilaukset** -tietokannan. 

    CREATE TABLE [dbo].[OrderInformation]( 
        [OrderID] [int] NOT NULL, 
        [CustomerID] [int] NOT NULL 
        ) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (123, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (149, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (857, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (321, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (564, 8) 

Nyt suorita seuraavat kyselyn **CustomerInformation** -taulukon luominen ja kirjoita esimerkkitiedot **Asiakkaat** -tietokantaan. 

    CREATE TABLE [dbo].[CustomerInformation]( 
        [CustomerID] [int] NOT NULL, 
        [CustomerName] [varchar](50) NULL, 
        [Company] [varchar](50) NULL 
        CONSTRAINT [CustID] PRIMARY KEY CLUSTERED ([CustomerID] ASC) 
    ) 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (1, 'Jack', 'ABC') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (2, 'Steve', 'XYZ') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (3, 'Lylla', 'MNO') 

## <a name="create-database-objects"></a>Tietokantaobjektien luominen
### <a name="database-scoped-master-key-and-credentials"></a>Tietokannan kohdistetuissa perustyyli-näppäintä ja tunnistetiedot

1. Avaa Visual Studiossa SQL Server Management Studiossa tai SQL Server Data Tools.
2. Tilaukset-tietokantayhteyden muodostamisessa ja suorittaa seuraavat T-SQL-komentoja:

        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>'; 
        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred 
        WITH IDENTITY = '<username>', 
        SECRET = '<password>';  

    "Käyttäjänimi" ja "salasana" pitäisi olla käyttäjänimi ja salasana, jota käytetään kirjautumista asiakkaat-tietokantaan.
    Azure Active Directoryn avulla joustavasti kyselyjen todennusta ei tueta.

### <a name="external-data-sources"></a>Ulkoiset tietolähteet
Voit luoda ulkoiseen tietolähteeseen, suorita seuraava komento tilaukset-tietokantaan: 

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH 
        (TYPE = RDBMS, 
        LOCATION = '<server_name>.database.windows.net', 
        DATABASE_NAME = 'Customers', 
        CREDENTIAL = ElasticDBQueryCred, 
    ) ;

### <a name="external-tables"></a>Ulkoiset taulukot
Luo ulkoinen taulukko Tilaukset-tietokantaan, joka vastaa CustomerInformation taulukon määritys:

    CREATE EXTERNAL TABLE [dbo].[CustomerInformation] 
    ( [CustomerID] [int] NOT NULL, 
      [CustomerName] [varchar](50) NOT NULL, 
      [Company] [varchar](50) NOT NULL) 
    WITH 
    ( DATA_SOURCE = MyElasticDBQueryDataSrc) 

## <a name="execute-a-sample-elastic-database-t-sql-query"></a>Esimerkki joustavasti tietokannan T-SQL-kyselyn suorittaminen

Kun olet määrittänyt ulkoisen tietolähteen ja ulkoiset taulukoiden voit nyt käyttää T-SQL-kysely ulkoiset taulukot. Suorita kysely tilaukset-tietokantaan: 

    SELECT OrderInformation.CustomerID, OrderInformation.OrderId, CustomerInformation.CustomerName, CustomerInformation.Company 
    FROM OrderInformation 
    INNER JOIN CustomerInformation 
    ON CustomerInformation.CustomerID = OrderInformation.CustomerID 

## <a name="cost"></a>Kustannukset

Tällä hetkellä joustavasti tietokannan kysely-toiminnon sisällytetään Azure SQL-tietokanta kustannukset.  

Alla on tietoja on [SQL-tietokannan hinnat](/pricing/details/sql-database). 


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->

<!--anchors-->
