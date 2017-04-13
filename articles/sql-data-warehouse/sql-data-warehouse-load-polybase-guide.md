<properties
   pageTitle="Oppaan PolyBase käyttäminen SQL-tietovarasto | Microsoft Azure"
   description="Ohjeita ja suosituksia PolyBase käyttäminen SQL-tietovarasto skenaarioita."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/30/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="guide-for-using-polybase-in-sql-data-warehouse"></a>Oppaan PolyBase käyttäminen SQL-tietovarasto

Tämän oppaan saat käytännön tietoja PolyBase käyttäminen SQL-tietovarasto.

Aloita-kohdasta [PolyBase tietojen lataaminen][] opetusohjelman.


## <a name="rotating-storage-keys"></a>Tallennustilan näppäimet kiertäminen

Aika haluat muuttaa pikanäppäin-blob-säiliö tietoturvasyistä.

Voit suorittaa tämän tehtävän eniten klassinen silloin, kun nimeltään "kiertäminen näppäimet" prosessin. Olet ehkä jo huomannut, että sinulla on kaksi tallennustilan näppäimet blob storage tilissäsi. Tämä on niin, että voit siirtyminen

Azure-tallennustilan tilin avaimien kiertäminen on yksinkertainen kolme vaihetta

1. Luo toisen tietokannan suodatetut tunnistetiedon toissijainen tallennusväline-pikanäppäin perusteella
2. Luo toinen perustuvaa tämän uuden tunnistetiedon ulkoinen tietolähde
3. Jätä ja luo ulkoisen taulukot uuteen ulkoiseen tietolähteeseen osoittamalla

Kun olet siirtänyt kaikki ulkoiset taulukot uuteen ulkoiseen tietolähteeseen sitten voit suorittaa tehtäviä puhdistus:

1. Pudota ensimmäisen ulkoiseen tietolähteeseen
2. Pudota ensimmäisen tietokannan suodatetut tunnistetiedon ensisijainen tallennustilan pikanäppäin perusteella
3. Kirjaudu sisään Azure ja luo valmis seuraavan kerran ensisijainen pikanäppäin

## <a name="query-azure-blob-storage-data"></a>Kyselyn Azure blob storage tiedot
Kyselyitä, jotka perustuvat ulkoiset taulukot käyttää ainoastaan taulukkonimeä, aivan kuin se oli relaatiotaulukko.

```sql
-- Query Azure storage resident data via external table.
SELECT * FROM [ext].[CarSensor_Data]
;
```

> [AZURE.NOTE] Ulkoisen taulukon, kyselyn voi epäonnistua virheen *"Kyselyn keskeytetty--suurin hylkää raja on saavutettu luettaessa ulkoisesta tietolähteestä"*. Tämä ilmaisee, että ulkoiset tiedot sisältää *dirty* tietueita. Tietueen pidetään likainen", jos todellisten tietojen tyypit/sarakkeiden määrä ei vastaa ulkoisen taulukon sarakkeen määritelmät tai tiedot poikkeaa määritetyn ulkoisessa tiedostomuodossa. Voit korjata ongelman, varmista, että ulkoinen taulukko ja ulkoisen tiedoston muoto määritykset ovat oikein eivätkä ulkoisten tietojen mukainen nämä määritykset. Ulkoisten tietojen tietuejoukon on muokattu, niin voit hylätä nämä tietueet kyselyjen luominen ULKOISEN taulukon DDL hylkää-asetuksilla.


## <a name="load-data-from-azure-blob-storage"></a>Tietojen lataaminen Azure-blob-säiliö
Tässä esimerkissä lataa tiedot Azure-blob-säiliö tietovarasto SQL-tietokantaan.

Tietojen tallentaminen suoraan poistaa kyselyjen tietojen siirto-aika. Columnstore indeksi tietojen tallentaminen parantaa kyselyn suorituskykyä analyysi kyselyjen mukaan enintään 10 x.

Tässä esimerkissä luoda taulukon AS SELECT-lauseen lataa tiedot. Uuden taulukon perii kyselyssä sarakkeet. Näiden sarakkeiden tietotyyppien sen periytyvät ulkoisen taulukkomääritys.

Luo taulukko liitetään Valitse on erittäin performant Transact-SQL-lause, joka lataa rinnakkain SQL-tietovarasto kaikki Laske solmut tiedot.  Se on alun perin kehitetty Analytics Platform järjestelmän erittäin rinnakkain käsittely (MPP)-ohjelma ja on nyt SQL-tietovarasto.

```sql
-- Load data from Azure blob storage to SQL Data Warehouse

CREATE TABLE [dbo].[Customer_Speed]
WITH
(   
    CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([CarSensor_Data].[CustomerKey])
)
AS
SELECT *
FROM   [ext].[CarSensor_Data]
;
```

Katso, [Valitse Luo taulukon AS (Transact-SQL)][].

## <a name="create-statistics-on-newly-loaded-data"></a>Luo tilastotietoja ladatut tietoihin

Azure SQL-tietovarasto ei vielä tuki automaattinen luominen tai automaattisen päivityksen tilastotiedot.  Jotta saat parhaan suorituskyvyn-kyselyissä, on tärkeää, että tilastotiedot luodaan kaikkien taulukoiden kaikkien sarakkeiden jälkeen ensimmäisen kuormituksen tai merkittäviä muutoksia esiintyvät tiedot.  Yksityiskohtainen kuvaus tilasto on aiheessa [tilastotiedot][] aiheista kehittäminen-ryhmässä.  Alla on lyhyt Esimerkki tilastoja taulukon luomisesta ladattu tässä esimerkissä.

```sql
create statistics [SensorKey] on [Customer_Speed] ([SensorKey]);
create statistics [CustomerKey] on [Customer_Speed] ([CustomerKey]);
create statistics [GeographyKey] on [Customer_Speed] ([GeographyKey]);
create statistics [Speed] on [Customer_Speed] ([Speed]);
create statistics [YearMeasured] on [Customer_Speed] ([YearMeasured]);
```

## <a name="export-data-to-azure-blob-storage"></a>Tietojen vieminen Azure-blob-säiliö
Tässä osassa esitellään tietojen viemisestä Azure-blob-säiliö SQL-tietovarasto. Tässä esimerkissä käytetään luoda ULKOISEN taulukon AS Valitse erittäin performant Transact-SQL-lauseen rinnakkain-tietojen vieminen Laske solmujen eli.

Seuraavassa esimerkissä luodaan käyttämällä sarakkeen määritelmät ja dbo tiedot ulkoisen taulukon Weblogs2014. Web-päiväkirjoja taulukko. Ulkoisen taulukkomääritys on tallennettu SQL-tietovarasto ja SELECT-lauseen tulokset viedään tietolähteen määrittämän blob-säilö "/ arkistoida/log2014 /" hakemiston. Tiedot on viety määritetyn tekstin-tiedostomuodossa.

```sql
CREATE EXTERNAL TABLE Weblogs2014 WITH
(
    LOCATION='/archive/log2014/',
    DATA_SOURCE=azure_storage,
    FILE_FORMAT=text_file_format
)
AS
SELECT
    Uri,
    DateRequested
FROM
    dbo.Weblogs
WHERE
    1=1
    AND DateRequested > '12/31/2013'
    AND DateRequested < '01/01/2015';
```


## <a name="working-around-the-polybase-utf-8-requirement"></a>Toimi PolyBase UTF-8 vaatimus ympärille
Esitä PolyBase osoitteessa tukee ladattaessa tiedostoja, jotka on koodattu UTF-8. Kun UTF-8 käyttää saman merkkien koodaus kuin ASCII-PolyBase näkyy myös tuki ladataan tiedot, jotka ovat koodattu ASCII. Kuitenkin PolyBase ei tue muiden merkkien koodaus kuten UTF-16 / Unicode tai ASCII erikoismerkkejä. Laajennettu ASCII sisältää merkit, joilla on yhteiset Saksan kielen umlaut kuten korostukset.

Voit kiertää tämä vaatimus paras on UTF-8 koodaus uudelleen kirjoittaminen.

Voit tehdä tämän useilla tavoilla. Alla on kaksi tapaa PowerShellin avulla:

### <a name="simple-example-for-small-files"></a>Esimerkki pieni tiedostoille

Alla on yksinkertainen yhden rivin Powershell-komentosarja, joka luo tiedosto.

```PowerShell
Get-Content <input_file_name> -Encoding Unicode | Set-Content <output_file_name> -Encoding utf8
```

Tämä on helppo tapa koodata uudelleen tiedot on kuitenkin mukaan ei tavoin mahdollisimman tehokkaasti. Io streaming seuraavassa esimerkissä on paljon, paljon nopeammin ja kertyy saman tuloksen.

### <a name="io-streaming-example-for-larger-files"></a>Suurempia tiedostoja IO Streaming-Esimerkki

Koodin otosten alla on monimutkaisempi, mutta se virtauttaa tietolähteen kohdentamisen se rivejä on paljon tehokkaampi. Käyttää tätä tapaa suurempia tiedostoja.

```PowerShell
#Static variables
$ascii = [System.Text.Encoding]::ASCII
$utf16le = [System.Text.Encoding]::Unicode
$utf8 = [System.Text.Encoding]::UTF8
$ansi = [System.Text.Encoding]::Default
$append = $False

#Set source file path and file name
$src = [System.IO.Path]::Combine("C:\input_file_path\","input_file_name.txt")

#Set source file encoding (using list above)
$src_enc = $ansi

#Set target file path and file name
$tgt = [System.IO.Path]::Combine("C:\output_file_path\","output_file_name.txt")

#Set target file encoding (using list above)
$tgt_enc = $utf8

$read = New-Object System.IO.StreamReader($src,$src_enc)
$write = New-Object System.IO.StreamWriter($tgt,$append,$tgt_enc)

while ($read.Peek() -ne -1)
{
    $line = $read.ReadLine();
    $write.WriteLine($line);
}
$read.Close()
$read.Dispose()
$write.Close()
$write.Dispose()
```

## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja tietojen siirtäminen SQL-tietovarasto on artikkelissa [tietojen siirron yleiskuvaus][].

<!--Image references-->

<!--Article references-->
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Lataa tiedot ja PolyBase]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Tilastotiedot]: ./sql-data-warehouse-tables-statistics.md
[tietojen siirron yleiskuvaus]: ./sql-data-warehouse-overview-migrate.md

<!--MSDN references-->
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server destination adapter]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx

[CREATE EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/dn935022.aspx
[Luo ULKOISESSA TIEDOSTOMUODOSSA (Transact-SQL)]: https://msdn.microsoft.com/library/dn935026) [Luo ULKOISEN taulukon (Transact-SQL)] .aspx: https://msdn.microsoft.com/library/dn935021.aspxx

[DROP EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/mt146367.aspx
[DROP EXTERNAL FILE FORMAT (Transact-SQL)]: https://msdn.microsoft.com/library/mt146379.aspx
[DROP EXTERNAL TABLE (Transact-SQL)]: https://msdn.microsoft.com/library/mt130698.aspx

[Luo taulukon AS SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[INSERT...SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/ms174335.aspx
[CREATE MASTER KEY (Transact-SQL)]: https://msdn.microsoft.com/library/ms174382.aspx
[CREATE CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189522.aspx
[CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/mt270260.aspx
[DROP CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189450.aspx

<!-- External Links -->
