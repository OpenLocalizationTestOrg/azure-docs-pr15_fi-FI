<properties
   pageTitle="Azure Data Factory käyttäminen SQL-tietovarasto | Microsoft Azure"
   description="Azure tietojen Factory (SYÖTTÖ) käyttäminen Azure SQL-tietovarasto, ratkaisujen kehittämiseen liittyviä vinkkejä."
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
   ms.date="08/08/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="use-azure-data-factory-with-sql-data-warehouse"></a>SQL-tietovarasto Azure Data Factory käyttäminen

Azure Data Factory mahdollistaa täysin hallitun orchestrating tietojen siirto ja SQL-tietovarasto tallennettujen toimintosarjojen suorittamisen.  Näin voit helpommin käyttöönottoa varten ja joissa monimutkaisia Pura Muunna ja lataa (ETL) menettelyt SQL-tietovarasto kanssa. Lisätietoja yleiskatsaus Azure Data Factory on [Azure Data Factory ohjeissa][].

## <a name="data-movement"></a>Tietojen siirto

Azure Data Factory mahdollistaa tietojen siirto välillä sekä paikallisesti että eri Azure-palveluissa.  Yleistä, nykyisen integrointi Azure Data Factory tukee tietojen siirto ja sieltä pois seuraavista sijainneista:

+ Azure-blob-säiliö
+ Azure SQL-tietokanta
+ Paikallisen SQL Serverin kanssa
+ SQL Server-IaaS

Voit määrittää tietojen kopioiminen tehtävän lisätietoja [Azure Data Factory tietojen kopioiminen][]

## <a name="stored-procedures"></a>Tallennettujen toimintosarjojen
 Sen avulla voidaan ajoittaa tiedonsiirto samalla tavalla Azure Data Factory voi käyttää myös orchestrate tallennettujen toimintosarjojen suorittamisen.  Tällöin luoda monimutkaisia putkistot ja laajentaa Azure Data Factory mahdollisuus SQL-tietovarasto laskennallinen esitysmuotoa.

## <a name="next-steps"></a>Seuraavat vaiheet
Katso yleiskuvaus integrointi on [SQL-tietovarasto integroinnin yleiskatsaus][].
Artikkelissa kehitys Lisävihjeitä [SQL-tietovarasto kehittäminen yleiskatsaus][].

<!--Image references-->

<!--Article references-->

[Kopioi tiedot ja Azure Data Factory]: ../data-factory/data-factory-data-movement-activities.md
[SQL-tietovarasto kehittäminen yleiskatsaus]: ./sql-data-warehouse-overview-develop.md
[SQL-tietovarasto integroinnin yleiskatsaus]: ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory-asiakirjat]:https://azure.microsoft.com/documentation/services/data-factory/

