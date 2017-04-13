<properties
    pageTitle="SQL-tietokanta-palvelun kaikista aiheista | Microsoft Azure"
    description="Taulukon kaikista aiheista Azure-palvelun nimi SQL-tietokantaan, jotka ovat http://azure.microsoft.com/documentation/articles/ ja otsikko ja kuvaus."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor="MightyPen"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="genemi"/>


# <a name="all-topics-for-azure-sql-database-service"></a>Kaikki aiheet Azure SQL-tietokanta-palveluun

Tässä artikkelissa on lueteltu jokaisen artikkelista, johon koskee Azure **SQL-tietokanta** -palvelun. Voit hakea avainsanojen tämä sivu nykyiseen halutut aiheet **Ctrl + F**, avulla.




## <a name="new"></a>Uusi

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 1 | [Daxko/CSI käytetään Azure nopeuttaa sen kehittämisen aikana ja asiakkaan palvelujen ja suorituskyvyn parantamiseksi](sql-database-implementation-daxko.md) | Tietoja siitä, miten Daxko/CSI käyttää SQL-tietokantaan nopeuttamiseksi sen kehittämisen aikana ja asiakkaan palvelujen ja suorituskyvyn parantamiseksi |
| 2 | [Azure avulla GEP yleinen reach ja tehokkuuden lisäämiseksi](sql-database-implementation-gep.md) | Tietoja siitä, miten GEP käyttää SQL-tietokantaan tavoittaa enemmän yleisiä asiakkaat ja saavuttamiseksi tehokkuuden lisäämiseksi |
| 3 | [Azure, jossa SnelStart on nopeasti laajennettu business-palveluja on 1 000 uusi Azure SQL-tietokantoja kuukaudessa](sql-database-implementation-snelstart.md) | Tietoja siitä, miten SnelStart käyttää laajennettavat nopeasti business-palveluja on 1 000 uusi Azure SQL-tietokantoja kuukaudessa SQL-tietokantaan |
| 4 | [Umbraco käyttää nopeasti säännöstä ja skaalausta Servicesiin pilveen vuokraajiin tuhansia Azure SQL-tietokanta](sql-database-implementation-umbraco.md) | Tietoja siitä, miten Umbraco käyttää nopeasti valmistelu SQL-tietokantaan ja skaalaus-palveluiden tuhansia vuokraajiin pilveen |
| 5 | [Hallinta PowerShellin Azure SQL-tietokanta](sql-database-manage-powershell.md) | Azure SQL-tietokannan hallinta PowerShellin avulla. |
| 6 | [Azure AD-MFA SQL-tietokantaan ja SQL-tietovarasto SSMS tuki](sql-database-ssms-mfa-authentication.md) | Usean Monimenetelmäinen todentaminen käyttäminen SSMS SQL-tietokantaan ja SQL-tietovarasto. |
| 7 | [Kerrotaan tietokannan tapahtuman yksiköt (DTUs) ja joustavasti tietokannan tapahtuman yksiköt (eDTUs)](sql-database-what-is-a-dtu.md) | Mitä Azure SQL-tietokannan tietoja tapahtuman yksikkö on. |


## <a name="updated-articles-sql-database"></a>Päivitetty artikkeleita on SQL-tietokantaan

Tässä osassa on luettelo artikkeleissa, joka on päivitetty viimeksi, jossa päivitys on suuri tai merkittäviä. Kunkin päivitetyn artikkelin karkea katkelma korotuksia lisätty teksti tulee näkyviin. Artikkelit on päivitetty **2016-10-05** **2016-08-22** päivämäärävälillä.

| &nbsp; | Artikkelissa | Päivitetty teksti, koodikatkelman | Päivitetty, kun |
| --: | :-- | :-- | :-- |
| 8 | [Hallinta PowerShellin Azure SQL-tietokanta](sql-database-command-line-tools.md) | Luoda resurssiryhmä sekä SQL-tietokantaan ja liittyvät Azure resurssit ja sitten uusi-AzureRmResourceGroup (https://msdn.microsoft.com/library/azure/mt759837.aspx) cmdlet-komennon. *$resourceGroupName = "resourcegroup1" $resourceGroupLocation = "northcentralus" uusi AzureRmResourceGroup-$resourceGroupName nimi-kohtaan $resourceGroupLocation* Lisätietoja on artikkelissa Azure PowerShellin Azure Resource Manager (... / powershell-azure-resurssi-manager.md). Katso esimerkki-komentosarja luo SQL-tietokanta PowerShell-komentosarjaa (sql-tietokanta-get-aloittaminen-powershell.md create-a-sql-database-powershell-script). **Luo SQL-tietokantapalvelinta** Luo uusi-AzureRmSqlServer (https://msdn.microsoft.com/library/azure/mt603715.aspx) cmdlet-komento SQL-tietokantapalvelimeen. Korvaa *palvelin1* palvelimesi nimi. Palvelimen nimi on oltava yksilöllinen palvelimilta Azure SQL-tietokantaan. Jos palvelimen nimi on jo, järjestelmä ilmoittaa virheestä. Tämä komento voi kestää useita minuutteja. Resou | 2016-09 – 14 |
| 9 | [Kopioi PowerShellin Azure SQL-tietokantaan](sql-database-copy-powershell.md) |   SQL tietokannan lähde (nykyisen tietokannan kopioi) – $sourceDbName = "MJP1" $sourceDbServerName = "palvelin1" $sourceDbResourceGroupName = "rg1" SQL-tietokannan kopiota (uusi tietokanta luodaan) – $copyDbName = "db1_copy" $copyDbServerName = "palvelin2" $copyDbResourceGroupName = "rg2" kopio palvelimeen--uusi AzureRmSqlDatabaseCopy - ResourceGroupName $sourceDbResourceGroupName tietokannan - palvelin $sourceDbServerName - nimi $sourceDbName - CopyDatabaseName $copyDbName tietokannan kopioiminen toiseen palvelimeen--uusi AzureRmSqlDatabaseCopy - ResourceGroupName $sourceDbResourceGroupName - palvelin $sourceDbServerName - nimi $sourceDbName - CopyResourceGroupName $copyDbResourceGroupName - CopyServerName $copyDbServerName - CopyDatabaseName $copyDbName kopioi tietokannan joustavasti tietokannan resurssivarantoon--$poolName = "pool1" uusi AzureRmSqlDatabaseCopy - ResourceGroupName $ sourceDbResourceGroupName - palvelin $sourceDbServerName - nimi $sourceDbName - CopyReso | 2016 09-08 |
| 10 | [Luo joustavasti tietokannan resurssivarantoon C#](sql-database-elastic-pool-create-csharp.md) |   Console.WriteLine ("palomuurin...");  FirewallRuleGetResponse fwr = CreateOrUpdateFirewallRule (_sqlMgmtClient, _resourceGroupName, _serverName, _firewallRuleName, _startIpAddress, _endIpAddress);  Console.WriteLine ("palomuurin:" + fwr. FirewallRule.Id);  Console.WriteLine ("joustavasti resurssivarantoon...");  ElasticPoolCreateOrUpdateResponse epr = CreateOrUpdateElasticDatabasePool (_sqlMgmtClient _resourceGroupName _serverName, _poolName, _poolEdition, _poolDtus, _databaseMinDtus, _databaseMaxDtus);  Console.WriteLine ("joustavasti resurssivarantoon:" + epr. ElasticPool.Id);  Console.WriteLine("Database...");  DatabaseCreateOrUpdateResponse dbr = CreateOrUpdateDatabase (_sqlMgmtClient, _resourceGroupName, _serverName, _databaseName, _poolName);  Console.WriteLine ("tietokannan:" + dbr. Database.Id);  Console.WriteLine ("paina mitä tahansa näppäintä Jatka...");  Console.ReadKey();  } staattinen ResourceGroup CreateOrUpdateResourceGroup(ResourceManagementClient resourceMgmtClient, string subscriptionId, string resourceGroupNa | 2016-09 – 14 |
| 11 | [PowerShell-komentosarjaa yksilöimiseen tietokantojen sopii joustavasti tietokannan resurssivarantoon](sql-database-elastic-pool-database-assessment-powershell.md) | **Varten joustavasti tietokannan resurssivarantoon ehdokkaiden on jättää pää- ja tietokannoissa, jotka ovat jo resurssivarantoon. Voit lisätä pois jätettyjen luettelosta Lisää tietokantojen tarvittaessa** $ListOfDBs = Get-AzureRmSqlDatabase $servername - palvelimen nimi. Split('.') 0 - ResourceGroupName $ResourceGroupName / Where-objektin {$_. Nimi - notin ("pääluetteloon") - ja $_. CurrentServiceLevelObjectiveName - notin ("ElasticPool")- ja $_. CurrentServiceObjectiveName - notin ("ElasticPool")} $outputConnectionString = "tietolähteen = $outputServerName; integroitu suojaus = false; alkuperäisen luettelon = $outputdatabaseName; Käyttäjätunnus = $outputDBUsername; Salasana = $outputDBpassword "$destinationTableName ="resource_stats_output" **taulukon arvot sivustokokoelman tulostus-tietokannan luominen** $sql =" Jos NOT EXISTS (Valitse * sys.objects-WHERE object_id = OBJECT_ID(N'$($destinationTableName)') ja kirjoita (N'U ")) Aloita Luo taulukko $($destinationTableName) (Tietokannan_nimi varchar(128), hitaissa varchar(20), end_time datetime, avg_cpu liukuluku, keskiarvo_ | 2016-09 – 29 |
| 12 | [SQL-tietokanta-opetusohjelma: SQL-tietokannan luominen minuuttia Azure-portaalissa](sql-database-get-started.md) |   ! Uuden sql-tietokannan 1 (. / media/sql-database-get-started/sql-database-new-database-1.png) 3. Valitse **SQL-tietokannan** SQL-tietokanta-sivu avautuu. Tämä sivu sisällön vaihtelee sen mukaan montako tilauksistasi ja olemassa olevat objektit (kuten aiemmin palvelimet).  ! Uuden sql-tietokannan 2 (. / media/sql-database-get-started/sql-database-new-database-2.png) 4. **Tietokannan nimi** -tekstiruutuun anna sille nimi ensimmäisen tietokannan – esimerkiksi "Omat tietokannan". Vihreä valintamerkki osoittaa, että olet määrittänyt kelvollinen nimi.  ! Uuden sql-tietokannan 3 (. / media/sql-database-get-started/sql-database-new-database-3.png) 5. Jos sinulla on useita tilauksia, valitse tilaus. 6. **Resurssiryhmä**valitsemalla **Luo uusi** ja anna ensimmäinen resurssiryhmän – esimerkiksi "Omat--resurssiryhmä". Vihreä valintamerkki osoittaa, että olet määrittänyt kelvollinen nimi.  ! Uuden sql-tietokannan 4 (. / media/sql-database-get-started/sql-database-new-database-4.png) 7. Valitse **lähde* | 2016 09-08 |
| 13 | [Kokeile SQL-tietokantaan: Käytä C# SQL-tietokannan luominen SQL-tietokanta-kirjastoa varten .NET](sql-database-get-started-csharp.md) | **Luo pääasiallista resursseihin palvelu** Seuraavaa PowerShell-komentosarjaa luo Active Directory (AD)-sovelluksen ja palvelun lyhennys, joka todentaa Microsoftin C-sovelluksen annettava. Komentosarja tulostaa edellisen C otoksen annettava arvot. Lisätietoja on kohdassa Azure PowerShellin luominen palvelun lyhennys, voit käyttää resursseja (... / resurssi-ryhmä-todennusta-palvelu-principal.md).  Azure kirjautuminen.  Lisää AzureRmAccount Jos sinulla on useita tilauksia, kommentointi ja määritä tilaukseen, jota haluat käyttää.  $subscriptionId = "{lt-xxxx-xxxx-xxxx-xxxxxxxxxxxx}" määrittäminen AzureRmContext - SubscriptionId $subscriptionId antaa nämä arvot AAD uusi sovellus.  $appName on sovelluksen näyttönimi, on oltava yksilölliset hakemistossa.  $uri ei tarvitse olla todellisia uri.  $secret on luot salasana.  $appName = "{sovelluksen-nimi}" $uri = "http://{app-name}" $secret = "{sovelluksen-salasanan}" luominen AAD-sovelluksen $azureAdApplication = uusi AzureRmADApp | 2016-09-23 |
| 14 | [Azure SQL-tietokantoja Azure-portaalissa hallinta](sql-database-manage-portal.md) | Voit tarkastella tai päivittää tietokanta-asetukset, valitse SQL-tietokanta-sivu, valitse haluamasi asetus:! SQL-tietokanta-asetukset (. / media/sql-database-manage-portal/settings.png) **Mistä löydän SQL tietokantojen palvelimen täydellinen nimi?** Voit tarkastella tietokantojen-palvelimen nimen selvittäminen-napsauttamalla **Yleiskatsaus** **SQL-tietokanta** -sivu ja palvelimen nimi:! SQL-tietokanta-asetukset (. / media/sql-database-manage-portal/server-name.png) **palomuurisäännöt, joiden avulla voit hallita SQL server- ja tietokannan hallintaa?** Voit tarkastella, luoda tai päivittää palomuurisäännöt, valitse **Määritä palomuurin** **SQL-tietokanta** -sivu. Lisätietoja on artikkelissa Azure-portaalissa (sql-tietokanta-asetusten määrittäminen-palomuuri-settings.md) Azure SQL-tietokanta on palvelintason palomuurisääntö määrittäminen. ! palomuurin säännöt (. / media/sql-database-manage-portal/sql-database-firewall.png) **Miten voin muuttaa oma SQL tietokanta service taso tai suorituskyvyn tason?** Jos haluat päivittää palvelun taso tai suorituskyvyn taso SQL-tietokantaan, valitse ** hinnoittelu taso (s | 2016-09-20 |





## <a name="get-started"></a>Käytön aloittaminen

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 15 | [Muodostaa usean vuokraajan sovellukset käyttämällä Azure SQL-tietokantaa, jossa eristystaso ja tehokkuutta](sql-database-build-multi-tenant-apps.md) | Lue, miten SQL-tietokanta muodostaa usean vuokraajan sovellukset |
| 16 | [Yhteyden muodostaminen SQL-tietokantaa, jossa SQL Server Management Studiossa ja otoksen T-SQL-kyselyn suorittaminen](sql-database-connect-query-ssms.md) | Opettele muodostamaan Azure SQL-tietokantaan SQL Server Management Studiossa (SSMS) avulla. Suorita sitten otoksen kyselyn käyttämällä Transact-SQL (T-SQL). |
| 17 | [Yhteyden muodostaminen SQL-tietokantaan käyttämällä .NET (C#)](sql-database-develop-dotnet-simple.md) | Käytä oleva Tämä nopea Käynnistä muodostaa uusi sovellus, jolla C# ja palautettua mukaan tehokkaita relaatiotietokannasta pilveen Azure SQL-tietokantaan. |
| 18 | [Luo uusi ryhmä joustavasti tietokannan Azure-portaalissa](sql-database-elastic-pool-create-portal.md) | Voit lisätä skaalattava joustavasti tietokannan resurssivarantoon SQL tietokanta-määritys helpompaa hallinta ja resurssien jakaminen useiden tietokantojen välillä. |
| 19 | [Tutustu Azure SQL-tietokanta-opetusohjelmat](sql-database-explore-tutorials.md) | Lisätietoja SQL-tietokanta-ominaisuuksia ja toimintoja |
| 20 | [SQL-tietokanta-opetusohjelma: SQL-tietokannan luominen minuuttia Azure-portaalissa](sql-database-get-started.md) | Opettele looginen SQL-tietokantapalvelin, johon, palvelimen palomuurisäännön, SQL-tietokantaan ja tietojen määrittäminen. Lisäksi opit asiakastyökalut yhteydessä, määrittää käyttäjiä ja tietokantaan palomuurin siirtosäännön määrittäminen. |
| 21 | [Azure SQL-tietokanta suojaa ja suojaa](sql-database-helps-secures-and-protects.md) | Katso, kuinka SQL-tietokantaan auttaa suojatun ja suojaaminen |
| 22 | [Azure SQL-tietokanta oppii &amp; sopeutuu](sql-database-learn-and-adapt.md) | Lisätietoja SQL-tietokantaan oppii ja sopeutuu |
| 23 | [Valitse pilvestä SQL Server-asetus: SQL Azure (PaaS)-tietokanta tai SQL Server Azure VMs (IaaS)](sql-database-paas-vs-sql-server-iaas.md) | Katso, mitä cloud SQL Server-asetus sopii sovelluksesi: SQL Azure (PaaS)-tietokanta tai pilveen-Azuren näennäiskoneiden SQL Server. |
| 24 | [Azure SQL-tietokannan valitseminen suoraan selaimessa](sql-database-scale-on-the-fly.md) | Lue, miten SQL-tietokantaan Skaalaa suoraan selaimessa |
| 25 | [Tutustu Azure SQL-tietokannan ratkaisu pika käynnistyy](sql-database-solution-quick-starts.md) | Lisätietoja Azure SQL-tietokannan ratkaisut |
| 26 | [Azure SQL-tietokanta toimii ympäristössäsi](sql-database-works-in-your-environment.md) | Lue, miten SQL-tietokannan avulla, suojaa ja suojaa |



## <a name="build-an-app"></a>Sovelluksen luominen

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 27 | [Vianmääritys, diagnosointi ja estää SQL yhteyden virheet ja lyhytkestoisia virheet SQL-tietokantaan](sql-database-connectivity-issues.md) | Opettele liittyviä ongelmia, diagnosointi ja estää SQL-yhteysvirhe tai lyhytkestoisia virhe Azure SQL-tietokantaan.  |
| 28 | [Yhteyden muodostaminen Visual Studiossa SQL-tietokantaan](sql-database-connect-query.md) | Kirjoita ohjelman C# kyselyyn ja yhteyden muodostaminen SQL-tietokantaan. Voit lukea lisää IP-osoitteet, yhteysmerkkijonon, suojattu kirjautuminen ja vapaa Visual Studio. |
| 29 | [Lisäksi 1433 ADO.NET 4.5 ja SQL-tietokannan Vipuventtiili V12 portit](sql-database-develop-direct-route-ports-adonet-v12.md) | Joskus asiakasyhteydet ADO.NET Azure SQL-tietokannan Vipuventtiili V12 ohittaa välityspalvelimen ja käsitellä tietokannan. Portit kuin 1433 ole tärkeä. |
| 30 | [SQL-virhekoodit SQL-tietokantaan asiakassovellukset: tietokannan yhteysvirhe ja muut ongelmat](sql-database-develop-error-messages.md) | Lisätietoja SQL-virhekoodit SQL-tietokanta-asiakassovellusten, kuten yleisiä tietokannan yhteys virheitä, tietokannan kopion ongelmat ja yleisten virheiden varten.  |
| 31 | [Yhteyden muodostaminen SQL-tietokantaan PHP avulla Windowsiin](sql-database-develop-php-simple.md) | Esittää otoksen PHP ohjelmaan, joka yhdistää asiakaskoneesta Windows Azure SQL-tietokanta ja linkkejä asiakkaan tarvitsemia tarvittavat ohjelmisto-osiin. |
| 32 | [Kokeile SQL-tietokantaan: Käytä C# SQL-tietokannan luominen SQL-tietokanta-kirjastoa varten .NET](sql-database-get-started-csharp.md) | Kokeile SQL-tietokannan SQL- ja C#-sovellusten kehittämiseen ja luo Azure SQL-tietokantaan ja C#-SQL-tietokanta-kirjaston käytön .NET. |
| 33 | [Käytön aloittamista käsittelevät JSON Azure SQL-tietokantaan](sql-database-json-features.md) | Azure SQL-tietokantaan voit jäsennä, kyselyjen ja tietojen muotoileminen JavaScript Object Notation (JSON)-merkintätapaa. |
| 34 | [Yhteyden muodostaminen Azure SQL-tietokantaan ongelmien vianmääritys](sql-database-troubleshoot-common-connection-issues.md) | Ohjeet ja yhteyden yleisten virheiden ratkaiseminen Azure SQL-tietokantaan. |
| 35 | [Virhe "Palvelimen tietokanta ei ole tällä hetkellä käytettävissä" yhdistettäessä sql-tietokantaan](sql-database-troubleshoot-connection.md) | Vianmääritys tietokanta palvelin ei ole tällä hetkellä käytettävissä-virhe, kun sovellus muodostaa yhteyden muodostaminen SQL-tietokantaan. |



## <a name="develop"></a>Kehittäminen

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 36 | [Hae tarvittavat arvot todennustapa sovelluksen voit käyttää SQL-tietokanta-koodista](sql-database-client-id-keys.md) | Luo palvelun lyhennys SQL-tietokannan avaaminen koodi. |
| 37 | [Yhteyden muodostaminen SQL-tietokanta käyttämällä Java JDBC Windows](sql-database-develop-java-simple.md) | Esittää Java koodin otoksen avulla voit muodostaa yhteyden Azure SQL-tietokantaan. Esimerkissä JDBC, ja se suoritetaan Windows asiakastietokoneessa. |
| 38 | [Yhteyden muodostaminen SQL-tietokantaan Node.js avulla](sql-database-develop-nodejs-simple.md) | Esittää Node.js koodin otoksen avulla voit muodostaa yhteyden Azure SQL-tietokantaan. |
| 39 | [SQL-tietokannan kehittäminen yleiskatsaus](sql-database-develop-overview.md) | Lisätietoja käytettävissä connectivity kirjastoja ja sovelluksia yhteyden muodostaminen SQL-tietokantaan parhaat käytännöt. |
| 40 | [Yhteyden muodostaminen SQL-tietokantaan Python avulla](sql-database-develop-python-simple.md) | Esittää Python koodin otoksen avulla voit muodostaa yhteyden Azure SQL-tietokantaan. |
| 41 | [Yhteyden muodostaminen SQL-tietokantaan Ruby avulla](sql-database-develop-ruby-simple.md) | Anna Ruby koodin otoksen voit suorittaa muodostaa Azure SQL-tietokantaan. |
| 42 | [Käytön aloittaminen ajallinen taulukoiden Azure SQL-tietokantaan](sql-database-temporal-tables.md) | Opettele ajallinen taulukoiden käyttäminen Azure SQL-tietokannan käytön aloittaminen. |



## <a name="performance-and-scale"></a>Suorituskyky ja skaalattavuus

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 43 | [SQL-tietokannan tukipalvelu](sql-database-advisor.md) | Azure SQL-tietokanta-Advisor ja neuvoo, joka voi parantaa nykyisen kyselyn suorituskykyä aiemmin SQL-tietokannat. |
| 44 | [SQL-tietokannan Advisor Azure-portaalissa](sql-database-advisor-portal.md) | Voit Azure portaalin Azure SQL-tietokanta-Advisor tarkistaa ja toteuttaa suosituksia, joka voi parantaa nykyisen kyselyn suorituskykyä aiemmin SQL-tietokannat. |
| 45 | [Azure SQL-tietokantaan ensisijainen yleiskatsaus](sql-database-benchmark-overview.md) | Tässä ohjeaiheessa kerrotaan Azure SQL-tietokannan ensisijainen käytetään määritettäessä Azure SQL-tietokannan suorituskykyä. |
| 46 | [Kun joustavasti tietokannan resurssivarantoon voidaan käyttää?](sql-database-elastic-pool-guidance.md) | Joustavasti tietokannan varanto on käytettävissä olevia resursseja, jotka on jaettu ryhmittäin joustavasti tietokantojen kokoelma. Tässä asiakirjassa on ohjeita Arvioi sopivuus joustavasti tietokannan resurssivarannon käyttäminen tietokantojen ryhmä. |
| 47 | [Joustavasti Tietokantatyökalut käytön aloittaminen](sql-database-elastic-scale-get-started.md) | SELITYS Basic joustavasti tietokannan työkalut-toiminnon, Azure SQL-tietokanta, mukaan lukien helppo otoksen sovelluksen suorittamiseen. |
| 48 | [SQL-tietokantaan usein kysytyt kysymykset](sql-database-faq.md) | Vastauksia yleisiin kysymyksiin asiakkaille Kysy cloud tietokannat ja Azure SQL-tietokanta, Microsoftin relaatiotietokannasta hallintajärjestelmän (RDBMS) ja tietokannan palveluna pilveen. |
| 49 | [Ladatun (ennakkoversio) SQL-tietokannan käytön aloittaminen](sql-database-in-memory.md) | SQL muistissa technologies parantaa suorituskykyä tapahtumien ja analytics toiminnoista. Lue, miten voit hyödyntää seuraavia tekniikoita. |
| 50 | [Voit parantaa sovelluksen suorituskyvyn SQL-tietokannan käyttäminen ladatun OLTP (ennakkoversio)](sql-database-in-memory-oltp-migration.md) | Ottaa käyttöön ladatun OLTP tapahtumien suorituskyvyssä SQL-tietokannassa. |
| 51 | [Näytön OLTP ladatun tallennustilan](sql-database-in-memory-oltp-monitoring.md) | Arviota ja näytön XTP ladatun tallennustilan käyttää, kapasiteetin; Ratkaise virhe kapasiteetin 41823 |
| 52 | [Toimimasta Azure SQL-tietokantaan kysely-säilö](sql-database-operate-query-store.md) | Lue, miten toimia kyselyn säilön Azure SQL-tietokantaan |
| 53 | [SQL-tietokannan suorituskykyä tietoja](sql-database-performance.md) | Azure SQL-tietokanta on suorituskyky-työkaluja, joiden avulla voit tunnistaa aluetta, joka voi parantaa nykyisen kyselyn suorituskykyä. |
| 54 | [Azure SQL-tietokantaan ja yksittäisen tietokantojen suorituskyky](sql-database-performance-guidance.md) | Tämän artikkelin avulla voit selvittää, mitä service-tason valitseminen sovelluksen. Se myös suosittelee tapoja paranna sovelluksen käyttämistä eniten Azure SQL-tietokantaan. |
| 55 | [Azure SQL-tietokannan kyselyn suorituskykyä tietoja](sql-database-query-performance.md) | Useimmat Keskusyksikön muissa kyselyt kyselyn suorituskyvyn seurantaa tunnistaa Azure SQL-tietokannan. |
| 56 | [Palvelun taso ja suorituskyvyn tason muuttaminen (hinnoittelu taso) SQL-tietokanta](sql-database-scale-up.md) | Muuta palvelutaso ja suorituskyvyn tasoon Azure SQL-tietokantaan esitetään, kuinka voit skaalata SQL-tietokantaan, ylös tai alas. Muuttaminen Azure SQL-tietokantaan hinnoittelu taso. |
| 57 | [Tietokannan suorituskyvyn Azure SQL-tietokantaan valvominen](sql-database-single-database-monitor.md) | Tutustu erilaisiin Azure työkalut ja dynaaminen hallinta näkymien tietokannan seurantaa varten. |
| 58 | [SQL-tietokannan suorituskyvyn säätäminen vihjeitä](sql-database-troubleshoot-performance.md) | Vinkkejä suorituskyvyn säätö Azure SQL-tietokantaan arviointi ja Käyttömukavuuden. |
| 59 | [Voit käyttää SQL-tietokanta-sovelluksen suorituskyvyssä jonottaminen](sql-database-use-batching-to-improve-performance.md) | Aiheessa näyttöä, batching tietokannan toimintojen huomattavasti imroves nopeuden ja skaalattavuus Azure SQL-tietokanta-sovelluksista. Vaikka batching näistä menetelmistä sovellu minkä tahansa SQL Server-tietokantaan, artikkelissa keskitytään Azure. |



## <a name="tools-for-scaling-out"></a>Työkaluja laajentaminen

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 60 | [Suunnittele kuviot multitenant SaaS sovellusten ja Azure SQL-tietokanta](sql-database-design-patterns-multi-tenancy-saas-applications.md) | Tässä artikkelissa käsitellään vaatimukset ja yleisiä tietoja arkkitehtuuri kuviot sovellusten cloud-ympäristössä kannattaa ottaa huomioon multitenant tietokannan ja näiden kuvioiden liittyvät eri kompromissien. Se myös käsitellään miten Azure SQL-tietokanta, joustavasti jakavat ja joustavasti Työkalut, osoite ei haavoittuvan tavalla näitä vaatimuksia. |
| 61 | [Siirtää aiemmin luotujen tietokantojen asteikko itsestään](sql-database-elastic-convert-to-use-elastic-tools.md) | Muuntaa sharded tietokannat joustavasti Tietokantatyökalut luomalla shard kartan hallinta |
| 62 | [Skaalattavissa cloud tietokantojen luominen](sql-database-elastic-database-client-library.md) | Skaalattavissa .NET tietokannan sovellusten ja joustavasti tietokannan asiakas-kirjaston luominen |
| 63 | [Suorituskyvyn laskureita shard kartan hallinta](sql-database-elastic-database-perf-counters.md) | ShardMapManager luokan ja tietojen riippuvaiset reititys suorituskyvyn laskureita |
| 64 | [Shard, käyttämällä joustavasti Tietokantatyökalut lisääminen](sql-database-elastic-scale-add-a-shard.md) | Määritä joustavasti asteikko-ohjelmointirajapinnan käyttäminen lisäämään uusia shards shard. |
| 65 | [Jaa ja yhdistäminen-palvelun käyttöön](sql-database-elastic-scale-configure-deploy-split-and-merge.md) | Jakaminen ja joustavasti Tietokantatyökalut yhdistäminen |
| 66 | [Tietoja riippuvaiset reititys](sql-database-elastic-scale-data-dependent-routing.md) | Miten voit käyttää ShardMapManager luokan .NET-sovelluksissa tietojen riippuva reititys-ominaisuus, joustavasti tietokantojen Azure SQL-tietokanta |
| 67 | [Joustavasti Tietokantatyökalut usein kysytyt kysymykset](sql-database-elastic-scale-faq.md) | Usein kysyttyjä kysymyksiä Azure SQL-tietokannan joustavasti asteikko. |
| 68 | [Joustavasti tietokannan Työkalut-sanasto](sql-database-elastic-scale-glossary.md) | SELITYS termit joustavasti Tietokantatyökalut varten |
| 69 | [Azure SQL-tietokanta ja laajentaminen](sql-database-elastic-scale-introduction.md) | Service (SaaS) kehittäjät ohjelmiston voit helposti luoda joustavasti, skaalattava tietokantojen näillä työkaluilla pilveen |
| 70 | [Käyttää joustavasti tietokannan asiakkaan kirjaston tunnistetiedot](sql-database-elastic-scale-manage-credentials.md) | Voit määrittää oikean suojaustason tunnistetietoja, järjestelmänvalvojan joustavasti tietokanta-sovellusten, vain luku-tilaan |
| 71 | [Usean shard kyselyt](sql-database-elastic-scale-multishard-querying.md) | Kyselyjen suorittaminen joustavasti tietokannan asiakas-kirjaston käytön shards yli. |
| 72 | [Tietojen siirtäminen skaalata ulos cloud tietokantojen välillä](sql-database-elastic-scale-overview-split-and-merge.md) | Kerrotaan, miten voit käsitellä shards ja siirtää tiedot itse isännöityä palvelu joustavasti tietokannan ohjelmointirajapinnan kautta. |
| 73 | [Skaalaa ulos tietokantojen shard kartan hallinnan kanssa](sql-database-elastic-scale-shard-map-management.md) | Opi käyttämään ShardMapManager joustavasti tietokannan asiakkaan kirjastoon |
| 74 | [Jaa ja yhdistäminen suojausasetukset](sql-database-elastic-scale-split-merge-security-configuration.md) | Määritä x409 salauksen varmenteet |
| 75 | [Uusimman joustavasti tietokannan asiakas-kirjasto-sovelluksen päivittäminen](sql-database-elastic-scale-upgrade-client-library.md) | Sovellusten ja -kirjastojen avulla Nuget päivittäminen |
| 76 | [Kohteen Framework joustavasti tietokannan asiakkaan kirjastosta](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) | Käytä joustavasti tietokannan asiakkaan kirjasto ja kohteen Framework coding tietokannat |
| 77 | [Joustavasti tietokannan asiakkaan kirjaston käyttäminen Dapper](sql-database-elastic-scale-working-with-dapper.md) | Joustavasti tietokannan asiakkaan kirjaston käyttäminen Dapper. |
| 78 | [Usean vuokraajan sovellusten joustavasti Tietokantatyökalut ja rivin käyttäjätason suojauksen](sql-database-elastic-tools-multi-tenant-row-level-security.md) | Opettele käyttämään joustavasti Tietokantatyökalut yhdessä rivin käyttäjätason suojauksen luonnissa Azure SQL-tietokantaan, joka tukee useita vuokraajan shards sovellus on erittäin skaalattava tietojen taso. |



## <a name="elastic-jobs"></a>Joustavasti töitä

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 79 | [Voit luoda ja hallita skaalatun ulos Azure SQL-tietokantoja joustavasti töitä (ennakkoversio)](sql-database-elastic-jobs-create-and-manage.md) | Käy läpi luomisen ja hallinnan joustavasti tietokannan työn. |
| 80 | [Töiden joustavasti tietokannan käytön aloittaminen](sql-database-elastic-jobs-getting-started.md) | Opi käyttämään joustavasti tietokannan projektit |
| 81 | [Skaalattu ulos cloud tietokantojen hallinta](sql-database-elastic-jobs-overview.md) | On kuvattu joustavasti tietokannan työ-palvelu |
| 82 | [Voit luoda ja hallita SQL-tietokantaan joustavasti tietokannan projektit-PowerShellin (ennakkoversio)](sql-database-elastic-jobs-powershell.md) | PowerShellin Azure SQL-tietokanta jakavat hallinta |
| 83 | [Asentaminen joustavasti tietokannan töiden yleiskatsaus](sql-database-elastic-jobs-service-installation.md) | Tutustu asennuksen joustavasti työ-toiminnon avulla. |
| 84 | [Poista joustavasti tietokannan työt osat](sql-database-elastic-jobs-uninstall.md) | Töiden joustavasti tietokantatyökalu asennuksen poistaminen |



## <a name="elastic-pools"></a>Joustavasti jakavat

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 85 | [Mikä on Azure joustavasti resurssivarantoon?](sql-database-elastic-pool.md) | Hallitse satoja tai tuhansia tietokantoja, joissa resurssivarantoon. Yhden hinnalla suorituskyky-yksiköt joukko voidaan jakaa ryhmän. Siirrä tietokantojen ulospäin -palvelussa on. |
| 86 | [Luo joustavasti tietokannan resurssivarantoon C#](sql-database-elastic-pool-create-csharp.md) | C# tietokannan kehittäminen tekniikoita avulla voit luoda skaalattava joustavasti tietokannan varannon Azure SQL-tietokantaan, jotta voit jakaa resursseja useita tietokantoja. |
| 87 | [Luo uusi ryhmä joustavasti tietokannan PowerShellin avulla](sql-database-elastic-pool-create-powershell.md) | Opettele käyttämään PowerShell asteikko-kohtaa Azure SQL-tietokanta luomalla skaalattava joustavasti tietokannan resurssivarantoon hallittavan useiden tietokantojen resurssit. |
| 88 | [PowerShell-komentosarjaa yksilöimiseen tietokantojen sopii joustavasti tietokannan resurssivarantoon](sql-database-elastic-pool-database-assessment-powershell.md) | Joustavasti tietokannan varanto on käytettävissä olevia resursseja, jotka on jaettu ryhmittäin joustavasti tietokantojen kokoelma. Tässä tiedostossa on Powershell-komentosarja auttavat arvioimaan sopivuus joustavasti tietokannan resurssivarannon käyttäminen tietokantojen ryhmä. |
| 89 | [Seurata ja hallita joustavasti tietokanta-ryhmän ja C#](sql-database-elastic-pool-manage-csharp.md) | C# tietokannan kehittäminen tekniikoita avulla voit hallita Azure SQL-tietokanta-joustavasti tietokannan resurssivarantoon. |
| 90. | [Seurata ja hallita joustavasti tietokanta-ryhmän Azure-portaalissa](sql-database-elastic-pool-manage-portal.md) | Katso, miten Azure portaalin ja SQL-tietokantaan valmiin liiketoimintatietojen avulla hallita, valvoa ja oikealle-kokoiselle skaalattava joustavasti tietokannan resurssivarantoon tietokannan suorituskyvyn parantaminen ja hallita kustannukset. |
| 91 | [Seurata ja hallita joustavasti tietokanta-ryhmän PowerShellin avulla](sql-database-elastic-pool-manage-powershell.md) | Opettele joustavasti tietokanta-ryhmän hallinta PowerShellin avulla. |
| 92 | [Seurata ja hallita joustavasti tietokanta-ryhmän kanssa Transact-SQL](sql-database-elastic-pool-manage-tsql.md) | Käytä T-SQL Azure SQL-tietokannan luominen joustavasti resurssivarantoon. Tai siirtää datbase ja siitä uloskirjautuminen jakavat T-SQL avulla. |
| 93 | [Joustavasti tietokannan resurssivarantoon laskutus- ja hintatiedot](sql-database-elastic-pool-price.md) | Tietyn joustavasti tietokannan jakavat hintatiedot. |



## <a name="elastic-query"></a>Joustavasti kysely

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 94 | [Raportin kaikissa skaalata ulos cloud-tietokannat (ennakkoversio)](sql-database-elastic-query-getting-started.md) | ristiviittauksen tietokantakyselyt tietokannan käyttämisestä |
| 95 | [Aloita rajat-tietokantakyselyt (pystysuora jakaminen) (ennakkoversio)](sql-database-elastic-query-getting-started-vertical.md) | Voit käyttää joustavasti tietokantakyselyn pystysuunnassa osioitua tietokannat |
| 96 | [Raportointi on mahdollista skaalata ulos cloud-tietokannat (ennakkoversio)](sql-database-elastic-query-horizontal-partitioning.md) | Voit määrittää joustavasti kyselyjen yli vaakasuunnassa osiot |
| 97: ssä | [Azure SQL-tietokantaan joustavasti tietokannan kyselyn yleiskatsaus (ennakkoversio)](sql-database-elastic-query-overview.md) | Yleistä joustavasti kysely-ominaisuus |
| 98 | [Kysely cloud-tietokantojen eri rakenteita (ennakkoversio)](sql-database-elastic-query-vertical-partitioning.md) | Voit määrittää rajat tietokantakyselyt pystysuora osioiden päälle |



## <a name="elastic-transaction"></a>Joustavasti tapahtuman

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 99 | [Jaetut tapahtumat yli cloud tietokannat](sql-database-elastic-transactions-overview.md) | Yleistä joustavasti tietokannan tapahtumat Azure SQL-tietokanta |



## <a name="backup-and-recovery"></a>Varmuuskopiointi ja palauttaminen

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 100 | [SQL-tietokannan varmuuskopiointi](sql-database-automated-backups.md) | Tietoja SQL-tietokanta on valmis, joiden avulla voit palauttaa tietokannan varmuuskopioita takaisin Azure SQL-tietokantaan aiempaan aika tai kopioida tietokannan uuden tietokannan tietyn maantieteellisen alueen (enintään 35 päivää). |
| 101 | [Yleistä liiketoiminnan jatkuvuus Azure SQL-tietokanta](sql-database-business-continuity.md) | Lisätietoja Azure SQL-tietokanta tukee cloud Liiketoiminnan jatkuvuus ja tietokannan palauttaminen ja auttaa säilyttää kriittisten cloud sovellukset. |
| 102 | [Voit palauttaa yhden taulukon Azure SQL-tietokannan varmuuskopiointi](sql-database-cloud-migrate-restore-single-table-azure-backup.md) | Opettele palauttamaan yhdeksi taulukoksi Azure SQL-tietokanta varmuuskopiosta. |
| 103 | [Suunnittele sovelluksen cloud palauttaminen käyttämällä Active Geo-replikoinnin SQL-tietokantaan](sql-database-designing-cloud-solutions-for-disaster-recovery.md) | Opettele suunnitella cloud tietojen palauttaminen ratkaisuja liiketoiminnan jatkuvuuden suunnittelu geo replikoinnin käyttäminen sovelluksen tietojen varmuuskopiointi Azure SQL-tietokantaan. |
| 104 | [Palauttaa Azure SQL-tietokanta tai automaattisesti toissijainen](sql-database-disaster-recovery.md) | Opi palauttamaan tietokannan alueellisen palvelinkeskuksen käyttökatkosta tai virhe Azure SQL tietokanta aktiivinen Geo-replikoinnin ja Geo palauttaminen ominaisuuksia. |
| 105 | [Tietojen palauttaminen Poraudu suorittamiseen](sql-database-disaster-recovery-drills.md) | Lue ohjeet ja parhaita käytäntöjä, jotka auttavat tietojen palauttaminen työkaluja Azure SQL-tietokannan avulla säilyttää oman toiminta kriittinen yrityssovellusten joustavat virheet ja katkokset. |
| 106 | [Sovellusten SQL-tietokannan joustavasti resurssivarantoon varalle](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md) | Lue, miten voit suunnitella cloud-ratkaisun tietojen palauttamista varten valitsemalla oikean automaattisesti kuvion. |
| 107 | [RecoveryManager-luokan avulla shard kartan ongelmien ratkaiseminen](sql-database-elastic-database-recovery-manager.md) | Ongelmien shard karttojen RecoveryManager-luokan avulla |
| 108 | [Azure SQL-tietokannan luominen BACPAC tiedoston tuominen](sql-database-import.md) | Luo Azure SQL-tietokantaan tuomalla BACPAC aiemmin luotu tiedosto. |
| 109 | [Palauttaa Azure SQL-tietokantaan aiempaan ajankohtaan Azure-portaalissa](sql-database-point-in-time-restore-portal.md) | Palauttaa Azure SQL-tietokantaan edellisessä kohdassa samanaikaisesti. |
| 110 | [Palauttaa Azure SQL-tietokantaan aiempaan ajankohtaan PowerShellin avulla](sql-database-point-in-time-restore-powershell.md) | Palauttaa Azure SQL-tietokantaan aiempaan ajankohtaan |
| 112 | [Palauttaa Azure SQL-tietokantaan, käyttämällä automaattista tietokannan varmuuskopiointi](sql-database-recovery-using-backups.md) | Lisätietoja ajankohta palauttaminen, jonka avulla voit palata Azure SQL-tietokantaan edellisessä kohdassa senhetkinen (enintään 35 päivää). |
| 113 | [Palauta poistetut Azure SQL-tietokanta Azure-portaalissa](sql-database-restore-deleted-database-portal.md) | Palauta poistetut Azure SQL-tietokanta (Azure-portaalin). |
| 114 | [Palauta poistetut Azure SQL-tietokanta PowerShell-toiminnolla](sql-database-restore-deleted-database-powershell.md) | Palauta poistetut Azure SQL-tietokanta (PowerShell). |
| 115 | [Tietokannan palauttaminen aiempaan senhetkinen tai poistetun tietokannan palauttaminen tietojen center-käyttökatkosta palauttaminen](sql-database-troubleshoot-backup-and-restore.md) | Opi palauttamaan cloud tietokannan virheiden ja katkokset käyttämällä varmuuskopiot ja replikoiden Azure SQL-tietokantaan. |



## <a name="migrate"></a>Siirtää

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 116 | [Vie BACPAC-tiedoston SqlPackage SQL Server-tietokantaan](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md) | Microsoft Azure SQL-tietokanta-tietokannan siirto vieminen tietokantaan, vie BACPAC tiedoston sqlpackage |
| 117 | [SQL Server-tietokantaan vieminen SQL Server Management Studiossa BACPAC-tiedostoon](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md) | Microsoft Azure SQL-tietokanta-tietokannan siirto vieminen tietokantaan, vie BACPAC tiedosto, Vie tiedot taso-sovelluksen ohjattu toiminto |
| 118 | [SQL-tietokantaan SqlPackage BACPAC tiedoston tuominen](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md) | Microsoft Azure SQL-tietokanta-tietokannan siirto-tietokannan tuominen, tuo BACPAC tiedoston sqlpackage |
| 119 | [Tuo BACPAC käyttämällä SSMS SQL-tietokantaan](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md) | Microsoft Azure SQL-tietokanta-tietokannan käyttöönotto-tietokannan siirto-tietokannan tuominen Vie tietokantaan, ohjattu siirto |
| 120 | [Siirtää SQL Server-tietokannan käyttäminen Microsoft Azure-tietokannan käyttöönotto tietokannan SQL-tietokantaan](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md) | Microsoft Azure SQL-tietokanta-tietokannan siirtäminen, ohjattu Microsoft Azure-tietokannan luominen |
| 121 | [Siirtää Azure SQL-tietokanta tapahtumien replikoinnin SQL Server-tietokantaan](sql-database-cloud-migrate-compatible-using-transactional-replication.md) | Tietokannan, tuominen Microsoft Azure SQL-tietokanta-tietokannan siirto tapahtumien sallittuja |
| 122 | [Määrittää SQL-tietokantaan yhteensopivuuden SqlPackage.exe käyttäminen](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md) | Microsoft Azure SQL-tietokanta-tietokannan siirto, SQL-tietokanta-yhteensopivuus-SqlPackage |
| 123 | [SQL Server Management Studiossa avulla voit määrittää SQL-tietokantaan yhteensopivuuden ennen siirtoa Azure SQL-tietokantaan](sql-database-cloud-migrate-determine-compatibility-ssms.md) | Microsoft Azure SQL-tietokanta-tietokannan siirto, SQL-tietokantaan yhteensopivuuden, Vie tiedot taso sovelluksen ohjattu |
| 124 | [SQL Azure siirron ohjatun toiminnon avulla voit korjata SQL Server-tietokannan yhteensopivuusongelmat ennen siirtoa Azure SQL-tietokantaan](sql-database-cloud-migrate-fix-compatibility-issues.md) | Microsoft Azure SQL-tietokanta-tietokannan siirto, yhteensopivuuden, ohjattu SQL Azure-siirto |
| 125 | [SQL Server-tietokantaan siirtäminen SQL Server Data Tools for Visual Studio avulla Azure SQL-tietokanta](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md) | Microsoft Azure SQL-tietokanta-tietokannan siirto, yhteensopivuuden, SQL Azure siirron ohjattu, SSDT |
| 126 | [Korjaa SQL Serverin tietokantaan yhteensopivuusongelmat käyttäen SQL Server Management Studiossa ennen siirtoa SQL-tietokantaan](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md) | Microsoft Azure SQL-tietokanta-tietokannan siirto, yhteensopivuuden, ohjattu SQL Azure-siirto |
| 127 | [Arkistoida Azure SQL-tietokantaan BACPAC tiedostoon PowerShell-toiminnolla](sql-database-export-powershell.md) | Arkistoida Azure SQL-tietokantaan BACPAC tiedostoon PowerShell-toiminnolla |
| 128 | [Voit luoda Azure SQL-tietokantaan käyttämällä PowerShell BACPAC-tiedoston tuominen](sql-database-import-powershell.md) | Voit luoda Azure SQL-tietokantaan käyttämällä PowerShell BACPAC-tiedoston tuominen |
| 129 | [Tietojen lataaminen CSV Azure SQL-tietovarasto (tasainen tiedostot)](sql-database-load-from-csv-with-bcp.md) | Pieni tietojen koko, joko käyttää bcp tuomiseen Azure SQL-tietokantaan. |
| 130 | [Siirrä tietokannat palvelinten välillä tilaukset ja Azure-ja uloskirjautuminen](sql-database-troubleshoot-moving-data.md) | Vaiheittaiset ohjeet, jos haluat kopioida, siirtää ja siirtää tiedot ja tietokantojen Azure SQL-tietokantaan. |



## <a name="move-data-in-and-out"></a>Tietojen siirtäminen sisään ja ulos

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 131 | [SQL Serverin tietokantaan siirron pilveen SQL-tietokantaan](sql-database-cloud-migrate.md) | Katso, miten paikallisen SQL Serverin tietokantaan siirto Azure SQL-tietokanta pilveen. Testaa yhteensopivuuden ennen tietokannan siirto Tietokantatyökalut siirron avulla. |
| 132 | [Kopioi Azure SQL-tietokanta](sql-database-copy.md) | Luoda kopion Azure SQL-tietokantaan |
| 133 | [Arkistoida Azure SQL-tietokantaan BACPAC tiedostoon Azure-portaalissa](sql-database-export.md) | Arkistoida Azure SQL-tietokantaan BACPAC tiedostoon Azure-portaalissa |



## <a name="security"></a>Tietoturva

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 134 | [Yhteyden muodostaminen SQL-tietokanta tai SQL-tietovarasto Azure Active Directory-todennuksen avulla](sql-database-aad-authentication.md) | Opettele muodostamaan yhteys SQL-tietokantaan Azure Active Directory-todennuksen avulla. |
| 135 | [Aina salattu: Luottamuksellisten tietojen SQL-tietokantaan ja tallentaa salausavaimet sertifikaatin Windows-kaupasta](sql-database-always-encrypted.md) | Suojaa luottamukselliset tiedot SQL-tietokantaan minuutteina. |
| 136 | [Aina salattu: Luottamuksellisten tietojen SQL-tietokantaan ja tallentaa salausavaimet Azure avain säilöön](sql-database-always-encrypted-azure-key-vault.md) | Suojaa luottamukselliset tiedot SQL-tietokantaan minuutteina. |
| 137 | [SQL-tietokanta - alatason asiakkaiden tuki- ja IP-päätepisteen muutosten seuranta](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md) | Lisätietoja SQL-tietokantaan alatason asiakkaiden tuki- ja IP päätepisteen muutosten seuranta. |
| 138 | [Määritä Azure SQL-tietokanta palvelintason palomuurisäännöt PowerShell-toiminnolla](sql-database-configure-firewall-settings-powershell.md) | Opettele määrittämään palomuurin IP-osoitteet, jotka käyttävät Azure SQL-tietokannat. |
| 139 | [Määritä Azure SQL-tietokanta palvelintason palomuurisäännöt REST-Ohjelmointirajapinnan käyttäminen](sql-database-configure-firewall-settings-rest.md) | Opettele määrittämään palomuurin IP-osoitteet, jotka käyttävät Azure SQL-tietokannat. |
| 140 | [Määritä käyttämällä T-SQL Azure SQL-tietokanta palvelintason ja tietokannan tason palomuurisäännöt](sql-database-configure-firewall-settings-tsql.md) | Opettele määrittämään palomuurin IP-osoitteet, jotka käyttävät Azure SQL-tietokannat. |
| 141 | [SQL tietokanta Dynamic Data Masking (Azure-portaalin) käytön aloittaminen](sql-database-dynamic-data-masking-get-started.md) | Miten SQL tietokannan Dynamic Data Masking Azure-portaalin käytön aloittaminen |
| 142 | [SQL tietokannan Dynamic Data Masking (perinteinen Azure-portaalin) käytön aloittaminen](sql-database-dynamic-data-masking-get-started-portal.md) | Miten SQL tietokannan dynaaminen tietojen Masking perinteinen Azure-portaalin käytön aloittaminen |
| 143 | [Määritä Azure SQL-tietokantaan palomuurisäännöt \- yleiskatsaus](sql-database-firewall-configure.md) | Opettele määrittämään SQL-tietokantaan palomuurin kanssa palvelintason ja tietokannan tason palomuurisäännöt voit hallita käyttöoikeuksia. |
| 144 | [SQL-tietokanta-opetusohjelma: käyttäjätilien hallita tietokannan ja käyttää SQL-tietokannan luominen](sql-database-get-started-security.md) | Lue, miten voit luoda käyttäjätilejä, jos haluat käyttää ja hallita tietokannan. |
| 145 | [SQL-tietokannan todennus- ja: käyttöoikeuksien myöntäminen](sql-database-manage-logins.md) | Lisätietoja SQL-tietokannan suojauksen hallinta, erityisesti miten tietokannan access ja kirjaudu sisään Suojausvyöhykkeet palvelintason pääasiallista tilin kautta. |
| 146 | [Azure SQL-tietokannan suojausohjeita ja rajoitukset](sql-database-security-guidelines.md) | Tutustu Microsoft Azure SQL-tietokanta-ohjeita ja suojausta koskevat rajoitukset. |
| 147 | [SQL-tietokannan uhkien tunnistus käytön aloittaminen](sql-database-threat-detection-get-started.md) | Miten SQL tietokanta uhkien tunnistus Azure-portaalin käytön aloittaminen |
| 148 | [Voit suorittaa yleisiä hallintatehtäviä, kuten järjestelmänvalvojan salasanan Azure SQL-tietokantaan](sql-database-troubleshoot-permissions.md) | Tässä artikkelissa käsitellään yleisten hallinnollisten tehtävien suorittamiseen SQL-tietokantaan. Esimerkiksi järjestelmänvalvojan salasanan, myöntämistä ja käyttöoikeuksien poistaminen. |



## <a name="manage-your-database"></a>Tietokannan hallinta

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 149 | [SQL-tietokannan tarkistaminen käytön aloittaminen](sql-database-auditing-get-started.md) | SQL-tietokannan tarkistaminen käytön aloittaminen |
| 150 | [Määritä saapuvan Azure SQL-tietokanta palvelintason palomuurisääntö Azure-portaalissa](sql-database-configure-firewall-settings.md) | Opettele määrittämään palomuurin IP-osoitteet, jotka käyttävät Azure SQL server. |
| 151 | [Kopioi SQL Azure-tietokannan Azure-portaalissa](sql-database-copy-portal.md) | Luoda kopion Azure SQL-tietokantaan |
| 152 | [Azure SQL-tietokantoja Azure automaatio hallinta](sql-database-manage-automation.md) | Lue lisätietoja siitä, miten Azure automaatio-palvelun avulla voidaan hallita Azure SQL-tietokantoja tasolla. |
| 153 | [Yleistä: hallintatyökalut SQL-tietokantaan](sql-database-manage-overview.md) | Vertaa asetukset hallintaan Azure SQL-tietokanta |
| 154 | [Seurannan Azure SQL-tietokanta dynaaminen hallinta-näkymien käyttäminen](sql-database-monitoring-with-dmvs.md) | Opettele tunnistamaan ja yleisiä suorituskykyongelmien vianmääritys dynaaminen hallinta näkymien avulla voit valvoa Microsoft Azure SQL-tietokanta. |
| 155 | [SQL-tietokannan suojaaminen](sql-database-security.md) | Lisätietoja Azure SQL-tietokantaan ja SQL Serverin suojaus, mukaan lukien pilveen erot ja SQL Server-ympäristöön sen tullessa todennus, todennus, yhteyden suojaus, salaus ja yhteensopivuuden. |



## <a name="extended-events"></a>Laajennettu tapahtumat

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 156 | [Tapahtuman tiedostojen kohde koodi laajennettu tapahtumien SQL-tietokantaan](sql-database-xevent-code-event-file.md) | Sisältää PowerShell ja Transact-SQL kaksivaiheinen koodi-esimerkki, joka näyttää tapahtumatiedoston kohteen laajennettu tapahtuman Azure SQL-tietokantaan. Azuren tallennustila on pakollinen osa Tämä skenaario. |
| 157 | [Soi puskurin kohde koodi laajennettu tapahtumien SQL-tietokantaan](sql-database-xevent-code-ring-buffer.md) | Sisältää Transact-SQL-koodi otoksen, joka tehdään helposti ja pika käyttämällä soi puskurin-kohteen Azure SQL-tietokantaan. |
| 158 | [Laajennettu tapahtumien SQL-tietokantaan](sql-database-xevent-db-diff-from-svr.md) | Tässä artikkelissa kuvataan laajennettu tapahtumien (XEvents) Azure SQL-tietokantaan ja miten tapahtuman istuntojen vaihdella hiukan Microsoft SQL Server-istunnot tapahtuma. |



## <a name="geo-replication"></a>GEO sallittuja

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 159 | [Aloita suunniteltu tai suunnittelematon automaattisesti Azure SQL-tietokantaan Azure-portaalissa](sql-database-geo-replication-failover-portal.md) | Aloita suunniteltu tai suunnittelematon automaattisesti Azure SQL-tietokannan Azure-portaalissa |
| 160 | [Aloita suunniteltu tai suunnittelematon automaattisesti Azure SQL-tietokantaan PowerShellin avulla](sql-database-geo-replication-failover-powershell.md) | Aloita suunniteltu tai suunnittelematon automaattisesti Azure SQL-tietokannan PowerShellin avulla |
| 161 | [Aloita suunniteltu tai suunnittelematon automaattisesti, jossa Transact-SQL Azure SQL-tietokanta](sql-database-geo-replication-failover-transact-sql.md) | Aloita suunniteltu tai suunnittelematon automaattisesti, käyttämällä Transact-SQL Azure SQL-tietokanta |
| 162 | [Yleistä: SQL tietokannan aktiivinen Geo-replikoiminen](sql-database-geo-replication-overview.md) | Aktiivinen Geo-replikoinnin mahdollistaa 4 replikoita tietokannan kaikista Azure palvelinkeskusten asetukset. |
| 163 | [Geo replikoinnin määrittäminen Azure SQL-tietokanta Azure-portaalissa](sql-database-geo-replication-portal.md) | Geo replikoinnin määrittäminen Azure SQL-tietokanta Azure-portaalissa |
| 164 | [Geo replikoinnin määrittäminen PowerShellin Azure SQL-tietokanta](sql-database-geo-replication-powershell.md) | Aktiivinen Geo-replikoinnin määrittäminen PowerShellin Azure SQL-tietokanta |
| 165 | [Azure SQL-tietokanta-suojauksen hallintaa jälkeen palauttaminen](sql-database-geo-replication-security-config.md) | Tässä ohjeaiheessa kerrotaan suojaustietoja suojauksen hallinta tietokannan palauttaminen tai automaattisesti jälkeen. |
| 166 | [Geo replikoinnin määrittäminen Transact-SQL Azure SQL-tietokanta](sql-database-geo-replication-transact-sql.md) | Geo replikoinnin määrittäminen käyttämällä Transact-SQL Azure SQL-tietokanta |
| 167 | [GEO-palauttaminen varmuuskopiosta geo tarpeettomat Azure-portaalissa Azure SQL-tietokanta](sql-database-geo-restore-portal.md) | GEO-Palauta geo tarpeettomat varmuuskopiosta (Azure-portaalin) Azure SQL-tietokantaan. |
| 168 | [Palauttaa Azure SQL-tietokantaan geo tarpeettomat varmuuskopiosta PowerShell-toiminnolla](sql-database-geo-restore-powershell.md) | Palauttaa Azure SQL-tietokantaan uuden palvelimen geo tarpeettomat varmuuskopiosta |



## <a name="upgrade"></a>Päivittäminen

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 169 | [Kyselyn suorituskykyä entistä yhteensopivuuden tason 130 Azure SQL-tietokantaan](sql-database-compatibility-level-query-performance-130.md) | Ohjeita ja tarkistamalla, mitä yhteensopivuuden tasoa parhaiten Azure SQL-tietokanta tai Microsoft SQL Server-tietokannan Työkalut |
| 170 | [Juokseva päivitykset cloud sovellusten SQL, tietokanta, aktiivinen Geo-replikoinnin hallinta](sql-database-manage-application-rolling-upgrade.md) | Opettele käyttämään online päivitykset cloud-sovelluksesi tukemaan Azure SQL-tietokanta geo-replikoinnin. |
| 171 | [Päivityksen Azure SQL-tietokannan Vipuventtiili V12 Azure-portaalissa](sql-database-upgrade-server-portal.md) | Kerrotaan, miten voit päivittää Azure SQL-tietokannan Vipuventtiili V12 mukaan lukien verkko- ja Business tietokannan päivittäminen ja sen tietokantojen siirtyminen suoraan Azure-portaalissa joustavasti tietokanta-ryhmän V11 palvelimen päivittäminen. |
| 172 | [Päivityksen Azure SQL-tietokannan Vipuventtiili V12 PowerShellin avulla](sql-database-upgrade-server-powershell.md) | Kerrotaan, miten voit päivittää Azure SQL-tietokannan Vipuventtiili V12 mukaan lukien verkko- ja Business tietokannan päivittäminen ja sen tietokantojen siirtyminen suoraan PowerShellin joustavasti tietokanta-ryhmän V11 palvelimen päivittäminen. |
| 173 | [Suunnittelu ja SQL-tietokannan Vipuventtiili V12 päivityksen valmisteleminen](sql-database-v12-plan-prepare-upgrade.md) | Tässä artikkelissa kuvataan valmistelut ja rajoitukset, jotka osallistuvat päivitystä versioon Vipuventtiili V12, Azure SQL-tietokanta. |
| 174 | [SQL-tietokannan Vipuventtiili V12 uudet ominaisuudet](sql-database-v12-whats-new.md) | Tässä artikkelissa kuvataan, miksi järjestelmien, jotka käyttävät Azure SQL-tietokanta pilveen hyötyä päivittämällä versioon Vipuventtiili V12 nyt. |
| 175 | [Web- ja Business-version Auringonlasku usein kysytyt kysymykset](sql-database-web-business-sunset-faq.md) | Ota selvää, kun Azure SQL-Web- ja Business-tietokantoja poistettu käytöstä ja ominaisuuksista ja uusi palvelutasot toimintoja. |



## <a name="tools"></a>Työkalut

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 176 | [Hallinta PowerShellin Azure SQL-tietokanta](sql-database-command-line-tools.md) | Azure SQL-tietokannan hallinta PowerShellin avulla. |
| 177 | [SQL-tietokanta-opas: Excel muodostaa Azure SQL-tietokantaan ja raportin luominen](sql-database-connect-excel.md) | Opettele Microsoft Excelin yhdistäminen pilveen Azure SQL-tietokantaan. Voit tuoda tietoja Exceliin, raportointi ja tietojen tarkasteluun. |
| 178 | [SQL-tietokannan luominen ja suorittaa yleisiä tietokannan suorittamalla PowerShell cmdlet-komennot](sql-database-get-started-powershell.md) | Opi nyt SQL-tietokannan luominen PowerShellin avulla. Yleisiä tietokannan määritystehtäviä voit hallitaan PowerShellin cmdlet-komennot. |
| 179 | [Käytön aloittaminen Azure SQL-tietojen synkronointi (ennakkoversio)](sql-database-get-started-sql-data-sync.md) | Tässä opetusohjelmassa avulla pääset alkuun SQL Azure-tietojen synkronointi (esikatselu). |
| 180 | [Azure SQL-tietokanta SQL Server Management Studiossa hallinta](sql-database-manage-azure-ssms.md) | Opettele käyttämään SQL Server Management Studiossa SQL-tietokanta-palvelimia ja tietokantojen hallinta. |
| 181 | [Azure SQL-tietokantoja Azure-portaalissa hallinta](sql-database-manage-portal.md) | Opettele Azure-portaalin avulla voit hallita relaatiotietokannasta pilveen Azure-portaalissa. |
| 182 | [Palvelun taso ja suorituskyvyn tason muuttaminen (hinnoittelu taso) PowerShellin avulla on SQL-tietokanta](sql-database-scale-up-powershell.md) | Muuta palvelutaso ja suorituskyvyn tasoon Azure SQL-tietokantaan esitetään, kuinka voit skaalata SQL-tietokantaan ylös tai alas PowerShellin avulla. Muuttaminen PowerShellin Azure SQL-tietokantaan hinnoittelu taso. |
| 183 | [Yhteyden muodostaminen SQL-tietokantaan Azure RemoteApp SQL Server Management Studiossa avulla](sql-database-ssms-remoteapp.md) | Tässä opetusohjelmassa avulla voit opetella käyttämään SQL Server Management Studiossa Azure RemoteApp turvallisuus ja suorituskyky yhdistettäessä SQL-tietokantaan |



## <a name="reference"></a>Viittaus

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 184 | [SQL-tietokantaan ja SQL Serverin kirjastot](sql-database-libraries.md) | Näyttää kunkin ohjaimen, asiakasohjelmia avulla voit muodostaa yhteyden Azure SQL-tietokanta tai Microsoft SQL Server pienin versionumero. Linkki on tarkoitettu ohjaimet, jotka on julkaistu yhteisön sijaan Microsoft versiotiedot. |
| 185 | [Azure SQL-tietokantaan resurssien rajoitukset](sql-database-resource-limits.md) | Tällä sivulla kerrotaan joitakin yleisiä resurssien rajoitukset Azure SQL-tietokantaan. |
| 186 | [Azure SQL-tietokannan Transact-SQL-erot](sql-database-transact-sql-information.md) | Transact-SQL-lauseita, jotka ovat vähemmän kuin täysin tueta Azure SQL-tietokanta |



## <a name="miscellaneous"></a>Muut

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 187 | [Kopioi PowerShellin Azure SQL-tietokantaan](sql-database-copy-powershell.md) | Luo PowerShellin Azure SQL-tietokanta |
| 188 | [Kopioi käyttämällä Transact-SQL Azure SQL-tietokantaan](sql-database-copy-transact-sql.md) | Luo käyttämällä Transact-SQL Azure SQL-tietokanta |
| 189 | [Azure SQL-tietokannan Yleiset rajoitukset ja ohjeita](sql-database-general-limitations.md) | Tällä sivulla kerrotaan Yleiset rajoituksia Azure SQL-tietokanta sekä alueet yhteensopivuus ja tuki. |
| 190 | [SQL-tietokantaan hinnoittelu taso suositukset](sql-database-service-tier-advisor.md) | Vaihdettaessa hinnoittelua tasoa Azure-portaalissa, hinnoittelua taso suosituksia on, jos suositellun taso, joka sopii parhaiten aiemmin Azure SQL-tietokantaan päivän työmäärän suorittamista varten. Hinnoittelu tasoa kuvaavat SQL-tietokantaan palvelun taso ja suorituskyvyn taso. |


&nbsp;

<hr/>

<a name="extras_append"></a>

## <a name="extras"></a>Lisäominaisuudet

<a name="extra_related_articles"></a>

### <a name="related-articles-from-other-azure-services"></a>Aiheeseen liittyviä artikkeleita Azure muista palveluista

- [Azure Azure App palveluiden sovelluksen varmuuskopiointi](../app-service-web/web-sites-backup.md)

<a name="extra_documentation_tools"></a>

### <a name="documentation-tools"></a>Ohjeet Työkalut

- [Päivitykset ilmoitettiin Azure SQL-tietokantaan](http://azure.microsoft.com/updates/?service=sql-database)

- [Etsi kaikki asiakirjat tyypit Microsoft Azure suodattimilla](http://azure.microsoft.com/search/documentation/)

<a name="extra_learning_path_graphics"></a>

### <a name="learning-path-graphics"></a>Oppimiskeskuksen polku grafiikka

- [Oppimiskeskuksen polku grafiikan kaikissa Microsoft Azure-palveluissa](http://azure.microsoft.com/documentation/learning-paths/)

- [Oppimispolku: Lisätietoja Azure SQL-tietokanta](http://azure.microsoft.com/documentation/learning-paths/sql-database-training-learn-sql-database/)

- [Oppimispolku: SQL-tietokantaan joustavasti asteikko](http://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/)


<!--
AzIxAA.ExtraAppend.sql-database.txt
C:\_MainW\VS15\AzureIndexAllArticles\AzureIndexAllArticles\In-Out\In\

This bullet link is improperly disallowed by publishing automation due to presence of string 'docuXXmentation/arXXticles':
- [Search SQL Database documentation, with filters](http://azure.microsoft.com/docuXXmentation/arXXticles/?service=sql-database)
-->


