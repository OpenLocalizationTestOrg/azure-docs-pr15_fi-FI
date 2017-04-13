<properties 
    pageTitle="Luo tai siirtäminen joustavasti resurssivarannon käyttäminen T-SQL Azure SQL-tietokantaan | Microsoft Azure" 
    description="Käytä T-SQL Azure SQL-tietokannan luominen joustavasti resurssivarantoon. Tai siirtää datbase ja siitä uloskirjautuminen jakavat T-SQL avulla." 
    services="sql-database" 
    documentationCenter="" 
    authors="srinia" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-management" 
    ms.date="05/27/2016"
    ms.author="srinia"/>

# <a name="monitor-and-manage-an-elastic-database-pool-with-transact-sql"></a>Seurata ja hallita joustavasti tietokanta-ryhmän kanssa Transact-SQL  

> [AZURE.SELECTOR]
- [Azure portal](sql-database-elastic-pool-manage-portal.md)
- [PowerShellin](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL](sql-database-elastic-pool-manage-tsql.md)

[Luo tietokanta (Azure SQL-tietokanta)](https://msdn.microsoft.com/library/dn268335.aspx) ja [Muuta Database(Azure SQL Database)](https://msdn.microsoft.com/library/mt574871.aspx) -komentojen avulla voit luoda ja siirtää tietokantojen sisään ja ulos joustavasti jakavat. Joustavasti varanto on oltava olemassa ennen kuin voit käyttää seuraavia komentoja. Nämä komennot vaikuttavat vain tietokantoja. Uusi jakavat luomista ja resurssivarantoon ominaisuuksista (kuten min- ja max eDTUs)-asetus ei voi muuttaa T-SQL-komentoja.

## <a name="create-a-new-database-in-an-elastic-pool"></a>Luo uusi tietokanta joustavasti resurssivarantoon
Luo tietokanta-komennolla SERVICE_OBJECTIVE-vaihtoehto.   

    CREATE DATABASE db1 ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3M100] ));
    -- Create a database named db1 in a pool named S3M100.

Kaikki tietokannat joustavasti varannon perivät palvelun tason joustavasti altaan (Basic, Vakio, Premium). 


## <a name="move-a-database-between-elastic-pools"></a>Tietokannan siirtäminen joustavasti jakavat välillä
Muuta TIETOKANTAA-komennon käyttäminen Muokkaa ja määritä palvelun\_OBJEKTIIVISTEN vaihtoehto lisää JOUSTAVASTI\_ALTAAN; Määritä nimi kohde-ryhmän nimeä.

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [PM125] ));
    -- Move the database named db1 to a pool named P1M125  

## <a name="move-a-database-into-an-elastic-pool"></a>Tietokannan siirtäminen joustavasti resurssivarantoon 
Muuta TIETOKANTAA-komennon käyttäminen Muokkaa ja määritä palvelun\_OBJEKTIIVISTEN vaihtoehto ELASTIC_POOL; Määritä nimi kohde-ryhmän nimeä.

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3100] ));
    -- Move the database named db1 to a pool named S3100.

## <a name="move-a-database-out-of-an-elastic-pool"></a>Tietokannan siirtäminen pois joustavasti resurssivarantoon
Muuta TIETOKANTAA-komennolla ja määritä SERVICE_OBJECTIVE suorituskyvyn tasot (S0, S1 jne.).

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = 'S1');
    -- Changes the database into a stand-alone database with the service objective S1.

## <a name="list-databases-in-an-elastic-pool"></a>Luettelon tietokantojen joustavasti resurssivarantoon
Käytä [sys.database\_palvelun \_tavoitteiden näkymän](https://msdn.microsoft.com/library/mt712619) luettelon kaikki joustavasti resurssivarantoon tietokannat. Kirjaudu sisään perusmuodon kysely näkymän tietokanta.

    SELECT d.name, slo.*  
    FROM sys.databases d 
    JOIN sys.database_service_objectives slo  
    ON d.database_id = slo.database_id
    WHERE elastic_pool_name = 'MyElasticPool'; 

## <a name="get-resource-usage-data-for-a-pool"></a>Resurssien käyttötietojen hankkiminen resurssivarantoon

Käytä [sys.elastic\_resurssivarantoon \_resurssin \_tilasto näkymän](https://msdn.microsoft.com/library/mt280062.aspx) tarkistaa resurssien käyttö tilastotietoja joustavasti resurssivarantoon, looginen palvelimessa. Kirjaudu sisään perusmuodon kysely näkymän tietokanta.

    SELECT * FROM sys.elastic_pool_resource_stats 
    WHERE elastic_pool_name = 'MyElasticPool'
    ORDER BY end_time DESC;

## <a name="get-resource-usage-for-an-elastic-database"></a>Hae joustavasti tietokannan resurssien käyttö

Käytä [sys.dm\_ db\_ resurssin\_tilasto näkymän](https://msdn.microsoft.com/library/dn800981.aspx) tai [sys.resource \_tilasto näkymän](https://msdn.microsoft.com/library/dn269979.aspx) tutkia resurssin käyttötilastoja joustavasti varannon tietokannan. Tämä toimenpide on samankaltainen kuin kyselyt mikä tahansa yksittäinen tietokannan resurssien käyttö.

## <a name="next-steps"></a>Seuraavat vaiheet

Kun olet luonut joustavasti tietokannan resurssivarantoon, voit hallita joustavasti tietokantojen altaan luomalla joustavasti työt. Joustavasti työt helpottaa T-SQL-komentosarjojen suorittamisen vastaan altaan tietokantojen määrää. Lisätietoja on artikkelissa [joustavasti tietokannan töiden yleiskatsaus](sql-database-elastic-jobs-overview.md). 

Katso [Skaalaus ulos Azure SQL-tietokantaan](sql-database-elastic-scale-introduction.md): joustavasti Tietokantatyökalut avulla asteikko-kohtaa, Siirrä tiedot, kyselyn tai luoda tapahtumia.
