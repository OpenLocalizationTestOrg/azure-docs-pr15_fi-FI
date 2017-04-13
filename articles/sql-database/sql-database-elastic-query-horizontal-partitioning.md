<properties
    pageTitle="Raportoinnin skaalatun ulos cloud tietokantojen yli | Microsoft Azure"
    description="Voit määrittää joustavasti kyselyjen yli vaakasuunnassa osiot"    
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
    ms.date="05/27/2016"
    ms.author="torsteng" />

# <a name="reporting-across-scaled-out-cloud-databases-preview"></a>Raportointi on mahdollista skaalata ulos cloud-tietokannat (ennakkoversio)

![Kysely shards][1]

Sharded tietokantojen jakaa rivien skaalatun ulos tietojen taso. Rakenne on sama kuin osallistuvan tietokantoja, kutsutaan myös vaakasuora jakaminen. Joustavasti Queryn avulla voit luoda raportteja, jotka ulottuvat sharded tietokannan kaikki tietokannat.

Katso pika-aloitusopas [raportointi skaalata ulos cloud tietokantojen välillä](sql-database-elastic-query-getting-started.md).

Katso-sharded tietokantojen [kyselyn eri rakenteita tietokantojen pilven kautta](sql-database-elastic-query-vertical-partitioning.md). 

 
## <a name="prerequisites"></a>Edellytykset

* Joustavasti tietokannan asiakas-kirjaston käytön shard kartan luominen. Katso [Shard yhdistää hallinta](sql-database-elastic-scale-shard-map-management.md). Tai käyttää malli-sovellusta [joustavasti Tietokantatyökalut käytön aloittaminen](sql-database-elastic-scale-get-started.md).
* Voit myös tarkastella [artikkelista aiemmin luotujen tietokantojen skaalata ulos tietokantoihin](sql-database-elastic-convert-to-use-elastic-tools.md).
* Käyttäjän on oltava muuttaa minkä tahansa ULKOISEN TIETOLÄHTEEN käyttöoikeus. Nämä oikeudet sisältyy käyttöoikeuksien muuta TIETOKANTAA.
* MUUTTAA minkä tahansa ULKOISEEN TIETOLÄHTEESEEN käyttöoikeudet tarvitaan viitata pohjana olevassa tietolähteessä.

## <a name="overview"></a>Yleiskatsaus

Alikyselyn luominen sharded tietojen taso metatietojen esittäminen joustavasti kysely-tietokantaan. 


1. [LUO AVAIN PERUSTYYLI](https://msdn.microsoft.com/library/ms174382.aspx)
2. [LUO TIETOKANTA SUODATETUT TUNNISTETIEDON](https://msdn.microsoft.com/library/mt270260.aspx)
3. [LUO ULKOINEN TIETOLÄHDE](https://msdn.microsoft.com/library/dn935022.aspx)
4. [LUO ULKOINEN TAULUKKO](https://msdn.microsoft.com/library/dn935021.aspx) 

## <a name="11-create-database-scoped-master-key-and-credentials"></a>1.1 suodatetut tietokannan avaimen ja tunnistetietojen luominen 

Tunnistetieto käytetään joustavasti kyselyn muodostaminen remote tietokantoja.  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]
 
>[AZURE.NOTE] Varmista, että *"\<käyttäjänimi\>"* ei sisällä *"@servername"* liite. 

## <a name="12-create-external-data-sources"></a>1.2 luominen ulkoisiin tietolähteisiin

Syntaksi:

    <External_Data_Source> ::=    
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH                                            
            (TYPE = SHARD_MAP_MANAGER,
                    LOCATION = '<fully_qualified_server_name>',
            DATABASE_NAME = ‘<shardmap_database_name>',
            CREDENTIAL = <credential_name>, 
            SHARD_MAP_NAME = ‘<shardmapname>’ 
                   ) [;] 

### <a name="example"></a>Esimerkki 

    CREATE EXTERNAL DATA SOURCE MyExtSrc 
    WITH 
    ( 
        TYPE=SHARD_MAP_MANAGER,
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ShardMapDatabase', 
        CREDENTIAL= SMMUser, 
        SHARD_MAP_NAME='ShardMap' 
    );
 
Hakea nykyisen ulkoisten tietolähteiden luettelo: 

    select * from sys.external_data_sources; 

Ulkoisen tietolähteen viittaa shard kartta. Joustavasti kysely käyttää sitten ulkoisen tietolähteen ja pohjana shard kartan luettelointi tietokannoissa, jotka osallistuminen tietojen taso. Samoja tunnistetietoja käytetään voi lukea shard kartan ja shards tietojen käyttöä joustavasti kyselyn käsiteltäessä. 

## <a name="13-create-external-tables"></a>1.3 ulkoisen taulukoiden luominen 
 
Syntaksi:  

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name  
        ( { <column_definition> } [ ,...n ])     
        { WITH ( <sharded_external_table_options> ) }
    ) [;]  
    
    <sharded_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>,       
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 
      DISTRIBUTION = SHARDED(<sharding_column_name>) | REPLICATED |ROUND_ROBIN

**Esimerkki**

    CREATE EXTERNAL TABLE [dbo].[order_line]( 
         [ol_o_id] int NOT NULL, 
         [ol_d_id] tinyint NOT NULL,
         [ol_w_id] int NOT NULL, 
         [ol_number] tinyint NOT NULL, 
         [ol_i_id] int NOT NULL, 
         [ol_delivery_d] datetime NOT NULL, 
         [ol_amount] smallmoney NOT NULL, 
         [ol_supply_w_id] int NOT NULL, 
         [ol_quantity] smallint NOT NULL, 
         [ol_dist_info] char(24) NOT NULL 
    ) 
    
    WITH 
    ( 
        DATA_SOURCE = MyExtSrc, 
        SCHEMA_NAME = 'orders', 
        OBJECT_NAME = 'order_details', 
        DISTRIBUTION=SHARDED(ol_w_id)
    ); 

Nouda ulkoiset taulukot luettelo nykyisestä tietokannasta: 

    SELECT * from sys.external_tables; 

Voit katkaista ulkoisen taulukot:

    DROP EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name[;]

### <a name="remarks"></a>Huomautuksia

TIETOJA\_LÄHDE-lause määrittää ulkoisen tietolähteen (shard kartta), jota käytetään ulkoisen taulukon.  

RAKENTEEN\_nimi ja\_nimi lauseet yhdistäminen ulkoisen taulukkomäärityksen taulukon eri rakenteessa. Jos jätetään pois, remote objektin rakenteen oletetaan "dbo" ja sen nimen oletetaan olevan sama kuin määritetty ulkoisen taulukkonimi. Tämä on kätevä, jos remote-taulukon nimi on varattu tietokannan kohtaa, johon haluat luoda ulkoisen taulukon. Esimerkiksi haluat määrittää ulkoisen taulukon saat kooste Näytä näkymien luettelo tai DMVs skaalatun ulos tietoihisi taso. Luettelon näkymien ja DMVs jo paikallisesti, koska et voi käyttää heidän nimensä ulkoisen taulukon määrityksen. Sen sijaan käyttää eri nimellä ja luettelo-näkymä tai DMV nimissään rakenteen\_nimen ja/tai OBJEKTIN\_nimi lauseita. (Katso alla oleva esimerkki). 

JAKAUMAN lauseke määrittää tämän taulukon jakautuman. Kyselyn käyttämällä voit luoda tehokkaasti kyselyn palvelupaketin jakauman-lauseessa tiedot.  

1. **SHARDED** tarkoittaa, että tiedot vaakasuunnassa osioidaan yli tietokannat. Tietojen jakelu osioinnin näppäintä on **< sharding_column_name >** -parametri.
2. **REPLIKOITU** tarkoittaa, että taulukon samanlaiset kopiot löytyvät kunkin tietokannan. Se on varmistaa, että tietokannat replikat ovat samat.
3. **PYÖRISTÄ\_MIKKO** tarkoittaa, että taulukon vaakasuunnassa osioitu sovellusta riippuvaisten jakauman-menetelmällä. 

**Tietojen taso viite**: ulkoisen taulukon DDL viittaa ulkoisesta tietolähteestä. Ulkoisen tietolähteen määrittää shard kartan, joka sisältää ulkoisen taulukon tiedot, voit etsiä kaikki tietokannat tietojen taso. 


### <a name="security-considerations"></a>Suojausasiat 

Käyttäjät voivat käyttää ulkoisen taulukon automaattisesti käyttöösi kohdasta ulkoisen tietolähteen määrityksen alan tunnistetiedon remote taulukoihin. Vältä toivottavien laajentamisen ulkoisen tietolähteen tunnistetiedon kautta. Käytä myöntäminen tai PERUUTTAA ulkoisen taulukon aivan kuin se on Normaali taulukko.  

Kun olet määrittänyt ulkoisen tietolähteen ja ulkoiset taulukot, voit käyttää koko T-SQL nyt ulkoiset taulukot päälle.

## <a name="example-querying-horizontal-partitioned-databases"></a>Esimerkki: kyselyt vaakasuuntainen osioitua tietokannat 

Seuraava kysely suorittaa kolme tapaa liitoksen varastoja, tilaukset ja rivien välinen ja käyttää useita koosteet ja valikoiva suodatin. Se olettaa (1) vaakasuuntainen osioinnin (sharding) ja (2), varastoja, tilaukset ja rivit ovat sharded varaston tunnus-sarakkeen mukaan ja että joustavasti kyselyn voit etsiä yhtä liitokset shards- ja käsitellä shards rinnakkain ja kyselyn kallista osa. 

    select  
         w_id as warehouse,
         o_c_id as customer,
         count(*) as cnt_orderline,
         max(ol_quantity) as max_quantity,
         avg(ol_amount) as avg_amount, 
         min(ol_delivery_d) as min_deliv_date
    from warehouse 
    join orders 
    on w_id = o_w_id
    join order_line 
    on o_id = ol_o_id and o_w_id = ol_w_id 
    where w_id > 100 and w_id < 200 
    group by w_id, o_c_id 
 
## <a name="stored-procedure-for-remote-t-sql-execution-spexecuteremote"></a>Tallennetun toimintosarjan suorittamisen T-SQL for: sp\_execute_remote

Joustavasti kyselyn esitellään myös tallennetun toimintosarjan, joka sisältää shards suora pääsy. Tallennetun toimintosarjan kutsutaan [sp\_suorittaa \_remote](https://msdn.microsoft.com/library/mt703714) ja sitä voidaan käyttää remote tallennettujen toimintosarjojen tai T-SQL-koodin suorittaminen etäyhteyden tietokantoja. Vie seuraavat parametrit: 

* Tietolähteen nimi (nvarchar): ulkoisen tietolähteen tyyppi RDBMS nimi. 
* Kysely (nvarchar): T-SQL-kysely suoritetaan kunkin shard. 
* Ilmoitus parametrin (nvarchar) - Valinnainen: merkkijono, jossa tiedot määritelmät parametrien (kuten sp_executesql) Kyselyparametrin käytetään. 
* Parametrin arvo-luettelo - Valinnainen: CSV-luettelon parametriarvot (kuten sp_executesql).

Sp\_suorittaa\_etäyhteyksien käyttää ulkoisen tietolähteen säädetty Kutsuparametrit suorittaa annetun T-SQL-lauseen remote tietokannat. Se käyttää ulkoisen tietolähteen tunnistetiedon muodostaa shardmap manager-tietokanta- ja etä-tietokannat.  

Esimerkki: 

    EXEC sp_execute_remote
        N'MyExtSrc',
        N'select count(w_id) as foo from warehouse' 

## <a name="connectivity-for-tools"></a>Yhteys-työkaluissa  

Merkkijonojen säännöllisesti SQL Server-yhteyden avulla voit muodostaa sovelluksen BI ja tietojen integrointi Työkalut ulkoisen taulukon määritelmät tietokantaan. Varmista, että SQL Server on tuettu työkalu tietolähteenä. Valitse viittaus joustavasti kyselyn-tietokantaan, kuten SQL Server-tietokantaan yhdistetty työkalu ja käytä ulkoiset taulukot työkalua tai sovelluksen paikallisten taulukoiden Jos. 

## <a name="best-practices"></a>Parhaat käytännöt 

* Varmista, että joustavasti kyselyn päätepisteen tietokanta on käyttöoikeudet shardmap tietokannan ja kaikki shards SQL DB palomuurien kautta.  

* Vahvista tai Pakota määrittämiä ulkoisen taulukon tietojen jakauman. Jos todellisten tietojen jakoa poikkeaa jakauman määritetyn taulukkomääritys-kyselyissä voi tuottaa odottamattomia tuloksia. 

* Joustavasti kyselyn tällä hetkellä Suorita shard korjauksen kun predikaatit päälle sharding avain mahdollistaa sen pois tiettyjä shards turvallisesti käsittely.

* Joustavasti kysely toimii parhaiten kyselyjen kohtaa, johon useimmat laskemisen voidaan toteuttaa shards. Yleensä saat parhaan kyselyn suorituskyvyn Valikoiva suodatin-predikaatit, joka voi tuottaa tulokseksi shards tai liitokset osioinnin näppäimiä, jotka voivat tehdä kaikki shards-osion tasattu tavalla. Muut kyselyn kuvioita on ehkä ladata suuria tietomääriä shards pään solmu ja huonosti suorituskyky voi

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-horizontal-partitioning/horizontalpartitioning.png
<!--anchors-->
