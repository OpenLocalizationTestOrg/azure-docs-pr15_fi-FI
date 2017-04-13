<properties
   pageTitle="Tietojen lataaminen SQL Server Azure SQL-tietovarasto (bcp) | Microsoft Azure"
   description="Pieni tietojen koko, joko käyttää bcp tietojen vieminen tasainen tiedostot SQL Serverin ja tuoda tiedot suoraan Azure SQL-tietovarasto."
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
   ms.date="06/30/2016"
   ms.author="lodipalm;barbkess;sonyama"/>


# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-flat-files"></a>Tietojen lataaminen SQL Server Azure SQL-tietovarasto (tasainen tiedostot)

> [AZURE.SELECTOR]
- [SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
- [PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md)
- [BCP](sql-data-warehouse-load-from-sql-server-with-bcp.md)

For small tietojoukot käyttää bcp komentorivin apuohjelma SQL Server-tietojen vieminen ja lataa se suoraan Azure SQL-tietovarasto avulla.

Tässä opetusohjelmassa käytetään bcp avulla:

- Taulukon vieminen SQL Serverin avulla bcp-komennon (tai luoda yksinkertaisen mallitiedosto)
- Tuominen tietuetiedostoon taulukon SQL-tietovarasto.
- Luo ladattu tietojen tilastot.

>[AZURE.VIDEO loading-data-into-azure-sql-data-warehouse-with-bcp]

## <a name="before-you-begin"></a>Ennen aloittamista

### <a name="prerequisites"></a>Edellytykset

Jos haluat askel Tässä opetusohjelmassa on:

- SQL-tietovarasto-tietokanta
- Asennettu bcp komentorivin apuohjelma
- Asennettu sqlcmd komentorivin apuohjelma

Voit ladata bcp ja sqlcmd apuohjelmia [Microsoft Download Centeristä][].

### <a name="data-in-ascii-or-utf-16-format"></a>Tietojen ASCII- tai UTF-16

Jos yrität tässä opetusohjelmassa omiin tietoihisi, tiedot on käyttää ASCII- tai UTF-16 koodaus jälkeen bcp ei tue UTF-8. 

PolyBase tukee UTF-8, mutta se ei ole vielä tue UTF-16. Huomaa, että jos haluat yhdistää bcp PolyBase haluat muuntaa UTF-8 tiedot, kun se on viety SQL Serveristä. 


## <a name="1-create-a-destination-table"></a>1. kohdetaulukon luominen

Määritä SQL-tietovarasto, jotka ovat kuormituksen kohdetaulukko taulukon. Taulukon sarakkeiden on vastattava datatiedoston kunkin rivin tiedot.

Luo taulukko, Avaa komentorivi-ikkuna ja sqlcmd.exe avulla suorittamalla seuraavan komennon:


```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    WITH
    (
        CLUSTERED COLUMNSTORE INDEX,
        DISTRIBUTION = ROUND_ROBIN
    );
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

Tällä komennolla voit tarkistaa tiedot on ladattu oikein

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

## <a name="4-create-statistics"></a>4. luominen tilastotiedot

SQL-tietovarasto ei vielä tuki luominen automaattisesti tai automaattinen päivitys tilastotiedot. Saat parhaan suorituskyvyn kysely-on tärkeää luo tilastotietoja kaikkien taulukoiden kaikkien sarakkeiden ensimmäinen kuormituksen tai kun merkittäviä muutoksia esiintyvät tiedot. Katso tilastojen tarkan [tilastotiedot][]. 

Suorita seuraava komento ja luo tilastotietoja ladatut taulukossa.

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="5-export-data-from-sql-data-warehouse"></a>5. SQL-tietovarasto tietojen viennin
Hauskaa voit viedä tiedot, jotka lataamasi vain takaisin SQL-tietovarasto ulos.  Vie-komento on täysin sama kuin vieminen SQL Serveristä.

On kuitenkin tulokset ero. Koska tiedot tallennetaan SQL-tietovarasto, eri aikajaksoille paikkoihin, kun viet tietoja Laske kunkin solmun kirjoittaa sen tietoja kohdetiedosto. Kohdetiedosto tietojen järjestystä todennäköisesti olla sama kuin lähdetiedoston tietojen järjestystä.

### <a name="export-a-table-and-compare-exported-results"></a>Vie taulukko ja verrata viedyn tuloksia

Saat näkyviin viedyille tiedoille, Avaa komentorivi ja suorita tämä komento oman parametreilla. Palvelimen nimi on looginen Azure SQL-palvelimen nimi.

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
Voit tarkistaa tietoja viety oikein avaamalla uusi tiedosto. Tiedoston tietojen pitäisi täsmätä alla olevaa tekstiä, mutta todennäköisesti lajitellaan eri järjestyksessä:

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

### <a name="export-the-results-of-a-query"></a>Vie kyselyn tulokset

Voit viedä sen sijaan, että koko taulukon vieminen kyselyn tulosten bcp **queryout** -funktiota. 

## <a name="next-steps"></a>Seuraavat vaiheet
Saat yleiskatsauksen lataaminen [kuormituksen tietojen tuominen SQL-tietovarasto][].
Artikkelissa kehitys Lisävihjeitä [SQL-tietovarasto kehittäminen yleiskatsaus][].
Katso lisätietoja taulukon luomisesta SQL-tietovarasto [Taulukon yleistä][] tai [Luo taulukko syntaksi][] .

<!--Image references-->

<!--Article references-->

[Lataa tiedot SQL-tietovarasto]: ./sql-data-warehouse-overview-load.md
[SQL-tietovarasto kehittäminen yleiskatsaus]: ./sql-data-warehouse-overview-develop.md
[Taulukon yleiskatsaus]: ./sql-data-warehouse-tables-overview.md
[Tilastotiedot]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[Luo taulukko syntaksi]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft Download Centeristä]: https://www.microsoft.com/download/details.aspx?id=36433
