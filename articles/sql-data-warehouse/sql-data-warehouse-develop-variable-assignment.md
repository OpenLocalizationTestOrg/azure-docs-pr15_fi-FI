<properties
   pageTitle="Määritä SQL-tietovarasto muuttujat | Microsoft Azure"
   description="Määrittäminen Transact-SQL Azure SQL-tietovarasto muuttujat, ratkaisujen kehittämiseen liittyviä vinkkejä."
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

# <a name="assign-variables-in-sql-data-warehouse"></a>Määritä SQL-tietovarasto muuttujat
SQL-tietovarasto muuttujat määritetään käyttämällä `DECLARE` lause tai `SET` lause.

Kaikki seuraavat ovat täysin kelvollinen tavalla muuttujan arvo:

## <a name="setting-variables-with-declare"></a>Asetus muuttujat DECLARE kanssa

Valmistellaan muuttujat DECLARE kanssa on kätevä tapa määrittää SQL-tietovarasto muuttujan arvo.

```sql
DECLARE @v  int = 0
;
```

Voit myös määrittää kerralla enemmän kuin yksi muuttuja DECLARE. Et voi käyttää `SELECT` tai `UPDATE` toiminto:

```sql
DECLARE @v  INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Smith')
,       @v1 INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Jones')
;
```

Et voi alustaminen tai käyttää muuttujaa saman DECLARE-lause. Esitä kohdan alla olevassa esimerkissä **ei** voi käyttää @p1 on alustaa sekä käyttää samaa DECLARE-lauseessa. Tämä aiheuttaa virheen.

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## <a name="setting-values-with-set"></a>Asetus arvojen määrittäminen
On hyvin yleisiä menetelmän yhden muuttujan määrittämistä varten.

Kaikki seuraavissa esimerkeissä on kelvollinen asetus muuttujan määrittäminen tavalla:

```sql
SET     @v = (Select max(database_id) from sys.databases);
SET     @v = 1;
SET     @v = @v+1;
SET     @v +=1;
```

Voit määrittää yhden muuttujan kerrallaan vain omaavien. Kuin edellä näkevät koostetun operaattoreita on sallittu.

## <a name="limitations"></a>Rajoitukset
Et voi käyttää muuttujan valinta tai Päivitä.


## <a name="next-steps"></a>Seuraavat vaiheet
Artikkelissa kehitys Lisävihjeitä [kehittäminen yleiskatsaus][].

<!--Image references-->

<!--Article references-->
[kehittäminen yleiskatsaus]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
