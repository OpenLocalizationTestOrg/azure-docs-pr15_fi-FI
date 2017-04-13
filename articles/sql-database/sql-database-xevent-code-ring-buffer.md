<properties 
    pageTitle="SQL-tietokantaan XEvent soi puskurin koodi | Microsoft Azure" 
    description="Sisältää Transact-SQL-koodi otoksen, joka tehdään helppoa ja pika käyttämällä soi puskurin-kohteen Azure SQL-tietokantaan." 
    services="sql-database" 
    documentationCenter="" 
    authors="MightyPen" 
    manager="jhubbard" 
    editor="" 
    tags=""/>


<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="genemi"/>


# <a name="ring-buffer-target-code-for-extended-events-in-sql-database"></a>Soi puskurin kohde koodi laajennettu tapahtumien SQL-tietokantaan

[AZURE.INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Haluat valmis koodin otoksen nopeasti helpoiten sieppaus ja raportin tietoja laajennetusta tapahtuman testin aikana. Laajennettu tapahtumatietoja helpoin kohde on [soi puskurin kohde](http://msdn.microsoft.com/library/ff878182.aspx).


Tässä ohjeartikkelissa esitellään otoksen Transact-SQL-koodi, joka:


1. Luo taulukon tiedoilla osoittamaan kanssa.

2. Luo aiemmin laajennettu tapahtuman, eli **sqlserver.sql_statement_starting**istunnon.
    - Tapahtuma on rajoitettu SQL-lauseita, jotka sisältävät tietyn päivityksen merkkijonon: **lauseen, kuten % päivitys tabEmployee-%**.
    - Valitsee tapahtuman tulosteen lähettäminen tyypin soi puskurin, eli **package0.ring_buffer**kohde.

3. Käynnistää tapahtuma-istunnon.

4. Ongelmat pari yksinkertainen SQL-päivityksen lauseita.

5. Ongelmat, SQL: n SELECT tapahtuman tulosteen noutamiseen soi puskuri.
    - **sys.dm_xe_database_session_targets** ja dynaaminen hallinta muissa näkymissä (DMVs) liitettyinä.

6. Lopettaa istunnon tapahtuma.

7. Laskee soi puskurin kohteen, voit vapauttaa sen resursseja.

8. Laskee tapahtuma-istunnon ja esittely-taulukkoa.


## <a name="prerequisites"></a>Edellytykset


- Azure-tili ja tilauksen. Voit rekisteröityä [maksuttoman kokeiluversion käyttäjäksi](https://azure.microsoft.com/pricing/free-trial/).


- Mikä tahansa tietokanta, voit luoda taulukon.
 - Halutessasi voit [luoda **AdventureWorksLT** esittelyn tietokannan](sql-database-get-started.md) minuutteina.


- SQL Server Management Studiossa (ssms.exe) Ihannetapauksessa sen kuukausittainen päivitys uusimmat. Voit ladata uusimman ssms.exe kohteesta:
 - [Lataa SQL Server Management Studiossa](http://msdn.microsoft.com/library/mt238290.aspx)aiheessa.
 - [Esityksen linkin Valitse Lataa.](http://go.microsoft.com/fwlink/?linkid=616025)


## <a name="code-sample"></a>Koodiesimerkki


Hyvin pieni muutoksen kanssa soi puskurin koodi-malli voidaan suorittaa Azure SQL-tietokanta tai Microsoft SQL Server. Ero on solmu '_tietokanta' joissakin dynaaminen hallinta näkymissä (DMVs), kirjoita nimi FROM-lauseen, valitse vaiheessa 5 käytetään esiintymistä. Esimerkki:

- sys.dm_xe**_tietokanta**_session_targets
- sys.dm_xe_session_targets


&nbsp;


```
GO
----  Transact-SQL.
---- Step set 1.

SET NOCOUNT ON;
GO


IF EXISTS
    (SELECT * FROM sys.objects
        WHERE type = 'U' and name = 'tabEmployee')
BEGIN
    DROP TABLE tabEmployee;
END
GO


CREATE TABLE tabEmployee
(
    EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
    EmployeeId           int                not null  identity(1,1),
    EmployeeKudosCount   int                not null  default 0,
    EmployeeDescr        nvarchar(256)          null
);
GO


INSERT INTO tabEmployee ( EmployeeDescr )
    VALUES ( 'Jane Doe' );
GO

---- Step set 2.


IF EXISTS
    (SELECT * from sys.database_event_sessions
        WHERE name = 'eventsession_gm_azuresqldb51')
BEGIN
    DROP EVENT SESSION eventsession_gm_azuresqldb51
        ON DATABASE;
END
GO


CREATE
    EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD EVENT
        sqlserver.sql_statement_starting
            (
            ACTION (sqlserver.sql_text)
            WHERE statement LIKE '%UPDATE tabEmployee%'
            )
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
GO

---- Step set 3.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    STATE = START;
GO

---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
GO

---- Step set 5.


SELECT
    se.name                      AS [session-name],
    ev.event_name,
    ac.action_name,
    st.target_name,
    se.session_source,
    st.target_data,
    CAST(st.target_data AS XML)  AS [target_data_XML]
FROM
               sys.dm_xe_database_session_event_actions  AS ac

    INNER JOIN sys.dm_xe_database_session_events         AS ev  ON ev.event_name = ac.event_name
        AND CAST(ev.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_session_object_columns AS oc
         ON CAST(oc.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_session_targets        AS st
         ON CAST(st.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_sessions               AS se
         ON CAST(ac.event_session_address AS BINARY(8)) = CAST(se.address AS BINARY(8))
WHERE
        oc.column_name = 'occurrence_number'
    AND
        se.name        = 'eventsession_gm_azuresqldb51'
    AND
        ac.action_name = 'sql_text'
ORDER BY
    se.name,
    ev.event_name,
    ac.action_name,
    st.target_name,
    se.session_source
;
GO

---- Step set 6.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    STATE = STOP;
GO

---- Step set 7.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO

---- Step set 8.


DROP EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE;
GO

DROP TABLE tabEmployee;
GO
```


&nbsp;


## <a name="ring-buffer-contents"></a>Soi puskurin sisältö


Ssms.exe voidaan suorittaa koodin-malli.


Voit tarkastella tuloksia, on napsautettu solun sarakkeen otsikon **target_data_XML**-kohdassa.

Valitse tulosruudussa on napsautettu solun sarakkeen otsikon **target_data_XML**-kohdassa. Valitsemalla luotu toisessa tiedosto-välilehdessä ssms.exe joka tuloksen solun sisältö on näkyvissä, XML-muodossa.


Tulos näkyy seuraava lohko. Näyttää pitkä, mutta se on vain kaksi **<event>** osat.


&nbsp;


```
<RingBufferTarget truncated="0" processingTime="0" totalEventsProcessed="2" eventCount="2" droppedCount="0" memoryUsed="1728">
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.317Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>7</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>184</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>328</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.327Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>10</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>340</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>486</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
</RingBufferTarget>
```


#### <a name="release-resources-held-by-your-ring-buffer"></a>Vapauta pidetään soi puskuri resurssit


Kun olet käsitellyt soi puskurin, voit poistaa sen ja vapauta varmenteiden **Muuta** seuraavanlaisia resursseja:


```
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO
```


Tapahtuma-istunnon määritelmä on päivitetty, mutta ei pudotettu. Voit lisätä esiintymässä soi puskuri myöhemmin tapahtuma-istunnon:


```
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
```


## <a name="more-information"></a>Lisätietoja


Laajennettu tapahtumien Azure SQL-tietokantaan ensisijainen aihe on:


- [Laajennettu tapahtuman huomioon otettavia seikkoja SQL-tietokantaan](sql-database-xevent-db-diff-from-svr.md), joka kontrastin tietyiltä laajennettu tapahtumien työpöytäkäytössä eri ja Microsoft SQL Server Azure SQL-tietokanta.


Muut koodin otoksen aiheet laajennettu tapahtumien on osoitteessa on seuraavissa linkeissä. Sinun on säännöllisesti tarkistettava näet, onko malli on tarkoitettu Microsoft SQL Server Azure SQL-tietokanta ja minkä tahansa otoksen. Sitten voit päättää, onko otosten suorittamiseen tarvittavaa pieniä muutoksia.


- Koodin otoksen Azure SQL-tietokanta: [tapahtumatiedoston kohde koodi laajennettu tapahtumien SQL-tietokantaan](sql-database-xevent-code-event-file.md)


<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find the Objects That Have the Most Locks Taken on Them](http://msdn.microsoft.com/library/bb630355.aspx)
-->
