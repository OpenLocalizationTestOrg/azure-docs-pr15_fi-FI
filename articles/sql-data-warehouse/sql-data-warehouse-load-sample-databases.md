<properties
   pageTitle="Lataa mallitiedot SQL-tietovarasto | Microsoft Azure"
   description="Lataa mallitiedot SQL-tietovarasto"
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
   ms.date="08/16/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

#<a name="load-sample-data-into-sql-data-warehouse"></a>Lataa mallitiedot SQL-tietovarasto

Toimi seuraavasti lataamisen ja kyselyn Adventure Works-mallitietokantaa. Nämä komentosarjat suorittaa sqlcmd ensin SQL, joka luo taulukoiden ja näkymien avulla. Kun taulukot on luotu, komentosarjojen käyttää bcp lataa tiedot.  Jos sinulla ei ole vielä sqlcmd ja bcp asennettuna, noudata näitä linkkejä [bcp asennus][] ja [Asenna sqlcmd][].

##<a name="load-sample-data"></a>Lataa mallitiedot

1. Lataa [SQL-tietovarasto Adventure Works-komentosarjamallit][] zip-tiedoston.

2. Pura tiedostot ladattu zip paikallisessa tietokoneessa kansioon.

3. Muokkaa purettu tiedosto aw_create.bat ja määritä seuraavat muuttujat löydy tiedoston yläosassa.  Varmista, että jätä välillä ei ole välilyöntejä "=" ja parametri.  Seuraavassa on esimerkkejä siitä, miten lopputulokseen voi näyttää.

    ```
    server=mylogicalserver.database.windows.net
    user=mydwuser
    password=Mydwpassw0rd
    database=mydwdatabase
    ```

4. Suorita Windows cmd-kehote muokatun aw_create.bat.  Muista, olet tallentamasi aw_create.bat muokkaamasi version hakemistossa.
Tämä komentosarja on...
    * Pudota Adventure Works-taulukoita tai näkymiä, jotka ovat jo tietokannassa
    * Adventure Works-taulukoiden ja näkymien luominen
    * Lataa kunkin Adventure Works-taulukkoa, jossa bcp
    * Tarkista kunkin Adventure Works-taulukon rivimäärän
    * Kerää tilastotiedot jokaisessa sarakkeessa Adventure Works kullekin taulukolle


##<a name="query-sample-data"></a>Kyselyn mallitiedot

Kun mallitietoja on ladattu SQL-tietovarasto, voit suorittaa muutama kysely nopeasti.  Kyselyn suorittaminen Muodosta yhteys juuri luomasi Adventure Works-tietokantaan Azure SQL-DW Visual Studio ja SSDT-kohdassa kuvatulla tavalla [kyselyn Visual Studiossa][] asiakirjan.

Esimerkki yksinkertaisen select-lauseen saat kaikki työntekijöiden tiedot:

```sql
SELECT * FROM DimEmployee;
```

Monimutkaisen rakenteita, kuten GROUP BY avulla voit tarkastella myynnin kokonaissumma jokaisen päivän kyselyn Esimerkki:

```sql
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

Valitse ja WHERE-lauseen suodattamaan tilauksia ennen tiettyä päivämäärää Esimerkki:

```
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
WHERE OrderDateKey > '20020801'
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

SQL-tietovarasto tukee lähes kaikissa T-SQL-rakenteita, joka tukee SQL Server.  Sekä [siirtää koodin][] ohjeet on kuvattu eroja.

## <a name="next-steps"></a>Seuraavat vaiheet
Nyt kun olet ollut voit kokeilla jotkin kyselyt esimerkkitiedoilla, tarkista, kuinka voit [kehittää][], [ladata][]tai [siirtää][] SQL-tietovarasto.

<!--Image references-->

<!--Article references-->
[siirtää]: sql-data-warehouse-overview-migrate.md
[kehittäminen]: sql-data-warehouse-overview-develop.md
[lataaminen]: sql-data-warehouse-overview-load.md
[kysely, joka sisältää Visual Studio]: sql-data-warehouse-query-visual-studio.md
[siirtää koodi]: sql-data-warehouse-migrate-code.md
[Asenna bcp]: sql-data-warehouse-load-with-bcp.md
[Asenna sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--Other Web references-->
[Adventure Works komentosarjamallit SQL-tietovarasto varten]: https://migrhoststorage.blob.core.windows.net/sqldwsample/AdventureWorksSQLDW2012.zip
