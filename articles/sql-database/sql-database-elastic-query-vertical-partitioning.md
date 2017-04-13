<properties
    pageTitle="Kyselyn cloud tietokantojen eri rakenteen käyttäminen yli | Microsoft Azure"
    description="Voit määrittää rajat tietokantakyselyt pystysuunnassa osioiden päälle"    
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

# <a name="query-across-cloud-databases-with-different-schemas-preview"></a>Kysely cloud-tietokantojen eri rakenteita (ennakkoversio)

![Kysely eri tietokantojen taulukot][1]

Pystysuunnassa osioitu tietokannat käyttävät erilaisia arvojoukkoja taulukoiden eri tietokantoja. Tämä tarkoittaa, että rakenne on erilainen eri tietokantojen. Esimerkiksi kaikki taulukot varaston ovat yhden tietokannan vaikka kaikki accounting liittyvät taulukot ovat toisen tietokannan. 

## <a name="prerequisites"></a>Edellytykset

* Käyttäjän on oltava muuttaa minkä tahansa ULKOISEN TIETOLÄHTEEN käyttöoikeus. Nämä oikeudet sisältyy käyttöoikeuksien muuta TIETOKANTAA.
* MUUTTAA minkä tahansa ULKOISEEN TIETOLÄHTEESEEN käyttöoikeudet tarvitaan viitata pohjana olevassa tietolähteessä.

## <a name="overview"></a>Yleiskatsaus

**Huomautus**: toisin kuin kanssa vaakasuora jakaminen, DDL lauseiden eivät määräydy määrittäminen shard muuntomallin kautta joustavasti tietokannan asiakas-kirjaston tietoja taso.

1. [LUO AVAIN PERUSTYYLI](https://msdn.microsoft.com/library/ms174382.aspx)
2. [LUO TIETOKANTA SUODATETUT TUNNISTETIEDON](https://msdn.microsoft.com/library/mt270260.aspx)
3. [LUO ULKOINEN TIETOLÄHDE](https://msdn.microsoft.com/library/dn935022.aspx)
4. [LUO ULKOINEN TAULUKKO](https://msdn.microsoft.com/library/dn935021.aspx) 


## <a name="create-database-scoped-master-key-and-credentials"></a>Tietokannan suodatetut avaimen ja tunnistetietojen luominen 

Tunnistetieto käytetään joustavasti kyselyn muodostaminen remote tietokantoja.  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]
 
**Huomautus**    Varmistaa, että *<username>* ei sisällä *"@servername"* liite. 

## <a name="create-external-data-sources"></a>Luo ulkoisiin tietolähteisiin

Syntaksi:

    <External_Data_Source> ::=
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH 
               (TYPE = RDBMS,
                LOCATION = ’<fully_qualified_server_name>’,
                DATABASE_NAME = ‘<remote_database_name>’,  
                CREDENTIAL = <credential_name> 
                ) [;] 

**Tärkeää**   TYPE-parametri on määritettävä **RDBMS**. 

### <a name="example"></a>Esimerkki 

Seuraavassa esimerkissä havainnollistetaan luo lauseen käyttäminen ulkoisiin tietolähteisiin. 

    CREATE EXTERNAL DATA SOURCE RemoteReferenceData 
    WITH 
    ( 
        TYPE=RDBMS, 
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ReferenceData', 
        CREDENTIAL= SqlUser 
    ); 
 
Voit hakea nykyisen ulkoisten tietolähteiden luettelo: 

    select * from sys.external_data_sources; 

### <a name="external-tables"></a>Ulkoiset taulukot 

Syntaksi:

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name . ] table_name  
    ( { <column_definition> } [ ,...n ])     
    { WITH ( <rdbms_external_table_options> ) } 
    )[;] 
    
    <rdbms_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>, 
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 

### <a name="example"></a>Esimerkki  

    CREATE EXTERNAL TABLE [dbo].[customer]( 
        [c_id] int NOT NULL, 
        [c_firstname] nvarchar(256) NULL, 
        [c_lastname] nvarchar(256) NOT NULL, 
        [street] nvarchar(256) NOT NULL, 
        [city] nvarchar(256) NOT NULL, 
        [state] nvarchar(20) NULL, 
        [country] nvarchar(50) NOT NULL, 
    ) 
    WITH 
    ( 
           DATA_SOURCE = RemoteReferenceData 
    ); 

Seuraavassa esimerkissä esitetään, miten Nouda ulkoiset taulukot luettelo nykyisestä tietokannasta: 

    select * from sys.external_tables; 

### <a name="remarks"></a>Huomautuksia

Joustavasti kyselyn laajentaa aiemmin ulkoisen taulukon syntaksi, voit määrittää ulkoisen taulukoita, jotka käyttävät tyypin RDBMS ulkoisiin tietolähteisiin. Ulkoisen taulukon määrittelyn pystysuora jakaminen käsitellään seuraavia ominaisuuksia: 

* **Rakenne**: ulkoisen taulukon DDL määrittää mallin, kyselyt voi käyttää. Ulkoisen taulukon määritys rakenteessa on vastattava todellisten tietojen tallennussijainti etätietokanta taulukoiden rakennetta. 

* **Etätietokanta viite**: ulkoisen taulukon DDL viittaa ulkoisesta tietolähteestä. Ulkoisen tietolähteen määrittää loogisen palvelimen nimi ja tietokannan nimen todellisen taulukon tietojen tallennussijainti etätietokantaan. 

Käytä ulkoista tietolähdettä edellisessä kohdassa kuvatulla, Luo ulkoiset taulukot syntaksi on seuraavanlainen: 

DATA_SOURCE-lause määrittää ulkoisen tietolähteen (eli etätietokannan mahdollisessa pystysuora jakaminen), jota käytetään ulkoisen taulukon.  

SCHEMA_NAME ja Objektin_nimi lauseet on mahdollisuus yhdistää ulkoisen taulukkomääritys-etätietokanta rakenteessa taulukon tai taulukon eri nimellä, tarpeen mukaan. Tämä on kätevä, jos haluat määrittää ulkoisen taulukon luettelon näkymän tai DMV remote tietokannan – tai muussa tilanteessa, jossa remote taulukkonimi on varattu paikallisesti.  

DDL-lause pudottaa olemassa olevan ulkoisen taulukkomäärityksen ja paikallisen luettelon. Se ei vaikuta etätietokantaan. 

    DROP EXTERNAL TABLE [ [ schema_name ] . | schema_name. ] table_name[;]  

**Luo ja PUDOTA ULKOISEN taulukon käyttöoikeudet**: käyttöoikeuksien muuta tahansa ULKOISEEN TIETOLÄHTEESEEN tarvitaan ulkoisen taulukon DDL, joka tarvitaan myös viittaamaan pohjana olevassa tietolähteessä.  

## <a name="security-considerations"></a>Suojausasiat
Käyttäjät voivat käyttää ulkoisen taulukon automaattisesti käyttöösi kohdasta ulkoisen tietolähteen määrityksen alan tunnistetiedon remote taulukoihin. Ulkoisen taulukon käytön olisi hallinta huolellisesti siten vältät toivottavien laajentamisen ulkoisen tietolähteen tunnistetiedon kautta. Tavallinen SQL-käyttöoikeudet avulla voidaan MYÖNTÄÄ tai KUMOTA pääsyn ulkoisen taulukon aivan kuin se on Normaali taulukko.  


## <a name="example-querying-vertically-partitioned-databases"></a>Esimerkki: kyselyt pystysuunnassa osioitu tietokannat 

Seuraava kysely suorittaa kolme tapaa liitoksen remote taulukon ja tilaukset ja rivit paikallisen taulukoiden välille asiakkaille. Tämä on viittaus tietojen Käyttötapaus joustavasti kyselyn Esimerkki: 

    SELECT      
     c_id as customer,
     c_lastname as customer_name,
     count(*) as cnt_orderline, 
     max(ol_quantity) as max_quantity,
     avg(ol_amount) as avg_amount,
     min(ol_delivery_d) as min_deliv_date
    FROM customer 
    JOIN orders 
    ON c_id = o_c_id
    JOIN  order_line 
    ON o_id = ol_o_id and o_c_id = ol_c_id
    WHERE c_id = 100


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

Voit käyttää säännöllisesti SQL Server-yhteyden merkkijonot muodostaaksesi BI ja tietojen integrointi Työkalut SQL DB palvelimessa, jossa on käytössä joustavasti kyselyn ja ulkoiset taulukot, jotka on määritetty. Varmista, että SQL Server on tuettu työkalu tietolähteenä. Lisätietoja sitten joustavasti kysely tietokanta ja sen ulkoisen taulukoiden samalla tavalla kuin minkä tahansa SQL Serverin tietokannan, jotka muodostaa yhteyden oman-työkalulla. 

## <a name="best-practices"></a>Parhaat käytännöt 
 
* Varmista, että joustavasti kyselyn päätepisteen tietokanta on käyttöoikeudet etätietokantaan ottamalla access Azure-palveluiden sen SQL DB palomuurin määrityksen. Varmista myös ulkoisen tietolähteen määrityksen annettuja tunnistetietoja voi onnistuneesti Kirjaudu etätietokanta ja on remote taulukon käyttöoikeuksia.  

* Joustavasti kysely toimii parhaiten kyselyjen kohtaa, johon useimmat laskemisen voidaan toteuttaa remote tietokannat. Saat parhaan kyselyn suorituskyvyn Valikoiva suodattimen predikaatit, joka voi tuottaa tulokseksi remote tietokannat tai liitoksia, jotka voidaan suorittaa kokonaan etätietokanta yleensä. Muut kyselyn kuviot lataat suuria tietomääriä etätietokanta on ehkä ja voi suorittaa huonosti. 


## <a name="next-steps"></a>Seuraavat vaiheet

Kyselyn vaakasuunnassa osioitua tietokantoja (tunnetaan myös nimellä sharded tietokannat), on artikkelissa [kyselyiden sharded cloud-tietokannat (vaakasuunnassa osioitu) kautta](sql-database-elastic-query-horizontal-partitioning.md).

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]


<!--Image references-->
[1]: ./media/sql-database-elastic-query-vertical-partitioning/verticalpartitioning.png


<!--anchors-->
