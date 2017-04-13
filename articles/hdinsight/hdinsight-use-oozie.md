<properties
    pageTitle="Käytä Hadoop Oozie HDInsight | Microsoft Azure"
    description="Käytä Hadoop Oozie HDInsight-big datasta palvelu. Opettele Oozie työnkulun suunnittelu ja Lähetä Oozie työn."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="jgao"/>


# <a name="use-oozie-with-hadoop-to-define-and-run-a-workflow-in-hdinsight"></a>Oozie käyttäminen Hadoop määrittäminen ja HDInsight työnkulun suorittaminen

[AZURE.INCLUDE [oozie-selector](../../includes/hdinsight-oozie-selector.md)]

Opettele käyttämään Apache Oozie työnkulun määrittäminen ja suorittaa työnkulun Hdinsightista. Lisätietoja Oozie koordinaattien artikkelissa [Käytön aikapohjaisten Hadoop Oozie koordinaattien kanssa HDInsight][hdinsight-oozie-coordinator-time]. Lisätietoja Azure Data Factory on artikkelissa [käyttö Possu ja rakenteen kanssa Data Factory][azure-data-factory-pig-hive].

Apache Oozie on työnkulun/yhteensovittamisesta, joka hallitsee Hadoop työt. Hadoop-pino on integroitu ja se tukee Hadoop työt Apache MapReduce, Apache Possu, Apache rakenne ja Apache Sqoop. Myös se voidaan ajoittaa työt, jotka ovat järjestelmä, kuten Java-ohjelmia tai shell-komentosarjoja.

Otat noudattamalla tässä opetusohjelmassa työnkulku sisältää kaksi tehtävää:

![Työnkulkukaavio][img-workflow-diagram]

1. Rakenne-toiminto suoritetaan HiveQL komentosarjan esiintymiskertojen laskeminen kunkin lokin tason tyypin log4j tiedostossa. Log4j tiedostoille koostuu rivi-kentät, joka sisältää [LOKIN taso]-kenttä, joka näyttää tyyppi ja vakavuus, esimerkiksi:

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

    Rakenne-komentosarjan tulos on:

        [DEBUG] 434
        [ERROR] 3
        [FATAL] 1
        [INFO]  96
        [TRACE] 816
        [WARN]  4

    Lisätietoja rakenne-kohdassa [Käytä rakenne ja HDInsight][hdinsight-use-hive].

2.  Sqoop toiminto Vie HiveQL tulosteen Azure SQL-tietokannan taulukkoon. Saat lisätietoja Sqoop [Käytön Hadoop Sqoop kanssa HDInsight][hdinsight-use-sqoop].

> [AZURE.NOTE] Katso HDInsight klustereiden tuetut Oozie versiot, [uudet myöntämä HDInsight Hadoop-klusterin versioissa?] [hdinsight-versions].

###<a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat Tässä opetusohjelmassa, sinun on oltava seuraavasti:

- **Työasema Azure PowerShellin avulla**. 

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]
    
    Suorittamaan Windows PowerShell-komentosarjojen käyttää järjestelmänvalvojan oikeuksin ja suorittamisen käytännön asettaminen *RemoteSigned*. Katso lisätietoja, [Suorita Windows PowerShell-komentosarjojen][powershell-script].

##<a name="define-oozie-workflow-and-the-related-hiveql-script"></a>Määritä Oozie työnkulun ja Aiheeseen liittyvät HiveQL komentosarja

Oozie työnkulkujen määritelmät kirjoitetaan hPDL (XML prosessin Definition Language). Työnkulun oletusnimi on *workflow.xml*. Seuraavassa on työnkulun tiedoston, voit käyttää tässä opetusohjelmassa.

    <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
        <start to = "RunHiveScript"/>

        <action name="RunHiveScript">
            <hive xmlns="uri:oozie:hive-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                    <property>
                        <name>mapred.job.queue.name</name>
                        <value>${queueName}</value>
                    </property>
                </configuration>
                <script>${hiveScript}</script>
                <param>hiveTableName=${hiveTableName}</param>
                <param>hiveDataFolder=${hiveDataFolder}</param>
                <param>hiveOutputFolder=${hiveOutputFolder}</param>
            </hive>
            <ok to="RunSqoopExport"/>
            <error to="fail"/>
        </action>

        <action name="RunSqoopExport">
            <sqoop xmlns="uri:oozie:sqoop-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                    <property>
                        <name>mapred.compress.map.output</name>
                        <value>true</value>
                    </property>
                </configuration>
            <arg>export</arg>
            <arg>--connect</arg>
            <arg>${sqlDatabaseConnectionString}</arg>
            <arg>--table</arg>
            <arg>${sqlDatabaseTableName}</arg>
            <arg>--export-dir</arg>
            <arg>${hiveOutputFolder}</arg>
            <arg>-m</arg>
            <arg>1</arg>
            <arg>--input-fields-terminated-by</arg>
            <arg>"\001"</arg>
            </sqoop>
            <ok to="end"/>
            <error to="fail"/>
        </action>

        <kill name="fail">
            <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
        </kill>

        <end name="end"/>
    </workflow-app>

Käytettävissä on kaksi työnkulun määrittämät toimintoja. Käynnistä voit-toiminto on *RunHiveScript*. Jos toiminto on suoritettu onnistuneesti, seuraava toimi on *RunSqoopExport*.

RunHiveScript on useita muuttujia. Välittää arvot, kun lähetät Oozie työn työaseman Azure PowerShell-toiminnolla.

<table border = "1">
<tr><th>Työnkulun muuttujat</th><th>Kuvaus</th></tr>
<tr><td>${jobTracker}</td><td>Määrittää Hadoop Työn seuranta URL-osoite. Käytä <strong>jobtrackerhost:9010</strong> HDInsight versiot 3.0- ja 2.1.</td></tr>
<tr><td>${nameNode}</td><td>Määrittää Hadoop nimi-solmu URL-osoite. Käyttää oletusarvon tiedoston järjestelmän osoite, esimerkiksi <i>wasbs: / /&lt;containerName&gt;@&lt;storageAccountName&gt;. blob.core.windows.net</i>.</td></tr>
<tr><td>${queueName}</td><td>Määrittää työn lähetetään jonon nimen. Käytä <strong>oletusarvoista</strong>.</td></tr>
</table>

<table border = "1">
<tr><th>Rakenne-toiminnon muuttuja</th><th>Kuvaus</th></tr>
<tr><td>${hiveDataFolder}</td><td>Määrittää lähdekansion, Luo taulukko rakenne-komennon.</td></tr>
<tr><td>${hiveOutputFolder}</td><td>Määrittää lisää korvaa-lauseen Tulostekansio.</td></tr>
<tr><td>${hiveTableName}</td><td>Määrittää nimen, rakennetaulukko, joka viittaa log4j datatiedostot.</td></tr>
</table>

<table border = "1">
<tr><th>Sqoop toiminnon muuttuja</th><th>Kuvaus</th></tr>
<tr><td>${sqlDatabaseConnectionString}</td><td>Määrittää Azure SQL-tietokannan yhteysmerkkijono.</td></tr>
<tr><td>${sqlDatabaseTableName}</td><td>Määrittää Azure SQL-tietokannan taulukon kohtaa, johon tiedot viedään.</td></tr>
<tr><td>${hiveOutputFolder}</td><td>Määrittää rakenne Lisää korvaa-lauseen Tulostekansio. Tämä on samassa kansiossa Sqoop viennin (dir Vie).</td></tr>
</table>

Lisätietoja Oozie työnkulun ja työnkulkutoimintojen käyttäminen [Apache Oozie 4.0] ohjeissa[ apache-oozie-400] (for HDInsight 3.0-versiota) tai [Apache Oozie 3.3.2 dokumentaatio] [ apache-oozie-332] (for HDInsight versio 2.1).


Työnkulun rakenne-toiminto kutsuu HiveQL komentosarjatiedosto. Komentosarjatiedoston nimi sisältää kolme HiveQL lausetta:

    DROP TABLE ${hiveTableName};
    CREATE EXTERNAL TABLE ${hiveTableName}(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
    INSERT OVERWRITE DIRECTORY '${hiveOutputFolder}' SELECT t4 AS sev, COUNT(*) AS cnt FROM ${hiveTableName} WHERE t4 LIKE '[%' GROUP BY t4;

1. **PUDOTA TABLE-lause** poistaa log4j rakennetaulukko, jos se on olemassa.
2. **CREATE TABLE-lause** Luo log4j rakenteen ulkoisen taulukon, joka osoittaa log4j lokitiedosto haluamaasi kohtaan. Kenttäerotin on ";". Rivin oletuserotin on "\n". Ulkoisen rakennetaulukko käytetään välttää tiedosto poistetaan alkuperäisestä sijainnista, jos haluat suorittaa työnkulun Oozie useita kertoja.
3. **Lisää korvaa lauseen** laskee kunkin lokin tason tyypin log4j rakennetaulukko esiintymät ja tallentaa tulosteen Blob-objektien Azuren tallennustilaan.


On kolme muuttujaa käytetään komentosarja:

- ${hiveTableName}
- ${hiveDataFolder}
- ${hiveOutputFolder}

Työnkulun määritystiedosto (workflow.xml Tässä opetusohjelmassa) välittää nämä arvot HiveQL komentosarjan suorituksen aikana.

Työnkulun tiedoston ja HiveQL-tiedosto on tallennettu blob-säilö.  PowerShell-komentosarjaa, jota käytät jäljempänä tässä opetusohjelmassa Kopioi molempien tiedostojen tallennustilan oletustilin. 

##<a name="submit-oozie-jobs-using-powershell"></a>Lähettää Oozie työt PowerShellin avulla

Azure PowerShell tällä hetkellä ei voi minkä tahansa cmdlet-komennot, jolla määritetään Oozie töitä. **Käynnistä RestMethod** cmdlet-komennon avulla voit käynnistää Oozie verkkopalvelut. Oozie web services-Ohjelmointirajapinnan on HTTP muiden JSON-Ohjelmointirajapinnan. Lisätietoja Oozie verkkopalvelut API [Apache Oozie 4.0] ohjeissa[ apache-oozie-400] (for HDInsight 3.0-versiota) tai [Apache Oozie 3.3.2 ohjeista] [ apache-oozie-332] (for HDInsight versio 2.1).

Tässä osassa PowerShell-komentosarjaa tekee seuraavat toimet:

1. Muodosta yhteys Azure.
2. Luo Azure resurssiryhmä. Lisätietoja on artikkelissa [Azure PowerShellin Azure resurssien hallinta](../powershell-azure-resource-manager.md).
3. Luo Azure SQL-tietokanta-palvelimeen, Azure SQL-tietokantaan ja kaksi taulukkoa. Näitä käytetään työnkulun Sqoop-toiminnolla.

    Taulukkonimi on *log4jLogCount*.

4. Luo käyttöä Oozie töitä varten HDInsight-klusterin.

    Tutkia klusterin, voit käyttää Azure portal tai PowerShellin Azure.

5. Kopioi oozie työnkulun tiedosto ja HiveQL-komentosarjatiedosto käyttöjärjestelmän.

    Molempien tiedostojen on tallennettu julkisen Blob-säilö.
    
    - Kopioi komentosarja HiveQL (useoozie.hql) Azuren tallennustilaan (wasbs:///tutorials/useoozie/useoozie.hql).
    - Kopioi workflow.xml wasbs:///tutorials/useoozie/workflow.xml.
    - Kopioi tiedot-tiedosto (/ example/data/sample.log), wasbs:///tutorials/useoozie/data/sample.log.
     
6. Lähetä Oozie työn.

    Tutkia OOzie työn tulokset Azure SQL-tietokannan yhdistäminen Visual Studiossa tai muiden työkalujen avulla.

Seuraavassa on komentosarja.  Voit suorittaa komentosarja Windows PowerShell ise:. Tarvitset vain määrittää ensin 7 muuttujat.

    #region - provide the following values
    
    $subscriptionID = "<Enter your Azure subscription ID>"
    
    # SQL Database server login credentials used for creating and connecting
    $sqlDatabaseLogin = "<Enter SQL Database Login Name>"
    $sqlDatabasePassword = "<Enter SQL Database Login Password>"
    
    # HDInsight cluster HTTP user credential used for creating and connectin
    $httpUserName = "admin"  # The default name is "admin"
    $httpPassword = "<Enter HDInsight Cluster HTTP User Password>"
    
    # Used for creating Azure service names
    $nameToken = "<Enter an Alias>"
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
    #endregion
    
    #region - variables
    
    # Resource group variables
    $resourceGroupName = $namePrefix + "rg"
    $location = "East US 2" # used by all Azure services defined in this tutorial
    
    # SQL database varialbes
    $sqlDatabaseServerName = $namePrefix + "sqldbserver"
    $sqlDatabaseName = $namePrefix + "sqldb"
    $sqlDatabaseConnectionString = "Data Source=$sqlDatabaseServerName.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $sqlDatabaseMaxSizeGB = 10
    
    # Used for retrieving external IP address and creating firewall rules
    $ipAddressRestService = "http://bot.whatismyipaddress.com"
    $fireWallRuleName = "UseSqoop"
    
    # HDInsight variables
    $hdinsightClusterName = $namePrefix + "hdi"
    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $hdinsightClusterName
    #endregion
    
    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"
    
    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{
        Login-AzureRmAccount
        Select-AzureRmSubscription -SubscriptionId $subscriptionID
    }
    #endregion
    
    #region - Create Azure resouce group
    Write-Host "`nCreating an Azure resource group ..." -ForegroundColor Green
    try{
        Get-AzureRmResourceGroup -Name $resourceGroupName
    }
    catch{
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    }
    #endregion
    
    #region - Create Azure SQL database server
    Write-Host "`nCreating an Azure SQL Database server ..." -ForegroundColor Green
    try{
        Get-AzureRmSqlServer -ServerName $sqlDatabaseServerName -ResourceGroupName $resourceGroupName}
    catch{
        Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green
    
        $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
        $sqlLoginCredentials = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)
    
        $sqlDatabaseServerName = (New-AzureRmSqlServer `
                                    -ResourceGroupName $resourceGroupName `
                                    -ServerName $sqlDatabaseServerName `
                                    -SqlAdministratorCredentials $sqlLoginCredentials `
                                    -Location $location).ServerName
        Write-Host "`tThe new SQL database server name is $sqlDatabaseServerName." -ForegroundColor Cyan
    
        Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
        $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-workstation" `
            -StartIpAddress $workstationIPAddress `
            -EndIpAddress $workstationIPAddress
    
        #To allow other Azure services to access the server add a firewall rule and set both the StartIpAddress and EndIpAddress to 0.0.0.0. 
        #Note that this allows Azure traffic from any Azure subscription to access the server.
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-Azureservices" `
            -StartIpAddress "0.0.0.0" `
            -EndIpAddress "0.0.0.0"
    }
    #endregion
    
    #region - Create and validate Azure SQL database
    Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
    
    try {
        Get-AzureRmSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName
    }
    catch {
        New-AzureRMSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName `
            -Edition "Standard" `
            -RequestedServiceObjectiveName "S1"
    }
    #endregion
    
    #region - Create SQL database tables
    Write-Host "Creating the log4jlogs table  ..." -ForegroundColor Green
    
    $sqlDatabaseTableName = "log4jLogsCount"
    $cmdCreateLog4jCountTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
            [Level] [nvarchar](10) NOT NULL,
            [Total] float,
        CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
        (
        [Level] ASC
        )
        )"
    
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = $sqlDatabaseConnectionString
    $conn.Open()
    
    # Create the log4jlogs table and index
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.Connection = $conn
    $cmd.CommandText = $cmdCreateLog4jCountTable
    $cmd.ExecuteNonQuery()
    
    $conn.close()
    #endregion
    
    #region - Create HDInsight cluster
    
    Write-Host "Creating the HDInsight cluster and the dependent services ..." -ForegroundColor Green
    
    # Create the default storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_LRS
    
    # Create the default Blob container
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                        -StorageAccountName $defaultStorageAccountName `
                                        -StorageAccountKey $defaultStorageAccountKey 
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext 
    
    # Create the HDInsight cluster
    $pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)
    
    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $HDInsightClusterName `
        -Location $location `
        -ClusterType Hadoop `
        -OSType Windows `
        -ClusterSizeInNodes 2 `
        -HttpCredential $httpCredential `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultBlobContainerName 
    
    # Validate the cluster
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName
    #endregion
    
    #region - copy Oozie workflow and HiveQL files
    
    Write-Host "Copy workflow definition and HiveQL script file ..." -ForegroundColor Green
    
    # Both files are stored in a public Blob
    $publicBlobContext = New-AzureStorageContext -StorageAccountName "hditutorialdata" -Anonymous
    
    # WASB folder for storing the Oozie tutorial files.
    $destFolder = "tutorials/useoozie"  # Do NOT use the long path here
    
    Start-CopyAzureStorageBlob `
        -Context $publicBlobContext `
        -SrcContainer "useoozie" `
        -SrcBlob "useooziewf.hql"  `
        -DestContext $defaultStorageAccountContext `
        -DestContainer $defaultBlobContainerName `
        -DestBlob "$destFolder/useooziewf.hql" `
        -Force
    
    Start-CopyAzureStorageBlob `
        -Context $publicBlobContext `
        -SrcContainer "useoozie" `
        -SrcBlob "workflow.xml"  `
        -DestContext $defaultStorageAccountContext `
        -DestContainer $defaultBlobContainerName `
        -DestBlob "$destFolder/workflow.xml" `
        -Force
    
    #validate the copy
    Get-AzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -Container $defaultBlobContainerName `
        -Blob $destFolder/workflow.xml
    
    Get-AzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -Container $defaultBlobContainerName `
        -Blob $destFolder/useooziewf.hql
    
    #endregion
    
    #region - copy the sample.log file
    
    Write-Host "Make a copy of the sample.log file ... " -ForegroundColor Green
    
    Start-CopyAzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -SrcContainer $defaultBlobContainerName `
        -SrcBlob "example/data/sample.log"  `
        -DestContext $defaultStorageAccountContext `
        -DestContainer $defaultBlobContainerName `
        -destBlob "$destFolder/data/sample.log" 
    
    #validate the copy
    Get-AzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -Container $defaultBlobContainerName `
        -Blob $destFolder/data/sample.log
    
    #endregion
    
    #region - submit Oozie job
    
    $storageUri="wasbs://$defaultBlobContainerName@$defaultStorageAccountName.blob.core.windows.net"
    
    $oozieJobName = $namePrefix + "OozieJob"
    
    #Oozie WF variables
    $oozieWFPath="$storageUri/tutorials/useoozie"  # The default name is workflow.xml. And you don't need to specify the file name.
    $waitTimeBetweenOozieJobStatusCheck=10
    
    #Hive action variables
    $hiveScript = "$storageUri/tutorials/useoozie/useooziewf.hql"
    $hiveTableName = "log4jlogs"
    $hiveDataFolder = "$storageUri/tutorials/useoozie/data"
    $hiveOutputFolder = "$storageUri/tutorials/useoozie/output"
    
    #Sqoop action variables
    $sqlDatabaseConnectionString = "jdbc:sqlserver://$sqlDatabaseServerName.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServerName;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    
    $OoziePayload =  @"
    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>
    
    <property>
        <name>nameNode</name>
        <value>$storageUrI</value>
    </property>
    
    <property>
        <name>jobTracker</name>
        <value>jobtrackerhost:9010</value>
    </property>
    
    <property>
        <name>queueName</name>
        <value>default</value>
    </property>
    
    <property>
        <name>oozie.use.system.libpath</name>
        <value>true</value>
    </property>
    
    <property>
        <name>hiveScript</name>
        <value>$hiveScript</value>
    </property>
    
    <property>
        <name>hiveTableName</name>
        <value>$hiveTableName</value>
    </property>
    
    <property>
        <name>hiveDataFolder</name>
        <value>$hiveDataFolder</value>
    </property>
    
    <property>
        <name>hiveOutputFolder</name>
        <value>$hiveOutputFolder</value>
    </property>
    
    <property>
        <name>sqlDatabaseConnectionString</name>
        <value>&quot;$sqlDatabaseConnectionString&quot;</value>
    </property>
    
    <property>
        <name>sqlDatabaseTableName</name>
        <value>$SQLDatabaseTableName</value>
    </property>
    
    <property>
        <name>user.name</name>
        <value>$httpUserName</value>
    </property>
    
    <property>
        <name>oozie.wf.application.path</name>
        <value>$oozieWFPath</value>
    </property>
    
    </configuration>
    "@
    
    Write-Host "Checking Oozie server status..." -ForegroundColor Green
    $clusterUriStatus = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/admin/status"
    $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $httpCredential -OutVariable $OozieServerStatus
    
    $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
    $oozieServerSatus = $jsonResponse[0].("systemMode")
    Write-Host "Oozie server status is $oozieServerSatus."
    
    # create Oozie job
    Write-Host "Sending the following Payload to the cluster:" -ForegroundColor Green
    Write-Host "`n--------`n$OoziePayload`n--------"
    $clusterUriCreateJob = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/jobs"
    $response = Invoke-RestMethod -Method Post -Uri $clusterUriCreateJob -Credential $httpCredential -Body $OoziePayload -ContentType "application/xml" -OutVariable $OozieJobName #-debug
    
    $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
    $oozieJobId = $jsonResponse[0].("id")
    Write-Host "Oozie job id is $oozieJobId..."
    
    # start Oozie job
    Write-Host "Starting the Oozie job $oozieJobId..." -ForegroundColor Green
    $clusterUriStartJob = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?action=start"
    $response = Invoke-RestMethod -Method Put -Uri $clusterUriStartJob -Credential $httpCredential | Format-Table -HideTableHeaders #-debug
    
    # get job status
    Write-Host "Sleeping for $waitTimeBetweenOozieJobStatusCheck seconds until the job metadata is populated in the Oozie metastore..." -ForegroundColor Green
    Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck
    
    Write-Host "Getting job status and waiting for the job to complete..." -ForegroundColor Green
    $clusterUriGetJobStatus = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?show=info"
    $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $httpCredential
    $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
    $JobStatus = $jsonResponse[0].("status")
    
    while($JobStatus -notmatch "SUCCEEDED|KILLED")
    {
        Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state...waiting $waitTimeBetweenOozieJobStatusCheck seconds for the job to complete..."
        Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $httpCredential
        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $JobStatus = $jsonResponse[0].("status")
        $jobStatus
    }
    
    Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state!" -ForegroundColor Green
    
    #endregion


**Jos haluat käyttää opetusohjelman**

Jos haluat suorittaa työnkulun uudelleen, sinun on poistettava seuraavasti:

- Rakenne-komentosarjan kohdetiedosto
- Log4jLogsCount-taulukon tiedot

Tässä on esimerkki PowerShell-komentosarjaa, joiden avulla voit:

    $resourceGroupName = "<AzureResourceGroupName>"
    
    $defaultStorageAccountName = "<AzureStorageAccountName>"
    $defaultBlobContainerName = "<ContainerName>"

    #SQL database variables
    $sqlDatabaseServerName = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabasePassword = "<SQLDatabaseLoginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"
    $sqlDatabaseTableName = "log4jLogsCount"

    Write-host "Delete the Hive script output file ..." -ForegroundColor Green
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                -ResourceGroupName $resourceGroupName `
                                -Name $defaultStorageAccountName)[0].Value
    $destContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
    Remove-AzureStorageBlob -Context $destContext -Blob "tutorials/useoozie/output/000000_0" -Container $defaultBlobContainerName

    Write-host "Delete all the records from the log4jLogsCount table ..." -ForegroundColor Green
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = "Data Source=$sqlDatabaseServerName.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $conn.open()
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.connection = $conn
    $cmd.commandtext = "delete from $sqlDatabaseTableName"
    $cmd.executenonquery()

    $conn.close()

##<a name="next-steps"></a>Seuraavat vaiheet
Tässä opetusohjelmassa opit Oozie työnkulun määrittäminen ja suorittaminen Oozie työn PowerShell-toiminnolla. Lisätietoja on seuraavissa artikkeleissa:

- [Aikapohjaisten Oozie koordinaattien käyttäminen Hdinsightiin][hdinsight-oozie-coordinator-time]
- [Aloita käyttäminen HDInsight-rakenne Hadoop analysointia mobile luuri käyttöä varten][hdinsight-get-started]
- [Azure-Blob-säiliö käyttäminen Hdinsightiin][hdinsight-storage]
- [Hallinta PowerShellin avulla Hdinsightiin][hdinsight-admin-powershell]
- [Lataa tiedot HDInsight Hadoop projekteille][hdinsight-upload-data]
- [Sqoop käyttäminen HDInsight Hadoop][hdinsight-use-sqoop]
- [Hadoop HDInsight-rakenteen käyttäminen][hdinsight-use-hive]
- [Possu käyttäminen Hadoop-Hdinsightiin][hdinsight-use-pig]
- [Kehitä Java MapReduce ohjelmien Hdinsightiin][hdinsight-develop-mapreduce]


[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563



[azure-data-factory-pig-hive]: ../data-factory/data-factory-data-transformation-activities.md
[hdinsight-oozie-coordinator-time]: hdinsight-use-oozie-coordinator-time.md
[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md


[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[sqldatabase-create-configue]: ../sql-database-create-configure.md
[sqldatabase-get-started]: ../sql-database-get-started.md

[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: ../storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: ../powershell-install-configure.md
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: https://technet.microsoft.com/en-us/library/ee176961.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.Preparation.Output1.png  
[img-runworkflow-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.RunWF.Output.png

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
