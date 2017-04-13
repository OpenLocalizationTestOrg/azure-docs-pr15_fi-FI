<properties
   pageTitle="Siirtää oman aiemmin Azure SQL-tietovarasto premium tallennustilan | Microsoft Azure"
   description="SQL-tietovarasto premium tallennustilan siirtämistä koskevia ohjeita"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="happynicolle"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/24/2016"
   ms.author="nicw;barbkess;sonyama"/>

# <a name="migration-to-premium-storage-details"></a>Siirron Premium tallennustilan tiedot
SQL-tietovarasto käyttöön viimeksi [Suurempi suorituskyvyn ennustettavuutta tallennustila Premium][].  On nyt tapoja siirtää olemassa olevat tiedot varastoja tällä hetkellä-standardin tallennustilan Premium-tallennustilan.  Lukea lisätietoja siitä, miten ja milloin automaattinen siirrot ilmenee ja siirtäminen itse, jos haluat määrittää, käyttökatkot ajankohdan.

Jos sinulla on useampi kuin yksi tietovarasto, se siirretään myös voit tarkistaa [automaattisen siirron aikataulun][] alla avulla.

## <a name="determine-storage-type"></a>Määrittää tallennustyyppi
Jos olet luonut DW ennen alla päivämääriä, vakio tallennustila on käytössä.  Kunkin tietovarasto vakio säilössä eli automaattinen siirto on ilmoitus tietovarasto sivu yläreunassa [Azure-portaalissa][] , jossa lukee "premium tallennustilan päivityksen*tulevista edellyttävät käyttökatkosta.  Lue lisää ->*. "

| **Alue**          | **DW, jotka on luotu ennen tätä päivää**   |
| :------------------ | :-------------------------------- |
| Australia Itä      | Premium tallennustila ei ole vielä käytettävissä |
| Australia varaaja | 5 elokuussa 2016                    |
| Brasilia Etelä        | 5 elokuussa 2016                    |
| Kanada Keski      | 2016 25 toukokuu                      |
| Kanada Itä         | 26 toukokuu 2016                      |
| Keskitetyn USA          | 26 toukokuu 2016                      |
| Kiinan Itä          | Premium tallennustila ei ole vielä käytettävissä |
| Kiinan Pohjois         | Premium tallennustila ei ole vielä käytettävissä |
| Itä-Aasian           | 2016 25 toukokuu                      |
| Yhdysvaltojen Itä             | 26 toukokuu 2016                      |
| Itä US2            | 27 toukokuu 2016                      |
| Intia Keski       | 27 toukokuu 2016                      |
| Intia Etelä         | 26 toukokuu 2016                      |
| Intia Länsi          | Premium tallennustila ei ole vielä käytettävissä |
| Japani Itä          | 5 elokuussa 2016                    |
| Japani Länsi          | Premium tallennustila ei ole vielä käytettävissä |
| Pohjois-keskitetyn USA    | Premium tallennustila ei ole vielä käytettävissä |
| Pohjois-Eurooppa        | 5 elokuussa 2016                    |
| Etelä keskitetyn USA    | 27 toukokuu 2016                      |
| Kaakkoisaasialaiset Aasian      | 2016 24 toukokuu                      |
| Länsi Europe         | 2016 25 toukokuu                      |
| Länsi keskitetyn USA     | 26 elokuussa 2016                   |
| Länsi USA             | 26 toukokuu 2016                      |
| Länsi US2            | 26 elokuussa 2016                   |

## <a name="automatic-migration-details"></a>Automaattinen siirron tiedot
Oletusarvon mukaan on siirtää tietokannan puolestasi aikana 6 pm ja 6: 00-alueen ajaksi aikana [automaattisen siirron aikataulun][] .  Olemassa olevan tietovarasto on käyttökelvoton siirron aikana.  Olemme arvioida, että siirron kestää noin tunnin TT tallennustilaa kohti tietovarasto kohden.  On myös varmistaa, että sinun ei peri jääviä automaattisen siirron aikana.

> [AZURE.NOTE] Ei voi käyttää aiemmin luotuja tietovarasto siirron aikana.  Kun siirto on valmis, Data Warehouse ovat online-tilaan.

Tietoja alla on Microsoftin kestää puolestasi siirron suorittamiseen ja ei edellytä mitään osallistuminen osaltasi vaihetta.  Tässä esimerkissä Kuvitellaan, että aiemmin DW vakio säilössä tällä hetkellä nimeltä "MyDW."

1.  Microsoft nimeää "MyDW", "MyDW_DO_NOT_USE_ [aikaleima]"
2.  Microsoft keskeyttää "MyDW_DO_NOT_USE_ [aikaleima]."  Tänä aikana otetaan varmuuskopion.  Voit nähdä useita Keskeytä/ansioluettelot, jos olemme ilmenee ongelmia tämän prosessin aikana.
3.  Microsoft Luo uusi DW, nimeltä "MyDW" Premium tallennustilan varmuuskopiosta vaiheessa 2.  "MyDW" ei tule vasta, kun palautus on valmis.
4.  Kun palautus on valmis, "MyDW" palauttaa saman DWUs ja keskeytetty tai aktiivinen tilaan ennen siirtoa.
5.  Kun siirto on valmis, Microsoft poistaa "MyDW_DO_NOT_USE_ [aikaleima]"
    
> [AZURE.NOTE] Nämä asetukset eivät siirtää siirron osana:
> 
>   -  Tietokannan tasolla tarkistaminen on oltava uudelleen käyttöön
>   -  Palomuurisäännöt **tietokannan** tasolla täytyy olla readded.  Palomuurisäännöt **palvelimen** tasolla ei voi vaikuttaa.

### <a name="automatic-migration-schedule"></a>Automaattinen siirron aikataulu
Automaattinen siirrot ilmetä-6 pm – 6: 00 (paikallinen aika alueittain) aikana seuraavan käyttökatkosta aikataulun mukaisesti.

| **Alue**          | **Arvioitu alkamispäivä**     | **Arvioitu päättymispäivä**       |
| :------------------ | :--------------------------- | :--------------------------- |
| Australia Itä      | Määritetään ei ole vielä           | Määritetään ei ole vielä           |
| Australia varaaja | 10 elokuussa 2016              | 24 elokuussa 2016              |
| Brasilia Etelä        | 10 elokuussa 2016              | 24 elokuussa 2016              |
| Kanada Keski      | 2016 23 kesäkuussa                | 1 heinäkuussa 2016                 |
| Kanada Itä         | 2016 23 kesäkuussa                | 1 heinäkuussa 2016                 |
| Keskitetyn USA          | 2016 23 kesäkuussa                | 4 heinäkuussa 2016                 |
| Kiinan Itä          | Määritetään ei ole vielä           | Määritetään ei ole vielä           |
| Kiinan Pohjois         | Määritetään ei ole vielä           | Määritetään ei ole vielä           |
| Itä-Aasian           | 2016 23 kesäkuussa                | 1 heinäkuussa 2016                 |
| Yhdysvaltojen Itä             | 2016 23 kesäkuussa                | 11 heinäkuussa 2016                |
| Itä US2            | 2016 23 kesäkuussa                | 8 heinäkuussa-2016                 |
| Intia Keski       | 2016 23 kesäkuussa                | 1 heinäkuussa 2016                 |
| Intia Etelä         | 2016 23 kesäkuussa                | 1 heinäkuussa 2016                 |
| Intia Länsi          | Määritetään ei ole vielä           | Määritetään ei ole vielä           |
| Japani Itä          | 10 elokuussa 2016              | 24 elokuussa 2016              |
| Japani Länsi          | Määritetään ei ole vielä           | Määritetään ei ole vielä           |
| Pohjois-keskitetyn USA    | Määritetään ei ole vielä           | Määritetään ei ole vielä           |
| Pohjois-Eurooppa        | 10 elokuussa 2016              | 31 elokuussa 2016              |
| Etelä keskitetyn USA    | 2016 23 kesäkuussa                | 2 heinäkuussa 2016                 |
| Kaakkoisaasialaiset Aasian      | 2016 23 kesäkuussa                | 1 heinäkuussa 2016                 |
| Länsi Europe         | 2016 23 kesäkuussa                | 8 heinäkuussa-2016                 |
| Länsi keskitetyn USA     | 14 elokuussa 2016              | 31 elokuussa 2016              |
| Länsi USA             | 2016 23 kesäkuussa                | 7 heinäkuussa 2016                 |
| Länsi US2            | 14 elokuussa 2016              | 31 elokuussa 2016              |

## <a name="self-migration-to-premium-storage"></a>Itsenäisen siirron Premium säilöön
Jos haluat määrittää, kun yhteyttä käyttökatkot ilmenee, voit siirtää tietovarasto vakio säilössä Premium tallennustilan seuraavasti.  Jos haluat siirtää itse, sinun on suoritettava itsenäisen siirron, ennen kuin automaattinen siirron alkaa alueella olevat välttämiseksi Ristiriidan aiheuttavat automaattisen siirron (Lisätietoja [automaattisen siirron aikataulu][]).

### <a name="self-migration-instructions"></a>Itsenäisen siirron ohjeita
Jos haluat määrittää, että käyttökatkot, voit siirtää Data Warehouse itse käyttämällä varmuuskopiointi ja palauttaminen.  Siirron palauttaminen-osa on tarkoitus kestää noin tunnin TT tallennustilaa kohti DW kohden.  Jos haluat säilyttää alkuperäisen nimen, kun siirto on valmis, ohjeiden, [uudelleennimeäminen siirron aikana][]. 

1.  [Pidä][] yhteyttä DW, joka kestää automaattisen varmuuskopioinnin
2.  Viimeisin tilannevedoksen [palauttaminen][]
3.  Poista oman aiemmin DW vakio säilössä. **Jos et tee tämä vaihe, jonka veloitetaanko sekä DWs.**

> [AZURE.NOTE] Nämä asetukset eivät siirtää siirron osana:
> 
>   -  Tietokannan tasolla tarkistaminen on oltava uudelleen käyttöön
>   -  Palomuurisäännöt **tietokannan** tasolla täytyy olla readded.  Palomuurisäännöt **palvelimen** tasolla ei voi vaikuttaa.

#### <a name="optional-steps-to-rename-during-migration"></a>Valinnainen: siirron aikana uudelleennimeämisen vaiheet 
Tietokannoista samaan looginen palvelimeen ei voi olla samaa nimeä. SQL-tietovarasto tukee nyt voi nimetä uudelleen DW.

Tässä esimerkissä Kuvitellaan, että aiemmin DW vakio säilössä tällä hetkellä nimeltä "MyDW."

1.  Nimeä "MyDW" komennolla muuta TIETOKANTAA, että jokin seuraavasti, kuten "MyDW_BeforeMigration."  Tämä komento keskeyttää kaikki olemassa olevat tapahtumat, ja se on tehtävä perusmuodon tietokannan onnistuu.
```
ALTER DATABASE CurrentDatabasename MODIFY NAME = NewDatabaseName;
```
2.  [Keskeytä][] "MyDW_BeforeMigration" joka kestää automaattisen varmuuskopioinnin
3.  [Palauttaa][] uusimmasta tilannevedoksen uuden tietokannan nimeä, jolla on kanssa (ex: "MyDW")
4.  Poista "MyDW_BeforeMigration".  **Jos et tee tämä vaihe, jonka veloitetaanko sekä DWs.**

> [AZURE.NOTE] Nämä asetukset eivät siirtää siirron osana:
> 
>   -  Tietokannan tasolla tarkistaminen on oltava uudelleen käyttöön
>   -  Palomuurisäännöt **tietokannan** tasolla täytyy olla readded.  Palomuurisäännöt **palvelimen** tasolla ei voi vaikuttaa.

## <a name="next-steps"></a>Seuraavat vaiheet
Premium tallennustilan muuttamaan on on lisätty myös tietokantatiedostot Data Warehouse pohjana arkkitehtuuri Blob-objektien määrän.  Suurenna muutoksen suorituskyvyn edut, on suositeltavaa liitetty Columnstore indeksejä käyttämällä seuraavaa komentosarjaa Muodosta uudelleen.  Alla olevaa komentosarjaa toimii pakottamalla aiemmin soluihin muita BLOB-objektit.  Jos ei toimenpiteitä tiedot levittää luonnollisesti ajan kuluessa kuin tietovarasto taulukoiksi Lataa lisää tietoja.

**Vaatimukset:**

1.  Tietovarasto sisältävät toimivat 1000 DWUs tai uudempi versio (katso [asteikko Laske power][])
2.  Komentosarjan suorittavan käyttäjän on oltava [mediumrc roolin][] tai uudempi versio
    1.  Voit lisätä rooliin käyttäjän, suorita seuraavat: 
        1.  ````EXEC sp_addrolemember 'xlargerc', 'MyUser'````

````sql
-------------------------------------------------------------------------------
-- Step 1: Create Table to control Index Rebuild
-- Run as user in mediumrc or higher
--------------------------------------------------------------------------------
create table sql_statements
WITH (distribution = round_robin)
as select 
    'alter index all on ' + s.name + '.' + t.NAME + ' rebuild;' as statement,
    row_number() over (order by s.name, t.name) as sequence
from 
    sys.schemas s
    inner join sys.tables t
        on s.schema_id = t.schema_id
where
    is_external = 0
;
go
 
--------------------------------------------------------------------------------
-- Step 2: Execute Index Rebuilds.  If script fails, the below can be rerun to restart where last left off
-- Run as user in mediumrc or higher
--------------------------------------------------------------------------------

declare @nbr_statements int = (select count(*) from sql_statements)
declare @i int = 1
while(@i <= @nbr_statements)
begin
      declare @statement nvarchar(1000)= (select statement from sql_statements where sequence = @i)
      print cast(getdate() as nvarchar(1000)) + ' Executing... ' + @statement
      exec (@statement)
      delete from sql_statements where sequence = @i
      set @i += 1
end;
go
-------------------------------------------------------------------------------
-- Step 3: Cleanup Table Created in Step 1
--------------------------------------------------------------------------------
drop table sql_statements;
go
````

Jos käytössä ilmenee ongelmat tietovarasto, [Luo tuki lippu][] ja viittaus "Siirron Premium-tallennustilan" kuin mahdollinen syy.

<!--Image references-->

<!--Article references-->
[Automaattinen siirron aikataulu]: #automatic-migration-schedule
[self-migration to Premium Storage]: #self-migration-to-premium-storage
[Luo tuki-lippu]: sql-data-warehouse-get-started-create-support-ticket.md
[Azure paired region]: best-practices-availability-paired-regions.md
[main documentation site]: services/sql-data-warehouse.md
[Keskeytä]: sql-data-warehouse-manage-compute-portal.md/#pause-compute
[Palauttaminen]: sql-data-warehouse-restore-database-portal.md
[siirron aikana uudelleennimeäminen]: #optional-steps-to-rename-during-migration
[Skaalaa Laske power]: sql-data-warehouse-manage-compute-portal/#scale-compute-power
[mediumrc rooli]: sql-data-warehouse-develop-concurrency/#workload-management

<!--MSDN references-->


<!--Other Web references-->
[Suurempi suorituskyvyn ennustettavuutta Premium tallennustila]: https://azure.microsoft.com/en-us/blog/azure-sql-data-warehouse-introduces-premium-storage-for-greater-performance/
[Azure Portal]: https://portal.azure.com
