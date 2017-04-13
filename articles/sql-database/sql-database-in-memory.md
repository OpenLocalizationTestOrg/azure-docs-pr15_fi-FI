<properties
    pageTitle="SQL ladatun, pääset alkuun | Microsoft Azure"
    description="SQL muistissa technologies parantaa suorituskykyä tapahtumien ja analytics toiminnoista. Lue, miten voit hyödyntää seuraavia tekniikoita."
    services="sql-database"
    documentationCenter=""
    authors="jodebrui"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jodebrui"/>


# <a name="get-started-with-in-memory-preview-in-sql-database"></a>Ladatun (ennakkoversio) SQL-tietokannan käytön aloittaminen

Ladatun ominaisuuksia parantaa suorituskykyä tapahtumien ja analytics työmääriä oikean tilanteissa.

Tässä ohjeaiheessa korostaa kaksi esittelyt, yhden ladatun OLTP ja yksi ladatun analyysitiedot. Kunkin esittely tulee täydellinen vaiheet ja joudut aloittamaan Suorita esittely koodi. Mahdollistaa seuraavat vaihtoehdot:

- Näet suoritustulokset; erot variaatiot koodin avulla tai
- Lue koodin ymmärtää skenaariota ja katso, miten voit luoda ja hyödyntää olevan muistin objektit.

> [AZURE.VIDEO azure-sql-database-in-memory-technologies]

- [Nopeasti Käynnistä 1: ladatun OLTP Technologies nopeammin T-SQL-suorituskyvyn](http://msdn.microsoft.com/library/mt694156.aspx) -on toinen artikkeli avulla pääset alkuun.

#### <a name="in-memory-oltp"></a>Ladatun OLTP

Ladatun [OLTP](#install_oltp_manuallink) (online tapahtuman käsittely) ominaisuuksia ovat seuraavat:

- Muistin optimoitu taulukot.
- Käännetty grafiikkatiedostomuotoja tallennettujen toimintosarjojen.


Muistin optimoitu taulukolla on yksi esitys itsensä aktiivisen muistin lisäksi kiintolevyllä vakio esitys. Liiketoiminnan tapahtumat taulukossa merkittävästi suorituskykyä, sillä ne suoraan käsitellä esitys, joka on aktiivinen muistiin.

Ladatun OLTP, jossa voit tehostaa ylöspäin ja 30 kertaa myönnä tapahtuman siirtonopeuden työmäärää yksityiskohtia mukaan.


Grafiikkatiedostomuotoja käännetty tallennettujen toimintosarjojen edellyttävät vähemmän tietokoneen ohjeita suorituksen aikana kuin perinteisen tulkita tallennettuja toimintosarjoja. On tullut alkuperäisen kääntäminen tuloksen kestoja, jotka ovat 1/100th tulkittava kesto.


#### <a name="in-memory-analytics"></a>Ladatun Analytics 

-Toiminnon muistissa [Analytics](#install_analytics_manuallink) on:

Columnstore indeksit suorituskyvyssä analytics ja raportointi kyselyt. 


#### <a name="real-time-analytics"></a>Reaaliaikainen Analytics

[Reaaliaikaisen](http://msdn.microsoft.com/library/dn817827.aspx) analysoinnissa yhdistää ladatun OLTP sekä Analytics:

- Reaaliaikainen liiketoimintatietojen toiminnallisia tietojen perusteella.


#### <a name="availability"></a>Käytettävyys


GA, yleiseen käyttöön:

- [Columnstore indeksit](http://msdn.microsoft.com/library/dn817827.aspx) , jotka ovat *levyn*.


Esikatselu:

- Ladatun OLTP
- Reaaliaikainen toiminnallisia Analytics


Huomioon otettavia seikkoja samalla, kun olevan muistin ominaisuudet ovat esikatselu on kuvattu [jäljempänä tämän aiheen](#preview_considerations_for_in_memory).


> [AZURE.NOTE] Esikatselu nämä ominaisuudet ovat käytettävissä vain [*Premium*](sql-database-service-tiers.md) tietokantoja ei joustavasti jakavat ja Basic tai Standard tietokantoja ei ole käytettävissä.  Premium tietokantojen joustavasti jakavat tuki on tulossa. 


<a id="install_oltp_manuallink" name="install_oltp_manuallink"></a>

&nbsp;

## <a name="a-install-the-in-memory-oltp-sample"></a>A. Asenna olevan muistin OLTP-Esimerkki

Voit luoda AdventureWorksLT [Vipuventtiili V12] mallitietokannan [Azure portal](https://portal.azure.com/)muutamalla hiiren napsautuksella. Sitten tässä jaksossa kerrotaan, miten voit täydentää AdventureWorksLT tietokannan kanssa:

- Ladatun taulukot.
- Grafiikkatiedostomuotoja käännetty tallennetun toimintosarjan.


#### <a name="installation-steps"></a>Asennusohjeita

1. [Azure portal](https://portal.azure.com/)Vipuventtiili V12 palvelimessa Premium tietokannan luominen. **Lähteen** asettaminen AdventureWorksLT [Vipuventtiili V12] mallitietokannan.
 - Lisätietoja on näet [ensimmäisen Azure SQL-tietokannan luominen](sql-database-get-started.md).

2. Muodosta yhteys tietokantaan ja SQL Server Management Studiossa [(SSMS.exe)](http://msdn.microsoft.com/library/mt238290.aspx).

3. Kopioi [ladatun OLTP Transact-SQL-komentosarja](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_oltp_sample.sql) Leikepöydän.
 - T-SQL-komentosarja luo tarvittavat muistissa objektit vaiheessa 1 luomasi AdventureWorksLT mallitietokannassa.

4. T-SQL-komentosarjan liittäminen SSMS ja Suorita komentosarja.
 - On tärkeää `MEMORY_OPTIMIZED = ON` CREATE TABLE lauseita, kuin tekstimuodossa:


```
CREATE TABLE [SalesLT].[SalesOrderHeader_inmem](
    [SalesOrderID] int IDENTITY NOT NULL PRIMARY KEY NONCLUSTERED ...,
    ...
) WITH (MEMORY_OPTIMIZED = ON);
```


#### <a name="error-40536"></a>Virhe 40536


Jos saat virhekoodin 40536, kun suoritat T-SQL-komentosarjan, suorita seuraava T-SQL-komentosarja vahvistamiseksi tukeeko tietokannan ladatun:


```
SELECT DatabasePropertyEx(DB_Name(), 'IsXTPSupported');
```


Ja tuloksena on **0** tarkoittaa ladatun ei tue ja 1 tarkoittaa, että se on tuettu. Vianmäärityksessä:

- Varmista, että tietokanta on luotu, kun olevan muistin OLTP-ominaisuuksia on ollut aktiivinen esikatselua varten.
- Varmista tietokanta on Premium-palvelutaso.


#### <a name="about-the-created-memory-optimized-items"></a>Luotu muistin optimoitu kohteista

**Taulukot**: näyte sisältää seuraavat muistin optimoitu taulukot:

- SalesLT.Product_inmem
- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem
- Demo.DemoSalesOrderHeaderSeed
- Demo.DemoSalesOrderDetailSeed


Voit tarkastaa muistin optimoitu taulukoiden kautta **Object Explorer** -SSMS mukaan:

- Napsauta **taulukot** > **Suodatin** > **Suodatusasetukset** > **Optimoidaan muisti** on 1.


Tai voit tehdä kyselyn luettelon näkymien seuraavasti:


```
SELECT is_memory_optimized, name, type_desc, durability_desc
    FROM sys.tables
    WHERE is_memory_optimized = 1;
```


**Grafiikkatiedostomuotoja käännetty tallennetun toimintosarjan**: SalesLT.usp_InsertSalesOrder_inmem voit tarkastaa luettelon näkymän kyselyn avulla:


```
SELECT uses_native_compilation, OBJECT_NAME(object_id), definition
    FROM sys.sql_modules
    WHERE uses_native_compilation = 1;
```


&nbsp;

## <a name="run-the-sample-oltp-workload"></a>Suorita otoksen OLTP työmäärää

Ainoa seuraavat kaksi *tallennettujen toimintosarjojen* ero on, että ensin käyttää muistin optimoitu versio taulukot, kun toinen menetelmässä tavallisten levyn taulukoiden:

- SalesLT**.** usp_InsertSalesOrder**_inmem**
- SalesLT**.** usp_InsertSalesOrder**_ondisk**


Tässä osassa näet, miten kätevää **ostress.exe** -apuohjelman avulla voit suorittaa kaksi tallennettujen toimintosarjojen stressful tasolla. Voit vertailla, kuinka kauan rekisteröijän kaksi kuormitus toimii suorittamiseen.


Kun suoritat ostress.exe, on suositeltavaa välittää suunniteltu molemmat parametriarvot:

- Suorita paljon samanaikaisia yhteyksiä käyttämällä - n100.
- On monta kertaa, yhteys silmukassa satoja mukaan käyttämällä - r500.


Kuitenkin saatat haluta alkaa paljon pienemmät arvot, kuten - n10 ja - r50 varmistaa, kaikki toimii.


### <a name="script-for-ostressexe"></a>Komentosarjan ostress.exe


Tässä osassa näkyy T-SQL-komentosarja, joka on upotettu meidän ostress.exe komentorivin. Komentosarja käyttää T-SQL-komentosarjalla asensit aiemmin luotuja kohteita.


Seuraavaa komentosarjaa Lisää otoksen myynnin tilauksen kanssa viisi rivikohteita seuraavat muistin optimoitu *taulukot*:

- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem


```
DECLARE
    @i int = 0,
    @od SalesLT.SalesOrderDetailType_inmem,
    @SalesOrderID int,
    @DueDate datetime2 = sysdatetime(),
    @CustomerID int = rand() * 8000,
    @BillToAddressID int = rand() * 10000,
    @ShipToAddressID int = rand() * 10000;
    
INSERT INTO @od
    SELECT OrderQty, ProductID
    FROM Demo.DemoSalesOrderDetailSeed
    WHERE OrderID= cast((rand()*60) as int);
    
WHILE (@i < 20)
begin;
    EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT,
        @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od;
    SET @i = @i + 1;
end
```


Jotta _ondisk ostress.exe edellisen T-SQL-version, voit yksinkertaisesti korvaa sekä esiintymää *_inmem* alimerkkijono *_ondisk*. Nämä korvaa vaikuttavat taulukoita ja tallennettujen toimintosarjojen nimiä.


### <a name="install-rml-utilities-and-ostress"></a>Asenna RML apuohjelmien ja ostress


Ihannetapauksessa aiot suorittaa ostress.exe Azure-AM. Voit luoda [Azure virtuaalikoneen](https://azure.microsoft.com/documentation/services/virtual-machines/) saman Azure maantieteellisen alueen AdventureWorksLT tietokannan sijainti. Mutta voit suorittaa ostress.exe kannettavassa tietokoneessa sen sijaan.


AM tai joista isännöidä voit valita, asenna uusinnan Markup Language (RML)-apuohjelmat, jotka sisältävät ostress.exe.

- Lisätietoja [ladatun OLTP](http://msdn.microsoft.com/library/mt465764.aspx)esimerkkitietokannan ostress.exe.
 - Tai Katso [Ladatun OLTP mallitietokannan](http://msdn.microsoft.com/library/mt465764.aspx).
 - Tai artikkelissa [asentamiseen ostress.exe blogi](http://blogs.msdn.com/b/psssql/archive/2013/10/29/cumulative-update-2-to-the-rml-utilities-for-microsoft-sql-server-released.aspx)



<!--
dn511655.aspx is for SQL 2014,
[Extensions to AdventureWorks to Demonstrate In-Memory OLTP]
(http://msdn.microsoft.com/library/dn511655&#x28;v=sql.120&#x29;.aspx)

whereas for SQL 2016+
[Sample Database for In-Memory OLTP]
(http://msdn.microsoft.com/library/mt465764.aspx)
-->



### <a name="run-the-inmem-stress-workload-first"></a>Suorita _inmem kuormitus työmäärää ensin


Voit suorittaa tämän ostress.exe komentorivin *RML Cmd kehote* ikkunan. Komentoriviparametrit ohjata ostress avulla:

- 100 yhteydet samanaikaisesti (-n100).
- Kunkin yhteys T-SQL-komentosarjan suorittaminen 50 kertaa (-r50).


```
ostress.exe -n100 -r50 -S<servername>.database.windows.net -U<login> -P<password> -d<database> -q -Q"DECLARE @i int = 0, @od SalesLT.SalesOrderDetailType_inmem, @SalesOrderID int, @DueDate datetime2 = sysdatetime(), @CustomerID int = rand() * 8000, @BillToAddressID int = rand() * 10000, @ShipToAddressID int = rand()* 10000; INSERT INTO @od SELECT OrderQty, ProductID FROM Demo.DemoSalesOrderDetailSeed WHERE OrderID= cast((rand()*60) as int); WHILE (@i < 20) begin; EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od; set @i += 1; end"
```


Voit suorittaa edellisen ostress.exe komentoriviltä seuraavasti:


1. Tietokannan tietojen sisällön palauttamisen suorittamalla seuraavan komennon SSMS, jos haluat poistaa kaikki tiedot, jotka aikaisemmin lisätty kaikki edellisen suoritetaan:
```
EXECUTE Demo.usp_DemoReset;
```

2. Kopioi teksti edellisen ostress.exe komentorivin leikepöytääsi.

3. Korvaa `<placeholders>` varten-parametrit -S - U -P -d reaali oikeat arvot.

4. Tee muokatun komentorivin RML Cmd-ikkuna.


#### <a name="result-is-a-duration"></a>Tulos on kesto


Kun ostress.exe on valmis, suorita kesto kuin sen viimeisen rivin tulosteen kirjoitetaan RML Cmd-ikkunassa. Esimerkiksi lyhentää testin suorittaminen viimeksi noin 1,5 minuuttia:

`11/12/15 00:35:00.873 [0x000030A8] OSTRESS exiting normally, elapsed time: 00:01:31.867`


#### <a name="reset-edit-for-ondisk-then-rerun"></a>Palauta, Muokkaa _ondisk ja valitse Suorita


Kun sinulla on tulos _inmem asennuksesta, suorittamalla seuraavat vaiheet suorittamalla _ondisk varten:


1. Palauttaa tietokannan SSMS, jos haluat poistaa kaikki tiedot, jotka on lisätty edellisen asennuksen käyttämää suorittamalla seuraavan komennon:
```
EXECUTE Demo.usp_DemoReset;
```

2. Muokkaa ostress.exe komentorivin korvaa kaikki *_inmem* *_ondisk*.

3. Suorita ostress.exe toisen kerran ja siepata kesto tulos.

4. Palauttaa tietokannan, mitä voi testitiedot suuria määriä vastaava poistettavaksi uudelleen.


#### <a name="expected-comparison-results"></a>Odotettu vertailun tulokset

Ladatun Microsoftin testien on näkyvät **9 kertaa** parannus, tämä simplistic kuormituksen ostress Azure-AM sama kuin tietokannan Azure alueen käytössä.



<a id="install_analytics_manuallink" name="install_analytics_manuallink"></a>

&nbsp;


## <a name="b-install-the-in-memory-analytics-sample"></a>B. Asenna olevan muistin Analytics-Esimerkki


Tässä osassa voit verrata IO ja tilastot-tulokset columnstore indeksin, ja perinteinen b-puun indeksi käytettäessä.


Reaaliaikainen analysoinnissa-OLTP työmäärää on usein parasta löytyvät columnstore indeksi. Lisätietoja on [Columnstore indeksit kuvattu](http://msdn.microsoft.com/library/gg492088.aspx).



### <a name="prepare-the-columnstore-analytics-test"></a>Valmistele columnstore analytics-testi


1. Azure portaalin avulla voit luoda tuoreita AdventureWorksLT tietokannan otosten.
 - Käytä tarkka nimi.
 - Valitse minkä tahansa Premium-palvelutaso.

2. Kopioi [sql_in memory_analytics_sample](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_analytics_sample.sql) Leikepöydän.
 - T-SQL-komentosarja luo tarvittavat muistissa objektit vaiheessa 1 luomasi AdventureWorksLT mallitietokannassa.
 - Komentosarja luo dimensio-taulukossa ja kaksi faktataulukoiden. Kertoma-taulukoiden täytetään 3,5 miljoonaa riviä.
 - Komentosarjan saattaa kestää 15 minuuttia.

3. T-SQL-komentosarjan liittäminen SSMS ja Suorita komentosarja.
 - Tärkeää on **CREATE INDEX** -lause **COLUMNSTORE** -avainsana kuin:<br/>`CREATE NONCLUSTERED COLUMNSTORE INDEX ...;`

4. Määritä AdventureWorksLT yhteensopivuuden tasolle 130:<br/>`ALTER DATABASE AdventureworksLT SET compatibility_level = 130;`
 - Tason 130 ei liity suoraan ladatun ominaisuuksia. Mutta tason 130 tarjoaa yleensä nopeammin kuin 120 kyselyn suorituskykyä.


#### <a name="crucial-tables-and-columnstore-indexes"></a>Tärkeitä taulukot ja columnstore indeksit


- dbo. FactResellerSalesXL_CCI on taulukko, jossa on liitetty **columnstore** indeksiin, johon mukautettuja pakkaamisen *tasolla* .

- dbo. FactResellerSalesXL_PageCompressed on taulukko, joka sisältää vastaavat säännöllisesti liitetty indeksin, joka on pakattu vain *sivun* tasolla.


#### <a name="crucial-queries-to-compare-the-columnstore-index"></a>Voit verrata columnstore hakemiston tärkeitä kyselyt


[Seuraavassa](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/clustered_columnstore_sample_queries.sql) on voit suorittaa useita T-SQL-Kyselytyypit suorituskykyparannukset näkevän. T-SQL-komentosarja vaiheen 2 on kyselyitä, jotka kiinnostavat suoraan kahdet. Kyselyt vaihdella vain yhdelle riville:


- `FROM FactResellerSalesXL_PageCompressed a`
- `FROM FactResellerSalesXL_CCI a`


Liitetty columnstore indeksi on FactResellerSalesXL**_CCI** taulukon.

T-SQL-komentosarja-ote tulostaa tilastotiedot IO ja kuhunkin taulukkoon kyselyn ajan.


```
/*********************************************************************
Step 2 -- Overview
-- Page Compressed BTree table v/s Columnstore table performance differences
-- Enable actual Query Plan in order to see Plan differences when Executing
*/
-- Ensure Database is in 130 compatibility mode
ALTER DATABASE AdventureworksLT SET compatibility_level = 130
GO

-- Execute a typical query that joins the Fact Table with dimension tables
-- Note this query will run on the Page Compressed table, Note down the time
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO

SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_PageCompressed a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
WHERE e.ProductCategoryKey =2
    AND c.FullDateAlternateKey BETWEEN '1/1/2014' AND '1/1/2015'
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO
SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO


-- This is the same Prior query on a table with a Clustered Columnstore index CCI 
-- The comparison numbers are even more dramatic the larger the table is, this is a 11 million row table only.
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO
SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_CCI a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
WHERE e.ProductCategoryKey =2
    AND c.FullDateAlternateKey BETWEEN '1/1/2014' AND '1/1/2015'
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO

SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO
```



<a id="preview_considerations_for_in_memory" name="preview_considerations_for_in_memory"></a>


## <a name="preview-considerations-for-in-memory-oltp"></a>Ladatun OLTP esikatselu huomioon otettavia seikkoja


Azure SQL-tietokantaan olevan muistin OLTP-ominaisuuksia on ollut [käytössä oleva 2015 28 lokakuussa esikatselu](https://azure.microsoft.com/updates/public-preview-in-memory-oltp-and-real-time-operational-analytics-for-azure-sql-database/).


Nykyinen esikatselussa ladatun OLTP tuetaan vain:

- Tietokantoja, jotka ovat *Premium* palvelutaso.

- Tietokantoja, jotka on luotu olevan muistin OLTP-ominaisuuksia on ollut aktiivinen.
 - Uuden tietokannan ei tue olevan muistin OLTP, jos se on palautettu tietokannasta, joka on luotu ennen olevan muistin OLTP ominaisuuksia aktiivinen.


Epävarmoissa tapauksissa voit aina suorittaa seuraavat T-SQL SELECT varmistaa, että tukeeko tietokannan ladatun OLTP. Tulos **1** tarkoittaa, että tietokanta tue ladatun OLTP:

```
SELECT DatabasePropertyEx(DB_NAME(), 'IsXTPSupported');
```


Jos kysely palauttaa arvon **1**, ladatun OLTP tuetaan tässä tietokannassa ja minkä tahansa tietokannan kopiota ja tietokannan palauttaminen luotu perusteella tätä tietokantaa.


#### <a name="objects-allowed-only-at-premium"></a>Objektien sallittu vain Premium


Jos tietokanta sisältää seuraavanlaisia ladatun OLTP objekteja tai tyypit, päivitysprosessin Premium tietokannan Basic tai Standard service-tason ei tueta. Tietokannan muuntaminen, poista nämä objektit:

- Muistin optimoitu taulukot
- Muistin optimoitu taulukoiden tyypit
- Grafiikkatiedostomuotoja käännetty moduulit


#### <a name="other-relationships"></a>Muita suhteita


- Käyttämällä ladatun OLTP ominaisuuksia joustavasti jakavat tietokantoja ei tue esikatselun aikana.
 - Jos haluat siirtää tietokannan, joka on tai on ollut ladatun OLTP objektien joustavasti resurssivarantoon, toimi seuraavasti:
  - 1. Pudota tietokannan muistin optimoitu taulukoiden, taulukoiden tyypit ja grafiikkatiedostomuotoja käännetty T-SQL-moduulit
  - 2. Muuttaa tietokannan service-tason vakio
  - 3. Tietokannan siirtäminen joustavasti resurssivarantoon

- Käyttämällä ladatun OLTP kanssa SQL Data Warehouse ei tue.
 - Ladatun Analytics columnstore indeksi-ominaisuus on tuettu SQL-tietovarasto.

- Kyselyn säilön varaa sisä käännetty grafiikkatiedostomuotoja moduulit kyselyt.

- Ladatun OLTP joitakin Transact-SQL-ominaisuuksia ei tueta. Tämä koskee sekä Microsoft SQL Server Azure SQL-tietokantaan. Lisätietoja on artikkelissa:
 - [Ladatun OLTP Transact-SQL-tuki](http://msdn.microsoft.com/library/dn133180.aspx)
 - [Ladatun OLTP ei tue Transact-SQL-rakenteita](http://msdn.microsoft.com/library/dn246937.aspx)


## <a name="next-steps"></a>Seuraavat vaiheet


- Kokeile [Käytä ladatun OLTP aiemmin Azure SQL-sovelluksessa.](sql-database-in-memory-oltp-migration.md)


## <a name="additional-resources"></a>Lisäresursseja

#### <a name="deeper-information"></a>Tarkempaa tietoa

- [Lisätietoja ladatun OLTP, joka koskee sekä Microsoft SQL Server Azure SQL-tietokanta](http://msdn.microsoft.com/library/dn133186.aspx)

- [Reaaliaikainen toiminnallisia Analytics MSDN-tietoja](http://msdn.microsoft.com/library/dn817827.aspx)

- Tekninen raportti [Yleiset työmäärää mallit ja huomioon otettavia seikkoja siirtämisestä](http://msdn.microsoft.com/library/dn673538.aspx), jossa kuvataan työmäärää kuviot, jossa ladatun OLTP on yleisesti merkittäviä etuja.

#### <a name="application-design"></a>Sovelluksen suunnittelu

- [Muistin OLTP (ladatun optimointi)](http://msdn.microsoft.com/library/dn133186.aspx)

- [Käytä ladatun OLTP aiemmin Azure SQL-sovelluksessa.](sql-database-in-memory-oltp-migration.md)

#### <a name="tools"></a>Työkalut

- [SQL Server Data Tools esikatselu (SSDT)](http://msdn.microsoft.com/library/mt204009.aspx), kuukausittainen uusimman version.

- [SQL Server-uusinnan Markup Language (RML)-apuohjelmat kuvaus](http://support.microsoft.com/en-us/kb/944837)

- [Näytön ladatun tallennustilan](sql-database-in-memory-oltp-monitoring.md) ladatun OLTP.

