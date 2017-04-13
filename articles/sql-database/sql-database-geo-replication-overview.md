<properties
    pageTitle="Aktiivinen Geo-replikoinnin Azure SQL-tietokantaan"
    description="Aktiivinen Geo-replikoinnin mahdollistaa 4 replikoita tietokannan kaikista Azure palvelinkeskusten asetukset."
    services="sql-database"
    documentationCenter="na"
    authors="stevestein"
    manager="jhubbard"
    editor="monicar" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="NA"
    ms.date="09/26/2016"
    ms.author="sstein" />

# <a name="overview-sql-database-active-geo-replication"></a>Yleistä: SQL tietokannan aktiivinen Geo-replikoiminen

Aktiivinen Geo-replikoinnin avulla voit määrittää enintään neljä luettavissa toissijainen tietokantojen samassa tai eri tietojen center sijainneissa (alueet). Toissijainen tietokantoja on saatavana kyselyt ja tietojen center-käyttökatkosta tai ensisijainen tietokantayhteyden muodostamisessa ei tue.

>[AZURE.NOTE] Kaikki palvelutasot kaikki tietokannat on nyt aktiivinen Geo-replikoinnin (luettavissa secondaries). Huhtikuussa 2017 ei voi lukea toissijainen tyyppi poistettu käytöstä ja ei voi lukea aiemmin luotujen tietokantojen päivitetään automaattisesti luettavissa secondaries.

 Voit määrittää aktiivisen Geo-replikoinnin käyttämällä [Azure portal](sql-database-geo-replication-portal.md), [PowerShell](sql-database-geo-replication-powershell.md), [Transact-SQL](sql-database-geo-replication-transact-sql.md)- tai [REST API - Luo tai Päivitä tietokannan](https://msdn.microsoft.com/library/azure/mt163685.aspx).

> [AZURE.SELECTOR]
- [Määritä: Azure portal](sql-database-geo-replication-portal.md)
- [Määritä: PowerShell](sql-database-geo-replication-powershell.md)
- [Määritä: T-SQL](sql-database-geo-replication-transact-sql.md)

Jos tahansa syystä ensisijaisen tietokannan epäonnistuu tai yksinkertaisesti on otettava offline-tilassa, voit *automaattisesti* kaikkiin toissijainen tietokantoja. Kun automaattisesti on käytössä jokin toissijainen tietokantoja, muiden secondaries linkitetään automaattisesti uuden ensisijainen.

Voit automaattisesti toissijainen [Azure portal](sql-database-geo-replication-failover-portal.md), [PowerShell](sql-database-geo-replication-failover-powershell.md), [Transact-SQL](sql-database-geo-replication-failover-transact-sql.md), [REST API - suunniteltu automaattisesti](https://msdn.microsoft.com/ibrary/azure/mt575007.aspx)tai [REST API - suunnittelematon automaattisesti](https://msdn.microsoft.com/library/azure/mt582027.aspx).


> [AZURE.SELECTOR]
- [Automaattisesti: Azure portal](sql-database-geo-replication-failover-portal.md)
- [Automaattisesti: PowerShell](sql-database-geo-replication-failover-powershell.md)
- [Automaattisesti: T-SQL-](sql-database-geo-replication-failover-transact-sql.md)

Automaattisesti, kun varmistat todennusvaatimukset palvelin ja tietokanta on määritetty uusi ensisijaisessa. Lisätietoja on artikkelissa [SQL-tietokannan suojaus palauttaminen jälkeen](sql-database-geo-replication-security-config.md).


Aktiivinen Geo-replikoinnin ominaisuus toteuttaa, jolla tarjoaa tietokannan luotettavuutta samaan Microsoft Azure-alueen sisällä tai eri alueilla (geo redundancy). Aktiivinen Geo-replikoinnin kopioi asynkronisesti vahvistettu tapahtumat tietokannasta enintään neljä kopiota tietokannan eri palvelimissa, käyttämällä lukea varten eristystaso eristystaso vahvistettu tilannevedoksen (RCSI). Kun aktiivinen Geo-replikoinnin on määritetty, toissijaisen tietokanta on luotu määritetyssä palvelimessa. Alkuperäisen tietokannan tulee ensisijaisen tietokannan. Ensisijaisen tietokannan kopioi vahvistettu tapahtumat asynkronisesti jokaiseen toissijainen tietokannat. Vain koko tapahtumat, replikoida. Kun vaiheissa, toissijaisen tietokanta saattaa olla hieman takana ensisijaisen tietokannan, toissijaisen tietolähteen on varmasti sisältää osittaisia tapahtumat. RPO tiedot tukikäytännöistä [Liiketoiminnan jatkuvuus yleiskatsaus](sql-database-business-continuity.md).

Yksi ensisijainen edut aktiivinen Geo-replikoinnin on, että se sisältää tietokannan tason palauttamisratkaisun pienen Palautumisaika. Kun siirrät toissijaisen tietokanta toisella alueella palvelimessa, voit lisätä suurin toimintakykyyn sovelluksen. Rajat-alueen redundancy mahdollistaa sovellukset voivat kadota lopullisesti palvelinkeskukseen, aiheutuvat luonnonmullistusten, kriittinen ihmisten virheitä tai haittaohjelmien säädösten osien tai koko palvelinkeskuksen palauttaminen. Seuraavassa kuvassa on esimerkki Active Geo-replikoinnin määritetty ensisijaisen Pohjois keskitetyn US alueen ja toissijainen Etelä keskitetyn US-alueella.

![GEO replikoinnin suhde](./media/sql-database-active-geo-replication/geo-replication-relationship.png)

Toisen tärkein etu on toissijainen tietokannat ovat luettavissa ja voidaan käyttää vain luku-toiminnoista, kuten raportoinnin työt purku. Jos haluat vain toissijaisen tietokanta käytettäviä kuormituksen tasaamisen, voit luoda niistä ensisijaiseksi samalla alueella. Luot toissijainen samalla alueella, Suurenna sovelluksen toimintakykyyn, kriittinen virheet.  

Muissa tilanteissa aktiivinen Geo-replikoinnin käyttökohteen ovat seuraavat:

- **Tietokannan siirto**: aktiivinen Geo-replikoinnin avulla voit siirtää tietokannan palvelimesta toiseen verkossa pienin käyttökatkot kanssa.
- **Sovelluksen päivitykset**: Voit luoda ylimääräistä toissijainen virheiden takaisin kopiona sovelluksen päivitysten aikana.

Tavoitteet reaali Liiketoiminnan jatkuvuus lisääminen tietokantaan redundancy palvelinkeskusten välillä on vain osa ratkaisua. Palauttaminen sovellus (palvelu)-loppuun, kun kriittinen virhe edellyttää, että kaikki komponentit, jotka muodostavat palvelu ja riippuvaiset palvelut. Esimerkkejä nämä osat ovat Asiakasohjelmistoa (esimerkiksi mukautetut JavaScript ja selain), WWW, varasto ja DNS. On tärkeää, että kaikki komponentit ovat joustavat saman virheet ja saatavilla palautus aika tavoitteen (RTO) sovelluksen sisällä. Sen vuoksi sinun on tunnistaa kaikki riippuvaiset palvelut ja ymmärrät oikeudet ja niiden ominaisuuksista. Valitse sinun on tehtävä riittävä vaiheet varmistaa, että palvelu toimii aikana vikasietotilaa palveluja, johon se on riippuvainen. Saat lisätietoja palauttaminen ratkaisujen suunnitteleminen [Cloud ratkaisujen suunnitteleminen, tietojen palauttaminen Active Geo-replikointia käyttämällä](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

## <a name="active-geo-replication-capabilities"></a>Aktiivinen Geo replikoinnin ominaisuudet
Aktiivinen Geo-replikoinnin-toiminto tarjoaa olennaiset seuraavia ominaisuuksia:

- **Automaattinen asynkroninen replikoinnin**: Voit luoda toissijaisen tietokanta vain lisäämällä aiemmin luotuun tietokantaan. Toissijaisen voi luoda vain toiseen Azure SQL-tietokanta-palvelimeen. Luomiasi toissijaisen tietokanta lisätään ensisijaisen tietokannan kopioituja tietoja. Tämä toimenpide on nimeltään valuuttamuunnosten. Kun toissijaisen tietokanta on luotu ja kylvetään, ensisijaisen tietokannan päivitykset, asynkronisesti replikoida toissijaisen tietokanta automaattisesti. Asynkroninen replikoinnin tarkoittaa, että tapahtumat on vahvistettu ensisijainen tietokannan, ennen kuin he ovat replikoida toissijaisen tietokanta. 

- **Useiden toissijainen tietokantojen**: kahden tai useamman toissijainen tietokantojen Suurenna redundanssin ja ensisijaisen tietokannan ja sovelluksen suojaustaso. Jos useita toissijainen tietokantoja, sovelluksen pysyy suojatun, vaikka jokin toissijainen tietokantojen epäonnistuu. Jos luettelossa on vain yksi toissijainen tietokanta ja epäonnistuu, sovellus on määritetty suurempi riski, ennen kuin uusi toissijainen tietokanta on luotu.

- **Luettavissa toissijainen tietokannat**: sovellus voi käyttää vain luku-toimintojen käyttäminen samassa tai eri käyttöoikeudet käytetään avaaminen ensisijaisen tietokannan toissijainen tietokannan. Toissijainen tietokannat toimivat tilannevedoksen eristystaso tilassa varmistaakseen replikoinnin perus (log uusinnan) päivitysten viivästyy ei kyselyjen suorittaa toissijaisen.

>[AZURE.NOTE] Log uusinnan viivästyy toissijaisen tietokanta, jos rakenteen päivitykset, jotka se vastaanottaa on ensisijainen, jotka edellyttävät toissijainen tietokannan rakenteen lukitusta.

- **Aktiivinen geo-replikoinnin joustavasti resurssivarantoon tietokantojen**: aktiivinen Geo-replikoinnin voidaan määrittää minkä tahansa joustavasti tietokannan varannon tietokannan. Toissijaisen tietokanta voi olla toisen joustavasti tietokannan resurssivarantoon. Normaali-tietokantoja varten toissijaisen voi joustavasti tietokannan resurssivarantoon ja päin vastoin, kunhan palvelutasot ovat samat. 

- **Toissijaisen tietokanta määritettäviä suorituskyvyn tasoon**: toissijaisen tietokanta voidaan luoda pienempi kuin ensisijaisen suorituskyvyn taso. Ensisijainen ja toissijainen tietokantojen tarvitaan palvelun samalla tasolla. Tämä asetus ei suositella sovellusten hyvin tietokannan kirjoittaminen aktiviteettiin koska parantavat replikoinnin viive kasvattaa merkittäviin tietojen menettämisen riskiä jälkeen automaattisesti. Lisäksi jälkeen automaattisesti sovelluksen suorituskyvyn vaikuttaa ennen kuin uusi ensisijainen päivitetään suorituskyvyn ylemmälle tasolle. Azure-portaalissa log IO prosentti-kaavio on erinomainen tapa arvioida toissijaisen, joita tarvitaan ylläpitää replikoinnin kuormituksen mahdollisimman vähän suorituskyvyn taso. Esimerkiksi, jos ensisijainen tietokanta on 6 (1000 DTU) ja sen loki IO prosenttia on 50 % toissijaisen on oltava vähintään P4 (500 DTU). Voit myös hakea [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) tai [sys.dm_db_resource_stats]( https://msdn.microsoft.com/library/dn800981.aspx) tietokanta-näkymien käyttäminen IO lokitiedot.  Katso lisätietoja SQL-tietokannan suorituskykyä tasoilla, [SQL-tietokanta-asetukset ja suorituskykyä](sql-database-service-tiers.md). 

- **Käyttäjän ohjaa automaattisesti ja tuntisesta**: toissijaisen tietokanta erikseen voi päivittää ensisijaisuus milloin tahansa sovelluksen tai käyttäjän. Todellinen käyttökatkosta aikana "suunnittelematon" vaihtoehto käytetään, joka korostaa heti toissijainen on ensisijainen. Kun epäonnistuneita perus palauttaa ja on käytettävissä, järjestelmä merkitsee palautetun ensisijaisen toissijainen nimellä ja noutaa sen uuden ensisijainen tasalla automaattisesti. Replikoinnin asynkroninen laatu vuoksi pienen osan tietojen katoavat aikana suunnittelematon failovers Jos ensisijainen epäonnistuu, ennen kuin se kopioi toissijaisen viimeisimmät muutokset. Kun useita secondaries kanssa ensisijaisen epäonnistuu päälle, järjestelmä telakoit replikoinnin yhteydet automaattisesti sekä linkkejä jäljellä olevan secondaries juuri ylemmän tason ensisijainen ilman käyttäjän toimia. Kun käyttökatkosta, jotka aiheuttivat vikasietotilaa, joiden on lievity, kohdalla voi olla olisi palauttaa ensisijainen alueen sovelluksen. Saadakseen ne automaattisesti-komento on kutsuttu "suunniteltu"-vaihtoehto. 

- **Tunnistetietojen ja palomuurisäännöt synkronoinnissa**: suosittelemme käytön [tietokantaan palomuurisäännöt](sql-database-firewall-configure.md) geo replikoida tietokannat niin sääntöjen voi replikoida tietokannassa varmistamiseksi toissijainen piilotetusta on ensisijainen palomuurisääntöjä. Tämän menetelmän ei tarvitse asiakkaat voivat määrittää ja hallita palomuurisäännöt palvelimissa, isännöinnin sekä ensisijainen ja toissijainen tietokannat manuaalisesti. Vastaavasti käyttämällä [sisälsi tietokannan käyttäjiä](sql-database-manage-logins.md) varten tietojen käytön varmistaa ensisijainen ja toissijainen tietokantoja on aina sama käyttäjä tunnistetiedot niin automaattisesti aikana, ei ole häiriöiden vuoksi lähdeluetteloiden kirjautumiset ja salasanat. [Azure Active Directory](../active-directory/active-directory-whatis.md)lisäksi asiakkaat voit hallita käyttöoikeuden ensisijainen ja toissijainen tietokantojen ja poistamisesta edellyttää tietokantojen kokonaan tunnistetietojen hallinta.

## <a name="upgrading-or-downgrading-a-primary-database"></a>Päivitysprosessin ensisijaisen tietokannan tai päivittäminen
Voit päivittää tai alentaa ensisijaisen tietokannan eri suorituskyvyn tasolle (sisällä palvelun samalla tasolla) ilman yhteyden katkaiseminen toissijainen tietokantoja. Päivitystä, on suositeltavaa toissijainen-tietokannan päivittäminen ensin ja päivitä sitten ensisijainen. Kun päivitysprosessin, käänteinen järjestys: alentaa ensisijainen ensin ja alentaa toissijaisen. 

Toissijaisen tietokanta on oltava sama palvelutaso, ensisijaisena, jotta siirtyminen ensisijaisen tietokannan eri taso edellyttää, että Lopeta geo replikoinnin-linkki ja nimeä toissijaisen tietokanta ja pudota se. Valitse ensisijainen siirtäminen uuteen palvelutaso ja geo replikoinnin uudelleen. Uusi toissijainen luodaan automaattisesti ja ensisijaisen suorituskyvyn samalla tasolla oletusarvoisesti.

## <a name="preventing-the-loss-of-critical-data"></a>Tärkeitä tietoja menettämisen estäminen
Leveä alueen verkkojen odotusaika vuoksi jatkuva kopioi käyttää asynkroninen replikoinnin järjestelmä. Asynkroninen replikoinnin tekee kadota tietoja välttämätön virheen ilmetessä. Jotkin sovellukset voivat kuitenkin vaatia ei ole tietojen menettämisen. Sovelluksen kehittäjän soittaa suojaamiseen kriittiset päivitykset heti, kun vahvistetaan tapahtuman [sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx) järjestelmän kuvatulla tavalla. Kutsuminen **sp_wait_for_database_copy_sync** lohkot puheluja viestiketjun, kunnes viimeisen vahvistettu tapahtuman on ollut replikoida toissijaisen tietokanta. Menettelyn odottaa, kunnes kaikki jonossa olevat tapahtumat on ollut kuitata toissijaisen tietokanta. **sp_wait_for_database_copy_sync** on määritetty tietyn jatkuva Kopioi linkki. Kaikki käyttäjät, joilla ensisijaisen tietokannan yhteys voivat osallistua seuraavasti.

>[AZURE.NOTE] **Sp_wait_for_database_copy_sync** toimintosarjan puhelun aiheutuvat viive voi merkittävästi. Viive riippuu tapahtuman log pituus koon tällä hetkellä ja puhelun Palauta, ennen kuin koko loki replikoida. Vältä puhelut näin paitsi välttämättömiä.

## <a name="programmatically-managing-active-geo-replication"></a>Aktiivinen Geo-replikoinnin ohjelmallisesti hallinta

Kuten edellä aiemmin aktiivinen Geo-replikoinnin voidaan hallita myös PowerShellin Azure ja REST-Ohjelmointirajapinnalla käyttäminen ohjelmallisesti. Seuraavissa taulukoissa kuvataan olevat komennot.

- **Azure Resurssienhallinta API ja Roolipohjainen suojaus**: aktiivinen Geo-replikoinnin sisältää joukon [Azure Resurssienhallinta ohjelmointirajapinnan]( https://msdn.microsoft.com/library/azure/mt163571.aspx) hallinnan, mukaan lukien [Azure Resurssienhallinta-pohjainen PowerShellin cmdlet-komennot](sql-database-geo-replication-powershell.md). Nämä API edellyttää resurssiryhmiä ja tukea Roolipohjainen suojaus (RBAC). Lisätietoja Accessin ottamisesta käyttöön roolit artikkelissa [Azure Role-Based käyttöoikeuksien hallinta](../active-directory/role-based-access-control-configure.md).

>[AZURE.NOTE] Monia uusia ominaisuuksia Active Geo-replikoinnin tuetaan vain käyttämällä [Azure Resurssienhallinta](../azure-resource-manager/resource-group-overview.md) [Azure SQL-REST API](https://msdn.microsoft.com/library/azure/mt163571.aspx) ja [Azure SQL-tietokannan PowerShellin cmdlet-komennot](https://msdn.microsoft.com/library/azure/mt574084.aspx)perusteella. (Perinteinen) REST-Ohjelmointirajapinnalla] (https://msdn.microsoft.com/library/azure/dn505719.aspx) ja [Azure SQL-tietokanta (perinteinen) cmdlet-komennot](https://msdn.microsoft.com/library/azure/dn546723.aspx) ovat tuettuja yhteensopivuuden, joten Azure Resurssienhallinta-pohjainen-ohjelmointirajapinnan käyttäminen on suositeltavaa. 


### <a name="transact-sql"></a>Transact-SQL

|Komento|Kuvaus|
|-------|-----------|
|[MUUTTAA TIETOKANTAA (Azure SQL-tietokanta)](https://msdn.microsoft.com/library/mt574871.aspx)|Lisää toissijaisen edelleen palvelimen argumentin avulla luodun tietokannan ja käynnistää tietojen replikoinnin toisen tietokannan luominen|
|[MUUTTAA TIETOKANTAA (Azure SQL-tietokanta)](https://msdn.microsoft.com/library/mt574871.aspx)|AUTOMAATTISESTI tai FORCE_FAILOVER_ALLOW_DATA_LOSS avulla voit siirtyä toissijaisen tietokanta on ensisijainen aloittaminen automaattisesti
|[MUUTTAA TIETOKANTAA (Azure SQL-tietokanta)](https://msdn.microsoft.com/library/mt574871.aspx)|Poista toissijaisen edelleen palvelimen avulla voit lopettaa tietojen replikoinnin, SQL-tietokantaan ja määritetty toissijainen tietokannan välillä.|
|[sys.geo_replication_links (Azure SQL-tietokanta)](https://msdn.microsoft.com/library/mt575501.aspx)|Palauttaa kaikkien replikoinnin linkkien kunkin tietokannan tietoja looginen Azure SQL-tietokanta-palvelimeen.|
|[sys.dm_geo_replication_link_status (Azure SQL-tietokanta)](https://msdn.microsoft.com/library/mt575504.aspx)|Saa replikoinnin viimeksi, viimeisen replikoinnin viive ja muita tietoja replikoinnin linkin annetun SQL-tietokantaan.|
|[sys.dm_operation_status (Azure SQL-tietokanta)](https://msdn.microsoft.com/library/dn270022.aspx)|Näyttää kaikki tietokannan toiminnot, kuten replikoinnin linkkien tilan tila.|
|[sp_wait_for_database_copy_sync (Azure SQL-tietokanta)](https://msdn.microsoft.com/library/dn467644.aspx)|Odota, kunnes kaikki vahvistettu tapahtumat replikoida ja toissijainen aktiivisen tietokantaikkunan Kuittaa sovellusten käynnistymistä.|
||||

### <a name="powershell"></a>PowerShellin

|Cmdlet-komento|Kuvaus|
|------|-----------|
|[Hae AzureRmSqlDatabase](https://msdn.microsoft.com/en-us/library/azure/mt603648.aspx)|Saa yksi tai useampi tietokanta.|
|[Uusi AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt603689.aspx)|Luo toisen tietokannan aiemmin luodun tietokannan ja aloittaa tietojen replikoinnin.|
|[Määritä AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/en-us/library/mt619393.aspx)|Siirtyy toissijaisen tietokanta on ensisijainen aloittaminen automaattisesti.|
|[Poista AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/en-us/library/mt603457.aspx)|Lopettaa tietojen replikoinnin SQL-tietokantaan ja määritetty toissijainen tietokannan välillä.|
|[Hae AzureRmSqlDatabaseReplicationLink](https://msdn.microsoft.com/library/mt619330.aspx)|Saa geo replikoinnin linkit Azure SQL-tietokantaan ja resurssiryhmä tai SQL Serverin välillä.|
||||

### <a name="rest-api"></a>REST-OHJELMOINTIRAJAPINNALLA

|OHJELMOINTIRAJAPINTA|Kuvaus|
|---|-----------|
|[Luo tai Päivitä tietokannan (createMode = palauttaminen)](https://msdn.microsoft.com/library/azure/mt163685.aspx)|Luo, päivittää tai palauttaa ensisijaisen tai toissijaisen tietokanta.|
|[Hae luominen tai päivittäminen tietokannan tila](https://msdn.microsoft.com/library/azure/mt643934.aspx)|Palauttaa tilan luominen-toiminnon aikana.|
|[Määritä toissijaisen tietokanta ensisijaisena (suunnitellun automaattisesti)](https://msdn.microsoft.com/ibrary/azure/mt575007.aspx)|Voit viedä Geo replikoinnin-partnership luomisella ensisijainen uuden tietokannan toissijainen tietokannasta.|
|[Määritä toissijaisen tietokanta ensisijaisena (suunnittelematon automaattisesti)](https://msdn.microsoft.com/library/azure/mt582027.aspx)|Pakota toissijaisen tietokanta automaattisesti ja määrittää toissijaisen ensisijainen.|
|[Replikoinnin linkkien hankkiminen](https://msdn.microsoft.com/library/azure/mt600929.aspx)|Hakee kaikki replikoinnin linkit tietyn SQL-tietokannan geo replikoinnin yhteistyössä. Se hakee sys.geo_replication_links luettelon-näkymässä näkyvät tiedot.|
|[Replikoinnin linkki](https://msdn.microsoft.com/library/azure/mt600778.aspx)|Noutaa linkin tiettyyn replikoinnin geo replikoinnin partnership annetun SQL-tietokannan. Se hakee sys.geo_replication_links luettelon-näkymässä näkyvät tiedot.|
||||



## <a name="next-steps"></a>Seuraavat vaiheet

- Liiketoiminnan jatkuvuus-yleiskatsaus ja tilanteita, joissa on artikkelissa [liiketoiminnan jatkuvuuden yhteenveto](sql-database-business-continuity.md)
- Lisätietoja Azure SQL-tietokanta automaattisen varmuuskopioinnin on artikkelissa [SQL-tietokantaan automaattisen varmuuskopioinnin](sql-database-automated-backups.md).
- Lisätietoja automaattisen varmuuskopioinnin käyttämisestä palauttamista varten on artikkelissa [tietokannan palvelun käynnistämä-varmuuskopioista palauttaminen](sql-database-recovery-using-backups.md).
- Lisätietoja käyttämisestä automaattisen varmuuskopioinnin arkistointia varten on artikkelissa [tietokannan kopiota](sql-database-copy.md).
- Lisätietoja käyttöoikeuksien vaatimukset uusi ensisijainen palvelin ja tietokanta on kohdassa [SQL-tietokantaan security palauttaminen jälkeen](sql-database-geo-replication-security-config.md).
