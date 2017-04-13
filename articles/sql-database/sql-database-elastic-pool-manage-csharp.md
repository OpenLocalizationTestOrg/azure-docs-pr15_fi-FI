<properties
    pageTitle="Seurata ja hallita joustavasti tietokanta-ryhmän ja C# | Microsoft Azure"
    description="C# tietokannan kehittäminen tekniikoita avulla voit hallita Azure SQL-tietokanta-joustavasti tietokannan resurssivarantoon."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="csharp"
    ms.workload="data-management"
    ms.date="10/04/2016"
    ms.author="sstein"/>

# <a name="monitor-and-manage-an-elastic-database-pool-with-cx23"></a>Seurata ja hallita joustavasti tietokanta-ryhmän C & #x 23. 

> [AZURE.SELECTOR]
- [Azure portal](sql-database-elastic-pool-manage-portal.md)
- [PowerShellin](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL](sql-database-elastic-pool-manage-tsql.md)


Opi hallitsemaan [joustavasti tietokannan resurssivarantoon](sql-database-elastic-pool.md) käyttämällä C ja #x 23;. 

>[AZURE.NOTE] SQL-tietokannan monia uusia ominaisuuksia tuetaan vain, kun käytät [Azure resurssien hallinnan käyttöönottomalli](../azure-resource-manager/resource-group-overview.md), joten käytä aina uusimmat **Azure SQL tietokanta hallinnan kirjasto .NET ([docs](https://msdn.microsoft.com/library/azure/mt349017.aspx) | [NuGet paketti](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql))**. Vanhat [perinteinen käyttöönoton malli-pohjainen kirjastoihin](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Sql) tuetaan vain yhteensopivuus niin Suosittelemme käyttämään uudempaan mukaan Resurssienhallinta-kirjastot.

Tämän artikkelin seuraavien vaiheiden suorittamiseen tarvitset seuraavat:

- Joustavasti resurssivarantoon (haluat hallita ryhmän) Resurssivarantoon-kohdassa [luominen joustavasti tietokanta-ryhmän ja C#](sql-database-elastic-pool-create-csharp.md).
- Visual Studio. Katso Visual Studio maksutta, [Visual Studio lataukset](https://www.visualstudio.com/downloads/download-visual-studio-vs) -sivu.


## <a name="move-a-database-into-an-elastic-pool"></a>Tietokannan siirtäminen joustavasti resurssivarantoon

Voit siirtää erillinen tietokannan tai loitontaminen resurssivarantoon.  

    // Retrieve current database properties.

    currentDatabase = sqlClient.Databases.Get("resourcegroup-name", "server-name", "Database1").Database;

    // Configure create or update parameters with existing property values, override those to be changed.
    DatabaseCreateOrUpdateParameters updatePooledDbParameters = new DatabaseCreateOrUpdateParameters()
    {
        Location = currentDatabase.Location,
        Properties = new DatabaseCreateOrUpdateProperties()
        {
            Edition = "Standard",
            RequestedServiceObjectiveName = "ElasticPool",
            ElasticPoolName = "ElasticPool1",
            MaxSizeBytes = currentDatabase.Properties.MaxSizeBytes,
            Collation = currentDatabase.Properties.Collation,
        }
    };

    // Update the database.
    var dbUpdateResponse = sqlClient.Databases.CreateOrUpdate("resourcegroup-name", "server-name", "Database1", updatePooledDbParameters);

## <a name="list-databases-in-an-elastic-pool"></a>Luettelon tietokantojen joustavasti resurssivarantoon

Kaikki tietokannat resurssivarantoon hakemiseen kutsua [ListDatabases](https://msdn.microsoft.com/library/microsoft.azure.management.sql.elasticpooloperationsextensions.listdatabases) -menetelmää.

    //List databases in the elastic pool
    DatabaseListResponse dbListInPool = sqlClient.ElasticPools.ListDatabases("resourcegroup-name", "server-name", "ElasticPool1");
    Console.WriteLine("Databases in Elastic Pool {0}", "server-name.ElasticPool1");
    foreach (Database db in dbListInPool)
    {
        Console.WriteLine("  Database {0}", db.Name);
    }

## <a name="change-performance-settings-of-a-pool"></a>Resurssivarantoon suorituskyvyn asetusten muuttaminen

Noutaa aiemmin resurssivarantoon ominaisuudet. Muokkaa arvoja ja suorita CreateOrUpdate-menetelmää.

    var currentPool = sqlClient.ElasticPools.Get("resourcegroup-name", "server-name", "ElasticPool1").ElasticPool;

    // Configure create or update parameters with existing property values, override those to be changed.
    ElasticPoolCreateOrUpdateParameters updatePoolParameters = new ElasticPoolCreateOrUpdateParameters()
    {
        Location = currentPool.Location,
        Properties = new ElasticPoolCreateOrUpdateProperties()
        {
            Edition = currentPool.Properties.Edition,
            DatabaseDtuMax = 50, /* Setting DatabaseDtuMax to 50 limits the eDTUs that any one database can consume */
            DatabaseDtuMin = 10, /* Setting DatabaseDtuMin above 0 limits the number of databases that can be stored in the pool */
            Dtu = (int)currentPool.Properties.Dtu,
            StorageMB = currentPool.Properties.StorageMB,  /* For a Standard pool there is 1 GB of storage per eDTU. */
        }
    };

    newPoolResponse = sqlClient.ElasticPools.CreateOrUpdate("resourcegroup-name", "server-name", "ElasticPool1", newPoolParameters);


## <a name="latency-of-elastic-pool-operations"></a>Viive joustavasti resurssivarantoon toiminnot

- Tietokannan tai max eDTUs tietokantaa kohden kohti min-eDTUs muuttaminen yleensä suorittaa päättymässä tai vähemmän.
- Kun haluat muuttaa ryhmän koko (eDTUs) määräytyy altaan kaikki tietokannat yhteenlasketun koon. Muutokset keskimääräinen 90 minuuttia enintään 100 gigatavua. Jos esimerkiksi kaikki tietokannat altaan yhteensä tilaa on 200 gt, valitse odotettu viive muuttaminen kohti resurssivarantoon resurssivarantoon-eDTU on 3 tuntia tai vähemmän.




## <a name="additional-resources"></a>Lisäresursseja

- [SQL-virhekoodit SQL-tietokantaan asiakassovellukset: tietokannan yhteysvirhe ja muiden ongelmien](sql-database-develop-error-messages.md).
- [SQL-tietokantaan](https://azure.microsoft.com/documentation/services/sql-database/)
- [Azure resurssien hallinnan API](https://msdn.microsoft.com/library/azure/dn948464.aspx)
- [Luo uusi ryhmä joustavasti tietokannan C#](sql-database-elastic-pool-create-csharp.md)
- [Kun joustavasti tietokannan resurssivarantoon voidaan käyttää?](sql-database-elastic-pool-guidance.md)
- Katso [Skaalaus ulos Azure SQL-tietokantaan](sql-database-elastic-scale-introduction.md): joustavasti Tietokantatyökalut avulla asteikko-kohtaa, Siirrä tiedot, kyselyn tai luoda tapahtumia.

