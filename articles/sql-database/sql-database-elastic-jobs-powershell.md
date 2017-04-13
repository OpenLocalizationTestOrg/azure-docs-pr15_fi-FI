<properties 
    pageTitle="Luoda ja hallita joustavasti tietokannan töitä PowerShellin avulla" 
    description="PowerShellin Azure SQL-tietokanta jakavat hallinta" 
    services="sql-database" documentationCenter=""  
    manager="jhubbard" 
    authors="ddove"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove" />

# <a name="create-and-manage-a-sql-database-elastic-database-jobs-using-powershell-preview"></a>Voit luoda ja hallita SQL-tietokantaan joustavasti tietokannan projektit-PowerShellin (ennakkoversio)

> [AZURE.SELECTOR]
- [Azure portal](sql-database-elastic-jobs-create-and-manage.md)
- [PowerShellin](sql-database-elastic-jobs-powershell.md)



PowerShell-ohjelmointirajapinnan **joustavasti tietokannan töitä** (esikatselunäkymässä) avulla voit määrittää ryhmän tietokannoista, joiden komentosarjat suoritetaan. Tässä artikkelissa kerrotaan, miten voit luoda ja hallita **töitä joustavasti tietokannan** käyttämällä PowerShell cmdlet-komennot. Artikkelissa [joustavasti töiden yleiskatsaus](sql-database-elastic-jobs-overview.md). 

## <a name="prerequisites"></a>Edellytykset
* Azure tilaus. Katso maksuttoman kokeiluversion [yhden kuukauden maksuton](https://azure.microsoft.com/pricing/free-trial/).
* Joustavasti Tietokantatyökalut luotujen tietokantojen joukko. Katso [joustavasti Tietokantatyökalut käytön aloittaminen](sql-database-elastic-scale-get-started.md).
* Azure PowerShell. Lisätietoja on artikkelissa [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md).
* **Joustavasti tietokannan projektit** PowerShell-paketin: Katso [asentaminen joustavasti tietokannan projektit](sql-database-elastic-jobs-service-installation.md)

### <a name="select-your-azure-subscription"></a>Valitse Azure tilauksen

Valitse tilaus, sinun on tilauksen tunnus (**– SubscriptionId**) tai tilauksen nimeä (**-SubscriptionName**). Jos sinulla on useita tilauksia, voit suorittaa **Get-AzureRmSubscription** cmdlet-komento ja kopioi haluamasi Tilaustiedot tulosjoukko. Kun olet tilauksen tiedot, suorita seuraavat komentosovelmalla voit määrittää tämän tilauksen oletukseksi eli luomisen ja ylläpidon työt kohde:

    Select-AzureRmSubscription -SubscriptionId {SubscriptionID}

[PowerShell ise:](https://technet.microsoft.com/library/dd315244.aspx) suositellaan käyttö kehittämään ja suorita PowerShell-komentosarjojen vastaan joustavasti tietokannan projektit.

## <a name="elastic-database-jobs-objects"></a>Joustavasti tietokantaobjektien töitä

Seuraavassa taulukossa on lueteltu kaikki objektityypit **Joustavasti tietokannan** töiden sekä sen kuvaus ja kiinnostavat PowerShell-ohjelmointirajapinnan ulos.

<table style="width:100%">
  <tr>
    <th>Objektityyppi</th>
    <th>Kuvaus</th>
    <th>Aiheeseen liittyvät PowerShell-ohjelmointirajapinnan</th>
  </tr>
  <tr>
    <td>Tunnistetietojen</td>
    <td>Käytä tietokantoihin komentosarjojen suorittamisen tai DACPACs sovelluksen yhdistettäessä käyttäjänimi ja salasana. <p>Salasana salataan ennen lähettäminen ja tallentaminen joustavasti tietokannan projektit-tietokantaan.  Salasanan salaus puretaan joustavasti tietokannan projektit-palvelun tunnistetiedon luodaan ja ladattuja asennuksen komentosarjan kautta.</td>
    <td><p>Hae AzureSqlJobCredential</p>
    <p>Uusi AzureSqlJobCredential</p><p>Määritä AzureSqlJobCredential</p></td></td>
  </tr>

  <tr>
    <td>Komentosarja</td>
    <td>Transact-SQL-komentosarja voi käyttää kaikissa tietokantojen suorittamista varten.  Komentosarjan olisi kirjoitti on idempotent, koska palvelun yrittää virheiden yhteydessä komentosarjan suorittaminen.
    </td>
    <td>
    <p>Hae AzureSqlJobContent</p>
    <p>Hae AzureSqlJobContentDefinition</p>
    <p>Uusi AzureSqlJobContent</p>
    <p>Määritä AzureSqlJobContentDefinition</p>
    </td>
  </tr>

  <tr>
    <td>DACPAC</td>
    <td><a href="https://msdn.microsoft.com/library/ee210546.aspx">Tietojen tason sovelluksen </a> package käytetään kaikissa tietokannat.

    </td>
    <td>
    <p>Hae AzureSqlJobContent</p>
    <p>Uusi AzureSqlJobContent</p>
    <p>Määritä AzureSqlJobContentDefinition</p>
    </td>
  </tr>
  <tr>
    <td>Tietokannan kohde</td>
    <td>Tietokannan ja palvelimen nimi valitsemalla Azure SQL-tietokantaan.

    </td>
    <td>
    <p>Hae AzureSqlJobTarget</p>
    <p>Uusi AzureSqlJobTarget</p>
    </td>
  </tr>
  <tr>
    <td>Shard kartan kohde</td>
    <td>Tietokannan kohde ja tunnistetiedon käytettävät tiedot joustavasti tietokanta-shard kartan tallennettuna yhdistelmä.
    </td>
    <td>
    <p>Hae AzureSqlJobTarget</p>
    <p>Uusi AzureSqlJobTarget</p>
    <p>Määritä AzureSqlJobTarget</p>
    </td>
  </tr>
<tr>
    <td>Mukautetun sivustokokoelman kohde</td>
    <td>Tietokantojen käyttäminen yhdessä suorittamisen määritetty ryhmä.</td>
    <td>
    <p>Hae AzureSqlJobTarget</p>
    <p>Uusi AzureSqlJobTarget</p>
    </td>
  </tr>
<tr>
    <td>Mukautetun sivustokokoelman lapsen kohde</td>
    <td>Tietokannan kohde, johon on viittaus mukautetun kokoelmasta.</td>
    <td>
    <p>Lisää AzureSqlJobChildTarget</p>
    <p>Poista AzureSqlJobChildTarget</p>
    </td>
  </tr>

<tr>
    <td>Työn</td>
    <td>
    <p>Työn, joka voidaan käyttää käynnistettävän suorittamisen tai täyttämään aikataulun parametrit määritys.</p>
    </td>
    <td>
    <p>Hae AzureSqlJob</p>
    <p>Uusi AzureSqlJob</p>
    <p>Määritä AzureSqlJob</p>
    </td>
  </tr>

<tr>
    <td>Projektin suorittaminen</td>
    <td>
    <p>Tehtävät, jotka tarvitaan täyttämään komentosarjan suorittaminen tai soveltaminen käyttöoikeustietoja tietokantayhteyksiä kanssa virheet kohde DACPAC säilö käsitellään suorittamisen käytännön mukaisesti.</p>
    </td>
    <td>
    <p>Hae AzureSqlJobExecution</p>
    <p>Käynnistä AzureSqlJobExecution</p>
    <p>Lopeta AzureSqlJobExecution</p>
    <p>Odota AzureSqlJobExecution</p>
  </tr>

<tr>
    <td>Projektin tehtävän suorittaminen</td>
    <td>
    <p>Työn yhtenä yksikkönä työn täyttämiseen.</p>
    <p>Jos työtehtävän määrittäminen ei ole onnistuneesti voi suorittaa, tuloksena olevat Poikkeusviesti kirjataan ja uusi vastaavia työn tehtävä luodaan ja suorittaa määritetyn suorittamisen käytännön mukaisesti.</p></p>
    </td>
    <td>
    <p>Hae AzureSqlJobExecution</p>
    <p>Käynnistä AzureSqlJobExecution</p>
    <p>Lopeta AzureSqlJobExecution</p>
    <p>Odota AzureSqlJobExecution</p>
  </tr>

<tr>
    <td>Työn suorittamisen käytäntö</td>
    <td>
    <p>Ohjausobjektien projektin suorittaminen aikakatkaisu, yritä rajoitukset ja uudelleenyritykset välit.</p>
    <p>Joustavasti tietokannan työt on oletusarvon projektin suorittaminen käytännön mikä aiheuttaa olennaisesti äärettömän uudelleenyritykset projektin tehtävän virheiden kunkin uudelleen välit eksponentiaalisen backoff kanssa.</p>
    </td>
    <td>
    <p>Hae AzureSqlJobExecutionPolicy</p>
    <p>Uusi AzureSqlJobExecutionPolicy</p>
    <p>Määritä AzureSqlJobExecutionPolicy</p>
    </td>
  </tr>

<tr>
    <td>Aikataulu</td>
    <td>
    <p>Ajan perusteella määrityksen suorittamisen tapahtuu löytävä välin tai yksi kerrallaan.</p>
    </td>
    <td>
    <p>Hae AzureSqlJobSchedule</p>
    <p>Uusi AzureSqlJobSchedule</p>
    <p>Määritä AzureSqlJobSchedule</p>
    </td>
  </tr>

<tr>
    <td>Työn Käynnistimet</td>
    <td>
    <p>Yhdistämismäärityksen työn ja aikataulun käynnistimen suorittaminen aikataulun mukaan.</p>
    </td>
    <td>
    <p>Uusi AzureSqlJobTrigger</p>
    <p>Poista AzureSqlJobTrigger</p>
    </td>
  </tr>
</table>

## <a name="supported-elastic-database-jobs-group-types"></a>Tuetut joustavasti tietokannan työt ryhmitellä tyypit
Työn suorittaa Transact-SQL (T-SQL) komentosarjoja tai DACPACs sovelluksen tietokantojen ryhmä. Kun työ on lähetetty, joka suoritetaan koko ryhmälle tietokantoja, työn "laajentaa" lapsen työt kohtaa, johon jokainen suorittaa yhteen tietokannalle pyydetty suorittamisen ryhmässä kyselyjä. 
 
Ryhmät, joihin voit luoda kahdentyyppisiä on: 

* [Shard kartta](sql-database-elastic-scale-shard-map-management.md) -ryhmän: kun työ on lähetetty kohde shard kartan, työn shard kartan määrittämään shards asetetaan kyselyt ja luo lapsen kunkin shard työt shard määrityksen.
* Mukautetun sivustokokoelman ryhmän: mukautetut tietokannat joukko. Kun työn kohteena Mukautettu sivustokokoelman, se luo lapsen tietokantojen työt tällä hetkellä mukautetun-kokoelman.

## <a name="to-set-the-elastic-database-jobs-connection"></a>Voit määrittää joustavasti tietokannan työt yhteys

Yhteyden on määritettävä työt *ohjausobjektin tietokanta* ennen työt ohjelmointirajapinnan käyttäminen. Käynnissä cmdlet käynnistää tunnistetiedon-ikkuna, jossa voit pyytää käyttäjänimeä ja salasanaa, jotka on luotu joustavasti tietokannan työt asennettaessa ponnahdusikkuna. Kaikkien annettu sisällä tämän artikkelin Esimerkeissä oletetaan, että tätä ensimmäistä vaihetta on jo suoritettu.

Avaa yhteyden joustavasti tietokannan projektit:

    Use-AzureSqlJobConnection -CurrentAzureSubscription 

## <a name="encrypted-credentials-within-the-elastic-database-jobs"></a>Salatun tunnistetiedot sisällä joustavasti tietokannan projektit

Tietokannan tunnistetietoja voi lisätä töiden *hallinta tietokannan* salattu salasana. On tarpeen, jotta töitä, joka suoritetaan voi tehdä myöhemmin (joko työaikatauluja) tunnistetietojen tallentaminen.
 
Salauksen toimii kautta asennuksen komentosarjaa osana luotu varmenne. Asennus-komentosarja luo ja lataa varmenteen tuominen salatut salasanat salauksen Azure-pilvipalvelussa sijaitsevaan. Azure-pilvipalvelussa tallentaa myöhemmin työt *ohjausobjektin tietokannan* salaaminen annettujen salasanan tarvitsematta sertifikaatin avulla asennettu paikallisesti PowerShell-Ohjelmointirajapinnan tai Azure perinteinen portaalin käyttöliittymän, jonka julkinen avain.
 
Tunnistetietojen salasanat ovat salattuja ja suojatun käyttäjät, joilla on lukuoikeudet joustavasti tietokantaobjekteihin työt. Mutta se on mahdollista haittaohjelmien käyttäjä, jolla luku-ja kirjoitusoikeudet joustavasti tietokannan työt objektien Pura salasanan. Tunnistetiedot on suunniteltu työn suorituskerran uudelleen. Tunnistetiedot välitetään kohdetietokantojen yhteyksiä muodostettaessa. Ei ole tällä hetkellä mitään rajoituksia käytetään kunkin tunnistetiedon kohde-tietokantoja, haittaohjelmien käyttäjä voi lisätä tietokantaan haittaohjelmien käyttäjän hallita tietokannan kohde. Käyttäjä voi käynnistää myöhemmin työn kohdistamisen tämä tietokanta voitto tunnistetiedon salasana.

Suojauksen parhaita käytäntöjä joustavasti tietokannan projektit ovat seuraavat:

* Rajoita luotettujen käyttäjille ohjelmointirajapinnan käyttö.
* Tunnistetiedot on oltava vähintään projektin tehtävien suorittamiseen tarvittavat oikeudet.  Lisätietoja näkevät sisällä SQL Server MSDN [todennus ja käyttöoikeudet](https://msdn.microsoft.com/library/bb669084.aspx) -artikkelissa.

### <a name="to-create-an-encrypted-credential-for-job-execution-across-databases"></a>Voit luoda salattuja tunnistetiedon, työn suorittamista varten yli tietokannat

Jos haluat luoda uuden salattuja tunnistetiedon, [**Hae tunnistetiedon cmdlet-komento**](https://technet.microsoft.com/library/hh849815.aspx) pyytää käyttäjänimeä ja salasanaa, jolla voi välittää [**Uusi AzureSqlJobCredential cmdlet-komento**](https://msdn.microsoft.com/library/mt346063.aspx).

    $credentialName = "{Credential Name}"
    $databaseCredential = Get-Credential
    $credential = New-AzureSqlJobCredential -Credential $databaseCredential -CredentialName $credentialName
    Write-Output $credential

### <a name="to-update-credentials"></a>Jos haluat päivittää tunnistetiedot

Salasanojen muuttuessa [**määrittäminen AzureSqlJobCredential cmdlet**](https://msdn.microsoft.com/library/mt346062.aspx) -komennolla ja määrittää **CredentialName** -parametrin.

    $credentialName = "{Credential Name}"
    Set-AzureSqlJobCredential -CredentialName $credentialName -Credential $credential 

## <a name="to-define-an-elastic-database-shard-map-target"></a>Voit määrittää joustavasti tietokannan shard kartta-kohde

Suorittaa kaikki tietokannoissa työn shard joukon (luodaan käyttämällä [joustavasti tietokannan asiakkaan kirjaston](sql-database-elastic-database-client-library.md)), Määritä shard kartan tietokannan kohde. Tässä esimerkissä edellyttää sharded sovellus, joka on luotu joustavasti tietokannan asiakas-kirjasto. Katso [joustavasti tietokannan Työkalut otoksen käytön aloittaminen](sql-database-elastic-scale-get-started.md).

Tietokannan kohde on määritettävä shard kartan manager-tietokanta ja valitse tietyn shard kartan on määritettävä kohteeksi.

    $shardMapCredentialName = "{Credential Name}"
    $shardMapDatabaseName = "{ShardMapDatabaseName}" #example: ElasticScaleStarterKit_ShardMapManagerDb
    $shardMapDatabaseServerName = "{ShardMapServerName}"
    $shardMapName = "{MyShardMap}" #example: CustomerIDShardMap
    $shardMapDatabaseTarget = New-AzureSqlJobTarget -DatabaseName $shardMapDatabaseName -ServerName $shardMapDatabaseServerName
    $shardMapTarget = New-AzureSqlJobTarget -ShardMapManagerCredentialName $shardMapCredentialName -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapDatabaseServerName -ShardMapName $shardMapName
    Write-Output $shardMapTarget

## <a name="create-a-t-sql-script-for-execution-across-databases"></a>T-SQL-komentosarjan suorittaminen luominen eri tietokannat

Kun luot T-SQL-komentosarjojen suorittamisen, on erittäin suositeltava ja muodostamisesta on [idempotent](https://en.wikipedia.org/wiki/Idempotence) ja joustavat vastaan virheet. Joustavasti tietokannan työt yrittää komentosarjan suorittaminen aina, kun suorittamisen tapahtuu virhe, virhe luokittelu riippumatta.

[**Uusi AzureSqlJobContent cmdlet-komennon**](https://msdn.microsoft.com/library/mt346085.aspx) avulla voit luoda ja tallentaa komentosarjan suorittaminen ja **- ContentName** ja **- CommandText** parametrien määrittäminen.

    $scriptName = "Create a TestTable"

    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
        CREATE TABLE TestTable(
            TestTableId INT PRIMARY KEY IDENTITY,
            InsertionTime DATETIME2
        );
    END
    GO
    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO"

    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="create-a-new-script-from-a-file"></a>Luo uusi komentosarja-tiedostosta
Jos tiedostossa on määritetty T-SQL-komentosarja, tämän toiminnon avulla voit tuoda komentosarja:

    $scriptName = "My Script Imported from a File"
    $scriptPath = "{Path to SQL File}"
    $scriptCommandText = Get-Content -Path $scriptPath
    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="to-update-a-t-sql-script-for-execution-across-databases"></a>T-SQL-komentosarjan suorittaminen päivittyminen tietokannat  

Tämä PowerShell-komentosarjaa päivittää aiemmin luotua komentosarjaa T-SQL-komennon teksti.

Määritä seuraavat muuttujat vastaamaan asettaminen haluamasi komentosarja-määritys:

    $scriptName = "Create a TestTable"
    $scriptUpdateComment = "Adding AdditionalInformation column to TestTable"
    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
    CREATE TABLE TestTable(
        TestTableId INT PRIMARY KEY IDENTITY,
        InsertionTime DATETIME2
    );
    END
    GO

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN
    ALTER TABLE TestTable
    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO"

### <a name="to-update-the-definition-to-an-existing-script"></a>Päivitä aiemmin luotua komentosarjaa määritys

    Set-AzureSqlJobContentDefinition -ContentName $scriptName -CommandText $scriptCommandText -Comment $scriptUpdateComment 

## <a name="to-create-a-job-to-execute-a-script-across-a-shard-map"></a>Työn komentosarjan suorittaminen yli shard kartan luominen

Tämä PowerShell-komentosarjaa käynnistyy komentosarjan suorittaminen työn joustavasti asteikko-shard kartan kunkin shard yli.

Määritä seuraavat muuttujat haluamasi komentosarjan ja kohde:

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $credentialName = "{Credential Name}"
    $shardMapTarget = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName 
    $job = New-AzureSqlJob -ContentName $scriptName -CredentialName $credentialName -JobName $jobName -TargetId $shardMapTarget.TargetId
    Write-Output $job

## <a name="to-execute-a-job"></a>Projektin suorittaminen 

Tämä PowerShell-komentosarjaa suorittaa aiemmin luodun työn:

Päivitä seuraavat muuttujan vastaamaan haluamasi projektin nimeä, jonka haluat on suoritettu:

    $jobName = "{Job Name}"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName 
    Write-Output $jobExecution
 
## <a name="to-retrieve-the-state-of-a-single-job-execution"></a>Noutaa yhden projektin suorittaminen tila

[**Get-AzureSqlJobExecution cmdlet**](https://msdn.microsoft.com/library/mt346058.aspx) -komennolla ja voit tarkastella projektin suorittaminen tilan **JobExecutionId** -parametrin.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecution = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId
    Write-Output $jobExecution

**IncludeChildren** -parametrin saman **Get-AzureSqlJobExecution** cmdlet-komennon avulla voit tarkastella lapsen työn suorituskerran tilan eli kunkin kunkin tietokannalle kohdistettu projektin suorittaminen tietyn tila.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions 

## <a name="to-view-the-state-across-multiple-job-executions"></a>Jos haluat useita työn suorituskerran tilan tarkasteleminen

[**Get-AzureSqlJobExecution cmdlet-komento**](https://msdn.microsoft.com/library/mt346058.aspx) on useita valinnaisten parametrien, jonka avulla voit näyttää useita työn suorituskerran suodatettu annettujen parametrien avulla. Seuraavassa esitellään joitakin mahdollista saada AzureSqlJobExecution käyttötapaa:

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

## <a name="to-retrieve-failures-within-job-task-executions"></a>Projektin tehtävän suorituskerran sisällä virheiden noutaminen

**JobTaskExecution objekti** sisältää tehtävän sekä viestin ominaisuus elinkaari ominaisuuden. Jos projektin tehtävän suorittaminen, epäonnistui, elinkaari-ominaisuuden arvoksi tulee *epäonnistui* ja sanomaominaisuuden asetetaan tuloksena Poikkeusviesti ja pinossa. Jos projektin ei onnistunut, on tärkeää voit tarkastella projektin tehtävistä, joihin ei onnistunut annetun projektille.

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions) 
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }

## <a name="to-wait-for-a-job-execution-to-complete"></a>Projektin suorittaminen, viimeistele odottamaan

Odottamaan työn tehtävä voidaan käyttää seuraavaa PowerShell-komentosarjaa:

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
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval 
    -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
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

Joustavasti tietokannan työt tukee peruutus pyynnöt työt.  Jos joustavasti tietokannan työt havaitsee parhaillaan suorittaa työn peruutus pyynnön, se yrittää lopettaa työn.

Liittyy joustavasti tietokannan työt voi suorittaa peruutuksen kahdella eri tavalla:

1. Peruuta suorittavien tehtävät: Jos peruutuksen havaitaan, kun tehtävä on käynnissä, peruutus yritetään parhaillaan kuvasuhde tehtävän kuluessa.  Esimerkki: Jos näkyvissä on tällä hetkellä käytössä suoritetaan, kun peruutuksen yritetään pitkään käynnissä kyselyn, yritetään peruuttaa kyselyn ole.
2. Peruuttaminen tehtävän uudelleenyritykset: Jos peruutuksen havaitaan ohjausobjektin viestiketjun mukaan, ennen kuin tehtävä käynnistetään suorittamista varten, ohjausobjektin säikeen välttää tehtävä ja määritellä pyynnön, kun peruutettu.

Jos projektin peruutus haetaan ylemmän tason työn, peruutus-pyyntö voi käyttää, ylemmän tason työn ja kaikki sen alatason työt.
 
Lähetä peruutus-pyyntö, [**Lopeta AzureSqlJobExecution cmdlet**](https://msdn.microsoft.com/library/mt346053.aspx) -komennolla ja määrittää **JobExecutionId** -parametrin.

    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId

## <a name="to-delete-a-job-and-job-history-asynchronously"></a>Työn ja Työhistoria asynkronisesti

Joustavasti tietokannan työt tukee asynkroninen poistaminen työt. Työn voidaan merkitä poistettavaksi ja järjestelmä poistaa työ- ja kaikki sen Työhistoria, kun kaikki projektin suorituskerran suorittanut työn. Järjestelmä ei Peruuta aktiivisen työn suorituskerran automaattisesti.  

Käynnistää [**Stop AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346053.aspx) Peruuta aktiivisen työn suorituskerran.

Käynnistettävän työn poistamista, [**Poista AzureSqlJob cmdlet**](https://msdn.microsoft.com/library/mt346083.aspx) -komennolla ja määrittää **työ** -parametrin.

    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName
 
## <a name="to-create-a-custom-database-target"></a>Luo mukautettu tietokannan kohde

Voit määrittää mukautetun tietokannan kohteiden suoraan suorittamisen tai merkitsemiseen mukautetun tietokannan ryhmän. Esimerkiksi koska **joustavasti tietokannan jakavat** ei ole vielä suoraan tueta käyttämällä PowerShell-ohjelmointirajapinnan, voit luoda mukautetun tietokannan kohde- ja mukautetun tietokannan sivustokokoelman kohde, joka kattaa kaikki ryhmän tietokannat.

Määritä seuraavat muuttujat vastaamaan halutun tietokannan tietoja:

    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName 

## <a name="to-create-a-custom-database-collection-target"></a>Luo mukautettu tietokannan sivustokokoelman kohde

[**Uusi AzureSqlJobTarget**](https://msdn.microsoft.com/library/mt346077.aspx) cmdlet-komennon avulla voit määrittää mukautetun tietokannan sivustokokoelman kohde suorittamisen käyttöön useita määritetyn tietokannan kohteiden välillä. Kun olet luonut tietokannan ryhmän, tietokantojen voidaan liittää Mukautettu sivustokokoelman kohde.

Määritä seuraavat muuttujat vastaamaan haluamasi mukautettu sivustokokoelman kohde määritykset:

    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 

### <a name="to-add-databases-to-a-custom-database-collection-target"></a>Voit lisätä mukautetun tietokannan sivustokokoelman kohde tietokannat

Voit lisätä mukautetun tietystä kokoelmasta tietokanta käyttämällä [**Lisää AzureSqlJobChildTarget**](https://msdn.microsoft.comlibrary/mt346064.aspx) cmdlet-komento.

    $databaseServerName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName 

#### <a name="review-the-databases-within-a-custom-database-collection-target"></a>Tarkista tietokantojen mukautetun tietokannan sivustokokoelman kohteeseen

[**Get-AzureSqlJobTarget**](https://msdn.microsoft.com/library/mt346077.aspx) cmdlet-komennon avulla voit noutaa mukautetun tietokannan sivustokokoelman kohteeseen ali-tietokannat. 
 
    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets

### <a name="create-a-job-to-execute-a-script-across-a-custom-database-collection-target"></a>Työn komentosarjan suorittaminen yli mukautetun tietokannan sivustokokoelman kohteen luominen

[**Uusi AzureSqlJob**](https://msdn.microsoft.com/library/mt346078.aspx) cmdlet-komennon avulla voit luoda mukautetun tietokannan sivustokokoelman kohde määrittämiä tietokantojen lukujoukon vastaan työn. Joustavasti tietokannan työt laajentamalla työn kunkin vastaavat tietokannan liittyvät mukautetun tietokannan sivustokokoelman kohde useita alatason työt ja varmistaa, että komentosarja kohdistaa tietokantojen. On tärkeää, että komentosarjat ovat idempotent on joustavat uudelleenyritykset.

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job

## <a name="data-collection-across-databases"></a>Tietojen kerääminen yli tietokannat

Työn voit suorittaa kyselyn tietokantojen ryhmän koko ja lähetä tulokset tiettyyn taulukkoon. Taulukon voidaan suorittaa kysely jälkeen kertoma näet kyselyn tulokset kunkin tietokannasta. Tämä on asynkroninen tapa kyselyn suorittaminen useita tietokantoja yli. Epäonnistuneiden yritysten käsitellään automaattisesti uudelleenyritykset kautta.

Määritetyn kohdetaulukko luodaan automaattisesti, jos sitä ei ole vielä. Uuden taulukon vastaa palautetut tulosjoukko rakenne. Jos komentosarjan palauttaa useita tulosjoukkoja, joustavasti tietokannan työt lähettää vain ensimmäisen kohdetaulukko.

Seuraavaa PowerShell-komentosarjaa suorittaa komentosarjan ja sen tulosten kerää määritettyyn taulukkoon. Tämä komentosarja oletetaan, että T-SQL-komentosarja on luotu joka tulostaa yhden tuloksen määrittäminen ja mukautetun tietokannan sivustokokoelman kohde on luotu.

Tämä komentosarja käyttää [**Get-AzureSqlJobTarget**](https://msdn.microsoft.com/library/mt346077.aspx) cmdlet-komento. Komentosarjan, tunnistetiedot ja suorittamisen kohde parametrien määrittäminen:

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

### <a name="to-create-and-start-a-job-for-data-collection-scenarios"></a>Luominen ja tietojen keräämisen skenaarioissa työn käynnistäminen

Tämä komentosarja käyttää [**Käynnistä AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346055.aspx) cmdlet-komento.
 
    $job = New-AzureSqlJob -JobName $jobName 
    -CredentialName $executionCredentialName 
    -ContentName $scriptName 
    -ResultSetDestinationServerName $destinationServerName 
    -ResultSetDestinationDatabaseName $destinationDatabaseName 
    -ResultSetDestinationSchemaName $destinationSchemaName 
    -ResultSetDestinationTableName $destinationTableName 
    -ResultSetDestinationCredentialName $destinationCredentialName 
    -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution

## <a name="to-schedule-a-job-execution-trigger"></a>Jos haluat ajoittaa projektin suorittaminen käynnistin

Seuraavaa PowerShell-komentosarjaa voidaan luoda toistuvan aikataulun. Tämä komentosarja käyttää minuutit aikaväli, mutta [**Uusi AzureSqlJobSchedule**](https://msdn.microsoft.com/library/mt346068.aspx) tukee myös - DayInterval ja - HourInterval, - MonthInterval, - WeekInterval parametrit. Aikataulu, joka suorittaa vain kerran voidaan luoda mukaan kulkeva - etsii kerran.

Uuden aikataulun luominen:

    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule 
    -MinuteInterval $minuteInterval 
    -ScheduleName $scheduleName 
    -StartTime $startTime 
    Write-Output $schedule

### <a name="to-trigger-a-job-executed-on-a-time-schedule"></a>Käynnistettävän työn suorittaa aikataulu

Työn käynnistäminen voidaan määrittää on aikataulun mukaan suorittaa työn. Seuraavaa PowerShell-komentosarjaa voidaan luoda työn käynnistäminen.

Käytä [Uutta AzureSqlJobTrigger](https://msdn.microsoft.com/library/mt346069.aspx) ja määritä haluamasi työn ja joissa vastaamaan seuraavat muuttujat:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger
    -ScheduleName $scheduleName
    –JobName $jobName
    Write-Output $jobTrigger

### <a name="to-remove-a-scheduled-association-to-stop-job-from-executing-on-schedule"></a>Ajoitetun työn suorittaminen aikataulussa lopettavat välisen suhteen poistaminen

Lopettaa löytävä suorittaminen työn käynnistimen kautta, voit poistaa työn käynnistäminen. Poista työn käynnistimen lopettavat työn suorittamisen [**Poista AzureSqlJobTrigger cmdlet**](https://msdn.microsoft.com/library/mt346070.aspx)-aikataulun mukaan.

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger 
    -ScheduleName $scheduleName 
    -JobName $jobName

### <a name="retrieve-job-triggers-bound-to-a-time-schedule"></a>Hae työn käynnistimien sidottu aikataulu

Seuraavaa PowerShell-komentosarjaa voidaan hankkiminen ja näyttää tietyn aikataulun rekisteröity työn Käynnistimet.

    $scheduleName = "{Schedule Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -ScheduleName $scheduleName
    Write-Output $jobTriggers

### <a name="to-retrieve-job-triggers-bound-to-a-job"></a>Noutaa työn käynnistimien sidottu työ 

[Hae AzureSqlJobTrigger](https://msdn.microsoft.com/library/mt346067.aspx) avulla voit hankkia ja näyttää sisältävä rekisteröity projektin aikatauluja.

    $jobName = "{Job Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -JobName $jobName
    Write-Output $jobTriggers

## <a name="to-create-a-data-tier-application-dacpac-for-execution-across-databases"></a>Voit luoda tietojen tason sovelluksen (DACPAC) suorittamista varten yli tietokannat

DACPAC luomisesta on artikkelissa [tietojen tason sovellukset](https://msdn.microsoft.com/library/ee210546.aspx). Ottaa käyttöön DACPAC, käytä [Uusi AzureSqlJobContent cmdlet-komento](https://msdn.microsoft.com/library/mt346085.aspx). DACPAC on oltava käytettävissä-palveluun. On suositeltavaa luodut DACPAC lataaminen Azuren tallennustilaan ja luo [Jaettu Access allekirjoitus](../storage/storage-dotnet-shared-access-signature-part-1.md) DACPAC.

    $dacpacUri = "{Uri}"
    $dacpacName = "{Dacpac Name}"
    $dacpac = New-AzureSqlJobContent -DacpacUri $dacpacUri -ContentName $dacpacName 
    Write-Output $dacpac

### <a name="to-update-a-data-tier-application-dacpac-for-execution-across-databases"></a>Päivittyminen tietokantojen suorittamisen tietojen taso-sovellus (DACPAC)

Aiemmin DACPACs rekisteröity kuluessa joustavasti tietokannan työt voidaan päivittää siten, että uusi URI. [**Aseta AzureSqlJobContentDefinition cmdlet-komennon**](https://msdn.microsoft.com/library/mt346074.aspx) avulla voit päivittää aiemmin rekisteröidyn DACPAC DACPAC-URI:

    $dacpacName = "{Dacpac Name}"
    $newDacpacUri = "{Uri}"
    $updatedDacpac = Set-AzureSqlJobDacpacDefinition -ContentName $dacpacName -DacpacUri $newDacpacUri
    Write-Output $updatedDacpac

## <a name="to-create-a-job-to-apply-a-data-tier-application-dacpac-across-databases"></a>Työn tietojen taso-sovellus (DACPAC) välittyminen kaikkiin tietokantojen luominen

Sen jälkeen, kun DACPAC on luotu joustavasti tietokannan projektit-työn voi luoda DACPAC välittyminen kaikkiin tietokantojen ryhmän. Seuraavaa PowerShell-komentosarjaa voidaan luominen DACPAC projektin eri tietokantojen mukautetun kokoelma:

    $jobName = "{Job Name}"
    $dacpacName = "{Dacpac Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget 
    -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob 
    -JobName $jobName 
    -CredentialName $credentialName 
    -ContentName $dacpacName -TargetId $target.TargetId
    Write-Output $job 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-powershell/cmd-prompt.png
[2]: ./media/sql-database-elastic-jobs-powershell/portal.png
<!--anchors-->
