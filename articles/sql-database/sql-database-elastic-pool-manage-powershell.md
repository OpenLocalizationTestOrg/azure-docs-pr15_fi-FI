<properties 
    pageTitle="Hallitse joustavasti tietokanta-ryhmän (PowerShell) | Microsoft Azure" 
    description="Opettele joustavasti tietokanta-ryhmän hallinta PowerShellin avulla."  
    services="sql-database" 
    documentationCenter="" 
    authors="srinia" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management" 
    ms.date="06/22/2016"
    ms.author="srinia"/>

# <a name="monitor-and-manage-an-elastic-database-pool-with-powershell"></a>Seurata ja hallita joustavasti tietokanta-ryhmän PowerShellin avulla 

> [AZURE.SELECTOR]
- [Azure portal](sql-database-elastic-pool-manage-portal.md)
- [PowerShellin](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL](sql-database-elastic-pool-manage-tsql.md)

Hallitse [joustavasti tietokannan resurssivarantoon](sql-database-elastic-pool.md) käyttämällä PowerShell cmdlet-komennot. 

Katso yleisiä virhekoodit [SQL-virhekoodit SQL-tietokantaan asiakassovellukset: tietokannan yhteysvirhe ja muiden ongelmien](sql-database-develop-error-messages.md).

Jakavat arvot löytyvät [eDTU ja tallennustilaa rajoitukset](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases). 

## <a name="prerequisites"></a>Edellytykset

* Azure PowerShell 1.0 tai uudempi versio. Lisätietoja on artikkelissa [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md).
* Joustavasti tietokannan jakavat ovat käytettävissä vain SQL-tietokannan Vipuventtiili V12 palvelimiin. Jos sinulla on SQL-tietokannan V11 palvelimen [Vipuventtiili V12 päivittäminen ja luoda varannon PowerShellin käyttäminen](sql-database-upgrade-server-portal.md) yhdessä vaiheessa.


## <a name="move-a-database-into-an-elastic-pool"></a>Tietokannan siirtäminen joustavasti resurssivarantoon

Voit siirtää tietokannan tai pois resurssivarantoon [Määrittäminen AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619433.aspx)kanssa. 

    Set-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## <a name="change-performance-settings-of-a-pool"></a>Resurssivarantoon suorituskyvyn asetusten muuttaminen

Suorituskyky kärsii, kun olet muuttanut asetuksia altaan sopimaan kasvu. [Määritä AzureRmSqlElasticPool](https://msdn.microsoft.com/library/azure/mt603511.aspx) -cmdlet-komennolla. -Dtu parametrin arvoksi eDTUs resurssivarantoon kohden. Katso [eDTU ja tallennustilaa rajoitukset](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases) mahdollisista arvoista.  

    Set-AzureRmSqlElasticPool –ResourceGroupName “resourcegroup1” –ServerName “server1” –ElasticPoolName “elasticpool1” –Dtu 1200 –DatabaseDtuMax 100 –DatabaseDtuMin 50 


## <a name="get-the-status-of-pool-operations"></a>Resurssivarannon toimintojen tilan

Resurssivarantoon luominen voi viedä aikaa. Voit seurata tilaa resurssivarantoon-toimintoa, mukaan lukien luominen ja päivitykset, käytä [Get-AzureRmSqlElasticPoolActivity](https://msdn.microsoft.com/library/azure/mt603812.aspx) cmdlet-komento.

    Get-AzureRmSqlElasticPoolActivity –ResourceGroupName “resourcegroup1” –ServerName “server1” –ElasticPoolName “elasticpool1” 


## <a name="get-the-status-of-moving-an-elastic-database-into-and-out-of-a-pool"></a>Tilan siirtäminen joustavasti tietokannan sisään ja ulos resurssivarantoon

Tietokannan siirtäminen voi viedä aikaa. Jäljitä [Hanki AzureRmSqlDatabaseActivity](https://msdn.microsoft.com/library/azure/mt603687.aspx) cmdlet-komennolla Siirrä tila.

    Get-AzureRmSqlDatabaseActivity -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## <a name="get-resource-usage-data-for-a-pool"></a>Resurssien käyttötietojen hankkiminen resurssivarantoon

Arvot, jotka on haettu resurssin resurssivarantoon rajoitus prosentteina:   


| Metrijärjestelmän nimi | Kuvaus |
| :-- | :-- |
| suorittimen\_prosenttia | Keskiarvon laskeminen käyttö prosentteina altaan rajoitus. |
| Fyysinen\_tietojen\_lukea\_prosenttia | Keskimääräinen i/o-käyttö prosentteina altaan rajoitus perusteella. |
| lokitiedoston\_kirjoittaminen\_prosenttia | Keskiarvo kirjoittaa resurssien käyttö prosentteina altaan rajoitus. | 
| DTU\_kulutus\_prosenttia | Keskimääräinen eDTU käyttö prosentteina altaan eDTU rajoitus | 
| tallennustilan\_prosenttia | Keskimääräinen tallennustilan käyttö prosentteina altaan tallennustilarajan. |  
| työntekijöiden\_prosenttia | Suurin samanaikainen työntekijöiden (pyyntöjen) prosentteina altaan rajoitus perusteella. |  
| istunnot\_prosenttia | Samanaikainen istuntojen enimmäismäärä prosentteina altaan rajoitus perusteella. | 
| eDTU_limit | Nykyisen joustavasti varannon DTU asetuksen joustavasti kannan määritetyn ajanjakson aikana. |
| tallennustilan\_raja | Nykyinen joustavasti varannon tallennustilan enimmäismäärä on määrittäminen joustavasti kannan megatavuina määritetyn ajanjakson aikana. |
| eDTU\_käytetään | Keskimääräinen eDTUs käyttämä aikaväliä resurssivarantoon. |
| tallennustilan\_käytetään | Keskimääräinen tavujen tällä aikavälillä resurssivarantoon tallennustilasta |

**Arvot rakeisuuden/säilytys kausien:**

* Tietoja palautetaan 5 minuutin rakeisuuden-palvelussa.  
* Tietojen säilytys on 35 päivää.  

Tämä cmdlet-komennosta ja API rajoittaa rivit, jotka voidaan hakea yhden puhelun 1000 rivit (noin 3 päivän 5 minuutin rakeisuuden tietoja). Tämä komento voidaan kutsua useita kertoja eri Aloita/Lopeta aikavälien noutaa kanssa, mutta 

Voit noutaa arvot:

    $metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/elasticPools/franchisepool -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015")  


## <a name="get-resource-usage-data-for-an-elastic-database"></a>Resurssien käyttötietojen noutaminen joustavasti tietokannan

Nämä API ovat samat kuin nykyinen (Vipuventtiili V12)-ohjelmointirajapinnan käytettävien resurssien käyttöön erillinen tietokannan, paitsi seuraavat semanttinen ero seuranta. 

Tämä API arvot, jotka on haettu ilmaistaan prosentteina kohti max eDTUs (tai vastaava pää pohjana metrijärjestelmän, kuten suorittimen, IO jne) kyseiseen resurssivarantoon määrittäminen. Esimerkiksi 50 % käyttö minkä tahansa nämä arvot osoittaa, että 50 prosenttia on tietyn resurssin kulutus-tietokannan pää raja kyseiselle resurssille ylemmän tason varannon kohden. 

Voit noutaa arvot:

    $metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/databases/myDB -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015") 

## <a name="add-an-alert-to-a-pool-resource"></a>Ilmoituksen lisääminen resurssivarantoon resurssi

Voit lisätä resursseja Lähetä sähköposti-ilmoitusten tai ilmoitusten merkkijonoja [URL-Osoitteen päätepisteet](https://msdn.microsoft.com/library/mt718036.aspx) , kun resurssin käynnit käyttö raja-arvon, voit määrittää ilmoitusten säännöt. Lisää AzureRmMetricAlertRule-cmdlet-komennolla. 

> [AZURE.IMPORTANT]Resurssien käytön valvonta joustavasti jakavat on vähintään 20 minuutin viive. Määrittää ilmoitusasetukset on alle 30 minuuttia joustavasti jakavat ei tällä hetkellä tueta. Minkä tahansa ilmoitusten määrittäminen joustavasti jakavat pisteeseen (parametrin nimeltä "-WindowSize"-PowerShell API) on alle 30 minuuttia saattaa ei voi käynnistää. Varmista, että kaikki ilmoitukset joustavasti jakavat määrittäminen käyttää ajan (puskuri) vähintään 30 minuuttia.

Tässä esimerkissä Lisää ilmoituksen ilmoituksen saaminen, kun resurssivarantoon eDTU kulutus menee tiettyjä kynnysarvo.

    # Set up your resource ID configurations
    $subscriptionId = '<Azure subscription id>'      # Azure subscription ID
    $location =  '<location'                         # Azure region
    $resourceGroupName = '<resource group name>'     # Resource Group
    $serverName = '<server name>'                    # server name
    $poolName = '<elastic pool name>'                # pool name 

    #$Target Resource ID
    $ResourceID = '/subscriptions/' + $subscriptionId + '/resourceGroups/' +$resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/elasticpools/' + $poolName

    # Create an email action
    $actionEmail = New-AzureRmAlertRuleEmail -SendToServiceOwners -CustomEmail JohnDoe@contoso.com

    # create a unique rule name
    $alertName = $poolName + "- DTU consumption rule"

    # Create an alert rule for DTU_consumption_percent
    Add-AzureRMMetricAlertRule -Name $alertName -Location $location -ResourceGroup $resourceGroupName -TargetResourceId $ResourceID -MetricName "DTU_consumption_percent"  -Operator GreaterThan -Threshold 80 -TimeAggregationOperator Average -WindowSize 00:60:00 -Actions $actionEmail 

## <a name="add-alerts-to-all-databases-in-a-pool"></a>Ilmoitusten lisääminen resurssivarantoon kaikki tietokannat

Voit lisätä kaikki tietokannan joustavasti sarjassa sähköposti-ilmoitukset lähetetään ilmoitus säännöt tai ilmoitusten merkkijonoja, kun resurssin [URL-Osoitteen päätepisteet](https://msdn.microsoft.com/library/mt718036.aspx) käynnit ilmoituksen ja määritä käyttö raja-arvon. 

> [AZURE.IMPORTANT] Resurssien käytön valvonta joustavasti jakavat on vähintään 20 minuutin viive. Määrittää ilmoitusasetukset on alle 30 minuuttia joustavasti jakavat ei tällä hetkellä tueta. Minkä tahansa ilmoitusten määrittäminen joustavasti jakavat pisteeseen (parametrin nimeltä "-WindowSize"-PowerShell API) on alle 30 minuuttia saattaa ei voi käynnistää. Varmista, että kaikki ilmoitukset joustavasti jakavat määrittäminen käyttää ajan (puskuri) vähintään 30 minuuttia.

Tässä esimerkissä Lisää ilmoituksen kussakin sarjassa, saat ilmoituksen saaminen, kun kyseisen tietokannan DTU kulutus menee tiettyjä kynnysarvo tietokannat.

    # Set up your resource ID configurations
    $subscriptionId = '<Azure subscription id>'      # Azure subscription ID
    $location = '<location'                          # Azure region
    $resourceGroupName = '<resource group name>'     # Resource Group
    $serverName = '<server name>'                    # server name
    $poolName = '<elastic pool name>'                # pool name 

    # Get the list of databases in this pool.
    $dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName

    # Create an email action
    $actionEmail = New-AzureRmAlertRuleEmail -SendToServiceOwners -CustomEmail JohnDoe@contoso.com

    # Get resource usage metrics for a database in an elastic database for the specified time interval.
    foreach ($db in $dbList)
    {
    $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName

    # create a unique rule name
    $alertName = $db.DatabaseName + "- DTU consumption rule"

    # Create an alert rule for DTU_consumption_percent
    Add-AzureRMMetricAlertRule -Name $alertName  -Location $location -ResourceGroup $resourceGroupName -TargetResourceId $dbResourceId -MetricName "dtu_consumption_percent"  -Operator GreaterThan -Threshold 80 -TimeAggregationOperator Average -WindowSize 00:60:00 -Actions $actionEmail

    # drop the alert rule
    #Remove-AzureRmAlertRule -ResourceGroup $resourceGroupName -Name $alertName
    } 



## <a name="collect-and-monitor-resource-usage-data-across-multiple-pools-in-a-subscription"></a>Kerää ja seurata käyttö resurssitietojen yli useita jakavat tilauksessa

Kun sinulla on runsaasti tietokantojen tilauksen, on hankalaa seurannassa kunkin joustavasti resurssivarantoon erikseen. Sen sijaan SQL tietokanta PowerShellin cmdlet-komennot ja T-SQL-kyselyjä voidaan yhdistää useita jakavat ja seurantaa ja resurssien käyttö analyysi tietokantansa resurssien käyttötietojen keräämiseen. [Esimerkki käyttöönoton](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) joukon, powershell-komentosarjojen löytyy GitHub SQL Server näytteiden säilöön, sekä ohjeet kuvaus ja miten sitä käytetään.

Voit käyttää malli-käyttöönoton seuraavasti alla.


1. Lataa [komentosarjojen ja ohjeissa](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools):
2. Muokkaa komentosarjat-ympäristössä. Määritä vähintään yksi palvelimissa, joina joustavasti jakavat isännöidään.
3. Määritä missä kerättyjen arvot ovat tallennetaan telemetriatietojen tietokantaan. 
4. Voit mukauttaa komentosarjan, joka määrittää komentosarjojen suorittamisen kestoa.

Korkean tason komentosarjat toimimalla seuraavasti:

*   Luetteloi kaikki palvelimet annetun Azure tilauksen (tai palvelinten määritetyn luettelo).
*   Suorittaa taustalla työn kullekin palvelimelle. Työn suorittaa silmukassa säännöllisin väliajoin ja kerää kaikki palvelimen jakavat telemetriatietojen tiedot. Sen jälkeen Lataa kerättyjen tietojen määritetyn telemetriatietojen tietokantaan.
*   Luetteloi kunkin ryhmän tietokannan resurssien käyttötietojen keräämiseen tietokantojen luettelo. Sen jälkeen Lataa kerättyjen tietojen telemetriatietojen tietokantaan.

Telemetriatietojen tietokantaan kerättyjen arvot voidaan analysoida tarkkailla joustavasti jakavat ja ei-tietokannat. Komentosarja asentaa myös ennalta määritetty-arvo-funktio (TVF) telemetriatietojen tietokantaan kooste avulla määritetyn ajanjakson mittaukset. Esimerkiksi tulokset TVF avulla voidaan näyttää "ylimmät N joustavasti jakavat suurin eDTU käyttö tietyn ajan ikkunassa kanssa." Voit myös käyttää analyyttisten työkaluja, kuten Excel- tai Power BI kerättyjen tietojen analysointia ja kysely.

## <a name="example-retrieve-resource-consumption-metrics-for-a-pool-and-its-databases"></a>Esimerkki: noutaa resurssin kulutus mittaukset resurssivarantoon ja sen tietokannat

Tässä esimerkissä hakee kulutus mittaukset annetun joustavasti resurssivarantoon ja kaikki sen tietokannat. Kerättyjen tietojen muotoiltu ja kirjoitettu muotoiltu .csv-tiedostoon. Tiedoston voi selata Excelissä.

    $subscriptionId = '<Azure subscription id>'       # Azure subscription ID
    $resourceGroupName = '<resource group name>'             # Resource Group
    $serverName = <server name>                              # server name
    $poolName = <elastic pool name>                          # pool name
        
    # Login to Azure account and select the subscription.
    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId
    
    # Get resource usage metrics for an elastic pool for the specified time interval.
    $startTime = '4/27/2016 00:00:00'  # start time in UTC
    $endTime = '4/27/2016 01:00:00'    # end time in UTC
    
    # Construct the pool resource ID and retrive pool metrics at 5 minute granularity.
    $poolResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/elasticPools/' + $poolName
    $poolMetrics = (Get-AzureRmMetric -ResourceId $poolResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime) 
    
    # Get the list of databases in this pool.
    $dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName
    
    # Get resource usage metrics for a database in an elastic database for the specified time interval.
    $dbMetrics = @()
    foreach ($db in $dbList)
    {
        $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName
        $dbMetrics = $dbMetrics + (Get-AzureRmMetric -ResourceId $dbResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime)
    }
    
    #Optionally you can format the metrics and output as .csv file using the following script block.
    $command = {
    param($metricList, $outputFile)

    # Format metrics into a table.
    $table = @()
    foreach($metric in $metricList) { 
      foreach($metricValue in $metric.MetricValues) {
        $sx = New-Object PSObject -Property @{
            Timestamp = $metricValue.Timestamp.ToString()
            MetricName = $metric.Name; 
            Average = $metricValue.Average;
            ResourceID = $metric.ResourceId 
          }
          $table = $table += $sx
      }
    }
    
    # Output the metrics into a .csv file.
    write-output $table | Export-csv -Path $outputFile -Append -NoTypeInformation
    }
    
    # Format and output pool metrics
    Invoke-Command -ScriptBlock $command -ArgumentList $poolMetrics,c:\temp\poolmetrics.csv
    
    # Format and output database metrics
    Invoke-Command -ScriptBlock $command -ArgumentList $dbMetrics,c:\temp\dbmetrics.csv



## <a name="latency-of-elastic-pool-operations"></a>Viive joustavasti resurssivarantoon toiminnot

- Muuttaminen kohti tietokannan tai max eDTUs tietokantaa kohden min-eDTUs yleensä suorittaa 5 minuuttia tai vähemmän.
- Muuttaminen kohti resurssivarantoon eDTUs määräytyy altaan kaikki tietokannat käyttämän tilan kokonaismäärä. Muutokset keskimääräinen 90 minuuttia enintään 100 gigatavua. Esimerkiksi jos tilaa yhteensä käyttämä altaan kaikki tietokannat on 200 gt, muuttamiseen kohti resurssivarantoon resurssivarantoon-eDTU odotettu viive on 3 tuntia tai vähemmän.

## <a name="migrate-from-v11-to-v12-servers"></a>Siirtää Vipuventtiili V12 palvelinten V11

PowerShellin cmdlet-komennot ovat käytettävissä aloittaa, lopettaa tai valvoa päivityksen Azure SQL-tietokannan Vipuventtiili V12 V11 tai muita pre-Vipuventtiili V12-versiosta.

- [SQL-tietokannan Vipuventtiili V12 päivityksen PowerShellin avulla](sql-database-upgrade-server-powershell.md)

Oppaat tietoja näiden PowerShell-cmdlet-kohdassa:


- [Hae AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603582.aspx)
- [Käynnistä AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt619403.aspx)
- [Lopeta AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603589.aspx)


Lopeta-cmdlet-komento tarkoittaa peruuttaa, ei Keskeytä. Tällä ei voi palata päivityksen kuin alusta alkaen uudelleen. Lopeta-cmdlet-komento tyhjentää ja vapauttaa kaikki tarvittavat resurssit.

## <a name="next-steps"></a>Seuraavat vaiheet

- [Luo joustavasti töitä](sql-database-elastic-jobs-overview.md) Joustavasti työt avulla voit suorittaa T-SQL-komentosarjat altaan tietokantojen määrää.
- Katso [Skaalaus ulos Azure SQL-tietokantaan](sql-database-elastic-scale-introduction.md): joustavasti Tietokantatyökalut avulla asteikko-kohtaa, Siirrä tiedot, kyselyn tai luoda tapahtumia.
