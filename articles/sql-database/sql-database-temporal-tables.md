<properties
   pageTitle="Käytön aloittaminen ajallinen Azure SQL-tietokannan taulukoiden | Microsoft Azure"
   description="Opettele ajallinen taulukoiden käyttäminen Azure SQL-tietokannan käytön aloittaminen."
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sql-database"
   ms.date="08/29/2016"
   ms.author="carlrab"/>

#<a name="getting-started-with-temporal-tables-in-azure-sql-database"></a>Käytön aloittaminen ajallinen taulukoiden Azure SQL-tietokantaan

Ajallinen taulukkojen Azure SQL-tietokanta, jonka avulla voit seurata ja analysoida koko muutoshistorian tiedoissa ilman edellyttää mukautetun coding ohjelmoitavuus uusi ominaisuus. Ajallinen taulukoiden pitää läheisesti toisiinsa ajan kontekstiin, niin, että tallennettuja tietoja voidaan tulkita tiedot on voimassa vain tietyn ajan kuluessa. Tämän ominaisuuden ajallinen taulukoiden avulla tehokas aikapohjaisten analysointia ja hakeminen tietojen kehitys-tiedot.

##<a name="temporal-scenario"></a>Ajallinen skenaario

Tässä artikkelissa kerrotaan vaihe vaiheelta, miten käyttämiseen ajallinen taulukoiden sovelluksen skenaariossa. Oletetaan, että haluat käyttäjän seuraaminen uuteen sivustoon, joka on parhaillaan kehitetty alusta alkaen tai olemassa olevan verkkosivuston, jotka haluat siirtää käyttäjän toiminnan analytics kanssa. Vaivaton tässä esimerkissä on oletetaan, että tietyn ajanjakson aikana avattujen web-sivujen määrä on ilmaisin, joka tallennetaan ja valvoa sivustoon tietokannan, joka sijaitsee Azure SQL-tietokanta on. Historiallinen analyysin käyttäjän toiminnan lähinnä hakemaan syötteitä sivuston muuttamiseen ja antaa parempia kokemus Vierailijan.

Tässä skenaariossa tietokantamallin on kuvattu yksinkertainen – käyttäjän toiminnan metrijärjestelmä yksittäisen kokonaislukukenttä **PageVisited**, jossa esitetään ja kirjataan sekä perustietoja käyttäjäprofiili. Lisäksi aikaa analyysia varten säilyttää sarjan rivejä kunkin käyttäjän kohdalla missä jokaisella rivillä kuvaa tietyn käyttäjän avoimelle tietyn ajanjakson aikana sivujen määrä.

![Rakenne](./media/sql-database-temporal-tables/AzureTemporal1.png)

Lähettäjä sinun ei tarvitse sijoittaa minkä tahansa työmäärään sovelluksesi, jos haluat säilyttää tehtävätiedot. Ajallinen taulukoita Tämä toimenpide on automaattinen - antamalla koko joustavuutta sivuston suunnitteluun ja enemmän aikaa itse Tietoanalyysi keskittyminen aikana. Riittää, sinun on tehtävä on varmistaa, että **WebSiteInfo** taulukko on määritetty [ajallinen järjestelmän versiotietoja](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_0). Tarkat vaiheet käyttämiseen ajallinen taulukoiden tässä skenaariossa on kuvattu seuraavassa.

##<a name="step-1-configure-tables-as-temporal"></a>Vaihe 1: Määritä taulukoiden ajallinen

Sen mukaan, käynnistetään uusi kehittäminen vai aiemmin luotuun sovellukseen päivittämisen sisällytetään ajallinen taulukoiden luominen tai muokata aiemmin luotuja lisäämällä ajallinen määritteet. Yleinen tapauksessa käyttämässäsi skenaariossa voivat olla sekoitus näistä vaihtoehdoista. Suorita nämä [SQL Server Management Studiossa](https://msdn.microsoft.com/library/mt238290.aspx) (SSMS), [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) (SSDT) tai kaikki muut Transact-SQL-kehitystyökalun toiminto.


> [AZURE.IMPORTANT] On suositeltavaa, että käytät Management Studiossa uusimman version aina pysyvän synkronoituja Microsoft Azure ja SQL-tietokantaan. [Päivitä SQL Server Management Studiossa](https://msdn.microsoft.com/library/mt238290.aspx).


###<a name="create-new-table"></a>Luo uusi taulukko

SSMS Object Explorer pikavalikon vaihtoehtoa "Uuden järjestelmän versiotietoja taulukon" avulla voit avata kyselyeditori ajallinen taulukon mallin komentosarjan ja täytä mallin "Määrittää arvot, Malliparametrit" (Ctrl + VAIHTO + M) avulla:

![SSMSNewTable](./media/sql-database-temporal-tables/AzureTemporal2.png)

SSDT, valitse on "Ajallinen taulukon (järjestelmä-versio)-mallin, kun lisäät tietokannan projektin uudet kohteet. Joka avaa taulukon suunnitteluohjelman ja, joiden avulla voit määrittää helposti taulukon asettelu:

![SSDTNewTable](./media/sql-database-temporal-tables/AzureTemporal3.png)

Voit myös Luo ajallinen taulukon määrittämällä Transact-SQL-lauseita suoraan, kuten alla olevassa esimerkissä. Huomaa, että jokaisen ajallinen taulukon pakollinen osat ovat KAUDEN määritys ja SYSTEM_VERSIONING-lause, joka viittaa toisen Käyttäjätaulukko, joka tallentaa historiallisten rivin versioita:

````
CREATE TABLE WebsiteUserInfo 
(  
    [UserID] int NOT NULL PRIMARY KEY CLUSTERED 
  , [UserName] nvarchar(100) NOT NULL
  , [PagesVisited] int NOT NULL 
  , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )  
 WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.WebsiteUserInfoHistory));
````

Kun luot järjestelmän versiotietoja ajallinen taulukon, mukana historiataulukko, jossa oletusarvo-määritysten luodaan automaattisesti. Oletusarvon historiataulukossa sisältää sivun pakkaaminen käyttöön liitetty B puun indeksi kauden sarakkeiden (loppuun, Käynnistä). Tässä määrityksessä on useimpien skenaariot, jossa ajallinen taulukoiden käytetään, erityisesti niiden [tietojen seurannassa](https://msdn.microsoft.com/library/mt631669.aspx#Anchor_0). 

Tässä tapauksessa emme pyritään analysoida aikapohjaisten trendi päälle pidempi tietojen historia ja suurentaa tietojoukot niin historiataulukossa tallennustilan-valinta on liitetty columnstore indeksi. Liitetty columnstore on erittäin hyvä pakkaus ja analytical kyselyjen suorituskykyä. Ajallinen taulukoiden avulla voit joustavasti määrittämistä indeksit nykyisen ja ajallinen taulukot kokonaan erikseen. 

**Huomautus**: Columnstore indeksit ovat vain käytettävissä premium-palvelutaso.

Seuraavaa komentosarjaa näkyy, miten liitetty columnstore voi muuttaa oletusarvon indeksi historiataulukossa:

````
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory
ON dbo.WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON); 
````

Objekti-Explorerissa lisäämällä, on helppo tunnistaa tiettyä kuvaketta edustaa ajallinen taulukot samalla, kun sen Muutosloki-taulukon kohde näkyy alisolmu.

![AlterTable](./media/sql-database-temporal-tables/AzureTemporal4.png)

###<a name="alter-existing-table-to-temporal"></a>Muuta ajallinen olemassa oleva taulukko

Kannen oletetaan, että vaihtoehtoinen skenaario, jossa WebsiteUserInfo taulukon jo olemassa, mutta ei ole suunniteltu pitämään historian muutoksista. Tässä tapauksessa voit laajentaa vain aiemmin luodun taulukon luomisella ajallinen, kuten seuraavassa esimerkissä:

````
ALTER TABLE WebsiteUserInfo 
ADD 
    ValidFrom datetime2 (0) GENERATED ALWAYS AS ROW START HIDDEN  
        constraint DF_ValidFrom DEFAULT DATEADD(SECOND, -1, SYSUTCDATETIME())
    , ValidTo datetime2 (0)  GENERATED ALWAYS AS ROW END HIDDEN   
        constraint DF_ValidTo DEFAULT '9999.12.31 23:59:59.99'
    , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo); 

ALTER TABLE WebsiteUserInfo  
SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.WebsiteUserInfoHistory));
GO

CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory
ON dbo.WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON); 
````

##<a name="step-2-run-your-workload-regularly"></a>Vaihe 2: Suorita havainnollistamiseen säännöllisesti

Ajallinen taulukoiden tärkeimmät etuna on se, että sinun ei tarvitse muuttaa tai muuta sivuston minkäänlaista suorittamiseen muutosten seuranta. Luomiasi ajallinen taulukoiden jatkuvat läpinäkyvä rivin aiempia aina, kun teet muutoksia tietoihisi. 

Jotta voit hyödyntää automaattisten muutosten jäljityksen tietyn artikkelista oletetaan, että vain päivittää sarakkeen **PagesVisited** aina, kun käyttäjä lopettaa voidakseen istunnon sivustossa:

````
UPDATE WebsiteUserInfo  SET [PagesVisited] = 5 
WHERE [UserID] = 1;
````

On tärkeää huomaat, että päivityskysely ei tarvitsee tietää täsmällisen kellonajan, kun todellinen toiminto tapahtui eikä miten historiatietoja säilytetään tulevien analysointia varten. Azure SQL-tietokanta automaattisesti käsittelemien sekä ominaisuuksia. Seuraavassa kaaviossa on kuvattu, miten historiatiedot luodaan jokaisen päivityksen yhteydessä.

![TemporalArchitecture](./media/sql-database-temporal-tables/AzureTemporal5.png)

##<a name="step-3-perform-historical-data-analysis"></a>Vaihe 3: Historiatietoja analysoida

Kun ajallinen järjestelmä-versiotiedot ovat käytössä, historiallisten tietojen analysointi on nyt yhden kyselyn olet poissa. Tässä artikkelissa annamme muutamia esimerkkejä kyseinen osoite yleisiä analyysi tilanteita, joissa - kaikki tietoja, tutustu eri vaihtoehtoja, jotka on otettu käyttöön [FOR SYSTEM_TIME](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_3) -lause.

Saat Ylin 10 käyttäjää vyöhykkeen verran vuodesta tunnin sitten web-sivut, suorita tämä kysely:

````
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
SELECT TOP 10 * FROM dbo.WebsiteUserInfo FOR SYSTEM_TIME AS OF @hourAgo
ORDER BY PagesVisited DESC
````

Voit helposti muokata analysointiin saavuttaman päivä, joka on sitten-sivusto-vierailut kuukausi sitten tämä kysely tai haluat aiemman milloin tahansa.

Voit suorittaa basic tilastoanalyyseja edelliseen päivään, käytä seuraavassa esimerkissä:

````
DECLARE @twoDaysAgo datetime2 = DATEADD(DAY, -2, SYSUTCDATETIME());
DECLARE @aDayAgo datetime2 = DATEADD(DAY, -1, SYSUTCDATETIME());

SELECT UserID, SUM (PagesVisited) as TotalVisitedPages, AVG (PagesVisited) as AverageVisitedPages,
MAX (PagesVisited) AS MaxVisitedPages, MIN (PagesVisited) AS MinVisitedPages,
STDEV (PagesVisited) as StDevViistedPages
FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME BETWEEN @twoDaysAgo AND @aDayAgo
GROUP BY UserId
````

Voit etsiä tietyn käyttäjän toiminnan ajanjakson aikana, käytä SISÄLSI IN-lause:

````
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
DECLARE @twoHoursAgo datetime2 = DATEADD(HOUR, -2, SYSUTCDATETIME());
SELECT * FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME CONTAINED IN (@twoHoursAgo, @hourAgo)
WHERE [UserID] = 1;
````

Kuvan visualisointi on hyötyä ajallinen kyselyjen kuin voit näyttää kehityksen ja käyttötavat intuitiivisella tavalla hyvin helposti:

![TemporalGraph](./media/sql-database-temporal-tables/AzureTemporal6.png)

##<a name="evolving-table-schema"></a>Taulukkorakenteen kehittyville

Yleensä tarvitset muuttaa ajallinen taulukkorakennetta, kun teet sovellusten kehittämiseen. Kohdalla Suorita säännöllisesti ALTER TABLE-lauseet ja Azure SQL-tietokanta otettu laajenevat Muutosloki-taulukon muutokset. Seuraavaa komentosarjaa näkyy, kuinka voit lisätä muita määrite seurantaa varten:

````
/*Add new column for tracking source IP address*/
ALTER TABLE dbo.WebsiteUserInfo 
ADD  [IPAddress] varchar(128) NOT NULL CONSTRAINT DF_Address DEFAULT 'N/A';
````

Voit muuttaa sarakemäärityksen vastaavasti havainnollistamiseen ollessa aktiivinen:

````
/*Increase the length of name column*/
ALTER TABLE dbo.WebsiteUserInfo 
    ALTER COLUMN  UserName nvarchar(256) NOT NULL;
````

Lopuksi voit poistaa sarakkeen, joka enää tarvitse.

````
/*Drop unnecessary column */
ALTER TABLE dbo.WebsiteUserInfo 
    DROP COLUMN TemporaryColumn; 
````
    
Voit myös muuttaa ajallinen Taulukkorakenteen, kun olet muodostanut yhteyden tietokantaan (online-tilassa) tai tietokannan projektin (offline-tilassa) osana uusimman [SSDT](https://msdn.microsoft.com/library/mt204009.aspx) avulla.

##<a name="controlling-retention-of-historical-data"></a>Historiallisten tietojen säilytys hallinta

Järjestelmän versiotietoja ajallinen taulukoita historiataulukossa saattavat parantua tietokannan koko tavallisten taulukoiden yli. Suuri ja koskaan kasvava historia-taulukkoa voi olla molemmat vuoksi täysin tallennustilan kustannuksia sekä käyttöön myös suorituskykyä verot ajallinen kyselyt ongelma. Näin ollen kehittäminen varten Muutosloki-taulukon tietojen hallinta tietojen säilytyskäytännön on tärkeää näkökohdat suunnittelu ja hallinta ajallinen jokaisen taulukon elinkaari. Azure SQL-tietokantaan sinulla on hallintaan historiatietoja ajallinen taulukon seuraavilla tavoilla:

- [Taulukon jakaminen](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_2)
- [Mukautetun uudelleenjärjestäminen komentosarja](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_3)

##<a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja ajallinen taulukot Tutustu [MSDN-ohjeista](https://msdn.microsoft.com/library/dn935015.aspx).
Käy kanavan 9 [todellisia asiakkaiden ajallinen implemenation success Tarinan](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) ja katso [live ajallinen esittely](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).
