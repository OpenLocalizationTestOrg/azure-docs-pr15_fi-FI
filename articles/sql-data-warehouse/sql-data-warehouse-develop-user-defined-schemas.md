<properties
   pageTitle="Käyttäjän määrittämiä rakenteita-SQL-tietovarasto | Microsoft Azure"
   description="Vihjeitä, ratkaisujen kehittämiseen Azure SQL-tietovarasto Transact-SQL-rakenteita käytön."
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
   ms.date="06/14/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="user-defined-schemas-in-sql-data-warehouse"></a>Käyttäjän määrittämiä rakenteita SQL varastossa

Perinteinen tietojen varastoja käyttää usein erillisten tietokantojen luomiseen sovelluksen rajojen työmäärää, toimialueen tai suojaus. Perinteinen SQL Server-tietovarasto saattavat sisältää esimerkiksi väliaikaisen tietokannan data warehouse tietokantaan ja tietojen mart tietokantoja. Tämä topologian tietokantojen toimii työmäärää sekä suojaus-arkkitehtuuri reunaa.

Sen sijaan SQL-tietovarasto suoritetaan koko tietojen varaston kuormituksen yhden tietokannasta. Ristiviittauksen tietokannan liitokset eivät ole sallittuja. Tämän vuoksi SQL-tietovarasto odottaa varasto avulla voi olla tallennettuna yhden tietokannan kaikki taulukot.

> [AZURE.NOTE] SQL Data Warehouse ei tue cross tietokantakyselyt tule. Näin ollen data warehouse käyttöotot, joka hyödyntää tätä mallia on tarkistettava.

## <a name="recommendations"></a>Suosituksia

Nämä ovat kokoamisesta työmääriä, suojaus, toimialueen ja toiminnallisia rajat käyttämällä käyttäjän määrittämiä rakenteita koskevia suosituksia

1. Koko data warehouse havainnollistamiseen käyttää yksi tietovarasto SQL-tietokanta
2. Yhdistäminen aiemmin data warehouse ympäristön yhden tietovarasto SQL-tietokantaan
3. Hyödyntää **käyttäjän määrittämiä rakenteita** antamaan aiemmin käyttöön tietokantojen käyttäminen reunaa.

Jos käyttäjän määrittämiä rakenteita ei ole käytetty aiemmin käytössäsi puhtaalta pöydältä. Käyttää vanha tietokannan nimi yksinkertaisesti pohjana käyttäjän määrittämiä-rakenteita tietovarasto SQL-tietokantaan.

Jos rakenteet on jo käytetty sinulla on muutamalla eri tavalla:

1. Poista vanha rakenteen nimiä ja aloittaa ajan tasalla
2. Säilytä aiempien versioiden rakenteen nimet mukaan valmiiksi odotetaan vanha rakenteen nimi taulukkonimi
3. Säilytä aiempien versioiden rakenteen nimet käyttämällä näkymiä taulukon ylimääräisiä rakenteessa vanha rakenne on luotava uudelleen.

> [AZURE.NOTE] Ensimmäisen tarkastuksen vaihtoehto 3 saattaa näyttää esimerkiksi eniten kiinnostavia vaihtoehto. Pirun näköinen on kuitenkin tiedot. Näkymät ovat vain luku-SQL-tietovarasto. Tietoja tai taulukon muutoksista on suoritettava perustaulukon vastaan. Vaihtoehto 3 esitellään myös näkymien kerroksen järjestelmään. Haluat ehkä antaa tämän joitakin muita haluat näyttää, jos käytössäsi on näkymät-kohdassa arkkitehtuuri jo.


### <a name="examples"></a>Esimerkkejä:

Toteuta käyttäjän määrittämiä rakenteita tietokannan nimien perusteella

```sql
CREATE SCHEMA [stg]; -- stg previously database name for staging database
GO
CREATE SCHEMA [edw]; -- edw previously database name for the data warehouse
GO
CREATE TABLE [stg].[customer] -- create staging tables in the stg schema
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[customer] -- create data warehouse tables in the edw schema
(       CustKey BIGINT NOT NULL
,       ...
);
```

Säilytä aiempien versioiden rakenteen nimet mukaan valmiiksi odotetaan ne taulukon nimeä. Käytä rakenteita työmäärää reunaa.

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- edw defines the data warehouse boundary
GO
CREATE TABLE [stg].[dim_customer] --pre-pend the old schema name to the table and create in the staging boundary
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[dim_customer] --pre-pend the old schema name to the table and create in the data warehouse boundary
(       CustKey BIGINT NOT NULL
,       ...
);
```

Säilytä aiempien versioiden rakenteen nimet näkymien käyttäminen

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- stg defines the data warehouse boundary
GO
CREATE SCHEMA [dim]; -- edw defines the legacy schema name boundary
GO
CREATE TABLE [stg].[customer] -- create the base staging tables in the staging boundary
(       CustKey BIGINT NOT NULL
,       ...
)
GO
CREATE TABLE [edw].[customer] -- create the base data warehouse tables in the data warehouse boundary
(       CustKey BIGINT NOT NULL
,       ...
)
GO
CREATE VIEW [dim].[customer] -- create a view in the legacy schema name boundary for presentation consistency purposes only
AS
SELECT  CustKey
,       ...
FROM    [edw].customer
;
```

> [AZURE.NOTE] Muutoksista rakenteen strategia on suojausmalli lukemalla tietokannan. Monissa tapauksissa välttämättä voi helpottaa suojausmalli määrittämällä rakenne-tason käyttöoikeudet. Jos eritellympiä käyttöoikeuksia tarvitaan, voit käyttää tietokannan roolit.

## <a name="next-steps"></a>Seuraavat vaiheet
Artikkelissa kehitys Lisävihjeitä [kehittäminen yleiskatsaus][].

<!--Image references-->

<!--Article references-->
[kehittäminen yleiskatsaus]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
