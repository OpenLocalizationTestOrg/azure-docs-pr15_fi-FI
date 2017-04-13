<properties 
    pageTitle="Aktiivinen Geo-replikoinnin määrittäminen PowerShellin Azure SQL-tietokanta | Microsoft Azure" 
    description="Aktiivinen Geo-replikoinnin määrittäminen PowerShellin Azure SQL-tietokanta" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
   ms.workload="NA"
    ms.date="07/14/2016"
    ms.author="sstein"/>

# <a name="configure-geo-replication-for-azure-sql-database-with-powershell"></a>Geo replikoinnin määrittäminen PowerShellin Azure SQL-tietokanta

> [AZURE.SELECTOR]
- [Yleiskatsaus](sql-database-geo-replication-overview.md)
- [Azure Portal](sql-database-geo-replication-portal.md)
- [PowerShellin](sql-database-geo-replication-powershell.md)
- [T-SQL](sql-database-geo-replication-transact-sql.md)

Tässä artikkelissa kerrotaan aktiivinen Geo-replikoinnin määrittäminen PowerShellin SQL-tietokantaan.

Aloita automaattisesti PowerShellin avulla on artikkelissa [aloittaa suunniteltu tai suunnittelematon automaattisesti Azure SQL-tietokantaan PowerShellin avulla](sql-database-geo-replication-failover-powershell.md).

>[AZURE.NOTE] Kaikki palvelutasot kaikki tietokannat on nyt aktiivinen Geo-replikoinnin (luettavissa secondaries). Huhtikuussa 2017-ei voi lukea toissijainen tyyppi poistua ja ei voi lukea aiemmin luotujen tietokantojen päivitetään automaattisesti luettavissa secondaries.



Määritä aktiivinen Geo-replikoinnin PowerShellin avulla, sinun on seuraava:

- Azure tilaus. 
- Azure SQL-tietokantaan - ensisijainen tietokanta, jonka haluat kopioida.
- Azure PowerShell 1.0 tai uudempi. Voit ladata ja asentaa seuraavat [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md)PowerShellin Azure-moduuleja.


## <a name="configure-your-credentials-and-select-your-subscription"></a>Määritä tunnistetiedot ja valitse tilauksen

Ensin sinun on muodostettava access Azure-tili, joten PowerShellin aloitus- ja suorittamalla sitten seuraavan cmdlet-komennon. Kirjoita Kirjaudu sisään näytön saman sähköpostiosoitteella ja salasanalla, jonka avulla Kirjaudu Azure-portaaliin.


    Login-AzureRmAccount

Kun olet kirjautunut onnistuneesti näet tietoja näytöstä, joka sisältää tunnus, olet kirjautunut sisään ja Azure tilaukset, joihin sinulla on pääsy.


### <a name="select-your-azure-subscription"></a>Valitse Azure tilauksen

Valitse tilaus, sinun on tilauksen tunnus. Voit kopioida tilauksen tunnus näyttää edellisessä vaiheessa tiedot tai jos käytössäsi on useita tilauksia ja on enemmän tietoja voit suorittaa **Get-AzureRmSubscription** cmdlet-komento ja kopioi haluamasi Tilaustiedot tulosjoukon. Seuraavan cmdlet-komennon käyttämällä tilauksen tunnus määrittäminen voimassa oleva tilaus:

    Select-AzureRmSubscription -SubscriptionId 4cac86b0-1e56-bbbb-aaaa-000000000000

**Valitse AzureRmSubscription** onnistuneesti suorittamisen jälkeen näkyviin tulee PowerShell-kohta.


## <a name="add-secondary-database"></a>Lisää toissijainen tietokanta


Seuraavat vaiheet Luo uuden toissijaisen tietokannan Geo replikoinnin partnership.  
  
Jos haluat ottaa käyttöön toissijainen on oltava tilauksen omistaja tai rinnakkaisomistajan. 

**Uusi AzureRmSqlDatabaseSecondary** cmdlet-komennon avulla voit lisätä toissijaisen tietokanta kumppanin palvelimessa yhteys paikalliseen tietokantaan palvelimessa, johon olet yhteydessä (ensisijainen tietokanta). 

Tämä cmdlet-komento korvaa **Käynnistä AzureSqlDatabaseCopy** **– IsContinuous** -parametrin.  Se siirtää **AzureRmSqlDatabaseSecondary** objektia, jota voidaan käyttää muita cmdlet-komennot on helpompi tunnistaa tietyn replikoinnin linkki. Tämä cmdlet-komento palauttaa kun toissijaisen tietokanta on luotu ja täysin kylvetään. Tietokannan koon mukaan voi kestää minuuteista tuntia.

Replikoitua tietokantaa toissijainen palvelimessa on sama nimi kuin tietokannan ensisijainen palvelimessa ja on oletusarvoisesti palvelun samalla tasolla. Toissijaisen tietokanta voidaan lukea tai ei voi lukea ja voi olla yhden tietokannan tai joustavasti tietokannan. Lisätietoja on artikkelissa [Uusi AzureRMSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt603689.aspx) ja [Palvelutasot](sql-database-service-tiers.md).
Kun toissijaisen on luotu ja kylvetään, tietojen alkaa ensisijainen tietokannan replikoiminen toissijainen uuteen tietokantaan. Seuraavia ohjeita kerrotaan, kuinka voit tehdä tämän PowerShellin avulla ei voi lukea ja helppotajuisesti secondaries yhteen tietokantaan tai joustavasti tietokannan luomiseen.

Jos kumppanin tietokanta on jo (esimerkiksi – tuloksena lopetetaan edellisen Geo replikoinnin yhteyden) komento epäonnistuu.



### <a name="add-a-non-readable-secondary-single-database"></a>Lisää ei voi lukea toissijainen (yksi-tietokanta)

Seuraava komento luo tietokannan "mydb" palvelin "srv2" resurssin ryhmän "rg2" ei voi lukea toissijainen:

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" -AllowConnections "No"



### <a name="add-readable-secondary-single-database"></a>Lisää luettavissa toissijainen (yksi-tietokanta)

Seuraava komento luo luettavissa toissijainen tietokannan "mydb" palvelin "srv2" resurssin ryhmän "rg2":

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" -AllowConnections "All"




### <a name="add-a-non-readable-secondary-elastic-database"></a>Lisää ei voi lukea toissijainen (joustavasti tietokanta)

Seuraava komento luo tietokannan "mydb" ei voi lukea toissijainen joustavasti tietokannan ryhmän nimeltä "ElasticPool1" palvelin "srv2" resurssin ryhmän "rg2":

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" –SecondaryElasticPoolName "ElasticPool1" -AllowConnections "No"


### <a name="add-a-readable-secondary-elastic-database"></a>Lisää luettavissa toissijainen (joustavasti tietokanta)

Seuraava komento luo luettavissa toissijainen tietokannan "mydb" joustavasti tietokannan ryhmän nimeltä "ElasticPool1" palvelin "srv2" resurssin ryhmän "rg2":

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" –SecondaryElasticPoolName "ElasticPool1" -AllowConnections "All"





## <a name="remove-secondary-database"></a>Poista toissijaisen tietokanta

**Poista AzureRmSqlDatabaseSecondary** cmdlet-komennon avulla voit lopettaa pysyvästi sitten replikoinnin suhde toissijaisen tietokanta ja sen ensisijainen välillä. Yhteys-tilauksen jälkeen toissijaisen tietokanta on luku-ja kirjoitusoikeudet tietokannan. Jos yhteys toissijainen tietokantaan katkeaa komento onnistuu, mutta toissijaisen tulee luku-ja kirjoitusoikeudet, kun yhteys on palautettu. Lisätietoja on artikkelissa [Poista AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt603457.aspx) ja [Palvelutasot](sql-database-service-tiers.md).

Tämä cmdlet-komento korvaa Stop AzureSqlDatabaseCopy replikoinnin. 

Pakotetun päättymispäivämäärä, joka poistaa replikoinnin linkin ja jättää entisen toissijaisen erillinen tietokantaan, joka ei täysin replikoida päättäminen ennen kuin vastaa poistaminen. Kaikki Linkitä tiedot poistetaan entisen ensisijainen ja toissijainen, entinen Jos tai kun ne ovat käytettävissä. Tämä cmdlet-komento palauttaa kun replikoinnin linkki poistetaan. 


Jos haluat poistaa toissijaisia, käyttäjien pitäisi olla kirjoitusoikeudet ensisijainen ja toissijainen tietokantoihin RBAC mukaan. Katso lisätietoja Roolipohjainen käytönhallinta.

Seuraavat poistaa replikoinnin linkin palvelin "srv2" resurssin ryhmän "rg2", "mydb"-tietokannasta. 

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | Get-AzureRmSqlDatabaseReplicationLink –SecondaryResourceGroup "rg2" –PartnerServerName "srv2"
    $secondaryLink | Remove-AzureRmSqlDatabaseSecondary 


## <a name="monitor-geo-replication-configuration-and-health"></a>Näytön geo replikoinnin määritys ja kunto

Seurannan tehtäviin kuuluu seuranta geo replikoinnin-määritys ja tietojen replikoinnin kunnon valvonta.  

[Hae AzureRmSqlDatabaseReplicationLink](https://msdn.microsoft.com/library/mt619330.aspx) voidaan sys.geo_replication_links luettelon-näkymässä näkyvät eteenpäin replikoinnin linkkejä koskevien tietojen hakemiseen.

Seuraava komento hakee replikoinnin linkin ensisijainen tietokannan "mydb" ja toissijaisen palvelimen "srv2" resurssin ryhmän "rg2", valitse tilaa.

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | Get-AzureRmSqlDatabaseReplicationLink –PartnerResourceGroup "rg2” –PartnerServerName "srv2”


## <a name="next-steps"></a>Seuraavat vaiheet

- Tietoja Active Geo-replikoinnin, artikkelissa - [Aktiivinen Geo-replikointi](sql-database-geo-replication-overview.md)
- Liiketoiminnan jatkuvuus-yleiskatsaus ja tilanteita, joissa on artikkelissa [liiketoiminnan jatkuvuuden yhteenveto](sql-database-business-continuity.md)

