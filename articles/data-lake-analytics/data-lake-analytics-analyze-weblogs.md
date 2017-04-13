<properties
   pageTitle="Analysoi sivuston lokit käyttämällä Azure tietojen järvi Analytics | Azure"
   description="Lue, miten voit analysoida sivuston lokit käyttäminen tietojen järvi Analytics. "
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

# <a name="tutorial-analyze-website-logs-using-azure-data-lake-analytics"></a>Opetusohjelma: Analysoida sivuston lokit Azure tietojen järvi analyysin avulla

Lue, miten voit analysoida sivuston lokit käyttäminen tietojen järvi Analytics, erityisesti tietämään, mitä viittaajat havaittiin virheitä ne yritit sivustosta.

>[AZURE.NOTE] Jos haluat nähdä toimi sovelluksen, tallentaa, kun haluat [käyttää Azure tietojen järvi Analytics vuorovaikutteiset opetusohjelmat](data-lake-analytics-use-interactive-tutorials.md)läpi. Tässä opetusohjelmassa perustuu sama skenaario ja samaa koodia. Tässä opetusohjelmassa tarkoituksena on antaa kehittäjät kokemukset luomalla ja suorittamalla tietojen järvi Analytics-sovelluksen lopusta loppuun.

## <a name="prerequisites"></a>Edellytykset:

- **Visual Studio 2015, Visual Studio 2013 Päivitä 4, tai Visual Studio 2012, jossa on Visual C++ asennettuna**.
- **Microsoft Azure SDK .NET 2.5-version tai yläpuolella**.  Asenna se käyttämällä [WWW-ympäristö asennusohjelma](http://www.microsoft.com/web/downloads/platform.aspx).
- **[Tietoja järvi Tools for Visual Studio](http://aka.ms/adltoolsvs)**.

    Kun tietojen järvi Tools for Visual Studio on asennettu, näet **Tietoja järvi** valikon Visual Studiossa:

    ![Visual Studio U-SQL-valikko](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-menu.png)

- **Tietoja järvi analyysin ja Visual Studio järvi Datatyökalut basic tuntemus**. Aloita-kohdasta:

    - [Azure-portaalissa Azure tietojen järvi Analytics käytön aloittaminen](data-lake-analytics-get-started-portal.md).
    - [Tietoja järvi työkaluilla Visual Studio kehittää U-SQL-komentosarja](data-lake-analytics-data-lake-tools-get-started.md).

- **Tietoja järvi Analytics-tili.**  Lisätietoja on kohdassa [Create Azure tietojen järvi Analytics-tiliä](data-lake-analytics-get-started-portal.md#create_adl_analytics_account).

    Järvi Datatyökalut ei tue luominen tietojen järvi Analytics-tilit.  Näin sinun on luotava se Azure-portaalissa, PowerShellin Azure, .NET SDK tai Azure CLI.
- **Lataa mallitiedot tietojen järvi Analytics-tilille.** Katso [Järvi tietosäilö oletustilin ladata SearchLog.tsv](data-lake-analytics-get-started-portal.md#update-data-to-the-default-adl-storage-account).

    Tietoja järvi Analytics työn suorittamista varten sinun on tietoja. Vaikka järvi Datatyökalut tukee lataus tietoja, Lataa mallitiedot tehdä tässä opetusohjelmassa on helpompi seurata portaalin avulla.

## <a name="connect-to-azure"></a>Yhteyden muodostaminen Azure

Ennen kuin voit luoda ja testaa U-SQL-komentosarjat, sinun on ensin muodostettava Azure.

**Muodosta yhteys tietoihin järvi Analytics**

1. Avaa Visual Studio.
2. Valitse **Tietojen järvi** -valikossa **asetusten**.
4. Valitse **Kirjaudu sisään**tai **Muuta käyttäjien** , jos käyttäjä on kirjautunut sisään, ja noudata ohjeita.
5. Valitse **OK** ja sulje asetusten valintaikkunassa.

**Voit selata tietojen järvi Analytics-tilit**

1. Avaa Visual Studio **Palvelimen Explorer** painamalla **CTRL + ALT + S**.
2. **Palvelimen Explorer**Laajenna **Azure**ja laajenna sitten **Tietojen järvi Analytics**. Näet on luettelon tietojen järvi Analytics-tileistä, jos niitä on. Et voi luoda tietojen järvi Analytics-tilit Studio. Luo tili-artikkelissa [Azure tietojen järvi Analytics Azure-portaalissa aloittaminen](data-lake-analytics-get-started-portal.md) tai [Azure PowerShellin Azure tietojen järvi Analytics käytön aloittaminen](data-lake-analytics-get-started-powershell.md).

## <a name="develop-u-sql-application"></a>Kehittää U SQL-sovellus

U-SQL-sovellus on enimmäkseen U-SQL-komentosarja. Lisätietoja U-SQL-kohdassa [U SQL käytön aloittaminen](data-lake-analytics-u-sql-get-started.md).

Voit lisätä sovelluksen lisäämisen käyttäjän määrittämiä operaattoreita.  Lisätietoja on artikkelissa [kehittää U-SQL käyttäjän määrittämät tiedot järvi Analytics työt operaattorit](data-lake-analytics-u-sql-develop-user-defined-operators.md).

**Voit luoda ja lähettää tiedot järvi Analytics-työ**

1. **Tiedosto** -valikosta **Uusi**ja valitse sitten **Projekti**.
2. Valitse U-SQL-projektin tyyppi.

    ![Uusi Visual Studio U-SQL-projekti](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)

3. Valitse **OK**. Visual studio Luo ratkaisun Script.usql-tiedoston.
4. Kirjoita seuraavaa komentosarjaa Script.usql-tiedostoon:

        // Create a database for easy reuse, so you don't need to read from a file every time.
        CREATE DATABASE IF NOT EXISTS SampleDBTutorials;

        // Create a Table valued function. TVF ensures that your jobs fetch data from the weblog file with the correct schema.
        DROP FUNCTION IF EXISTS SampleDBTutorials.dbo.WeblogsView;
        CREATE FUNCTION SampleDBTutorials.dbo.WeblogsView()
        RETURNS @result TABLE
        (
            s_date DateTime,
            s_time string,
            s_sitename string,
            cs_method string,
            cs_uristem string,
            cs_uriquery string,
            s_port int,
            cs_username string,
            c_ip string,
            cs_useragent string,
            cs_cookie string,
            cs_referer string,
            cs_host string,
            sc_status int,
            sc_substatus int,
            sc_win32status int,
            sc_bytes int,
            cs_bytes int,
            s_timetaken int
        )
        AS
        BEGIN

            @result = EXTRACT
                s_date DateTime,
                s_time string,
                s_sitename string,
                cs_method string,
                cs_uristem string,
                cs_uriquery string,
                s_port int,
                cs_username string,
                c_ip string,
                cs_useragent string,
                cs_cookie string,
                cs_referer string,
                cs_host string,
                sc_status int,
                sc_substatus int,
                sc_win32status int,
                sc_bytes int,
                cs_bytes int,
                s_timetaken int
            FROM @"/Samples/Data/WebLog.log"
            USING Extractors.Text(delimiter:' ');
            RETURN;
        END;

        // Create a table for storing referrers and status
        DROP TABLE IF EXISTS SampleDBTutorials.dbo.ReferrersPerDay;
        @weblog = SampleDBTutorials.dbo.WeblogsView();
        CREATE TABLE SampleDBTutorials.dbo.ReferrersPerDay
        (
            INDEX idx1
            CLUSTERED(Year ASC)
            PARTITIONED BY HASH(Year)
        ) AS

        SELECT s_date.Year AS Year,
            s_date.Month AS Month,
            s_date.Day AS Day,
            cs_referer,
            sc_status,
            COUNT(DISTINCT c_ip) AS cnt
        FROM @weblog
        GROUP BY s_date,
                cs_referer,
                sc_status;

    U-SQL on artikkelissa [tietojen järvi Analytics U-SQL-kielen käytön aloittaminen](data-lake-analytics-u-sql-get-started.md).    

5. Uuden U-SQL-komentosarjan lisääminen projektiin ja syötä seuraavat:

        // Query the referrers that ran into errors
        @content =
            SELECT *
            FROM SampleDBTutorials.dbo.ReferrersPerDay
            WHERE sc_status >=400 AND sc_status < 500;

        OUTPUT @content
        TO @"/Samples/Outputs/UnsuccessfulResponses.log"
        USING Outputters.Tsv();

6. Siirry takaisin ensimmäisen U-SQL-komentosarja ja **Lähetä** -painikkeen vieressä, Määritä Analytics-tilillesi.
7. **Ratkaisunhallinnassa** **Script.usql**napsauttamalla hiiren kakkospainikkeella ja valitse sitten **Luo komentosarjan**. Tarkista tulokset tulostus-ruudussa.
8. **Ratkaisunhallinnassa** **Script.usql**napsauttamalla hiiren kakkospainikkeella ja valitse sitten **Lähetä komentosarjan**.
9. Tarkista **Analytics-tili** on sitä kohtaa, johon haluat suorittaa työn, ja valitse sitten **Lähetä**. Lähetyksen tulokset ja työn linkki ovat käytettävissä tietojen järvi Tools for Visual Studio tulokset-ikkunan, kun lähetys on valmis.
10. Odota, kunnes projekti on suoritettu.  Jos työ epäonnistui-lähdetiedosto todennäköisesti puuttuu.  Katso tämä opetusohjelma valmistelevat osa. Saat lisätietoja vianmäärityksestä [näyttö ja vianmääritys Azure tietojen järvi Analytics työt](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).

    Kun työ on valmis, näet on seuraavassa kuvassa:

    ![tietoja järvi analytics analysoida Web-päiväkirjoja sivuston lokit](./media/data-lake-analytics-analyze-weblogs/data-lake-analytics-analyze-weblogs-job-completed.png)

11. Toista vaiheet 7 – 10 nyt **Script1.usql**.

>[AZURE.NOTE]Ei voi lukea tai kirjoittaa U-SQL-taulukko, joka on luotu tai muokata saman komentosarjan.  Minkä vuoksi käyttää kaksi komentosarjoja, tässä esimerkissä.

**Jos haluat nähdä projektin tulos**

1. **Palvelimen Explorer**Laajenna **Azure**, laajenna **Tietojen järvi Analytics**, laajenna tietojen järvi Analytics-tilisi, laajenna **Tallennustilan tilit**, järvi tietosäilö oletustilin hiiren kakkospainikkeella ja valitse **Explorer**.
2.  Kaksoisnapsauta **näytteiden** , Avaa kansio ja kaksoisnapsauta sitten **tulostaa**.
3.  Kaksoisnapsauta **UnsuccessfulResponsees.log**.
4.  Jotta voit siirtyä suoraan tulosteen myös kaksoisnapsauttamalla kohdetiedosto sisällä työn kaavionäkymä.

## <a name="see-also"></a>Katso myös

Aloita tietojen järvi Analytics eri työkaluilla, katso:

- [Aloita tietojen järvi Analytics Azure-portaalissa](data-lake-analytics-get-started-portal.md)
- [Aloita tietojen järvi Analytics Azure PowerShellin avulla](data-lake-analytics-get-started-powershell.md)
- [Käyttämällä .NET SDK tietojen järvi Analytics käytön aloittaminen](data-lake-analytics-get-started-net-sdk.md)

Jos haluat nähdä kehittäminen aiheita:

- [U-SQL-komentosarjojen käyttämällä järvi Datatyökalut Visual Studio kehittäminen](data-lake-analytics-data-lake-tools-get-started.md)
- [Azure tietojen järvi Analytics U-SQL-kielen käytön aloittaminen](data-lake-analytics-u-sql-get-started.md)
- [Kehittää tietojen järvi Analytics työt U SQL käyttäjän määrittämät operaattorit](data-lake-analytics-u-sql-develop-user-defined-operators.md)
