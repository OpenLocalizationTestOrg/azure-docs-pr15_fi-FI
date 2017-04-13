<properties
   pageTitle="Päätökset ja coding SQL-tietovarasto kehittäminen | Microsoft Azure"
   description="Kehitystä, suunnitteluun, suositukset ja coding tekniikoiden SQL-tietovarasto."
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
   ms.date="08/16/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="design-decisions-and-coding-techniques-for-sql-data-warehouse"></a>Suunnitteluun ja SQL-tietovarasto coding tekniikat

Katso ymmärtämään SQL-tietovarasto avaimen suunnitteluun, suositukset ja coding tekniikoita kehittäminen näissä artikkeleissa kautta.

## <a name="key-design-decisions"></a>Avaimen suunnitteluun
Seuraavissa artikkeleissa Korosta avaimen käsitteitä ja rakenteen päätöksiä tarvitset käyttämällä SQL-tietovarasto jaettujen tietojen varastossa kehittäminen ymmärtäminen:

- [yhteydet][]
- [samanaikainen][]
- [tapahtumat][]
- [käyttäjän määrittämiä rakenteita][]
- [Taulun jakelu][]
- [taulukon indeksit][]
- [taulukko, osiot][]
- [CTAS][]
- [tilastotiedot][]

## <a name="development-recommendations-and-coding-techniques"></a>Kehittäminen suosituksia ja coding tekniikat
Tietyn coding tekniikoita, vihjeet ja suosituksia kehittäminen SQL-tietovarasto korostaminen seuraavissa artikkeleissa:

- [tallennettujen toimintosarjojen][]
- [otsikot][]
- [näkymät][]
- [Väliaikaiset taulukot][]
- [Dynaamisen SQL][]
- [Toistuminen][]
- [ryhmän mukaan-vaihtoehdot][]
- [muuttujan][]

## <a name="next-steps"></a>Seuraavat vaiheet
Kun olet käynyt läpi kehittäminen on artikkeleissa voit tarkastella lisätietoja [Transact-SQL-viittaus][] -sivulla tuetut syntaksista SQL-tietovarasto.

<!--Image references-->

<!--Article references-->
[samanaikainen]: ./sql-data-warehouse-develop-concurrency.md
[yhteydet]: ./sql-data-warehouse-connect-overview.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[Dynaamisen SQL]: ./sql-data-warehouse-develop-dynamic-sql.md
[ryhmän mukaan-vaihtoehdot]: ./sql-data-warehouse-develop-group-by-options.md
[otsikot]: ./sql-data-warehouse-develop-label.md
[Toistuminen]: ./sql-data-warehouse-develop-loops.md
[tilastotiedot]: ./sql-data-warehouse-tables-statistics.md
[tallennettujen toimintosarjojen]: ./sql-data-warehouse-develop-stored-procedures.md
[Taulun jakelu]: ./sql-data-warehouse-tables-distribute.md
[taulukon indeksit]: ./sql-data-warehouse-tables-index.md
[taulukko, osiot]: ./sql-data-warehouse-tables-partition.md
[Väliaikaiset taulukot]: ./sql-data-warehouse-tables-temporary.md
[tapahtumat]: ./sql-data-warehouse-develop-transactions.md
[käyttäjän määrittämiä rakenteita]: ./sql-data-warehouse-develop-user-defined-schemas.md
[muuttujan]: ./sql-data-warehouse-develop-variable-assignment.md
[näkymät]: ./sql-data-warehouse-develop-views.md
[Transact-SQL-viittaus]: ./sql-data-warehouse-overview-reference.md

<!--MSDN references-->
[renaming objects]: https://msdn.microsoft.com/library/mt631611.aspx

<!--Other Web references-->
