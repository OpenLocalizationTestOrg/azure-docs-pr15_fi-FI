<properties
    pageTitle="Luo Hadoop, HBase, myrsky tai ohjattu klustereiden PowerShellin Azure Hdinsightiin Linux | Microsoft Azure"
    description="Opettele Hadoop, HBase, myrsky tai ohjattu klustereiden luoda Linux HDInsight Azure PowerShell-toiminnolla."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/05/2016"
    ms.author="nitinme"/>

# <a name="create-linux-based-clusters-in-hdinsight-by-using-azure-powershell"></a>Luoda Linux-pohjaiset klustereiden käyttämällä PowerShellin Azure HDInsight

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Azure PowerShell on tehokas komentosarjat ympäristössä, joiden avulla voit automatisoida käyttöönotto- ja Microsoft Azure oman työmääriä hallinnointi ja hallita. Tässä asiakirjassa on tietoja siitä, miten voit luoda Linux-pohjaiset HDInsight-klusterin PowerShellin Azure. Se on myös esimerkki-komentosarjan.

> [AZURE.NOTE] Azure PowerShell on vain Windows-asiakkaiden käytettävissä. Jos käytössäsi on Linux-, Unix- tai Mac OS X-asiakas, saat lisätietoja Azure-CLI avulla voit luoda klusterin [luominen käyttämällä Azure CLI Linux-pohjaiset HDInsight-klusterin](hdinsight-hadoop-create-linux-clusters-azure-cli.md) .

## <a name="prerequisites"></a>Edellytykset
Sinulla on oltava seuraavat toimet ennen aloittamista seuraavasti:

- Azure tilaus. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- Azure PowerShell.
    Saat lisätietoja käyttäminen PowerShellin Azure Hdinsightiin [HDInsight hallinta PowerShellin avulla](hdinsight-administer-use-powershell.md). Katso HDInsight Windows PowerShellin cmdlet-komennot-luettelosta [HDInsight cmdlet-viittaus](https://msdn.microsoft.com/library/azure/dn858087.aspx).

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

### <a name="access-control-requirements"></a>Access-ohjausobjektin vaatimukset

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-clusters"></a>Klustereiden luominen

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Voit luoda PowerShellin Azure HDInsight-klusterin, sinun on suoritettava seuraavasti:

- Luo Azure resurssiryhmä
- Azure-tallennustilan tilin luominen
- Luo Azure-Blob-säilö
- HDInsight-klusterin luominen

Kaksi tärkeimmät parametrit, sinun on määritettävä luomaan Linux klustereiden ovat oikeat, Määritä OS ja SSH käyttäjätiedot:

- Varmista, että määrität **Linux** **- OSType** -parametrin.
- Voit määrittää SSH käyttäjän salasanan tai SSH julkisella avaimella, käytettävä varausyksiköiden remote istuntojen SSH. Jos määrität SSH käyttäjän salasanan ja SSH julkisella avaimella, avain ohitetaan. Jos haluat käyttää SSH käyttäjäavainten remote istunnoissa, määritettävä tyhjää SSH salasanaa pyydettäessä seuraavasti. Lisätietoja SSH käyttäminen Hdinsightista on seuraavissa artikkeleissa:

    * [Linux-pohjaiset Hadoop HDInsight Linux, Unix tai OS X-SSH käyttäminen](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [SSH käyttäminen Linux-pohjaiset Hadoop-HDInsight Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

Seuraavaa komentosarjaa kerrotaan, miten voit luoda uuden klusterin:

    $token ="<SpecifyAnUniqueString>"

    $resourceGroupName = $token + "rg"      # Provide a Resource Group name
    $clusterName = $token
    $defaultStorageAccountName = $token + "store"   # Provide a Storage account name
    $defaultStorageContainerName = $token + "container"
    $location = "East US 2"     # Change the location if needed
    $clusterNodes = 1           # The number of nodes in the HDInsight cluster

    # Sign in to Azure
    Login-AzureRmAccount

    # Select the subscription to use if you have multiple subscriptions
    #$subscriptionID = "<SubscriptionName>"        # Provide your Subscription Name
    #Select-AzureRmSubscription -SubscriptionId $subscriptionID

    # Create an Azure Resource Group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create an Azure Storage account and container used as the default storage
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -StorageAccountName $defaultStorageAccountName `
        -Location $location `
        -Type Standard_LRS
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -Name $defaultStorageAccountName -ResourceGroupName $resourceGroupName)[0].Key1
    $destContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer -Name $defaultStorageContainerName -Context $destContext

    # Create an HDInsight cluster
    $credentials = Get-Credential -Message "Enter Cluster user credentials" -UserName "admin"
    $sshCredentials = Get-Credential -Message "Enter SSH user credentials"

    # The location of the HDInsight cluster must be in the same data center as the Storage account.
    $location = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $defaultStorageAccountName | %{$_.Location}

    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -HttpCredential $credentials `
        -Location $location `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultStorageContainerName  `
        -ClusterSizeInNodes $clusterNodes `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.4" `
        -SshCredential $sshCredentials

Voit määrittää **$clusterCredentials** arvoja käytetään klusterin Hadoop-käyttäjätilin luominen. Tämän tilin avulla voit muodostaa yhteyttä klusterin.

Voit määrittää **$sshCredentials** arvoja käytetään SSH käyttäjän klusterin luomiseen. Tämän tilin avulla voit aloittaa SSH etäistunnon klusterin ja suorita työt.

> [AZURE.IMPORTANT] Tämä komentosarja on määritettävä, jotka ovat klusterin työntekijä solmujen määrän. Jos aiot käyttää useampaa kuin 32 työntekijä-solmut (joko klusterin luominen tai skaalauksen klusterin luonnin jälkeen), määritä myös pään solmun koon vähintään 8 Sydämiä ja 14 Gigatavua RAM-Muistia.
>
> Saat lisätietoja solmu kokojen ja niihin liittyvät kustannukset, [HDInsight hinnat](https://azure.microsoft.com/pricing/details/hdinsight/).

Voi kestää jopa 20 minuutin ajan klusterin luominen.

Seuraavassa esimerkissä esitetään, kuinka voit lisätä tallennustilaa tilin:

    # Create another storage account used as additional storage account
    $additionalStorageAccountName = $token + "store2"
    New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $additionalStorageAccountName -Location $location -Type Standard_LRS
    $additionalStorageAccountKey = (Get-AzureRmStorageAccountKey -Name $additionalStorageAccountName -ResourceGroupName $resourceGroupName)[0].Value

    $config = New-AzureRmHDInsightClusterConfig
    Add-AzureRmHDInsightStorage -Config $config -StorageAccountName "$additionalStorageAccountName.blob.core.windows.net" -StorageAccountKey $additionalStorageAccountKey

    # Create a new HDInsight cluster
    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -HttpCredential $credentials `
        -Location $location `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultStorageContainerName  `
        -ClusterSizeInNodes $clusterNodes `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.4" `
        -SshCredential $sshCredentials `
        -Config $config

## <a name="customize-clusters"></a>Klustereiden mukauttaminen

- Katso [mukauttaminen HDInsight klustereiden käyttämällä automaattinen](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).
- Katso [mukauttaa Windows-pohjaisesta HDInsight klustereiden komentosarja-toiminnon avulla](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).

## <a name="delete-the-cluster"></a>Klusterin poistaminen

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet luonut HDInsight-klusterin, avulla opit yhteyttä klusterin käsittelemisestä on seuraavissa resursseissa.

### <a name="hadoop-clusters"></a>Hadoop klustereiden

* [Rakenteen käyttäminen Hdinsightiin](hdinsight-use-hive.md)
* [Possu käyttäminen Hdinsightiin](hdinsight-use-pig.md)
* [MapReduce käyttäminen Hdinsightiin](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>HBase klustereiden

* [HBase HDInsight-käytön aloittaminen](hdinsight-hbase-tutorial-get-started-linux.md)
* [Kehittää Java HBase HDInsight-sovelluksia](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Myrsky klustereiden

* [Kehitä Java topologioissa myrsky HDInsight-](hdinsight-storm-develop-java-topology.md)
* [Myrsky HDInsight-Python osien käyttäminen](hdinsight-storm-develop-python-topology.md)
* [Ottaa käyttöön ja topologioissa myrsky HDInsight-ja seuranta](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="spark-clusters"></a>Ohjattu klustereiden

* [Luo erillisen-sovelluksen käyttäminen Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Suorita työt etäyhteyden käyttämällä Livy ohjattu klusterissa](hdinsight-apache-spark-livy-rest-interface.md)
* [Ohjattu BI: vuorovaikutteinen tietojen analysoinnissa ohjattu käyttäminen HDInsight kanssa Liiketoimintatieto-työkaluista](hdinsight-apache-spark-use-bi-tools.md)
* [Ohjattu koneen Learning kanssa: Käytä ohjattu elintarvikkeiden tulokset ennustetaan HDInsight-](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Ohjattu virtautetun median: Käytä ohjattu HDInsight reaaliaikainen streaming sovellusten luomiseen:](hdinsight-apache-spark-eventhub-streaming.md)
