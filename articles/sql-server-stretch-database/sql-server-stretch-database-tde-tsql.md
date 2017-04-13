<properties
   pageTitle="Ota käyttöön SQL Server Venytys tietokannan Azure TSQL läpinäkyvä salauksen (TDE) | Microsoft Azure"
   description="Ota käyttöön SQL Server Venytys tietokannan Azure TSQL läpinäkyvä salauksen (TDE)"
   services="sql-server-stretch-database"
   documentationCenter=""
   authors="douglaslMS"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-server-stretch-database"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="06/14/2016"
   ms.author="douglaslMS"/>

# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure-transact-sql"></a>Läpinäkyvä salauksen (TDE) käyttöön Venytys tietokannan Azure (Transact-SQL)
> [AZURE.SELECTOR]
- [Azure portal](sql-server-stretch-database-encryption-tde.md)
- [TSQL](sql-server-stretch-database-tde-tsql.md)

Läpinäkyvä tietojen salaus (TDE) auttaa suojaamaan tietokonetta haittaohjelmien toiminnan suorittamalla tarvitsematta sovelluksen muutokset reaaliaikainen salausta ja salauksen tietokannan, liitetty varmuuskopiot ja tapahtuman lokitiedostojen rest-palvelussa.

TDE salaa koko tietokannan tallennustilaan kutsutaan tietokannan salausavaimen symmetrisen avaimen avulla. Tietokannan salausavaimen on suojattu valmiin palvelinvarmennetta. Valmiin palvelinvarmennetta on yksilöllinen kullekin Azure palvelimelle. Microsoft kiertää nämä varmenteet automaattisesti vähintään kerran 90 päivää. Katso yleiskuvaus TDE, [Läpinäkyvä tietojen salaus (TDE)].

##<a name="enabling-encryption"></a>Salauksen ottaminen käyttöön

Ottaa käyttöön Azure TDE tietokantaan, joka on tallentaminen tiedot uuteen Venytys on otettu käyttöön SQL Server-tietokannasta, seuraavilla tavoilla:

1. Azure palvelimessa, jossa tietokantaan, joka on järjestelmänvalvojan tai perusmuodon tietokannan **dbmanager** -roolin jäsen kirjautumisen avulla *pää* -tietokantayhteyden muodostamisessa
2. Suorittaa seuraavan lauseen tietokannan salaaminen.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION ON;
```

##<a name="disabling-encryption"></a>Salauksen poistaminen käytöstä

TDE käytöstä Azure tietokannassa, jossa on tallentaminen tiedot uuteen Venytys on otettu käyttöön SQL Server-tietokannasta, seuraavilla tavoilla:

1. Kirjautuminen järjestelmänvalvojana tai perusmuodon tietokannan **dbmanager** -roolin jäsen eli käyttämällä *perusmuodon* tietokantayhteyden muodostamisessa
2. Suorittaa seuraavan lauseen tietokannan salaaminen.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION OFF;
```

##<a name="verifying-encryption"></a>Tarkistetaan salaus

Voit tarkistaa salauksen tila Azure-tietokannan, joka on tallentaminen tiedot uuteen Venytys on otettu käyttöön SQL Server-tietokannasta, tee seuraavat kohdat:

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


<!--Anchors-->
[Läpinäkyvä salauksen (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->

<!--Link references-->
