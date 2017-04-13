<properties
   pageTitle="Lataa tiedot SQL-tietovarasto bcp avulla | Microsoft Azure"
   description="Katso, mitä bcp on ja miten sitä käytetään tietovaraston skenaariot."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="twounder"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/10/2016"
   ms.author="mausher;barbkess;sonyama"/>


# <a name="load-data-with-bcp"></a>Lataa bcp tietojasi

> [AZURE.SELECTOR]
- [Redgate](sql-data-warehouse-load-with-redgate.md)  
- [Tietoja Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
- [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)  
- [BCP](sql-data-warehouse-load-with-bcp.md)


**[BCP][]** on komentorivin joukkona Lataa apuohjelma, jonka avulla voit kopioida tietoja SQL Server, datatiedostoista ja SQL-tietovarasto välillä. Avulla bcp, voit tuoda taulukoita SQL-tietovarasto paljon rivejä tai tietojen tuominen datatiedostojen SQL Server-taulukoihin. Kun käyttänyt queryout-vaihtoehdon, paitsi bcp edellyttää ei Transact-SQL.

BCP on nopea ja helppo tapa siirtyä pienempi tietojoukot sisään ja ulos tietovarasto SQL-tietokanta. Tarkka tietomäärän, on suositeltavaa kuormituksen/Pura kautta bcp riippuu edelleen Azure tietokeskuksen yhteys verkkoon.  Yleensä dimension taulukoita voi ladata ja uuttaa helposti bcp kanssa, kuitenkin bcp ei ole suositeltavaa ladataan tai puretaan suurista tietomääristä.  Polybase on suositeltu työkalu ladataan ja purkaminen suurista tietomääristä kuin se tekee paremmin työn hyödyntäminen SQL-Tietovarasto erittäin rinnakkain käsittely-arkkitehtuuri.

Bcp avulla voit:

- Yksinkertainen komentorivin apuohjelman avulla voit ladata tietoja SQL-tietovarasto.
- Poimi tiedot SQL-tietovarasto yksinkertainen komentorivin apuohjelma avulla.

Tässä opetusohjelmassa kerrotaan, miten voit:

- Tietojen tuominen-komennon käyttöä bcp taulukon
- Tietojen vieminen taulukon uisng bcp ulos-komento

>[AZURE.VIDEO loading-data-into-azure-sql-data-warehouse-with-bcp]

## <a name="prerequisites"></a>Edellytykset

Jos haluat askel Tässä opetusohjelmassa on:

- SQL-tietovarasto-tietokanta
- Asennettu bcp komentoriviltä-apuohjelma
- Asennettu SQLCMD komentoriviltä-apuohjelma

>[AZURE.NOTE] Voit ladata bcp ja sqlcmd apuohjelmia [Microsoft Download Centeristä][].

## <a name="import-data-into-sql-data-warehouse"></a>Tietojen tuominen SQL-tietovarasto

Tässä opetusohjelmassa luodaan taulukon Azure SQL-tietovarasto ja tuo tiedot taulukkoon.

### <a name="step-1-create-a-table-in-azure-sql-data-warehouse"></a>Vaihe 1: Taulukon luominen SQL Azure-tietovarasto

Komentoriviltä suorittamalla seuraava kysely luoda taulukon omassa sqlcmd avulla:

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

>[AZURE.NOTE] Saat lisätietoja taulukon luomisesta SQL-tietovarasto ja vaihtoehdot WITH-lauseessa [Taulukon yleistä][] tai [CREATE TABLE-syntaksia][] .

### <a name="step-2-create-a-source-data-file"></a>Vaihe 2: Luo lähde-datatiedosto

Avaa Muistio ja kopioi seuraavat rivit tiedot uuteen tekstitiedostoon ja Tallenna tiedosto paikalliseen temp kansioon, C:\Temp\DimDate2.txt.

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

> [AZURE.NOTE] On tärkeää muistaa, että bcp.exe ei tue UTF-8-tiedosto-koodaus. Käytä ASCII-tiedostoja tai UTF-16 koodattu tiedostoja, bcp.exe käytettäessä.

### <a name="step-3-connect-and-import-the-data"></a>Vaihe 3: Yhteyden ja tuoda tiedot
Käytä bcp, voit muodostaa yhteyden ja tuoda tiedot käyttämällä korvaa arvot tarvittaessa seuraava komento:

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t  ','
```

Voit tarkistaa, tiedot on ladattu suorittamalla seuraava kysely sqlcmd avulla:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

Tämä on palauttavat seuraavat tulokset:

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

### <a name="step-4-create-statistics-on-your-newly-loaded-data"></a>Vaihe 4: Luo tilastotiedot ladatut tiedot

Azure SQL-tietovarasto ei vielä tuki automaattinen luominen tai automaattisen päivityksen tilastotiedot. Jotta saat parhaan suorituskyvyn-kyselyissä, on tärkeää, että tilastotiedot luodaan kaikkien taulukoiden kaikkien sarakkeiden jälkeen ensimmäisen kuormituksen tai merkittäviä muutoksia esiintyvät tiedot. Yksityiskohtainen kuvaus tilasto on aiheessa [tilastotiedot][] aiheista kehittäminen-ryhmässä. Alla on lyhyt Esimerkki tilastoja taulukon luomisesta ladattu tässä esimerkissä

Suorittaa seuraavat luominen tilastotiedot lauseet sqlcmd kehote:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="export-data-from-sql-data-warehouse"></a>Tietojen tuominen SQL-tietovarasto
Tässä opetusohjelmassa luodaan datatiedoston SQL-tietovarasto-taulukosta. Vie tiedot edellä luomaasi uusi datatiedostona DimDate2_export.txt.

### <a name="step-1-export-the-data"></a>Vaihe 1: Vie tiedot

Bcp-apuohjelman avulla voit yhdistää ja vie tiedot käyttämällä korvaa arvot tarvittaessa seuraava komento:

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
Voit tarkistaa tietoja viety oikein avaamalla uusi tiedosto. Tiedoston tietojen pitäisi täsmätä seuraava teksti:

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

>[AZURE.NOTE] Hajautettu järjestelmien laatu vuoksi tietojen eivät välttämättä ole sama yli tietovarasto SQL-tietokannat. Toinen vaihtoehto on bcp **queryout** -funktion avulla voit kirjoittaa kyselyn Pura sen sijaan, että vie koko taulukon.

## <a name="next-steps"></a>Seuraavat vaiheet
Saat yleiskatsauksen lataaminen [kuormituksen tietojen tuominen SQL-tietovarasto][].
Artikkelissa kehitys Lisävihjeitä [SQL-tietovarasto kehittäminen yleiskatsaus][].

<!--Image references-->

<!--Article references-->

[Lataa tiedot SQL-tietovarasto]: ./sql-data-warehouse-overview-load.md
[SQL-tietovarasto kehittäminen yleiskatsaus]: ./sql-data-warehouse-overview-develop.md
[Taulukon yleiskatsaus]: ./sql-data-warehouse-tables-overview.md
[Tilastotiedot]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[BCP]: https://msdn.microsoft.com/library/ms162802.aspx
[Luo taulukko syntaksi]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft Download Centeristä]: https://www.microsoft.com/download/details.aspx?id=36433
