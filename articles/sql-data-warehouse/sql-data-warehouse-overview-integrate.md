<properties
   pageTitle="SQL-tietovarasto integroitu ratkaisujen luominen | Microsoft Azure"
   description="Työkalut ja kumppaneiden kanssa ratkaisuja, jotka SQL-tietovarasto integroida. "
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
   ms.date="05/31/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

#<a name="leverage-other-services-with-sql-data-warehouse"></a>Hyödyntää muiden palveluiden kanssa SQL-tietovarasto
Lisäksi sen perustoiminnot SQL-tietovarasto avulla käyttäjä voi hyödyntää monia muiden Azure se: n rinnalla.  Tarkemmin sanottuna olemme ovat tällä hetkellä tulleet monitasoisissa integroida seuraavat vaiheet:

+ Power BI
+ Azure Data Factory
+ Azure koneen oppiminen
+ Azure Stream Analytics

Yritämme yhdistämään Lisää palveluja Azure ekosysteemissä yli.

##<a name="power-bi"></a>Power BI
Power BI-integroinnin avulla voit hyödyntää SQL-tietovarasto dynaaminen ilmoittaa Laske potenssiin ja visualisointi Power BI. Power BI-integrointi on tällä hetkellä:

+ **Suora yhteyden**: monimutkaisempi looginen kopioiminen palvelimesta vastaan SQL-tietovarasto-yhteyttä.  Tämä on nopeampaa analyysi suurempi asteikolla.
+ **Avaa Power BI**: "Avaa Power BI-painike välittää esiintymän tiedot Power BI-salliminen Lisää saumattoman yhteyden.

Katso [Power BI integroida](./sql-data-warehouse-integrate-power-bi.md) tai lisätietoja [Power BI-ohjeista](http://blogs.msdn.com/b/powerbi/archive/2015/06/24/exploring-azure-sql-data-warehouse-with-power-bi.aspx) .

##<a name="azure-data-factory"></a>Azure Data Factory
Azure Data Factory antaa käyttäjien luoda monimutkaisia Pura kuormituksen putkistot hallitun ympäristö.  Azure Data Factory SQL tietovarasto integrointi sisältää seuraavat:

+ **Tallennettujen toimintosarjojen**: Orchestrate SQL-tietovarasto tallennettujen toimintosarjojen suorittamisen.
+ **Kopioi**: käyttäminen tietojen siirtämiseen SQL-tietovarasto SYÖTTÖ.  Tätä toimintoa voi käyttää SYÖTTÖ on vakio tietojen siirtämistä järjestelmä tai PolyBase kattaa-kohdassa. 

Katso lisätietoja [Azure Data Factory integroida](./sql-data-warehouse-integrate-azure-data-factory.md) tai lisätietoja [Azure Data Factory ohjeissa](https://azure.microsoft.com/documentation/services/data-factory/) .

##<a name="azure-machine-learning"></a>Azure koneen oppiminen
Azure koneen oppiminen on täysin hallitun analytics-palvelu, jonka avulla käyttäjät voivat luoda monimutkaisia mallien hyödyntäminen suuri joukko ennakoivan työkaluja.  SQL Data Warehouse tue lähde-ja näiden mallien kohdetta seuraavia toimintoja:

+ **Tietojen lukeminen:** Asema mallien avulla T-SQL vastaan SQL-tietovarasto tasolla.
+ **Tietojen kirjoittaminen:** Vahvista muutokset takaisin SQL-tietovarasto minkä tahansa mallista.

Artikkelissa [Azure koneen Learning integroida](./sql-data-warehouse-integrate-azure-machine-learning.md) tai lisätietoja [Azure koneen Learning ohjeissa](https://azure.microsoft.com/services/machine-learning/) .

##<a name="azure-stream-analytics"></a>Azure Stream Analytics
Azure Stream Analytics on monimutkainen, täysin hallitun infrastruktuurin käsittelyn ja muissa Azure tapahtuma-toiminnosta tapahtumatietoja.  SQL-tietovarasto integrointi mahdollistaa streaming tehokkaasti käsittelee ja tallennettu rinnalla relaatiotietoja ottaminen käyttöön tarkempaa, monimutkaisemman analyysin tiedot.  

+ **Projektin Output:** Lähettää tulosteen Stream Analytics töistä suoraan SQL-tietovarasto.

Artikkelissa [Azure Stream Analytics integroida](./sql-data-warehouse-integrate-azure-stream-analytics.md) tai lisätietoja [Azure Stream Analytics-ohjeista](https://azure.microsoft.com/documentation/services/stream-analytics/) .

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop/

[Azure Data Factory]: sql-data-warehouse-integrate-azure-data-factory.md
[Azure Machine Learning]: sql-data-warehouse-integrate-azure-machine-learning.md
[Azure Stream Analytics]: sql-data-warehouse-integrate-azure-stream-analytics.md
[Power BI]: sql-data-warehouse-integrate-power-bi.md
[Partners]: sql-data-warehouse-partner-business-intelligence.md

<!--MSDN references-->

<!--Other Web references-->
