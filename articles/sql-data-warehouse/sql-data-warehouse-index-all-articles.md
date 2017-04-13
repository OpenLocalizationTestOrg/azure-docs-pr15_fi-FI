<properties
    pageTitle="SQL-tietovarasto palvelun kaikista aiheista | Microsoft Azure"
    description="Taulukon kaikista aiheista Azure-palvelun nimi SQL-tietovarasto järjestelmässä olevat http://azure.microsoft.com/documentation/articles/ ja otsikko ja kuvaus."
    services="sql-data-warehouse"
    documentationCenter=""
    authors="barbkess"
    manager="jhubbard"
    editor="MightyPen"/>

<tags
    ms.service="sql-data-warehouse"
    ms.workload="sql-data-warehouse"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="barbkess"/>


# <a name="all-topics-for-azure-sql-data-warehouse-service"></a>Kaikki aiheet Azure SQL-tietovarasto-palveluun

Tässä artikkelissa on lueteltu jokaisen artikkelista, johon koskee Azure **SQL-tietovarasto** -palvelun. Voit hakea avainsanojen tämä sivu nykyiseen halutut aiheet **Ctrl + F**, avulla.




## <a name="new"></a>Uusi

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 1 | [SQL-tietovarasto varmuuskopiointi](sql-data-warehouse-backups.md) | Lisätietoja SQL-tietovarasto valmiin tietokannan varmuuskopioita, joiden avulla voit palauttaa Azure SQL-tietovarasto palautettava tai eri maantieteellinen alue. |


## <a name="updated-articles-sql-data-warehouse"></a>Päivitetty artikkeleita on SQL-tietovarasto

Tässä osassa on luettelo artikkeleissa, joka on päivitetty viimeksi, jossa päivitys on suuri tai merkittäviä. Kunkin päivitetyn artikkelin karkea katkelma korotuksia lisätty teksti tulee näkyviin. Artikkelit on päivitetty **2016-10-05** **2016-08-22** päivämäärävälillä.

| &nbsp; | Artikkelissa | Päivitetty teksti, koodikatkelman | Päivitetty, kun |
| --: | :-- | :-- | :-- |
| 2 | [Tietojen lataaminen Azure-blob-säiliö SQL-tietovarasto (PolyBase)](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md) | Ja-jäljittämiseksi tavua ja tiedostojen Valitse r.command, s.request_id, r.status (eri input_name), saavat arvon nbr_files, summa (s.bytes_processed) / vähintään 1 024/vähintään 1 024 kuin gb_processed sys.dm_pdw_exec_requests r sisäliitoksen sys.dm_pdw_dms_external_work s-r.request_id = s.request_id WHERE r. selitteen = ' CTAS: lataa yhtiön. DimProduct-tai r. selitteen = ' CTAS: lataa yhtiön. FactOnlineSales' GROUP BY r.command, s.request_id, r.status ORDER BY nbr_files desc gb_processed desc;  | 2016-09-07 |
| 3 | [SQL-tietovarasto palauttaminen](sql-data-warehouse-restore-database-overview.md) | **Keskeytetty tietovarasto palauttaminen** Palauttaa tietovarasto, joka on keskeytetty, sinun täytyy ensin tuoda online-tilaan. Kun tietovarasto on online-tilaan, sinun on palauttaminen asioista valittavana seitsemän päivän aikana. **Palauttaa geo tarpeettomat alue** Jos käytössäsi on geo tarpeettomat tallennustilaa, voit palauttaa tietovarasto pisteparin tietokeskuksen eri maantieteellinen alue. Tietovarasto palautetaan viimeisen päivän varmuuskopiosta. **Palauttaa aikajana** Voit palauttaa tietokannan palauttaminen tahansa viimeisten seitsemän päivän aikana. Tilannevedoksia Käynnistä aina neljän kahdeksan tuntia ja ovat käytettävissä seitsemän päivän ajan. Kun tilannevedoksen on yli seitsemän päivää vanhoja, se päättyy ja sen palauttaminen-kohta ei ole enää käytettävissä. **Palauttaa kustannukset** Palautetun tietovarasto tallennustilan maksu laskutetaan Azure Premium tallennustilan määrä. Jos viet hiiren palautetulle tietovarasto, perittävän tallennustilan Azure Premium tallennustilan määrä. Keskeyttäminen etuna on se ei ole maksu | 2016-09 – 29 |





## <a name="get-started"></a>Käytön aloittaminen

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 4 | [Azure SQL-tietovarasto-todennus](sql-data-warehouse-authentication.md) | Azure Active Directory (AAD) ja SQL Server todennusta Azure SQL-tietovarasto. |
| 5 | [Azure SQL-tietovarasto parhaat käytännöt](sql-data-warehouse-best-practices.md) | Kehitä Azure SQL-tietovarasto ratkaisuja suosituksia ja parhaita käytäntöjä, sinun tulee tietää tavalliseen tapaan. Näiden avulla voit onnistuu. |
| 6 | [Azure SQL-tietovarasto ohjaimet](sql-data-warehouse-connection-strings.md) | Yhteysmerkkijonot ja SQL-tietovarasto ohjaimet |
| 7 | [Yhteyden muodostaminen Azure SQL-tietovarasto](sql-data-warehouse-connect-overview.md) | Etsiminen palvelimen nimi ja yhteysmerkkijonon, että voit Azure SQL-tietovarasto |
| 8 | [Azure koneen Learning tietojen analysoiminen](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md) | Azure Konepohjaisten Oppimistekniikoiden avulla voit luoda ennakoivan konepohjaisten oppimistekniikoiden malliin perustuvan Azure SQL-tietovarasto tallennettuja tietoja. |
| 9 | [Kyselyn Azure SQL-tietovarasto (sqlcmd)](sql-data-warehouse-get-started-connect-sqlcmd.md) | Kysely Azure SQL-tietovarasto komentorivivalitsimet apuohjelman sqlcmd. |
| 10 | [SQL-tietovarasto tietokannan luominen käyttämällä Transact-SQL (TSQL)](sql-data-warehouse-get-started-create-database-tsql.md) | Opettele luomaan Azure SQL-tietovarasto TSQL kanssa |
| 11 | [SQL-tietovarasto tuki lippu luominen](sql-data-warehouse-get-started-create-support-ticket.md) | Miten tuki lippu luodaan Azure SQL-tietovarasto. |
| 12 | [Lataa tiedot ja Azure Data Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md) | Lisätietoja Azure Data Factory tietojen lataaminen |
| 13 | [SQL-tietovarasto PolyBase tietojen lataaminen](sql-data-warehouse-get-started-load-with-polybase.md) | Lue PolyBase on ja miten sitä käytetään tietovaraston skenaarioita. |
| 14 | [Luo Azure SQL-tietovarasto](sql-data-warehouse-get-started-provision.md) | Opettele luomaan Azure SQL-tietovarasto Azure-portaalissa |
| 15 | [Luo SQL-tietovarasto PowerShellin avulla](sql-data-warehouse-get-started-provision-powershell.md) | Luo SQL-tietovarasto PowerShell-toiminnolla |
| 16 | [Power BI tietojen visualisointi](sql-data-warehouse-get-started-visualize-with-power-bi.md) | Power BI SQL-tietovarasto tietojen visualisointi |
| 17 | [Kyselyn Azure SQL-tietovarasto (Visual Studiossa)](sql-data-warehouse-query-visual-studio.md) | Kyselyn SQL-tietovarasto Visual Studiossa. |



## <a name="develop"></a>Kehittäminen

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 18 | [SQL-tietovarasto tapahtumia optimointi](sql-data-warehouse-develop-best-practices-transactions.md) | Parhaiden käytäntöjen ohjeita kirjoittamiseen tehokas tapahtuman päivitykset Azure SQL-tietovarasto |
| 19 | [SQL-tietovarasto samanaikainen ja kuormituksen hallinta](sql-data-warehouse-develop-concurrency.md) | Ratkaisujen kehittämiseen ymmärtäminen Azure SQL-tietovarasto samanaikainen ja työmäärää hallinta. |
| 20 | [Luo taulukko valitsemalla (CTAS) kuin SQL-tietovarasto](sql-data-warehouse-develop-ctas.md) | Koodaus ja Luo taulukko nimellä select Azure SQL-tietovarasto (CTAS)-lauseen, ratkaisujen kehittämiseen liittyviä vinkkejä. |
| 21 | [Dynaamisen SQL-SQL-tietovarasto](sql-data-warehouse-develop-dynamic-sql.md) | Avulla dynaamisen SQL Azure SQL-tietovarasto for, ratkaisujen kehittämiseen liittyviä vinkkejä. |
| 22 | [Ryhmittelyperuste SQL-tietovarasto asetukset](sql-data-warehouse-develop-group-by-options.md) | Vihjeitä ryhmän Azure SQL-tietovarasto asetukset, ratkaisujen kehittämiseen. |
| 23 | [Käytä otsikoita välineen kyselyihin SQL-tietovarasto](sql-data-warehouse-develop-label.md) | Vihjeitä, ratkaisujen kehittämiseen Azure SQL-tietovarasto otsikot välineen kyselyjen käyttäminen. |
| 24 | [SQL-tietovarasto silmukat](sql-data-warehouse-develop-loops.md) | Transact-SQL-silmukoita ja korvaaminen kohdistimet Azure SQL-tietovarasto, ratkaisujen kehittämiseen liittyviä vinkkejä. |
| 25 | [SQL-tietovarasto tallennettuja toimintosarjoja.](sql-data-warehouse-develop-stored-procedures.md) | Vihjeitä tallennettuja toimintosarjoja Azure SQL-tietovarasto, ratkaisujen kehittämiseen. |
| 26 | [SQL-tietovarasto tapahtumat](sql-data-warehouse-develop-transactions.md) | Vihjeitä tapahtumat-Azure SQL-tietovarasto, ratkaisujen kehittämiseen. |
| 27 | [Käyttäjän määrittämiä rakenteita SQL varastossa](sql-data-warehouse-develop-user-defined-schemas.md) | Vihjeitä, ratkaisujen kehittämiseen Azure SQL-tietovarasto Transact-SQL-rakenteita käytön. |
| 28 | [Määritä SQL-tietovarasto muuttujat](sql-data-warehouse-develop-variable-assignment.md) | Määrittäminen Transact-SQL Azure SQL-tietovarasto muuttujat, ratkaisujen kehittämiseen liittyviä vinkkejä. |
| 29 | [SQL-tietovarasto näkymissä](sql-data-warehouse-develop-views.md) | Vihjeitä, ratkaisujen kehittämiseen Azure SQL-tietovarasto Transact-SQL-näkymien käyttäminen. |
| 30 | [Suunnitteluun ja SQL-tietovarasto coding tekniikat](sql-data-warehouse-overview-develop.md) | Kehitystä, suunnitteluun, suositukset ja coding tekniikoiden SQL-tietovarasto. |



## <a name="manage"></a>Hallinta

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 31 | [Laske power Azure SQL-tietovarasto (yleiskatsaus)-hallinta](sql-data-warehouse-manage-compute-overview.md) | Suorituskyvyn mittakaava Azure SQL-tietovarasto-ominaisuuksien ulos. Skaalaa ulos säätämällä DWUs tai keskeyttää ja jatkaa Laske resurssien kustannusten tallentamiseen. |
| 32 | [Laske virtaa Azure SQL-tietovarasto (Azure portaalin) hallinta](sql-data-warehouse-manage-compute-portal.md) | Azure portaalin tehtävien hallintaan Laske power. Skaalaa Laske resurssien säätämällä DWUs. Tai keskeyttäminen ja jatkaminen Laske resurssien kustannusten tallentamiseen. |
| 33 | [Laske virtaa Azure SQL-tietovarasto (PowerShell) hallinta](sql-data-warehouse-manage-compute-powershell.md) | Voit hallita tehtäviä PowerShell Laske power. Skaalaa Laske resurssien säätämällä DWUs. Tai keskeyttäminen ja jatkaminen Laske resurssien kustannusten tallentamiseen. |
| 34 | [Laske power Azure SQL Data Warehouse (REST) hallinta](sql-data-warehouse-manage-compute-rest-api.md) | Voit hallita tehtäviä PowerShell Laske power. Skaalaa Laske resurssien säätämällä DWUs. Tai keskeyttäminen ja jatkaminen Laske resurssien kustannusten tallentamiseen. |
| 35 | [Hallitse Laske virtaa Azure SQL-tietovarasto (T-SQL)](sql-data-warehouse-manage-compute-tsql.md) | Transact-SQL (T-SQL) tehtävät muuttamalla DWUs suorituskyvyn asteikko-kohtaa. Voit tallentaa kustannukset skaalaus takaisin huippu esiintymistä. |
| 36 | [Valvoa havainnollistamiseen DMVs käyttäminen](sql-data-warehouse-manage-monitor.md) | Opettele valvoa havainnollistamiseen DMVs avulla. |
| 37 | [Azure SQL-tietovarasto tietokantojen hallinta](sql-data-warehouse-overview-manage.md) | Yleiskatsaus SQL-tietovarasto tietokantojen hallintaa. Sisältää hallintatyökalut, DWUs ja skaalaus-kohtaa suorituskyvyn vianmääritys kyselyn suorituskykyä, muodostamisesta hyvä suojauskäytäntöjä ja tietokannan palauttaminen tietovirheitä tai alueellisen käyttökatkosta. |
| 38 | [Azure SQL-tietovarasto näytön Käyttäjäkyselyt](sql-data-warehouse-overview-manage-user-queries.md) | Yleistä huomioon otettavia seikkoja, parhaita käytäntöjä, ja tehtävät käyttäjän kyselyjen Azure SQL-tietovarasto seurantaa varten |
| 39 | [SQL-tietovarasto palauttaminen](sql-data-warehouse-restore-database-overview.md) | Tietokannan palauttaminen asetukset palautetaan Azure SQL-tietovarasto tietokannan yleiskatsaus. |
| 40 | [Palauttaa Azure SQL-tietovarasto (portaalin)](sql-data-warehouse-restore-database-portal.md) | Azure portaalin tehtävät Azure SQL-tietovarasto palauttamista varten. |
| 41 | [Palauttaa Azure SQL-tietovarasto (PowerShell)](sql-data-warehouse-restore-database-powershell.md) | Azure SQL-tietovarasto palauttaminen PowerShell tehtävät. |
| 42 | [Palauttaa Azure SQL-tietovarasto (REST API)](sql-data-warehouse-restore-database-rest-api.md) | REST API tehtävät Azure SQL-tietovarasto palauttamista varten. |



## <a name="tables-and-indexes"></a>Taulukot ja indeksit

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 43 | [Taulukot SQL-tietovarasto tietotyypit](sql-data-warehouse-tables-data-types.md) | Tietotyyppien Azure SQL-tietovarasto taulukoiden käytön aloittaminen. |
| 44 | [SQL-tietovarasto taulukoiden jakaminen](sql-data-warehouse-tables-distribute.md) | Käytön aloittaminen jakaminen Azure SQL-tietovarasto taulukot. |
| 45 | [SQL-tietovarasto indeksoinnin taulukot](sql-data-warehouse-tables-index.md) | Taulukon Azure SQL-tietovarasto indeksoinnin aloittaminen. |
| 46 | [Yleiskatsaus SQL-tietovarasto taulukot](sql-data-warehouse-tables-overview.md) | SQL Azure varaston arvotaulukot käytön aloittaminen. |
| 47 | [SQL-tietovarasto taulukoiden jakaminen](sql-data-warehouse-tables-partition.md) | Taulukon jakaminen Azure SQL-tietovarasto käytön aloittaminen. |
| 48 | [Taulukot SQL-tietovarasto tilastoja hallinta](sql-data-warehouse-tables-statistics.md) | Taulukot SQL Azure-tietovarasto tilastoja käytön aloittaminen. |
| 49 | [SQL-tietovarasto väliaikaiset taulukot](sql-data-warehouse-tables-temporary.md) | Tilapäinen taulukot SQL Azure-tietovarasto käytön aloittaminen. |



## <a name="integrate"></a>Integroi

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 50 | [SQL-tietovarasto Azure Data Factory käyttäminen](sql-data-warehouse-integrate-azure-data-factory.md) | Azure tietojen Factory (SYÖTTÖ) käyttäminen Azure SQL-tietovarasto, ratkaisujen kehittämiseen liittyviä vinkkejä. |
| 51 | [SQL-tietovarasto Azure koneen Oppimiskeskuksen käyttäminen](sql-data-warehouse-integrate-azure-machine-learning.md) | Opetusohjelma Azure SQL-tietovarasto Azure koneen Learning käyttöä varten, ratkaisujen kehittämiseen. |
| 52 | [SQL-tietovarasto Azure Stream Analytics käyttäminen](sql-data-warehouse-integrate-azure-stream-analytics.md) | Azure Stream Analytics käyttäminen Azure SQL-tietovarasto, ratkaisujen kehittämiseen liittyviä vinkkejä. |
| 53 | [Power BI käyttäminen SQL-tietovarasto](sql-data-warehouse-integrate-power-bi.md) | Power BI käyttäminen Azure SQL-tietovarasto, ratkaisujen kehittämiseen liittyviä vinkkejä. |
| 54 | [Hyödyntää muiden palveluiden kanssa SQL-tietovarasto](sql-data-warehouse-overview-integrate.md) | Työkalut ja kumppaneiden kanssa ratkaisuja, jotka SQL-tietovarasto integroida.  |



## <a name="load"></a>Lataa

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 55 | [Tietojen lataaminen Azure-blob-säiliö Azure SQL-tietovarasto (Azure Data Factory)](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md) | Lisätietoja Azure Data Factory tietojen lataaminen |
| 56 | [Tietojen lataaminen Azure-blob-säiliö SQL-tietovarasto (PolyBase)](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md) | Opettele käyttämään PolyBase lataamaan tietojen Azure-blob-säiliö SQL-tietovarasto. Lataa joitakin taulukoita julkisten tietojen Contoso jälleenmyynti tietovarasto rakenne. |
| 57 | [Tietojen lataaminen SQL Server Azure SQL-tietovarasto (AZCopy)](sql-data-warehouse-load-from-sql-server-with-azcopy.md) | Tietojen vieminen tasainen tiedostoja, voit tuoda tietoja Azure-blob-säiliö AZCopy ja PolyBase ingest tietojen tuominen SQL Azure-tietovarasto SQL Server käyttää bcp. |
| 58 | [Tietojen lataaminen SQL Server Azure SQL-tietovarasto (tasainen tiedostot)](sql-data-warehouse-load-from-sql-server-with-bcp.md) | Pieni tietojen koko, joko käyttää bcp tietojen vieminen tasainen tiedostot SQL Serverin ja tuoda tiedot suoraan Azure SQL-tietovarasto. |
| 59 | [Lataa tiedot SQL Server Azure SQL tietojen varasto (SSIS) tuominen](sql-data-warehouse-load-from-sql-server-with-integration-services.md) | Näytetään, miten voit luoda SQL Server Integration Services (SSIS)-paketti, jos haluat siirtää tietoja useista tietolähteistä SQL-tietovarasto. |
| 60 | [SQL-tietovarasto PolyBase tietojen lataaminen](sql-data-warehouse-load-from-sql-server-with-polybase.md) | Tietojen vieminen tasainen tiedostoja, voit tuoda tietoja Azure-blob-säiliö AZCopy ja PolyBase ingest tietojen tuominen SQL Azure-tietovarasto SQL Server käyttää bcp. |
| 61 | [Oppaan PolyBase käyttäminen SQL-tietovarasto](sql-data-warehouse-load-polybase-guide.md) | Ohjeita ja suosituksia PolyBase käyttäminen SQL-tietovarasto skenaarioita. |
| 62 | [Lataa mallitiedot SQL-tietovarasto](sql-data-warehouse-load-sample-databases.md) | Lataa mallitiedot SQL-tietovarasto |
| 63 | [Lataa bcp tietojasi](sql-data-warehouse-load-with-bcp.md) | Katso, mitä bcp on ja miten sitä käytetään tietovaraston skenaarioita. |
| 64 | [Ladata tietoja SQL Azure-tietovarasto](sql-data-warehouse-overview-load.md) | Lue Yleisiä tilanteita, joissa tietojen SQL-tietovarasto ladataan. Näitä ovat PolyBase, Azure-blob-säiliö, tasainen tiedostoja ja levyllä-toimitus. Voit käyttää myös kolmannen osapuolen työkaluja. |



## <a name="migrate"></a>Siirtää

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 65 | [SQL-koodin siirtäminen SQL-tietovarasto](sql-data-warehouse-migrate-code.md) | SQL-koodin siirtämisestä Azure SQL-tietovarasto, ratkaisujen kehittämiseen liittyviä vinkkejä. |
| 66 | [Tietojen siirtäminen](sql-data-warehouse-migrate-data.md) | Tietojen siirtäminen SQL Azure-tietovarasto, ratkaisujen kehittämiseen liittyviä vinkkejä. |
| 67 | [Data Warehouse siirron apuohjelman (ennakkoversio)](sql-data-warehouse-migrate-migration-utility.md) | Siirtää SQL-tietovarasto. |
| 68 | [Rakenteen siirtäminen SQL-tietovarasto](sql-data-warehouse-migrate-schema.md) | Rakenteen siirtämisestä Azure SQL-tietovarasto, ratkaisujen kehittämiseen liittyviä vinkkejä. |
| 69 | [Siirtää SQL-tietovarasto ratkaisu](sql-data-warehouse-overview-migrate.md) | Siirron joka tuo ratkaisu Azure SQL-tietovarasto ympäristö ohjeita. |



## <a name="partners"></a>Kumppaneille

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 70 | [SQL-tietovarasto business intelligence kumppaneille](sql-data-warehouse-partner-business-intelligence.md) | Kolmannen osapuolen business intelligence kumppanien ratkaisuja, jotka tukevat SQL-tietovarasto luettelot. |
| 71 | [SQL-tietovarasto tietojen integrointi kumppanien](sql-data-warehouse-partner-data-integration.md) | Kolmannen osapuolen kumppaneiden kanssa tietojen integrointi ratkaisuja, jotka tukevat SQL Azure-tietovarasto luettelot. |
| 72 | [SQL-tietovarasto tietojen hallinta-kumppanit](sql-data-warehouse-partner-data-management.md) | Kolmannen osapuolen tietojen hallinnan kumppaneiden kanssa ratkaisuja, jotka tukevat SQL-tietovarasto luettelot. |



## <a name="reference"></a>Viittaus

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 73 | [SQL-tietovarasto ohjeaiheista](sql-data-warehouse-overview-reference.md) | Viittaus sisällön SQL-tietovarasto linkit. |
| 74 | [PowerShellin cmdlet-komennot ja REST API SQL-tietovarasto varten](sql-data-warehouse-reference-powershell-cmdlets.md) | Etsi Azure SQL-tietovarasto sekä keskeyttäminen ja jatkaminen tietokannan yläreunan PowerShellin cmdlet-komennot. |
| 75 | [Kieli-osat](sql-data-warehouse-reference-tsql-language-elements.md) | Luettelo linkeistä Transact-SQL-kielen-elementit, joita käytetään SQL-tietovarasto viittaus sisällön. |
| 76 | [Transact-SQL-ohjeaiheet](sql-data-warehouse-reference-tsql-statements.md) | Viittaus sisällön SQL-tietovarasto käyttämä Transact-SQL-ohjeita on linkkejä. |
| 77 | [Järjestelmänäkymiä](sql-data-warehouse-reference-tsql-system-views.md) | Järjestelmän linkkejä näkee sisällön SQL-tietovarasto varten. |



## <a name="security"></a>Tietoturva

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 78 | [SQL-tietovarasto - alatason asiakkaille tuki Masking valvonnan ja dynaamiset tiedot](sql-data-warehouse-auditing-downlevel-clients.md) | Lisätietoja SQL-tietovarasto alatason asiakkaiden tuki tietojen tarkistaminen |
| 79 | [Azure SQL-tietovarasto tarkistaminen](sql-data-warehouse-auditing-overview.md) | Azure SQL-tietovarasto tarkistaminen käytön aloittaminen |
| 80 | [Aloita kanssa läpinäkyvä tietojen salaus (TDE)-SQL-tietovarasto](sql-data-warehouse-encryption-tde.md) | Läpinäkyvä salauksen (TDE)-SQL-tietovarasto |
| 81 | [Aloita kanssa läpinäkyvä tietojen salaus (TDE)](sql-data-warehouse-encryption-tde-tsql.md) | Läpinäkyvä salauksen (TDE)-SQL-tietovarasto (T-SQL) |
| 82 | [SQL-tietovarasto-tietokannan suojaaminen](sql-data-warehouse-overview-manage-security.md) | Azure SQL-tietovarasto tietokannan suojaaminen, ratkaisujen kehittämiseen liittyviä vinkkejä. |



## <a name="miscellaneous"></a>Muut

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 83 | [Asenna Visual Studio 2015 ja SSDT SQL-tietovarasto](sql-data-warehouse-install-visual-studio.md) | Asenna Visual Studio ja SQL Server Development Tools (SSDT) Azure SQL-tietovarasto |
| 84 | [Siirron Premium tallennustilan tiedot](sql-data-warehouse-migrate-to-premium-storage.md) | SQL-tietovarasto premium tallennustilan siirtämistä koskevia ohjeita |
| 85 | [Aloita uhkien tunnistus](sql-data-warehouse-security-threat-detection.md) | Miten uhkien tunnistus käytön aloittaminen |
| 86 | [SQL-tietovarasto kapasiteetin rajoitukset](sql-data-warehouse-service-capacity-limits.md) | Yhteydet, tietokannoista, taulukoita ja kyselyjä ja SQL-tietovarasto suurimmat arvot. |
| 87 | [Azure SQL-tietovarasto vianmääritys](sql-data-warehouse-troubleshoot.md) | Vianmääritys Azure SQL-tietovarasto. |

