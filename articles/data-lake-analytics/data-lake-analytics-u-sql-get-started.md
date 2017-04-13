<properties
   pageTitle="U-SQL-komentosarjojen käyttämällä järvi Datatyökalut Visual Studio kehittäminen | Azure"
   description="Opi asentamaan järvi Datatyökalut Visual Studiossa, voit kehittää ja testaa U-SQL-komentosarjoja. "
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="tutorial-get-started-with-azure-data-lake-analytics-u-sql-language"></a>Opetusohjelma: Azure tietojen järvi Analytics U-SQL-kielen käytön aloittaminen

U SQL on kieli, joka yhtenäistää SQL edut kanssa oman koodin ilmeikäs power voit käsitellä kaikkia tietoja milloin tahansa asteikko. U-SQL-käyttäjän skaalattava jaettu kysely ominaisuuksien avulla voit analysoida tietoja kaupan ja relaatio stores, kuten SQL Azure-tietokannan välillä tehokkaasti.  Sen avulla prosessi erimuotoisia tietoja lisäämällä rakenne lukeminen, lisätä mukautetun logiikan ja UDF päivän ja sisältää laajennettavuus siitä, kuinka tasolla tietoa hakuindeksointi voidaan suojata hallintaa. Lisätietoja takana U-SQL-suunnittelumalli, tutustu tämän [Visual Studio blogimerkintä](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).

Eroja ANSI SQL-tai T-SQL. Esimerkiksi sen avainsanoilla, kuten Valitse on oltava isoin kirjaimin kirjoitetut.

Se on tyyppi järjestelmä- ja expression kielen sisällä select-lauseet-predikaatit jne sijainti: C#.
Tämä tarkoittaa tietotyypit ovat C#-tyypit ja tietotyypit käyttää C# NULL semantiikkaan liittyvien sisällä lause vertailu-toiminnot noudattamalla C# syntaksi (esimerkiksi == "tapahtuman nimenä").  Tämä tarkoittaa myös, että arvot ovat koko .NET-objektit, joilla voit käyttää mitä tahansa menetelmää helposti toimimaan objektin (esimerkiksi "f o o o". Split(' ')).

Lisätietoja on artikkelissa [U-SQL-viittaus](http://go.microsoft.com/fwlink/p/?LinkId=691348).

###<a name="prerequisites"></a>Edellytykset

Sinun on suoritettava [Opetusohjelma: kehittää U-SQL-komentosarjoja järvi Datatyökalut Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).

Opetusohjelman suoritit tietojen järvi Analytics työn seuraavat U-SQL-komentosarjan:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    OUTPUT @searchlog   
        TO "/output/SearchLog-first-u-sql.csv"
    USING Outputters.Csv();

Tämä komentosarja ei ole mitään muunnos vaiheet. Se lukee lähdetiedoston nimi **SearchLog.tsv**, schematizes se ja tulostaa Rivijoukko takaisin tiedostoon, jota kutsutaan **SearchLog-ensimmäinen-u-sql.csv**.

Huomaa kysymysmerkki kesto-kentän tietotyyppi-kohdan vieressä. Tämä tarkoittaa kesto-kentässä voi olla null.

Jotkin käsitteitä ja komentosarja-kohdan avainsanat:

- **Rivijoukko muuttujat**: kunkin kyselylauseke, joka tuottaa Rivijoukko voi määrittää muuttuja. U SQL seuraa T-SQL muuttujan nimeämismuoto, esimerkiksi **@searchlog** komentosarjan.
    Huomautus varauksen Pakota suorittamisen. Se pelkästään nimiä lausekkeen ja antaa selvitysyhteisön mahdollisuus monimutkaisia lausekkeita.
- **PURA** avulla voit määrittää rakenteen luku. Rakenteen määrittämän sarakkeen nimi- ja C# tyyppi nimipari saraketta kohti. Tiedostossa käytetään niin **Extractor**, esimerkiksi **Extractors.Tsv()** Pura tsv tiedostot. Voit luoda mukautetun ulosvetimiä.
- **Tulos** tulee Rivijoukko ja serializes sitä. Outputters.Csv() tulosteen CSV-tiedoston tuominen määritettyyn sijaintiin. Voit luoda myös mukautettuja Outputters.
- Huomaa, että kaksi polut ovat suhteellisia polkuja. Voit käyttää myös suoria polkuja.  Esimerkki

        adl://<ADLStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv

    Sinun on käytettävä absoluuttinen polku linkitetyn tallennustilan tileihin tiedostoja.  Liitetyn Azure-tallennustilan tilin tallennettuja tiedostoja syntaksi on seuraava:

        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Azure Blob-säilö julkisen BLOB tai julkinen säilöjen käyttöoikeudet eivät ole tuettuja.

## <a name="use-scalar-variables"></a>Skalaaripäivämäärän muuttujien käyttäminen

Voit skalaarinen muuttujat sekä helpottaa komentosarjan ylläpito. Edellinen U-SQL-komentosarja voidaan kirjoittaa myös seuraavanlaiselta:

    DECLARE @in  string = "/Samples/Data/SearchLog.tsv";
    DECLARE @out string = "/output/SearchLog-scalar-variables.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM @in
        USING Extractors.Tsv();

    OUTPUT @searchlog   
        TO @out
        USING Outputters.Csv();

## <a name="transform-rowsets"></a>Muunna rivijoukot

Muunna rivijoukot **Valitse** avulla:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-rowsets.csv"
        USING Outputters.Csv();

WHERE-lauseen käyttää [C# totuusarvolauseke](https://msdn.microsoft.com/library/6a71f45d.aspx). Voit tehdä omia lausekkeista ja funktioista C#-lausekielellä. Voit suorittaa myös monimutkaisia suodattaminen yhdistämällä ne looginen konjunktioiden (and-operaattorien) ja disjunctions (ORs).

Seuraavaa komentosarjaa käyttää DateTime.Parse()-menetelmällä ja yhdessä.

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    @rs1 =
        SELECT Start, Region, Duration
        FROM @rs1
        WHERE Start >= DateTime.Parse("2012/02/16") AND Start <= DateTime.Parse("2012/02/17");

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-datatime.csv"
        USING Outputters.Csv();

Huomaa, että ensimmäinen Rivijoukko saatu käsittelee toista kyselyä ja näin tulos on kaksi suodatinta yhdistelmä. Voit myös käyttää muuttujan nimi ja nimet on rajattu lexically.

## <a name="aggregate-rowsets"></a>Kooste rivijoukot

U SQL avulla voit esimerkiksi tuttuja **ORDER BY** **GROUP BY** ja koosteet.

Seuraava kysely löytää kokonaiskesto myynnit alueittain ja tulostaa sitten yläreunassa 5 kestot järjestyksessä.

U-SQL-rivijoukot Säilytä järjestykseen seuraavan kyselyn. Siis haluat tilata tulos, lisää ORDER BY tulos lausekkeen alla kuvatulla tavalla:

    DECLARE @outpref string = "/output/Searchlog-aggregation";
    DECLARE @out1    string = @outpref+"_agg.csv";
    DECLARE @out2    string = @outpref+"_top5agg.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
    GROUP BY Region;

    @res =
    SELECT *
    FROM @rs1
    ORDER BY TotalDuration DESC
    FETCH 5 ROWS;

    OUTPUT @rs1
        TO @out1
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();
    OUTPUT @res
        TO @out2
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

U SQL ORDER BY-lause on yhdistettävä Nouda-lause SELECT-lausekkeessa.

OTTAA U-SQL-lauseen avulla voidaan rajoittaa tulosteen ryhmiin, jotka täyttävät HAVING-ehto:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/Searchlog-having.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

## <a name="join-data"></a>Tietojen yhdistäminen

U SQL tarjoaa yleisiä join-operaattoreita, kuten INNER JOIN, vasemmalle/oikealle/TÄYDEN ULKOLIITOKSEN, LÄPIKUULTAVA liity liittymään paitsi taulukot, mutta rivijoukot, (myös ne valmistettu tiedostot).

Seuraavaa komentosarjaa yhdistää ilmoitus-vaikutelman log searchlog ja antaa us kyselymerkkijonon ilmoitusten tietyn päivämäärän.

    @adlog =
        EXTRACT UserId int,
                Ad string,
                Clicked int
        FROM "/Samples/Data/AdsLog.tsv"
        USING Extractors.Tsv();

    @join =
        SELECT a.Ad, s.Query, s.Start AS Date
        FROM @adlog AS a JOIN <insert your DB name>.dbo.SearchLog1 AS s
                        ON a.UserId == s.UserId
        WHERE a.Clicked == 1;

    OUTPUT @join   
        TO "/output/Searchlog-join.csv"
        USING Outputters.Csv();


U SQL tukee vain ANSI-yhteensopiva join-syntaksia: Rowset1 liity Rowset2 edelleen lause. Vanha syntaksi-Rowset1, jossa Rowset2 lause ei tueta.
LIITOKSEN-lause on oltava tasa-Kutsu osallistujat ja ilman lauseketta. Jos haluat käyttää lauseketta, lisää se edellisen Rivijoukko select-lauseessa. Jos haluat tehdä erilaista vertailua, siirtäminen WHERE-lauseen.


## <a name="create-databases-table-valued-functions-views-and-tables"></a>Tietokantojen, taulukkoarvoisten funktioiden, näkymien ja taulukoiden luominen

U SQL avulla voit käyttää tietoja tietokanta ja rakenteen yhteydessä. Näin ei tarvitse lukea ja kirjoittaa tiedostoja aina.

Jokaisen U-SQL-komentosarja suoritetaan tietokannan oletusarvo (perus) ja rakenteen oletusarvo (DBO) sen oletussijainnin. Voit luoda oman tietokannan ja/tai rakennetta. Jos haluat muuttaa rivin kontekstissa, muuttaa yhteydessä **käyttää** -lauseen avulla.


### <a name="create-a-table-valued-function-tvf"></a>Luo taulukkoarvoisen funktion (TVF)

Toistaa edellisen U-SQL-komentosarja käyttämällä PURA saman lähdetiedoston luettaessa. U SQL taulukkoarvoisen funktion avulla voit Sisällytä myöhempää tiedot.   

Seuraavaa komentosarjaa Luo TVF, eli *Searchlog()* oletusarvon tietokanta ja rakenteen:

    DROP FUNCTION IF EXISTS Searchlog;

    CREATE FUNCTION Searchlog()
    RETURNS @searchlog TABLE
    (
                UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
    )
    AS BEGIN
    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
    USING Extractors.Tsv();
    RETURN;
    END;

Seuraavaa komentosarjaa näytetään, miten voit käyttää edellisen komentosarjan TVF:

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM Searchlog() AS S
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/SerachLog-use-tvf.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

### <a name="create-views"></a>Näkymien luominen

Jos sinulla on vain yksi kyselylauseke, joka haluat abstrakti ja et halua parameterize sitä, voit luoda näkymän taulukkoarvoisten-funktion sijasta.

Seuraavaa komentosarjaa luo oletusarvon tietokannassa ja rakenteen *SearchlogView* -näkymän:

    DROP VIEW IF EXISTS SearchlogView;

    CREATE VIEW SearchlogView AS  
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
    USING Extractors.Tsv();

Seuraavaa komentosarjaa esittelee määritetyn näkymän avulla:

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM SearchlogView
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/Searchlog-use-view.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

### <a name="create-tables"></a>Taulukoiden luominen

Kaltaisilta relaatiotietokannasta taulukon U SQL antaa Luo taulukko, jossa valmiin mallin tai taulukon luominen ja päätellä kysely, joka täyttää (tunnetaan myös nimellä Luo taulukko liitetään Valitse tai CTAS) taulukon rakenteen perusteella.

Seuraavaa komentosarjaa luominen tietokannan ja kaksi taulukkoa:

    DROP DATABASE IF EXISTS SearchLogDb;
    CREATE DATABASE SeachLogDb
    USE DATABASE SearchLogDb;

    DROP TABLE IF EXISTS SearchLog1;
    DROP TABLE IF EXISTS SearchLog2;

    CREATE TABLE SearchLog1 (
                UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string,

                INDEX sl_idx CLUSTERED (UserId ASC)
                    PARTITIONED BY HASH (UserId)
    );

    INSERT INTO SearchLog1 SELECT * FROM master.dbo.Searchlog() AS s;

    CREATE TABLE SearchLog2(
        INDEX sl_idx CLUSTERED (UserId ASC)
                PARTITIONED BY HASH (UserId)
    ) AS SELECT * FROM master.dbo.Searchlog() AS S; // You can use EXTRACT or SELECT here


### <a name="query-tables"></a>Taulukot

Voit tehdä kyselyn taulukot (luotu Edellinen komentosarja) samalla tavalla kuin teet kyselyn datatiedostot päälle. Sijaan Rivijoukko, käyttämällä PURA, voit nyt viitata taulukon nimeä.

Taulukoiden lukeminen on muokattu aikaisemmin Muunna komentosarja:

    @rs1 =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM SearchLogDb.dbo.SearchLog2
    GROUP BY Region;

    @res =
        SELECT *
        FROM @rs1
        ORDER BY TotalDuration DESC
        FETCH 5 ROWS;

    OUTPUT @res
        TO "/output/Searchlog-query-table.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

Huomaa, että ei tällä hetkellä voi suorittaa SELECT saman komentosarjan kuin komentosarja taulukossa kohtaa, johon luot taulukon.


##<a name="conclusion"></a>Tekemistä

Mitä käsitellään opetusohjelman on vain U-SQL-osaa. Tässä opetusohjelmassa aluetta, koska se voi kattaa kaikki ohjelmat, kuten:

- Käytä toimintojen koske Pura osat merkkijonoja, taulukot ja kartat riveihin.
- Toimi osioitua tietojoukkoa (tiedostojoukot ja osioitua taulukot).
- Käyttäjän määrittämät operaattoreita, kuten ulosvetimiä, outputters, suorittimien määrä, käyttäjän määrittämä kokoajiksi C# kehittäminen
- U-SQL-windowing funktioilla.
- Hallitse näkymiä, taulukkoarvoisten funktioiden ja tallennettujen toimintosarjojen U-SQL-koodin.
- Suorittaa käsittely-solmut mukautettua koodia.
- Muodostaa Azure SQL-tietokannat ja federate kyselyjen ne ja U-SQL- ja Azure tietojen järvi tiedot.

## <a name="see-also"></a>Katso myös

- [Microsoft Azure tietojen järvi Analytics yleiskatsaus](data-lake-analytics-overview.md)
- [U-SQL-komentosarjojen käyttämällä järvi Datatyökalut Visual Studio kehittäminen](data-lake-analytics-data-lake-tools-get-started.md)
- [U-SQL-ikkunassa funktioilla Azure tietojen järvi Analytics töiden](data-lake-analytics-use-window-functions.md)
- [Valvo ja Azure tietojen järvi Analytics työt Azure-portaalissa vianmääritys](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

## <a name="let-us-know-what-you-think"></a>Kerro mielipiteesi

- [Ominaisuuden pyytäminen](http://aka.ms/adlafeedback)
- [Pyydä apua keskustelupalstoilla](http://aka.ms/adlaforums)
- [Anna palautetta U SQL](http://aka.ms/usqldiscuss)
