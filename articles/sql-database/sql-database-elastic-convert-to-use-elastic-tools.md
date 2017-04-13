<properties
   pageTitle="Siirtää aiemmin luotujen tietokantojen asteikko itsestään | Microsoft Azure"
   description="Muuntaa sharded tietokannat joustavasti Tietokantatyökalut luomalla shard kartan hallinta"
   services="sql-database"
   documentationCenter=""
   authors="ddove"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="10/24/2016"
   ms.author="ddove"/>

# <a name="migrate-existing-databases-to-scale-out"></a>Siirtää aiemmin luotujen tietokantojen asteikko itsestään

Hallita skaalata ulos sharded tietokannoistasi Azure SQL-tietokanta Tietokantatyökalut (kuten [joustavasti tietokannan asiakkaan kirjaston](sql-database-elastic-database-client-library.md)) avulla. Sinun on ensin muunnettava aiemmin määritetyt tietokannat [shard yhdistää hallinta](sql-database-elastic-scale-shard-map-management.md). 

## <a name="overview"></a>Yleiskatsaus
Jos haluat siirtää aiemmin luodun sharded tietokannan: 

1. Valmistele [shard kartan manager-tietokanta](sql-database-elastic-scale-shard-map-management.md).
2. Shard kartan luominen
3. Valmistele yksittäisiä shards.  
2. Lisää yhdistämismääritykset shard kartan.

Näistä menetelmistä voidaan toteuttaa [.NET Framework client kirjaston](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)tai osoitteessa [Azure SQL-DB - joustavasti tietokannan Työkalut komentosarjoja](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db)PowerShell-komentosarjojen avulla. Tässä esimerkissä käytetään PowerShell-komentosarjojen.

Saat lisätietoja ShardMapManager [Shard yhdistää hallinta](sql-database-elastic-scale-shard-map-management.md). Katso yleiskuvaus joustavasti Tietokantatyökalut- [joustavasti tietokannan ominaisuuksien yleiskatsaus](sql-database-elastic-scale-introduction.md).

## <a name="prepare-the-shard-map-manager-database"></a>Valmistele shard kartan manager-tietokanta

Shard kartan hallinta on erityisen tietokannan, jonka tietoihin haluat skaalata ulos tietokantojen hallinta. Voit käyttää aiemmin luotuun tietokantaan tai luoda uuden tietokannan. Huomaa, että visuaalisessa muodossa shard kartan hallinnan tietokannan tulee shard saman tietokannan. Huomaa, että PowerShell-komentosarjaa ei luoda tietokannan puolestasi. 

## <a name="step-1-create-a-shard-map-manager"></a>Vaihe 1: Luo shard kartan hallinta

    # Create a shard map manager. 
    New-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 
    #<server_name> and <smm_db_name> are the server name and database name 
    # for the new or existing database that should be used for storing 
    # tenant-database mapping information.

### <a name="to-retrieve-the-shard-map-manager"></a>Noutaa shard kartan hallinta

Luonnin jälkeen voit hakea shard kartan hallinnan cmdlet. Tämä vaihe tarvita aina, kun haluat käyttää ShardMapManager objektia.

    # Try to get a reference to the Shard Map Manager  
    $ShardMapManager = Get-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 

  
## <a name="step-2-create-the-shard-map"></a>Vaihe 2: shard-määrityksen luominen

Valittava shard kartan luominen tyyppi. Valinnan määräytyy tietokannan rakenne: 

1. Yhtä vuokraajan tietokantaa kohden (ehtojen perusteella, katso [sanasto](sql-database-elastic-scale-glossary.md).) 
2. Useita alihallinnat (kahdenlaisia) tietokantaa kohden:
    3. Luettelon yhdistäminen
    4. Alueen yhdistäminen
 

**Luettelon yhdistäminen** shard kartan luominen yhden vuokraajan-mallin. Yhden vuokraajan mallin määrittää yhtä tietokantaa kohden. Tämä on tehokas mallin, SaaS ohjelmistokehittäjille se helpottaa hallinta.

![Luettelon yhdistäminen][1]

Usean vuokraajan mallin määrittää useita alihallinnat yhteen tietokantaan (ja ryhmien omistajien voi jakaa useiden tietokantojen). Käytä tätä mallia, kun kukin vuokraajan on pieni tietojen tarpeisiin toiminta. Tämän mallin on määrittää alihallinnat solualueen tietokanta käyttämällä **alueen yhdistämismääritys**. 
 

![Alueen yhdistäminen][2]

Tai voit ottaa käyttöön usean vuokraajan tietokantamallin *luettelon yhdistämisen* avulla voit määrittää useita alihallinnat yhden tietokannan. Esimerkiksi MJP1 käytetään vuokraajan tunnus 1 – 5 tietojen tallentamiseen ja DB2 tallentaa vuokraajan 7 ja vuokraajan 10. 

![Valitse yksittäinen DB alihallinnat Muliple][3] 

**Valintasi mukaan, valitse jokin seuraavista vaihtoehdoista:**

### <a name="option-1-create-a-shard-map-for-a-list-mapping"></a>Vaihtoehto 1: luettelon yhdistämistä varten shard kartan luominen
ShardMapManager-objektin shard kartan luominen. 

    # $ShardMapManager is the shard map manager object. 
    $ShardMap = New-ListShardMap -KeyType $([int]) 
    -ListShardMapName 'ListShardMap' 
    -ShardMapManager $ShardMapManager 
 
 
### <a name="option-2-create-a-shard-map-for-a-range-mapping"></a>Vaihtoehto 2: Luo shard kartan alueen yhdistämistä varten

Huomaa, että voit käyttää tätä yhdistäminen kuviota, vuokraajan tunnus arvojen on oltava jatkuva alueet ja hyväksy on välin alueet ohitetaan alueen yksinkertaisesti tietokantojen luotaessa.

    # $ShardMapManager is the shard map manager object 
    # 'RangeShardMap' is the unique identifier for the range shard map.  
    $ShardMap = New-RangeShardMap 
    -KeyType $([int]) 
    -RangeShardMapName 'RangeShardMap' 
    -ShardMapManager $ShardMapManager 

### <a name="option-3-list-mappings-on-a-single-database"></a>Vaihtoehto 3: Luettelon yhden tietokannan määritykset
Luettelon kartan luominen myös tämän mallin määrittäminen edellyttää, kuten vaiheessa 2-vaihtoehto 1.

## <a name="step-3-prepare-individual-shards"></a>Vaihe 3: Valmisteleminen yksittäisiä shards

Lisää kunkin shard (tietokanta) shard kartan hallinta. Tämä valmistelee yksittäiset tietokannat yhdistäminen tietojen tallentamista varten. Suorita tämä menetelmä kunkin shard.
     
    Add-Shard 
    -ShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>'
    # The $ShardMap is the shard map created in step 2.
 

## <a name="step-4-add-mappings"></a>Vaihe 4: Lisää yhdistäminen

Yhdistäminen yhteen määräytyy shard kartan luomasi tyyppi. Jos olet luonut luettelon kartan, voit lisätä luettelon yhdistäminen. Jos olet luonut alueen kartan, voit lisätä alueen yhdistämismääritykset.

### <a name="option-1-map-the-data-for-a-list-mapping"></a>Vaihtoehto 1: Yhdistä yhdistämismäärityksen luettelon tiedot

Yhdistä tietoja lisäämällä luettelon yhdistämismäärityksen kunkin vuokraajan.  

    # Create the mappings and associate it with the new shards 
    Add-ListMapping 
    -KeyType $([int]) 
    -ListPoint '<tenant_id>' 
    -ListShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 

### <a name="option-2-map-the-data-for-a-range-mapping"></a>Vaihtoehto 2: Yhdistä tiedot alueen yhdistämistä varten

Lisää kaikki vuokraajan tunnus alueen – tietokannan yhteydet alue-määritykset:

    # Create the mappings and associate it with the new shards 
    Add-RangeMapping 
    -KeyType $([int]) 
    -RangeHigh '5' 
    -RangeLow '1' 
    -RangeShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 


### <a name="step-4-option-3-map-the-data-for-multiple-tenants-on-a-single-database"></a>Vaihe 4-vaihtoehto 3: tiedot yhdistetään useita alihallinnat yhden tietokannan varten

Kunkin vuokraajan suorittaa Lisää-ListMapping (vaihtoehto 1 yläpuolella). 


## <a name="checking-the-mappings"></a>Tarkistuksen yhdistämismääritykset

Tietoja aiemmin shards ja niihin liittyvät määritykset voidaan suorittaa kysely seuraavia komentoja käyttämällä:  

    # List the shards and mappings 
    Get-Shards -ShardMap $ShardMap 
    Get-Mappings -ShardMap $ShardMap 

## <a name="summary"></a>Yhteenveto

Kun asennus on valmis, voit aloittaa joustavasti tietokannan asiakas-kirjasto. Voit käyttää myös [tietojen riippuvaiset reititys](sql-database-elastic-scale-data-dependent-routing.md) ja [usean shard kyselyn](sql-database-elastic-scale-multishard-querying.md).

## <a name="next-steps"></a>Seuraavat vaiheet


Pyydä PowerShell-komentosarjojen [Azure SQL-DB-Lisää joustavasti tietokannan Työkalut sripts](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).

Työkaluja ovat myös GitHub: [Azure, Lisää joustavasti-db Työkalut](https://github.com/Azure/elastic-db-tools).

Jaa ja yhdistäminen-työkalun avulla voit siirtää tietoja tai usean vuokraajan mallista yhtä vuokraajan malliin. Katso [Jaa Yhdistä työkalun](sql-database-elastic-scale-get-started.md).

## <a name="additional-resources"></a>Lisäresursseja

Yleisiä tietoja arkkitehtuuri kuviot usean vuokraajan ohjelmiston muodossa--palvelun (SaaS) tietokannan sovellusten tietoja on artikkelissa [Rakenne kuviot usean vuokraajan SaaS sovellusten Azure SQL-tietokanta](sql-database-design-patterns-multi-tenancy-saas-applications.md).

## <a name="questions-and-feature-requests"></a>Kysymyksiä ja ehdottaa ominaisuuksia

Kysymyksiä Ota saavuttaminen us [SQL-tietokanta-keskustelupalstalla](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) ja ehdottaa ominaisuuksia, lisää ne [SQL-tietokantaan palautetta keskustelupalstalle](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-convert-to-use-elastic-tools/listmapping.png
[2]: ./media/sql-database-elastic-convert-to-use-elastic-tools/rangemapping.png
[3]: ./media/sql-database-elastic-convert-to-use-elastic-tools/multipleonsingledb.png
 
