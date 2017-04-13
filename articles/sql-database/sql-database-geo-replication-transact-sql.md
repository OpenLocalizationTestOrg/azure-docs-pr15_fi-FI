<properties
    pageTitle="Geo replikoinnin määrittäminen Transact-SQL Azure SQL-tietokanta | Microsoft Azure"
    description="Geo replikoinnin määrittäminen käyttämällä Transact-SQL Azure SQL-tietokanta"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="NA"
    ms.date="10/13/2016"
    ms.author="carlrab"/>

# <a name="configure-geo-replication-for-azure-sql-database-with-transact-sql"></a>Geo replikoinnin määrittäminen Transact-SQL Azure SQL-tietokanta

> [AZURE.SELECTOR]
- [Yleiskatsaus](sql-database-geo-replication-overview.md)
- [Azure Portal](sql-database-geo-replication-portal.md)
- [PowerShellin](sql-database-geo-replication-powershell.md)
- [T-SQL](sql-database-geo-replication-transact-sql.md)

Tässä artikkelissa kerrotaan, miten Active Geo-replikoinnin määrittämiseksi Transact-SQL Azure SQL-tietokantaan.

Aloittaa automaattisesti käyttämällä Transact-SQL-kohdassa [Aloita suunniteltu tai suunnittelematon automaattisesti Azure SQL-tietokantaan Transact - SQL](sql-database-geo-replication-failover-transact-sql.md).

>[AZURE.NOTE] Kaikki palvelutasot kaikki tietokannat on nyt aktiivinen Geo-replikoinnin (luettavissa secondaries). Huhtikuussa 2017-ei voi lukea toissijainen tyyppi poistua ja ei voi lukea aiemmin luotujen tietokantojen päivitetään automaattisesti luettavissa secondaries.

Määritä aktiivinen Geo-replikoinnin käyttämällä Transact-SQL-tarvitset seuraavat:

- Azure tilaus.
- Loogisen Azure SQL-tietokanta palvelimen <MyLocalServer> ja SQL-tietokannan <MyDB> -ensisijainen tietokanta, jonka haluat kopioida.
- Vähintään yksi looginen Azure SQL-tietokanta palvelimet < MySecondaryServer(n) > - looginen, jotka ovat kumppanin palvelimissa, johon luot toissijaisen tietokantoja.
- Kirjautuminen, joka on määritetty ensisijaiseksi DBManager on se geo-toisinto paikallisen tietokannan db_ownership ja olla DBManager, johon määrität Geo replikoinnin kumppanin palvelimesta.
- SQL Server Management Studiossa (SSMS)

> [AZURE.IMPORTANT] On suositeltavaa, että käytät Management Studiossa uusimman version aina pysyvän synkronoituja Microsoft Azure ja SQL-tietokantaan. [Päivitä SQL Server Management Studiossa](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="add-secondary-database"></a>Lisää toissijainen tietokanta

Voit **Muuttaa DATABASE** -lause geo replikoida toisen tietokannan luominen kumppanin palvelimessa. Tämä lause suorittaa palvelimeen, jossa voi replikoida tietokannan master-tietokantaan. Geo replikoida-tietokanta ("Ensisijainen tietokanta") on sama nimi kuin replikoinnin estäminen tietokannan, ja se on oletusarvoisesti ensisijaisen tietokannan palvelun samalla tasolla. Toissijaisen tietokanta voidaan lukea tai ei voi lukea ja voi olla yhden tietokannan tai joustavasti databbase. Lisätietoja on artikkelissa [Muuta TIETOKANTAA (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) ja [Palvelutasot](sql-database-service-tiers.md).
Kun toissijaisen tietokanta on luotu ja kylvetään, tietojen alkaa replikoiminen asynkronisesti ensisijainen tietokannasta. Seuraavia ohjeita kerrotaan, kuinka voit määrittää Geo replikoinnin Management Studiossa. Toimintaohjeet ei voi lukea ja helppotajuisesti secondaries yhteen tietokantaan tai joustavasti tietokannan luomiseen on käytettävissä.

> [AZURE.NOTE] Jos tietokanta on olemassa määritetty kumppanin kanssa on sama nimi kuin ensisijaisen tietokannan palvelimessa komento epäonnistuu.


### <a name="add-non-readable-secondary-single-database"></a>Lisää ei voi lukea toissijainen (yksi-tietokanta)

Seuraavien vaiheiden avulla voit luoda yksittäisen tietokantana ei voi lukea toissijainen.

1. 13.0.600.65 versio tai sitä uudempi versio SQL Server Management Studiossa.

     > [AZURE.IMPORTANT] Lataa SQL Server Management Studiossa [uusimman](https://msdn.microsoft.com/library/mt238290.aspx) version. On suositeltavaa, että käytät Management Studiossa uusimman version aina pysyvän synkronoituina muutoksia Azure-portaaliin.


2. Tietokantojen-kansion avaaminen, **Järjestelmän tietokantojen** -kansion laajentaminen, napsauta **perusmuodon**hiiren kakkospainikkeella ja valitse sitten **Uusi kysely**.

3. Seuraavat **ALTER DATABASE** -lause avulla voit tehdä paikalliseen tietokantaan Geo-replikoinnin ensisijainen MySecondaryServer1 missä MySecondaryServer1 helpossa muodossa palvelimen nimeä ei voi lukea toissijainen tietokannan kanssa.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer1> WITH (ALLOW_CONNECTIONS = NO);

4. Valitse **Suorita** kysely.


### <a name="add-readable-secondary-single-database"></a>Lisää luettavissa toissijainen (yksi-tietokanta)
Seuraavien vaiheiden avulla voit luoda yksittäisen tietokantana luettavissa toissijainen.

1. Muodostaa Management Studiossa looginen Azure SQL-tietokanta-palvelimeen.

2. Tietokantojen-kansion avaaminen, **Järjestelmän tietokantojen** -kansion laajentaminen, napsauta **perusmuodon**hiiren kakkospainikkeella ja valitse sitten **Uusi kysely**.

3. Seuraavat **ALTER DATABASE** -lause avulla voit tehdä paikalliseen tietokantaan Geo-replikoinnin ensisijainen luettavissa toissijainen tietokannan toissijainen palvelimessa.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer2> WITH (ALLOW_CONNECTIONS = ALL);

4. Valitse **Suorita** kysely.



### <a name="add-non-readable-secondary-elastic-database"></a>Lisää ei voi lukea toissijainen (joustavasti tietokanta)

Seuraavien vaiheiden avulla voit luoda ei voi lukea toissijainen joustavasti tietokantana.

1. Muodostaa Management Studiossa looginen Azure SQL-tietokanta-palvelimeen.

2. Tietokantojen-kansion avaaminen, **Järjestelmän tietokantojen** -kansion laajentaminen, napsauta **perusmuodon**hiiren kakkospainikkeella ja valitse sitten **Uusi kysely**.

3. Seuraavat **ALTER DATABASE** -lause avulla voit tehdä paikalliseen tietokantaan Geo-replikoinnin ensisijainen ei voi lukea toissijainen tietokannan joustavasti varannon toissijainen palvelimessa.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer3> WITH (ALLOW_CONNECTIONS = NO
           , SERVICE_OBJECTIVE = ELASTIC_POOL (name = MyElasticPool1));

4. Valitse **Suorita** kysely.



### <a name="add-readable-secondary-elastic-database"></a>Lisää luettavissa toissijainen (joustavasti tietokanta)
Seuraavien vaiheiden avulla voit luoda luettavissa toissijainen joustavasti tietokantana.

1. Muodostaa Management Studiossa looginen Azure SQL-tietokanta-palvelimeen.

2. Tietokantojen-kansion avaaminen, **Järjestelmän tietokantojen** -kansion laajentaminen, napsauta **perusmuodon**hiiren kakkospainikkeella ja valitse sitten **Uusi kysely**.

3. Seuraavat **ALTER DATABASE** -lause avulla voit tehdä paikalliseen tietokantaan Geo-replikoinnin ensisijainen luettavissa toissijainen tietokannan toissijainen palvelimessa joustavasti varannon.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer4> WITH (ALLOW_CONNECTIONS = ALL
           , SERVICE_OBJECTIVE = ELASTIC_POOL (name = MyElasticPool2));

4. Valitse **Suorita** kysely.



## <a name="remove-secondary-database"></a>Poista toissijaisen tietokanta

Voit lopettaa pysyvästi toissijainen tietokannan ja sen ensisijainen replikoinnin-partnership **ALTER DATABASE** -lause. Tämä lause on suoritettu perusmuodon tietokannan, jossa ensisijaisen tietokanta sijaitsee. Yhteyden päätyttyä toissijaisen tietokanta tulee tavallinen luku-ja kirjoitusoikeudet tietokannan. Jos yhteys toissijainen tietokantaan katkeaa komento onnistuu, mutta toissijaisen tulee luku-ja kirjoitusoikeudet, kun yhteys on palautettu. Lisätietoja on artikkelissa [Muuta TIETOKANTAA (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) ja [Palvelutasot](sql-database-service-tiers.md).

Seuraavien vaiheiden avulla voit poistaa geo replikoida toissijainen Geo replikoinnin partnership.

1. Muodostaa Management Studiossa looginen Azure SQL-tietokanta-palvelimeen.

2. Tietokantojen-kansion avaaminen, **Järjestelmän tietokantojen** -kansion laajentaminen, napsauta **perusmuodon**hiiren kakkospainikkeella ja valitse sitten **Uusi kysely**.

3. Poista geo replikoida toissijainen seuraavat **ALTER DATABASE** -lause avulla.

        ALTER DATABASE <MyDB>
           REMOVE SECONDARY ON SERVER <MySecondaryServer1>;

4. Valitse **Suorita** kysely.

## <a name="monitor-geo-replication-configuration-and-health"></a>Näytön Geo replikoinnin määritys ja kunto

Seurannan tehtäviin kuuluu seuranta Geo replikoinnin-määritys ja tietojen replikoinnin kunnon valvonta.  Voit **sys.dm_geo_replication_links** dynaaminen hallinta-näkymässä perusmuodon tietokannan palauttamaan tietoja kaikkien tietokantojen sulkeminen replikoinnin kaikki linkit looginen Azure SQL-tietokanta-palvelimeen. Tämä näkymä sisältää rivin kunkin ensisijaisen ja toissijaisen tietokantojen replikointi linkityksen. Voit palauttaa rivin kunkin Azure SQL-tietokantaan, joka on tällä hetkellä mielenkiinnon paremmin yllä replikoinnin replikoinnin linkin **sys.dm_replication_link_status** dynaaminen hallinta-näkymässä. Tämä sisältää sekä ensisijaisen ja toissijaisen tietokannat. Jos useamman kuin yhden jatkuvan replikoinnin linkki tietyn ensisijainen tietokannan, tämä taulukko sisältää rivin kunkin suhteet. Näkymän luodaan kaikki tietokannat, kuten loogiset perustyyli. Kuitenkin kyselyt tässä näkymässä looginen perustyylinäkymässä palauttaa tyhjän joukon. Voit esittää kaikki tietokannan toiminnot, kuten replikoinnin linkkien tilan **sys.dm_operation_status** dynaaminen hallinta-näkymässä. Lisätietoja on artikkelissa [sys.geo_replication_links (Azure SQL-tietokanta)](https://msdn.microsoft.com/library/mt575501.aspx), [sys.dm_geo_replication_link_status (Azure SQL-tietokanta)](https://msdn.microsoft.com/library/mt575504.aspx)ja [sys.dm_operation_status (Azure SQL-tietokanta)](https://msdn.microsoft.com/library/dn270022.aspx).

Seuraavien vaiheiden avulla voit valvoa Geo replikoinnin partnership.

1. Muodostaa Management Studiossa looginen Azure SQL-tietokanta-palvelimeen.

2. Tietokantojen-kansion avaaminen, **Järjestelmän tietokantojen** -kansion laajentaminen, napsauta **perusmuodon**hiiren kakkospainikkeella ja valitse sitten **Uusi kysely**.

3. Näytä kaikki tietokannat Geo replikoinnin linkit seuraavan lauseen avulla.

        SELECT database_id, start_date, modify_date, partner_server, partner_database, replication_state_desc, role, secondary_allow_connections_desc FROM [sys].geo_replication_links;

4. Valitse **Suorita** kysely.
5. Tietokantojen-kansion avaaminen, laajenna **Järjestelmän tietokantojen** -kansio, napsauta **MyDB**hiiren kakkospainikkeella ja valitse sitten **Uusi kysely**.
6. Seuraavan lauseen avulla voit näyttää replikoinnin ennusteiden ja edellisen replikoinnin oma toissijainen tietokantojen MyDB aikaan.

        SELECT link_guid, partner_server, last_replication, replication_lag_sec FROM sys.dm_geo_replication_link_status

7. Valitse **Suorita** kysely.
8. Näytä viimeisimmän geo replikoinnin-toimintoja, jotka liittyvät tietokannan MyDB seuraavan lauseen avulla.

        SELECT * FROM sys.dm_operation_status where major_resource_id = 'MyDB'
        ORDER BY start_time DESC

9. Valitse **Suorita** kysely.

## <a name="upgrade-a-non-readable-secondary-to-readable"></a>Päivittäminen ei voi lukea toissijainen luettavissa

Huhtikuussa 2017-ei voi lukea toissijainen tyyppi poistua ja ei voi lukea aiemmin luotujen tietokantojen päivitetään automaattisesti luettavissa secondaries. Jos käytät ei voi lukea secondaries tänään ja haluat päivittää niitä voi lukea, voit yksinkertainen seuraavasti kunkin toissijainen.

> [AZURE.IMPORTANT] Ei ole käytönaikainen päivitetään, voit lukea ei voi lukea toissijainen Omatoiminen-menetelmää. Jos poistat vain toissijainen, valitse ensisijaisen tietokannan on kunnes suojaamaton uusi toissijaisen on täysin synkronoitu. Jos sovelluksen SLA edellyttää, että ensisijainen on aina suojattu, ota huomioon rinnakkain toissijainen luominen eri palvelimen ennen käyttöä päivityksen vaiheet. Huomautus kunkin ensisijainen voi olla enintään 4 toissijainen tietokantaa.


1. Ensin *toissijaisen* palvelimen yhdistäminen ja ei voi lukea toissijainen tietokannan poistaminen käytöstä:  
        
        DROP DATABASE <MyNonReadableSecondaryDB>;

2. Nyt yhdistäminen *ensisijainen* palvelin ja Lisää uusi luettavissa toissijainen

        ALTER DATABASE <MyDB>
            ADD SECONDARY ON SERVER <MySecondaryServer> WITH (ALLOW_CONNECTIONS = ALL);




## <a name="next-steps"></a>Seuraavat vaiheet

- Tietoja Active Geo-replikoinnin, artikkelissa - [Aktiivinen Geo-replikointi](sql-database-geo-replication-overview.md)
- Liiketoiminnan jatkuvuus-yleiskatsaus ja tilanteita, joissa on artikkelissa [liiketoiminnan jatkuvuuden yhteenveto](sql-database-business-continuity.md)
