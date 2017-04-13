<properties
   pageTitle="Välineen kyselyihin otsikoiden käyttäminen SQL-tietovarasto | Microsoft Azure"
   description="Vihjeitä, ratkaisujen kehittämiseen Azure SQL-tietovarasto otsikot välineen kyselyjen käyttäminen."
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

# <a name="use-labels-to-instrument-queries-in-sql-data-warehouse"></a>Käytä otsikoita välineen kyselyihin SQL-tietovarasto
SQL-tietovarasto tukee käsite, kutsutaan kyselyn otsikot. Tarkista ennen ohjaaminen pois kaikki syvyys japanin yksi esimerkki:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

Tämän viimeisen rivin tunnisteet kyselyn merkkijonon "Omat kyselyn selite". Tämä on erityisen hyödyllisiä otsikko on kysely-pysty DMVs kautta. Tämä avulla us järjestelmä, jonka avulla ongelma kyselyt ja tunnistaminen ETL Suorita edistyminen.

Hyvä nimeämiskäytäntöä auttaa todella tähän. Esimerkiksi tähän tapaan ' PROJEKTIN: TOIMINTOSARJAN: LAUSEEN: KOMMENTTI ' yksilöimiseen tietolähteen ohjausobjektin koodissa toiseen kyselyn avulla.

Etsi otsikko voit seuraavan kyselyn, joka käyttää dynaamisen hallinta-näkymien:

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [AZURE.NOTE] On tärkeää, että voit rivittää hakasulkeisiin tai word-otsikon ympärille lainausmerkkejä kun kysely suoritetaan. Otsikko on varattu sana, ja se aiheuttaa virheen, jos se ei ole eroteltu.


## <a name="next-steps"></a>Seuraavat vaiheet
Artikkelissa kehitys Lisävihjeitä [kehittäminen yleiskatsaus][].

<!--Image references-->

<!--Article references-->
[kehittäminen yleiskatsaus]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
