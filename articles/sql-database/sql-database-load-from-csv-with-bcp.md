<properties
   pageTitle="Tietojen lataaminen CSV-tiedoston Azure SQL-Databaase (bcp) | Microsoft Azure"
   description="Pieni tietojen koko, joko käyttää bcp tuomiseen Azure SQL-tietokantaan."
   services="sql-database"
   documentationCenter="NA"
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/13/2016"
   ms.author="carlrab"/>


# <a name="load-data-from-csv-into-azure-sql-data-warehouse-flat-files"></a>Tietojen lataaminen CSV Azure SQL-tietovarasto (litteä tiedostot)

Komentorivin bcp-apuohjelman avulla voit tietojen tuominen CSV-tiedoston Azure SQL-tietokantaan.

## <a name="before-you-begin"></a>Ennen aloittamista

### <a name="prerequisites"></a>Edellytykset

Jos haluat askel Tässä opetusohjelmassa on:

- Looginen Azure SQL-tietokanta-palvelin ja tietokanta
- Asennettu bcp komentorivin apuohjelma
- Asennettu sqlcmd komentorivin apuohjelma

Voit ladata bcp ja sqlcmd apuohjelmia [Microsoft Download Centeristä][].

### <a name="data-in-ascii-or-utf-16-format"></a>Tietojen ASCII- tai UTF-16

Jos yrität tässä opetusohjelmassa omiin tietoihisi, tiedot on käyttää ASCII- tai UTF-16 koodaus jälkeen bcp ei tue UTF-8. 

## <a name="1-create-a-destination-table"></a>1. kohdetaulukon luominen

Määritä taulukon kohdetaulukko SQL-tietokantaan. Taulukon sarakkeiden on vastattava datatiedoston kunkin rivin tiedot.

Luo taulukko, Avaa komentorivi-ikkuna ja sqlcmd.exe avulla suorittamalla seuraavan komennon:


```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    ;
"
```


## <a name="2-create-a-source-data-file"></a>2. lähde-datatiedoston luominen

Avaa Muistio ja kopioi seuraavat rivit tiedot uuteen tekstitiedostoon ja Tallenna tiedosto paikalliseen temp kansioon, C:\Temp\DimDate2.txt. Nämä tiedot ovat ASCII-muodossa.

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

(Valinnainen) Omien tietojen tuominen SQL Server-tietokantaan, Avaa komentorivi-ikkuna ja suorittamalla seuraavan komennon. Taulukon nimi, palvelimen nimi, nimi, käyttäjänimi ja salasana korvaaminen omilla tiedoillasi.

```sql
bcp <TableName> out C:\Temp\DimDate2_export.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <Password> -q -c -t ','
```

## <a name="3-load-the-data"></a>3. Lataa tiedot
Voit ladata tietoja, Avaa komentorivi-ikkuna ja suorittamalla seuraavan komennon-arvojen korvaaminen palvelimen nimen, tietokannan nimi, käyttäjänimi ja salasana omilla tiedoillasi.

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <password> -q -c -t  ','
```

Tällä komennolla voit varmistaa, tiedot on ladattu oikein

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

Tulosten pitäisi näyttää tältä:

DateId |CalendarQuarter |FiscalQuarter
----------- |--------------- |-------------
20150101 |1 |3
20150201 |1 |3
20150301 |1 |3
20150401 |2 |4
20150501 |2 |4
20150601 |2 |4
20150701 |3 |1
20150801 |3 |1
20150801 |3 |1
20151001 |4 |2
20151101 |4 |2
20151201 |4 |2


## <a name="next-steps"></a>Seuraavat vaiheet

Siirtää SQL Server-tietokantaan, on artikkelissa [SQL Server-tietokannan siirto](sql-database-cloud-migrate.md).

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE syntax]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft Download Centeristä]: https://www.microsoft.com/download/details.aspx?id=36433
