<properties
    pageTitle="Valitse rivit, voit siirtää käyttämällä suodatin-funktiota (Venytys-tietokanta) | Microsoft Azure"
    description="Lue, miten voit valita rivejä, voit siirtää suodatin-funktiolla."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/28/2016"
    ms.author="douglasl"/>

# <a name="select-rows-to-migrate-by-using-a-filter-function-stretch-database"></a>Valitse rivit, voit siirtää käyttämällä suodatin-funktiota (Venytys-tietokanta)

Jos tallennat kylmän tiedot erilliseen taulukkoon, voit määrittää Venytys tietokantaan, voit siirtää koko taulukon. Jos taulukossa on tietoja sekä kuuman ja kylmän, toisaalta, voit määrittää suodatinfunktion, valitse Siirrä rivit. Suodatin-lause on sisäinen taulukko\-funktion arvon. Tässä ohjeaiheessa kerrotaan, miten kirjoittaa tekstiin taulukon\--funktion avulla voit siirtää rivien valitseminen arvon.

>   [AZURE.NOTE] Jos annat suodatinfunktion, joka suorittaa huonosti, tietojen siirtäminen suorittaa huonosti. Venytä tietokannan koskee filter-funktiossa taulukon toimintojen käyttäminen-operaattorilla.

Jos et määritä suodatin-funktio, koko taulukko on siirretty.

Kun suoritat Venytys ohjatun ottaminen käyttöön tietokantaa, voit siirtää koko taulukon tai voit määrittää ohjatun toiminnon Perussuodatus-funktio. Jos haluat siirtää rivien valitseminen suodatinfunktion erityyppisen avulla, tee jokin seuraavista seuraavat kohdat.

-   Sulje ohjattu toiminto ja suorita ALTER TABLE-lause Venytys käyttöön taulukko ja määritä suodatin-funktio.

-   ALTER TABLE-lause Määritä filter-funktiossa, kun suljet ohjatun toiminnon suorittaminen

ALTER TABLE lisäämällä funktion syntaksi on kuvattu tämän artikkelin.

## <a name="basic-requirements-for-the-filter-function"></a>Filter-funktiossa Basic vaatimukset
Sisäinen taulukon\-tärkeät funktion vaaditaan Venytys tietokannan suodatin-lause näyttää seuraavalta.

```tsql
CREATE FUNCTION dbo.fn_stretchpredicate(@column1 datatype1, @column2 datatype2 [, ...n])
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE <predicate>
```
Funktion parametrit on oltava sarakkeet tunnukset.

Rakenteen sidonta tarvitaan estää sarakkeet, joita pudotettu tai muutettu filter-funktiossa käytetään.

### <a name="return-value"></a>Palautusarvo
Jos funktio palauttaa muun\-tyhjän tuloksen rivi on vaihdettavissa uuteen. Muussa tapauksessa \- , jos funktio ei palauta tuloksen \- rivi ei ole vaihdettavissa uuteen.

### <a name="conditions"></a>Ehdot
&lt; *Lause* &gt; voi olla yksi ehto tai looginen AND-operaattorin liitetään useita ehtoja.

```
<predicate> ::= <condition> [ AND <condition> ] [ ...n ]
```
Peräkkäisten kerta, kun voi olla yksi alkeistyyppi ehto tai useita alkeistyyppi ehdot, jotka on liitetty looginen OR-operaattoria.

```
<condition> ::= <primitive_condition> [ OR <primitive_condition> ] [ ...n ]
```

### <a name="primitive-conditions"></a>Alkukantainen ehdot
Alkukantainen ehto tekemällä jommankumman seuraavista vertailuja.

```
<primitive_condition> ::=
{
<function_parameter> <comparison_operator> constant
| <function_parameter> { IS NULL | IS NOT NULL }
| <function_parameter> IN ( constant [ ,...n ] )
}
```

-   Vertaa funktioparametri vakion lauseke. Esimerkiksi `@column1 < 1000`.

    Esimerkki, joka tarkistaa, onko *päivämäärä* -sarakkeen arvon &lt; 1/1/2016.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate(@column1 datetime)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
    GO

    ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
        FILTER_PREDICATE = dbo.fn_stretchpredicate(date),
        MIGRATION_STATE = OUTBOUND
    ) )
    ```

-   Käyttää funktioparametri on tyhjä tai NULL IS NOT-operaattoria.

-   IN-operaattorin avulla voit verrata vakion arvoluettelon funktioparametri.

    Esimerkki, joka tarkistaa, onko arvo *toimituksen\_tila* sarake on `IN (N'Completed', N'Returned', N'Cancelled')`.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate(@column1 nvarchar(15))
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 IN (N'Completed', N'Returned', N'Cancelled')
    GO

    ALTER TABLE table1 SET ( REMOTE_DATA_ARCHIVE = ON (
        FILTER_PREDICATE = dbo.fn_stretchpredicate(shipment_status),
        MIGRATION_STATE = OUTBOUND
    ) )
    ```

### <a name="comparison-operators"></a>Vertailuoperaattorit
Seuraavat vertailuoperaattorit tuetaan.

`<, <=, >, >=, =, <>, !=, !<, !>`

```
<comparison_operator> ::= { < | <= | > | >= | = | <> | != | !< | !> }
```

### <a name="constant-expressions"></a>Vakion lausekkeet
Vakiot, jota käytetään filter-funktiossa voi olla deterministic lauseke, joka voi tuottaa tulokseksi, kun haluat määrittää funktion. Vakion lausekkeiden voi olla seuraavat kohdat.

-   Literaalien. Esimerkiksi `N’abc’, 123`.

-   Algebrallinen lausekkeet. Esimerkiksi `123 + 456`.

-   Deterministic toiminnot. Esimerkiksi `SQRT(900)`.

-   Deterministic muunnokset, jotka käyttävät CAST tai Muunna. Esimerkiksi `CONVERT(datetime, '1/1/2016', 101)`.

### <a name="other-expressions"></a>Muita lausekkeita
Voit käyttää ei BETWEEN-operaattorien ja BETWEEN Jos tuloksena funktion kuvatut, kun korvaat BETWEEN ja ei BETWEEN-operaattorien käyttäminen vastaavat ja-ja tai-lausekkeiden kanssa säännöt.

Et voi käyttää alikyselyjen tai muu kuin\-deterministic Funktiot, kuten Funktiot RAND() tai GETDATE().

## <a name="add-a-filter-function-to-a-table"></a>Filter-funktiossa lisääminen taulukkoon
Filter-funktiossa lisääminen taulukkoon suorittamalla **ALTER TABLE** -lause ja riveillä aiemmin luotuun taulukkoon, joka määrittää\-funktion arvon arvona **SUODATTIMEN\_PREDIKAATIN** parametri. Esimerkki:

```tsql
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE = dbo.fn_stretchpredicate(column1, column2),
    MIGRATION_STATE = <desired_migration_state>
) )
```
Kun sidot funktio lause taulukon, seuraavat kohdat ehdot täyttyvät.

-   Seuraavan kerran tietojen siirto tapahtuu, vain ne rivit, jonka funktio palauttaa muun\-tyhjä arvo siirretään.

-   Funktion käyttämät sarakkeet ovat sidottu rakenne. Et voi muuttaa näiden sarakkeiden, kunhan taulukon käyttää funktiota sen suodatin-lause.

Et voi poistaa tekstiin taulukon\-funktion arvon, kun taulukon käyttää funktiota sen suodatin-lause.

>   [AZURE.NOTE] Filter-funktiossa suorituskyvyn parantamiseksi Luo indeksi funktion käyttämät sarakkeet.

### <a name="passing-column-names-to-the-filter-function"></a>Sarakkeiden nimet välittämistä filter-funktio
Kun määrität filter-funktiossa taulukoksi, määritä sarakkeiden nimet välitetään yksi osa nimellä filter-funktio. Jos määrität kolmiosaisen nimiä, kun sarakkeen välittää nimiä, seuraavien kyselyitä, jotka perustuvat Venytys\-käytössä taulukon epäonnistuu.

Esimerkiksi jos määrität kolmiosaisen sarakkeen nimi, kuten seuraavassa esimerkissä, lause suoritetaan onnistuneesti, mutta seuraavat kyselyt taulukossa epäonnistuu.

```tsql
ALTER TABLE SensorTelemetry
  SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE=dbo.fn_stretchpredicate(dbo.SensorTelemetry.ScanDate),
    MIGRATION_STATE = OUTBOUND )
  )
```

Määritä filter-funktio, yksi osa sarakkeen nimellä seuraavan esimerkin mukaisesti.

```tsql
ALTER TABLE SensorTelemetry
  SET ( REMOTE_DATA_ARCHIVE = ON  (
    FILTER_PREDICATE=dbo.fn_stretchpredicate(ScanDate),
    MIGRATION_STATE = OUTBOUND )
  )
```

## <a name="addafterwiz"></a>Filter-funktiossa lisäämällä ohjatun toiminnon suorittaminen  

Jos haluat käyttää tätä funktiota ei voi luoda **Venytys käyttöön tietokannan** ohjatun toiminnon, voit suorittaa Määritä funktion, kun suljet ohjatun toiminnon ALTER TABLE-lause. Ennen kuin voit käyttää funktiota, mutta sinulla lopettaa tietojen siirtäminen, joka on jo käynnissä ja takaisin näkyviin siirretyt tiedot. (Lisätietoja siitä, miksi tämä on tarpeen, katso [Korvaa aiemmin luotu filter-funktio](#replacePredicate).  

1. Siirron käänteiseen ja tuoda tiedot uuteen jo Edellinen. Et voi peruuttaa tätä toimintoa, kun se käynnistyy. Voit myös maksamaan Azure kustannuksia lähtevä tiedonsiirron, \(juniin\). Saat lisätietoja, [miten Azure hinnat toimii](https://azure.microsoft.com/pricing/details/data-transfers/).  

    ```tsql  
    ALTER TABLE <table name>  
         SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = INBOUND ) ) ;   
    ```  

2. Odota siirron loppuun. Voit tarkistaa SQL Server Management Studiossa **Venytys tietokannan näytön** tilan tai voit tehdä kyselyn **sys.dm_db_rda_migration_status** -näkymä. Saat lisätietoja, [näyttö ja vianmääritys tietojen siirtäminen](sql-server-stretch-database-monitor.md) tai [sys.dm_db_rda_migration_status](https://msdn.microsoft.com/library/dn935017.aspx).  

3. Filter-funktio, jota haluat käyttää taulukon luominen  

4. Lisää funktio-taulukkoon ja Käynnistä uudelleen Azure tietojen siirto.  

    ```tsql  
    ALTER TABLE <table name>  
        SET ( REMOTE_DATA_ARCHIVE  
            (           
                FILTER_PREDICATE = <predicate>,  
                MIGRATION_STATE = OUTBOUND  
            )  
        );   
    ```  

## <a name="filter-rows-by-date"></a>Rivien suodattaminen päivämäärän mukaan
Seuraavassa esimerkissä siirtää rivit, joissa **date** -sarake sisältää arvon aiempi 1 tammikuussa 2016.

```tsql
-- Filter by date
--
CREATE FUNCTION dbo.fn_stretch_by_date(@date datetime2)
RETURNS TABLE
WITH SCHEMABINDING
AS
       RETURN SELECT 1 AS is_eligible WHERE @date < CONVERT(datetime2, '1/1/2016', 101)
GO
```

## <a name="filter-rows-by-the-value-in-a-status-column"></a>Rivien suodattaminen arvon tila-sarakkeen mukaan
Seuraavassa esimerkissä siirtää rivit, joissa **tila** -sarakkeessa on jokin määritetyt arvot.

```tsql
-- Filter by status column
--
CREATE FUNCTION dbo.fn_stretch_by_status(@status nvarchar(128))
RETURNS TABLE
WITH SCHEMABINDING
AS
       RETURN SELECT 1 AS is_eligible WHERE @status IN (N'Completed', N'Returned', N'Cancelled')
GO
```

## <a name="filter-rows-by-using-a-sliding-window"></a>Rivien suodattaminen liukuva-ikkunan avulla
Rivien suodattaminen käyttämällä liukuva-ikkunaa, ota huomioon seuraavat vaatimukset filter-funktio.

-   Funktio on oltava deterministic. Tämän vuoksi et voi luoda funktion, joka laskee automaattisesti kuin aika siirtoja liukuva ikkunan.

-   Funktio käyttää rakenteen sidonta. Tämän vuoksi ei voi yksinkertaisesti päivittää funktion "on lisätty" päivittäin kutsumalla **FUNKTIO muuttaa** Siirrä liukuva-ikkunaa.

Aloita suodatinfunktion, kuten seuraavassa esimerkissä, joka siirtää rivit, jossa **systemEndTime** -sarakkeen sisältää arvon aiempi 2016 1 tammikuussa.

```tsql
CREATE FUNCTION dbo.fn_StretchBySystemEndTime20160101(@systemEndTime datetime2)
RETURNS TABLE
WITH SCHEMABINDING  
AS  
RETURN SELECT 1 AS is_eligible
  WHERE @systemEndTime < CONVERT(datetime2, '2016-01-01T00:00:00', 101) ;
```

Filter-funktio käyttää taulukkoon.

```tsql
ALTER TABLE <table name>
SET (
        REMOTE_DATA_ARCHIVE = ON
                (
                        FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20160101 (SysEndTime)
                                , MIGRATION_STATE = OUTBOUND
                )
        )
;
```

Kun haluat päivittää liukuva ikkunan, tee seuraavat kohdat.

1.  Luo uusi funktio, joka määrittää liukuva uudessa ikkunassa. Seuraavassa esimerkissä valitsee päivämäärät aiempi 2 tammikuussa 2016, sen sijaan, että 2016 1 tammikuussa.

2.  Korvaa edellisen suodatinfunktion uuteen puheluja **ALTER TABLE**-seuraavan esimerkin mukaisesti.

3. Voit myös pudota edellisen filter-funktio, joka ei ole enää käytössä **PUDOTA FUNKTIO**-. (Tämä vaihe ei näy esimerkissä.)

```tsql
BEGIN TRAN
GO
        /*(1) Create new predicate function definition */
        CREATE FUNCTION dbo.fn_StretchBySystemEndTime20160102(@systemEndTime datetime2)
        RETURNS TABLE
        WITH SCHEMABINDING
        AS
        RETURN SELECT 1 AS is_eligible
               WHERE @systemEndTime < CONVERT(datetime2,'2016-01-02T00:00:00', 101)
        GO

        /*(2) Set the new function as the filter predicate */
        ALTER TABLE <table name>
        SET
        (
               REMOTE_DATA_ARCHIVE = ON
               (
                       FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20160102(SysEndTime),
                       MIGRATION_STATE = OUTBOUND
               )
        )
COMMIT ;
```

## <a name="more-examples-of-valid-filter-functions"></a>Lisää esimerkkejä kelvollinen Suodatinfunktiot

-   Seuraavassa esimerkissä yhdistää kaksi alkeistyyppi ehtoa looginen AND-operaattorin avulla.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate((@column1 datetime, @column2 nvarchar(15))
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
      WHERE @column1 < N'20150101' AND @column2 IN (N'Completed', N'Returned', N'Cancelled')
    GO

    ALTER TABLE table1 SET ( REMOTE_DATA_ARCHIVE = ON (
        FILTER_PREDICATE = dbo.fn_stretchpredicate(date, shipment_status),
        MIGRATION_STATE = OUTBOUND
    ) )
    ```

-   Seuraavassa esimerkissä useita ehtoja ja deterministic muuntaminen ja Muunna.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example1(@column1 datetime, @column2 int, @column3 nvarchar)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2015', 101)AND (@column2 < -100 OR @column2 > 100 OR @column2 IS NULL)AND @column3 IN (N'Completed', N'Returned', N'Cancelled')
    GO
    ```

-   Seuraavassa esimerkissä matemaattisia operaattoreita ja funktioita.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example2(@column1 float)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < SQRT(400) + 10
    GO
    ```

-   Seuraavassa esimerkissä ei BETWEEN-operaattorien ja BETWEEN. Tämä käyttö on kelvollinen, koska tuloksena funktion mukainen kuvatut, kun korvaat BETWEEN ja ei BETWEEN-operaattorien käyttäminen vastaavat ja-ja tai-lausekkeiden kanssa säännöt.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example3(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 BETWEEN 0 AND 100
                AND (@column2 NOT BETWEEN 200 AND 300 OR @column1 = 50)
    GO
    ```
    Edeltävä funktio on sama kuin seuraavan funktion, kun korvaat ei BETWEEN-operaattorien ja BETWEEN vastaavat ja-ja tai-lausekkeet.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example4(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 >= 0 AND @column1 <= 100AND (@column2 < 200 OR @column2 > 300 OR @column1 = 50)
    GO
    ```

## <a name="examples-of-filter-functions-that-arent-valid"></a>Esimerkkejä, jotka eivät ole kelvollisia Suodatinfunktiot

-   Seuraava funktio ei ole kelvollinen, koska se sisältää muun\-deterministic muuntaminen.

    ```tsql
    CREATE FUNCTION dbo.fn_example5(@column1 datetime)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < CONVERT(datetime, '1/1/2016')
    GO
    ```

-   Seuraava funktio ei ole kelvollinen, koska se sisältää muun\-deterministic funktiokutsun.

    ```tsql
    CREATE FUNCTION dbo.fn_example6(@column1 datetime)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < DATEADD(day, -60, GETDATE())
    GO
    ```

-   Seuraava funktio ei ole kelvollinen, koska se sisältää alikyselyn.

    ```tsql
    CREATE FUNCTION dbo.fn_example7(@column1 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 IN (SELECT SupplierID FROM Supplier WHERE Status = 'Defunct'))
    GO
    ```

-   Seuraavat toiminnot eivät ole kelvollisia koska lausekkeita, jotka algebrallinen operaattoreita tai sisäisten\-funktioissa on arvo on vakio määrittäessäsi funktio. Et voi sisällyttää sarakeviittauksia algebrallinen lausekkeissa tai funktiokutsut.

    ```tsql
    CREATE FUNCTION dbo.fn_example8(@column1 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 % 2 =  0
    GO

    CREATE FUNCTION dbo.fn_example9(@column1 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE SQRT(@column1) = 30
    GO
    ```

-   Seuraava funktio ei ole kelvollinen, koska se rikkoo kuvatut, kun korvaat BETWEEN-operaattorin ja vastaa lauseke sääntöjä.

    ```tsql
    CREATE FUNCTION dbo.fn_example10(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE (@column1 BETWEEN 1 AND 200 OR @column1 = 300) AND @column2 > 1000
    GO
    ```
    Edeltävä funktio vastaa seuraavan funktion jälkeen voit korvata BETWEEN-operaattorin ja vastaa lauseke. Tämä funktio ei ole kelvollinen, koska alkeistyyppi ehtoja voi käyttää vain looginen OR-operaattoria.

    ```tsql
    CREATE FUNCTION dbo.fn_example11(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE (@column1 >= 1 AND @column1 <= 200 OR @column1 = 300) AND @column2 > 1000
    GO
    ```

## <a name="how-stretch-database-applies-the-filter-function"></a>Miten Venytys tietokannan koskee filter-funktio
Venytä tietokannan koskee taulukon filter-funktio ja määrittää olevalla rivien toimintojen käyttäminen-operaattorilla. Esimerkki:

```tsql
SELECT * FROM stretch_table_name CROSS APPLY fn_stretchpredicate(column1, column2)
```
Jos funktio palauttaa muun\-tyhjän tuloksen rivin, rivin on vaihdettavissa uuteen.

## <a name="replacePredicate"></a>Korvaa aiemmin luotu filter-funktio
Voit korvata aiemmin määritetyt suodatinfunktion **ALTER TABLE** -lause uudelleen suorittamalla ja uuden arvon, joka määrittää **SUODATTIMEN\_PREDIKAATIN** parametria. Esimerkki:

```tsql
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE = dbo.fn_stretchpredicate2(column1, column2),
    MIGRATION_STATE = <desired_migration_state>
```
Uuden taulukon riveillä\-tärkeät funktio on seuraavien ehtojen mukaan.

-   Uudesta funktiosta on oltava joustavampi kuin edellinen toiminto.

-   Kaikki operaattoreita, jotka olivat vanha-funktiossa on oltava uudesta funktiosta.

-   Uusi funktio ei voi sisältää operaattoreita, joita ei ole vanha-funktiossa.

-   Operaattori argumentit järjestystä ei voi muuttaa.

-   Vain vakio arvot, jotka ovat osa `<, <=, >, >=` vertailu voidaan muuttaa tapaa, jolla on funktion joustavampi.

### <a name="example-of-a-valid-replacement"></a>Esimerkki kelvollinen korvaava
Oletetaan, että seuraava funktio on nykyinen suodatin-lause.

```tsql
CREATE FUNCTION dbo.fn_stretchpredicate_old (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
            AND (@column2 < -100 OR @column2 > 100)
GO
```
Seuraava funktio on kelvollinen korvaava, koska uuden Päivämäärävakio (joka määrittää myöhemmin määräpäivä) tekee funktion joustavampi.

```tsql
CREATE FUNCTION dbo.fn_stretchpredicate_new (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '2/1/2016', 101)
            AND (@column2 < -50 OR @column2 > 50)
GO
```

### <a name="examples-of-replacements-that-arent-valid"></a>Esimerkkejä korvauksia, jotka eivät ole kelvollisia
Seuraavan funktion ei kelpaa korvaa, koska uuden Päivämäärävakio (joka määrittää aiemmin määräpäivä) ei tehdä funktion joustavampi.

```tsql
CREATE FUNCTION dbo.fn_notvalidreplacement_1 (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2015', 101)
            AND (@column2 < -100 OR @column2 > 100)
GO
```
Seuraava funktio ei ole kelvollinen korvaava, koska jokin operaattoreita on poistettu.

```tsql
CREATE FUNCTION dbo.fn_notvalidreplacement_2 (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
            AND (@column2 < -50)
GO
```
Seuraava funktio ei kelpaa korvaa koska looginen AND-operaattorin kanssa on lisätty uusi ehto.

```tsql
CREATE FUNCTION dbo.fn_notvalidreplacement_3 (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
            AND (@column2 < -100 OR @column2 > 100)
            AND (@column2 <> 0)
GO
```

## <a name="remove-a-filter-function-from-a-table"></a>Filter-funktiossa poistaminen taulukosta
Voit siirtää koko taulukon valittujen rivien sijaan, Poista olemassa olevan funktion asettamalla **suodatin\_PREDIKAATIN** Null. Esimerkki:

```tsql
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE = NULL,
    MIGRATION_STATE = <desired_migration_state>
) )
```
Kun olet poistanut filter-funktio, kaikki taulukon rivit soveltuvat siirron. Tuloksena et voi määrittää suodatinfunktion myöhemmin samaan taulukkoon, ellei voit noutaa takaisin remote data taulukon azuren ensin. Tämä rajoitus on tilanne, jossa rivit, jotka eivät ole oikeutettuja siirron, kun annat uuden filter-funktiossa on jo siirretty Azure välttämiseksi.

## <a name="check-the-filter-function-applied-to-a-table"></a>Tarkista taulukon käytetty filter-funktio
Tarkistamaan filter-funktiossa käytetty taulukon Avaa luettelo-näkymä **sys.remote\_tietojen\_arkisto\_taulukoiden** ja tarkistaa **suodatin\_lause** sarakkeen. Jos arvo on null, koko taulukko on vaihdettavissa arkistointia varten. Saat lisätietoja, [sys.remote_data_archive_tables (Transact-SQL)](https://msdn.microsoft.com/library/dn935003.aspx).

## <a name="security-notes-for-filter-functions"></a>Suodatinfunktiot suojausta koskevia huomautuksia  
Vaarantuneen tilin db_owner oikeuksilla voit tehdä seuraavat kohdat.  

-   Luoda ja käyttää taulukkoarvoisen funktion, joka käyttää paljon resursseja palvelimeen tai pitkään, tuloksena on eston palvelun odottaa.  

-   Luoda ja käyttää taulukkoarvoisen funktion, joka mahdollistaa sellaisten johtaa, jonka käyttäjä ei ole nimenomaisesti myönnetty lukuoikeudet taulukon sisältöä.  

## <a name="see-also"></a>Katso myös

[Muuta taulukko (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx)
