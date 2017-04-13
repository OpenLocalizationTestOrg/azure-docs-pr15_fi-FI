<properties
   pageTitle="SQL-tietovarasto tapahtumia | Microsoft Azure"
   description="Vihjeitä tapahtumat-Azure SQL-tietovarasto, ratkaisujen kehittämiseen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/31/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="transactions-in-sql-data-warehouse"></a>SQL-tietovarasto tapahtumat

Kun tuttuun tapaan, SQL-tietovarasto tukee tapahtumia tietojen varaston kuormituksen osana. Jotta SQL-tietovarasto suorituskyvyn määritetään tasolla joitakin ominaisuuksia on kuitenkin rajoitettu verrattaessa SQL Server. Tässä artikkelissa korostaa erot ja näyttää muut. 

## <a name="transaction-isolation-levels"></a>Tapahtuman eristystaso tasot
SQL-tietovarasto toteuttaa HAPPAMAN tapahtumat. Tapahtumien tuen eristystaso on kuitenkin rajoitettu `READ UNCOMMITTED` ja tämä ei voi muuttaa. Voit ottaa käyttöön useita coding menetelmät estämiseksi dirty lukee tiedot, jos kyseessä on huolta puolestasi. Suosituimmat menetelmät hyödyntää CTAS ja taulukon osion vaihtaminen (kutsutaan usein liukuva ikkunan kuvion) haluat estää käyttäjiä, jotka yhä valmiiden tietojen kyselyt. Näkymiä, jotka valmiiksi tietojen suodattaminen on myös Suositut.  

## <a name="transaction-size"></a>Tapahtuman kokoa
Koko on rajallinen yhtä muokkaus tapahtuma. Rajaa käytetään tänään "kohti jakauman". Tämän vuoksi yhteensä voidaan laskea kertomalla rajoitus jakauman määrä. Lähes tapahtuman rivien enimmäismäärä jakamalla jakauman pää kunkin rivin koon mukaan. Muuttujan pituuden sarakkeiden huomioon ottaen keskimääräinen sarakkeen pituuden kuin käyttämällä enimmäiskoon.

Valitse seuraavat oletukset alla olevassa taulukossa on tehty:

* Tietoja jopa jakautuminen on tapahtunut 
* Keskimääräinen rivin pituus on 250 tavua

| [DWU][]    | Cap kohti jakauma (GiB) | Jaot määrä | Tapahtuman enimmäiskoko (GiB) | # Rivien jakauman kohden | Maks rivien tapahtumaa kohden |
| ------ | -------------------------- | ----------------------- | -------------------------- | ----------------------- | ------------------------ |
| DW100  |  1                         | 60                      |   60                       |   4,000,000             |    240,000,000           |
| DW200  |  1,5                       | 60                      |   90.                       |   6,000,000             |    360,000,000           |
| DW300  |  2,25                      | 60                      |  135                       |   9,000,000             |    540,000,000           |
| DW400  |  3                         | 60                      |  180                       |  12,000,000             |    720,000,000           |
| DW500  |  3,75                      | 60                      |  225                       |  15,000,000             |    900,000,000           |
| DW600  |  4.5                       | 60                      |  270                       |  18,000,000             |  1,080,000,000           |
| DW1000 |  7.5                       | 60                      |  450                       |  30 000 000             |  1,800,000,000           |
| DW1200 |  9                         | 60                      |  540                       |  36,000,000             |  2,160,000,000           |
| DW1500 | 11,25                      | 60                      |  675                       |  45,000,000             |  2,700,000,000           |
| DW2000 | 15                         | 60                      |  900                       |  60,000,000             |  3,600,000,000           |
| DW3000 | 22.5                       | 60                      |  1,350                     |  90,000,000             |  5,400,000,000           |
| DW6000 | 45                         | 60                      |  2,700                     | 180,000,000             | 10,800,000,000           |

Tapahtuman kokorajoitus otetaan käyttöön tapahtuman tai toiminnon kohden. Se ei ole käytetty kaikki samanaikaisia yli. Tämän vuoksi myyntitapahtumien sallitaan lokiin tietomäärän kirjoittamista. 

Optimoi ja minimoida lokiin kirjoitetut tiedot [tapahtumien parhaat käytännöt][] -artikkelissa.

> [AZURE.WARNING] Tapahtuman enimmäismäärä kokoa voidaan saavuttaa ainoastaan HASH tai jakaa ROUND_ROBIN taulukoiden tietojen määrä missä jopa. Jos tapahtuma on kirjoittaminen Vino tavalla jaot rajoitus on todennäköisesti saavutetaan ennen tapahtuman kokoa.
<!--REPLICATED_TABLE-->

## <a name="transaction-state"></a>Toimintotila
SQL-tietovarasto raportin epäonnistuneen tapahtuman, käyttämällä arvoa -2 XACT_STATE()-funktiota käytetään. Tämä tarkoittaa, että tapahtuma on epäonnistunut ja merkitty vain palauttamista varten

> [AZURE.NOTE] 2 osoittamaan epäonnistuneen tapahtuman XACT_STATE-funktio käyttää edustaa eri tavalla SQL Server-tietokantaan. SQL Server käyttää arvo-1 edustavan yhdistelmävisualisoinnin committable tapahtuma. SQL Server hyväksyt virheitä tapahtuman sisällä ilman sitä tarvitse merkitä kuin yhdistelmävisualisoinnin committable. Esimerkiksi `SELECT 1/0` aiheuttaa virheen, mutta Pakota tapahtumaa yhdistelmävisualisoinnin committable tilaan. SQL Server sallii lukee myös poistamalla committable tapahtuman. SQL-tietovarasto ei avulla voit tehdä tämän. Jos näyttöön tulee virhesanoma, SQL-tietovarasto tapahtuman sisällä syöttää automaattisesti-2-tilaan ja ei voi tehdä Valitse lauseet edelleen, kunnes lause on peruutettu. Tämän vuoksi on tärkeää Tarkista sovelluksen koodin nähdäksesi, jos se käyttää XACT_STATE() voit joutua koodin muutokset.

Esimerkiksi SQL Server voi näkyä tapahtuman, joka näyttää tältä:

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;
        
        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

Jos jätät koodisi ennalleen yläpuolella näyttöön tulee seuraava virhesanoma:

Virhesanoma 111233, tason 16, tila 1, rivi 1 111233; Nykyinen tapahtuma on keskeytetty ja odottavat muutokset on peruutettu. Syy: Vain peruutus-tilaan tapahtuman ei erikseen palautettiin ennen DDL, Enimmäiskuolleisuusrajaa tai SELECT-lauseessa.

Saat myös funktioita ERROR_ * tulos ei ole.

SQL-tietovarasto koodin täytyy olla hieman muokattavan:

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();
        
        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;
    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

Odotettu toiminta havaitaan nyt. Tapahtuman virheen hallinnasta ja funktioiden ERROR_ * arvot odotetulla tavalla.

Kaikki, joka on muuttunut on `ROLLBACK` tapahtuman oli tehtävä, ennen kuin virhetietojen luku `CATCH` estä.

## <a name="errorline-function"></a>Error_Line()-funktio
Kannattaa myös, että SQL Data Warehouse ei toteuta tai tue ERROR_LINE()-funktiota. Jos sinulla on sinun on poistettava sen standardien kanssa SQL-tietovarasto koodi. Käytä kyselyn otsikoiden koodisi toteuttamisesta vastaavia toimintoja. Lue lisätietoja [otsikko][] -artikkelista Saat lisätietoja tämän toiminnon.

## <a name="using-throw-and-raiserror"></a>THROW ja RAISERROR käyttäminen
THROW on enemmän Moderni nostaminen SQL-tietovarasto poikkeukset, mutta RAISERROR tuetaan myös. On, jotka ovat nykyarvo maksaa huomion kuitenkin joitakin eroja.

- Käyttäjän määrittämät numeroita ei voi olla THROW 100,000 150,000 alueen virhesanomat
- 50000 on kiinnitetty RAISERROR virhesanomat
- Sys.messages käyttöä ei tueta

## <a name="limitiations"></a>Limitiations
SQL-tietovarasto on muutamia muita rajoituksia, jotka liittyvät tapahtumat.

He ovat seuraavat:

- Ei ole jaettu tapahtumat
- Sisäkkäisiä tapahtumia ei sallita
- Ei tallenna pisteitä sallita
- Nimetty tapahtumia
- Ei ole merkityt tapahtumat
- Ei tukea DDL, kuten `CREATE TABLE` sisällä käyttäjän määrittämät tapahtuman

## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja optimointi tapahtumat on artikkelissa [tapahtumat parhaat käytännöt][].  Lisätietoja muista SQL-tietovarasto parhaista käytännöistä on artikkelissa [SQL-tietovarasto parhaita käytäntöjä][].

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[development overview]: ./sql-data-warehouse-overview-develop.md
[Tapahtumien parhaat käytännöt]: ./sql-data-warehouse-develop-best-practices-transactions.md
[SQL-tietovarasto parhaat käytännöt]: ./sql-data-warehouse-best-practices.md
[OTSIKKO]: ./sql-data-warehouse-develop-label.md

<!--MSDN references-->

<!--Other Web references-->
