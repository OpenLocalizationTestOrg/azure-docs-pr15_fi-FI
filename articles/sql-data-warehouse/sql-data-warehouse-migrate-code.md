<properties
   pageTitle="SQL-koodin siirtäminen SQL-tietovarasto | Microsoft Azure"
   description="SQL-koodin siirtämisestä Azure SQL-tietovarasto, ratkaisujen kehittämiseen liittyviä vinkkejä."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/02/2016"
   ms.author="lodipalm;barbkess;sonyama;jrj"/>

# <a name="migrate-your-sql-code-to-sql-data-warehouse"></a>SQL-koodin siirtäminen SQL-tietovarasto

Kun siirtänyt koodisi toisesta tietokannasta SQL-tietovarasto, sinun on todennäköisesti halutaan muuttaa koodin base. SQL-tietovarasto joitakin ominaisuuksia merkittävästi voi parantaa suorituskykyä, kun ne on suunniteltu toimimaan hajautettu tavalla. Kuitenkin jos haluat säilyttää suorituskyky ja skaalattavuus, jotkin ominaisuudet eivät myös ole käytettävissä.

## <a name="common-t-sql-limitations"></a>Yleisiä T-SQL-rajoitukset

Seuraavassa luettelossa on yhteenveto yleisimmistä toiminto, jolla Azure SQL-tietovarasto ei tueta. Linkit vievät ratkaisuehdotuksia ominaisuus jota ei tueta:

- [ANSI-liitokset päivitykset][]
- [ANSI-poistaa liitokset][]
- [Yhdistä-lause][]
- usean tietokannan liitokset
- [kohdistimet][]
- [VALITSE... TUOMINEN][]
- [LISÄÄ... SUORITUS][]
- tulostus-lause
- tekstiin sidottu käyttäjän määrittämät funktiot
- usean lauseen Funktiot
- [Yleiset taulukon lausekkeet](#Common-table-expressions)
- [rekursiivinen Yleiset taulukon lausekkeet (CTE)] (#Recursive-common-table-expressions-(CTE)
- CLR-Funktiot ja toimintosarjat
- $partition-funktio
- taulukon muuttujat
- taulukon arvo-parametrit
- Jaetut tapahtumat
- Vahvista / työmäärä peruuttaminen
- Tallenna tapahtuma
- suorittamisen kontekstit (suoritus AS)
- [Group by-lause ja rollup / kuution / joukot ryhmittelyasetukset][]
- [Sisennystasojen lisäksi 8][]
- [näkymissä päivittäminen][]
- [Valitse muuttujan varauksen käyttö][]
- [Dynaaminen SQL-merkkijonojen ei MAX-tietotyyppiä][]

Useimmat näistä rajoituksista voi lähettäjä aiemmin ympärille. Edellä mainitut asiaa kehittäminen artikkeleissa toimitetaan selitykset.

## <a name="supported-cte-features"></a>Tuetut CTE ominaisuudet

Yleiset taulukon lausekkeet (CTEs) ovat osittain tuettuja SQL-tietovarasto.  CTE seuraavat ominaisuudet ovat tällä hetkellä tueta:

- CTE voidaan määrittää SELECT-lauseessa.
- Luo NÄKYMÄ-lauseessa voi määrittää CTE.
- Luo taulukko liitetään Valitse (CTAS) lauseessa voidaan määrittää CTE.
- CTE voidaan määrittää luominen REMOTE taulukon AS Valitse (CRTAS)-lauseessa.
- CTE voidaan määrittää luoda ULKOISEN taulukon AS Valitse (CETAS)-lauseessa.
- Remote taulukon voidaan viitata CTE.
- Ulkoinen taulukko voidaan viitata CTE.
- Useita CTE kyselyn määrityksiä voidaan määrittää CTE.

## <a name="cte-limitations"></a>CTE rajoitukset

Yleisiä taulukkolausekkeen on joitakin rajoituksia SQL-tietovarasto mukaan lukien:

- CTE jäljessä on yksi SELECT-lauseessa. Lisää-päivitys, poistaminen ja Yhdistä-lauseet eivät ole tuettuja.
- Yleisiä taulukkolauseke, joka sisältää viittauksia itse (rekursiivinen yleisiä taulukkolauseke) ei ole tuettu (Katso osan alapuolella).
- Määrittämällä useamman kuin yhden lauseen kanssa CTE ei ole sallittua. Esimerkiksi jos CTE_query_definition sisältää alikyselyn, kyseisen alikyselyn ei voi olla sisäkkäiset-lause, joka määrittää toisen CTE kanssa.
- ORDER BY-lause ei voi käyttää CTE_query_definition, paitsi jos TOP-lause on määritetty.
- Kun CTE käytetään-lauseessa, joka on osa suurta, lause ennen sen noudatettava puolipisteellä eroteltuina.
- Kun sitä käytetään lauseet valmistellut sp_prepare, CTEs toimivat samalla tavalla kuin muiden SELECT-lauseet PDW. Kuitenkin jos CTEs käytetään osana sp_prepare valmistellut CETAS, toiminnan voit lykätä SQL Server-ja muut PDW lauseet sidonta on otettu käyttöön sp_prepare tavasta. Jos VALITSET, viittaukset CTE käyttää väärässä sarakkeessa, joka ei ole CTE sp_prepare välittää ilman tunnistaminen virheen, mutta virhe olla virhe ilmenee, sp_execute aikana sen sijaan.

## <a name="recursive-ctes"></a>Rekursiiviset CTEs

SQL-tietovarasto rekursiivinen CTEs ei tueta.  Rekursiiviset CTE migraion voi olla hieman valmiiksi ja parhaat prosessi voidaan eritellä kyselyjä useita vaiheita. Voit tavallisesti Käytä silmukan ja täytä väliaikaisen taulukossa kuin päälle rekursiivinen väliaikaisen kyselyt käytöstä. Kun väliaikainen taulukko lisätään yhden tuloksen joukkona Palauta tiedot. Samaa menetelmää on käytetty ratkaisemaan `GROUP BY WITH CUBE` [group by-lause ja rollup / kuution / joukot ryhmittelyasetukset][] -artikkelissa.

## <a name="unsupported-system-functions"></a>Järjestelmää ei tueta-Funktiot

Saatavilla on myös joitakin järjestelmän funktioita, joita ei tueta. Joitakin tärkeimmät niistä voi olla yleensä käyttää tietovaraston ovat seuraavat:

- NEWSEQUENTIALID()
- @@NESTLEVEL()
- @@IDENTITY()
- @@ROWCOUNT()
- ROWCOUNT_BIG
- ERROR_LINE()

Joidenkin ongelmien ratkaisu on aiemmin ympärille.

## <a name="rowcount-workaround"></a>@@ROWCOUNTVaihtoehtoinen menetelmä

Voit kiertää tuki puuttuminen @@ROWCOUNT, Luo tallennettu toimintosarja, joka noutaa viimeisen rivimäärä sys.dm_pdw_request_steps ja suorita `EXEC LastRowCount` jälkeen DML-lause.

```sql
CREATE PROCEDURE LastRowCount AS
WITH LastRequest as 
(   SELECT TOP 1    request_id
    FROM            sys.dm_pdw_exec_requests
    WHERE           session_id = SESSION_ID()
    AND             resource_class IS NOT NULL
    ORDER BY end_time DESC
),
LastRequestRowCounts as
(
    SELECT  step_index, row_count
    FROM    sys.dm_pdw_request_steps
    WHERE   row_count >= 0
    AND     request_id IN (SELECT request_id from LastRequest)
)
SELECT TOP 1 row_count FROM LastRequestRowCounts ORDER BY step_index DESC
;
```

## <a name="next-steps"></a>Seuraavat vaiheet
Kaikki tuetut T-SQL-lauseita kattavaan luetteloon [Transact-SQL-][]artikkeleissa.

<!--Image references-->

<!--Article references-->
[ANSI-liitokset päivitykset]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements
[ANSI-poistaa liitokset]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements
[Yhdistä-lause]: ./sql-data-warehouse-develop-ctas.md#replace-merge-statements
[LISÄÄ... SUORITUS]: ./sql-data-warehouse-tables-temporary.md#modularizing-code
[Transact-SQL-ohjeaiheet]: ./sql-data-warehouse-reference-tsql-statements.md

[kohdistimet]: ./sql-data-warehouse-develop-loops.md
[VALITSE... TUOMINEN]: ./sql-data-warehouse-develop-ctas.md#selectinto
[Group by-lause ja rollup / kuution / joukot ryhmittelyasetukset]: ./sql-data-warehouse-develop-group-by-options.md
[Sisennystasojen lisäksi 8]: ./sql-data-warehouse-develop-transactions.md
[näkymissä päivittäminen]: ./sql-data-warehouse-develop-views.md
[Valitse muuttujan varauksen käyttö]: ./sql-data-warehouse-develop-variable-assignment.md
[Dynaaminen SQL-merkkijonojen ei MAX-tietotyyppiä]: ./sql-data-warehouse-develop-dynamic-sql.md

<!--MSDN references-->

<!--Other Web references-->
