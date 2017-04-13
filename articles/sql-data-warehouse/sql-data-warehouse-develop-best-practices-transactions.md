<properties
   pageTitle="SQL-tietovarasto tapahtumia optimointi | Microsoft Azure"
   description="Parhaiden käytäntöjen ohjeita kirjoittamiseen tehokas tapahtuman päivitykset Azure SQL-tietovarasto"
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
   ms.date="07/31/2016"
   ms.author="jrj;barbkess"/>

# <a name="optimizing-transactions-for-sql-data-warehouse"></a>SQL-tietovarasto tapahtumia optimointi

Tässä artikkelissa kerrotaan, miten voit parantaa suorituskykyä tapahtumien koodin samalla, kun pitkä palautukset riskin pienentäminen.

## <a name="transactions-and-logging"></a>Tapahtumien ja kirjaaminen

Tapahtumat ovat tärkeä osa relaatio-tietokantamoduulin. SQL-tietovarasto käyttää tapahtumien tietojen muokkauksen aikana. Näihin tapahtumiin voi olla eksplisiittisiä tai implisiittisiä. Yksittäisen `INSERT`, `UPDATE` ja `DELETE` lauseet ovat kaikki esimerkkejä implisiittisen tapahtumat. Eksplisiittinen tapahtumat kirjoitetaan erikseen käyttämällä kehittäjä `BEGIN TRAN`, `COMMIT TRAN` tai `ROLLBACK TRAN` ja käytetään yleensä useita muokkaus-lauseet on yhdistetty toisiinsa atomisia yksikkönä. 

Azure SQL-tietovarasto tallentaa muutokset tietokantaan avulla tapahtumalokit. Kunkin jakauman on oma tapahtumaloki. Tapahtuman log kirjoituksia ovat automaattisia. Pakollinen määrityksiä ei. Kuitenkin samalla, kun tämä prosessi takaa Kirjoita sen sisältämiin katseltavan järjestelmässä. Voit pienentää tämä vaikutus kirjoittamalla muulla tavoin tehokas koodia. Muulla tavoin tehokas koodi kuuluu laajasti kahteen luokkaan.

- Hyödyntää mahdollisimman vähän kirjaaminen rakenteita mahdollisuuksien mukaan
- Tietojen kohdistetuissa erissä avulla voit välttää yksittäinen pitkä käynnissä olevat tapahtumat
- Hyväksyy siirtymistä annetun osion suuria muutoksia kuvio osio

## <a name="minimal-vs-full-logging"></a>Mahdollisimman vähän ja koko kirjaaminen

Toisin kuin täysin kirjautuneena toimintoja, jotka käyttävät tapahtumalokin jokaisen rivin muutosten seurantaan, vähimmäistukitiedot kirjautuneena toimintojen seurantaan määrin kohdistukset ja vain metatiedon muutokset. Vuoksi mahdollisimman vähän kirjaaminen liittyy kirjaaminen vain tiedot, joita tarvitaan peruutus epäonnistui tai eksplisiittinen pyynnön tapahtuman (`ROLLBACK TRAN`). Kun paljon vähemmän tietoja seurataan tapahtumalokiin vähimmäistukitiedot kirjautuneena toiminto suorittaa parempi vaihtoehto samankokoista täysin kirjautuneena toiminto. Lisäksi vähemmän kirjoituksia siirtyä tapahtumalokiin, koska paljon tietoja pienempi summa luodaan ja eli on enemmän i/o tehokkaasti.

Tapahtuman turvallisuus raja-arvot koskevat vain täysin kirjautuneena toimintoja.

>[AZURE.NOTE] Vähimmäistukitiedot kirjautuneena toiminnot voivat osallistua eksplisiittinen tapahtumat. Kun kohdistus rakenteet kaikki muutokset jäljitetään, on mahdollista aikaisempi vähimmäistukitiedot kirjautuneena toimintoja. On tärkeää ymmärtää, että muutos on kirjautunut "vähimmäistukitiedot" ei ole kirjautuneena poistaminen.

## <a name="minimally-logged-operations"></a>Vähimmäistukitiedot kirjautuneena toiminnot

Lokiin vähimmäistukitiedot pystyvät seuraavat toimenpiteet:

- Luo taulukon AS SELECT ([CTAS][])
- LISÄÄ... VALITSE
- INDEKSIN LUOMINEN
- ALTER INDEKSI-MUODOSTA
- DROP INDEX
- LYHENNÄ TAULUKKO
- POISTA TAULUKKO
- MUUTA TAULUKON VALITSIN-OSIO

<!--
- MERGE
- UPDATE on LOB Types .WRITE
- SELECT..INTO
-->

>[AZURE.NOTE] Sisäinen tietojen siirtämistä toimintoja (kuten `BROADCAST` ja `SHUFFLE`) ei koske tapahtuman turvallisuutta rajoitus.

## <a name="minimal-logging-with-bulk-load"></a>Mahdollisimman vähän kirjaaminen kanssa samalla kertaa lataaminen

`CTAS`ja `INSERT...SELECT` ovat molemmat joukkosähköposti kuormituksen toimintoja. Kuitenkin molemmat ovat vaikuttavat kohde taulukkomääritys ja lataa-skenaario määräytyvät. Alla on taulukko, jossa kerrotaan, jos Joukkotoiminnon kokonaan tai vähimmäistukitiedot kirjataan:  

| Ensisijainen indeksi               | Lataa-skenaario                                            | Lokiin kirjaaminen-tilassa |
| --------------------------- | -------------------------------------------------------- | ------------ |
| Keon                        | Mikä tahansa                                                      | **Mahdollisimman vähän**  |
| Ryhmitelty indeksi             | Tyhjä kohdetaulukko                                       | **Mahdollisimman vähän**  |
| Ryhmitelty indeksi             | Ladattu rivit eivät ole päällekkäiset kohde sivujen kanssa | **Mahdollisimman vähän**  |
| Ryhmitelty indeksi             | Ladattu rivien kaavioalueen päälle, sivujen kohde        | Koko         |
| Liitetty Columnstore indeksi | Erän koon > = 102,400 osion tasattu jakauman kohden | **Mahdollisimman vähän**  |
| Liitetty Columnstore indeksi | Erän koon < 102,400 osion tasattu jakauman kohden  | Koko         |

On otettava huomioon, että kaikki kirjoituksia päivittämään toissijaisen tai ei ole liitetty indeksit on aina täysin kirjautuneena toiminnot.

> [AZURE.IMPORTANT] SQL-tietovarasto on 60 jaot. Jos vuoksi kaikki rivit ovat tasaisesti ja purkamisen-osio, erä täytyy olla 6,144,000 rivejä tai suurempi kirjautunut vähimmäistukitiedot indeksin Columnstore kirjoitettaessa. Jos taulukossa on osioitu ja lisättävä rivit kattavat osion rajat, sinun on 6,144,000 riviä / osion reunan olettaen jopa jakautuman. Kunkin jakauman kunkin osion itsenäisesti saa lisää vähimmäistukitiedot kirjautunut sisään jakauman 102,400 rivin raja-arvo.

Tietojen lataaminen ei-tyhjät-taulukkoon, jossa on liitetty indeksi usein voi olla sekoitus täysin kirjautuneena ja vähimmäistukitiedot kirjautuneena rivien määrä. Ryhmitelty indeksi on tasapainoinen puun (puun b) sivut. Jos sivun tallennettavat sisältää jo toinen tapahtuma rivit, nämä kirjoituksia kirjataan kokonaan. Kuitenkin jos sivu on tyhjä sitten kyseiselle sivulle kirjoittaminen vähimmäistukitiedot kirjataan.

## <a name="optimizing-deletes"></a>Poistaa optimointi

`DELETE`on täysin kirjautuneena toiminto.  Jos haluat poistaa suuria määriä tietoja taulukkoon tai osion, se on usein kannattaa `SELECT` säilytettävä, tietoja, joita voit suorittaa vähimmäistukitiedot kirjautuneena toiminto.  Tämän vuoksi Luo uusi taulukko, jossa [CTAS][].  Luomiasi käyttää yhteystietokuvan vanha taulukon juuri luomasi taulukon [nimeä][] .

```sql
-- Delete all sales transactions for Promotions except PromotionKey 2.

--Step 01. Create a new table select only the records we want to kep (PromotionKey 2)
CREATE TABLE [dbo].[FactInternetSales_d]
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,   20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,   20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,   20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,   20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
WHERE   [PromotionKey] = 2
OPTION (LABEL = 'CTAS : Delete')
;

--Step 02. Rename the Tables to replace the 
RENAME OBJECT [dbo].[FactInternetSales]   TO [FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_d] TO [FactInternetSales];
```

## <a name="optimizing-updates"></a>Optimointi päivitykset

`UPDATE`on täysin kirjautuneena toiminto.  Jos haluat päivittää suuri määrä taulukon rivien tai osion voi usein olla tähän mennessä tehokkaampaa vähimmäistukitiedot kirjautuneena toiminnon, kuten [CTAS][] avulla.

Koko taulukon alla olevassa esimerkissä päivitys on muunnettu `CTAS` niin, että mahdollisimman vähän kirjaaminen on mahdollista.

Tässä tapauksessa jälkikäteen lisätään alennuksen määrä myynti-taulukossa:

```sql
--Step 01. Create a new table containing the "Update". 
CREATE TABLE [dbo].[FactInternetSales_u]
WITH
(   CLUSTERED INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,   20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,   20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,   20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,   20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
                )
)
AS 
SELECT
    [ProductKey]  
,   [OrderDateKey] 
,   [DueDateKey]  
,   [ShipDateKey] 
,   [CustomerKey] 
,   [PromotionKey] 
,   [CurrencyKey] 
,   [SalesTerritoryKey]
,   [SalesOrderNumber]
,   [SalesOrderLineNumber]
,   [RevisionNumber]
,   [OrderQuantity]
,   [UnitPrice]
,   [ExtendedAmount]
,   [UnitPriceDiscountPct]
,   ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,   [ProductStandardCost]
,   [TotalProductCost]
,   ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,   [TaxAmt]
,   [Freight]
,   [CarrierTrackingNumber] 
,   [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
OPTION (LABEL = 'CTAS : Update')
;

--Step 02. Rename the tables
RENAME OBJECT [dbo].[FactInternetSales]   TO [FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_u] TO [FactInternetSales];

--Step 03. Drop the old table
DROP TABLE [dbo].[FactInternetSales_old]
```

> [AZURE.NOTE] Luoda uudelleen suurten taulukoiden voit hyötyä käyttämisestä SQL-tietovarasto työmäärää hallintaominaisuudet. Lisätietoja Tutustu [samanaikaisen][] artikkelin kuormituksen hallinta-kohdassa.

## <a name="optimizing-with-partition-switching"></a>Kanssa osion siirtyminen optimointia

Suuret muutokset sisällä [taulukon osio][]on kohdatessaan siirtymistä kuvion osio on järkevää paljon. Jos tietojen muuttaminen ei merkittävä ja ulottuu useita osioita, valitse ainoastaan Iterointi päälle osiot kertyy saman tuloksen.

Osion valitsimen suorittavan vaiheet ovat seuraavat:
1. Luo tyhjä osio ulos
2. 'Päivitys' toimiminen CTAS
3. Olemassa olevia tietoja, siirry out taulukko
4. Uusien tietojen vaihtaminen
5. Tietojen tyhjennys

Kuitenkin tunnistaminen osioita, siirry ensin annettava luominen helper menettelyä, kuten alla. 

```sql
CREATE PROCEDURE dbo.partition_data_get
    @schema_name           NVARCHAR(128)
,   @table_name            NVARCHAR(128)
,   @boundary_value        INT
AS
IF OBJECT_ID('tempdb..#ptn_data') IS NOT NULL
BEGIN
    DROP TABLE #ptn_data
END
CREATE TABLE #ptn_data
WITH    (   DISTRIBUTION = ROUND_ROBIN
        ,   HEAP
        )
AS
WITH CTE
AS
(
SELECT  s.name                          AS [schema_name]
,       t.name                          AS [table_name]
,       p.partition_number              AS [ptn_nmbr]
,       p.[rows]                        AS [ptn_rows]
,       CAST(r.[value] AS INT)          AS [boundary_value]
FROM        sys.schemas                 AS s
JOIN        sys.tables                  AS t    ON  s.[schema_id]       = t.[schema_id]
JOIN        sys.indexes                 AS i    ON  t.[object_id]       = i.[object_id]
JOIN        sys.partitions              AS p    ON  i.[object_id]       = p.[object_id] 
                                                AND i.[index_id]        = p.[index_id] 
JOIN        sys.partition_schemes       AS h    ON  i.[data_space_id]   = h.[data_space_id]
JOIN        sys.partition_functions     AS f    ON  h.[function_id]     = f.[function_id]
LEFT JOIN   sys.partition_range_values  AS r    ON  f.[function_id]     = r.[function_id] 
                                                AND r.[boundary_id]     = p.[partition_number]
WHERE i.[index_id] <= 1
)
SELECT  *
FROM    CTE
WHERE   [schema_name]       = @schema_name
AND     [table_name]        = @table_name
AND     [boundary_value]    = @boundary_value
OPTION (LABEL = 'dbo.partition_data_get : CTAS : #ptn_data')
;
GO
```

Tämä toiminto suurentaa koodi käyttää uudelleen ja pitää siirtymistä Esimerkki tiiviimmän osion.

Seuraava koodi esitellään viiden vaiheen edellä mainituista saavuttamiseksi koko osion vaihtaminen säännöllisesti.

```sql
--Create a partitioned aligned empty table to switch out the data 
IF OBJECT_ID('[dbo].[FactInternetSales_out]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_out]
END

CREATE TABLE [dbo].[FactInternetSales_out]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED COLUMNSTORE INDEX
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20020101, 20030101
                                                )
                )
)
AS
SELECT *
FROM    [dbo].[FactInternetSales]
WHERE 1=2
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Create a partitioned aligned table and update the data in the select portion of the CTAS
IF OBJECT_ID('[dbo].[FactInternetSales_in]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_in]
END

CREATE TABLE [dbo].[FactInternetSales_in]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED COLUMNSTORE INDEX
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20020101, 20030101
                                                )
                )
)
AS 
SELECT
    [ProductKey]  
,   [OrderDateKey] 
,   [DueDateKey]  
,   [ShipDateKey] 
,   [CustomerKey] 
,   [PromotionKey] 
,   [CurrencyKey] 
,   [SalesTerritoryKey]
,   [SalesOrderNumber]
,   [SalesOrderLineNumber]
,   [RevisionNumber]
,   [OrderQuantity]
,   [UnitPrice]
,   [ExtendedAmount]
,   [UnitPriceDiscountPct]
,   ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,   [ProductStandardCost]
,   [TotalProductCost]
,   ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,   [TaxAmt]
,   [Freight]
,   [CarrierTrackingNumber] 
,   [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
WHERE   OrderDateKey BETWEEN 20020101 AND 20021231
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Use the helper procedure to identify the partitions
--The source table
EXEC dbo.partition_data_get 'dbo','FactInternetSales',20030101
DECLARE @ptn_nmbr_src INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_src

--The "in" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_in',20030101
DECLARE @ptn_nmbr_in INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_in

--The "out" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_out',20030101
DECLARE @ptn_nmbr_out INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_out

--Switch the partitions over
DECLARE @SQL NVARCHAR(4000) = '
ALTER TABLE [dbo].[FactInternetSales]   SWITCH PARTITION '+CAST(@ptn_nmbr_src AS VARCHAR(20))   +' TO [dbo].[FactInternetSales_out] PARTITION ' +CAST(@ptn_nmbr_out AS VARCHAR(20))+';
ALTER TABLE [dbo].[FactInternetSales_in] SWITCH PARTITION '+CAST(@ptn_nmbr_in AS VARCHAR(20))   +' TO [dbo].[FactInternetSales] PARTITION '     +CAST(@ptn_nmbr_src AS VARCHAR(20))+';'
EXEC sp_executesql @SQL

--Perform the clean-up
TRUNCATE TABLE dbo.FactInternetSales_out;
TRUNCATE TABLE dbo.FactInternetSales_in;

DROP TABLE dbo.FactInternetSales_out
DROP TABLE dbo.FactInternetSales_in
DROP TABLE #ptn_data
```

## <a name="minimize-logging-with-small-batches"></a>Lokitiedoston pienissä erissä pienentäminen

Suurien tietomäärien muokkaus-toimintoja se voi esittää kannattaa toiminto Jaa joukkojen tai alueen työmäärän yksikkö erissä.

Työn esimerkki on jäljempänä. Erän koko on määritetty trivial luvuksi Korosta tekniikka. Todellisuudessa erän koko olisi merkittävästi suurempia. 

```sql
SET NO_COUNT ON;
IF OBJECT_ID('tempdb..#t') IS NOT NULL
BEGIN
    DROP TABLE #t;
    PRINT '#t dropped';
END

CREATE TABLE #t
WITH    (   DISTRIBUTION = ROUND_ROBIN
        ,   HEAP
        )
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS seq_nmbr
,       SalesOrderNumber
,       SalesOrderLineNumber
FROM    dbo.FactInternetSales
WHERE   [OrderDateKey] BETWEEN 20010101 and 20011231
;

DECLARE @seq_start      INT = 1
,       @batch_iterator INT = 1
,       @batch_size     INT = 50
,       @max_seq_nmbr   INT = (SELECT MAX(seq_nmbr) FROM dbo.#t)
;

DECLARE @batch_count    INT = (SELECT CEILING((@max_seq_nmbr*1.0)/@batch_size))
,       @seq_end        INT = @batch_size
;

SELECT COUNT(*)
FROM    dbo.FactInternetSales f

PRINT 'MAX_seq_nmbr '+CAST(@max_seq_nmbr AS VARCHAR(20))
PRINT 'MAX_Batch_count '+CAST(@batch_count AS VARCHAR(20))

WHILE   @batch_iterator <= @batch_count
BEGIN
    DELETE
    FROM    dbo.FactInternetSales
    WHERE EXISTS
    (
            SELECT  1
            FROM    #t t
            WHERE   seq_nmbr BETWEEN  @seq_start AND @seq_end
            AND     FactInternetSales.SalesOrderNumber      = t.SalesOrderNumber
            AND     FactInternetSales.SalesOrderLineNumber  = t.SalesOrderLineNumber
    )
    ;

    SET @seq_start = @seq_end
    SET @seq_end = (@seq_start+@batch_size);
    SET @batch_iterator +=1;
END
```

## <a name="pause-and-scaling-guidance"></a>Keskeytä ja skaalausta ohjeet

Azure SQL-tietovarasto voit keskeyttää, jatkaa ja Skaalaa oman tietovarasto pyydettäessä. Kun keskeyttää tai skaalata SQL-tietovarasto on tärkeää ymmärtää tapahtumakartoitus tapahtumia lopetetaan heti. Avaa tapahtumia peruutettava vuoksi. Jos havainnollistamiseen oli pitkään käynnissä ja kesken tietojen muuttaminen ennen keskeyttäminen tai asteikko-toimintoa, toimisivat on voi kumota. Tämä saattaa vaikuttaa keskeyttäminen tai skaalata Azure SQL-tietovarasto tietokannan kuluvaa aikaa. 

> [AZURE.IMPORTANT] Molemmat `UPDATE` ja `DELETE` täysin kirjautuneena toimintoja ja niin näitä kumota/tehdä uudelleen toimintojen voi kestää huomattavasti kauemmin kuin vastaava kirjautunut vähimmäistukitiedot toimintoja. 

Paras tapaus on lentojen tietojen muokkaaminen tapahtumat valmis ennen keskeyttäminen tai skaalauksen SQL-tietovarasto avulla. Tämä ei aina voidaan kuitenkin käytännön. Riskien vähentäminen on pitkä peruuttaminen, voit tarvittaessa jokin seuraavista vaihtoehdoista:

- Kirjoita uudelleen pitkiä käynnissä olevia toimintoja käyttämällä [CTAS][]
- Jaa toiminto joukkojen; käsittelee alijoukkoa rivit

## <a name="next-steps"></a>Seuraavat vaiheet

Kohdassa [SQL-tietovarasto tapahtumat][] lisätietoja eristystaso tasot ja tapahtumien rajoitukset.  Yleisiä tietoja muista parhaista käytännöistä on-kohdassa [SQL Data Warehouse parhaita käytäntöjä][].

<!--Image references-->

<!--Article references-->
[SQL-tietovarasto tapahtumat]: ./sql-data-warehouse-develop-transactions.md
[taulukko-osio]: ./sql-data-warehouse-tables-partition.md
[Samanaikainen]: ./sql-data-warehouse-develop-concurrency.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[SQL Data Warehouse parhaat käytännöt]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->
[alter index]:https://msdn.microsoft.com/library/ms188388.aspx
[NIMEÄMINEN UUDELLEEN]: https://msdn.microsoft.com/library/mt631611.aspx

<!-- Other web references -->

