<properties
   pageTitle="SQL-tietovarasto kapasiteetin rajat | Microsoft Azure"
   description="Yhteydet, tietokannoista, taulukoita ja kyselyjä ja SQL-tietovarasto suurimmat arvot."
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
   ms.date="09/01/2016"
   ms.author="sonyama;barbkess;jrj"/>

# <a name="sql-data-warehouse-capacity-limits"></a>SQL-tietovarasto kapasiteetin rajoitukset

Seuraavissa taulukoissa sisältävät eri osien Azure SQL-tietovarasto sallittu suurimmat arvot.


## <a name="workload-management"></a>Kuormituksen hallinta

| Luokka            | Kuvaus                                       | Enimmäismäärä            |
| :------------------ | :------------------------------------------------ | :----------------- |
| [Varaston yksiköiden (DWU)][]| Maks DWU yhden SQL-tietovarasto varten | 6000               |
| [Varaston yksiköiden (DWU)][]| Maks DWU yhden SQL Serveriä varten         | oletusarvoisesti 6000<br/><br/> Kunkin SQL server (kuten myserver.database.windows.net) on oletusarvoisesti DTU kiintiö 45,000, joka sallii 6000 DWU ylöspäin. Tämä kiintiö on yksinkertaisesti turvallisuutta rajoitukset. Voit suurentaa kiintiön [luomalla tuki lippu][] ja valitsemalla *kiintiön* pyynnön tyyppiä.  Oman DTU laskemiseen tarvitsee, kerro 7.5 summan DWU tarpeen mukaan. Voit tarkastella oman nykyisen DTU kulutus SQL server-sivu-portaalissa. Keskeytetty- ja poistamalla keskeytetty tietokantojen laskea DTU kiintiön kohti. |
| Tietokantayhteys | Samanaikainen Avaa istunnot                          | vähintään 1 024<br/><br/>Sovellus tukee enimmäismäärä on 1 024 yhteyksiä, kukin ottavat Lähetä pyynnöt SQL-tietovarasto tietokannan samaan aikaan. Huomaa, että on kyselyitä, jotka todellisuudessa voit suorittaa samanaikaisesti enimmäismäärään. Kun samanaikainen enimmäismäärä on ylitetty-pyynnön siirtyy sisäisen jonon kohtaa, johon se odottaa käsittelyä.|
| Tietokantayhteys | Suurin sallittu muistintarve valmiiden lauseiden varten            | 20 MT              |
| [Kuormituksen hallinta][] | Samanaikainen kyselyiden enimmäismäärä                    | 32<br/><br/> Oletusarvon mukaan SQL-tietovarasto voit suorittaa enintään 32 samanaikainen kyselyt ja kyselyjen jäljellä olevien.<br/><br/>Samanaikainen tason saattaa heikentyä, kun käyttäjät on määritetty suurempi resurssiluokkaa tai SQL-tietovarasto on määritetty pienen DWU. Kyselyt, kuten DMV kyselyjen suorittaminen aina sallitaan.|
| [TempDB][]          | Enimmäiskoko Tempdb.                                | 399 Gigatavua kohden DW100. Tämän vuoksi osoitteessa DWU1000 Tempdb koko muuttuu 3,99 TT |


## <a name="database-objects"></a>Tietokantaobjektien

| Luokka          | Kuvaus                                  | Enimmäismäärä            |
| :---------------- | :------------------------------------------- | :----------------- |
| Tietokannan          | Enimmäiskoko                                     | Pakattu levyllä 240 TT<br/><br/>Tähän on erillinen tempdb tai loki tilaa ja näin ollen tähän on varattu pysyvä taulukoihin.  Liitetty columnstore pakkaus on arvioitu 5 X.  Tämä pakkaamisen avulla voit kasvattaa noin 1 tietokannan pt, kun kaikki taulukot on liitetty columnstore (oletusarvo taulukon-tyyppi).|
| Taulukko             | Enimmäiskoko                                     | Pakattu levyllä 60 TT   |
| Taulukko             | Taulukot tietokantaa kohden                          | 2 miljardia          |
| Taulukko             | Sarakkeiden taulukkoa kohden                            | vähintään 1 024 sarakkeet       |
| Taulukko             | Tavua saraketta kohti                             | Sarakkeen [tietotyypin][]määräytyy.  Rajoitus on merkki-tietotyyppien 8000 4000 nvarchar tai 2 gt MAX-tietotyyppien.|
| Taulukko             | Kullakin rivillä, määritetty koko tavua                  | 8060 tavua<br/><br/>Riviä kohden tavujen määrän lasketaan samalla tavalla, sillä se on SQL Serverin kanssa sivun pakkaaminen. SQL Server, kuten SQL-tietovarasto tukee rivin ylivuoto tallentamista, joka mahdollistaa **pituus sarakkeiden** miten rivin käytöstä. Pituus-rivit ovat valittuna rivi, vain 24-tavuinen pääkansion tallennetaan tärkeimmät tietueessa. Lisätietoja on artikkelissa [rivin ylivuoto tietojen yli 8 kt][] MSDN.|
| Taulukko             | Osioiden taulukkoa kohden                    | 15 000<br/><br/>Erinomainen suorituskyky Suosittelemme numeron pienentämisen osioiden on aikana tukevat edelleen oman yrityksesi tarpeita. Kun osioiden määrä kasvaa, katseltavan Data Definition Language (DDL) ja tietojen käsittelemisessä Language (Enimmäiskuolleisuusrajaa) toimille kasvaa ja aiheuttaa hidas suorituskyky.|
| Taulukko             | Merkkien osion reunaa.| 4000 |
| Indeksi             | Muut liitetty indeksit taulukkoa kohden.        | 999<br/><br/>Koskee vain rowstore taulukot.|
| Indeksi             | Liitetty indeksit taulukkoa kohden.            | 1<br><br/>Koskee rowstore ja columnstore taulukoita.|
| Indeksi             | Hakemiston koosta.                          | 900 tavua.<br/><br/>Koskee vain rowstore indeksit.<br/><br/>Indeksit varchar sarakkeet, joiden koko on enintään yli 900 tavua voi luoda, jos olemassa olevat sarakkeiden tiedot enintään 900 tavua, kun indeksi on luotu. Kuitenkin myöhemmin lisätä tai sarakkeet, jotka aiheuttavat yli 900 tavua kokonaiskoko toiminnot päivitys epäonnistuu.|
| Indeksi             | Avainsarake indeksiä kohti.                   | 16<br/><br/>Koskee vain rowstore indeksit. Liitetty columnstore indeksit kaikki sarakkeet.|
| Tilastotiedot        | Yhdistetyn sarakkeen arvojen koko.      | 900 tavua.         |
| Tilastotiedot        | Tilastotiedot objektille sarakkeet.           | 32                 |
| Tilastotiedot        | Tilastotiedot luotujen sarakkeiden taulukkoa kohden. | 30 000            |
| Tallennettujen toimintosarjojen | Suurin sisäkkäisyystasoa.               | 8                 |
| Näytä              | Kussakin näkymässä sarakkeet                         | 1 024             |


## <a name="loads"></a>Lataa

| Luokka          | Kuvaus                                  | Enimmäismäärä            |
| :---------------- | :------------------------------------------- | :----------------- |
| Lataa Polybase    | Tavua riviä kohden                                | 32,768<br/><br/>Polybase Lataa kokorajoituksena ladataan rivien sekä pienempi kuin 32K ja ei voi ladata VARCHR(MAX), NVARCHAR(MAX) ja VARBINARY(MAX).  Kun tämä rajoitus jo tänään, se poistetaan varsin pian.<br/><br/>


## <a name="queries"></a>Kyselyt

| Luokka          | Kuvaus                                  | Enimmäismäärä            |
| :---------------- | :------------------------------------------- | :----------------- |
| Kyselyn             | Käyttäjän taulukoiden jonossa kyselyjä.               | 1 000               |
| Kyselyn             | Järjestelmänäkymien samanaikaisen kyselyjä.          | 100                |
| Kyselyn             | Jonossa olevien kyselyjen järjestelmän näkymissä               | 1 000               |
| Kyselyn             | Suurin parametrit                           | 2098               |
| Erän             | Enimmäiskoko                                 | 65, 536 * 4096        |
| Valitse tulokset    | Sarakkeita riviä kohden                              | 4096<br/><br/>Valitse tulos voi olla koskaan yli 4096 sarakkeita riviä kohden. Ei ei takaa, että sinulla on aina 4096. Jos kyselysuunnitelma edellyttää väliaikaisen taulukossa, vähintään 1 024 sarakkeiden enimmäismäärä taulukkoa kohden voi käyttää.|
| VALITSE            | Sisäkkäiset alikyselyt                            | 32<br/><br/>Käytössä voi olla enintään 32 sisäkkäiset alikyselyt koskaan SELECT-lauseessa. Ei ei takaa, että sinulla on aina 32. Esimerkiksi LIITOKSEN voivat esitellä alikyselyn kyselyjä kyselyn suunnittelu. Alikyselyjen määrän myös voidaan rajoittaa käytettävissä oleva muisti.|
| VALITSE            | Sarakkeiden liity kohden                             | vähintään 1 024 sarakkeet<br/><br/>Käytössä voi olla enintään 1 024 sarakkeet koskaan liitoksen. Ei ei takaa, että sinulla on aina vähintään 1 024. Liity suunnitelman vaatiiko tilapäinen taulukko, jossa on enemmän sarakkeita kuin liity tulos 1 024 rajoitus koskee väliaikaisen taulukossa. |
| VALITSE            | Tavua / GROUP BY-sarakkeisiin.                  | 8060<br/><br/>Sarakkeiden GROUP BY-lause voi olla enintään 8060 tavua.|
| VALITSE            | Lajittelu-saraketta kohti tavua                   | 8060 tavua.<br/><br/>Sarakkeiden ORDER BY-lause voi olla enintään 8060 tavua.|
| Tunnisteet ja -vakiot lauseen kohden | Viitattu tunnisteet ja -vakiot määrä. | 65 535<br/><br/>SQL-tietovarasto rajoittaa tunnisteet ja -vakiot, joka voi sisältyä kyselyn yhdessä lausekkeessa. Tämä rajoitus on 65 535. Suurempi kuin tämän luvun tuloksena SQL Server-virhe 8632. Lisätietoja on artikkelissa [Sisäinen virhe: lauseke-palveluiden enimmäismäärä on saavutettu][].|


## <a name="metadata"></a>Metatietojen

| Järjestelmänäkymä                        | Rivien enimmäismäärä |
| :--------------------------------- | :------------|
| sys.dm_pdw_component_health_alerts | 10 000       |
| sys.dm_pdw_dms_cores               | 100          |
| sys.dm_pdw_dms_workers             | Viimeisin 1000 DMS työntekijöiden määrä SQL pyytää. |
| sys.dm_pdw_errors                  | 10 000       |
| sys.dm_pdw_exec_requests           | 10 000       |
| sys.dm_pdw_exec_sessions           | 10 000       |
| sys.dm_pdw_request_steps           | Ohjeet viimeisimmän, jotka on tallennettu sys.dm_pdw_exec_requests 1000 SQL-pyyntöjen määrä. |
| sys.dm_pdw_os_event_logs           | 10 000       |
| sys.dm_pdw_sql_requests            | Viimeisin, jotka on tallennettu sys.dm_pdw_exec_requests 1000 SQL pyynnöt. |


## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja viittaus on artikkelissa [SQL-tietovarasto viittaus yleiskatsaus][].

<!--Image references-->

<!--Article references-->
[Varaston yksiköiden (DWU)]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[SQL-tietovarasto viittaus yleiskatsaus]: ./sql-data-warehouse-overview-reference.md
[Kuormituksen hallinta]: ./sql-data-warehouse-develop-concurrency.md
[TempDB]: ./sql-data-warehouse-tables-temporary.md
[tietotyyppi]: ./sql-data-warehouse-tables-data-types.md
[tuen lippu luominen]: /sql-data-warehouse-get-started-create-support-ticket.md

<!--MSDN references-->
[Rivin ylivuoto tietojen yli 8 kt]: https://msdn.microsoft.com/library/ms186981.aspx
[Sisäinen virhe: lauseke-palveluiden enimmäismäärä on saavutettu]: https://support.microsoft.com/kb/913050
