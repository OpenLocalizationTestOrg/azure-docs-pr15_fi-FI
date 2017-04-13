<properties
    pageTitle="Päivityksen Azure SQL-tietokannan Vipuventtiili V12 PowerShellin avulla | Microsoft Azure"
    description="Kerrotaan, miten voit päivittää Azure SQL-tietokannan Vipuventtiili V12 mukaan lukien verkko- ja Business tietokannan päivittäminen ja sen tietokantojen siirtyminen suoraan PowerShellin joustavasti tietokanta-ryhmän V11 palvelimen päivittäminen."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="sstein"/>

# <a name="upgrade-to-azure-sql-database-v12-using-powershell"></a>Päivityksen Azure SQL-tietokannan Vipuventtiili V12 PowerShellin avulla


> [AZURE.SELECTOR]
- [Azure portal](sql-database-upgrade-server-portal.md)
- [PowerShellin](sql-database-upgrade-server-powershell.md)


SQL-tietokannan Vipuventtiili V12 on uusin versio, joten SQL-tietokannan Vipuventtiili V12 päivittämisestä on suositeltavaa.
SQL-tietokannan Vipuventtiili V12 on monia [etuja verrattuna edellisen version](sql-database-v12-whats-new.md) mukaan lukien:

- Parannettu yhteensopivuus SQL Serverin kanssa.
- Parannettu premium suorituskyky ja uusia suorituskyvyn tasoja.
- [Joustavasti tietokannan jakavat](sql-database-elastic-pool.md).

Tässä artikkelissa on ohjeita päivittämiseen aiemmin SQL-tietokannan V11 palvelimia ja tietokantoja SQL-tietokannan Vipuventtiili V12.

Aikana Vipuventtiili V12 päivittämiseen päivität Internetin kautta tai Business tietokantoja uusi palvelutaso käyttöohjeet päivittäminen Internetin kautta tai Business tietokantoja, joiden mukaan.

Lisäksi [joustavasti tietokannan resurssivarantoon](sql-database-elastic-pool.md) siirtämisestä voi kustannusten tehokkaampaa päivittäminen yksittäisen tietokantojen yksittäisen suorituskyvyn tasot (hinnoittelu tasoa). Jakavat yksinkertaistaa myös tietokannan hallinta, koska haluat ryhmän sen sijaan, että yksittäiset tietokannat suorituskyky-käyttöoikeustasojen hallinta erikseen suorituskyky-asetusten hallinta. Jos sinulla on useita palvelimissa tietokantoja, harkitse siirtämisen samaan palvelimeen ja ottaen hyödyntää laskemisesta resurssivarantoon.

Voit noudattaa siirrettävä tietokantojen helposti suoraan joustavasti tietokannan jakavat V11 palvelinten tämän artikkelin ohjeita.

Huomaa, että tietokantoja se ja toimivat päivitettäessä koko. Uusi suorituskyvyn taso tilapäinen pudottaminen tietokantaan yhteyksien todellinen siirtymä milloin voi esiintyä erittäin pienen ajan, joka on yleensä 90 sekuntia, mutta voi olla jopa 5 minuuttia. Jos sovellus on [lyhytkestoisia vika käsittelyn yhteyden päät](sql-database-connectivity-issues.md) on riittävä suorittaa yhteydet katkeavat päivitys lopussa.

SQL-tietokannan Vipuventtiili V12 päivittäminen ei voi kumota. Päivityksen jälkeen palvelin ei voi palauttaa V11.

Päivityksen Vipuventtiili V12 jälkeen [palvelun taso suosituksia](sql-database-service-tier-advisor.md) ja [joustavasti resurssivarantoon suositukset](sql-database-elastic-pool-create-portal.md) eivät heti ole käytettävissä, kunnes palvelulla aikaa oman työmääriä uudessa palvelimessa. V11 palvelimen suositus historia ei koske Vipuventtiili V12 palvelimet, niin se ei säily.  

## <a name="prepare-to-upgrade"></a>Päivityksen valmisteleminen

- **Päivitä kaikki Internetin kautta tai Business tietokannat**: Käytä portaalin tai [PowerShell päivittäminen tietokantojen ja palvelimen](sql-database-upgrade-server-powershell.md).
- **Tarkista ja Pysäytä Geo replikoinnin**: Jos Azure SQL-tietokanta on määritetty Geo replikoinnin olisi asiakirjan sen nykyiset määritykset ja [Lopeta Geo replikoinnin](sql-database-geo-replication-portal.md#remove-secondary-database). Kun päivitys on valmis uudelleen tietokannan Geo replikoinnin.
- **Avaa seuraavat portit, jos sinulla on Azure-AM-asiakkaat**: Jos asiakasohjelman muodostaa yhteyden SQL-tietokannan Vipuventtiili V12 samalla, kun asiakkaan suoritetaan Azure virtuaalikoneen (AM), sinun on avattava porttialueiden 11000 11999 ja 14000-14999 käyttöön AM. Lisätietoja on artikkelissa [SQL-tietokannan Vipuventtiili V12 portit](sql-database-develop-direct-route-ports-adonet-v12.md).


## <a name="prerequisites"></a>Edellytykset

Haluat päivittämiseksi Vipuventtiili V12 PowerShellin palvelimeen on asennettu ja käytössä uusin Azure-PowerShell. Lisätietoja on artikkelissa [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md).


## <a name="configure-your-credentials-and-select-your-subscription"></a>Määritä tunnistetiedot ja valitse tilauksen

Suorita PowerShell cmdlet-komennot vastaan Azure tilauksen sinun on muodostettava access Azure-tiliisi. Suorita seuraava ja voit valita jommankumman tunnistetietosi ja kirjaudu sisään-näyttöön kanssa. Käytä samaa sähköpostiosoitteella ja salasanalla, jonka avulla Kirjaudu Azure-portaaliin.

    Add-AzureRmAccount

Kun olet kirjautunut onnistuneesti näkyy tietoja näytöstä, joka sisältää tunnus olet kirjautunut sisään ja Azure tilaukset, joihin sinulla on pääsy.

Jos haluat työskentelemään tilaus on tilauksen tunnus (**– SubscriptionId**) tai tilauksen nimeä (**-SubscriptionName**). Voit kopioida sen edellisessä vaiheessa tai jos sinulla on useita tilauksia suorittamalla **Get-AzureRmSubscription** cmdlet-komento ja kopioi haluamasi Tilaustiedot tulosjoukon.

Suorita seuraavat cmdlet-komennolla voit määrittää nykyisen tilauksesi tilauksen tiedot:

    Set-AzureRmContext -SubscriptionId 4cac86b0-1e56-bbbb-aaaa-000000000000

Seuraavat komennot suoritetaan vastaan tilauksen vain valitun yläpuolella.

## <a name="get-recommendations"></a>Nouda suositukset

Saat palvelimen suositus päivityksen ajamalla seuraavan cmdlet-komennon:

    $hint = Get-AzureRmSqlServerUpgradeHint -ResourceGroupName “resourcegroup1” -ServerName “server1”

Lisätietoja on artikkelissa [joustavasti tietokannan resurssivarannon luominen](sql-database-elastic-pool-create-portal.md) ja [Azure SQL-tietokanta hinnat taso suosituksia](sql-database-service-tier-advisor.md).



## <a name="start-the-upgrade"></a>Päivityksen käynnistäminen

Suorita seuraavat cmdlet-komento palvelimen päivityksen käynnistäminen

    Start-AzureRmSqlServerUpgrade -ResourceGroupName “resourcegroup1” -ServerName “server1” -ServerVersion 12.0 -DatabaseCollection $hint.Databases -ElasticPoolCollection $hint.ElasticPools  


Kun suoritat tämän komennon päivitysprosessin alkaa. Voit mukauttaa suositus tulosteen ja anna cmdlet muokatun suositus.


## <a name="upgrade-a-server"></a>Palvelimen päivittäminen


    # Adding the account
    #
    Add-AzureRmAccount

    # Setting the variables
    #
    $SubscriptionName = 'YOUR_SUBSCRIPTION'
    $ResourceGroupName = 'YOUR_RESOURCE_GROUP'
    $ServerName = 'YOUR_SERVER'

    # Selecting the right subscription
    #
    Set-AzureRmContext -SubscriptionName $SubscriptionName

    # Getting the upgrade recommendations
    #
    $hint = Get-AzureRmSqlServerUpgradeHint -ResourceGroupName $ResourceGroupName -ServerName $ServerName

    # Starting the upgrade process
    #
    Start-AzureRmSqlServerUpgrade -ResourceGroupName $ResourceGroupName -ServerName $ServerName -ServerVersion 12.0 -DatabaseCollection $hint.Databases -ElasticPoolCollection $hint.ElasticPools  


## <a name="custom-upgrade-mapping"></a>Mukautettu päivittäminen yhdistäminen

Jos suositukset eivät sovellu server ja kokonaisuuden, sitten voit valita miten tietokantoja päivitetään ja yhdistää ne yhteen tai joustavasti tietokantoihin.

ElasticPoolCollection ja DatabaseCollection parametrit ovat valinnaisia:

    # Creating elastic pool mapping
    #
    $elasticPool = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeRecommendedElasticPoolProperties
    $elasticPool.DatabaseDtuMax = 100
    $elasticPool.DatabaseDtuMin = 0
    $elasticPool.Dtu = 800
    $elasticPool.Edition = "Standard"
    $elasticPool.DatabaseCollection = ("DB1", “DB2”, “DB3”, “DB4”)
    $elasticPool.Name = "elasticpool_1"
    $elasticPool.StorageMb = 800

    # Creating single database mapping for 2 databases. DBMain1 mapped to S0 and DBMain2 mapped to S2
    #
    $databaseMap1 = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeDatabaseProperties
    $databaseMap1.Name = "DBMain1"
    $databaseMap1.TargetEdition = "Standard"
    $databaseMap1.TargetServiceLevelObjective = "S0"

    $databaseMap2 = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeDatabaseProperties
    $databaseMap2.Name = "DBMain2"
    $databaseMap2.TargetEdition = "Standard"
    $databaseMap2.TargetServiceLevelObjective = "S2"

    # Starting the upgrade
    #
    Start-AzureRmSqlServerUpgrade –ResourceGroupName resourcegroup1 –ServerName server1 -ServerVersion 12.0 -DatabaseCollection @($databaseMap1, $databaseMap2) -ElasticPoolCollection @($elasticPool)



## <a name="monitor-databases-after-upgrading-to-sql-database-v12"></a>Näytön tietokantojen SQL-tietokannan Vipuventtiili V12 päivittämisen jälkeen


Päivityksen jälkeen on suositeltavaa seurannassa tietokantaan aktiivisesti sovellusten käytössä haluamasi suorituskykyä ja optimoi käyttö tarpeen mukaan.

Lisäksi voit valvoa joustavasti tietokanta jakavat [portaalissa](sql-database-elastic-pool-manage-portal.md) yksittäiset tietokannat seurantaa tai [PowerShellin](sql-database-elastic-pool-manage-powershell.md) avulla


**Kulutus resurssitietojen:** Basic, Vakio ja Premium tietokantojen kulutus resurssitietojen on käytettävissä [sys.dm_ db_ resource_stats](http://msdn.microsoft.com/library/azure/dn800981.aspx) DMV käyttäjän tietokannan kautta. Tämä DMV on lähellä reaaliaikainen kulutus resurssitietoa osoitteessa 15 toisen rakeisuuden edellisen tunnin toiminnon. Aikavälin DTU prosentti-kulutus lasketaan enimmäisprosenttiarvon kulutus suorittimen, IO ja kirjaudu dimensioista. Näin kyselyn keskimääräinen DTU prosentti kulutus päälle edellisen tunnin laskemiseen:

    SELECT end_time
         , (SELECT Max(v)
             FROM (VALUES (avg_cpu_percent)
                         , (avg_data_io_percent)
                         , (avg_log_write_percent)
           ) AS value(v)) AS [avg_DTU_percent]
    FROM sys.dm_db_resource_stats
    ORDER BY end_time DESC;

Seurannan lisätiedot:

- [Yksittäisen tietokantojen azure SQL-tietokanta suorituskyvyn ohjeita](http://msdn.microsoft.com/library/azure/dn369873.aspx).
- [Hinnan ja suorituskyvyn Huomioitavaa joustavasti tietokannan resurssivarantoon](sql-database-elastic-pool-guidance.md).
- [Seurannan Azure SQL-tietokanta dynaaminen hallinta-näkymien käyttäminen](sql-database-monitoring-with-dmvs.md)



**Ilmoitukset:** Määrittää 'Ilmoitukset' Azure-portaalissa ilmoittaa, kun DTU kulutus päivitetyn tietokannan lähestyy tiettyjä korkean tason. Tietokannan ilmoitukset voidaan määrittää eri suorituskyvyn mittarit, kuten DTU, Suoritin tai IO Log Azure portaali. Tietokannan selaamalla ja valitse **ilmoitusten säännöt** **asetukset** -sivu.

Voit esimerkiksi määrittäminen sähköposti-ilmoituksen "DTU prosentteina", jos DTU prosentti keskiarvo ylittää 75 % viimeisen 5 minuuttia päälle. Lisätietoja ilmoitusten määrittämisestä [ilmoitusten](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) vastaanottaminen viittaavat.



## <a name="next-steps"></a>Seuraavat vaiheet

- [Luo joustavasti tietokannan resurssivarantoon](sql-database-elastic-pool-create-portal.md) ja lisää ryhmään jotkin tai kaikki tietokannat.
- [Muuttaa tietokannan palvelun taso ja suorituskykyä](sql-database-scale-up.md).



## <a name="related-information"></a>Muita aiheeseen liittyviä tietoja

- [Hae AzureRmSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603582.aspx)
- [Käynnistä AzureRmSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt619403.aspx)
- [Lopeta AzureRmSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603589.aspx)
