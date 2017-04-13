<properties
   pageTitle="Valvoa havainnollistamiseen käyttämällä DMVs | Microsoft Azure"
   description="Opettele valvoa havainnollistamiseen DMVs avulla."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/08/2016"
   ms.author="sonyama;barbkess"/>

# <a name="monitor-your-workload-using-dmvs"></a>Valvoa havainnollistamiseen DMVs käyttäminen

Tässä artikkelissa käsitellään Valvo havainnollistamiseen ja tutkia kyselyn suoritus Azure SQL-tietovarasto dynaaminen hallinta näkymien (DMVs) avulla.

## <a name="permissions"></a>Käyttöoikeudet

Kyselyn tämän artikkelin DMVs, tarvitset Näytä TIETOKANNAN tilan tai OHJAUSOBJEKTIN käyttöoikeus. Yleensä myöntäminen Näytä TIETOKANNAN tila on ensisijainen käyttöoikeus on paljon hyvin rajoittava.

```sql
GRANT VIEW DATABASE STATE TO myuser;
```

## <a name="monitor-connections"></a>Näytön yhteydet

SQL-tietovarasto kaikki kirjautumiset kirjataan [sys.dm_pdw_exec_sessions][].  Tämä DMV on viimeksi 10 000 kirjautumiset.  Session_id on perusavain ja määritetään peräkkäin kunkin uuden sisäänkirjautumista varten.

```sql
-- Other Active Connections
SELECT * FROM sys.dm_pdw_exec_sessions where status <> 'Closed' and session_id <> session_id();
```

## <a name="monitor-query-execution"></a>Näytön kyselyn suorittaminen

SQL-tietovarasto suorittaa kaikki kyselyt kirjataan [sys.dm_pdw_exec_requests][].  Tämä DMV on suoritettu viimeksi 10 000 kyselyt.  Request_id yksilöi kummankin kyselyn ja on tämän DMV perusavain.  Request_id määritetään peräkkäin kunkin uuden kyselyn ja alussa on QID, joka tarkoittaa kyselyn ID-tunnuksellasi.  Tämä DMV varten tietyn session_id kyselyt näkyvät kaikki kyselyt annetun kirjautumista varten.

>[AZURE.NOTE] Tallennettujen toimintosarjojen käyttää useita pyytää tunnukset.  Pyyntö tunnukset määritetään järjestyksessä. 

Seuraavassa on ohjeita noudattamalla voit tutkia kyselyn suorituksen suunnitelmien ja kellonajat tietyn kyselyn.

### <a name="step-1-identify-the-query-you-wish-to-investigate"></a>Vaihe 1: Määritä haluat tutkia kysely

```sql
-- Monitor active queries
SELECT * 
FROM sys.dm_pdw_exec_requests 
WHERE status not in ('Completed','Failed','Cancelled')
  AND session_id <> session_id()
ORDER BY submit_time DESC;

-- Find top 10 queries longest running queries
SELECT TOP 10 * 
FROM sys.dm_pdw_exec_requests 
ORDER BY total_elapsed_time DESC;

-- Find a query with the Label 'My Query'
-- Use brackets when querying the label column, as it it a key word
SELECT  *
FROM    sys.dm_pdw_exec_requests
WHERE   [label] = 'My Query';
```

Edellisen kyselyn tuloksista, jotka haluat tutkia kyselyn **Huomautus pyytää tunnus** .

Kyselyjen **Suspended** tila on käytössä jonossa samanaikainen rajoitusten vuoksi. Nämä kyselyt näkyvät myös sys.dm_pdw_waits odottaa kysely, jossa UserConcurrencyResourceType tyyppi. Katso lisätietoja samanaikainen rajoitukset [samanaikainen ja työmäärää][] . Kyselyjen voit myös odottaa objektin lukitukset, kuten muista syistä.  Jos kyselyn odottaa resurssin, katso [Investigating kyselyjen Odotetaan resursseja][] edelleen alaspäin ohjeaiheen.

Helpottaa sys.dm_pdw_exec_requests taulukon kyselyn haun avulla voit määrittää kommentin kyselyn, joka on haetut sys.dm_pdw_exec_requests näkymässä [otsikko][] .

```sql
-- Query with Label
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query')
;
```

### <a name="step-2-investigate-the-query-plan"></a>Vaihe 2: Tutkia kyselyn suunnittelu

Pyydä tunnus avulla voit noutaa kyselyn hajautettu SQL (DSQL) suunnitelman [sys.dm_pdw_request_steps][].

```sql
-- Find the distributed query plan steps for a specific query.
-- Replace request_id with value from Step 1.

SELECT * FROM sys.dm_pdw_request_steps
WHERE request_id = 'QID####'
ORDER BY step_index;
```

Kun DSQL suunnitelma kestää odotettua kauemmin, syy voi olla monimutkaisia suunnitelman DSQL monessa vaiheessa tai vain yksi vaihe kestää kauan.  Jos palvelupaketti on monta vaiheet ja valitse Siirrä varallisuuden, harkitse optimointi taulukko-jaot vähentää tietojen siirto. [Taulun jakelu][] artikkelissa kerrotaan, miksi tietoja on siirrettävä kyselyn ratkaisemiseen ja kerrotaan joitakin jakauman strategioita Pienennä tietojen siirto.

Voit tutkia tarkemmin askel pitkään suoritettavien kyselyvaihe *operation_type* -sarakkeen tiedot ja Huomaa **Vaihe indeksi**seuraavasti:

- Jatka vaiheeseen 3 a **SQL**toimille: OnOperation, RemoteOperation, ReturnOperation.
- Jatka vaiheeseen 3 **Tietojen siirto**toimille: ShuffleMoveOperation, BroadcastMoveOperation, TrimMoveOperation, PartitionMoveOperation, MoveOperation, CopyOperation.

### <a name="step-3a-investigate-sql-on-the-distributed-databases"></a>Vaihe 3: a: tutkia SQL distributed tietokantoja

Tietojen noutaminen [sys.dm_pdw_sql_requests][], kaikkien hajautettu tietokantojen kyselyn vaiheen suorittamisen tietoja sisältävän pyytää-tunnus ja vaihe hakemiston avulla.

```sql
-- Find the distribution run times for a SQL step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_sql_requests
WHERE request_id = 'QID####' AND step_index = 2;
```

Kyselyvaihe suoritettaessa [DBCC PDW_SHOWEXECUTIONPLAN][] voidaan hakemiseen arvioitu SQL Server-suunnitelma vaihe käytössä tietyn jakauman SQL Server-suunnitelma välimuistista.

```sql
-- Find the SQL Server execution plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(1, 78);
```

### <a name="step-3b-investigate-data-movement-on-the-distributed-databases"></a>Vaihe 3: tietojen siirto hajautettu tietokantoja tutkia

Pyytää-tunnus ja vaihe indeksin avulla voit noutaa tietoja [sys.dm_pdw_dms_workers][]kunkin jakamisesta suorittaminen tietojen siirto-vaiheessa.

```sql
-- Find the information about all the workers completing a Data Movement Step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_dms_workers
WHERE request_id = 'QID####' AND step_index = 2;
```

- Tarkista *total_elapsed_time* -sarakkeesta, jos tietty jakauman kestää huomattavasti kauemmin kuin muiden tietojen siirtoa varten.
- Pitkään suoritettavien hajautuksessa Tarkista *rows_processed* -sarakkeesta, jos kyseisen jakauman siirretään rivien määrä on merkittävästi suurempi kuin muiden. Jos näin on, tämä saattaa johtua jakauman.vinous pohjana olevia tietoja.

Jos kysely on käynnissä, [DBCC PDW_SHOWEXECUTIONPLAN][] voidaan hakemiseen SQL Serverin arvioitu suunnitelman SQL Server-suunnitelma välimuistista käynnissä SQL vaiheen tietyn jakauman.

```sql
-- Find the SQL Server estimated plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(55, 238);
```

<a name="waiting"></a>
## <a name="monitor-waiting-queries"></a>Näytön odottaa kyselyt

Jos huomaat, että kysely ei tee edistymistä, koska se odottaa resurssin, tässä on kysely, joka näyttää kaikkien resurssien kyselyn odottaa.

```sql
-- Find queries 
-- Replace request_id with value from Step 1.

SELECT waits.session_id,
      waits.request_id,  
      requests.command,
      requests.status,
      requests.start_time,  
      waits.type,
      waits.state,
      waits.object_type,
      waits.object_name
FROM   sys.dm_pdw_waits waits
   JOIN  sys.dm_pdw_exec_requests requests
   ON waits.request_id=requests.request_id
WHERE waits.request_id = 'QID####'
ORDER BY waits.object_name, waits.object_type, waits.state;
```

Jos kysely on aktiivisesti odotustila resurssien toisen kyselyn tila on **AcquireResources**.  Jos kyselyssä on kaikki tarvittavat resurssit, siinä on **Granted**.

## <a name="next-steps"></a>Seuraavat vaiheet
Saat lisätietoja DMVs [järjestelmänäkymiä][] .
[SQL-tietovarasto parhaita käytäntöjä][] Saat lisätietoja parhaat käytännöt

<!--Image references-->

<!--Article references-->
[Manage overview]: ./sql-data-warehouse-overview-manage.md
[SQL-tietovarasto parhaat käytännöt]: ./sql-data-warehouse-best-practices.md
[Järjestelmänäkymiä]: ./sql-data-warehouse-reference-tsql-system-views.md
[Taulun jakelu]: ./sql-data-warehouse-tables-distribute.md
[Samanaikainen ja kuormituksen hallinta]: ./sql-data-warehouse-develop-concurrency.md
[Kyselyjen Odotetaan resursseja tutkiminen]: ./sql-data-warehouse-manage-monitor.md#waiting

<!--MSDN references-->
[sys.dm_pdw_dms_workers]: http://msdn.microsoft.com/library/mt203878.aspx
[sys.dm_pdw_exec_requests]: http://msdn.microsoft.com/library/mt203887.aspx
[sys.dm_pdw_exec_sessions]: http://msdn.microsoft.com/library/mt203883.aspx
[sys.dm_pdw_request_steps]: http://msdn.microsoft.com/library/mt203913.aspx
[sys.dm_pdw_sql_requests]: http://msdn.microsoft.com/library/mt203889.aspx
[DBCC PDW_SHOWEXECUTIONPLAN]: http://msdn.microsoft.com/library/mt204017.aspx
[DBCC PDW_SHOWSPACEUSED]: http://msdn.microsoft.com/library/mt204028.aspx
[OTSIKKO]: https://msdn.microsoft.com/library/ms190322.aspx
