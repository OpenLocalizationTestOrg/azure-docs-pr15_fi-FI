<properties
    pageTitle="Käytä Hadoop Sqoop HDInsight | Microsoft Azure"
    description="Opi käyttämään PowerShellin Azure työasemassa Sqoop Tuo ja vie Hadoop-klusterin ja Azure SQL-tietokantaan."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="jgao"/>

#<a name="use-sqoop-with-hadoop-in-hdinsight"></a>Sqoop käyttäminen HDInsight Hadoop

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Opi käyttämään Sqoop HDInsight tuomiseen ja viemiseen HDInsight-klusterin ja Azure SQL-tietokantaan tai SQL Server-tietokannan välillä.

Hadoop on luonnollinen valinta käsittelyyn rakenteeton ja semistructured tiedot, kuten lokit ja tiedostoja, mutta myös ehkä tarvitse käsitellä rakenteellisia tietoja, jotka on tallennettu relaatiotietokannasta.

[Sqoop] [ sqoop-user-guide-1.4.4] on suunniteltu tietojen siirtäminen Hadoop klustereiden ja relaatiotietokannasta välillä. Voit käyttää sitä tietojen tuominen relaatiotietokannasta hallintajärjestelmän (RDBMS), kuten SQL Server tai Oracle Hadoop distributed-tiedosto (HDFS)-järjestelmään MySQL Muunna Hadoop MapReduce tai rakenteen tiedot ja vie tiedot sitten takaisin RDBMS. Tässä opetusohjelmassa käytät SQL Server-tietokantaan relaatio tietokannan.

Katso Sqoop versioita, joita tuetaan HDInsight klustereiden [uudet HDInsight myöntämä klusterin versioissa?] [hdinsight-versions].

##<a name="understand-the-scenario"></a>Skenaario ymmärtäminen

HDInsight-klusterin sisältyy mallitietoja. Voit käyttää seuraavat kaksi esimerkit:

- Log4j lokitiedostoon, joka sijaitsee */example/data/sample.log*. Seuraavat lokit puretaan tiedostosta:

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

- Rakenne-taulukon *hivesampletable*, joka viittaa */hive/warehouse/hivesampletable*osoitteessa datatiedosto. Seuraavassa taulukossa on tietoja mobiililaitteeseen. 

  	| Kenttä | Tietotyyppi |
  	| ----- | --------- |
  	| clientid | merkkijono |
  	| querytime | merkkijono |
  	| Market | merkkijono |
  	| deviceplatform | merkkijono |
  	| devicemake | merkkijono |
  	| devicemodel | merkkijono |
  	| tila | merkkijono |
  	| maa | merkkijono |
  	| querydwelltime | Kaksinkertainen |
  	| istunto | bigint |
  	| sessionpagevieworder | bigint |

Ensin viedä *sample.log* ja *hivesampletable* Azure SQL-tietokantaan tai SQL Server ja tuo sitten taulukko, joka sisältää mobiililaitteen tiedot kohteeseen Hdinsightiin avulla seuraavasti:

    /tutorials/usesqoop/importeddata

## <a name="create-cluster-and-sql-database"></a>Klusterin ja SQL-tietokannan luominen

Tässä osassa näytetään, miten klusterin ja Azure portaalin ja Azure Resurssienhallinta-mallin avulla opetusohjelman suorittamista varten SQL tietokanta-mallien luomiseen.  Jos haluat käyttää PowerShellin Azure-kohdassa [lisäys A](#appendix-a---a-powershell-sample).

1. Valitse seuraavassa kuvassa resurssien hallinnan mallin avaaminen Azure-portaalissa.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fusesqoop%2Fcreate-linux-based-hadoop-cluster-in-hdinsight-and-sql-database.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    Resurssien hallinnan malli sijaitsee julkisen blob säilön, *https://hditutorialdata.blob.core.windows.net/usesqoop/create-linux-based-hadoop-cluster-in-hdinsight-and-sql-database.json*. 
    
    Resurssien hallinnan malli kutsuu bacpac-paketti, taulukko-mallit käyttöön SQL-tietokantaan.  Bacpac package sijaitsee myös julkisen blob säilön, https://hditutorialdata.blob.core.windows.net/usesqoop/SqoopTutorial-2016-2-23-11-2.bacpac. Jos haluat käyttää yksityinen säilön bacpac-tiedostoja, käytä seuraavia arvoja mallissa:
    
        "storageKeyType": "Primary",
        "storageKey": "<TheAzureStorageAccountKey>",
    
2. Valitse parametrit-sivu Anna seuraavat tiedot:

    - **ClusterName**: nimi, jonka aiot luoda Hadoop-klusterin.
    - **Klusterin käyttäjätunnus ja salasana**: Kirjautuminen oletusnimi on järjestelmänvalvoja.
    - **SSH käyttäjänimi ja salasana**.
    - **SQL-tietokanta palvelimen käyttäjätunnus ja salasana**.

    Seuraavat arvot ovat englanninkielisissä muuttujat-osassa:
    
  	|Oletus-tallennustilan tilin nimi|<CluterName>tallentaminen|
  	|----------------------------|-----------------|
  	|Azure SQL-tietokanta palvelimen nimi|<ClusterName>dbserver|
  	|Azure SQL-tietokannan nimi|<ClusterName>DB|
    
    Kirjoita muistiin nämä arvot.  Tarvitset niitä myöhemmin-opetusohjelman.
    
3 valitsemalla **OK** voit tallentaa parametrit.

4- **Mukautettu käyttöönotto** -sivu **resurssiryhmä** avattava luetteloruutu-ruutu ja valitse sitten Luo uusi resurssiryhmä **Uusi** . Resurssiryhmä on säilö, joka ryhmittelee klusterin, riippuvaiset tallennustilan tilin ja muut linkitetyn resurssi.

5 Valitse **ehdot**ja valitse sitten **Luo**.

6 valitsemalla **Luo**. Näkyviin tulee uusi ruutu liittyvä Submitting käyttöönoton mallin käyttöönottoa varten. Kestää noin 20 minuutin klusterin ja SQL-tietokannan luomiseen.

Jos haluat käyttää aiemmin luotuja Azure SQL-tietokantaan tai Microsoft SQL Server

- **Azure SQL-tietokantaan**: sinun on määritettävä Azure SQL-tietokantapalvelimen työaseman käyttö sallitaan palomuurisäännön. Lisätietoja luomisesta Azure SQL-tietokannan ja määrittäminen palomuuri, katso [käyttäminen Azure SQL-tietokantaan][sqldatabase-get-started]. 

    > [AZURE.NOTE] Oletusarvoisesti Azure SQL-tietokantaan avulla Azure-palvelut, kuten Azure Hdinsightiin yhteydet. Jos palomuuriasetus on poistettu käytöstä, se on käytössä Azure-portaalista. Katso lisätietoja Azure SQL-tietokannan luominen ja määrittäminen palomuurisäännöt-kohdassa [Create and määrittäminen SQL-tietokantaan][sqldatabase-create-configue].

- **SQL Server**: Jos HDInsight-klusterin on samassa verkossa virtual Azure-tietokannassa kuin SQL Server, voit tehdä ohjeita tämän artikkelin SQL Server-tietokannan tietojen vieminen ja tuominen.

    > [AZURE.NOTE] HDInsight tukee vain sijainti-virtual verkkojen ja sitä ei ole tällä hetkellä toimi affiniteetti ryhmän virtual verkkojen.

    * Voit luoda ja määrittää virtual verkko-kohdassa [Create virtual verkon Azure-portaalissa](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

        * Kun käytät SQL Server-että palvelinkeskukseen, sinun on määritettävä virtual verkon *sivusto sivusto* tai *pisteen sivuston*.

            > [AZURE.NOTE] **Pisteen sivuston** virtual verkoissa SQL Server on oltava käynnissä VPN-asiakasohjelman configuration-sovellus, joka on käytettävissä Azure virtual verkon asetusten **raporttinäkymät-ikkunan** .

        * Kun käytät SQL Server Azure virtuaalikoneen, minkä tahansa VPN-määritys voidaan Jos isännöinnin SQL Serverin virtuaalikoneen kuuluu virtual samassa verkossa kuin Hdinsightista.

    * Luomisesta HDInsight-klusterin virtual verkossa on artikkelissa [Luo Hadoop varausyksiköt HDInsight käyttämällä mukautettuja asetuksia](hdinsight-provision-clusters.md)

    > [AZURE.NOTE] SQL Server on myös määritettävä todennusta. Jotta voit suorittaa tämän artikkelin vaiheet, sinun on käytettävä SQL Server-kirjautuminen.


## <a name="run-sqoop-jobs"></a>Suorita Sqoop työt

HDInsight voit suorittaa Sqoop työt usealla eri tavalla. Seuraavan taulukon avulla voit päättää, mitä menetelmä sopii sinulle ja noudata vaiheittainen linkkiä.

| **Käytä tätä** , jos haluat...                                   | .. .an **Vuorovaikutteinen** käyttöliittymä | ... **Eräkäsittely** | .. .korvaava **klusterin käyttöjärjestelmä** | .. .from **Asiakkaan käyttöjärjestelmä** |
|:--------------------------------------------------------------|:---------------------------:|:-----------------------:|:------------------------------------------|:-----------------------------------------|
| [SSH](hdinsight-use-sqoop-mac-linux.md)                        |              ✔              |            ✔            | Linux                                     | Linux, Unix, Mac OS x: ssä tai Windows        |
| [.NET SDK Hadoop](hdinsight-hadoop-use-sqoop-dotnet-sdk.md) |           &nbsp;            |            ✔            | Linux- tai Windows                          | Windows (toistaiseksi)                        |
| [Azure PowerShell](hdinsight-hadoop-use-sqoop-powershell.md)  |           &nbsp;            |            ✔            | Linux- tai Windows                          | Windows                                  |

##<a name="limitations"></a>Rajoitukset

* Vie - ja Linux-pohjaiset HDInsight, Microsoft SQL Server tai Azure SQL-tietokannan tietojen vieminen Sqoop liittimen joukkona ei tue joukkona Lisää tällä hetkellä.

* Jonottaminen - Linux-pohjaiset HDInsight kanssa käytettäessä `-batch` vaihtaminen Lisää suoritettaessa, Sqoop suorittaa useita Lisää sijaan jonottaminen Lisää toimintoja.

##<a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet oppinut käyttämään Sqoop. Lisätietoja on artikkeleissa:

- [Rakenteen käyttäminen Hdinsightiin](hdinsight-use-hive.md)
- [Possu käyttäminen Hdinsightiin](hdinsight-use-pig.md)
- [Oozie käyttäminen HDInsight][hdinsight-use-oozie]: Oozie-työnkulun käyttäminen Sqoop-toiminto.
- [Analysoi viive lentotietoihin käyttämällä HDInsight][hdinsight-analyze-flight-data]: Käytä rakenne analysointiin lento viivyttää tiedot ja vie Azure SQL-tietokantaan Sqoop avulla.
- [Tietojen lataaminen HDInsight][hdinsight-upload-data]: Etsi tietojen lataaminen HDInsight/Azure-Blob-säiliö muilla tavoilla.


## <a name="appendix-a---a-powershell-sample"></a>Liite A - PowerShell-Esimerkki

PowerShell-esimerkki tekee seuraavat toimet:

1. Muodosta yhteys Azure.
2. Luo Azure resurssiryhmä. Lisätietoja on artikkelissa [Azure PowerShellin Azure resurssien hallinta](../powershell-azure-resource-manager.md)
3. Luo Azure SQL-tietokanta-palvelimeen, Azure SQL-tietokantaan ja kaksi taulukkoa. 

    Jos käytät SQL Server sen sijaan, voit luoda taulukoita seuraavista väittämistä:
    
        CREATE TABLE [dbo].[log4jlogs](
         [t1] [nvarchar](50),
         [t2] [nvarchar](50),
         [t3] [nvarchar](50),
         [t4] [nvarchar](50),
         [t5] [nvarchar](50),
         [t6] [nvarchar](50),
         [t7] [nvarchar](50))

        CREATE TABLE [dbo].[mobiledata](
         [clientid] [nvarchar](50),
         [querytime] [nvarchar](50),
         [market] [nvarchar](50),
         [deviceplatform] [nvarchar](50),
         [devicemake] [nvarchar](50),
         [devicemodel] [nvarchar](50),
         [state] [nvarchar](50),
         [country] [nvarchar](50),
         [querydwelltime] [float],
         [sessionid] [bigint],
         [sessionpagevieworder][bigint])

    Helpoin tapa tarkastella tietokanta ja taulukot on Visual Studiolla. Tietokantapalvelimen ja tietokannan voidaan tarkastaa Azure-portaalissa.

4. Luo HDInsight-klusterin.

    Tutkia klusterin, voit käyttää Azure portal tai PowerShellin Azure.

5. Esikäsittely lähde-datatiedosto.

    Tässä opetusohjelmassa Vie log4j lokitiedoston (erotinmerkkejä) ja rakenteen taulukon Azure SQL-tietokantaan. Erotinmerkkejä sisältävä tiedosto kutsutaan */example/data/sample.log*. Aiemmassa opetusohjelman näyttöön tuli log4j lokit muutamia esimerkkejä. Lokitiedosto on joitakin tyhjät rivit ja rivejä samalla nämä:
    
        java.lang.Exception: 2012-02-03 20:11:35 SampleClass2 [FATAL] unrecoverable system problem at id 609774657
            at com.osa.mocklogger.MockLogger$2.run(MockLogger.java:83)
    
    Tämä on hieno muita esimerkkejä, jotka käyttävät näitä tietoja, mutta olemme on poistettava poikkeukset, ennen kuin emme voi tuoda Azure SQL-tietokantaan tai SQL Server. Sqoop vienti epäonnistuu, jos on tyhjä merkkijono tai rivin, jossa vähemmän määrä elementtejä kuin määrä kenttiä, jotka on määritelty Azure SQL-tietokannan taulukkoon. Log4jlogs taulukossa on 7 merkkijono-tyypin kenttiä.

    Tämä toiminto luo uuden tiedoston klusterin: tutorials/usesqoop/data/sample.log. Muokatun tiedoston tutkia voit Azure portaalin, Azuren tallennustilaan-explorer-työkalua tai PowerShellin Azure. [Aloita HDInsight] [ hdinsight-get-started] on koodin otoksen PowerShellin Azure avulla voit ladata tiedoston ja näyttää tiedoston sisältöä.

6. Vieminen datatiedostoon Azure SQL-tietokantaan.

    Lähdetiedosto on tutorials/usesqoop/data/sample.log. Taulukko, johon tiedot viedään kutsutaan log4jlogs.
    
    > [AZURE.NOTE] Yhteysmerkkijonon tiedot, kuin tässä jaksossa pitäisi toimia Azure SQL-tietokantaan tai SQL Server. Nämä vaiheet on testattu käyttämällä asetukset ovat seuraavat:
    >
    > * **Azure virtual verkon pisteen sivuston määrittäminen**: virtual verkon yhteydessä HDInsight-klusterin SQL Server-yksityinen palvelinkeskukseen. Lisätietoja on kohdassa [Määritä pisteen sivuston VPN-verkon hallinta-portaalissa](../vpn-gateway/vpn-gateway-point-to-site-create.md) .
    > * **Azure Hdinsightiin 3.1**: [Luo Hadoop varausyksiköt käyttämällä mukautettuja asetuksia HDInsight](hdinsight-provision-clusters.md) Saat lisätietoja klusterin luominen virtual verkossa.
    > * **SQL Server 2014**: määritetty sallimaan todennus ja suorittamalla VPN-asiakasohjelman määritysten package muodostaminen näennäisen verkkoon suojatusti.

7. Rakenne-taulukon vieminen Azure SQL-tietokantaan.

8. Tuo mobiledata taulukon HDInsight-klusterin.

    Muokatun tiedoston tutkia voit Azure portaalin, Azuren tallennustilaan-explorer-työkalua tai PowerShellin Azure.  [Aloita HDInsight] [ hdinsight-get-started] on koodin otoksen PowerShellin Azure avulla voit ladata tiedoston ja näyttää tiedoston sisältöä.


### <a name="the-powershell-sample"></a>PowerShell-Esimerkki

    # Prepare an Azure SQL database to be used by the Sqoop tutorial
    
    #region - provide the following values
    
    $subscriptionID = "<Enter your Azure Subscription ID>"
    
    $sqlDatabaseLogin = "<Enter a SQL Database Login name>" #SQL Database server login
    $sqlDatabasePassword = "<Enter a Password>"
    
    $httpUserName = "admin"  #HDInsight cluster username
    $httpPassword = "<Enter a Password>"
    
    # used for creating Azure service names
    $nameToken = "<Enter an alias>" 
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
    
    # Used for creating tables and clustered indexes
    $cmdCreateLog4jTable = "CREATE TABLE [dbo].[log4jlogs](
        [t1] [nvarchar](50),
        [t2] [nvarchar](50),
        [t3] [nvarchar](50),
        [t4] [nvarchar](50),
        [t5] [nvarchar](50),
        [t6] [nvarchar](50),
        [t7] [nvarchar](50))"
    
    $cmdCreateLog4jClusteredIndex = "CREATE CLUSTERED INDEX log4jlogs_clustered_index on log4jlogs(t1)"
    
    $cmdCreateMobileTable = " CREATE TABLE [dbo].[mobiledata](
    [clientid] [nvarchar](50),
    [querytime] [nvarchar](50),
    [market] [nvarchar](50),
    [deviceplatform] [nvarchar](50),
    [devicemake] [nvarchar](50),
    [devicemodel] [nvarchar](50),
    [state] [nvarchar](50),
    [country] [nvarchar](50),
    [querydwelltime] [float],
    [sessionid] [bigint],
    [sessionpagevieworder][bigint])"
    
    $cmdCreateMobileDataClusteredIndex = "CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)"
    
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
    catch{Login-AzureRmAccount}
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
        $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)
    
        $sqlDatabaseServerName = (New-AzureRmSqlServer `
                                    -ResourceGroupName $resourceGroupName `
                                    -ServerName $sqlDatabaseServerName `
                                    -SqlAdministratorCredentials $credential `
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
    Write-Host "`nCreating an Azure SQL database ..." -ForegroundColor Green
    
    try {
        Get-AzureRmSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName
    }
    catch {
        Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
        New-AzureRMSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName `
            -Edition "Standard" `
            -RequestedServiceObjectiveName "S1"
    }
    
    #endregion
    
    #region - Create tables
    Write-Host "Creating the log4jlogs table and the mobiledata table ..." -ForegroundColor Green
    
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = $sqlDatabaseConnectionString
    $conn.Open()
    
    # Create the log4jlogs table and index
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.Connection = $conn
    $cmd.CommandText = $cmdCreateLog4jTable
    $ret = $cmd.ExecuteNonQuery()
    $cmd.CommandText = $cmdCreateLog4jClusteredIndex
    $cmd.ExecuteNonQuery()
    
    # Create the mobiledata table and index
    $cmd.CommandText = $cmdCreateMobileTable
    $cmd.ExecuteNonQuery()
    $cmd.CommandText = $cmdCreateMobileDataClusteredIndex
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
    
    #region - pre-process the source file
    
    Write-Host "Preprocessing the source file ..." -ForegroundColor Green
    
    # This procedure creates a new file with $destBlobName
    $sourceBlobName = "example/data/sample.log"
    $destBlobName = "tutorials/usesqoop/data/sample.log"
    
    # Define the connection string
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"
    
    # Create block blob objects referencing the source and destination blob.
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
    $sourceBlob = $storageContainer.GetBlockBlobReference($sourceBlobName)
    $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)
    
    # Define a MemoryStream and a StreamReader for reading from the source file
    $stream = New-Object System.IO.MemoryStream
    $stream = $sourceBlob.OpenRead()
    $sReader = New-Object System.IO.StreamReader($stream)
    
    # Define a MemoryStream and a StreamWriter for writing into the destination file
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream
    
    # Pre-process the source blob
    $exString = "java.lang.Exception:"
    while(-Not $sReader.EndOfStream){
        $line = $sReader.ReadLine()
        $split = $line.Split(" ")
    
        # remove the "java.lang.Exception" from the first element of the array
        # for example: java.lang.Exception: 2012-02-03 19:11:02 SampleClass8 [WARN] problem finding id 153454612
        if ($split[0] -eq $exString){
            #create a new ArrayList to remove $split[0]
            $newArray = [System.Collections.ArrayList] $split
            $newArray.Remove($exString)
    
            # update $split and $line
            $split = $newArray
            $line = $newArray -join(" ")
        }
    
        # remove the lines that has less than 7 elements
        if ($split.count -ge 7){
            write-host $line
            $writeStream.WriteLine($line)
        }
    }
    
    # Write to the destination blob
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $destBlob.UploadFromStream($memStream)
    
    #endregion
    
    #region - export a log file from the cluster to the SQL database
    
    Write-Host "Preprocessing the source file ..." -ForegroundColor Green
    
    $tableName_log4j = "log4jlogs"
    
    # Connection string for Azure SQL Database.
    # Comment if using SQL Server
    $connectionString = "jdbc:sqlserver://$sqlDatabaseServerName.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServerName;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    # Connection string for SQL Server.
    # Uncomment if using SQL Server.
    #$connectionString = "jdbc:sqlserver://$sqlDatabaseServerName;user=$sqlDatabaseLogin;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    
    $exportDir_log4j = "/tutorials/usesqoop/data"
    
    # Submit a Sqoop job
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_log4j --export-dir $exportDir_log4j --input-fields-terminated-by \0x20 -m 1"
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose
    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId
    
    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardError
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardOutput
    
    #endregion
    
    #region - export a Hive table
    
    $tableName_mobile = "mobiledata"
    $exportDir_mobile = "/hive/warehouse/hivesampletable"
    
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_mobile --export-dir $exportDir_mobile --fields-terminated-by \t -m 1"
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose
    
    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId
    
    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError
    
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardOutput
    
    #endregion
    
    #region - import a database
    
    $targetDir_mobile = "/tutorials/usesqoop/importeddata/"
    
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "import --connect $connectionString --table $tableName_mobile --target-dir $targetDir_mobile --fields-terminated-by \t --lines-terminated-by \n -m 1"
    
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose
    
    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId
    
    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError
    
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardOutput
    
    #endregion



[azure-management-portal]: https://portal.azure.com/

[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../sql-database/sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database/sql-database-get-started.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
