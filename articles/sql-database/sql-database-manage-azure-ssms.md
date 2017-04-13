<properties 
    pageTitle="Hallitse SQL-tietokanta on SSMS | Microsoft Azure" 
    description="Opettele käyttämään SQL Server Management Studiossa SQL-tietokanta-palvelimia ja tietokantojen hallinta." 
    services="sql-database" 
    documentationCenter=".net" 
    authors="stevestein" 
    manager="jhubbard" 
    editor="tysonn"/>

<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/29/2016" 
    ms.author="sstein"/>


# <a name="managing-azure-sql-database-using-sql-server-management-studio"></a>Azure SQL-tietokanta SQL Server Management Studiossa hallinta 


> [AZURE.SELECTOR]
- [Azure portal](sql-database-manage-portal.md)
- [SSMS](sql-database-manage-azure-ssms.md)
- [PowerShellin](sql-database-manage-powershell.md)

SQL Server Management Studiossa (SSMS) avulla voit hallita Azure SQL-tietokanta-palvelimia ja tietokantoja. Tässä ohjeaiheessa esitellään yleisten tehtävien SSMS. On jo luotu Azure SQL-tietokantaan, ennen kuin aloitat tietokantaan ja palvelimeen. Lisätietoja on kohdassa [ensimmäisen Azure SQL-tietokannan luominen](sql-database-get-started.md) ja [Yhdistä- ja kysely, jossa SSMS](sql-database-connect-query-ssms.md) .

On suositeltavaa, että käytät SSMS uusimman version käsiteltäessä Azure SQL-tietokantaan. 

> [AZURE.IMPORTANT] Käytä aina SSMS uusimman version, koska se on parannettu jatkuvasti uusimmat päivitykset ja Azure SQL-tietokanta-käyttöä varten. Uusimman version asentamisesta [Lataa SQL Server Management Studiossa](https://msdn.microsoft.com/library/mt238290.aspx).



## <a name="create-and-manage-azure-sql-databases"></a>Voit luoda ja hallita Azure SQL-tietokannat

Yhdistettynä **perusmuodon** tietokantaan, voit luoda tietokantojen palvelimessa ja muokkaa tai poista aiemmin luotujen tietokantojen. Seuraavat vaiheet kerrotaan, kuinka voit tehdä useita yleisiä tietokannan hallintatehtäviä Management Studiossa kautta. Näiden tehtävien suorittamiseen, varmista, että olet muodostanut yhteyden palvelintason pääasiallista sisäänkirjautumissivulle, jonka loit, kun määrität palvelimesi **perusmuodon** tietokannan.

Avaa kyselyikkunan Management Studiossa, tietokannat-kansion avaaminen, **Järjestelmän tietokantojen** -kansion laajentaminen, napsauta **perusmuodon**hiiren kakkospainikkeella ja valitse sitten **Uusi kysely**.

-   **Luo DATABASE** -lause avulla voit luoda tietokannan. Lisätietoja on artikkelissa [CREATE DATABASE (SQL-tietokanta)](https://msdn.microsoft.com/library/dn268335.aspx). Seuraavan lauseen Luo **myTestDB** -tietokannan, ja määrittää, että se on S0 Standard Edition tietokannan oletusarvoista enimmäiskoko 250 gigatavua.

        CREATE DATABASE myTestDB
        (EDITION='Standard',
         SERVICE_OBJECTIVE='S0');

Valitse **Suorita** kysely.

-   **ALTER DATABASE** -lause avulla voit muokata aiemmin luotuun tietokantaan, esimerkiksi jos haluat muuttaa, nimi ja tietokannan versio. Lisätietoja on artikkelissa [Muuta TIETOKANTAA (SQL-tietokanta)](https://msdn.microsoft.com/library/ms174269.aspx). Seuraavan lauseen Muokkaa tietokantaan, voit muuttaa edellisessä vaiheessa luomasi Standard S1 edition.

        ALTER DATABASE myTestDB
        MODIFY
        (SERVICE_OBJECTIVE='S1');

-   **Poista tietokanta** lauseen avulla voit poistaa aiemmin luotuun tietokantaan. Lisätietoja on artikkelissa [Poista tietokanta (SQL-tietokanta)](https://msdn.microsoft.com/library/ms178613.aspx). Seuraavan lauseen poistaa **myTestDB** tietokannan, mutta älä pudota se nyt koska sitä käytetään kirjautumiset luominen seuraavaan vaiheeseen.

        DROP DATABASE myTestBase;

-   Pää-tietokannassa on **sys.databases** näkymä, jonka avulla voit tarkastella tietoja kaikki tietokannat. Voit tarkastella kaikkia aiemmin luotujen tietokantojen, suorita seuraava komento:

        SELECT * FROM sys.databases;

-   SQL-tietokantaan **Käytä** -lauseessa ei tue tietokantojen välillä siirtymistä. Haluat muodostaa yhteyden suoraan kohdetietokanta.

>[AZURE.NOTE] Monia Transact-SQL-lauseita, voit luoda tai muokata tietokannan on suoritettava oma erä ja ei voi ryhmitellä muiden Transact-SQL-lauseita. Lisätietoja on artikkelissa lauseen koskevia tietoja.

## <a name="create-and-manage-logins"></a>Voit luoda ja hallita kirjautumiset

**Perusmuodon** tietokanta sisältää kirjautumiset ja mitkä kirjautumiset on oikeus luoda tietokantoja tai muita kirjautumiset. Hallitse kirjautumiset yhdistämällä palvelintason pääasiallista sisäänkirjautumissivulle, jonka loit, kun määrität palvelimesi **perusmuodon** tietokantaan. Voit **Luoda kirjautuminen**, **Muuta kirjautuminen**tai **PUDOTA kirjautuminen** lauseet suorittamaan kyselyitä, jotka perustuvat perusmuodon tietokantaan, joka hallitsee kirjautumiset yli koko palvelimelle. Lisätietoja [tietokantojen hallinta ja kirjautumiset SQL-tietokantaan](http://msdn.microsoft.com/library/azure/ee336235.aspx). 


-   **Luo kirjautuminen** lauseella palvelintason kirjautumisen luominen. Lisätietoja on artikkelissa [Luo LOGIN (SQL-tietokanta)](https://msdn.microsoft.com/library/ms189751.aspx). Seuraava lauseke Luo kutsutaan **login1**kirjautumisen. Korvaa **Salasana1** valittua salasana.

        CREATE LOGIN login1 WITH password='password1';

-   **Luo USER** -lause avulla voit myöntää tietokannan tason käyttöoikeudet. Kaikki kirjautumiset on luotava **perusmuodon** tietokannan. Yhteyden muodostaminen eri tietokantaan kirjautumiset-Myönnä sen tietokannan tason käyttöoikeudet käyttämällä tietokannan **Luominen USER** -lause. Lisätietoja on artikkelissa [Luo käyttäjä (SQL-tietokanta)](https://msdn.microsoft.com/library/ms173463.aspx). 

-   Jos haluat antaa login1 käyttöoikeudet **myTestDB**-tietokantaan, toimimalla seuraavasti:

 1.  Objektin Explorer tarkastella **myTestDB** tietokannan, jonka loit päivittämiseen hiiren kakkospainikkeella objektia Explorer palvelimen nimi ja valitse sitten **Päivitä**.  

     Jos yhteys on suljettu, voit muodostaa **Yhteyden Object Explorerissa** valitsemalla Tiedosto-valikosta.

 2. Napsauta **myTestDB** tietokanta ja valitse **Uusi kysely**.

    3.  Suorittaa seuraavan lauseen myTestDB tietokannalle Luo nimeltä **login1User** , joka vastaa palvelintason kirjautuminen **login1**tietokannan käyttäjä.

            CREATE USER login1User FROM LOGIN login1;

-   Käytä **sp\_addrolemember** tallennetun toimintosarjan käyttäjätilille tason oikeudet-tietokantaan. Lisätietoja on artikkelissa [sp_addrolemember (Transact-SQL)](http://msdn.microsoft.com/library/ms187750.aspx). Seuraavan lauseen antavat **login1User** vain luku-oikeudet tietokantaan **login1User** ja lisäämällä **db\_datareader** rooli.

        exec sp_addrolemember 'db_datareader', 'login1User';    

-   **Muuta kirjautuminen** -lauseella voidaan muokata aiemmin kirjautuminen, esimerkiksi jos haluat vaihtaa salasanan sisäänkirjautumista varten. Lisätietoja on artikkelissa [Muuta LOGIN (SQL-tietokanta)](https://msdn.microsoft.com/library/ms189828.aspx). **Muuta kirjautuminen** -lause on suoritettava **perusmuodon** tietokannalle. Siirry takaisin kyselyikkunasta, joka on yhteydessä tietokantaan. Seuraavan lauseen Muokkaa **login1** sisäänkirjautumissivulle salasanan. Korvaa **Uusisalasana** valinta ja **Vanha_salasana** salasana, jolla nykyinen salasana sisäänkirjautumista varten.

        ALTER LOGIN login1
        WITH PASSWORD = 'newPassword'
        OLD_PASSWORD = 'oldPassword';

-   **PUDOTA kirjautuminen** ilmoituksen avulla voit poistaa aiemmin kirjautuminen. Kirjautuminen palvelimen tasolla poistaminen poistaa myös kaikki käyttäjätilit, liittyvän tietokannan. Lisätietoja on artikkelissa [Poista tietokanta (SQL-tietokanta)](https://msdn.microsoft.com/library/ms178613.aspx). **PUDOTA kirjautuminen** -lause on suoritettava **perusmuodon** tietokannalle. Lause poistaa **login1** kirjautuminen.

        DROP LOGIN login1;

-   Pää-tietokannassa on **sys.sql\_kirjautumiset** Näytä, että voit tarkastella kirjautumiset. Jos haluat nähdä kaikki olemassa olevat kirjautumiset, suorittaa seuraavan lauseen:

        SELECT * FROM sys.sql_logins;

## <a name="monitor-sql-database-using-dynamic-management-views"></a>Valvoa SQL-tietokantaan dynaaminen hallinta-näkymien käyttäminen

SQL-tietokanta tukee useita dynaaminen hallinta näkymästä, joiden avulla voit seurata yksittäisiä tietokannan. Seuraavat muutamia esimerkkejä näiden näkymien avulla voit hakea näytön tietojen tyypin. Kaikki tiedot ja muita käyttöesimerkkejä on artikkelissa [dynaaminen hallinta-näkymien käyttäminen seuranta SQL-tietokantaan](https://msdn.microsoft.com/library/azure/ff394114.aspx).

-   **Näytä TIETOKANNAN tilan** käyttöoikeudet edellyttää kyselyt dynaaminen hallinta-näkymässä. **Näytä TIETOKANNAN tilan** oikeuden myöntäminen tietokannan avaamiseksi käyttäjälle, muodosta yhteys tietokantaan ja suorita tietokannassa seuraavan lauseen:

        GRANT VIEW DATABASE STATE TO login1User;

-   Tietokannan koon käyttämällä Laske **sys.dm\_db\_osion\_tilasto** näkymä. **Sys.dm\_db\_osion\_tilasto** näkymä palauttaa sivun ja rivien lukumäärä tiedot jokaiseen osioon tietokannan, jonka avulla voit laskea tietokannan koon. Seuraava kysely palauttaa tietokannan koko megatavuina:

        SELECT SUM(reserved_page_count)*8.0/1024
        FROM sys.dm_db_partition_stats;   

-   Käytä **sys.dm\_johto\_yhteydet** ja **sys.dm\_johto\_istuntojen** näkymien tietoja nykyisen käyttäjän yhteydet ja sisäinen tietokannan liittyvät tehtävät. Seuraava kysely palauttaa nykyisen yhteyden tietoja:

        SELECT
            e.connection_id,
            s.session_id,
            s.login_name,
            s.last_request_end_time,
            s.cpu_time
        FROM
            sys.dm_exec_sessions s
            INNER JOIN sys.dm_exec_connections e
              ON s.session_id = e.session_id;

-   Käytä **sys.dm\_johto\_kyselyn\_tilasto** kooste suorituskyvyn tilastotiedot Näytä välimuistiin kyselyn palvelupaketin. Seuraava kysely palauttaa tietoja luokitellut keskimääräinen suorittimen ajan mukaan viisi kyselyjä.

        SELECT TOP 5 query_stats.query_hash AS "Query Hash",
            SUM(query_stats.total_worker_time), SUM(query_stats.execution_count) AS "Avg CPU Time",
            MIN(query_stats.statement_text) AS "Statement Text"
        FROM
            (SELECT QS.*,
            SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
            ((CASE statement_end_offset
                WHEN -1 THEN DATALENGTH(ST.text)
                ELSE QS.statement_end_offset END
                    - QS.statement_start_offset)/2) + 1) AS statement_text
             FROM sys.dm_exec_query_stats AS QS
             CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
        GROUP BY query_stats.query_hash
        ORDER BY 2 DESC;
 
 
