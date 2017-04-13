<properties
   pageTitle="Indeksoinnin taulukot SQL-tietovarasto | Microsoft Azure"
   description="Taulukon Azure SQL-tietovarasto indeksoinnin aloittaminen."
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
   ms.date="07/12/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="indexing-tables-in-sql-data-warehouse"></a>SQL-tietovarasto indeksoinnin taulukot

> [AZURE.SELECTOR]
- [Yleiskatsaus][]
- [Tietotyypit][]
- [Jaa][]
- [Indeksi][]
- [Osion][]
- [Tilastotiedot][]
- [Tilapäinen][]

SQL-tietovarasto tarjoaa useita indeksoinnin asetuksia, kuten [liitetty columnstore indeksit][] [liitetty indeksejä tai indeksien löytyvät][].  Sen lisäksi myös on ei ole hakemisto-vaihtoehto tunnetaan myös nimellä [keon][].  Tässä artikkelissa käsitellään kunkin Indeksityyppi sekä vinkkejä useimmat mahdollisen hyödyn indeksejä käytön etuja. Katso siitä, miten voit luoda taulukon SQL-tietovarasto [luoda taulukon syntaksi][] tarkemmin.

## <a name="clustered-columnstore-indexes"></a>Liitetty columnstore indeksit

SQL-tietovarasto Luo indeksi on liitetty columnstore oletusarvoisesti, jos taulukkoa ei ole hakemistoasetukset on määritetty. Liitetty columnstore taulukoiden tarjoavat tietojen pakkaus ylimmän tason sekä parhaat yleistä kyselyn suorituskykyä.  Liitetty columnstore yleensä outperform liitetty indeksin tai keon taulukoita eikä ovat yleensä suurten taulukoiden paras vaihtoehto.  Seuraavien syiden takia liitetty columnstore on hyvä käynnistyy, kun et ole varma siitä, miten indeksoida taulukon.  

Liitetty columnstore taulukon luominen yksinkertaisesti määrittää LIITETTY COLUMNSTORE INDEKSIN WITH-lauseen ja jätä korvaava-lauseen:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED COLUMNSTORE INDEX );
```

On joitakin tilanteita, joissa liitetty columnstore ei ehkä hyvä vaihtoehto:

- Columnstore taulukot eivät tue toissijaisia ei ole liitetty indeksejä.  Harkitse keon tai ryhmitellyn indeksin taulukoiden sijaan.
- Columnstore taulukot eivät tue varchar(max) ja varbinary(max) nvarchar(max).  Harkitse keon tai ryhmitellyn indeksin sijaan.
- Columnstore taulukoita voi olla pienempi tehokas lyhytkestoisia tiedot.  Harkitse keon ja tallennettu väliaikaisia taulukoita.
- Pienempi kuin 100 miljoonaa rivejä on pieni taulukko.  Harkitse keon taulukot.

## <a name="heap-tables"></a>Keon taulukot

Kun tiedot ovat purkamisen tilapäisesti-SQL-tietovarasto, saatat huomata, että keon taulukoiden avulla saat yleinen nopeutuu.  Tämä johtuu siitä heaps latausten on nopeampaa kuin indeksi-taulukoihin ja joissakin tapauksissa myöhemmin luku voidaan toteuttaa välimuistista.  Jos haluat ladata tietoja vain vaiheen ennen kuin suoritat Lisää muunnoksia, ladataan keon taulukossa taulukko on paljon aiempaa nopeammin kuin liitetty columnstore taulukkoon tietojen lataaminen. Lisäksi ladataan [väliaikaisen taulukossa][tilapäinen] tietoja myös Lataa paljon nopeammin kuin ladataan taulukon muuntaminen pysyvä tallennuspaikka.  

Pieni haku taulukoissa ja pienempi kuin 100 miljoonaa rivien usein keon taulukoiden katsoo sen perustelluksi.  Klusterin columnstore taulukoiden alkaa saavuttamiseksi optimaalisen pakkaamisen jälkeen yli 100 miljoonaa rivit.

Keon taulukon luomiseen määrittämällä WITH-lauseessa KEON:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( HEAP );
```

## <a name="clustered-and-nonclustered-indexes"></a>Liitetty ja löytyvät indeksit

Liitetty indeksit saattaa outperform liitetty columnstore taulukot, kun noudetaan nopeasti yhdelle riville.  Kyselyjen missä yksittäinen tai muutamaa rivin haku on erittäin nopeus suorituskyvyn edellyttää harkitse klusterin indeksin tai löytyvät toissijainen indeksi.  Ryhmitellyn indeksin avulla haitta on vain kyselyjä, jolloin erittäin Valikoiva suodattimen käyttäminen liitetty indeksisarake hyötyä.  Voit parantaa suodattimen muiden sarakkeiden löytyvät indeksi voidaan lisätä muut sarakkeet.  Kuitenkin kunkin indeksiin, johon on lisätty taulukkoon Lisää välilyönti ja käsittelyn kesto Lataa.

Ryhmitellyn indeksin taulukon luomiseen määrittämällä WITH-lauseessa RYHMITELLYN INDEKSIN:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED INDEX (id) );
```

Voit lisätä ei ole liitetty indeksin taulukkoon, käytä seuraavaa syntaksia:

```SQL
CREATE INDEX zipCodeIndex ON t1 (zipCode);
```

> [AZURE.NOTE] Indeksin luominen ei ole liitetty luodaan oletusarvoisesti CREATE INDEX käytettäessä. Lisäksi ei ole liitetty indeksi on sallittu vain rivin tallennustilan taulukkoa (KEON tai INDEKSIN). Ei ole liitetty LIITETTY COLUMNSTORE indeksi päälle indeksejä ei ole sallittua tällä hetkellä.


## <a name="optimizing-clustered-columnstore-indexes"></a>Liitetty columnstore indeksit optimointi

Liitetty columnstore taulukot on järjestetty tietojen osia.  On hyvin segmentin laatu on ehdottoman tärkeää saavuttamiseksi optimaalisen kyselyn suorituskykyä columnstore taulukkoa.  Osion laatua voidaan mitata pakattu rivi-ryhmän rivien määrä.  Segmentin laatu on eniten optimaalisen, jossa on vähintään 100K rivien pakattu riviä kohden ryhmitellä ja saada suorituskyvyn kohti rivin ryhmän lähestymistapa rivien määrän 1 048 576 riviä, joka on rivin ryhmä voi sisältää useimmat rivit.

Alapuolella Näytä voi luoda ja käyttää järjestelmän keskimääräinen rivien riviä kohden ryhmitellä ja määritä optimaalisen klusterin columnstore indeksejä laskemiseen.  Tässä näkymässä viimeisen sarakkeen luo SQL-lause, jonka avulla voidaan muodostaa indeksejä uudelleen nimellä.

```sql
CREATE VIEW dbo.vColumnstoreDensity
AS
SELECT
        GETDATE()                                                               AS [execution_date]
,       DB_Name()                                                               AS [database_name]
,       s.name                                                                  AS [schema_name]
,       t.name                                                                  AS [table_name]
,   COUNT(DISTINCT rg.[partition_number])                   AS [table_partition_count]
,       SUM(rg.[total_rows])                                                    AS [row_count_total]
,       SUM(rg.[total_rows])/COUNT(DISTINCT rg.[distribution_id])               AS [row_count_per_distribution_MAX]
,   CEILING ((SUM(rg.[total_rows])*1.0/COUNT(DISTINCT rg.[distribution_id]))/1048576) AS [rowgroup_per_distribution_MAX]
,       SUM(CASE WHEN rg.[State] = 0 THEN 1                   ELSE 0    END)    AS [INVISIBLE_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE 0    END)    AS [INVISIBLE_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 1 THEN 1                   ELSE 0    END)    AS [OPEN_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE 0    END)    AS [OPEN_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 2 THEN 1                   ELSE 0    END)    AS [CLOSED_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE 0    END)    AS [CLOSED_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 3 THEN 1                   ELSE 0    END)    AS [COMPRESSED_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE 0    END)    AS [COMPRESSED_rowgroup_rows]
,       SUM(CASE WHEN rg.[State] = 3 THEN rg.[deleted_rows]   ELSE 0    END)    AS [COMPRESSED_rowgroup_rows_DELETED]
,       MIN(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_AVG]
,       'ALTER INDEX ALL ON ' + s.name + '.' + t.NAME + ' REBUILD;'             AS [Rebuild_Index_SQL]
FROM    sys.[pdw_nodes_column_store_row_groups] rg
JOIN    sys.[pdw_nodes_tables] nt                   ON  rg.[object_id]          = nt.[object_id]
                                                    AND rg.[pdw_node_id]        = nt.[pdw_node_id]
                                                    AND rg.[distribution_id]    = nt.[distribution_id]
JOIN    sys.[pdw_table_mappings] mp                 ON  nt.[name]               = mp.[physical_name]
JOIN    sys.[tables] t                              ON  mp.[object_id]          = t.[object_id]
JOIN    sys.[schemas] s                             ON t.[schema_id]            = s.[schema_id]
GROUP BY
        s.[name]
,       t.[name]
;
```

Nyt kun olet luonut näkymän, suorita tämä kysely selvittäminen taulukot rivin ryhmiin alle 100K rivien.  Voit halutessasi, niin, että 100K raja-arvon, jos hakua lisää optimaalisen segmentin laatua. 

```sql
SELECT  *
FROM    [dbo].[vColumnstoreDensity]
WHERE   COMPRESSED_rowgroup_rows_AVG < 100000
        OR INVISIBLE_rowgroup_rows_AVG < 100000
```

Kun suoritat kyselyn, voit aloittaa tietoja ja analysoida tulokset. Tässä taulukossa kerrotaan, mitä etsimään rivi-ryhmän analyysi.


| Sarakkeen                             | Tietojen käyttäminen                                                                                                                                                                      |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [table_partition_count]            | Jos taulukossa on osioitu, valitse saattaa haluamiasi suurempi Avaa rivin ryhmän laskee. Jakauman kunkin osion voi olla teoriassa liittyy Avaa rivi-ryhmä. Tämä ottaa huomioon analyysin kyselyjä. Pienen taulukon, jossa on osioitu voi optimoida poistamalla jakaminen kokonaan, kun tämä parantaa pakkaus.                                                                        |
| [row_count_total]                  | Taulukon summarivi määrä. Voit esimerkiksi laskea prosenttiosuuden rivit pakattu tilaan tämän arvon.                                                                      |
| [row_count_per_distribution_MAX]   | Jos kaikki rivit ovat tasaisesti arvo olisi kohteen rivimäärän jakauman kohden. Vertaa tätä arvoa compressed_rowgroup_count.                                 |
| [COMPRESSED_rowgroup_rows]         | Columnstore muodossa taulukon rivien kokonaismäärästä.                                                                                                                                 |
| [COMPRESSED_rowgroup_rows_AVG]     | Jos rivien määrän keskiarvo on huomattavasti pienempi kuin suurin # rivin ryhmän rivistä, arvioivat pakata tiedot CTAS tai muuta indeksi uudelleen avulla                     |
| [COMPRESSED_rowgroup_count]        | Rivin ryhmien columnstore muodossa määrä. Jos tämä arvo on erittäin suuri suhteessa taulukko on ilmaisin columnstore tiheyden on pieni.                                  |
| [COMPRESSED_rowgroup_rows_DELETED] | Rivit poistetaan loogisesti columnstore-muodossa. Jos luku on hyvin taulukon koon suhteessa, harkitse osion luotaessa tai indeksi muodostetaan uudelleen, kun tämä poistaa ne fyysisesti. |
| [COMPRESSED_rowgroup_rows_MIN]     | Tämän toiminnon avulla keskiarvo- ja MAX-sarakkeet yhdessä ymmärtää, että columnstore rivin ryhmien arvoalue. Vähän kuormituksen raja-arvon (102,400 osion tasattu jakauman kohden) kautta ehdottaa, että optimointi ovat käytettävissä tietojen lataus                                                                                                                                                 |
| [COMPRESSED_rowgroup_rows_MAX]     | Kuten edellä                                                                                                                                                                                  |
| [OPEN_rowgroup_count]              | Avaa rivin ryhmät ovat Normaali. Yksi todennäköisesti odottanut kohtuudella AVAA rivin ryhmä kohti taulukon jakauma (60). Ylimääräisen numerot Ehdota tietojen lataaminen osioiden välillä. Tarkista osioinnin strategia, varmista, että se on ääni                                                                                                                                                                                                |
| [OPEN_rowgroup_rows]               | Riviryhmässä voi olla 1 048 576 riviä ei enintään. Katso, miten koko Avaa riviryhmät ovat tällä hetkellä tätä arvoa käytetään                                                                 |
| [OPEN_rowgroup_rows_MIN]           | Avaa ryhmät osoittavat, että tiedot ovat trickle ladattavasta taulukkoon tai että edellisen kuormituksen spilled jäljellä olevien rivien rivin ryhmään. Käytä MIN-ja suurin ja AVG sarakkeet nähdäksesi, kuinka paljon tietoja on sat AVAA rivin ryhmät. Pieni taulukoiden ehkä 100 %: n kaikkien tietojen! Jolloin Muuta indeksi uudelleen tietojen pakottaminen columnstore.                                                                       |
| [OPEN_rowgroup_rows_MAX]           | Kuten edellä                                                                                                                                                                                  |
| [OPEN_rowgroup_rows_AVG]           | Kuten edellä                                                                                                                                                                                  |
| [CLOSED_rowgroup_rows]             | Tarkista suljettu rivin ryhmän rivejä kuin sanity-valintaruutu.                                                                                                                                       |
| [CLOSED_rowgroup_count]            | Suljettu rivin ryhmien määrä on oltava pieni, jos jokin näkyvät ollenkaan. Suljettu rivin ryhmät voidaan muuntaa pakattu rowg roups muuttaa INDEKSIN avulla... YHTEISÖN komento. Tämä ei kuitenkaan tavallisesti tehtävä. Suljetut ryhmät muunnetaan automaattisesti columnstore rivin ryhmiin taustan "monikon siirto-osoittimeksi"-prosessin.                                                                                               |
| [CLOSED_rowgroup_rows_MIN]         | Suljettu rivin ryhmien pitäisi olla erittäin suuri täytön korvaus. Jos suljettu rivi-ryhmän täyttö-korko on pieni, lisäanalyysiä, columnstore ei tarvita.                                   |
| [CLOSED_rowgroup_rows_MAX]         | Kuten edellä                                                                                                                                                                                  |
| [CLOSED_rowgroup_rows_AVG]         | Kuten edellä                                                                                                                                                                                  |
| [Rebuild_Index_SQL]         | SQL uudelleen taulukon columnstore indeksi                                                                                                                                                     |

## <a name="causes-of-poor-columnstore-index-quality"></a>Heikko columnstore indeksi laadun syitä

Jos olet määrittänyt taulukoiden ja laatu heikko osioon, haluat tunnistaa päämyöntäjien syy.  Seuraavassa on muita heikko segmentin quaility tavallisia syitä:

1. Kun indeksi on luotu muistinkäyttöön
2. Määrää Enimmäiskuolleisuusrajaa toiminnot
3. Pieni tai trickle kuormituksen toiminnot
4. Liian monta osiot

Nämä tekijät voivat aiheuttaa columnstore-indeksi on merkittävästi, joka on pienempi kuin optimaalisen 1 miljoona rivejä, rivin ryhmittäin.  Hän voi johtua rivien sijaan pakattu rivin ryhmän delta rivi-ryhmästä. 

### <a name="memory-pressure-when-index-was-built"></a>Kun indeksi on luotu muistinkäyttöön

Pakattu rivin ryhmittäin rivien määrä liittyvät suoraan rivi- ja muistin käsittelemiseen rivi-ryhmän leveys.  Kun rivejä on kirjoitettu columnstore taulukoiden muistin paineenalaisena, columnstore segmentin laatua voi kärsiä.  Tämän vuoksi paras käytäntö on antaa istunnon joka kirjoittaa columnstore indeksi taulukot access mahdollisimman paljon muistiin.  Koska vaikuttaa välillä muisti ja samanaikainen, oikea muistin varaamista ohjeita määräytyy kunkin rivin taulukon, olet varattu järjestelmään DWU summa ja määrä, voit tarkastella istuntoon, joka on kirjoittaminen taulukon samanaikainen paikkojen tiedot.  Paras käytäntö on suositeltavaa käynnistäminen xlargerc, jos käytössäsi on DW300 tai vähemmän largerc, jos käytössäsi on DW400 DW600 ja mediumrc, jos käytössäsi on DW1000 ja yläpuolella.

### <a name="high-volume-of-dml-operations"></a>Määrää Enimmäiskuolleisuusrajaa toiminnot

Suurta määrää Enimmäiskuolleisuusrajaa toiminnoista, joita päivittää ja poistaa rivejä voivat esitellä inefficiency columnstore kyselyjä. Tämä on erityisen TOSI, kun rivi-ryhmän rivien useimpia on muokattu.

- Rivin poistaminen pakattu rivin ryhmästä vain loogisesti merkitsee rivin poistetuiksi. Rivin pysyy pakattu rivi-ryhmässä, kunnes osio tai taulukon uudelleen.
- Rivin lisääminen Lisää olevan rivin niminen delta rivin ryhmän sisäinen rowstore-taulukko. Lisätylle riville muunnetaan ei columnstore, kunnes delta rivi-ryhmä on täynnä ja on merkitty suljettu. Rivin ryhmät on suljettu, kun ne saapuvat enintään 1 048 576 riviä kapasiteetti. 
- Rivin columnstore muodossa päivittäminen käsitellään looginen Poista ja sitten insert. Lisätylle riville voidaan tallentaa delta Storessa.

Erämuotoinen päivitys ja lisää toimintoja, jotka ylittävät 102,400 riviä kohden osion tasatut jakauman kirjoitetaan suoraan columnstore muotoon samalla kertaa. Kuitenkin jos tasainen jako, haluat voi muokata 6.144 miljoonaa rivien yhdellä kertaa tätä varten. Annetun osion rivien määrä tasattu kertymäfunktion arvo on pienempi kuin 102,400 rivit siirtyvät delta kauppa ja säilyvät siellä, kunnes riittävästi rivejä on lisätty tai muokata rivi-ryhmässä Sulje tai indeksi on uudelleen.

### <a name="small-or-trickle-load-operations"></a>Pieni tai trickle kuormituksen toiminnot

Pieni Lataa virtaus SQL-tietovarasto kutsutaan joskus myös kuin trickle Lataa. Ne vastaavat yleensä lähelle vakion stream parhaillaan ruuansulatukseen järjestelmän tiedot. Koska vuosta on lähellä jatkuva rivien määrä ei ole erityisen suuri. Tiedot on usein merkittävästi kohdassa columnstore muotoon suoraan lataamista varten tarvitaan raja-arvon.

Näissä tilanteissa on usein parempien Power View tiedot ensin Azure-blob-säiliö ja anna järjestelmän saavutustasopisteet ennen lataamista. Tätä tapaa kutsutaan usein *mikro jonottaminen*.

### <a name="too-many-partitions"></a>Liian monta osioihin

Huomioon otettavat toinen vaihtoehto on liitetty columnstore taulukkojen jakaminen vaikutus.  Ennen kuin jakaminen, SQL-tietovarasto jakaa jo tietojen 60 tietokantoihin.  Jakaminen edelleen jakaa tietoja.  Jos tietojen osioihin haluat huomioon, että **kunkin** osion on pystyttävä muodostamaan vähintään 1 miljoona rivien indeksi on liitetty columnstore hyötyvät.  Jos taulukon osion 100 osioihin, taulukon on vähintään 6 miljardia rivejä hyötyvät liitetty columnstore indeksi (60 jaot *100 osioiden* 1 miljoona rivejä). Jos 100 osio-taulukossa ei ole 6 miljardia rivejä, osioiden määrää tai kannattaa käyttää keon taulukon sijaan.

Kun taulukoissa on ladattu tietoja, noudata alapuolella tunnistaa ja taulukoita, joissa optimaalisen klusterin columnstore indeksejä uudelleen.

## <a name="rebuilding-indexes-to-improve-segment-quality"></a>Indeksit parantaa segmentin laatua muodostetaan uudelleen

### <a name="step-1-identify-or-create-user-which-uses-the-right-resource-class"></a>Vaihe 1: Määritä, tai Luo käyttäjän, joka käyttää oikean resurssiluokkaa

Yksi nopea tapa parantaa heti segmentin laatua on indeksin uudelleen.  Yllä näkymän palauttama SQL palauttaa on muuttaa indeksi uudelleen-komento, jonka avulla voidaan muodostaa indeksejä uudelleen.  Kun muodostetaan indeksejä uudelleen, muista varata muisti istuntoon, joka muodostaa indeksi.  Voit tehdä tämän suurentaa resurssiluokkaa käyttäjän, jolla on oikeudet tässä taulukossa suositellut vähän indeksin uudelleen.  Tietokannan omistajan käyttäjän resurssiluokkaa ei muutu, joten jos et ole luonut käyttäjä järjestelmään, tarvitset kiellä ensin.  On suositeltavaa pienin on xlargerc Jos käytössäsi on DW300 tai vähemmän largerc Jos käytössäsi on DW400 DW600 ja mediumrc, jos käytössäsi on DW1000 ja yläpuolella.

Alla on esimerkki siitä, miten varata muistia käyttäjälle suurentamalla niiden resurssiluokkaa.  Lue lisätietoja resurssi luokat ja miten voit luoda uuden käyttäjän löytyvät [samanaikainen ja kuormituksen] [ Concurrency] artikkelissa.

```sql
EXEC sp_addrolemember 'xlargerc', 'LoadUser'
```

### <a name="step-2-rebuild-clustered-columnstore-indexes-with-higher-resource-class-user"></a>Vaihe 2: Muodosta uudelleen liitetty columnstore indeksit suurempi resurssin luokan käyttäjän kanssa
Kirjaudu sisään käyttäjänä vaiheen 1 (kuten LoadUser), joka on nyt käyttämällä suurempi resurssiluokkaa, ja Suorita Muuta indeksi-lauseet.  Varmista, että tämä käyttäjällä ALTER taulukoita, joissa indeksi muodostetaan.  Näissä esimerkeissä näytetään uudelleen koko columnstore hakemiston tai siirtämisestä uudelleen osio. Suurten taulukoiden se on enemmän käytännön uudelleen indeksoi osio kerrallaan.

Sen sijaan, että indeksin muodostaminen uudelleen voi myös kopioi taulukko uuteen taulukkoon ohjatulla [CTAS][].  Millä tavalla parhaiten? Suurien tietomäärien [CTAS][] on yleensä nopeammin [Muuta indeksi][]. Pienempi tietomäärien [Muuta indeksi][] on helpompi käyttää, ja ei edellytä vaihtamaan taulukossa.  Saat lisätietoja siitä, miten voit muodostaa indeksejä kanssa CTAS **Rebuilding indeksit CTAS ja osion vaihtaminen** alla.

```sql
-- Rebuild the entire clustered index
ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD
```

```sql
-- Rebuild a single partition
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5
```

```sql
-- Rebuild a single partition with archival compression
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5 WITH (DATA_COMPRESSION = COLUMNSTORE_ARCHIVE)
```

```sql
-- Rebuild a single partition with columnstore compression
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5 WITH (DATA_COMPRESSION = COLUMNSTORE)
```

Offline-toiminto on SQL-tietovarasto indeksi muodostetaan uudelleen.  Lisätietoja joiden indeksit muuta indeksi uudelleen [Columnstore indeksit eheytys][] ja syntaksi aiheen [Muuta indeksi][]-kohdassa.
 
### <a name="step-3-verify-clustered-columnstore-segment-quality-has-improved"></a>Vaihe 3: Vahvista liitetty columnstore segmentin laatu on parannettu
Suorita kysely mitä tunnistettujen taulukko, jossa on heikko laatu määritetään ja tarkista segmentin laatu on entistä parempi.  Jos segmentin laatu paranna, se voi olla taulukon rivit on hyvin leveä.  Kannattaa käyttää DWU tai uudempi versio resurssiluokkaa, kun muodostetaan indeksejä uudelleen.

 
## <a name="rebuilding-indexes-with-ctas-and-partition-switching"></a>Joiden indeksit CTAS ja osion vaihtaminen

Tässä esimerkissä käytetään [CTAS][] ja siirtyminen uudelleen taulukon osio-osio. 

```sql
-- Step 1: Select the partition of data and write it out to a new table using CTAS
CREATE TABLE [dbo].[FactInternetSales_20000101_20010101]
    WITH    (   DISTRIBUTION = HASH([ProductKey])
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101,20010101
                                )
                            )
            )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

-- Step 2: Create a SWITCH out table
CREATE TABLE dbo.FactInternetSales_20000101
    WITH    (   DISTRIBUTION = HASH(ProductKey)
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101
                                )
                            )
            )
AS
SELECT *
FROM    [dbo].[FactInternetSales]
WHERE   1=2 -- Note this table will be empty

-- Step 3: Switch OUT the data 
ALTER TABLE [dbo].[FactInternetSales] SWITCH PARTITION 2 TO  [dbo].[FactInternetSales_20000101] PARTITION 2;

-- Step 4: Switch IN the rebuilt data
ALTER TABLE [dbo].[FactInternetSales_20000101_20010101] SWITCH PARTITION 2 TO  [dbo].[FactInternetSales] PARTITION 2;
```

Lisätietoja luominen uudelleen käyttämällä osioiden `CTAS`- [osio][] on artikkelissa.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja on artikkeleissa [Taulukon yleistä][Yleiskatsaus], [Taulukon tietojen tietotyyppejä][Tietotyyppejä], [jakaminen taulukon][välit], [Taulukon jakaminen]-[osio], [Ylläpito taulukon tilastotiedot][tilastotiedot] ja [Väliaikaisia taulukoita][tilapäinen].  Lisätietoja parhaista käytännöistä on artikkelissa [SQL Data Warehouse parhaita käytäntöjä][].

<!--Image references-->

<!--Article references-->
[Yleiskatsaus]: ./sql-data-warehouse-tables-overview.md
[Tietotyypit]: ./sql-data-warehouse-tables-data-types.md
[Jaa]: ./sql-data-warehouse-tables-distribute.md
[Indeksi]: ./sql-data-warehouse-tables-index.md
[Osion]: ./sql-data-warehouse-tables-partition.md
[Tilastotiedot]: ./sql-data-warehouse-tables-statistics.md
[Tilapäinen]: ./sql-data-warehouse-tables-temporary.md
[Concurrency]: ./sql-data-warehouse-develop-concurrency.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[SQL Data Warehouse parhaat käytännöt]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->
[ALTER INDEKSI]: https://msdn.microsoft.com/library/ms188388.aspx
[keon]: https://msdn.microsoft.com/library/hh213609.aspx
[liitetty indeksejä tai indeksien löytyvät]: https://msdn.microsoft.com/library/ms190457.aspx
[Luo taulukon syntaksi]: https://msdn.microsoft.com/library/mt203953.aspx
[Columnstore indeksit eheyttäminen.]: https://msdn.microsoft.com/library/dn935013.aspx#Anchor_1
[liitetty columnstore indeksit]: https://msdn.microsoft.com/library/gg492088.aspx

<!--Other Web references-->
