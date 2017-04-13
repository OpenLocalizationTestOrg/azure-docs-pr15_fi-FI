<properties
   pageTitle="SQL Server-tietokannan siirto SQL-tietokantaan | Microsoft Azure"
   description="Katso, miten paikallisen SQL Serverin tietokantaan siirto Azure SQL-tietokanta pilveen. Testaa yhteensopivuuden ennen tietokannan siirto Tietokantatyökalut siirron avulla."
   keywords="tietokannan siirron, sql server-tietokannan siirron, siirron Tietokantatyökalut, siirrä tietokanta, siirtää sql-tietokantaan"
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
   ms.workload="sqldb-migrate"
   ms.date="08/24/2016"
   ms.author="carlrab"/>

# <a name="sql-server-database-migration-to-sql-database-in-the-cloud"></a>SQL Serverin tietokantaan siirron pilveen SQL-tietokantaan

Tässä artikkelissa kerrotaan, kuinka paikallisen SQL Server 2005: n tai uudemman version tietokannan siirtäminen Azure SQL-tietokanta. Tämä tietokanta siirron prosessi voit siirtää rakenteen ja tietojen SQL Server-tietokannasta nykyisen ympäristössäsi SQL-tietokantaan. Onnistuu, aiemmin luotuun tietokantaan on ensin välitettävä yhteensopivuus-testi. [SQL-tietokannan Vipuventtiili V12](sql-database-v12-whats-new.md), jossa on jäljellä olevan yhteensopivuus, kuin palvelintason ja rajat-tietokannan toimintoihin liittyvät ongelma. Tietokantojen ja sovelluksia, jotka käyttävät [osittain tai funktioita, joita ei tueta](sql-database-transact-sql-information.md) on joitakin uudelleen suunnittelu korjattava näiden yhteensopivuusongelmien, ennen kuin SQL Server-tietokanta voidaan siirtää.

Siirtämisestä on seuraavassa on ohjeita sinulle mahdollisuuden käyttää:

- **Yhteensopivuus Tarkista**: Tarkista tietokannan [SQL-tietokannan Vipuventtiili V12](sql-database-v12-whats-new.md)yhteensopivuus. 
- **Korjaa yhteensopivuusongelmia, jos sellainen**: Jos vahvistus epäonnistuu, sinun on korjattava vahvistusvirheet.  
- **Suorita siirto** Kun tietokanta on yhteensopiva, voit käyttää useilla tavoilla siirto. 

SQL Server on suoritettava näistä tehtävistä useilla tavoilla. Tässä artikkelissa on yleiskatsaus käytettävissä olevat menetelmät kunkin tehtävän. Seuraavassa kaaviossa on kuvattu vaiheet ja menetelmistä.

  ![VSSSDT siirron kaavio](./media/sql-database-cloud-migrate/03VSSSDTDiagram.png)
  
 > [AZURE.NOTE] Siirtää SQL Server-tietokantaan, kuten Microsoft Accessin, Sybase, MySQL Oracle ja DB2 Azure SQL-tietokantaan, on artikkelissa [SQL Server Migration Assistant](http://blogs.msdn.com/b/ssma/).

## <a name="database-migration-tools-test-sql-server-database-compatibility-with-sql-database"></a>Siirron Tietokantatyökalut testaaminen SQL Server-tietokannan yhteensopivuus SQL-tietokantaan

Voit esikatsella SQL-tietokantaan yhteensopivuusongelmat, ennen kuin aloitat tietokannan siirtoprosessia-jollakin seuraavista tavoista:

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Päivityksen tukipalvelu](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

- [SQL Server Data Tools for Visual Studio ("SSDT")](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md): SSDT käyttää viimeisimmän yhteensopivuuden sääntöjä esiintyvien SQL-tietokannan Vipuventtiili V12 yhteensopivuusongelmia. Jos yhteensopivuusongelmia havaita, voit korjata suoraan tässä työkalussa havaittuja ongelmia. Tämä menetelmä on suositellut ja korjaa SQL-tietokannan Vipuventtiili V12 yhteensopivuusongelmat. 
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md): SqlPackage on komentorivin apuohjelma, joka testaa yhteensopivuuden ongelmat ja muodostaa raportin sisältävä olemassa yhteensopivuusongelmat. Jos käytät tätä työkalua, varmista, että uusimman version avulla voit käyttää viimeisimmän yhteensopivuuden säännöt. Jos virheitä löytyy, sinun on käytettävä muuta työkalua korjaa olemassa yhteensopivuusongelmat - suositellaan SSDT.  
- [Ohjattuun Vie tiedot taso-sovelluksen, SQL Server Management Studiossa](sql-database-cloud-migrate-determine-compatibility-ssms.md): ohjattu toiminto tunnistaa ja virheistä näyttöä. Jos virheitä löytyy, voit jatkaa ja SQL-tietokantaan siirtyminen on valmis. Jos virheitä löytyy, sinun on käytettävä muuta työkalua korjaa olemassa yhteensopivuusongelmat - suositellaan SSDT.
- [Microsoft SQL Server 2016 päivittäminen Advisor Preview'n](http://www.microsoft.com/download/details.aspx?id=48119): erillinen työkalun, joka on tällä hetkellä esikatselu, tunnistaa ja muodostaa raportin SQL-tietokannan Vipuventtiili V12 yhteensopivuusongelmia. Tämä työkalu ei ole vielä viimeisimmän yhteensopivuuden säännöt. Jos kieliasussa ei havaita, voit jatkaa ja SQL-tietokantaan siirtyminen on valmis. Jos virheitä löytyy, sinun on käytettävä muuta työkalua korjaa olemassa yhteensopivuusongelmat - suositellaan SSDT. 
- [SQL Azure siirron ohjattu ("SAMW")](sql-database-cloud-migrate-fix-compatibility-issues.md): SAMW on codeplex-työkalua, joka käyttää Azure SQL-tietokannan V11 yhteensopivuuden sääntöjä esiintyvien Azure SQL-tietokannan Vipuventtiili V12 yhteensopivuusongelmia. Jos yhteensopivuusongelmia havaita, asioita, jotka voidaan korjata suoraan tässä työkalussa. Tämä työkalu saattaa olla yhteensopivuusongelma, joita ei tarvitse vahvistaa. Se on ensimmäinen Azure SQL-tietokanta apua siirtotyökalun käytettävissä ja SQL Server-yhteisö tukee aktiivisesti. Tämä työkalu voidaan tehdä myös, että siirron työkalun itse. 

## <a name="fix-database-migration-compatibility-issues"></a>Korjaa tietokanta siirron yhteensopivuusongelmat

Jos yhteensopivuusongelmat havaitaan, sinun on korjattava ennen jatkamista SQL Server-tietokannan siirto. On yhteensopivuusongelmia, jotka eteen voi tulla sen mukaan, sekä erilaisia SQL Server-version lähdetietokannan ja siirryt tietokannan monimutkaisuutta. SQL Server vanhemmat versiot on yhteensopivuusongelman. Käytä kohdennettujen Internet-haku käyttämällä omaa hakukone vaihtoehtojen lisäksi on seuraavissa resursseissa:

- [SQL Server-tietokannan ominaisuuksia ei tueta Azure SQL-tietokantaan](sql-database-transact-sql-information.md)
- [SQL Server 2016 uudistetut tietokannan ohjelma toiminnot](https://msdn.microsoft.com/library/ms144262%28v=sql.130%29)
- [SQL Server 2014 uudistetut tietokannan ohjelma toiminnot](https://msdn.microsoft.com/library/ms144262%28v=sql.120%29)
- [SQL Server 2012: n uudistetut tietokannan ohjelma toiminnot](https://msdn.microsoft.com/library/ms144262%28v=sql.110%29)
- [SQL Server 2008 R2: n uudistetut tietokannan ohjelma toiminnot](https://msdn.microsoft.com/library/ms144262%28v=sql.105%29)
- [SQL Server 2005: n uudistetut tietokannan ohjelma toiminnot](https://msdn.microsoft.com/library/ms144262%28v=sql.90%29)

Lisäksi Internetistä etsimisestä ja käyttämisestä nämä resurssit, käytä [MSDN SQL Server-yhteisön keskustelupalstoihin](https://social.msdn.microsoft.com/Forums/sqlserver/home?category=sqlserver) tai [StackOverflow](http://stackoverflow.com/).

Jompikumpi seuraavista Tietokantatyökalut-siirron avulla voit havaita korjaaminen:

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

- [SQL Server Data Tools for Visual Studio ("SSDT")](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md): Jos haluat käyttää SSDT, tuotavien tietokannan rakenteen SQL Server Data Tools for Visual Studio "SSDT") ja luoda projektin SQL-tietokannan Vipuventtiili V12 käyttöönottoa. Valitse korjaa olemassa yhteensopivuusongelmat SSDT. Kun olet valmis, voit synkronoida muutokset takaisin lähdetietokannan (tai lähdetietokannan kopio. SSDT ei tällä hetkellä Testaa ja korjaa SQL-tietokannan Vipuventtiili V12 yhteensopivuusongelmat suositeltava. [Hallintapaketteihin käyttämällä SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)linkkiä.
- Käytä [SQL Server Management Studiossa ("SSMS")](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md): Jos haluat käyttää SSMS, suorita Transact-SQL-komentoja havaita toisen työkalulla virheiden korjaaminen. Tämä menetelmä on pääasiassa kokeneille käyttäjille, voit muokata suoraan lähdetietokannan tietokannan rakennetta. 
- Käytä [SQL Azure siirron ohjattu ("SAMW")](sql-database-cloud-migrate-fix-compatibility-issues.md): käyttämään SAMW Transact-SQL-komentosarja luo lähde-tietokannasta. Ohjattu toiminto muuntaa komentosarjan aina, kun se on mahdollista, jotta rakenteen Vipuventtiili SQL-tietokannan V12-yhteensopiva. Kun olet valmis, SAMW muodostaa yhteyden SQL tietokanta Vipuventtiili V12 komentosarjan suorittaminen. Tämä työkalu analysoi myös jäljitystiedostoja, voit selvittää yhteensopivuusongelmia. Komentosarja voi luoda vain rakenteen käyttäminen tai lisätä tiedot BCP-muodossa.

## <a name="migrate-a-compatible-sql-server-database-to-sql-database"></a>Yhteensopivat SQL Server-tietokannan siirtäminen SQL-tietokantaan

Siirtää yhteensopiva SQL Server-tietokantaan Microsoft tarjoaa useita eri skenaarioissa siirron menetelmiä. Tapa, voit valita riippuu oman poikkeama käyttökatkot, koosta ja monimutkaisuudesta SQL Server-tietokantaan ja yhteys Microsoft Azure pilveen varten.  

> [AZURE.SELECTOR]
- [Ohjattu SSMS siirto](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
- [BACPAC-tiedoston vieminen](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [Tuo BACPAC tiedostosta](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Tapahtumien sallittuja](sql-database-cloud-migrate-compatible-using-transactional-replication.md)

Siirtomenetelmä täyttämistavan ensimmäisen kysymyksen pyytää on onko voit varaa tietokantaa ulos tuotannon siirron aikana. Siirtyminen tietokannan vaikka aktiivisten tapahtumien määrä voi johtaa tietokannan epäyhtenäisyydet ja tietokannan vioittumisen. Tietokannan, poistamasta asiakasyhteydet [tietokannan tilannevedoksen](https://msdn.microsoft.com/library/ms175876.aspx)luominen on monia tapoja pysäytystilaan.

Siirtää mahdollisimman vähän käyttökatkot ja käytä [SQL Serverin tapahtuman replikoinnin](sql-database-cloud-migrate-compatible-using-transactional-replication.md) , jos tietokannan vastaa tapahtumien replikoinnin. Jos jotkin käyttökatkot varaa tai olet suorittamassa opitun tuotannon tietokannan myöhemmin siirron, toimi jollakin seuraavista tavoista:

- [Ohjattu SSMS siirto](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md): pienten Normaali tietokantoihin yhteensopiva SQL Server 2005: n tai uudemman version tietokannan siirtäminen onnistuu helposti vain käynnissä [Microsoft Azure-tietokannan käyttöönotto tietokannan](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md) SQL Server Management Studiossa.
- [Vie BACPAC tiedostoon](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md) ja valitse sitten [Tuo BACPAC tiedostosta](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md): Jos olet connectivity haasteita (ei connectivity, kaistanleveys on pieni tai aikakatkaisu ongelmat) ja Normaali, suuri tietokantoihin, käytä [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) tiedoston. Käyttämällä tätä menetelmää voit viedä BACPAC tiedoston SQL Server-rakenteen ja tiedot. BACPAC-tiedoston tuominen sitten SQL-tietokantaan ohjattua Vie tiedot taso sovelluksen SQL Server Management Studiossa tai [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) komentorivi-apuohjelmaa.
- BACPAC ja BCP käyttäminen yhdessä: käyttäminen [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) tiedostosta ja [BCP](https://msdn.microsoft.com/library/ms162802.aspx) paljon suurempi tietokantojen saavuttamiseksi suurempi parallelization kasvaa suorituskyvyn tosin suurempi monimutkaisuutta. Käyttämällä tätä menetelmää siirtää rakenne ja tiedot erikseen.
 - [Vie rakenne vain BACPAC-tiedostoon](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md).
 - [Tuo vain BACPAC tiedostosta rakenteen](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md) SQL-tietokantaan.
 - [BCP](https://msdn.microsoft.com/library/ms162802.aspx) avulla voit purkaa tiedot tasainen tiedostoja ja [rinnakkaisia Lataa](https://technet.microsoft.com/library/dd425070.aspx) tiedostot Azure SQL-tietokantaan.

     ![SQL Serverin tietokantaan siirto - siirtää pilveen SQL-tietokantaan.](./media/sql-database-cloud-migrate/01SSMSDiagram_new.png)

## <a name="next-steps"></a>Seuraavat vaiheet

- [Microsoft SQL Server 2016 päivityksen tukipalvelu esikatselu](http://www.microsoft.com/download/details.aspx?id=48119)
- [SSDT uusimmassa versiossa](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQL Server Management Studiossa uusimmassa versiossa](https://msdn.microsoft.com/library/mt238290.aspx)

##<a name="additional-resources"></a>Lisäresursseja

- [SQL-tietokannan Vipuventtiili V12](sql-database-v12-whats-new.md)
[Transact-SQL osittain tai funktioita, joita ei tueta](sql-database-transact-sql-information.md)
- [Siirtää SQL Server-tietokantoja SQL Server siirron hallinnan käyttäminen](http://blogs.msdn.com/b/ssma/)
