<properties
    pageTitle="Mukauta HDInsight klustereiden käyttämällä automaattinen | Microsoft Azure"
    description="Opettele mukauttamaan HDInsight klustereiden käyttämällä automaattinen."
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="jgao"/>

# <a name="customize-hdinsight-clusters-using-bootstrap"></a>Käyttämällä automaattinen HDInsight klustereiden mukauttaminen

Joskus haluat määrittää määritys-tiedostoja, jotka sisältävät:

- clusterIdentity.xml
- Core site.xml
- Gateway.XML
- hbase env.xml
- hbase site.xml
- hdfs site.xml
- rakenne-env.xml
- rakenne-site.xml
- mapred-sivusto
- oozie site.xml
- oozie env.xml
- myrsky site.xml
- tez site.xml
- webhcat site.xml
- kuitenkaan site.xml

Varausyksiköt ei voi säilyttää muutokset vuoksi imaging uudelleen. Saat lisätietoja uudelleen imaging- [Roolin esiintymän käynnistyy määräpäivän OS päivityksiä](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx). Voit säilyttää muutokset varausyksiköiden elinaika kautta voit käyttää HDInsight-klusterin mukauttaminen luomisen aikana. Tämä on suositeltu tapaa muuttaa klusterin määritykset ja Azure reimage Käynnistä uudelleen näitä tapahtumia säily. Näiden määritysten tehdyt muutokset tulevat voimaan ennen palvelun Käynnistä-painiketta, niin palvelut eivät tarvitse käynnistää uudelleen. 

Jos haluat käyttää Automaattinen 3 tavoilla:

- Azure PowerShellin käyttäminen

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]
    
- Käyttämällä .NET SDK-paketissa
- Azure Resurssienhallinta mallin käyttäminen

Lisätietoja lisäosat asennetaan HDInsight-klusterin luomisen yhteydessä on artikkelissa:

- [Mukauta HDInsight klustereiden komentosarja-toiminnolla (Linux)](hdinsight-hadoop-customize-cluster-linux.md)
- [Mukauta HDInsight klustereiden komentosarja-toiminnolla (Windows)](hdinsight-hadoop-customize-cluster.md)

## <a name="use-azure-powershell"></a>Azure PowerShellin käyttäminen

Seuraavan PowerShell-koodin mukauttaa rakenteen määritys:

    # hive-site.xml configuration
    $hiveConfigValues = @{ "hive.metastore.client.socket.timeout"="90" }
    
    $config = New-AzureRmHDInsightClusterConfig `
        | Set-AzureRmHDInsightDefaultStorage `
            -StorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
            -StorageAccountKey $defaultStorageAccountKey `
        | Add-AzureRmHDInsightConfigValues `
            -HiveSite $hiveConfigValues 
    
    New-AzureRmHDInsightCluster `
        -ResourceGroupName $existingResourceGroupName `
        -ClusterName $clusterName `
        -Location $location `
        -ClusterSizeInNodes $clusterSizeInNodes `
        -ClusterType Hadoop `
        -OSType Windows `
        -Version "3.2" `
        -HttpCredential $httpCredential `
        -Config $config 

Valmis toimiva PowerShell-komentosarjaa löytyvät [Lisäys A](#hdinsight-hadoop-customize-cluster-bootstrap.md/appx-a:-powershell-sample).

**Vahvista muutos:**

1. Kirjaudu [Azure portal](https://portal.azure.com).
2. Valitse vasemmanpuoleisessa ruudussa valitsemalla **Selaa**ja valitse sitten **HDInsight klustereiden**.
3. Valitse juuri luomasi käyttämällä PowerShell-komentosarjaa klusterin.
4. Valitse **Dashboard** sivu yläreunasta Ambari Käyttöliittymä avautuu.
5. Valitse vasemmasta valikosta **rakenne** .
6. Valitse **HiveServer2** **yhteenvetoon**.
7. Valitse **kokoonpanomääritysten yhteydessä** -välilehti.
8. Valitse vasemmasta valikosta **rakenne** .
9. Valitse **Lisäasetukset** -välilehti.
10. Vieritä alas ja laajenna sitten **Lisäasetukset-rakenne-sivusto**.
11. Etsi **hive.metastore.client.socket.timeout** -osassa.

Jotkin Lisää esimerkit mukauttamisesta määritysten muita tiedostoja:

    # hdfs-site.xml configuration
    $HdfsConfigValues = @{ "dfs.blocksize"="64m" } #default is 128MB in HDI 3.0 and 256MB in HDI 2.1

    # core-site.xml configuration
    $CoreConfigValues = @{ "ipc.client.connect.max.retries"="60" } #default 50

    # mapred-site.xml configuration
    $MapRedConfigValues = @{ "mapreduce.task.timeout"="1200000" } #default 600000

    # oozie-site.xml configuration
    $OozieConfigValues = @{ "oozie.service.coord.normal.default.timeout"="150" }  # default 120

Lisätietoja on artikkelissa Azim Uddin blogi liittyvä [mukauttaminen HDInsight-klusterin luominen](http://blogs.msdn.com/b/bigdatasupport/archive/2014/04/15/customizing-hdinsight-cluster-provisioning-via-powershell-and-net-sdk.aspx).

## <a name="use-net-sdk"></a>Käyttämällä .NET SDK-paketissa

Katso [luominen Linux-pohjaiset varausyksiköt käyttämällä .NET SDK Hdinsightista](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-bootstrap).

## <a name="use-resource-manager-template"></a>Resurssien hallinnan käyttö malli

Voit käyttää Automaattinen Resurssienhallinta mallissa:

    "configurations": {
        …
        "hive-site": {
            "hive.metastore.client.connect.retry.delay": "5",
            "hive.execution.engine": "mr",
            "hive.security.authorization.manager": "org.apache.hadoop.hive.ql.security.authorization.DefaultHiveAuthorizationProvider"
        }
    }


![hdinsight hadoop mukauttaminen klusterin käynnistyksen azure resurssien hallinnan malli](./media/hdinsight-hadoop-customize-cluster-bootstrap/hdinsight-customize-cluster-bootstrap-arm.png)



## <a name="see-also"></a>Katso myös

- [Luo Hadoop klustereiden HDInsight] [ hdinsight-provision-cluster] ohjeet HDInsight-klusterin luominen muiden mukautettujen asetusten avulla.
- [Kehitä komentosarja-toiminnon komentosarjojen Hdinsightiin][hdinsight-write-script]
- [Asentaminen ja käyttäminen ohjattu HDInsight klustereiden][hdinsight-install-spark]
- [Asentaminen ja käyttäminen HDInsight klustereiden R][hdinsight-install-r]
- [Asentaminen ja käyttäminen Solr HDInsight-klusterit](hdinsight-hadoop-solr-install.md).
- [Asentaminen ja käyttäminen Giraph HDInsight-klusterit](hdinsight-hadoop-giraph-install.md).

[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-write-script]: hdinsight-hadoop-script-actions.md
[hdinsight-provision-cluster]: hdinsight-provision-clusters.md
[powershell-install-configure]: powershell-install-configure.md


[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Vaiheet klusterin luonnin aikana"

## <a name="appx-a-powershell-sample"></a>Appx A: PowerShell-Esimerkki

Tämä PowerShell-komentosarjaa Luo HDInsight-klusterin ja mukauttaa rakenne-asetusta:

    ####################################
    # Set these variables
    ####################################
    #region - used for creating Azure service names
    $nameToken = "<ENTER AN ALIAS>" 
    #endregion

    #region - cluster user accounts
    $httpUserName = "admin"  #HDInsight cluster username
    $httpPassword = "<ENTER A PASSWORD>" #"<Enter a Password>"

    $sshUserName = "sshuser" #HDInsight ssh user name
    $sshPassword = "<ENTER A PASSWORD>" #"<Enter a Password>"
    #endregion

    ####################################
    # Service names and varialbes
    ####################################
    #region - service names
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

    $resourceGroupName = $namePrefix + "rg"
    $hdinsightClusterName = $namePrefix + "hdi"
    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $hdinsightClusterName

    $location = "East US 2"
    #endregion

    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    ####################################
    # Connect to Azure
    ####################################
    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion

    #region - Create an HDInsight cluster
    ####################################
    # Create dependent components
    ####################################
    Write-Host "Creating a resource group ..." -ForegroundColor Green
    New-AzureRmResourceGroup `
        -Name  $resourceGroupName `
        -Location $location

    Write-Host "Creating the default storage account and default blob container ..."  -ForegroundColor Green
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_GRS

    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageContext #use the cluster name as the container name

    ####################################
    # Create a configuration object
    ####################################
    $hiveConfigValues = @{ "hive.metastore.client.socket.timeout"="90" }
        
    $config = New-AzureRmHDInsightClusterConfig `
        | Set-AzureRmHDInsightDefaultStorage `
            -StorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
            -StorageAccountKey $defaultStorageAccountKey `
        | Add-AzureRmHDInsightConfigValues `
            -HiveSite $hiveConfigValues 

    ####################################
    # Create an HDInsight cluster
    ####################################
    $httpPW = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$httpPW)

    $sshPW = ConvertTo-SecureString -String $sshPassword -AsPlainText -Force
    $sshCredential = New-Object System.Management.Automation.PSCredential($sshUserName,$sshPW)

    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -Location $location `
        -ClusterSizeInNodes 1 `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.2" `
        -HttpCredential $httpCredential `
        -SshCredential $sshCredential `
        -Config $config

    ####################################
    # Verify the cluster
    ####################################
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName

    #endregion
