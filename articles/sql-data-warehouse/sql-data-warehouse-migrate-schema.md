<properties
   pageTitle="Rakenteen siirtäminen SQL-tietovarasto | Microsoft Azure"
   description="Rakenteen siirtämisestä Azure SQL-tietovarasto, ratkaisujen kehittämiseen liittyviä vinkkejä."
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
   ms.date="08/25/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="migrate-your-schema-to-sql-data-warehouse"></a>Rakenteen siirtäminen SQL-tietovarasto#

Seuraavat yhteenvetoja auttavat ymmärtämään SQL Serverin ja SQL-tietovarasto, joiden avulla voit siirtää tietokannan välisiä eroja.

## <a name="table-migration"></a>Taulukon siirtäminen

Siirtyminen taulukoissa, kun haluat tutustutaan SQL-tietovarasto taulukoissa taulukon ominaisuudet.  [Taulukon yleistä][] on tapa aloittaa.  Tässä artikkelissa esitellään tärkeimmät seikat taulukoksi, kuten taulukon tilastotiedot, jakaminen ja indeksoinnin hajautuksessa luotaessa.  Se kattaa myös joitakin [ominaisuuksia ei tueta taulukon][] ja ratkaisuehdotuksia.

SQL-tietovarasto tukee yleisiä liiketoiminta-tietotyypit.  Artikkelissa [tietotyyppien][] luettelo tukee ja [ei-tuetut tietotyypit][].  [Tietotyyppien][] -artikkelissa on myös kysely, joka tunnistaa [ei-tuetut tietotyypit][].  Kun muunnat tietojen tietotyyppejä, muista katsomalla [tietotyyppi parhaita käytäntöjä][].

## <a name="next-steps"></a>Seuraavat vaiheet
Kun tietokannan rakennetta on siirretty onnistuneesti SQL-tietovarasto, siirry seuraavissa artikkeleissa:

- [Tietojen siirtäminen][]
- [Siirtää koodi][]

Lisätietoja SQL-tietovarasto parhaita käytäntöjä [parhaista käytännöistä][] on artikkelissa.

<!--Image references-->

<!--Article references-->
[Siirtää koodi]: ./sql-data-warehouse-migrate-code.md
[Tietojen siirtäminen]: ./sql-data-warehouse-migrate-data.md
[parhaat käytännöt]: ./sql-data-warehouse-best-practices.md
[taulukon yleiskatsaus]: ./sql-data-warehouse-tables-overview.md
[taulukkotoimintoja ei tueta]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[tietotyypit]: ./sql-data-warehouse-tables-data-types.md
[ei-tuetut tietotyypit]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types
[tietotyyppi parhaat käytännöt]: ./sql-data-warehouse-tables-data-types.md#data-type-best-practices

<!--MSDN references-->


<!--Other Web references-->
