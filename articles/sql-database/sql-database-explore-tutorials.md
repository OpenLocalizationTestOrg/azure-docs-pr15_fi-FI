<properties
   pageTitle="Tutustu Azure SQL-tietokannan opetusohjelmat | Microsoft Azure"
   description="Lisätietoja SQL-tietokanta-ominaisuuksia ja toimintoja"
   keywords=""
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
   ms.workload="data-management"
   ms.date="08/24/2016"
   ms.author="carlrab"/>
   
# <a name="explore-azure-sql-database-tutorials"></a>Tutustu Azure SQL-tietokanta-opetusohjelmat

Alla olevia linkkejä vievät, luettelossa ominaisuus kullekin alueelle ja yksinkertainen vaiheen alkamis-opetusohjelma kullekin alueelle. Ratkaisu kohdistettu nopeasti käynnistyy, jotka osoittavat käytännön skenaarioille perusteella täydellinen ratkaisu Käytä SQL-tietokannan artikkelissa [Azure SQL tietokanta ratkaisu nopeasti käynnistyy](sql-database-solution-quick-starts.md).

## <a name="using-sql-server-management-studio"></a>SQL Server Management Studiossa

Seuraavat Opetusohjelmissa opit käyttämisestä SQL Server Management Studiossa hallinta ja kyselyjen Azure SQL-tietokantaan.


> [AZURE.IMPORTANT] On suositeltavaa, että käytät Management Studiossa uusimman version aina pysyvän synkronoituja Microsoft Azure ja SQL-tietokantaan. [Päivitä SQL Server Management Studiossa](https://msdn.microsoft.com/library/mt238290.aspx).


| Opetusohjelma  | Kuvaus  |
|---|---|---|
| [Yhteyden muodostaminen Azure SQL-tietokanta palvelintason pääasiallista kirjautuminen](sql-database-get-started-security.md#connect-to-azure-sql-database-using-a-server-level-principal-login)| Tässä opetusohjelmassa kerrotaan yhdistämisestä Azure SQL-tietokanta palvelintason pääasiallista Kirjaudu sisään.|
| [Yhteyden muodostaminen SQL Azure-tietokannan käyttäjänä](sql-database-get-started-security.md#connect-to-azure-sql-database-as-a-user) | Tässä opetusohjelmassa kerrotaan yhdistämisestä tietokannan tason käyttäjätunnuksella Azure SQL-tietokantaan.|
||||

## <a name="elastic-pools"></a>Joustavasti jakavat

Seuraavat Opetusohjelmissa opit [joustavasti jakavat](sql-database-elastic-pool.md) avulla voit hallita useita tietokantoja, joissa on paljon erilaisten ja odottamatonta käyttötavat suorituskyvyn tavoitteet.

| Opetusohjelma  | Kuvaus  |
|---|---|---|
| [Luo joustavasti resurssivarantoon](sql-database-elastic-pool-create-portal.md) | Tässä opetusohjelmassa kerrotaan luominen Azure SQL-tietokantoja skaalattava resurssivarantoon. |
| [Näytön joustavasti tietokanta](sql-database-elastic-pool-manage-portal.md#elastic-database-monitoring) | Tässä opetusohjelmassa kerrotaan seuraamisesta yksittäisiä joustavasti tietokannalle mahdollisia ongelmia. |
| [Ilmoituksen lisääminen resurssivarantoon resurssi](sql-database-elastic-pool-manage-portal.md#add-an-alert-to-a-pool-resource) | Tässä opetusohjelmassa kerrotaan sääntöjen lisääminen resurssit, Lähetä sähköposti edelleen henkilöille tai URL-Osoitteen päätepisteet ilmoitusten merkkijonojen, kun resurssin käynnit käyttö raja-arvon, joka määritetään. |
| [Tietokannan siirtäminen joustavasti resurssivarantoon](sql-database-elastic-pool-manage-portal.md#move-a-database-into-an-elastic-pool) | Tässä opetusohjelmassa kerrotaan, miten tietokannan siirtäminen joustavasti resurssivarantoon. |
| [Tietokannan siirtäminen pois joustavasti resurssivarantoon](sql-database-elastic-pool-manage-portal.md#move-a-database-out-of-an-elastic-pool) | Tässä opetusohjelmassa kerrotaan tietokannan siirtäminen pois joustavasti resurssivarantoon. |
| [Resurssivarantoon suorituskyvyn asetusten muuttaminen](sql-database-elastic-pool-manage-portal.md#change-performance-settings-of-a-pool) | Tässä opetusohjelmassa kerrotaan säätäminen resurssivarantoon suorituskyky ja tallennustilaa rajoitukset. |
||||

## <a name="elastic-database-jobs"></a>Joustavasti tietokannan projektit

Seuraavat Opetusohjelmissa opit käyttämisestä [joustavasti tietokannan projektit](sql-database-elastic-jobs-overview.md).

| Opetusohjelma  | Kuvaus  |
|---|---|---|
| [Joustavasti Tietokantatyökalut käytön aloittaminen](sql-database-elastic-scale-get-started.md) | Tässä opetusohjelmassa voit opetella joustavasti Tietokantatyökalut yksinkertainen sharded-sovelluksen ominaisuuksia. |
| [Azure SQL-tietokanta joustavasti työt käytön aloittaminen](sql-database-elastic-jobs-getting-started.md)  | Tässä opetusohjelmassa kerrotaan, miten voit luoda ja hallita työt, ryhmän liittyvät tietokantojen hallinta.  | 
||||

## <a name="elastic-queries"></a>Joustavasti kyselyt

Seuraavat Opetusohjelmissa opit käynnissä [joustavasti kyselyt](sql-database-elastic-query-overview.md). 

| Opetusohjelma  | Kuvaus  |
|---|---|---|
| [Kyselyt yli vaakasuunnassa osioidun (sharded) tietokannan)](sql-database-elastic-query-getting-started.md) | Tässä opetusohjelmassa kerrotaan raporttien luominen kaikki tietokannat vaakasuunnassa osioitua (sharded) tietokannan [joustavasti kyselyn](sql-database-elastic-query-overview.md) avulla |
| [Kyselyt pystysuunnassa Osioidun tietokannan kautta)](sql-database-elastic-query-getting-started-vertical.md#create-database-objects) | Tässä opetusohjelmassa kerrotaan raporttien luominen kaikki tietokannat pystysuunnassa tietokannan [joustavasti kyselyn](sql-database-elastic-query-overview.md) avulla |
| [Siirtää aiemmin luodun tietokannan asteikko-kohtaa](sql-database-elastic-convert-to-use-elastic-tools.md)| Tässä opetusohjelmassa kerrotaan skaalata vaakasuunnassa (shard) Azure SQL-tietokantaan. |
||||

## <a name="performance-optimization"></a>Suorituskyvyn optimointi

Seuraavat Opetusohjelmissa opit [yhden tietokannan suorituskykyä](sql-database-performance-guidance.md). Lisätietoja useiden tietokantojen suorituskyvyn optimointi [joustavasti jakavat](#elastic-pools).

| Opetusohjelma  | Kuvaus  |
|---|---|---|
| [Tietokannan palvelun taso ja suorituskyvyn tason muuttaminen](sql-database-scale-up.md#change-the-service-tier-and-performance-level-of-your-database) | Tässä opetusohjelmassa kerrotaan skaalata tai skaalata käyttämällä palvelutasot Azure SQL-tietokantaan toimintaa. |
| [SQL tietokanta Advisor kyselyn suorituskykyä tietoja](sql-database-performance.md#performance-overview) | Tässä opetusohjelmassa kerrotaan, avaaminen ja käyttäminen SQL tietokanta Advisor kyselyn suorituskykyä tietoja.|
| [SQL-tietokannan Advisor suorituskyvyn suositukset](sql-database-advisor.md#viewing-recommendations) | Tässä opetusohjelmassa kerrotaan, voit tarkastella ja käyttää SQL-tietokannan Advisor suorituskyvyn suosituksia. |
| [Tarkista yläreunan suorittimen käyttö kyselyissä](sql-database-query-performance.md#review-top-cpu-consuming-queries)| Tässä opetusohjelmassa kerrotaan, miten SQL tietokanta Advisor kyselyn suorituskykyä tietoja avulla voit tarkastella yläreunan suorittimen käyttö kyselyissä.|
| [Yksittäisen kyselyn tietojen tarkasteleminen](sql-database-query-performance.md#viewing-individual-query-details)| Tässä opetusohjelmassa kerrotaan, miten SQL tietokanta Advisor kyselyn suorituskykyä tietoja avulla voit tarkastella yksittäisiä kyselyn suorituskykyä.|
||||

## <a name="sql-database-migration-and-archive"></a>SQL-tietokannan siirtymis- ja arkisto 

Seuraavat Opetusohjelmissa opit [siirtäminen aiemmin luodun SQL Server-tietokannan Azure SQL-tietokantaan](sql-database-cloud-migrate.md).

| Opetusohjelma  | Kuvaus  |
|---|---|---|
| [SQL Server Data Tools for Visual Studio avulla yhteensopivuusongelmat tunnistus](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md#detecting-compatibility-issues-using-sql-server-data-tools-for-visual-studio) | Tässä opetusohjelmassa opit käyttämään SQL Server Data Tools for Visual Studio selvittää Azure SQL-tietokanta yhteensopivuus. |
| [SQL Server Data Tools for Visual Studio avulla yhteensopivuuden ongelmien korjaaminen](sql-database-cloud-migrate-fix-compatibility-issues-ssdt#fixing-compatibility-issues-using-sql-server-data-tools-for-visual-studio) | Tässä opetusohjelmassa kerrotaan, voit käyttää SQL Server Data Tools for Visual Studio Azure SQL-tietokanta yhteensopivuusongelmat korjaamiseksi. |
| [Määrittää SQL-tietokantaan yhteensopivuuden SqlPackage.exe käyttäminen](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md#using-sqlpackageexe) | Tässä opetusohjelmassa voit opetella SQLPackage.exe komentorivin-apuohjelman avulla voit määrittää Azure SQL-tietokanta yhteensopivuus.|
| [Määrittää SQL-tietokantaan yhteensopivuuden SSMS käyttäminen](sql-database-cloud-migrate-determine-compatibility-ssms.md#using-sql-server-management-studio) |Tässä opetusohjelmassa opit käyttämään SQL Server Management Studiossa selvittää Azure SQL-tietokanta yhteensopivuus.|
| [Siirtää SQL Server-tietokannan käyttäminen Microsoft Azure-tietokannan käyttöönotto tietokannan SQL-tietokantaan](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md#use-the-deploy-database-to-microsoft-azure-database-wizard) | Tässä opetusohjelmassa kerrotaan siirtäminen yhteensopiva SQL Server-tietokannan muodostaminen Microsoft Azure-tietokannan käyttöönotto-tietokannan käyttäminen SQL Server Management Studiossa Azure SQL-tietokantaan.
| [Vie BACPAC-tiedoston SSMS SQL Server-tietokantaan](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md) | Tässä opetusohjelmassa kerrotaan vieminen yhteensopiva SQL Server-tietokantaan ohjattua Vie tiedot taso sovelluksen SQL Server Management Studiossa BACPAC tiedostoon.|
| [Vie BACPAC-tiedoston SqlPackage SQL Server-tietokantaan](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md) | Tässä opetusohjelmassa kerrotaan vieminen yhteensopiva SQL Server-tietokantaan BACPAC tiedostoon SQLPackage.exe komentorivin-apuohjelman avulla.|
| [Azure SQL-tietokanta SSMS BACPAC tiedoston tuominen](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md) | Tässä opetusohjelmassa kerrotaan tietokannan tuomisesta Azure SQL-tietokantaan ohjattua Vie tiedot taso sovelluksen SQL Server Management Studiossa BACPAC tiedostosta. |
| [Azure SQL-tietokanta SqlPackage BACPAC tiedoston tuominen](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md#import-from-a-bacpac-file-into-azure-sql-database-using-sqlpackage) | Tässä opetusohjelmassa kerrotaan tietokannan tuomisesta Azure SQL-tietokanta-apuohjelmalla SQLPackage komentorivin BACPAC tiedostosta. |
| [BACPAC muototiedoston tuominen Azure SQL-tietokanta Azure-portaalissa](sql-database-import.md) | Tässä opetusohjelmassa kerrotaan tietokannan tuomisesta Azure SQL-tietokanta BACPAC-tiedostosta, joka on tallennettu Azure-blob Azure-portaalissa.|
| [BACPAC muototiedoston tuominen PowerShellin Azure SQL-tietokanta](sql-database-import-powershell.md) | Tässä opetusohjelmassa kerrotaan tietokannan tuomisesta Azure SQL-tietokanta BACPAC tiedostosta PowerShellin avulla.|
| [Arkisto Azure-portaalissa Azure SQL-tietokantaan](sql-database-export.md#export-your-database) | Tässä opetusohjelmassa kerrotaan arkistoiminen Azure-portaalissa BACPAC tiedostoon Azure SQL-tietokantaan. |
| [Arkistoida PowerShellin Azure SQL-tietokantaan](sql-database-export-powershell.md) | Tässä opetusohjelmassa kerrotaan arkistoiminen BACPAC-tiedoston PowerShellin Azure SQL-tietokantaan. |
| [Kopioi Azure-portaalissa Azure SQL-tietokantaan](sql-database-copy.md#copy-your-sql-database) | Tässä opetusohjelmassa kerrotaan kopioiminen Azure-portaalissa Azure SQL-tietokantaan. |
| [Kopioi PowerShellin Azure SQL-tietokantaan](sql-database-copy-powershell#copy-your-sql-database) | Tässä opetusohjelmassa kerrotaan kopioiminen PowerShellin Azure SQL-tietokantaan. |
| [Kopioi käyttämällä Transact-SQL Azure SQL-tietokantaan](sql-database-copy-transact-sql.md#copy-your-sql-database) | Tässä opetusohjelmassa kerrotaan kopioiminen käyttämällä Transact-SQL Azure SQL-tietokantaan. |
||||

##<a name="develop"></a>Kehittäminen

Seuraavat Opetusohjelmissa opit [SQL-tietokannan kehittäminen](sql-database-develop-overview.md) ja [connectivity kirjastojen](sql-database-libraries.md)avulla.

| Opetusohjelma  | Kuvaus  |
|---|---|---|
| [Yhteyden muodostaminen SQL-tietokantaan käyttämällä .NET (C#)](sql-database-develop-dotnet-simple.md) | Tässä opetusohjelmassa kerrotaan yhteyden muodostamisesta käyttämällä C# Azure SQL-tietokantaan. |
| [Yhteyden muodostaminen SQL-tietokantaan Java avulla](sql-database-develop-java-simple.md) | Tässä opetusohjelmassa kerrotaan yhteyden muodostamisesta käyttämällä Java Azure SQL-tietokantaan. |
| [Yhteyden muodostaminen SQL-tietokantaan Node.js avulla](sql-database-develop-nodejs-simple.md) | Tässä opetusohjelmassa kerrotaan yhteyden muodostamisesta käyttämällä Node.js Azure SQL-tietokantaan. |
| [Yhteyden muodostaminen SQL-tietokantaan PHP avulla](sql-database-develop-php-simple.md) | Tässä opetusohjelmassa kerrotaan yhteyden muodostamisesta käyttämällä PHP Azure SQL-tietokantaan. |
| [Yhteyden muodostaminen SQL-tietokantaan Python avulla](sql-database-develop-python-simple.md) | Tässä opetusohjelmassa kerrotaan yhteyden muodostamisesta käyttämällä Python Azure SQL-tietokantaan. |
| [Yhteyden muodostaminen SQL-tietokantaan Ruby avulla](sql-database-develop-ruby-simple.md) | Tässä opetusohjelmassa kerrotaan yhteyden muodostamisesta käyttämällä Ruby Azure SQL-tietokantaan. |
||||
 
## <a name="database-access"></a>Tietokannan käytön

Seuraavat Opetusohjelmissa opit [Kirjautumiset ja käyttäjien luontiin ja hallintaan](sql-database-manage-logins.md).

| Opetusohjelma  | Kuvaus  |
|---|---|---|
| [Azure-portaalissa Azure SQL-tietokantaan palomuurin palvelintason säännön luominen](sql-database-configure-firewall-settings.md)  | Tässä opetusohjelmassa kerrotaan määrittäminen SQL-tietokantaan palvelintason palomuuri Azure-portaalissa.  |
| [Käyttämällä Transact-SQL-tietokannan tason palomuurisäännön luominen](sql-database-configure-firewall-settings-tsql.md#database-level-firewall-rules) | Tässä opetusohjelmassa kerrotaan luominen käyttämällä Transact-SQL-tietokannan tason palomuurisäännön.|
| [Käyttämällä Transact-SQL server tason Palomuurisääntöjen hallinta](sql-database-configure-firewall-settings-tsql.md#manage-server-level-firewall-rules-through-transact-sql) | Tässä opetusohjelmassa kerrotaan hallinnasta palvelintason Azure SQL-tietokantaan palomuurin Transact-SQL.|
| [Hallitse palvelintason palomuurisäännöt PowerShellin avulla](sql-database-configure-firewall-settings-powershell.md#manage-firewall-rules-using-powershell) | Tässä opetusohjelmassa kerrotaan hallinta PowerShellin avulla Azure SQL-tietokanta palvelintason palomuuri.|
| [REST-Ohjelmointirajapinnan käyttäminen palvelintason Palomuurisääntöjen hallinta](sql-database-configure-firewall-settings-rest.md#manage-firewall-rules-using-the-service-management-rest-api) | Tässä opetusohjelmassa kerrotaan hallinnasta Azure SQL-tietokanta palvelintason palomuurin Palauta-Ohjelmointirajapinnan käyttäminen.|
| [Yhteyden muodostaminen Azure SQL-tietokanta palvelintason pääasiallista kirjautuminen](sql-database-get-started-security.md#connect-to-azure-sql-database-using-a-server-level-principal-login)| Tässä opetusohjelmassa kerrotaan yhdistämisestä Azure SQL-tietokanta palvelintason pääasiallista Kirjaudu sisään.|
| [Kirjautumista tietokannan käyttöoikeuden myöntäminen] (sql-database-manage-logins.md#granting-database-access-to-a-login() | Tässä opetusohjelmassa kerrotaan, miten tietokannan käyttöoikeuden palvelintason kirjautuminen.|
| [Luo uusi tietokanta-käyttäjän jakaman SSMS](sql-database-get-started-security.md#create-new-database-user-using-ssms) | Tässä opetusohjelmassa kerrotaan uuden tietokannan käyttäjän luominen aiemmin luotuun tietokantaan käyttämällä SSMS.|
| [Uuden tietokannan db_owner käyttöoikeuksien myöntäminen](sql-database-get-started-security.md#grant-new-database-user-dbowner-permissions) | Tässä opetusohjelmassa kerrotaan, miten aiemmin luotuun tietokantaan db_owner käyttöoikeuksien myöntäminen.|
| [Yhteyden muodostaminen SQL Azure-tietokannan käyttäjänä](sql-database-get-started-security.md#connect-to-azure-sql-database-as-a-user) | Tässä opetusohjelmassa kerrotaan yhdistämisestä tietokannan tason käyttäjätunnuksella Azure SQL-tietokantaan.|
||||


## <a name="data-security"></a>Tietojen suojaaminen

Seuraavat Opetusohjelmissa opit [Azure SQL-tietokannan tietojen suojaaminen](sql-database-security.md). 

| Opetusohjelma  | Kuvaus  |
|---|---|---|
| [Ota käyttöön uhkien tunnistus tietokannalle Azure-portaalissa](sql-database-threat-detection-get-started.md#set-up-threat-detection-for-your-database) | Tässä opetusohjelmassa kerrotaan määrittämisestä uhkien tunnistus tietokannan Azure-portaalissa.|
| [Suojattu luottamuksellisia tietoja uisng aina salattu](sql-database-always-encrypted-azure-key-vault.md) | Tässä opetusohjelmassa suojaa luottamukselliset tiedot Azure SQL-tietokantaan aina salattu ohjatun toiminnon avulla.|
| [Suojaa senstive tiedot läpinäkyvä salauksen avulla](https://msdn.microsoft.com/library/dn948096.aspx)| Tässä opetusohjelmassa opit käyttämään läpinäkyvä salauksen suojaa senstive tiedot.|
| [Salaa tietosarake](https://msdn.microsoft.com/library/ms179331.aspx)| Tässä opetusohjelmassa kerrotaan salaaminen käyttämällä Transact-SQL-tietoja sisältävä sarake.|
| [Dynaamiset tiedot naamioiminen määrittäminen](sql-database-dynamic-data-masking-get-started.md#set-up-dynamic-data-masking-for-your-database-using-the-azure-portal)  | Tässä opetusohjelmassa kerrotaan määrittämisestä dynaamiset tiedot piilottamisen Azure SQL-tietokantaan. |
||||

## <a name="business-continuity-and-query-scale-out"></a>Liiketoiminnan jatkuvuus ja kyselyn asteikko kautta

Seuraavat Opetusohjelmissa opit käyttäminen [Geo palauttaminen ja aktiivinen Geo-replikoinnin](sql-database-business-continuity.md) reccover virheet-Liiketoiminnan jatkuvuus ja kyselyn asteikko-kohtaa.

| Opetusohjelma  | Kuvaus  |
|---|---|---|
| [Palauttaa Azure SQL-tietokantaan aiempaan ajankohtaan Azure-portaalissa](sql-database-point-in-time-restore-portal.md)| Tässä opetusohjelmassa kerrotaan tietokannan palauttaminen aikaisempaan senhetkinen Azure-portaalissa.|
| [Palauttaa Azure SQL-tietokantaan aiempaan ajankohtaan PowerShellin avulla](sql-database-point-in-time-restore-powershell.md) | Tässä opetusohjelmassa kerrotaan tietokannan palauttaminen aikaisempaan senhetkinen PowerShellin avulla|
| [Palauta poistetut Azure SQL-tietokanta Azure-portaalissa](sql-database-restore-deleted-database-portal.md) | Tässä opetusohjelmassa kerrotaan Azure-portaalissa poistetun tietokannan palauttaminen.|
| [Palauta poistetut Azure SQL-tietokanta PowerShellin avulla](sql-database-restore-deleted-database-powershell.md) | Tässä opetusohjelmassa kerrotaan siitä, miten voit palauttaa poistetun tietokannan PowerShellin avulla.|
| [Geo replikoinnin määrittäminen Azure SQL-tietokanta Azure-portaalissa](sql-database-geo-replication-portal.md)| Tässä opetusohjelmassa kerrotaan määrittäminen aktiivinen Geo-replikoinnin Azure-portaalissa.|
| [Geo replikoinnin määrittäminen PowerShellin Azure SQL-tietokanta](sql-database-geo-replication-powershell.md)| Tässä opetusohjelmassa kerrotaan aktiivinen Geo-replikoinnin määrittämisestä PowerShellin avulla.|
| [Geo replikoinnin määrittäminen käyttämällä Transact-SQL Azure SQL-tietokanta](sql-database-geo-replication-transact-sql.md)| Tässä opetusohjelmassa kerrotaan aktiivinen Geo-replikoinnin määrittämisestä käyttämällä Transact-SQL.|
| [Aloita suunniteltu tai suunnittelematon automaattisesti Azure SQL-tietokannan Azure-portaalissa](sql-database-geo-replication-failover-portal.md) | Tässä opetusohjelmassa kerrotaan, kuinka voit siirtää geo replikoida toissijainen replikan Azure-portaalissa.|
| [Aloita suunniteltu tai suunnittelematon automaattisesti Azure SQL-tietokannan PowerShellin avulla](sql-database-geo-replication-failover-powershell.md) | Tässä opetusohjelmassa kerrotaan, kuinka voit siirtää geo replikoida toissijainen replikan PowerShellin avulla.|
| [Aloita suunniteltu tai suunnittelematon automaattisesti, käyttämällä Transact-SQL Azure SQL-tietokanta](sql-database-geo-replication-failover-transact-sql.md) | Tässä opetusohjelmassa kerrotaan, kuinka voit siirtää geo replikoida toissijainen replikan käyttämällä Transact-SQL.|
||||

## <a name="data-sync"></a>Tietojen synkronointi

Tässä opetusohjelmassa opit [Tietojen synkronointi](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf).

| Opetusohjelma  | Kuvaus  |
|---|---|---| 
| [Käytön aloittaminen Azure SQL-tietojen synkronointi (ennakkoversio)](sql-database-get-started-sql-data-sync.md)  | Tässä opetusohjelmassa kerrotaan Azure SQL-tietojen synkronointi Azure perinteinen portaalissa perusteet. |
||||

## <a name="next-steps"></a>Seuraavat vaiheet

[Tutustu Azure SQL-tietokannan ratkaisu pika käynnistyy](sql-database-solution-quick-starts.md)
