<properties
   pageTitle="SQL-tietovarasto (T-SQL) läpinäkyvä salauksen | Microsoft Azure"
   description="Läpinäkyvä salauksen (TDE)-SQL-tietovarasto (T-SQL)"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="09/24/2016"
   ms.author="rortloff;barbkess;sonyama"/>

# <a name="get-started-with-transparent-data-encryption-tde"></a>Aloita kanssa läpinäkyvä tietojen salaus (TDE)


> [AZURE.SELECTOR]
- [Yleistä suojauksesta](sql-data-warehouse-overview-manage-security.md)
- [Todennus](sql-data-warehouse-authentication.md)
- [Salauksen (portaalin)](sql-data-warehouse-encryption-tde.md)
- [Salauksen (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

## <a name="required-permssions"></a>Tarvittavat oikeudet

Jotta läpinäkyvä tietojen salaus (TDE) on oltava järjestelmänvalvoja tai dbmanager-roolin jäsen.

## <a name="enabling-encryption"></a>Salauksen ottaminen käyttöön

SQL-tietovarasto TDE käyttöön seuraavasti:

1. Palvelimessa, jossa tietokantaan, joka on järjestelmänvalvojan tai perusmuodon tietokannan **dbmanager** -roolin jäsen kirjautumisen avulla *pää* -tietokantayhteyden muodostamisessa
2. Suorittaa seuraavan lauseen tietokannan salaaminen.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Salauksen poistaminen käytöstä

SQL-tietovarasto TDE käytöstä seuraavasti:

1. Kirjautuminen järjestelmänvalvojana tai perusmuodon tietokannan **dbmanager** -roolin jäsen eli käyttämällä *perusmuodon* tietokantayhteyden muodostamisessa
2. Suorittaa seuraavan lauseen tietokannan salaaminen.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION OFF;
```

> [AZURE.NOTE] Keskeytetty SQL-tietovarasto täytyy jatkaa ennen kuin teet muutoksia TDE-asetukset.

## <a name="verifying-encryption"></a>Tarkistetaan salaus

Vahvistamiseksi salauksen tila SQL-tietovarasto, noudata seuraavia ohjeita:

1. Kirjautuminen järjestelmänvalvojana tai perusmuodon tietokannan **dbmanager** -roolin jäsen eli käyttämällä *perusmuoto* tai esiintymä-tietokantayhteyden muodostamisessa
2. Suorittaa seuraavan lauseen tietokannan salaaminen.

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

Tulos on ```1``` ilmaisee salatun tietokannan, ```0``` ilmaisee-salattu tietokanta.

## <a name="encryption-dmvs"></a>Salauksen DMVs  

- [sys.Databases][] 
- [sys.dm_pdw_nodes_database_encryption_keys][]


<!--Anchors-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.Databases]: http://msdn.microsoft.com/library/ms178534.aspx  
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx  

<!--Image references-->

<!--Link references-->
