<properties
   pageTitle="SQL-tietovarasto näkymien | Microsoft Azure"
   description="Vihjeitä, ratkaisujen kehittämiseen Azure SQL-tietovarasto Transact-SQL-näkymien käyttäminen."
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
   ms.date="07/01/2016"
   ms.author="jrj;barbkess;sonyama"/>


# <a name="views-in-sql-data-warehouse"></a>SQL-tietovarasto näkymissä

Näkymät ovat erityisen hyödyllisiä SQL-tietovarasto. Ne voidaan käyttää useilla eri tapoja parantaa äänenlaatua, ratkaisu.  Tässä artikkelissa esitellään täydentää ratkaisu näkymät sekä rajoituksia, jotka on otettava huomioon muutamia esimerkkejä.

> [AZURE.NOTE] Syntaksi `CREATE VIEW` ei ole kuvattu tämän artikkelin. Lue lisätietoja tämän viitetietoina MSDN-artikkelista [Luo NÄKYMÄ][] .

## <a name="architectural-abstraction"></a>Arkkitehtuuri otetaan
Yleisiä sovelluksen kuvion on luotava uudelleen luodaan luoda taulukon AS Valitse (CTAS) objektin uudelleennimeäminen kuviota, mutta tietojen lataaminen perään.

Alla olevassa esimerkissä Lisää uuden päivämäärän tietueet date-dimensio. Huomaa, miten uusi tabble DimDate_New, luomisen ja sitten uusi nimi korvaa taulukon alkuperäisen version.

```sql
CREATE TABLE dbo.DimDate_New
WITH (DISTRIBUTION = ROUND_ROBIN
, CLUSTERED INDEX (DateKey ASC)
)
AS
SELECT *
FROM   dbo.DimDate  AS prod
UNION ALL
SELECT *
FROM   dbo.DimDate_stg AS stg
;

RENAME OBJECT DimDate TO DimDate_Old;
RENAME OBJECT DimDate_New TO DimDate;

```

Tämän menetelmän voi aiheuttaa kuitenkin taulukot näkyvät ja käyttäjän näkymä samoin kuin "taulukossa ei ole" virhesanomia katoaa. Näkymien avulla voidaan tarjota käyttäjille yhdenmukainen kerroksen, kun taas pohjana olevat objektit on nimetty uudelleen. Ilmoittamalla käyttäjille näkymissä tietoihin pääsy tarkoittaa käyttäjien ei tarvitse näkyvyyden pohjana olevat taulukot. Tämä on yhtenäinen käyttäjäkokemuksen aikana varmistetaan, että tiedot varasto suunnittelijat voivat kehityttävä tietomalliin ja parantaa suorituskykyä käyttämällä CTAS tietojen lataaminen prosessin aikana.    

## <a name="performance-optimization"></a>Suorituskyvyn optimointi
Näkymien voidaan myös käyttää voidaan valvoa suorituskykyä optimoitu liitokset taulukoiden välille. Esimerkiksi näkymän voit sisällyttää tarpeettomat jaon avaimella liittävä ehdot Pienennä tietojen siirto osana.  Toinen etu näkymän voi olla pakottaa tietyn kyselyn tai liittävä vihje. Näkymien käyttäminen tällä tavalla takaa, että liitokset suoritetaan aina paras mahdollinen tavalla välttäminen käyttäjien muista oikea rakennetta, niiden liitokset edellyttää.

## <a name="limitations"></a>Rajoitukset
SQL-tietovarasto näkymät ovat vain metatiedot.  Tämän vuoksi seuraavat vaihtoehdot eivät ole käytettävissä:

-   Ei ole rakenteen sidonta-vaihtoehtoa
-   Taulukoita ei voi päivittää näkymän kautta
-   Näkymiä ei voi luoda väliaikaisia taulukoita päälle
-   Ei ole tukea Laajenna / NOEXPAND vihjeet
-   SQL-tietovarasto ei ole indeksoidut näkymät


## <a name="next-steps"></a>Seuraavat vaiheet
Artikkelissa kehitys Lisävihjeitä [SQL-tietovarasto kehittäminen yleiskatsaus][].
Saat `CREATE VIEW` syntaksi tutustumaan [Luo NÄKYMÄ][].

<!--Image references-->

<!--Article references-->
[SQL-tietovarasto kehittäminen yleiskatsaus]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[NÄKYMÄN LUOMINEN]: https://msdn.microsoft.com/en-us/library/ms187956.aspx

<!--Other Web references-->
