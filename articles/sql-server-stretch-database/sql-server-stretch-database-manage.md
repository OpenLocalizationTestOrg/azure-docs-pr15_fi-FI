<properties
    pageTitle="Hallinta ja vianmääritys Venytys tietokannan | Microsoft Azure"
    description="Lue, miten voit hallita ja Venytys tietokannan vianmääritys."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="douglasl"/>

# <a name="manage-and-troubleshoot-stretch-database"></a>Hallinta ja Venytys tietokannan vianmääritys

Hallinta ja vianmääritys Venytys tietokannan, työkalujen ja tämän artikkelin kuvatuilla tavoilla.

## <a name="manage-local-data"></a>Paikallisten tietojen hallinta

### <a name="LocalInfo"></a>Paikalliset tietokannat ja käytössä Venytys tietokannan taulukoiden hankkiminen
Avaa luettelon näkymien **sys.databases** ja voit tarkastella tietoja Venytys **sys.tables** \-otettu käyttöön SQL Server-tietokannat ja taulukot. Saat lisätietoja, [sys.databases (Transact-SQL)](https://msdn.microsoft.com/library/ms178534.aspx) ja [sys.tables (Transact-SQL)](https://msdn.microsoft.com/library/ms187406.aspx).

Näet kuinka paljon tilaa Venytys\-käytössä taulukko on käytössä SQL Serveristä, suorita seuraava lause.

 ```tsql
USE <Stretch-enabled database name>;
GO
EXEC sp_spaceused '<Stretch-enabled table name>', 'true', 'LOCAL_ONLY';
GO
 ```
## <a name="manage-data-migration"></a>Hallitse tietojen siirtäminen

### <a name="check-the-filter-function-applied-to-a-table"></a>Tarkista taulukon käytetty filter-funktio
Avaa luettelo-näkymä **sys.remote\_tietojen\_arkisto\_taulukoiden** ja tarkistaa **suodattimen\_lause** sarakkeen tunnistavan funktiota, joka Venytys tietokannan avulla voit siirtää rivien valitseminen. Jos arvo on null, koko taulukko on vaihdettavissa uuteen. Saat lisätietoja, [sys.remote_data_archive_tables (Transact-SQL)](https://msdn.microsoft.com/library/dn935003.aspx) ja [Valitse rivit, voit siirtää suodatin-funktiolla](sql-server-stretch-database-predicate-function.md).

### <a name="Migration"></a>Tietojen siirtäminen tilan tarkistaminen
Valitse **tehtävät | Venytä | Näytön** tietokannalle SQL Server Management Studiossa tietojen siirtäminen Venytys tietokannan valvonnasta seurannassa. Saat lisätietoja, [näyttö ja vianmääritys tietojen siirtäminen (Venytys-tietokanta)](sql-server-stretch-database-monitor.md).

Avaa dynaaminen hallinta-näkymässä **sys.dm\_db\_rda\_siirron\_tila** nähdäksesi, kuinka monta erissä ja tietorivejä on siirretty.

### <a name="Firewall"></a>Tietojen siirtäminen vianmääritys
Vianmäärityksestä ehdotuksia [näyttö ja vianmääritys tietojen siirtäminen (Venytys-tietokanta)](sql-server-stretch-database-monitor.md).

## <a name="manage-remote-data"></a>Tietojen hallinta

### <a name="RemoteInfo"></a>Hae tiedot remote tietokannat ja Venytys tietokannan taulukot
Luettelon näkymien avaaminen **sys.remote\_tietojen\_arkisto\_tietokantojen** ja **sys.remote\_tietojen\_arkisto\_taulukoiden** voit tarkastella tietoja remote tietokannat ja taulukot, johon siirretyt tiedot on tallennettu. Saat lisätietoja, [sys.remote_data_archive_databases (Transact-SQL)](https://msdn.microsoft.com/library/dn934995.aspx) ja [sys.remote_data_archive_tables (Transact-SQL)](https://msdn.microsoft.com/library/dn935003.aspx).

Näet, kuinka paljon tilaa Venytys käyttävä taulukon käyttää Azure-tietokannassa, suorita seuraava lause.

 ```tsql
USE <Stretch-enabled database name>;
GO
EXEC sp_spaceused '<Stretch-enabled table name>', 'true', 'REMOTE_ONLY';
GO
 ```

### <a name="delete-migrated-data"></a>Siirrettyjen tietojen poistaminen  
Jos haluat poistaa tiedot, jotka on jo siirretty Azure [sys.sp_rda_reconcile_batch](https://msdn.microsoft.com/library/mt707768.aspx)kuvattu noudattamalla.

## <a name="manage-table-schema"></a>Taulukkorakenteen hallinta

### <a name="dont-change-the-schema-of-the-remote-table"></a>Älä muuta remote taulukko rakenne
Älä muuta remote Azure taulukkoon, joka liittyy määritetty Venytys tietokannalle SQL Server-taulukko rakenne. Erityisesti Älä muokkaa nimi tai sarakkeen tietotyypin. Venytä tietokanta-toiminto on suunniteltu eri oletuksia suhteessa SQL Server-taulukko rakenne remote taulukko rakenne. Jos muutat remote rakennetta, Venytys tietokannan lakkaa toimimasta muutetun taulukon.

### <a name="reconcile-table-columns"></a>Täsmäytä taulukon sarakkeet  
Jos olet poistanut sarakkeiden vahingossa remote-taulukosta, suorita **sp_rda_reconcile_columns** voit lisätä palstoja remote taulukon siellä olevia Venytys\-otettu käyttöön SQL Server-taulukko, mutta ei remote taulukossa. Saat lisätietoja, [sys.sp_rda_reconcile_columns](https://msdn.microsoft.com/library/mt707765.aspx).  

  > [!IMPORTANT] Kun **sp_rda_reconcile_columns** Luo sarakkeet, jotka vahingossa remote-taulukosta, se ei palauta tietoja, jotka oli aiemmin poistetut sarakkeet.

**sp_rda_reconcile_columns** ei poista sarakkeet remote taulukosta siellä olevia remote-taulukossa, mutta ei Venytys\-otettu käyttöön SQL Server-taulukko. Jos remote Azure taulukon sarakkeet, joita ei enää ole Venytys\-otettu käyttöön SQL Server taulukon nämä ylimääräisiä sarakkeita ei estä Venytys tietokannan toimivat yleensä. Voit poistaa ylimääräiset sarakkeet myös manuaalisesti.  

## <a name="manage-performance-and-costs"></a>Suorituskyky ja kustannusten hallinta  

### <a name="troubleshoot-query-performance"></a>Kyselyn suorituskyvyn vianmääritys
Kyselyitä, jotka sisältävät Venytys\-käytössä olevat taulukot on tarkoitus suorittaa hitaammin kuin ennen taulukot on otettu käyttöön Venytys. Jos kyselyn suorituskykyä heikentää huomattavasti, tarkista seuraavat mahdolliset ongelmat.

-   Eri maantieteellinen alue kuin SQL Server Azure palvelimellesi ominaisuudet Määritä Azure-palvelin halutaan olevan samassa maantieteellinen alue kuin SQL Serverin verkon latenssin vähentämiseksi.

-   Verkko-ehtoa on heikentynyt. Lisätietoja uusien seurantakohteiden tai katkokset verkonvalvojalta.

### <a name="increase-azure-performance-level-for-resource-intensive-operations-such-as-indexing"></a>Suurenna Azure suorituskyvyn tasoa resurssin\-tehostettu toimintoja, kuten indeksointi
Kun luot, muodosta uudelleen tai järjestää indeksin suuri taulukkoa, joka on määritetty Venytys tietokannan ja arvelet Azure siirretyt tiedot sisältävä kyselyt tänä aikana, harkitse lisääntyvien vastaavan Azure etätietokanta suorituskyvyn tasoon toiminnon toistaminen. Saat lisätietoja suorituskykyä ja hinnoittelusta [SQL Server Venytys tietokannan hinnat](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

### <a name="you-cant-pause-the-sql-server-stretch-database-service-on-azure"></a>Ei voi keskeyttää Azure SQL Server-Venytys tietokanta-palvelu  
 Varmista, että valitset haluamasi suorituskyky ja hinnoittelu taso. Jos lisäät suorituskyky-tason väliaikaisesti resurssin\-tehostettu toiminnon palauttaminen edelliselle tasolle, kun toiminto on valmis. Saat lisätietoja suorituskykyä ja hinnoittelusta [SQL Server Venytys tietokannan hinnat](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).  

## <a name="change-the-scope-of-queries"></a>Muuta kyselyjen laajuus  
 Kyselyitä, jotka perustuvat Venytys\-käytössä olevat taulukot palauttavat paikallisen-ja etä oletusarvoisesti. Voit muuttaa kyselyt laajuus kaikki kyselyjen kaikki käyttäjät tai vain yhden kyselyn järjestelmänvalvojan oikeuksia.  

### <a name="change-the-scope-of-queries-for-all-queries-by-all-users"></a>Muuttaa kaikkien käyttäjien kaikki kyselyt kyselyt vaikutusaluetta  
 Voit muuttaa kaikkien kyselyjen kaikki käyttäjät, suorita tallennetun toimintosarjan **sys.sp_rda_set_query_mode**. Voit pienentää laajuutta kyselyn paikalliset tiedot, poista käytöstä kaikki kyselyt ja palauttaa oletusasetukset. Saat lisätietoja, [sys.sp_rda_set_query_mode](https://msdn.microsoft.com/library/mt703715.aspx).  

### <a name="queryHints"></a>Muuta kyselyjen laajuus yhteen kyselyn järjestelmänvalvoja  
 Jos haluat muuttaa yksittäisen kyselyn db_owner-roolin jäsen laajuutta, lisätä * *kanssa \( REMOTE_DATA_ARCHIVE_OVERRIDE = *arvo* \)** kyselyn Vihje SELECT-lauseessa. REMOTE_DATA_ARCHIVE_OVERRIDE kyselyn Vihje voi olla seuraavat arvot.  
 -   **LOCAL_ONLY**. Kyselyn paikalliset tiedot.  

 -   **REMOTE_ONLY**. Kyselyn tietojen vain.  

 -   **STAGE_ONLY**. Kyselyn vain tiedot-taulukon kohtaa, johon Venytys tietokannan vaiheiden rivit siirtämiseen ja säilyttää siirretyt rivien annettuna kautena siirron jälkeen. Tämä kysely vihje on ainoa tapa kyselyn väliaikaisen taulukossa.  

Esimerkiksi seuraava kysely palauttaa vain paikallisen tulokset.  

 ```tsql  
 USE <Stretch-enabled database name>;
 GO
 SELECT * FROM <Stretch_enabled table name> WITH (REMOTE_DATA_ARCHIVE_OVERRIDE = LOCAL_ONLY) WHERE ... ;
 GO
```  

## <a name="adminHints"></a>Järjestelmänvalvojan päivitykset ja poistaa  
 Oletusarvo ei voi päivittää, Poista rivit, jotka soveltuvat siirron tai rivit, jotka on jo siirretty, Venytys-\-käytössä taulukko. Ollessasi korjaamiseksi db_owner-roolin jäsen suorittamisen lisäämällä Päivitä tai poista-toimintoa * *kanssa \( REMOTE_DATA_ARCHIVE_OVERRIDE = *arvo* \)** kyselyn Vihje lausekkeeseen. REMOTE_DATA_ARCHIVE_OVERRIDE kyselyn Vihje voi olla seuraavat arvot.  
 -   **LOCAL_ONLY**. Päivitä tai Poista paikalliset tiedot.  

 -   **REMOTE_ONLY**. Päivitä tai poista vain remote data.  

 -   **STAGE_ONLY**. Päivitä tai poista vain tiedot-taulukon kohtaa, johon Venytys tietokannan vaiheiden rivit siirtämiseen ja säilyttää siirretyt rivien annettuna kautena siirron jälkeen.  

## <a name="see-also"></a>Katso myös

[Näytön Venytys tietokanta](sql-server-stretch-database-monitor.md)

[Varmuuskopion Venytys käyttävä tietokannat](sql-server-stretch-database-backup.md)

[Palauttaa Venytys käyttävä tietokannat](sql-server-stretch-database-restore.md)
