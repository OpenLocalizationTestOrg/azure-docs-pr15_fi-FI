<properties
   pageTitle="Kyselyn Azure SQL-tietovarasto (sqlcmd) | Microsoft Azure"
   description="Kysely Azure SQL-tietovarasto komentorivivalitsimet apuohjelman sqlcmd."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/06/2016"
   ms.author="barbkess;sonyama"/>

# <a name="query-azure-sql-data-warehouse-sqlcmd"></a>Kyselyn Azure SQL-tietovarasto (sqlcmd)

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure koneen oppiminen](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [SQLCMD](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Tätä vaiheittaista kyselyn Azure SQL-tietovarasto [sqlcmd][] komentorivin-apuohjelman avulla.  

## <a name="1-connect"></a>1. yhteyden

Aloita [sqlcmd][], Avaa komentokehote ja kirjoita **sqlcmd** SQL-tietovarasto tietokannan yhteysmerkkijonon perään. Yhteysmerkkijonon edellyttää seuraavia parametreja:

+ **Server (-S):** Palvelimen lomakkeeseen `<`palvelimen nimi`>`. database.windows.net
+ **Tietokannan (-d):** Tietokannan nimi.
+ **Ota lainausmerkeissä tunnukset (-voin):** Tarjottu tunnisteet on otettava käyttöön SQL-tietovarasto esiintymään.

Voit käyttää SQL Server-todennus, sinun on lisättävä käyttäjänimellä ja salasanalla parametrit:

+ **Käyttäjän (-U):** Palvelimen käyttäjän lomakkeessa `<`käyttäjä`>`
+ **Salasana (-P):** Liittyvä käyttäjän salasana.

Esimerkiksi yhteysmerkkijono saattaa näyttää seuraavalta:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
```

Jos haluat käyttää Azure Active Directory todennusta, sinun on lisättävä Azure Active Directory-parametrit:

+ **Azure Active Directory-todennus (-G):** käyttää Azure Active Directory käyttöoikeuksien

Esimerkiksi yhteysmerkkijono saattaa näyttää seuraavalta:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -G -I
```

> [AZURE.NOTE] Sinun on otettava [käyttöön Azure Active Directory käyttöoikeuksien](sql-data-warehouse-authentication.md) tarkistamiseen Active Directoryn avulla.

## <a name="2-query"></a>2. kyselyn

Yhteys, kun lähetät kaikki tuetut Transact-SQL-lauseet vastaan esiintymä.  Tässä esimerkissä kyselyt lähetetään vuorovaikutteisessa tilassa.

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

Seuraava näissä esimerkeissä näytetään, miten voit suorittaa Siirtoerän tila -Q-vaihtoehdon tai putkistot sqlcmd, että SQL kyselyissä.

```sql
sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I -Q "SELECT name FROM sys.tables;"
```

```sql
"SELECT name FROM sys.tables;" | sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I > .\tables.out
```

## <a name="next-steps"></a>Seuraavat vaiheet

[Sqlcmd ohjeissa][sqlcmd] saat lisää vaihtoehtoja sqlcmd tietoja.

<!--Image references-->

<!--Article references-->

<!--MSDN references--> 
[SQLCMD]: https://msdn.microsoft.com/library/ms162773.aspx
[Azure portal]: https://portal.azure.com

<!--Other Web references-->
