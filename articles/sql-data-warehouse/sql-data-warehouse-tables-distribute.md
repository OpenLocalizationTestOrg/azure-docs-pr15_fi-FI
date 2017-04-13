<properties
   pageTitle="SQL-tietovarasto taulukoiden jakaminen | Microsoft Azure"
   description="Käytön aloittaminen jakaminen Azure SQL-tietovarasto taulukot."
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
   ms.date="08/30/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="distributing-tables-in-sql-data-warehouse"></a>SQL-tietovarasto taulukoiden jakaminen

> [AZURE.SELECTOR]
- [Yleiskatsaus][]
- [Tietotyypit][]
- [Jaa][]
- [Indeksi][]
- [Osion][]
- [Tilastotiedot][]
- [Tilapäinen][]

SQL-tietovarasto on erittäin rinnakkain käsittely (MPP) jakaa tietokannan.  Tietojen jakaminen ja käsitteleminen ominaisuuksien useita solmujen, SQL-tietovarasto voit tarjota erittäin suuri skaalattavuus - paljon muutakin kuin mikä tahansa yksittäinen järjestelmä.  Päätät, miten voit jakaa tietojen sisällä SQL-tietovarasto on yksi tärkeimmät seikat parhaan mahdollisen suorituskyvyn saavuttamiseksi.   Parhaan mahdollisen suorituskyvyn avain on pienentäminen tietojen siirto ja puolestaan salaisuus pienentäminen tietojen siirto kannattaa valita oikean jakauman strategia.

## <a name="understanding-data-movement"></a>Tietoja tietojen siirto

MPP-järjestelmässä kunkin taulukon tiedot jaetaan yli useita pohjana olevia tietokantoja.  Useimmat optimoitu kyselyjen MPP-järjestelmässä voi välittää yksinkertaisesti kautta suorittamaan yksittäiset hajautettu tietokannat ilman toimia, jos muita tietokantoja välillä.  Esimerkiksi oletetaan, että jokin tietokanta on myyntitiedot, jossa on kaksi taulukkoa, myynti ja asiakkaiden kanssa.  Jos kysely, joka on sales-taulukon liittäminen Asiakas-taulukosta ja voit jakaa myynti ja asiakkaan taulukoiden määrittäminen asiakkaan luvulla kunkin asiakkaan sijoittaminen erillisellä, kyselyt, joiden myynti- ja liittyä voidaan ratkaista jokaisen tietokannassa ei ole kokemusta muut tietokannat.  Sen sijaan, jos myynnin tiedot jaettuna numero ja asiakastietojen mukaan asiakasnumeron, valitse kaikki tietyn tietokannan ei ole vastaavat tiedot kunkin asiakkaan ja jos haluat määrittää liittymään myynnin tiedot asiakkaan tietoihin, tarvitset hakemaan tiedot kunkin asiakkaan muut tietokannat.  Toisen esimerkin tietojen siirto on ilmetä asiakastiedot Siirry myyntitiedot, niin, että taulukot on liitetty.  

Tietojen siirto ei aina virheelliset asian, joskus on tarpeen kyselyn ratkaisemiseen.  Mutta kun tämä painaa voidaan välttää, luonnollisesti kysely suoritetaan nopeammin.  Tietojen siirto syntyy yleisimmin, kun taulukot on liitetty tai koosteet suoritetaan.  Usein sinun on suoritettava, jotta aikana voit ehkä optimointi yksi tilanne, kuten liitoksen, sinun on edelleen tietojen siirto auttaa ratkaisemaan muiden skenaarion, kuten kooste.  Kannattaa on käyttäminen joka on vähemmän työtä.  Useimmissa tapauksissa jakaminen suuri faktataulukoiden yleisesti liitettyjen sarakkeen on mahdollisimman tehokasta pienentämiseen useimmat tietojen siirto.  Liitoksen sarakkeiden tietojen jakaminen on paljon yhteinen menetelmä Pienennä tietojen siirto kuin sarakkeiden kooste osallistuvat tietojen jakaminen.

## <a name="select-distribution-method"></a>Valitse jakauma

SQL-tietovarasto jakaa tietoja taustalla, 60 tietokannat.  Yksittäisten tietokantojen kutsutaan **jakauman**.  Kun tiedot on ladattu kuhunkin taulukkoon, SQL-tietovarasto pitää tietää, miten voit jakaa tietoja yli 60 nämä jaot.  

Taulukon tasolla on määritetty jakauma ja tällä hetkellä on kaksi vaihtoehtoa:

1. **PYÖRISTÄ-funktiota Mikko** , joka jakaa tietoja tasaisesti mutta satunnaisesti.
2. **Hash Distributed** joka jakaa tietoja hajautuksessa yhden sarakkeen arvot

Oletusarvoisesti, kun et määritä data jakauman-menetelmällä taulukon jaetaan **PYÖRISTÄ-funktiota Mikko** jakauman menetelmällä.  Kuitenkin sinusta tulee kehittyneempiä käyttöympäristösi, kun haluat käyttämällä **hash distributed** taulukoiden minimoimaan tietojen siirto, joka puolestaan optimoi kyselyn suorituskykyä.

### <a name="round-robin-tables"></a>PYÖRISTÄ-funktiota Mikko taulukot

Käyttämällä PYÖRISTÄ-funktiota Mikko maksutavan tietojen jakaminen on hyvin siitä, miten se äänet.  Kun tiedot on ladattu, kunkin rivin lähetetään vain seuraava jakauman.  Tätä menetelmää jakaminen tiedot aina satunnaisesti jakaa tietoja erittäin tasaisesti kaikissa jaot.  Toisin sanoen ei ole lajittelu valmis PYÖRISTÄ-funktiota Mikko aikana joka sijoittaa tiedot.  PYÖRISTÄ-funktiota Mikko jakauman kutsutaan joskus satunnaisia hash tästä syystä.  Pyöreän pöydän hajautettu taulukkoa ei ole tarpeen ymmärtää tiedot.  Tästä syystä pyöreän pöydän taulukoiden tekevät usein hyvä ladataan kohteet.

Oletusarvon mukaan Jos jakauman mitään menetelmää valitaan, PYÖRISTÄ-funktiota Mikko jakaumamenetelmä käytetään.  Kun PYÖRISTÄ-funktiota Mikko taulukot on helppo käyttää, koska tiedot jaetaan satunnaisesti järjestelmän, se tarkoittaa sitä, että järjestelmä voi taata mitä jakauman kullakin rivillä on kuitenkin.  Järjestelmä on tuloksena joskus käynnistää tietojen siirrossa, voit parantaa tietojen, ennen kuin sitä voidaan ratkaista kyselyn.  Tämä painaa saattavat hidastaa kyselyissä.

Harkitse PYÖRISTÄ-funktiota Mikko jakauman taulukon seuraavissa tilanteissa:

- Kun yksinkertaista aloituskohta toimiminen
- Jos avainta ei ole ilmeisimmät liittävä
- Jos ei ole hyvällä perusavaimella sarakkeessa hash jakaminen taulukossa
- Jos taulukon Jaa common liity-avain ja muiden taulukoiden välille
- Jos liitos on pienempi kuin muiden liitosten merkittäviä
- Kun taulukko on väliaikainen väliaikaisen taulukossa

Molempia näistä esimerkeistä Luo PYÖRISTÄ-funktiota Mikko taulukkoon:

```SQL
-- Round Robin created by default
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
;

-- Explicitly Created Round Robin Table
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = ROUND_ROBIN
)
;
```

> [AZURE.NOTE] PYÖRISTÄ-funktiota Mikko ollessa käytössä eksplisiittinen käyttäjän DDL taulukon oletustietotyyppi parhaat käytännöt niin, että taulukon asettelu suhteessa ovat Tyhjennä muille.

### <a name="hash-distributed-tables"></a>Hash hajautettu taulukot

**Hash distributed** algoritmin avulla voit jakaa taulukoiden voi parantaa suorituskykyä skenaarioita varten pienentämällä tietojen siirto kyselyn yhteydessä.  Jaettu Hash-taulukkojen taulukot, jotka on jaettu hajautettu tietokantoihin hajautusalgoritmia käyttäminen yhdessä sarakkeessa, joka valitaan.  Jakauman sarake on mitä määrittää, kuinka tiedot jaetaan eri aikajaksoille tietokantoja yli.  Hash-funktio käyttää jakauman sarakkeen rivien liittäminen jaot.  Hajautusalgoritmin ja tuloksena jakeluryhmien on deterministic.  Tämä on käytettävä samaa tietotyyppiä saman arvon on aina sama jakauman.    

Tässä esimerkissä luodaan taulukon tunnus jakaa:

```SQL
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,  DISTRIBUTION = HASH([ProductKey])
)
;
```

## <a name="select-distribution-column"></a>Jakauman sarakkeen valitseminen

Kun valitset **hash** jakaa taulukon, sinun on yksittäinen jakauman sarakkeen valitsemiseksi.  Jakauman sarakkeen valittaessa on kolme tärkeintä tekijät ottaa huomioon.  

Valitse yksi sarake, joka on:

1. Eivät päivity
2. Jakaa tietoja tasaisesti tietojen jakauman.vinous välttäminen
3. Pienennä tietojen siirto

### <a name="select-distribution-column-which-will-not-be-updated"></a>Valitse jakeluryhmät sarakkeeseen, joka ei päivitetä

Jakauman-sarakkeiden järjestys ei voi päivittää, sen vuoksi, valitse staattinen arvoja sisältävä sarake.  Jos sarake on päivitettävä, sitä ei yleensä hyvien candidate.  Jos näkyvissä on palvelupyynnön, joissa sinun on päivitettävä jakauman sarakkeen, tämä voit tehdä poistaminen ensimmäisen rivin ja lisäämällä sitten uusi rivi.

### <a name="select-distribution-column-which-will-distribute-data-evenly"></a>Valitse jakeluryhmät sarakkeeseen, joka jakaa tietoja tasaisesti

Koska hajautettu järjestelmä suorittaa vain yhtä nopeasti kuin sen mahdollisimman pieneksi jakautuminen, on tärkeää Jaa työ tasaisesti yli jaot saavuttamiseksi tasapainoinen suorittamisen koko järjestelmässä.  Tapa, jolla työmäärä on jaettu hajautettu järjestelmässä perustuu missä kunkin jaettavaksi tiedot sijaitsevat.  Tämä on tärkeää voit valita oikean jakauman jakamiseksi tiedot niin, että kunkin jakelu on yhtä kuin työtä ja siirtää sen osan työn suorittamiseen samaan aikaan.  Kun työ jaetaan myös koko järjestelmässä, tiedot on saapuva jaot.  Tietoja ei täsmätään tasaisesti, emme Soita tämän **tietojen jakauman.vinous**.  

Voit jakaa tietoja tasaisesti ja estää tietojen jakauman.vinous, harkitse seuraavien asetusten valittaessa jakauman-sarake:

1. Valitse sarake, joka sisältää useita eri arvoja.
2. Vältä jakaminen sarakkeet, joissa on muutamia eri arvojen tiedot. 
3. Vältä jakaminen sarakkeet, joissa on Null-arvot high taajuus tiedot.
4. Vältä jakaminen päivämäärä-sarakkeen tiedot.

Koska jokaisen arvon on hajautusalgoritmilla 60 jaot 1, saavuttamiseksi tasainen jako haluat valita sarakkeessa, joka on erittäin yksilöllinen ja sisältää yli 60 yksilöllisiä arvoja.  Voit havainnollistaa, harkitse kirjoitusmuotoon, johon sarake on vain 40 yksilöllisiä arvoja.  Jos sarake on valittuna jakauman avaimeksi, taulukon tiedot Power View-40 jaot enintään 20 jaot jätä ei ole tietoja ja tekemään ei käsittely.  Vastaavasti muiden 40 jaot on vielä työn alla, jos tiedot on jaettu tasaisesti yli 60 jaot.  Tässä skenaariossa on esimerkki jakauman.vinous tiedot.

MPP-järjestelmään jokainen kyselyvaihe odottaa jaot niiden Jaa työn suorittamiseen.  Jos yksi jakauman jokaisen enemmän työtä kuin muiden, sitten muut jaot resurssin ovat olennaisesti siis vain odotetaan varattu jakauman.  Kun työ on ei jaettu tasaisesti yli jaot, emme Soita tämän **käsittelyn jakauman.vinous-funktio**.  Käsittely jakauman.vinous-funktio aiheuttaa kyselyjä hitaammin kuin jos työmäärää voit tasaisesti jakamisen jaot yli.  Tietojen jakauman.vinous johtaa jakauman.vinous käsittely.

Vältä jakaminen erittäin nullable saraketta kuin null-arvoja, maa-saman jakauman. Jakaminen päivämäärä-sarakkeen heikentää myös käsittely jakauman.vinous-funktio, koska kaikki tiedot tietyn päivämäärän Power View-saman jakauman. Jos useiden käyttäjien suoritetaan kyselyjen kaikki suodattimet samana päivänä sitten 60 jaot vain 1 tekevät kaikki tietyn päivämäärän jälkeen työ on vain yksi jakamisesta. Tässä skenaariossa kyselyt suoritetaan todennäköisesti 60 kertaa hitaammin Jos tiedot on tasaisesti levitä kaikki jaot. 

Kun hyvällä perusavaimella ei ole sarakkeita, kannattaa käyttämällä PYÖRISTÄ-funktiota Mikko jakauma.

### <a name="select-distribution-column-which-will-minimize-data-movement"></a>Valitse jakeluryhmät sarakkeeseen, joka minimoi tietojen siirto

Pienentäminen tietojen siirto valitsemalla oikean jakauman-sarakkeessa on yksi tärkeimmät strategioita SQL-tietovarasto suorituskyvyn optimoiminen.  Tietojen siirto syntyy yleisimmin, kun taulukot on liitetty tai koosteet suoritetaan.  Sarakkeiden käytetään `JOIN`, `GROUP BY`, `DISTINCT`, `OVER` ja `HAVING` kaikki Tee **hyvä** lauseet hash-jakauman ehdokkaiden. 

Toisaalta, sarakkeet `WHERE` lauseen Älä **ei** Tee hyvä hash sarakkeen ehdokkaille ne rajoittaa mitä jaot tällaisessa kyselyssä, koska ilmenneet käsittely jakauman.vinous.  Hyvä esimerkki sarakkeeseen, joka voi olla aiheuttajalta jakelua, mutta usein heikentää jakauman.vinous tämän käsittely on date-sarake.

Niinkään Jos sinulla on kaksi suuri kertoma liittyvät taulukot yhdistävää usein liitoksen, saat useimmat suorituskyvyn jakamalla johonkin liitettävien sarakkeiden kummassakin taulukossa.  Jos käytössäsi on taulukko, jota ei ole liitetty toiseen suuri faktataulukon, katso usein tekstimuodossa olevat sarakkeet `GROUP BY` lause.

On joitakin keskeisiä perusteet, jotka on täytyttävä voit välttää tietojen siirto liitoksen aikana:

1. Taulukot yhdistävää liitoksen on oltava hash jakaa **yhden** liitoksen osallistuminen sarakkeet.
2. Liitettävien sarakkeiden tietotyyppien on oltava samat molempien taulukoiden välille.
3. Sarakkeet on liitetty yhtä suuri kuin-operaattoria.
4. Liitoksen lajin ei ehkä ole `CROSS JOIN`.


## <a name="troubleshooting-data-skew"></a>Tietojen jakauman.vinous vianmääritys

Kun taulukkotiedot jaetaan hash-jakauman menetelmällä ei mahdollisuutta, että jotkin jaot Vino on muiden säilyttämättä mittasuhteita enemmän tietoja. Liikaa tietoja jakauman.vinous-funktio voivat vaikuttaa kyselyn suorituskykyä, koska lopputuloksen jaettu kysely on odotettava pisimmän käynnissä jakauman loppuun. Tietoja jakauman.vinous-funktio aste mukaan voit joutua osoite.

### <a name="identifying-skew"></a>Tunnistaa jakauman.vinous-funktio

On helppo tunnistaa taulukon, kuten Vino on käyttää `DBCC PDW_SHOWSPACEUSED`.  Tämä on hyvin nopeasti ja yksinkertaista taulukkorivit, jotka on tallennettu eri tietokannan 60 jaot määrä voi.  Muista, että viimeksi tasapainoinen suorituskyvyn, eri aikajaksoille taulukon rivit olisi jakamisen tasaisesti kaikki jaot yli.

```sql
-- Find data skew for a distributed table
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Jos testaat Azure SQL-tietovarasto dynaaminen hallinta näkymät (DMV) voit suorittaa tarkempaa analyysia.  Alkavan, Luo käyttäminen [Taulukon yleistä][yleiskuvaus] artikkelista SQL näkymän [dbo.vTableSizes][] näkymä.  Kun näkymä on luotu, suorita tämä kysely tunnistaa, mitä taulukoita on yli 10 prosenttia tietojen jakauman.vinous-funktio.

```sql
select *
from dbo.vTableSizes 
where two_part_name in 
    (
    select two_part_name
    from dbo.vTableSizes
    where row_count > 0
    group by two_part_name
    having min(row_count * 1.000)/max(row_count * 1.000) > .10
    )
order by two_part_name, row_count
;
```

### <a name="resolving-data-skew"></a>Tietojen jakauman.vinous ratkaiseminen

Ei kaikki jakauman.vinous on tarpeeksi takaa korjaa.  Jos merkittävämpiä kuin joissakin tapauksissa jotkin kyselyt taulukon suorituskyky voi aiheutuvat tietojen jakauman.vinous haittaa.  Voit päättää, jos korjaa tiedot jakauman.vinous taulukon pitäisi ymmärrät mahdollisimman paljon tietomääriä ja kyselyistäsi-havainnollistamiseen.   Yksi tapa tarkastella vaikutus jakauman.vinous-funktio on valvoa jakauman.vinous-funktio kyselyn suorituskykyä ja erityisesti keston kyselyihin vaikutus kestää valmiina yksittäisiä jaot vaikutus [Kyselyn seuranta][] -artikkelin ohjeiden avulla.

Tietojen jakaminen on etsiminen oikea tasapaino pienentäminen tietojen jakauman.vinous-funktio ja pienentäminen tietojen siirto. Nämä voit Vastakkaiset tavoitteet ja joskus kannattaa pitää tiedot jakauman.vinous tietojen siirto vähentämiseksi. Esimerkiksi kun jakauman sarake on usein liitokset ja koosteet jaetun sarakkeessa, sinun voidaan pienentäminen tietojen siirto. Etuna on mahdollisimman vähän tietojen siirto saattaa merkittävämpiä kuin aiheutuvat vaikutus jakauman.vinous tiedot. 

Voit ratkaista tietojen jakauman.vinous-funktio tyypillinen tapa on luoda uudelleen eri jakauman sarakkeen taulukkoon. Vaikka tällä ei voi muuttaa aiemmin luotuun taulukkoon, voi muuttaa taulukon jakautumisen jakauman-sarakkeessa se uudelleen [CTAS] [].  Seuraavassa on kaksi esimerkkejä tietojen jakauman.vinous ratkaiseminen:

### <a name="example-1-re-create-the-table-with-a-new-distribution-column"></a>Esimerkki 1: Luo taulukko, jossa uusi jakauman sarake uudelleen

Tässä esimerkissä käytetään uudelleenluomiseen taulukko, jossa on eri hash jakauman sarakkeen [CTAS] []. 

```sql
CREATE TABLE [dbo].[FactInternetSales_CustomerKey] 
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  HASH([CustomerKey])
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_CustomerKey')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_CustomerKey] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_CustomerKey] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_CustomerKey] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_CustomerKey] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_CustomerKey] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_CustomerKey] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_CustomerKey] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_CustomerKey] ([SalesAmount]);

--Rename the tables
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_ProductKey];
RENAME OBJECT [dbo].[FactInternetSales_CustomerKey] TO [FactInternetSales];
```

### <a name="example-2-re-create-the-table-using-round-robin-distribution"></a>Esimerkki 2: Luo uudelleen taulukon käyttämällä PYÖRISTÄ-funktiota Mikko.

Tässä esimerkissä käytetään [CTAS] [] Luo uudelleen taulukon, jossa on PYÖRISTÄ-funktiota Mikko sijaan hash-jakauman. Tämä muutos tuottaa myös tietojen jakelu kustannuksella parannettu tietojen siirto. 

```sql
CREATE TABLE [dbo].[FactInternetSales_ROUND_ROBIN] 
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  ROUND_ROBIN
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_ROUND_ROBIN')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_ROUND_ROBIN] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_ROUND_ROBIN] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_ROUND_ROBIN] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_ROUND_ROBIN] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_ROUND_ROBIN] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_ROUND_ROBIN] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_ROUND_ROBIN] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_ROUND_ROBIN] ([SalesAmount]);

--Rename the tables
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_HASH];
RENAME OBJECT [dbo].[FactInternetSales_ROUND_ROBIN] TO [FactInternetSales];
```

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja taulukon rakennenäkymä-kohdassa [välit][], [indeksi][], [osio][], [Tietotyyppejä][], [tilastoja][] ja [Väliaikaisia taulukoita][tilapäinen] artikkelit.

Yleiskuvaus parhaista käytännöistä on artikkelissa [SQL Data Warehouse parhaat käytännöt][].


<!--Image references-->

<!--Article references-->
[Yleiskatsaus]: ./sql-data-warehouse-tables-overview.md
[Tietotyypit]: ./sql-data-warehouse-tables-data-types.md
[Jaa]: ./sql-data-warehouse-tables-distribute.md
[Indeksi]: ./sql-data-warehouse-tables-index.md
[Osion]: ./sql-data-warehouse-tables-partition.md
[Tilastotiedot]: ./sql-data-warehouse-tables-statistics.md
[Tilapäinen]: ./sql-data-warehouse-tables-temporary.md
[SQL Data Warehouse parhaat käytännöt]: ./sql-data-warehouse-best-practices.md
[Kyselyn seuranta]: ./sql-data-warehouse-manage-monitor.md
[dbo.vTableSizes]: ./sql-data-warehouse-tables-overview.md#querying-table-sizes

<!--MSDN references-->
[DBCC PDW_SHOWSPACEUSED()]: https://msdn.microsoft.com/library/mt204028.aspx

<!--Other Web references-->
