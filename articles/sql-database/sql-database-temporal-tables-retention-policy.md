<properties
   pageTitle="Historiallisten tietojen ajallinen taulukoiden säilytyskäytäntö hallinta | Microsoft Azure"
   description="Opettele käyttämään ajallinen säilytyskäytäntö pitämään niin, että historiatiedoissa."
   services="sql-database"
   documentationCenter=""
   authors="bonova"
   manager="drasumic"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sql-database"
   ms.date="10/12/2016"
   ms.author="bonova"/>

#<a name="manage-historical-data-in-temporal-tables-with-retention-policy"></a>Historiallinen ja säilytyskäytäntö ajallinen taulukoiden tietojen hallinta

Ajallinen taulukot saattavat parantua tietokannan koko on suurempi kuin tavallisten taulukoiden, etenkin silloin, kun historiatietoja säilyttää pidemmän aikaa. Näin ollen historiatietoja säilytyskäytäntö on tärkeää näkökohdat suunnittelu ja hallinta ajallinen jokaisen taulukon elinkaari. Azure SQL-tietokantaan ajallinen taulukot sisältyvät helposti käytettävällä säilytys järjestelmä, jonka avulla voit suorittaa tämän tehtävän.

Ajallinen historia säilytys on määritetty yksittäisen taulukon tasolla, jonka avulla käyttäjät voivat luoda joustavia erääntyminen käytännöt. Käyttämällä ajallinen säilytys on helppoa: se vaatii vain yksi parametri on määritettävä aikana taulukon luominen tai rakenteen muuttaminen.

Kun olet määrittänyt säilytyskäytännön, Azure SQL-tietokanta käynnistyy tarkistetaan säännöllisesti, onko historiallisten rivit, jotka soveltuvat automaattisen tietojen uudelleenjärjestäminen. Vastaavat rivit tunnistamista ja niiden poistaminen siitä Historia-taulukosta ilmetä läpinäkyvä, taustalla tehtävä on ajoitettu ja järjestelmä suorittaa. Muutosloki-taulukon rivien ikä-ehto on valittuna edustava SYSTEM_TIME kauden päättymispäivä sarakkeen perusteella. Jos säilytysajan jälkeinen aika-koodi on määritetty esimerkiksi kuusi kuukautta, taulukkorivit oikeuta uudelleenjärjestäminen täytettävä seuraava ehto:

````
ValidTo < DATEADD (MONTH, -6, SYSUTCDATETIME())
````

Yllä olevassa esimerkissä on oletetaan, että **ValidTo** sarake vastaa SYSTEM_TIME kauden lopussa.

##<a name="how-to-configure-retention-policy"></a>Säilytyskäytännön määrittämisestä?

Ennen kuin määrität ajallinen taulukon säilytyskäytännön, tarkista ensin, onko ajallinen historiallisten säilytys käytössä *tietokannan tasolla*.

````
SELECT is_temporal_history_retention_enabled, name
FROM sys.databases
````

Tietokannan merkinnän **is_temporal_history_retention_enabled** on määritetty oletusarvoisesti käytössä, mutta käyttäjät voivat muuttaa kanssa ALTER DATABASE-lause. Se myös automaattisesti asetetaan käytöstä [valitsemalla aika palauttaminen](sql-database-point-in-time-restore-portal.md) toiminnon jälkeen. Ajallinen historia säilytys uudelleenjärjestäminen tietokannan käyttöön Suorita seuraava komento:

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

> [AZURE.IMPORTANT] Voit määrittää säilytys ajallinen taulukoiden, vaikka **is_temporal_history_retention_enabled** on ei käytössä, mutta automaattisen uudelleenjärjestäminen erääntyneet rivien ei käynnistyy tällöin.


Säilytyskäytännön on määritetty taulukon luonnissa määrittämällä HISTORY_RETENTION_PERIOD-parametrin arvon:

````
CREATE TABLE dbo.WebsiteUserInfo
(  
    [UserID] int NOT NULL PRIMARY KEY CLUSTERED
  , [UserName] nvarchar(100) NOT NULL
  , [PagesVisited] int NOT NULL
  , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )  
 WITH
 (
     SYSTEM_VERSIONING = ON
     (
        HISTORY_TABLE = dbo.WebsiteUserInfoHistory,
        HISTORY_RETENTION_PERIOD = 6 MONTHS
     )
 );
````

Azure SQL-tietokannan avulla voit määrittää säilytysaika käyttämällä eri aikayksiköt: päivää, viikkoa, kuukautta ja vuotta. Jos HISTORY_RETENTION_PERIOD jätetään pois, oletusarvona on ÄÄRETTÖMÄN säilytys. Voit käyttää ÄÄRETTÖMÄN avainsanan myös erikseen.

Joissakin tilanteissa haluat ehkä määrittää säilytys taulukon luomisen jälkeen tai muuttaa aiemmin määritetty arvo. Tässä tapauksessa käytä ALTER TABLE-lause:

````
ALTER TABLE dbo.WebsiteUserInfo
SET (SYSTEM_VERSIONING = ON (HISTORY_RETENTION_PERIOD = 9 MONTHS));
````

> [AZURE.IMPORTANT]  Asettaminen SYSTEM_VERSIONING käytöstä *ei säilytä* säilytys kauden arvo. SYSTEM_VERSIONING asettaminen edelleen ilman HISTORY_RETENTION_PERIOD määritetyn erikseen tulokset ÄÄRETTÖMÄN säilytysaika.

Tarkistettava nykyisen tilan säilytyskäytännön käyttäminen seuraavan kyselyn, joka yhdistää ajallinen säilytys mahdollistamisen merkinnän säilytys kausilla yksittäisiä taulukkoja tietokannan tasolla:

````
SELECT DB.is_temporal_history_retention_enabled,
SCHEMA_NAME(T1.schema_id) AS TemporalTableSchema,
T1.name as TemporalTableName,  SCHEMA_NAME(T2.schema_id) AS HistoryTableSchema,
T2.name as HistoryTableName,T1.history_retention_period,
T1.history_retention_period_unit_desc
FROM sys.tables T1  
OUTER APPLY (select is_temporal_history_retention_enabled from sys.databases
where name = DB_NAME()) AS DB
LEFT JOIN sys.tables T2   
ON T1.history_table_id = T2.object_id WHERE T1.temporal_type = 2
````


##<a name="how-sql-database-deletes-aged-rows"></a>SQL-tietokantaan poistotapaa tilanne rivit?

Uudelleenjärjestäminen prosessin määräytyy indeksi asettelun historia-taulukkoa. On tärkeää huomaat, että *vain historia-taulukoita, joissa on liitetty indeksi (B puun tai columnstore) voi olla rajallinen säilytyskäytäntö määritetty*. Taustatehtävän luodaan suorittamiseen erääntyneet tietojen uudelleenjärjestäminen kaikki ajallinen taulut, joihin säilytysajan rajallinen.
Uudelleenjärjestäminen logiikan rowstore (puun B) liitetty indeksin poistaa erääntyneet pienempi joukkojen (enintään 10 K)-rivin pienentäminen paineita lokeista ja i/o: n alijärjestelmää. Vaikka uudelleenjärjestäminen logiikan käyttämällä vaaditun B puun indeksin poistot vanhempi kuin säilytysaika et voi taata kunnolla rivien järjestystä. Näin ollen *eivät tule mitään riippuvuuden uudelleenjärjestäminen sovellusten tilauksessa*.

Liitetty columnstore uudelleenjärjestäminen tehtävä poistaa [rivin ryhmien](https://msdn.microsoft.com/library/gg492088.aspx) samalla kertaa (sisältävät yleensä kunkin rivin 1 miljoona), joka on erittäin tehokkaasti, erityisesti silloin, kun historiatietoja luodaan suuri tahdissa.

![Liitetty columnstore säilytys](./media/sql-database-temporal-tables-retention-policy/cciretention.png)


Erinomainen tietojen pakkaus ja tehokkaan säilytys uudelleenjärjestäminen tekee liitetyt columnstore indeksi sopivaa vaihtoehtoa skenaarioissa kun havainnollistamiseen Luo nopeasti historiatietoja suuri määrä. Kyseisen kuvio on tyypillinen tehostettu [tapahtumien käsittely toiminnoista, jotka käyttävät ajallinen taulukoiden](https://msdn.microsoft.com/library/mt631669.aspx) muutosten seuranta ja valvonta, trendianalyysin tai IoT tietojen nieltynä.

##<a name="index-considerations"></a>Indeksi huomioon otettavia seikkoja

Uudelleenjärjestäminen tehtävä taulukoiden rowstore liitetty indeksi edellyttää indeksin, aloittaa SYSTEM_TIME jakson vastaavan sarakkeen. Jos indeksiä ei ole valittu, et voi määrittää rajallinen säilytysaika:

*Virhesanoma 13765, tason 16 tila 1 <br> </br> järjestelmän versiotietoja ajallinen taulukon 'temporalstagetestdb.dbo.WebsiteUserInfo' rajallinen säilytysaika määrittäminen epäonnistui, koska historiataulukossa 'temporalstagetestdb.dbo.WebsiteUserInfoHistory' ei ole liitetty indeksi pakollinen. Liitetty columnstore tai alkaen sarakkeesta, joka vastaa SYSTEM_TIME loppuun B puun indeksin luominen kauden Muutosloki-taulukon.*

On tärkeää huomaat, että luoma Azure SQL-tietokanta jo oletusarvo historiataulukossa on liitetty indeksi, joka on yhteensopiva säilytyskäytäntö. Jos yrität poistaa taulukon, jossa on rajallinen säilytysaika, indeksi-toiminto epäonnistuu ja seuraava virhesanoma:

*Virhesanoma 13766, tason 16 tila 1 <br> </br> ei voi poistaa ryhmitellyn indeksin 'WebsiteUserInfoHistory.IX_WebsiteUserInfoHistory', koska se on käytössä automaattinen uudelleenjärjestäminen erääntyneet tiedot. Harkitse asetukset HISTORY_RETENTION_PERIOD ÄÄRETTÖMÄN vastaavan järjestelmän versiotietoja ajallinen taulukon, jos haluat tämän indeksin pudota.*

Liitetty columnstore indeksoinnin uudelleenjärjestäminen toimii optimaalisesti, jos historiallisten rivejä on lisätty nousevaan järjestykseen (järjestetty kausisarake lopussa), joka on aina sen jälkeen tapausta, kun SYSTEM_VERSIONIOING järjestelmä täyttänyt historia-taulukkoa. Jos Muutosloki-taulukon rivit ei järjestetään mukaan loppuun kausisarake (joka voi olla tapauksessa, jos olet siirtänyt aiemmin historiatietoja), sinun on luotava uudelleen liitetty columnstore indeksi B puun rowstore indeksin, joka on oikein vyöhykkeen, saavuttamiseksi optimaalisen päälle.

Vältä historiataulukko, jossa rajallinen säilytysaika liitetty columnstore indeksi muodostetaan uudelleen, koska voivat muuttua järjestys rivin ryhmien luonnollisesti asettamia järjestelmän versiotiedot-toiminto. Jos haluat muodostaa liitetty columnstore indeksi historiataulukossa, voit tehdä sen luomalla uudelleen sen, yhteensopiva B puun indeksi säilyttäminen järjestys tarvittavat säännöllisesti tietojen uudelleenjärjestäminen rowgroups päälle. Samoin olisi otettava, jos ajallinen taulukon luominen aiemmin luotuun historia-taulukkoon, joka on liitetty sarakkeen indeksiä ei ole taattua tiedot järjestyksessä:

````
/*Create B-tree ordered by the end of period column*/
CREATE CLUSTERED INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory (ValidTo)
WITH (DROP_EXISTING = ON);
GO
/*Re-create clustered columnstore index*/
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON);
````

Kun äärellisestä säilytysaika on määritetty liitetty columnstore indeksi historiataulukossa, ei voi luoda uusia ei ole liitetty B puun indeksit taulukon:

````
CREATE NONCLUSTERED INDEX IX_WebHistNCI ON WebsiteUserInfoHistory ([UserName])
````

Yritetään suorittaa edellä lauseen epäonnistuu ja seuraava virhesanoma:

*Virhesanoma 13772, tason 16 tila 1 <br> </br> indeksiä ei voi luoda ei ole liitetty ajallinen historia-taulukkoa 'WebsiteUserInfoHistory' koska se on rajallinen säilytysaika ja liitetty columnstore indeksi on määritetty.*

##<a name="querying-tables-with-retention-policy"></a>Kyselyt taulukoita, joissa säilytyskäytäntö

Kaikki kyselyt ajallinen taulukon suodattaminen automaattisesti historiallisten rivien vastaavat rajallinen säilytyskäytännön, voit välttää odottamattomat ja epäyhtenäisiä tuloksia, koska erääntyneet rivit voivat poistaa uudelleenjärjestäminen tehtävästä, *milloin tahansa ajoissa ja haluamaansa järjestyksessä*.

Seuraava kuva esittää yksinkertaisen kyselyn kyselyn suunnitteleminen:

````
SELECT * FROM dbo.WebsiteUserInfo FROM SYSTEM_TIME ALL;
````

Kyselysuunnitelma sisältää muita suodatinta on käytetty kausisarake (ValidTo) loppuun (korostettuna) Muutosloki-taulukon "Liitetty indeksi skannata"-operaattoria. Tässä esimerkissä oletetaan, että 1 kuukausi säilytysaika määritettiin WebsiteUserInfo taulukon.

![Säilytys-kysely-suodatin](./media/sql-database-temporal-tables-retention-policy/queryexecplanwithretention.png)

Jos testaat historiataulukossa suoraan, voit nähdä rivit, jotka ovat vanhempia kuin määritetty säilytys kausi, mutta ilman mitään takuu toistettavien kyselyn tuloksiin. Seuraavassa kuvassa on Muutosloki-taulukon, kyselyn suorittaminen kyselyn suunnitteleminen ilman muita suodattimet:

![Kyselyt historia ilman säilytys suodatin](./media/sql-database-temporal-tables-retention-policy/queryexecplanhistorytable.png)

Ei riippuvaisia organisaatiosi liiketoimintalogiikan lukeminen lisäksi säilytysaika historiataulukossa, näkyviin voi tulla epäyhtenäisiä tai odottamattomia tuloksia, kun. On suositeltavaa ajallinen kyselyjen käyttäminen FOR SYSTEM_TIME lauseen ajallinen taulukoiden tietojen analysointia varten.

##<a name="point-in-time-restore-considerations"></a>Valitse aika palauttaminen huomioon otettavia seikkoja

Kun luot uuden tietokannan palauttamalla [luodun tietokannan tiettyyn kohtaan samanaikaisesti](sql-database-point-in-time-restore-portal.md), se on ajallinen säilytys käytöstä tietokannan tasolla. (**is_temporal_history_retention_enabled** merkinnän asetukseksi ei käytössä). Tämän toiminnon avulla voi tutkia kaikkien historiallisten rivien palauttaminen, kun tarvitsematta erääntyneet rivit poistetaan, ennen kuin voit siirtyä kyselyn ne. Voit käyttää sitä on *rajattu hiusristikon sisään historiatietoja määritetyn säilytys päättymisen jälkeen*.

Sano ajallinen taulukolla on yksi kuukausi säilytys tietyn ajanjakson ajalta. Jos tietokanta on luotu Premium-palvelu taso, sinun olisi voivat luoda tietokannan kopion tietokannan tilan määrittäminen 35 päivää taaksepäin menneisyydessä. Tehokkaasti tukevat avulla voit analysoida historiallisten rivit, jotka ovat enintään 65 päivän kyselyt historiataulukossa suoraan.

Jos haluat aktivoida ajallinen säilytys uudelleenjärjestäminen, suorita seuraava Transact-SQL-lause pilkun jälkeen aika palauttaminen:

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

##<a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja ajallinen taulukoiden käyttäminen sovellusten uloskuittauksen [Ajallinen taulukoita SQL Azure-tietokannan käytön aloittaminen](sql-database-temporal-tables.md).

Käy kanavan 9 [todellisia asiakkaiden ajallinen käyttöönoton onnistumisen Tarinan](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) ja katso [live ajallinen esittely](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).

Lisätietoja ajallinen taulukot Tarkista [MSDN-ohjeista](https://msdn.microsoft.com/library/dn935015.aspx).
