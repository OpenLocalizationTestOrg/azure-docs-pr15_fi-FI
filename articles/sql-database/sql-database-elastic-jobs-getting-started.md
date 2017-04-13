<properties
    pageTitle="Töiden joustavasti tietokannan käytön aloittaminen"
    description="Opi käyttämään joustavasti tietokannan projektit"
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="ddove"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="ddove" />

# <a name="getting-started-with-elastic-database-jobs"></a>Töiden joustavasti tietokannan käytön aloittaminen

Joustavasti tietokannan töiden (ennakkoversio) Azure SQL-tietokannan avulla voit luotettavuutta suorittaa T-SQL-komentosarjoja, jotka ulottuvat useiden tietokantojen automaattisesti uudelleen ja antamisen potentiaalisen valmistuminen oikeudet. Saat lisätietoja joustavasti tietokannan työn ominaisuus [ominaisuuden yhteenveto-sivu](sql-database-elastic-jobs-overview.md).

Tässä ohjeaiheessa laajentaa otosten löytyvät [joustavasti Tietokantatyökalut käytön aloittaminen](sql-database-elastic-scale-get-started.md). Kun olet valmis, se: Katso, miten voit luoda ja hallita töitä, jotka liittyvät tietokantojen ryhmän hallinta. Ei ole pakollinen jotta voit hyödyntää joustavasti työt edut joustavasti asteikko-työkaluilla.

## <a name="prerequisites"></a>Edellytykset

Lataa ja suorita [joustavasti tietokannan Työkalut otoksen käytön aloittaminen](sql-database-elastic-scale-get-started.md).

## <a name="create-a-shard-map-manager-using-the-sample-app"></a>Luo shard kartan hallinnan malli-sovelluksen käyttäminen

Tässä luodaan shard kartan sekä useita shards, perään tietojen syötön kyselyjä shards hallinta. Jos sinulla on jo shards sharded tietojen kanssa, voit ohittaa seuraavat vaiheet ja siirry seuraavaan osioon.

1. Luoda ja suorittaa **joustavasti Tietokantatyökalut käytön aloittaminen** -sovelluksen malli. Noudata ohjeita, kunnes vaiheessa 7-kohdassa [Lataa ja suorita otoksen sovellus](sql-database-elastic-scale-get-started.md#Getting-started-with-elastic-database-tools). Vaihe 7 lopussa tulee näkyviin Seuraava komentokehote:

    ![komentokehote][1]

2.  Kirjoita-komentoikkunassa "1" ja paina **Enter**-näppäintä. Tämä luo shard kartan hallinta ja lisää kaksi shards palvelimeen. Kirjoita "3" ja paina **Enter**-näppäintä. Toista tämä toimenpide neljä kertaa. Tämä lisää oman shards otoksen tietorivit.

3.  [Azure-portaalin](https://portal.azure.com) näyttää kolmen uusille tietokannoille Vipuventtiili v12-palvelimeen:

    ![Visual Studio vahvistus][2]

    Tässä vaiheessa luodaan mukautettu tietokannan kokoelma, joka kuvastaa shard määrityksen kaikki tietokannat. Tämä sallii us luominen ja suorittaminen työn, joka lisää uusi taulukko shards yli.

Seuraavassa on yleensä loisi shard kartta-kohteen **Uusi AzureSqlJobTarget** cmdlet-komennolla. Tietokannan kohde on määritettävä shard kartan manager-tietokanta ja sitten tietyn shard kartan on määritetty kohde. Sen sijaan tarkastellaan Luetteloi kaikki tietokannat-palvelimessa ja Lisää uusi mukautettu sivustokokoelman lukuun ottamatta tietokannasta tietokannat.

##<a name="creates-a-custom-collection-and-adds-all-databases-in-the-server-to-the-custom-collection-target-with-the-exception-of-master"></a>Luo mukautettu sivustokokoelman ja Lisää kaikki tietokannat-palvelimessa Mukautettu sivustokokoelman lukuun ottamatta perustyyli kohteeseen.


    $customCollectionName = "dbs_in_server"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 
    $ResourceGroupName = "ddove_samples"
    $ServerName = "samples"
    $dbsinserver = Get-AzureRMSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName 
    $dbsinserver | %{
    $currentdb = $_.DatabaseName 
    $ErrorActionPreference = "Stop"
    Write-Output ""

    Try
    {
       New-AzureSqlJobTarget -ServerName $ServerName -DatabaseName $currentdb | Write-Output
    }
    Catch 
    {
        $ErrorMessage = $_.Exception.Message
        $ErrorCategory = $_.CategoryInfo.Reason
                
        if ($ErrorCategory -eq 'UniqueConstraintViolatedException')
        {
             Write-Host $currentdb "is already a database target." 
        }

        else
        {
            throw $_
        }
    
    }

    Try
    {
        if ($currentdb -eq "master")
        {
            Write-Host $currentdb "will not be added custom collection target" $CustomCollectionName "."
        }

        else
        {
            Add-AzureSqlJobChildTarget -CustomCollectionName $CustomCollectionName -ServerName $ServerName -DatabaseName $currentdb
            Write-Host $currentdb "was added to" $CustomCollectionName "."
        }

    }
    Catch 
    {
        $ErrorMessage = $_.Exception.Message
        $ErrorCategory = $_.CategoryInfo.Reason

        if ($ErrorCategory -eq 'UniqueConstraintViolatedException')
        {
             Write-Host $currentdb "is already in the custom collection target" $CustomCollectionName"."
        }

        else
        {
            throw $_
        }
    }
    $ErrorActionPreference = "Continue"
}
    
## <a name="create-a-t-sql-script-for-execution-across-databases"></a>T-SQL-komentosarjan suorittaminen luominen eri tietokannat

    $scriptName = "NewTable"
    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'Test')
    BEGIN
        CREATE TABLE Test(
            TestId INT PRIMARY KEY IDENTITY,
            InsertionTime DATETIME2
        );
    END
    GO
    INSERT INTO Test(InsertionTime) VALUES (sysutcdatetime());
    GO"

    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

##<a name="create-the-job-to-execute-a-script-across-the-custom-group-of-databases"></a>Työn komentosarjan suorittaminen yli tietokantojen mukautetun ryhmän luominen

    $jobName = "create on server dbs"
    $scriptName = "NewTable"
    $customCollectionName = "dbs_in_server"
    $credentialName = "ddove66"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job


##<a name="execute-the-job"></a>Suorittaa työ 

Aiemmin luodun työn suorittaminen voidaan käyttää seuraavaa PowerShell-komentosarjaa:

Päivitä seuraavat muuttujan vastaamaan haluamasi projektin nimeä, jonka haluat on suoritettu:

    $jobName = "create on server dbs"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName 
    Write-Output $jobExecution
 
## <a name="retrieve-the-state-of-a-single-job-execution"></a>Noutaa yhden projektin suorittaminen tila

**IncludeChildren** -parametrin saman **Get-AzureSqlJobExecution** cmdlet-komennon avulla voit tarkastella lapsen työn suorituskerran tilan eli kunkin kunkin tietokannalle kohdistettu projektin suorittaminen tietyn tila.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions 

## <a name="view-the-state-across-multiple-job-executions"></a>Useita työn suorituskerran tilan tarkasteleminen

**Get-AzureSqlJobExecution** cmdlet-komento on useita valinnaisten parametrien, jonka avulla voit näyttää useita työn suorituskerran suodatettu annettujen parametrien avulla. Seuraavassa esitellään joitakin mahdollista saada AzureSqlJobExecution käyttötapaa:

Hae kaikki ylimmän tason työtehtävän suorituskerran:

    Get-AzureSqlJobExecution

Hae kaikki ylimmän tason työn suorituskerran passiivinen suorituskerran mukaan lukien:

    Get-AzureSqlJobExecution -IncludeInactive

Hae kaikki lapsen työn suorituskerran annettu työn suorittamisen tunnuksen, mukaan lukien passiivinen suorituskerran:

    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId –IncludeInactive -IncludeChildren

Hae kaikki projektin suorituskerran luotu aikataulun / projektin yhdistelmä, mukaan lukien passiiviset työt:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive

Hae kaikista projekteista kohdistamisen määritetyn shard kartan, mukaan lukien passiiviset työt:

    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId –IncludeInactive

Hae kaikista projekteista kohdistamisen määritetyn mukautetun sivustokokoelman, mukaan lukien passiiviset työt:

    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId –IncludeInactive
 
Projektin tehtävän suorituskerran sisällä tiettyä projektia suorittamisen luettelon noutaminen:

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions 

Projektin tehtävän suorittaminen tietojen hakeminen:

Seuraavaa PowerShell-komentosarjaa avulla voidaan tarkastella projektin tehtävän suorittaminen, joka on erityisen hyödyllinen, kun virheenkorjaus suoritusvirheistä.

    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution

## <a name="retrieve-failures-within-job-task-executions"></a>Projektin tehtävän suorituskerran sisällä virheiden noutaminen

JobTaskExecution objekti sisältää tehtävän sekä viestin ominaisuus elinkaari ominaisuus. Jos projektin tehtävän suorittaminen, epäonnistui, elinkaari-ominaisuuden arvoksi tulee *epäonnistui* ja sanomaominaisuuden asetetaan tuloksena Poikkeusviesti ja pinossa. Jos projektin ei onnistunut, on tärkeää voit tarkastella projektin tehtävistä, joihin ei onnistunut annetun projektille.

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions) 
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }

## <a name="waiting-for-a-job-execution-to-complete"></a>Projektin suorittaminen, viimeistele odotetaan

Projektitehtävän suorittamiseen odottamaan voidaan käyttää seuraavaa PowerShell-komentosarjaa:

    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId 

## <a name="create-a-custom-execution-policy"></a>Mukautetun suorittaminen-käytännön luominen

Joustavasti tietokannan työt tukee luominen mukautetun suorittaminen käytäntöjä, joilla voi suojata käynnistettäessä työt.
  
Käytännöt suorittamisen salliminen tällä hetkellä määrittäminen:

* Nimi: Suorittamisen käytännön tunnus.
* Aikakatkaisun: Kokonaisaika ennen työn peruutetaan joustavasti tietokannan töiden.
* Alkuperäinen väli: Aikaväli odotettava, ennen kuin ensimmäinen uudelleen.
* Suurin väli:-Pää, yritä väliajoin käyttämään.
* Yritä aikavälin Backoff kertoimen: Lasketaan seuraavan aikaväliä kertoimen.  Seuraavan kaavan avulla: (alkuperäinen uudelleen aikaväli) * Math.pow ((aikaväli Backoff kertoimen) (yrityskertojen määrä) - 2). 
* Suurin yritykset: Yritä enimmäismäärä yrittää suorittaa projektissa.

Oletuskäytäntö suorittamisen käyttää seuraavat arvot:

* Nimi: Oletuskäytäntö suorittaminen
* Aikakatkaisun: 1 viikko
* Alkuperäinen väli: 100 millisekuntia
* Suurin väli: puolessa tunnissa
* Yritä aikavälin kertoimen: 2
* Suurin yritykset: 2,147,483,647

Luo haluamasi suorittamisen käytäntö:

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 10
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $executionPolicy

### <a name="update-a-custom-execution-policy"></a>Päivitä mukautetun suorittaminen-käytännön luominen

Päivitä haluamasi suorittamisen käytännön päivittäminen:

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 15
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $updatedExecutionPolicy = Set-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $updatedExecutionPolicy
 
## <a name="cancel-a-job"></a>Työn peruuttaminen

Joustavasti tietokannan työt tukee työt peruutus pyynnöt.  Jos joustavasti tietokannan työt havaitsee parhaillaan suorittaa työn peruutus pyynnön, se yrittää lopettaa työn.

Liittyy joustavasti tietokannan työt voi suorittaa peruutuksen kahdella eri tavalla:

1. Peruuttaminen suorittavien tehtävät: Jos peruutuksen havaitaan, kun tehtävä on käynnissä, peruutus yritetään parhaillaan kuvasuhde tehtävän kuluessa.  Esimerkki: Jos näkyvissä on tällä hetkellä käytössä suoritetaan, kun peruutuksen yritetään pitkään käynnissä kyselyn, yritetään peruuttaa kyselyn ole.
2. Peruuttaa tehtävän uudelleenyritykset: Jos peruutuksen tunnistaman ohjausobjektin säikeen ennen tehtävän käynnistetään suorittamista varten, ohjausobjektin säikeen välttää tehtävä ja määritellä pyynnön, kun peruutettu.

Jos projektin peruutus haetaan ylemmän tason työn, peruutus-pyyntö voi käyttää, ylemmän tason työn ja kaikki sen alatason työt.
 
Lähetä peruutus-pyyntö, **Lopeta AzureSqlJobExecution** -cmdlet-komennolla ja määrittää **JobExecutionId** -parametrin.

    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId

## <a name="delete-a-job-by-name-and-the-jobs-history"></a>Työn nimi ja projektin historian poistaminen

Joustavasti tietokannan työt tukee asynkroninen poistaminen työt. Työn voidaan merkitä poistettavaksi ja järjestelmä poistaa työ- ja kaikki sen Työhistoria, kun kaikki projektin suorituskerran suorittanut työn. Järjestelmä ei peruuttaa aktiivisen työn suorituskerran automaattisesti.  

Sen sijaan Pysäytä AzureSqlJobExecution on ladattava Peruuta aktiivisen työn suorituskerran.

Käynnistettävän työn poistamista, **Poista AzureSqlJob** -cmdlet-komennolla ja määrittää **työ** -parametrin.

    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName
 
## <a name="create-a-custom-database-target"></a>Luo mukautettu tietokannan kohde
Mukautetun tietokannan kohteiden voidaan määrittää joustavasti tietokannan työt, joita voidaan käyttää suoraan suorittamisen tai merkitsemiseen mukautetun tietokannan ryhmän. Koska **joustavasti tietokannan jakavat** eivät ole vielä suoraan tuettuja PowerShell-ohjelmointirajapinnan kautta, voit luoda ainoastaan mukautetun tietokannan kohde- ja mukautetun tietokannan sivustokokoelman kohde, joka kattaa kaikki ryhmän tietokannat.

Määritä seuraavat muuttujat vastaamaan halutun tietokannan tietoja:

    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName 

## <a name="create-a-custom-database-collection-target"></a>Luo mukautettu tietokannan sivustokokoelman kohde
Mukautetun tietokannan sivustokokoelman kohde voidaan määrittää suorittamisen käyttöön useita määritetyn tietokannan kohteiden välillä. Tietokannan ryhmän luomisen jälkeen tietokantojen voi liittää Mukautettu sivustokokoelman kohteeseen.

Määritä seuraavat muuttujat vastaamaan haluamasi mukautettu sivustokokoelman kohde määritykset:

    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 

### <a name="add-databases-to-a-custom-database-collection-target"></a>Lisää tietokanta mukautetun tietokannan sivustokokoelman kohde

Tietokannan kohteiden voidaan liittää ryhmän tietokantojen luominen mukautetun tietokannan sivustokokoelman kohteet. Aina, kun työ on luotu, joka sopii mukautetun tietokannan sivustokokoelman kohde, se voidaan laajentaa kohteen suorittamisen aikana ryhmään liittyviä tietokantoja.

Lisää mukautettu tietystä kokoelmasta halutun tietokannan:

    $serverName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName 

#### <a name="review-the-databases-within-a-custom-database-collection-target"></a>Tarkista tietokantojen mukautetun tietokannan sivustokokoelman kohteeseen

**Get-AzureSqlJobTarget** cmdlet-komennon avulla voit noutaa mukautetun tietokannan sivustokokoelman kohteeseen ali-tietokannat. 
 
    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets

### <a name="create-a-job-to-execute-a-script-across-a-custom-database-collection-target"></a>Työn komentosarjan suorittaminen yli mukautetun tietokannan sivustokokoelman kohteen luominen

**Uusi AzureSqlJob** cmdlet-komennon avulla voit luoda mukautetun tietokannan sivustokokoelman kohde määrittämiä tietokantojen lukujoukon vastaan työn. Joustavasti tietokannan työt laajentamalla työn kunkin vastaavat tietokannan liittyvät mukautetun tietokannan sivustokokoelman kohde useita alatason työt ja varmistaa, että komentosarja kohdistaa tietokantojen. On tärkeää, että komentosarjat ovat idempotent on joustavat uudelleenyritykset.

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job

## <a name="data-collection-across-databases"></a>Tietojen kerääminen yli tietokannat

**Joustavasti tietokannan työt** tukee kysely suoritetaan koko ryhmälle tietokantojen ja lähettää tulokset määritetyn tietokannan taulukkoon. Taulukon voidaan suorittaa kysely jälkeen kertoma näet kyselyn tulokset kunkin tietokannasta. Tämä on asynkroninen järjestelmä suorittaa kysely useita tietokantoja yli. Esimerkiksi jonkin tietokannoista, on tilapäisesti poissa käytöstä tapauksissa epäonnistumisen käsitellään automaattisesti uudelleenyritykset kautta.

Määritetyn kohdetaulukko luodaan automaattisesti, jos sitä ei vielä ole, vastaavat palautetut tulosjoukko rakenne. Jos komentosarjan suorittaminen palauttaa useita tulosjoukkoja, joustavasti tietokannan työt lähettää vain ensimmäisen vaiheen annettu kohdetaulukkoon.

Seuraavaa PowerShell-komentosarjaa voidaan suorittaa komentosarjan kerääminen sen tulokset määritettyyn taulukkoon. Tämä komentosarja oletetaan, että T-SQL-komentosarja on luotu joka tulostaa yhden tuloksen määrittäminen ja mukautetun tietokannan sivustokokoelman kohde on luotu.

Määritä haluamasi komentosarjan, tunnistetiedot ja suorittamisen kohde seuraavasti:

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $executionCredentialName = "{Execution Credential Name}"
    $customCollectionName = "{Custom Collection Name}"
    $destinationCredentialName = "{Destination Credential Name}"
    $destinationServerName = "{Destination Server Name}"
    $destinationDatabaseName = "{Destination Database Name}"
    $destinationSchemaName = "{Destination Schema Name}"
    $destinationTableName = "{Destination Table Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName

### <a name="create-and-start-a-job-for-data-collection-scenarios"></a>Luominen ja tietojen keräämisen skenaarioissa työn käynnistäminen
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $executionCredentialName -ContentName $scriptName -ResultSetDestinationServerName $destinationServerName -ResultSetDestinationDatabaseName $destinationDatabaseName -ResultSetDestinationSchemaName $destinationSchemaName -ResultSetDestinationTableName $destinationTableName -ResultSetDestinationCredentialName $destinationCredentialName -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution

## <a name="create-a-schedule-for-job-execution-using-a-job-trigger"></a>Työn käynnistäminen suorittaminen aikataulun luominen

Seuraavaa PowerShell-komentosarjaa voidaan luoda löytävä aikataulun. Tämä komentosarja käyttää minuutin väli, mutta uusi AzureSqlJobSchedule tukee myös - DayInterval ja - HourInterval, - MonthInterval, - WeekInterval parametrit. Aikataulu, joka suorittaa vain kerran voidaan luoda mukaan kulkeva - etsii kerran.

Uuden aikataulun luominen:

    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule -MinuteInterval $minuteInterval -ScheduleName $scheduleName -StartTime $startTime 
    Write-Output $schedule

### <a name="create-a-job-trigger-to-have-a-job-executed-on-a-time-schedule"></a>Työn käynnistimen on suoritettu aikataulun projektin luominen

Työn käynnistäminen voidaan määrittää on aikataulun mukaan suorittaa työn. Seuraavaa PowerShell-komentosarjaa voidaan luoda työn käynnistäminen.

Määritä seuraavat muuttujat vastaamaan haluamasi työn ja joissa:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger -ScheduleName $scheduleName –JobName $jobName
    Write-Output $jobTrigger

### <a name="remove-a-scheduled-association-to-stop-job-from-executing-on-schedule"></a>Voit lopettaa projektin aikataulun suorittaminen ajoitetun välisen suhteen poistaminen

Lopettaa löytävä suorittaminen työn käynnistimen kautta, voit poistaa työn käynnistäminen. Poista työn käynnistimen lopettavat työn suorittamisen **Poista AzureSqlJobTrigger** -cmdlet-komennolla aikataulun mukaan.

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger -ScheduleName $scheduleName -JobName $jobName





## <a name="import-elastic-database-query-results-to-excel"></a>Tuo Excelin joustavasti tietokannan kyselytulokset

 Voit tuoda-tulokset kyselyn Excel-tiedostoon.

1. Käynnistä Excel 2013: ssa.
2.  Siirry **tiedot** -valintanauhassa.
3.  **Muista** lähteistä ja valitse **SQL Serveristä**.

    ![Excel-tietojen tuominen muista lähteistä][5]
4.  Kirjoita **Ohjatussa tietoyhteyden muodostamisessa** palvelimen nimi ja kirjaudu sisään tunnistetiedot. Valitse sitten **Seuraava**.
5.  Valitse valintaikkunassa, **Valitse tietokanta, joka sisältää haluamasi tiedot**, **ElasticDBQuery** tietokanta.
6.  Valitse **Asiakkaat** -taulukon luettelonäkymässä ja valitse **Seuraava**. Valitse **Valmis**.
7.  **Tuontitiedot** -lomake, **Valitse kuinka haluat tarkastella tämän työkirjan tiedot**-kohdassa Valitse **taulukko** ja valitse **OK**.

**Asiakkaat** -taulukon eri shards tallennetaan kaikki rivit täytetään Excel-laskentataulukko.

## <a name="next-steps"></a>Seuraavat vaiheet
Voit nyt käyttää Excelin tietoja funktiot. BI- ja tietojen integrointi Työkalut joustavasti kysely-tietokantayhteyden muodostamisessa palvelimen nimen, tietokannan nimi ja tunnistetiedot yhteysmerkkijonon avulla. Varmista, että SQL Server on tuettu työkalu tietolähteenä. Lisätietoja joustavasti kyselytietokannan ja ulkoiset taulukot, kuten SQL Server-tietokantaan ja SQL Server-taulukoihin, jotka muodostaa yhteyden oman-työkalulla.

### <a name="cost"></a>Kustannukset
Ei uusia maksutonta joustavasti tietokannan kysely-toiminnon avulla. Tällä hetkellä ominaisuus on käytettävissä vain premium tietokantojen kuin päätepiste, mutta shards voi olla minkä tahansa palvelun tason.

Alla on tietoja artikkelissa [SQL tietokanta hinnoittelua tiedot](https://azure.microsoft.com/pricing/details/sql-database/).


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->
