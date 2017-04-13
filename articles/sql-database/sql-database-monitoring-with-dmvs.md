<properties
   pageTitle="Seurannan dynaamisen hallinta-näkymien käyttäminen Azure SQL-tietokanta | Microsoft Azure"
   description="Opettele tunnistamaan ja yleisiä suorituskykyongelmien vianmääritys dynaaminen hallinta näkymien avulla voit valvoa Microsoft Azure SQL-tietokanta."
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/20/2016"
   ms.author="carlrab"/>

# <a name="monitoring-azure-sql-database-using-dynamic-management-views"></a>Seurannan Azure SQL-tietokanta dynaaminen hallinta-näkymien käyttäminen

Microsoft Azure SQL-tietokanta mahdollistaa alijoukkoa dynaaminen hallinta näkymien vianmääritys suorituskykyongelmia, joka voi johtua kyselyt estetystä tai pitkäkestoisia, resurssi pullonkaulojen, heikko kyselyn suunnitelmien ja niin edelleen. Tämä artikkeli sisältää tiedot siitä, miten esiintyvien yleisten suorituskykyongelmia dynaaminen hallinta näkymien avulla.

SQL-tietokanta tukee osittain dynaaminen hallinta näkymien kolmeen luokkaan:

- Tietokannan liittyvät dynaaminen hallinta näkymät.
- Suorittamisen liittyvät dynaaminen hallinta näkymät.
- Tapahtuman liittyvät dynaaminen hallinta näkymät.

Lisätietoja dynaamisen hallinta näkymissä on [dynaamisia hallinta näkymiä ja funktioita (Transact-SQL)](https://msdn.microsoft.com/library/ms188754.aspx) SQL Server Books Online.

## <a name="permissions"></a>Käyttöoikeudet

SQL-tietokantaan kyselyt dynaaminen hallinta-näkymässä edellyttää **NÄKYMÄN TIETOKANNAN tilan** oikeuksia. **Näytä TIETOKANNAN tilan** oikeudet palauttaa nykyisen tietokannan kaikki objektit tietoja.
**Näytä TIETOKANNAN tilan** oikeuden myöntäminen tietokannan avaamiseksi käyttäjälle, suorita seuraava kysely:

```GRANT VIEW DATABASE STATE TO database_user; ```

Paikallisen SQL Server-esiintymän dynaamisen hallinta näkymien palauttaa palvelimen tiedot. SQL-tietokantaan ne palauttavat koskevat vain nykyistä looginen tietokannan tiedot.

## <a name="calculating-database-size"></a>Tietokannan koon laskeminen

Seuraava kysely palauttaa tietokannan (megatavuina) koko:

```
-- Calculates the size of the database.
SELECT SUM(reserved_page_count)*8.0/1024
FROM sys.dm_db_partition_stats;
GO
```

Seuraava kysely palauttaa tietokannan koon yksittäiset objektit (megatavuina):

```
-- Calculates the size of individual database objects.
SELECT sys.objects.name, SUM(reserved_page_count) * 8.0 / 1024
FROM sys.dm_db_partition_stats, sys.objects
WHERE sys.dm_db_partition_stats.object_id = sys.objects.object_id
GROUP BY sys.objects.name;
GO
```

## <a name="monitoring-connections"></a>Yhteyksien seuranta

Voit noutaa tietoja tiettyyn Azure SQL-tietokanta palvelimeen ja kunkin yhteyden tietoja yhteyksiä [sys.dm_exec_connections](https://msdn.microsoft.com/library/ms181509.aspx) -näkymä. Lisäksi [sys.dm_exec_sessions](https://msdn.microsoft.com/library/ms176013.aspx) näkymä on hyötyä, kun kaikki aktiivisen käyttäjän yhteydet ja sisäinen tehtävien tietoja haetaan.
Seuraava kysely palauttaa nykyisen yhteyden tietoja:

```
SELECT
    c.session_id, c.net_transport, c.encrypt_option,
    c.auth_scheme, s.host_name, s.program_name,
    s.client_interface_name, s.login_name, s.nt_domain,
    s.nt_user_name, s.original_login_name, c.connect_time,
    s.login_time
FROM sys.dm_exec_connections AS c
JOIN sys.dm_exec_sessions AS s
    ON c.session_id = s.session_id
WHERE c.session_id = @@SPID;
```

> [AZURE.NOTE] Suoritettaessa **sys.dm_exec_requests** ja **sys.dm_exec_sessions näkymiä**, jos tietokanta on **Näytä TIETOKANNAN tilan** käyttöoikeudet, näet kaikki suoritetaan istuntojen tietokantaan. Muussa tapauksessa näet vain nykyisessä istunnossa.

## <a name="monitoring-query-performance"></a>Kyselyn suorituskyvyn valvominen

Hidas tai pitkä käynnissä kyselyt voivat käyttää merkittäviä järjestelmäresursseja. Tässä osassa kerrotaan, miten tunnistaa muutamia yleisiä kyselyn suorituskykyongelmia dynaaminen hallinta näkymien avulla. Vanhempi mutta silti hyödyllisiä viittaus vianmääritystä varten on Microsoft TechNet [SQL Server 2008: n suorituskykyongelmien vianmääritys](http://download.microsoft.com/download/D/B/D/DBDE7972-1EB9-470A-BA18-58849DB3EB3B/TShootPerfProbs2008.docx) -artikkelissa.

### <a name="finding-top-n-queries"></a>Ylimmät N kyselyjen etsiminen

Seuraava esimerkki palauttaa keskiarvo suorittimen ajan mukaan luokitellut viisi kyselyjä tietoja. Tässä esimerkissä kokoaa mukaan kyselyn niiden hash kyselyt niin, että niiden kumulatiivinen resurssin kulutus on ryhmitelty loogisesti vastaavat kyselyt.

```
SELECT TOP 5 query_stats.query_hash AS "Query Hash",
    SUM(query_stats.total_worker_time) / SUM(query_stats.execution_count) AS "Avg CPU Time",
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
```

### <a name="monitoring-blocked-queries"></a>Kyselyt estetystä seuranta

Hidas tai pitkäkestoisia kyselyt voi osallistua liiallinen resurssin kulutus ja mahdollinen kyselyt estetystä. Eston syy voi olla heikko sovelluksen rakenne, virheellinen kyselyn suunnitelmat, puutteen hyödyllisiä indeksit ja niin edelleen. Sys.dm_tran_locks-tarkastella nykyisen lukitsemalla toiminnon tietojen saaminen Azure SQL-tietokantaan. Esimerkiksi koodi, SQL Server Books Online-artikkelissa [sys.dm_tran_locks (Transact-SQL)](https://msdn.microsoft.com/library/ms190345.aspx) .

### <a name="monitoring-query-plans"></a>Kyselyn suunnitelmien seuranta

Tehoton kyselyn suunnitelman saattavat parantua myös suorittimen kulutus. Seuraavassa esimerkissä [sys.dm_exec_query_stats](https://msdn.microsoft.com/library/ms189741.aspx) -näkymässä voit selvittää, mitkä Queryn kaikkein kumulatiivisen suorittimen.

```
SELECT
    highest_cpu_queries.plan_handle,
    highest_cpu_queries.total_worker_time,
    q.dbid,
    q.objectid,
    q.number,
    q.encrypted,
    q.[text]
FROM
    (SELECT TOP 50
        qs.plan_handle,
        qs.total_worker_time
    FROM
        sys.dm_exec_query_stats qs
    ORDER BY qs.total_worker_time desc) AS highest_cpu_queries
    CROSS APPLY sys.dm_exec_sql_text(plan_handle) AS q
ORDER BY highest_cpu_queries.total_worker_time DESC;
```

## <a name="see-also"></a>Katso myös

[Johdanto SQL-tietokantaan](sql-database-technical-overview.md)
