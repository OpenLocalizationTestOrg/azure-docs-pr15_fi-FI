<properties
   pageTitle="Ratkaisu siirtäminen SQL-tietovarasto | Microsoft Azure"
   description="Siirron joka tuo ratkaisu Azure SQL-tietovarasto ympäristö ohjeita."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/30/2016"
   ms.author="barbkess;jrj;sonyama"/>

# <a name="migrate-your-solution-to-sql-data-warehouse"></a>Siirtää SQL-tietovarasto ratkaisu

SQL-tietovarasto on jaettu tietokanta, joka skaalaa elastically vastaamaan omia tarpeita. Suorituskyky ja skaalattavuus pitämään kaikki SQL Server-ominaisuuksia ei otettu käyttöön SQL-tietovarasto sisällä. Seuraavissa ohjeaiheissa siirron kosketa johonkin avaimen tekijät SQL-tietovarasto ratkaisu siirtämistä varten. Kuvioiden ja siten perinteinen tavoista ei aina parhaiten eri rakenteen suunnitteleminen tietojen varastoja akselille esitellään. Saatat siksi, että mukauttamisesta aiemman ratkaisun varmistaa voit hyödyntää SQL-tietovarasto myöntämä hajautettu ympäristössä.

Myös on tärkeää muistaa, että SQL-tietovarasto on mukaan Microsoft Azure-ympäristössä. Tämän vuoksi Valmistele voi myös sisältää tietojen siirtäminen pilveen. Tiedonsiirto on oma aihe ja tulisi pitää huolellisesti; Kun erityisesti asemista kasvavat. Tietojen siirtäminen ja tietojen lataamisen on erillinen hyödyllisiä.

## <a name="migration-guidance"></a>Siirto-ohjeet

Varmista, että olet lukenut näistä artikkeleista, jotta ymmärrät tuotteen erot ja peruskäsitteet ennen siirtymistä koulusi kautta.

- [Siirtää rakenteen][]
- [Tietojen siirtäminen][]
- [Siirtää koodi][]

## <a name="next-steps"></a>Seuraavat vaiheet

KISSA (asiakkaan neuvoa ryhmä) on myös erinomainen joitakin SQL-tietovarasto ohjeita, joiden niitä julkaista blogit kautta.  Tutustu niiden artikkelissa [Azure SQL-tietovarasto käytännössä tietojen siirtämistä][] siirron muita ohjeita.

<!--Image references-->

<!--Article references-->
[Siirtää rakenteen]: sql-data-warehouse-migrate-schema.md
[Tietojen siirtäminen]: sql-data-warehouse-migrate-data.md
[Siirtää koodi]: sql-data-warehouse-migrate-code.md


<!--MSDN references-->


<!--Other Web references-->
[Luotujen tietojen Azure SQL-tietovarasto käytännössä]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/
