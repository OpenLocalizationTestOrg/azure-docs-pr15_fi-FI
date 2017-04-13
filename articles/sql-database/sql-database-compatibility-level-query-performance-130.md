<properties
    pageTitle="Yhteensopivuuden tasolla, miten voit arvioida | Microsoft Azure"
    description="Ohjeita ja tarkistamalla, mitä yhteensopivuuden tasoa parhaiten Azure SQL-tietokanta tai Microsoft SQL Server-tietokannan Työkalut"
    services="sql-database"
    documentationCenter=""
    authors="alainlissoir"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.devlang="NA"
    ms.tgt_pltfrm="NA"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="alainl"/>


# <a name="improved-query-performance-with-compatibility-level-130-in-azure-sql-database"></a>Kyselyn suorituskykyä entistä yhteensopivuuden tason 130 Azure SQL-tietokantaan


Azure SQL-tietokanta on käynnissä läpinäkyvä tietokantojen tuhansia satoja useita eri yhteensopivuuden tasojen säilyttäminen ja taata vastaavan Microsoft SQL Server-versioon yhteensopivuus kaikkien asiakkaiden!

Tämän vuoksi mitään estää asiakkaat, joiden muuttaa minkä tahansa aiemmin luotujen tietokantojen uusimman yhteensopivuuden tasolle hyötymästä uuden kyselyn optimointitoiminnon ja kyselyn suoritin toiminnot. Muistutukseksi historia SQL versioiden yhteensopivuuden Oletustasot tasaus ovat seuraavat:

- 100: SQL Server 2008 ja Azure SQL-tietokannan V11.
- 110: SQL Server 2012 ja Azure SQL-tietokannan V11.
- 120: SQL Server 2014 ja Azure SQL-tietokannan Vipuventtiili V12.
- 130: SQL Server 2016 ja Azure SQL-tietokannan Vipuventtiili V12.


> [AZURE.IMPORTANT] **Vuoden 2016**Azure SQL-tietokanta käynnistetään yhteensopivuuden oletustaso on 130 120 **vastaluotu** tietokantojen sijaan.
> 
> Ennen vuoden 2016 luotujen tietokantojen tulee *ei* vaikuta ja ylläpitää niiden yhteensopivuuden tasoa (100, 110 tai 120). Tietokannoissa, jotka Azure SQL-tietokanta-versiosta V11 Vipuventtiili V12 ei ole niiden yhteensopivuuden tason siirtäminen muuttaa joko.


Tässä artikkelissa tutustumme edut yhteensopivuuden tason 130 ja miten voit hyödyntää näitä etuja. Olemme osoitteen mahdolliset puoli-vaikutukset kyselyn suorituskykyä aiemmin SQL-sovelluksissa.


## <a name="about-compatibility-level-130"></a>Tietoja yhteensopivuudesta tason 130


Ensimmäisen kerran, jos haluat tietää tietokannan yhteensopivuuden tasoa, suorita seuraava Transact-SQL-lause.


```
SELECT compatibility_level
    FROM sys.databases
    WHERE name = '<YOUR DATABASE_NAME>’;
```


Ennen tämän muutoksen tasolle 130 tapahtuu **juuri** luonut tietokantoja, oletetaan, että Tarkista mitä tämä muutos on esimerkkejä hyvin basic kyselyn kautta ja katso, miten kuka tahansa voi hyötyä siitä.

Kyselyn käsittely-relaatiotietokannat voivat olla hyvin monimutkaisia ja voi aiheuttaa paljon tietokoneen tiede- ja matematiikan ymmärtämään luontaisen rakenteen valinnat ja toiminnan. Tämän asiakirjan sisällön on tarkoituksella yksinkertaistettu varmistaaksesi, että kaikki, joilla on joitakin pienin tekniset taustan ymmärtää yhteensopivuuden tason muuttaminen ja määrittää, miten se voi olla hyötyä sovellukset.

Katsotaanpa lyhyesti yhteensopivuuden tason 130 tuo taulukkoa.  Voit etsiä lisätietoja [muuttaa TIETOKANNAN yhteensopivuuden](https://msdn.microsoft.com/library/bb510680.aspx)tasolla (Transact-SQL), mutta seuraavassa on lyhyt yhteenveto:

- Lisää toiminto lisää select-lauseen voi olla Salli muut samanaikaiset tai valita rinnakkain-palvelupaketti, ennen kuin tämä toiminto on yksi threaded aikana.
- Muistin optimoitu taulukko ja taulukon muuttujat kyselyiden nyt voi olla rinnakkain suunnitelmat, mutta ennen kuin tämä toiminto on myös single-threaded.
- Muistin optimoitu taulukon tilastoja voi nyt näyte, ja ne ovat automaattinen päivitetään. Katso [uusiin Database Engine: ladatun OLTP](https://msdn.microsoft.com/library/bb510411.aspx#InMemory) lisätietoja.
- Erän tilassa v/s rivin tilan muutokset sarakkeen indeksien
  - Taulukon sarakkeen kaupan indeksi lajittelee nyt erä-tilassa.
  - Erän-tilassa, kuten TSQL VIIVEEN ja LIMITYSAJAN ajatuksia toimivat nyt Windowing koosteet.
  - Sarakkeen kaupan taulukoita, joissa on useita eri lausekkeita kyselyjä toimia erä-tilassa.
  - DOP suoritettavat kyselyt = 1 tai ja serial palvelupaketti myös suorittaa Siirtoerän-tilassa.
- Lopuksi kardinaliteetti arvio parannuksia ovat todella tulossa yhteensopivuuden viereiset 120, mutta käyttäjille, että sinä käynnissä alemmalla tasolla yhteensopivuuden eli 100, eli 110, yhteensopivuutta siirtäminen tason 130 myös tuoda parannuksia ja nämä etuja sovellusten kyselyn suorituskykyä.


## <a name="practicing-compatibility-level-130"></a>Jossa yhteensopivuuden tason 130 Harjoitus


Ensimmäinen aloitetaan joitakin taulukoita, indeksit ja satunnaisia tietoja, jotka on luotu kokeilemalla seuraavia näitä uusia ominaisuuksia. Esimerkkejä TSQL komentosarjan voi suorittaa SQL Server 2016- tai Azure SQL-tietokantaan. Kuitenkin luodessasi Azure SQL-tietokantaan, varmista, että valitset vähintään P2 tietokannan koska siihen tarvitaan vähintään pari sydämiä, jotta monisäikeisyys ja näin ollen hyötyä nämä ominaisuudet.


```
-- Create a Premium P2 Database in Azure SQL Database

CREATE DATABASE MyTestDB
    (EDITION=’Premium’, SERVICE_OBJECTIVE=’P2′);
GO

-- Create 2 tables with a column store index on
-- the second one (only available on Premium databases)

CREATE TABLE T_source
    (Color varchar(10), c1 bigint, c2 bigint);

CREATE TABLE T_target
    (c1 bigint, c2 bigint);

CREATE CLUSTERED COLUMNSTORE INDEX CCI ON T_target;
GO

-- Insert few rows.

INSERT T_source VALUES
    (‘Blue’, RAND() * 100000, RAND() * 100000),
    (‘Yellow’, RAND() * 100000, RAND() * 100000),
    (‘Red’, RAND() * 100000, RAND() * 100000),
    (‘Green’, RAND() * 100000, RAND() * 100000),
    (‘Black’, RAND() * 100000, RAND() * 100000);

GO 200

INSERT T_source SELECT * FROM T_source;

GO 10
```


Nyt Katsotaanpa ikkunan toimintoja joitakin tulossa yhteensopivuuden viereiset 130 kyselyn käsittely-ominaisuuksia.


## <a name="parallel-insert"></a>Rinnakkaisia Lisää


Suoritetaan alla TSQL lauseet suorittaa yhteensopivuuden tasolle, 120 ja 130, joka suorittaa INSERT INTO vastaavasti yhden ketjutettu mallin (120) ja Salli muut samanaikaiset mallin (130)-kohdassa Lisää-toimintoa.


```
-- Parallel INSERT … SELECT … in heap or CCI
-- is available under 130 only

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO 

-- The INSERT part is in serial

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130
GO

-- The INSERT part is in parallel

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

SET STATISTICS XML OFF;
```


Pyytämällä todellisen kysely-palvelupaketti, joka katsoo sen graafinen esitys tai sen XML-sisällön selviää mitä kardinaliteetti arvio funktio on toista. Katsoo suunnitelmat – rinnakkais-kuva 1, selkeästi näemme, että sarakkeen kaupan Lisää suorittamisen siirtyy-serial 120 rinnakkain-130. Huomaa, että muuta 130-suunnitelmassa näkyy kaksi rinnakkaisia nuolet kuvaavat siitä, että nyt kyselymerkkijonon suorittamisen kyselymerkkijonon-kuvake on varmasti rinnakkaisia. Jos sinulla on suuri Lisää toimintojen suorittamiseen, rinnakkain suorittamisen määrään core tietokannalle, käytettävissä olevalla on linkitetty paranee; enintään 100 kertaa, joka on nopeampaa tilanteesi!


*Kuva 1: Lisää toiminto vaihtuu serial rinnakkain kanssa yhteensopivuuden tason 130.*


![Kuva 1](./media/sql-database-compatibility-level-query-performance-130/figure-1.jpg)


## <a name="serial-batch-mode"></a>SARJA erän tila


Vastaavasti siirtäminen yhteensopivuuden tasolle 130 tietoriviä käsiteltäessä mahdollistaa eräkäsittely tilassa. Ensin erä tilassa toiminnot ovat käytettävissä vain silloin, kun sarakkeen store-indeksi on sijainti. Se erän edustaa yleensä noin 900 rivit- ja käyttää multicore suorittimen suurempi muistin siirtonopeuden optimoitu koodi-logiikkaa ja hyödyntää suoraan pakattujen tietojen sarake-säilön aina, kun se on mahdollista. Näissä tilanteissa SQL Server 2016 käsitellä samalla kertaa noin 900 rivien sijaan 1 aikaa, riviä ja seurauksena toiminnon yleistä yhteen kustannus on nyt jaettu koko erä pienentämisestä kustannusten riveittäin yleinen. Tämän jaetun summan toimintojen yhdistetty sarake kaupan pakkaamisen lähinnä vähentää yhdistävää, valitse poistettavat tila-toiminto viive. Voit etsiä lisätietoja sarakkeen myymälän ja erä [Columnstore indeksit opas](https://msdn.microsoft.com/library/gg492088.aspx)tilassa.


```
-- Serial batch mode execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- The scan and aggregate are in row mode

SELECT C1, COUNT (C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO 

– The scan and aggregate are in batch mode,
-- and force MAXDOP to 1 to show that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Sellaisena kuin se on näkyvissä alla tarkkailemalla kyselyn suunnitelmien rinnakkais, kuva 2, emme voi kohdassa, että käsittelytila on muuttunut yhteensopivuuden taso ja näin ollen suoritettaessa kyselyjä sekä yhteensopivuuden tasolla kokonaan, näemme, että yleensä käsittely jakautuu verrattuna erä tila (14 %), jossa 2 erissä käsitelty tilassa rivi (86 %). Suurenna tietojoukon, etu kasvaa.


*Kuva 2: Valitse toimintojen muutokset serial erä tilaan yhteensopivuuden tason 130 avulla.*


![Kuva 2](./media/sql-database-compatibility-level-query-performance-130/figure-2.jpg)


## <a name="batch-mode-on-sort-execution"></a>Lajittele suorittamisen erä-tilassa


Samalla tavalla kuin edellä, mutta lajittelutoiminto käytössä erä tila (yhteensopivuuden taso 130) rivin tila (yhteensopivuuden taso 120) siirtymistä parantaa suorituskykyä Lajittele samoista syistä.


```
-- Batch mode on sort execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- The scan and aggregate are in row mode

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

-- The scan and aggregate are in batch mode,
-- and force MAXDOP to 1 to show that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Näkyvissä-rinnakkais-kuva 3, on artikkelissa vastaavan rivin tilassa Lajittele 81 prosenttia, kun erä tila on vain 19 % kustannusten (81 % ja Lajittele itse 56 %).


*Kuva 3: Lajittelu muuttaa rivin erä tilaan yhteensopivuuden tason 130 avulla.*


![Kuva 3](./media/sql-database-compatibility-level-query-performance-130/figure-3.png)


Selvästi näitä esimerkkejä sisältää vain kymmeniä tuhansia rivejä, joka on mitään kun katsoo käytettävissä useimmissa SQL Server data nämä päivää. Projektin nämä vastaan miljoonia rivejä vain sen sijaan ja tämä voi kääntää spared päivittäin odottavat havainnollistamiseen laatu suorittamisen muutaman minuutin.


## <a name="cardinality-estimation-ce-improvements"></a>Kardinaliteetti arvio (CE) parannukset


SQL Server 2014 otettu käyttöön, minkä tahansa tietokannan yhteensopivuuden tasolla 120 tai yläpuolella tekee arvioinnin kardinaliteetti käyttää toimintoja. Kardinaliteetti arviointi on logiikan avulla määritetään, miten sen arvioidut kustannukset perustuva kysely suoritetaan SQL server. Arvio ohjelma laskee syötettä osallistuvan kysely objekteihin liittyvä tilastotiedot. Käytännössä, milloin ylätason, kardinaliteetti arvio Funktiot ovat rivin Laske arvioita arvoja eri arvo laskee jakautumisen tiedot ja kaksoiskappaleiden laskee sisältyy taulukot ja kysely viittaa objekteja. Käytön näiden arvioiden väärä voi aiheuttaa tarpeettomat levyn i/o (eli TempDB valuu) muisti ei riitä myöntää vuoksi tai serial suunnitelman suorittamisen valikoiman päälle rinnakkaisia suunnitelma-suorittamisen muun muassa. On tehty, virheellinen arvioiden voi aiheuttaa yleinen kansiohierarkiassa kyselyn suorittamisen. Puolille parempi arvioiden tarkempi arvioiden johtaa paremmin kyselyn suorituskerran!

Mainittu ennen kyselyn optimointi ja arvioita ovat monimutkaisia tärkeää, mutta jos haluat lisätietoja kyselyn suunnitelmien ja kardinaliteetti arviointi, voit katsoa asiakirjaan [Ja SQL Server 2014 kardinaliteetti arviointi optimointi oma kysely-suunnitelmien](https://msdn.microsoft.com/library/dn673537.aspx) tarkempaa perinpohjaisesti käsittelevään artikkeliin varten.


## <a name="which-cardinality-estimation-do-you-currently-use"></a>Mitä kardinaliteetti arvio parhaillaan käytät?


Voit määrittää, mitkä kardinaliteetti arvio kyselyissä on käytössä, valitse vain käytetään alla kyselyn mallit. Huomaa, että ensimmäinen tässä esimerkissä suoritetaan yhteensopivuuden tason 110, mikä vanha kardinaliteetti arvio-funktioiden käyttäminen.


```
-- Old CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 110;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


Kun suorittamisen on valmis, XML-linkkiä ja tarkastella ensimmäisen kyselymerkkijonon ominaisuudet alla kuvatulla tavalla. Huomautus kutsutaan CardinalityEstimationModelVersion määritettynä 70 ominaisuuden nimi. Se ei tarkoita tietokannan yhteensopivuuden taso on määritetty (se on määritetty, muodossa näkyvät yllä TSQL lauseet 110) SQL Server 7.0-versioon, mutta arvo 70 vastaa vain SQL Server 7.0, joka ei ole tärkeimmät muutokset oli ennen SQL Server-2014 (joka sisältää 120 yhteensopivuuden taso) jälkeen vanha kardinaliteetti arvio toimintoja.


*Kuva 4: CardinalityEstimationModelVersion on määritetty 70 käytettäessä yhteensopivuuden tason 110- tai alapuolelle.*


![Kuva 4](./media/sql-database-compatibility-level-query-performance-130/figure-4.png)


Voit myös muuttaa yhteensopivuus-tason 130 ja käytöstä uuden kardinaliteetti arvio-funktion avulla LEGACY_CARDINALITY_ESTIMATION edelleen [muuttaa TIETOKANNAN VAIKUTUSALUE](https://msdn.microsoft.com/library/mt629158.aspx)kokoonpanon määrittäminen. Tämä on täysin sama kuin käyttämällä 110 kardinaliteetti arvio funktion näkökulmasta, periytymään viimeisimmän kyselyn käsittelyn yhteensopivuuden taso. Näin voit hyötyä käsittelyn ominaisuuksia uusimman yhteensopivuuden viereiset (eli erä tilan) uuden kyselyn mutta riippuvaisia edelleen vanha kardinaliteetti arvio toimintoja tarvittaessa.


```
-- Old CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = ON;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


Yhteensopivuuden tasoa 120 tai 130 yksinkertaisesti siirtäminen ottaa käyttöön uuden kardinaliteetti arvio-toimintoja. Siinä tapauksessa, oletusarvo CardinalityEstimationModelVersion määritetään vastaavasti 120 tai 130 muodossa näkyvät jäljempänä.


```
-- New CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = OFF;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


*Kuva 5: CardinalityEstimationModelVersion on määritetty 130 käytettäessä 130 yhteensopivuuden taso.*


![Kuva 5](./media/sql-database-compatibility-level-query-performance-130/figure-5.jpg)


## <a name="witnessing-the-cardinality-estimation-differences"></a>Witnessing kardinaliteetti arvio erot


Nyt oletetaan, että Suorita hieman enemmän kompleksiluvun kyselyn henkilöihin liittyvät INNER JOIN ja WHERE-lauseen joitakin predikaatit kanssa ja katsotaan rivin Laske arvio vanha kardinaliteetti arvio-funktion ensimmäisen kerran.


```
-- Old CE row estimate with INNER JOIN and WHERE clause

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = ON;
GO

SET STATISTICS XML ON;

SELECT T.[c2]
    FROM
                   [dbo].[T_source] S
        INNER JOIN [dbo].[T_target] T  ON T.c1=S.c1
    WHERE
        S.[Color] = ‘Red’  AND
        S.[c2] > 2000  AND
        T.[c2] > 2000
    OPTION (RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Tämä kysely suoritetaan tehokkaasti palauttaa 200,704 rivit, kun rivin arvio vanha kardinaliteetti arvio toimintojen väittää 194,284 rivit. Selvästi said ennen, kuin rivien määrä tuloksia myös sen mukaan, kuinka usein suoritit edellisen esimerkin, joka täyttää katkeamattomana toistona jokaisen osoitteessa taulukoita. Selvästi predikaatit kyselyssä on valmisteltu todellinen arvio lukuun ottamatta pöydän muoto-sisällön tiedot vaikuttavat ja miten tiedoista todella yhdistää toistensa kanssa.


*Kuva 6: Rivin Laske arvio on 194,284 tai 6,000 rivit käytöstä 200,704 riveiltä oikein.*


![Kuva 6](./media/sql-database-compatibility-level-query-performance-130/figure-6.jpg)


Samalla tavalla katsotaan nyt suorittaa saman kyselyn uusi kardinaliteetti arvio toimintojen.


```
-- New CE row estimate with INNER JOIN and WHERE clause

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = OFF;
GO

SET STATISTICS XML ON;

SELECT T.[c2]
    FROM
                   [dbo].[T_source] S
        INNER JOIN [dbo].[T_target] T  ON T.c1=S.c1
    WHERE
        S.[Color] = ‘Red’  AND
        S.[c2] > 2000  AND
        T.[c2] > 2000
    OPTION (RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Katsoo alla, nyt näemme rivin arvio on 202,877, tai paljon tarkempi ja suurempi kuin vanha kardinaliteetti arvio.

*Kuva 7: Rivien laskeminen arvio on nyt 202,877 194,284 sijaan.*


![Kuva 7](./media/sql-database-compatibility-level-query-performance-130/figure-7.jpg)


Todellisuudessa tulosjoukko on 200,704 rivit (mutta ne sen mukaan, kuinka usein edellisen esimerkin kyselyjen suoritettu, mutta, koska TSQL käyttää Funktiot RAND()-lauseen, palautetaan todellisten arvojen voi erota yhden Suorita toiseen). Tässä esimerkissä uuden kardinaliteetti arvio onko vuoksi paremmin työn etsiminen arviointi rivien määrän, koska 202,877 on paljon lähempänä 200,704, kuin 194,284! Sukunimi, jos haluat muuttaa WHERE-lauseen-predikaatit tasa (sijaan ">" esiintymän), tämä tehdä välillä vanhan arvioiden ja uutta entistä erilaiset riippuen siitä, kuinka monta vastaa kardinaliteetti-funktiota voi käyttää.

Selvästi tässä tapauksessa on noin 6000 rivit käytöstä-todellinen määrä ei vastaa paljon tietoja tietyissä tilanteissa. Nyt Transponoi tämä miljoonia rivejä useista taulukoista ja monimutkaisia kyselyjä ja joskus arviota, voi olla käytöstä miljoonia rivejä, ja siksi keräys ylöspäin väärä suorittamisen suunnitelma tai pyytää muisti ei riitä myöntää TempDB valuu ja jotta i/o, jotka on paljon suurempi.

Jos sinulla on mahdollisuus harjoituksen yleisimmät kyselyt ja tietojoukkoja, vertailu ja kokeile itse mukaan, kuinka suuri osa vanhat ja uudet arvioiden vaikuttavat samalla, kun joitakin saattoi olla vain enemmän käytöstä todellisuudessa irti tai joitakin muiden vain yksinkertaisesti lähemmäksi todellinen rivin laskee todella palautettujen tulosten joukkoja. Kaikki nämä vaihtelevat muodon kyselyissä, Azure SQL-tietokannan ominaisuuksien, laatu ja koko oman tietojoukkoja ja käytettävissä heistä tilastotiedot. Jos juuri luomasi Azure SQL-tietokanta-esiintymä-kyselyn optimointitoiminnon on luominen alusta alkaen sijaan uudelleen edellisen kysely suoritetaan tehtyä tilastotiedot sen knowledge. Arvioiden on hyvin tilannekohtaiset ja lähes tiettyä-palvelimen nimen ja sovelluksen kaikissa tilanteissa. Se on olennainen osa projektinhallintaa kannattaa pitää mielessä!


## <a name="some-considerations-to-take-into-account"></a>Seikkoja, jotka on otettava huomioon


Vaikka useimmat työmääriä hyötyä yhteensopivuuden tason 130, ennen kuin voit päättää tuotantoympäristössä-yhteensopivuus-tason sinulla lähinnä 3 vaihtoehtoja:

1. Siirtää yhteensopivuuden tasolle 130 ja katso, kuinka asiat suorittaa. Siltä varalta, huomaat, että jotkin muutoksista, voit vain yksinkertaisesti Määritä yhteensopivuusasetukset tason takaisin sen alkuperäiseen tasoa tai Säilytä 130 ja vain Käänteinen kardinaliteetti arvio takaisin vanha tila (edellä kuvatulla tämä yksin voitu ratkaisemaan).
2. Testaa perusteellisesti samalla tuotannon kuormitus sovellukset, hienosäätää ja Vahvista ennen kuin siirryt tuotannon suorituskykyä. Jos ongelmista on sama kuin yllä, voit palata alkuperäiseen yhteensopivuuden taso ja tai yksinkertaisesti palauttaa kardinaliteetti arvio vanha tila.
3. Lopullinen vaihtoehto ja viimeisimmän tapa osoite näitä kysymyksiä on hyödyntää kysely-kaupasta. Tämä on tämän päivän suositella! Voit helpottaa kyselyissä yhteensopivuus-kohdassa analysis tason 120 tai alla ja 130, on et voi suositeltavaa tarpeeksi käyttää kyselyn kauppaa. Kyselyn säilö on käytettävissä Azure SQL-tietokannan Vipuventtiili V12 uusimman version, ja se on suunniteltu helpottamaan kyselyn suorituskyvyn vianmääritykseen. Ajattele kyselyn säilön lentojen tietojen tallennin tietokannan kerääminen ja esittäminen kaikki kyselyt historiallisten yksityiskohtaisia tietoja. Tämä yksinkertaistaa merkittävästi suorituskyvyn forensics vähentämällä ajan ja ratkaise ongelmat. Voit etsiä lisätietoja osoitteessa [kyselyn kaupan: lentojen tietojen tallennin tietokannan](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/).


AT ylätason, jos olet jo joukon yhteensopivuuden tasolla 120 tai alla-tietokantojen ja aiot siirtää jotkin niistä 130 tai koska havainnollistamiseen valmistella automaattisesti uusille tietokannoille, jotka ovat pian voi määrittää oletusarvoisesti 130, ota huomioon kun se on julkaistu:

- Ennen kuin muutat uuden yhteensopivuuden tasolle tuotannon, ota käyttöön kyselyn säilö. Voit viitata [tietokannan yhteensopivuustila](https://msdn.microsoft.com/library/bb895281.aspx) ja käyttää kyselyn kauppaa lisätietoja.
- Testaa seuraavaksi kaikki tärkeät toiminnoista, jotka edustavat tietojen ja kyselyjen tuotannon kaltaisessa ympäristön ja vertaa listan suorituskykyä ja raportoidun kyselyn myymälän mukaan. Jos kohtaat joitakin muutoksista, voit tunnistaa kyselyn kaupan regressed kyselyt ja käyttää pakottaminen vaihtoehto kyselyn kaupasta suunnitelman (eli palvelupaketti kiinnittäminen). Siinä tapauksessa voit lopullisesti yhteensopivuuden tason 130 pysy ja käyttää entisen kyselyn suunnitelman ehdotettu kyselyn säilön.
- Jos haluat hyödyntää uusia ominaisuuksia ja toimintoja Azure SQL-tietokanta (joka toimii SQL Server 2016), mutta merkitsevä muutosten mukaan yhteensopivuuden tasolle 130, ei auta, voi kannattaa pakottaminen yhteensopivuus-tason takaisin tason se sopii havainnollistamiseen käyttämällä ALTER DATABASE-lause. Mutta huomioon ensin, että kyselyn kaupan suunnitelman kiinnittäminen vaihtoehto on paras vaihtoehto, koska käytetä 130 lähinnä jäävät vanhemman SQL Server-version toimintoja tasolla.
- Jos sinulla on multitenant sovellukset, jotka kestävät useiden tietokantojen, se saattaa olla tarpeen päivittää tietokantoja yhdenmukaisia yhteensopivuus-tason varmistamiseksi kaikki tietokannat; yli valmistelu logiikan vanhat ja uudet valmistellun niistä. Sovelluksen työmäärää suorituskyvyn voi olla luottamuksellisia siitä, että jotkin tietokannat käynnissä olevat yhteensopivuuden eri tasoilla ja siksi yhteensopivuuden tason yhdenmukaisuuden minkä tahansa tietokannan välillä voi olla edellyttää, jotta voit tarjota asiakkaillesi saman käyttökokemuksen koko tauluun. Huomaa, että se ei ole valtuutusta, riippuu siitä, miten sovelluksesi vaikuttaa yhteensopivuus-tason todella.
- Lopuksi koskeva kardinaliteetti arvio ja tavoin muuttaminen yhteensopivuuden, ennen kuin jatkat tuotannon, on suositeltavaa Testaa uuden olosuhteissa määrittääksesi, jos sovellus hyötyä kardinaliteetti arvio parannuksista tuotannon havainnollistamiseen.


## <a name="conclusion"></a>Tekemistä


Kaikki SQL Server 2016 parannukset hyötyvät Azure SQL-tietokannan avulla voit parantaa kyselyn suorituskerran selkeästi. Kuten-on! Uusi ominaisuus, kuten oikea arviointi on tehtävä määrittää tarkkojen ehtojen vallitessa tietokannan työmäärää toimii parhaiten. Kokemus näkyy useimmissa työmäärää odotetaan vähintään suoritetaan läpinäkyvä yhteensopivuuden tason 130, samalla uuden kyselyn toiminnot ja uusi kardinaliteetti arvio. Joka said onko, ovat aina poikkeuksia ja suorittamalla ERISNIMI määräpäivä kassaan on tärkeää selvittää, kuinka paljon voit hyötyä nämä parannukset arviointi. Ja uudelleen, kysely-kaupan voi olla arvokkaita ohjeita teet toimisivat!

Kuten SQL Azure kehitystyö, voit odottaa yhteensopivuuden tason 140 tulevaisuudessa. Kun tieto on aikaa, emme alkaa puhuminen siitä, mitä tulevien yhteensopivuuden tason 140 tuo mukanaan samalla tavalla kuin lyhyesti on kerrottu tähän yhteensopivuuden millaiset 130 on joka tuo tänään.

Nyt japanin unohda, kesäkuussa 2016 alkaen Azure SQL-tietokanta muuttuu yhteensopivuuden oletustaso osoitteesta 120 130 uusien tietokantojen. Huomioon!


## <a name="references"></a>Viittaukset


- [Mitä uusia ominaisuuksia tietokantamoduuli](https://msdn.microsoft.com/library/bb510411.aspx#InMemory)

- [Blogi: Kyselyn kaupan: lentojen tietojen tallennin tietokannalle Borko Novakovic 8 kesäkuussa 2016 mukaan](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/)

- [Muuta TIETOKANNAN yhteensopivuuden tason (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx)

- [MUUTA TIETOKANNAN SUODATETUT MÄÄRITYS](https://msdn.microsoft.com/library/mt629158.aspx)

- [Yhteensopivuus-tason 130 Azure SQL-tietokannan Vipuventtiili V12](https://azure.microsoft.com/updates/compatibility-level-130-for-azure-sql-database-v12/)

- [Kyselyn optimointi suunnitelmien SQL Serverin kanssa 2014 kardinaliteetti arviointi](https://msdn.microsoft.com/library/dn673537.aspx)

- [Columnstore indeksit opas](https://msdn.microsoft.com/library/gg492088.aspx)

- [Blogi: Parantaa kyselyn suorituskykyä Alain Lissoir yhteensopivuuden tasolla 130 Azure SQL-tietokantaan, 6 toukokuun 2016](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/05/06/improved-query-performance-with-compatibility-level-130-in-azure-sql-database/)



<!--
Improved Query Performance with Compatibility Level 130 in Azure SQL Database

May 6, 2016 by Alain Lissoir (AlainL), on GitHub 'alainlissoir'.

https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/05/06/improved-query-performance-with-compatibility-level-130-in-azure-sql-database/

..... Now, above.
....................
..... Soon, below?

CAPS / MSDN ideally, but instead on ACom:
.. # Assess effects of latest compatibility level on query performance, how to

sql-database-compatibility-level-query-performance-130.md

genemi = MightyPen , 2016-05-20  Friday  17:00pm
-->
