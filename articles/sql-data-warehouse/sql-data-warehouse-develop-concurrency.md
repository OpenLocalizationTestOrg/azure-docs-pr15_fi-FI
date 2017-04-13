<properties
   pageTitle="Samanaikainen ja työmäärää hallinnan SQL-tietovarasto | Microsoft Azure"
   description="Ratkaisujen kehittämiseen ymmärtäminen Azure SQL-tietovarasto samanaikainen ja työmäärää hallinta."
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
   ms.date="09/27/2016"
   ms.author="sonyama;barbkess;jrj"/>

# <a name="concurrency-and-workload-management-in-sql-data-warehouse"></a>SQL-tietovarasto samanaikainen ja kuormituksen hallinta

Jotta ennakoitavissa suorituskyky tasolla, Microsoft Azure SQL-tietovarasto avulla voit hallita samanaikainen tasot ja Resurssin kohdistukset, kuten muistin ja Suorittimen priorisointi. Tässä artikkelissa esitellään samanaikainen ja kuormituksen hallinta-selvitys siitä, miten molempia ominaisuuksia on toteutettu ja kuinka voidaan hallita niiden tietojen varastossa käsitteistä. SQL-tietovarasto kuormituksen hallinta on tarkoitettu tukitehtäviä usean käyttäjän ympäristössä. Se ei ole tarkoitettu usean vuokraajan toiminnoista.

## <a name="concurrency-limits"></a>Samanaikainen rajoitukset

SQL-tietovarasto sallii enintään 1 024 samanaikaisia yhteyksiä. Kaikki 1 024 yhteydet voit lähettää kyselyjä samanaikaisesti. Optimoi siirtonopeuden SQL-tietovarasto voi kuitenkin jono varmistaa, että kukin kysely saa mahdollisimman vähän muistia myöntäminen jotkin kyselyt. Kyselyn suoritusaika Queuing tapahtuu. Queuing kyselyjen samanaikainen rajoitukset saavutetaan, kun SQL-tietovarasto voit suurentaa yhteensä siirtonopeuden varmistaa, että aktiivinen kyselyt saavat vakavasti tarvittavan muistiresurssien käytön.  

Samanaikainen rajoitukset on noudatettava kaksi: *samanaikainen kyselyt* ja *samanaikainen paikkaa*. Kyselyn suorittaminen se on suoritettava kyselyn samanaikainen rajan ja samanaikainen saapumisaikojen.

- Samanaikainen on kyselyjen suorittaminen samanaikaisesti. SQL-tietovarasto tukee suurempia DWU koot enintään 32 samanaikainen kyselyt.
- Samanaikainen paikkojen kohdistetaan DWU perusteella. Kunkin 100 DWU on 4 samanaikainen paikkaa. Esimerkiksi DW100 varaa 4 samanaikainen paikkojen ja DW1000 varaa 40. Jokainen kyselyn siinä käytetään vähintään yksi samanaikainen paikkaa, määräytyy [resurssiluokkaa](#resource-classes) kyselyn. Kyselyjen suorittaminen smallrc resurssiluokkaa tarjoaman samanaikainen yksi paikka. Kyselyjen suorittaminen suurempi resurssiluokkaa tarjoaman Lisää samanaikainen paikkaa.

Seuraavassa taulukossa kuvataan samanaikainen kyselyt ja samanaikainen paikkojen eri DWU koon rajoitukset.

### <a name="concurrency-limits"></a>Samanaikainen rajoitukset

|  DWU   | Samanaikainen Max-kyselyt  | Samanaikainen paikkojen varattu |
| :----  | :---------------------: | :-------------------------: |
| DW100  |            4            |                4            |
| DW200  |            8            |                8            |
| DW300  |           12            |               12            |
| DW400  |           16            |               16            |
| DW500  |           20            |               20            |
| DW600  |           24            |               24            |
| DW1000 |           32            |               40            |
| DW1200 |           32            |               48            |
| DW1500 |           32            |               60            |
| DW2000 |           32            |               80            |
| DW3000 |           32            |              120            |
| DW6000 |           32            |              240            |

Yksi nämä raja-arvot täyttyessä uusia kyselyjä jonossa ja suorittaa ensimmäisen sisään, tarkoitettua välein.  On valmis kyselyt ja kyselyjen ja paikkojen kuuluu alle, jonossa olevien kyselyjen julkaistaan. 

> [AZURE.NOTE]  *Hakukyselyjä suoritetaan yksinomaan-näkymien dynaaminen hallinta (DMVs) tai luettelon näkymät* eivät määräydy jonkin samanaikainen. Voit valvoa järjestelmän kyselyiden suorittaminen sitä määrä riippumatta.

## <a name="resource-classes"></a>Resurssin luokat

Resurssin luokkien käyttöoikeuksia voidaan hallita muistin varaamista ja suorittimen jaksot kyselyn annettu ohjeita. Voit määrittää *tietokannan rooleja*lomakkeen käyttäjän neljä resurssin luokat. Neljä resurssin luokat ovat **smallrc**, **mediumrc**, **largerc**ja **xlargerc**. Smallrc käyttäjät saavat muistin pienemmän määrän ja voit hyödyntää suurempi samanaikainen. Sen sijaan xlargerc määritetyt käyttäjät saavat suuria määriä muistia ja näin ollen vähemmän kyselyjä voidaan suorittaa samanaikaisesti.

Oletusarvoisesti jokaisella käyttäjällä on pieni resurssiluokkaa smallrc jäsen. Menettelyn `sp_addrolemember` käytetään niin, että resurssiluokkaa, ja `sp_droprolemember` käytetään pienentää resurssi-luokka. Esimerkiksi Tämä komento suurentaa resurssiluokkaa, loaduser largerc:

```sql
EXEC sp_addrolemember 'largerc', 'loaduser'
```

Hyvä on määritettävä käyttäjille pysyvästi resurssin luokan muuttaminen resurssin niiden luokkien sijaan. Esimerkiksi latausten liitetty columnstore taulukoiden luominen laadukkaan indeksit muistia jaettaessa. Voit varmistaa, että Lataa on suurempi muistiin, Luo käyttäjän erikseen tietojen lataaminen ja määrittää käyttäjän pysyvästi suurempi resurssiluokkaa.

Kontekstityyppejä on joitakin kyselyitä, jotka eivät saa etua suurempi muistin varaamista. Järjestelmä ohittaa niiden luokan resurssivaraukset ja Suorita nämä kyselyt aina sijaan pieni resurssiluokkaa. Jos nämä kyselyt suoritetaan aina pieni resurssiluokkaa, ne voidaan suorittaa, kun samanaikainen paikkojen ovat paineenalaisena ja ne eivät kuluttavat Lisää paikkojen kuin tarvitaan. Lisätietoja on kohdassa [resurssin luokan poikkeukset](#query-exceptions-to-concurrency-limits) .

Joitakin lisätietoja resurssiluokkaa:

- *Muuta roolin* oikeudet tarvitaan käyttäjän resurssiluokkaa muuttaminen.  
- Vaikka voit lisätä käyttäjän vähintään yhtä suurempi resurssin luokat, käyttäjät tulevat suurin resurssiluokkaa, johon ne on varattu määritteissä. Jos käyttäjä on määritetty mediumrc ja largerc, suurempi resurssiluokkaa (largerc) on resurssiluokkaa, joka voi käyttää.  
- Järjestelmän järjestelmänvalvojan käyttäjän resurssiluokkaa ei voi muuttaa.

Esimerkki-kohdassa [muuttaminen käyttäjän resurssin luokan Esimerkki](#changing-user-resource-class-example).

## <a name="memory-allocation"></a>Muistin varaamista

On etuja sekä haittoja lisääntyvien käyttäjän resurssiluokkaa. Lisääntyvien resurssiluokkaa käyttäjän antaa käyttöoikeuden kyselyt muistia, joka tarkoittaa kyselyt suoritetaan nopeammin.  Kuitenkin suurempi resurssin luokkien pienentää myös samanaikainen kyselyitä, jotka voidaan suorittaa. Tämä on trade-off kohdistaminen yhteen kyselyn muistia suuria määriä tai muita kyselyjä, jolloin on myös muistin kohdistukset samanaikaisen suorittamisen salliminen välillä. Jos yksi käyttäjä on annettu hyvin kohdistukset muistin kyselyn, muut käyttäjät ei ole pääsyä kyseisen saman muisti kyselyn.

Seuraavassa taulukossa yhdistää varattu kunkin jakauman DWU ja resurssien luokan mukaan.

### <a name="memory-allocations-per-distribution-mb"></a>Muistin kohdistukset kohti jakauma (Mt)

|  DWU   | smallrc | mediumrc | largerc | xlargerc |
| :----- | :-----: | :------: | :-----: | :------: |
| DW100  |   100   |    100   |    200  |    400   |
| DW200  |   100   |    200   |    400  |    800   |
| DW300  |   100   |    200   |    400  |    800   |
| DW400  |   100   |    400   |    800  |  1,600   |
| DW500  |   100   |    400   |    800  |  1,600   |
| DW600  |   100   |    400   |    800  |  1,600   |
| DW1000 |   100   |    800   |  1,600  |  3,200   |
| DW1200 |   100   |    800   |  1,600  |  3,200   |
| DW1500 |   100   |    800   |  1,600  |  3,200   |
| DW2000 |   100   |  1,600   |  3,200  |  6,400   |
| DW3000 |   100   |  1,600   |  3,200  |  6,400   |
| DW6000 |   100   |  3,200   |  6,400  |  12,800  |

Edellisen taulukosta näet, että käytössä DW2000 xlargerc resurssin luokan kyselyn on pääsy 6,400 Megatavua kunkin 60 hajautettu tietokannat.  SQL-tietovarasto ovat 60 jaot. Tämän vuoksi laskemiseen annetun resurssiluokkaa kyselyn muisti kohdistus yllä olevista arvoista olisi kerrotaan 60.

### <a name="memory-allocations-system-wide-gb"></a>Muistin kohdistukset järjestelmää (gt)

|  DWU   | smallrc | mediumrc | largerc | xlargerc |
| :----- | :-----: | :------: | :-----: | :------: |
| DW100  |    6    |    6     |    12   |    23    |
| DW200  |    6    |    12    |    23   |    47    |
| DW300  |    6    |    12    |    23   |    47    |
| DW400  |    6    |    23    |    47   |    94    |
| DW500  |    6    |    23    |    47   |    94    |
| DW600  |    6    |    23    |    47   |    94    |
| DW1000 |    6    |    47    |    94   |   188    |
| DW1200 |    6    |    47    |    94   |   188    |
| DW1500 |    6    |    47    |    94   |   188    |
| DW2000 |    6    |    94    |   188   |   375    |
| DW3000 |    6    |    94    |   188   |   375    |
| DW6000 |    6    |   188    |   375   |   750    |

Tämän taulukon järjestelmää muistin kohdistukset, näet kyselyn DW2000 xlargerc resurssin luokan käytössä on varattu 375 Gigatavua muistia yhteensä (6,400 Mt * 60 jaot / 1 024 muuntaminen gt) SQL-tietovarasto kokonaisuudessaan päälle.

## <a name="concurrency-slot-consumption"></a>Samanaikainen paikka kulutus

SQL-tietovarasto antaa muistia kyselyihin käynnissä suurempi resurssin luokkiin. Muisti on kiinteä resurssi.  Tämän vuoksi kysely varattu muistia vähemmän samanaikaisia kyselyjä voidaan suorittaa. Seuraavassa taulukossa reiterates kaikki edellisen käsitteitä samassa näkymässä, jossa näkyy samanaikainen paikkojen DWU mukaan käytettävissä ja kunkin resurssiluokkaa käyttämä paikkojen määrän.

### <a name="allocation-and-consumption-of-concurrency-slots"></a>Kohdistus- ja samanaikainen paikkojen kulutus

|  DWU   | Samanaikainen kyselyiden enimmäismäärä  | Samanaikainen paikkojen varattu | Paikkojen smallrc käyttämä |  Paikkojen mediumrc käyttämä |  Paikkojen largerc käyttämä |  Paikkojen xlargerc käyttämä |
| :----  | :---------------------: | :-------------------------: | :-----: | :------: | :-----: | :------: |
| DW100  |            4            |                4            |    1    |     1    |    2    |    4     |
| DW200  |            8            |                8            |    1    |     2    |    4    |    8     |
| DW300  |           12            |               12            |    1    |     2    |    4    |    8     |
| DW400  |           16            |               16            |    1    |     4    |    8    |   16     |
| DW500  |           20            |               20            |    1    |     4    |    8    |   16     |
| DW600  |           24            |               24            |    1    |     4    |    8    |   16     |
| DW1000 |           32            |               40            |    1    |     8    |   16    |   32     |
| DW1200 |           32            |               48            |    1    |     8    |   16    |   32     |
| DW1500 |           32            |               60            |    1    |     8    |   16    |   32     |
| DW2000 |           32            |               80            |    1    |    16    |   32    |   64     |
| DW3000 |           32            |              120            |    1    |    16    |   32    |   64     |
| DW6000 |           32            |              240            |    1    |    32    |   64    |  128     |


Tässä taulukossa voit tarkastella, SQL-tietovarasto suorittaminen kuin DW1000 varaa enintään 32 samanaikainen kyselyt ja korkeintaan 40 samanaikainen paikkaa. Jos kaikki käyttäjät ovat käynnissä smallrc, 32 samanaikaisia kyselyjen suorittaa, koska kukin kysely tarjoaman 1 samanaikainen paikka. Jos kaikki käyttäjät DW1000 on käynnissä mediumrc, kukin kysely kohdistettu 800 Megatavua kohti jakauma 47 gigatavun (gt) muisti kohdistus kysely ja samanaikainen rajoittuvat 5 käyttäjille (40 samanaikainen paikkojen / 8 lähtö saapumisajat mediumrc käyttäjää kohden).

## <a name="query-importance"></a>Kyselyn tärkeys

SQL-tietovarasto sisältää resurssin luokkia työmäärää ryhmiä käyttämällä. On yhteensä kahdeksan työmäärää ryhmiä, jotka ohjataan resurssin luokat DWU erikokoisista yli. Mikä tahansa DWU SQL-tietovarasto käyttää vain neljä kahdeksan työmäärää ryhmät. Tämä on järkevää, koska työmäärää kunkin ryhmän on varattu johonkin neljä resurssin luokkien: smallrc, mediumrc, largerc, tai xlargerc. Merkitys tietoja työmäärää-ryhmistä on, että osa työmäärästä ryhmiin määritetään suurempi *Tärkeys*. Merkitys käytetään suorittimen ajoitus. Kyselyitä, jotka toimivat hyvin tärkeäksi saavat kolme kertaa enemmän kuin normaali tärkeys sisältävän Suoritin jaksot. Tämän vuoksi samanaikainen paikka yhdistämismääritykset määrittävät myös suorittimen prioriteetti. Kyselyn siinä käytetään 16 tai useita paikkaa, kun se suoritetaan hyvin tärkeäksi.

Seuraavassa taulukossa on tärkeä yhdistämismääritykset työmäärää kunkin ryhmän.

### <a name="workload-group-mappings-to-concurrency-slots-and-importance"></a>Kuormituksen ryhmän yhdistämismääritykset samanaikainen paikkojen ja tärkeys

| Kuormituksen ryhmät | Samanaikainen paikka yhdistäminen | Mt ja jakelu | Merkitys yhdistäminen |
| :-------------- | :----------------------: | :---------------: | :----------------- |
| SloDWGroupC00   |            1             |         100       |       Normaali       |
| SloDWGroupC01   |            2             |         200       |       Normaali       |
| SloDWGroupC02   |            4             |         400       |       Normaali       |
| SloDWGroupC03   |            8             |         800       |       Normaali       |
| SloDWGroupC04   |           16             |       1,600       |       Suuri         |
| SloDWGroupC05   |           32             |       3,200       |       Suuri         |
| SloDWGroupC06   |           64             |       6,400       |       Suuri         |
| SloDWGroupC07   |          128             |      12,800       |       Suuri         |

**Kohdistus- ja samanaikainen paikkojen kulutus** kaaviosta näet, että DW500 käyttää 1, 4, 8 tai 16 samanaikainen paikkojen smallrc, mediumrc, largerc ja xlargerc, vastaavassa järjestyksessä. Voit etsiä kyseiset arvot edellisen esimerkin kaavion etsiminen resurssin jokaisesta tärkeys.

### <a name="dw500-mapping-of-resource-classes-to-importance"></a>DW500 yhdistäminen resurssin luokkien tärkeys

| Resurssiluokkaa | Kuormituksen-ryhmä | Samanaikainen paikkojen käytetään | Mt ja jakelu | Merkitys |
| :------------- | :------------- | :--------------------: | :---------------: | :--------- |
| smallrc        | SloDWGroupC00  |           1            |         100       |   Normaali   |
| mediumrc       | SloDWGroupC02  |           4            |         400       |   Normaali   |
| largerc        | SloDWGroupC03  |           8            |         800       |   Normaali   |
| xlargerc       | SloDWGroupC04  |          16            |        1,600      |   Suuri     |


Voit käyttää DMV kysely voit tarkastella resurssin säädin näkökulmasta muistin resurssivaraukset yksityiskohtaisesti erot tai analysointiin aktiivinen ja historiallisten työmäärää ryhmien käyttö vianmäärityksessä.

```sql
WITH rg
AS
(   SELECT  
     pn.name                        AS node_name
    ,pn.[type]                      AS node_type
    ,pn.pdw_node_id                 AS node_id
    ,rp.name                        AS pool_name
    ,rp.max_memory_kb*1.0/1024              AS pool_max_mem_MB
    ,wg.name                        AS group_name
    ,wg.importance                  AS group_importance
    ,wg.request_max_memory_grant_percent        AS group_request_max_memory_grant_pcnt
    ,wg.max_dop                     AS group_max_dop
    ,wg.effective_max_dop               AS group_effective_max_dop
    ,wg.total_request_count             AS group_total_request_count
    ,wg.total_queued_request_count          AS group_total_queued_request_count
    ,wg.active_request_count                AS group_active_request_count
    ,wg.queued_request_count                AS group_queued_request_count
    FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
    JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    
            ON  wg.pdw_node_id  = rp.pdw_node_id
            AND wg.pool_id      = rp.pool_id
    JOIN    sys.dm_pdw_nodes pn
            ON  wg.pdw_node_id  = pn.pdw_node_id
    WHERE   wg.name like 'SloDWGroup%'
        AND     rp.name = 'SloDWPool'
)
SELECT  pool_name
,       pool_max_mem_MB
,       group_name
,       group_importance
,       (pool_max_mem_MB/100)*group_request_max_memory_grant_pcnt AS max_memory_grant_MB
,       node_name
,       node_type
,       group_total_request_count
,       group_total_queued_request_count
,       group_active_request_count
,       group_queued_request_count
FROM    rg
ORDER BY
    node_name
,   group_request_max_memory_grant_pcnt
,   group_importance
;
```

## <a name="queries-that-honor-concurrency-limits"></a>Kysely, joka tukee samanaikainen rajoitukset

Useimmat kyselyt on noudatettava resurssin luokat. Nämä kyselyt on mahtuu sekä samanaikainen kysely ja samanaikainen paikka raja-arvot. Käyttäjä voi valita jättäminen pois kyselyn samanaikainen paikka mallista.

Voit reiterate, seuraavista väittämistä tukee resurssin luokat:

- VALITSE LISÄÄ
- PÄIVITYS
- POISTA
- Valitse (kun kyselyt käyttäjän taulukot)
- ALTER INDEKSI-MUODOSTA
- ALTER INDEKSI UUDELLEEN
- MUUTA TAULUKON MUODOSTA
- INDEKSIN LUOMINEN
- LIITETTY COLUMNSTORE INDEKSIN LUOMINEN
- LUO TAULUKON AS SELECT (CTAS)
- Tietojen lataaminen
- Tietojen siirtämistä toiminnot suoritetaan tietojen siirtämistä Service (DMS) mukaan

## <a name="query-exceptions-to-concurrency-limits"></a>Kyselyn poikkeusten samanaikainen rajoitukset

Jotkin kyselyt eivät tukee resurssiluokkaa, johon käyttäjä on määritetty. Samanaikainen rajoituksia nämä poikkeukset tehdään, kun muistin tietyn komennon tarvittavat resurssit ovat pieni, usein koska komento on metatiedot-toimintoa. Poikkeukset tavoitteena on suurempi muistin kohdistukset kyselyitä, jotka on ne koskaan välttämiseksi. Tällaisissa tapauksissa oletusarvon pieni resurssiluokkaa (smallrc) käytetään aina käyttäjä saa todellinen resurssiluokkaa riippumatta. Esimerkiksi `CREATE LOGIN` suoritetaan aina smallrc. Tämän toiminnon suorittamiseen tarvittavat resurssit ovat erittäin pienen, joten se ei tulkita kyselyyn sisällytettävät samanaikainen paikka-malli.  Nämä kyselyt ovat myös ei rajoitettu 32 käyttäjän samanaikainen rajoitus, nämä kyselyt rajattomasti suorittamisen istunnon raja on 1 024 istuntoja.

Seuraavista väittämistä Ota huomioon resurssin luokat:

- Luo tai poista taulukko
- MUUTA TAULUKKO... Vaihda, Jaa tai Yhdistä-osio
- MUUTA INDEKSIN POISTAMINEN KÄYTÖSTÄ
- DROP INDEX
- Luo, Päivitä tai PUDOTA tilastotiedot
- LYHENNÄ TAULUKKO
- ALTER TODENNUS
- LUO KIRJAUTUMINEN
- Luo, muuta tai Poista käyttäjä
- Luo, muuta tai poista TOIMINTOSARJAN
- Luo tai poista NÄKYMÄ
- LISÄÄ ARVOT
- Valitse järjestelmänäkymiä ja DMVs
- KERROTAAN
- DBCC

<!--
Removed as these two are not confirmed / supported under SQLDW
- CREATE REMOTE TABLE AS SELECT
- CREATE EXTERNAL TABLE AS SELECT
- REDISTRIBUTE
-->

## <a name="change-a-user-resource-class-example"></a>Vaihda käyttäjän resurssin luokka-Esimerkki

1. **Luo kirjautuminen:** Avaa yhteyden SQL Server, jossa tietovarasto SQL-tietokanta **perusmuodon** tietokannan ja suorittaa seuraavat komennot.

    ```sql
    CREATE LOGIN newperson WITH PASSWORD = 'mypassword';
    CREATE USER newperson for LOGIN newperson;
    ```

    > [AZURE.NOTE] Se on hyvä käyttäjän luominen perusmuodon tietokannan Azure SQL-tietovarasto käyttäjille. Käyttäjän luominen perustyyli avulla käyttäjä voi kirjautua sisään työkaluilla, kuten SSMS määrittämättä tietokannan nimi.  Se myös antaa niiden objektin Resurssienhallinnan avulla voit tarkastella kaikkien tietokantojen SQL Server.  Saat lisätietoja luomisen ja ylläpidon käyttäjien [suojatun tietovarasto SQL-tietokantaan][].

2. **Luo SQL-tietovarasto käyttäjän:** Avaa yhteyden **SQL-tietovarasto** -tietokantaan ja suorita seuraava komento.

    ```sql
    CREATE USER newperson FOR LOGIN newperson;
    ```

3. **Käyttöoikeuksien myöntäminen:** Seuraavassa esimerkissä myöntää `CONTROL` **Tietovarasto SQL** -tietokantaan. `CONTROL`tietokannan taso on SQL Server db_owner kokonaan.

    ```sql
    GRANT CONTROL ON DATABASE::MySQLDW to newperson;
    ```

4. **Suurenna resurssiluokkaa:** Käyttäjän lisääminen suurempi työmäärää hallinnan rooli seuraavan kyselyn avulla.

    ```sql
    EXEC sp_addrolemember 'largerc', 'newperson'
    ```

5. **Pienentää resurssiluokkaa:** Seuraava kysely avulla voit poistaa käyttäjän työmäärää hallinnan rooli.

    ```sql
    EXEC sp_droprolemember 'largerc', 'newperson';
    ```

    > [AZURE.NOTE] Ei voida poistaa käyttäjän smallrc.

## <a name="queued-query-detection-and-other-dmvs"></a>Jonossa olevien kyselyyn tunnistaminen ja muut DMVs

Voit käyttää `sys.dm_pdw_exec_requests` DMV tunnistavan kyselyitä, jotka ovat jonossa olevat samanaikainen. Samanaikainen paikka odotetaan kyselyt on tila on **hyllytetty**.

```sql
SELECT   r.[request_id]              AS Request_ID
        ,r.[status]              AS Request_Status
        ,r.[submit_time]             AS Request_SubmitTime
        ,r.[start_time]              AS Request_StartTime
        ,DATEDIFF(ms,[submit_time],[start_time]) AS Request_InitiateDuration_ms
        ,r.resource_class                         AS Request_resource_class
FROM    sys.dm_pdw_exec_requests r;
```

Kuormituksen hallinta roolit voi tarkastella ja `sys.database_principals`.

```sql
SELECT  ro.[name]           AS [db_role_name]
FROM    sys.database_principals ro
WHERE   ro.[type_desc]      = 'DATABASE_ROLE'
AND     ro.[is_fixed_role]  = 0;
```

Seuraava kysely näyttää roolin kullekin käyttäjälle määritetään.

```sql
SELECT   r.name AS role_principal_name
        ,m.name AS member_principal_name
FROM    sys.database_role_members rm
JOIN    sys.database_principals AS r            ON rm.role_principal_id     = r.principal_id
JOIN    sys.database_principals AS m            ON rm.member_principal_id   = m.principal_id
WHERE   r.name IN ('mediumrc','largerc', 'xlargerc');
```

SQL-tietovarasto on odotettava tyypit seuraavasti:

- **LocalQueriesConcurrencyResourceType**: kyselyitä, jotka istut samanaikainen paikka framework ulkopuolella. DMV kyselyt ja järjestelmä toimii kuten `SELECT @@VERSION` on esimerkkejä paikallisen kyselyt.
- **UserConcurrencyResourceType**: kyselyitä, jotka istut samanaikainen paikka kehyksen sisälle. Kyselyitä, jotka perustuvat käyttäjän taulukot edustavat esimerkkejä, jota käyttää tämän resurssin laji.
- **DmsConcurrencyResourceType**: odottaa tietojen siirtämistä toimintojen tuloksena.
- **BackupConcurrencyResourceType**: Tämä odottaa ilmaisee, että tietokannan varmuuskopioidaan. Suurin arvo tämän resurssin laji on 1. Jos useita varmuuskopioita on pyydetty samaan aikaan, muihin sarakkeisiin jono.

`sys.dm_pdw_waits` DMV avulla voidaan nähdä pyyntö odottaa resursseja.

```sql
SELECT  w.[wait_id]
,       w.[session_id]
,       w.[type]            AS Wait_type
,       w.[object_type]
,       w.[object_name]
,       w.[request_id]
,       w.[request_time]
,       w.[acquire_time]
,       w.[state]
,       w.[priority]
,   SESSION_ID()            AS Current_session
,   s.[status]          AS Session_status
,   s.[login_name]
,   s.[query_count]
,   s.[client_id]
,   s.[sql_spid]
,   r.[command]         AS Request_command
,   r.[label]
,   r.[status]          AS Request_status
,   r.[submit_time]
,   r.[start_time]
,   r.[end_compile_time]
,   r.[end_time]
,   DATEDIFF(ms,r.[submit_time],r.[start_time])     AS Request_queue_time_ms
,   DATEDIFF(ms,r.[start_time],r.[end_compile_time])    AS Request_compile_time_ms
,   DATEDIFF(ms,r.[end_compile_time],r.[end_time])      AS Request_execution_time_ms
,   r.[total_elapsed_time]
FROM    sys.dm_pdw_waits w
JOIN    sys.dm_pdw_exec_sessions s  ON w.[session_id] = s.[session_id]
JOIN    sys.dm_pdw_exec_requests r  ON w.[request_id] = r.[request_id]
WHERE   w.[session_id] <> SESSION_ID();
```

`sys.dm_pdw_resource_waits` DMV näkyy vain tietyn kyselyn käyttämä resurssin odottaa. Resurssin odotusaika mittaa vain Odotetaan resursseja toimitettavien verrattuna signaalin odotusaika, pohjana SQL-palvelinten ajoittaminen suorittimen käyttö kyselyn kuluvaa aikaa on aika.

```sql
SELECT  [session_id]
,       [type]
,       [object_type]
,       [object_name]
,       [request_id]
,       [request_time]
,       [acquire_time]
,       DATEDIFF(ms,[request_time],[acquire_time])  AS acquire_duration_ms
,       [concurrency_slots_used]                    AS concurrency_slots_reserved
,       [resource_class]
,       [wait_id]                                   AS queue_position
FROM    sys.dm_pdw_resource_waits
WHERE   [session_id] <> SESSION_ID();
```

`sys.dm_pdw_wait_stats` DMV voi käyttää odottaa historiallisten trendi analyysi.

```sql
SELECT  w.[pdw_node_id]
,       w.[wait_name]
,       w.[max_wait_time]
,       w.[request_count]
,       w.[signal_time]
,       w.[completed_count]
,       w.[wait_time]
FROM    sys.dm_pdw_wait_stats w;
```

## <a name="next-steps"></a>Seuraavat vaiheet

Saat lisätietoja tietokannan käyttäjien ja suojauksen hallinta [suojatun tietovarasto SQL-tietokantaan][]. Lisätietoja siitä, miten suurempi resurssin luokat voit parantaa liitetty columnstore indeksi laatua on artikkelissa [Rebuilding indeksit parantaa segmentin laatua].

<!--Image references-->

<!--Article references-->
[SQL-tietovarasto-tietokannan suojaaminen]: ./sql-data-warehouse-overview-manage-security.md
[Indeksit parantaa segmentin laatua muodostetaan uudelleen]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[SQL-tietovarasto-tietokannan suojaaminen]: ./sql-data-warehouse-overview-manage-security.md

<!--MSDN references-->
[Managing Databases and Logins in Azure SQL Database]:https://msdn.microsoft.com/library/azure/ee336235.aspx

<!--Other Web references-->
