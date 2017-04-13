<properties
    pageTitle="Hallitse Hadoop varausyksiköt HDInsight PowerShellin avulla | Microsoft Azure"
    description="Lue, miten käyttämällä PowerShellin Azure Hdinsightiin Hadoop-klustereiden hallinnollisten tehtävien suorittamiseen."
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    tags="azure-portal"
    authors="mumian"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="jgao"/>

# <a name="manage-hadoop-clusters-in-hdinsight-by-using-azure-powershell"></a>Hallitse Hadoop varausyksiköt HDInsight Azure PowerShell-toiminnolla

[AZURE.INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Azure PowerShell on tehokas komentosarjat ympäristössä, joiden avulla voit hallita ja automatisoida käyttöönotto- ja Azure oman työmääriä hallinnointi. Tässä artikkelissa kerrotaan hallinnasta Hadoop varausyksiköt Azure Hdinsightiin paikallisen PowerShellin Azure-konsolin käyttämällä Windows PowerShellin avulla. HDInsight PowerShell-cmdlet-komentojen luetteloon, tutustu [HDInsight-cmdlet-viittaus][hdinsight-powershell-reference].



**Edellytykset**

Ennen kuin aloitat tämän artikkelin, sinulla on oltava seuraavasti:

- **Azure-tilaus**. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

##<a name="install-azure-powershell"></a>Azure PowerShellin asentaminen

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

Jos olet asentanut PowerShellin Azure versio 0,9 x, se on poistettava ennen asennusta uudemmalla versiolla.

Voit tarkistaa asennettu PowerShell-version seuraavasti:

    Get-Module *azure*
    
Poista vanhempi versio, suorita Ohjauspaneelin Ohjelmat ja toiminnot. 


##<a name="create-clusters"></a>Klustereiden luominen

Katso [luominen Linux-pohjaiset varausyksiköt PowerShellin Azure Hdinsightiin](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)

##<a name="list-clusters"></a>Luettelon klustereiden
Käytä seuraavaa komentoa luettelon kaikki varausyksiköt voimassa oleva tilaus:

    Get-AzureRmHDInsightCluster

##<a name="show-cluster"></a>Näytä klusterin

Seuraavalla komennolla voit näyttää tietoja tiettyyn klusteri voimassa oleva tilaus:

    Get-AzureRmHDInsightCluster -ClusterName <Cluster Name>

##<a name="delete-clusters"></a>Klustereiden poistaminen

Käytä seuraavaa komentoa klusterin poistaminen:

    Remove-AzureRmHDInsightCluster -ClusterName <Cluster Name>

Voit myös poistaa klusterin poistamalla, joka sisältää klusterin resurssiryhmä. Huomaa, tämä toiminto poistaa kaikki resurssit ryhmästä, mukaan lukien tallennustilan oletustilin.

    Remove-AzureRmResourceGroup -Name <Resource Group Name>
            
##<a name="scale-clusters"></a>Klustereiden asteikko
Klusterin skaalaus ominaisuuden avulla voit muuttaa klusteriin, joka toimii Azure Hdinsightiin eikä sinun tarvitse luoda uudelleen klusterin käyttämä työntekijä solmujen määrän.

>[AZURE.NOTE] Vain klusterit HDInsight version 3.1.3 kanssa tai uudempi versio tukee. Jos et ole varma, että klusterin-version, voit tarkistaa ominaisuudet-sivulla.  Katso [luettelo- ja Näytä klustereiden](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).

Vaikutukset HDInsight tukemat klusterin mistäkin tietojen solmujen määrän muuttaminen:

- Hadoop

    Voit suurentaa saumattomasti Hadoop-klusterin, jossa on käytössä ilman vaikuttavat odotetaan tai käynnissä töitä työntekijä näkyvien solmujen määrän. Uusi työt voidaan lähettää myös, kun toiminto on käynnissä. Skaalauksen toiminnon virheiden käsitellään tilanteen, niin, että klusterin aina jää toimintojen tilaan.

    Kun Hadoop-klusterin on skaalattu vähentämällä tietojen solmujen määrän-klusterin palvelut käynnistetään uudelleen. Tämä aiheuttaa kaikki käynnissä olevat ja odottavat työt epäonnistuu skaalauksen toiminnon päätyttyä. Voit kuitenkin tiedot työt, kun toiminto on valmis.

- HBase

    Saumattomasti voit lisätä tai poistaa solmujen HBase-klusteriin on käynnissä. Alueellisten palvelimet ovat automaattisesti saapuva skaalauksen toiminnon muutaman minuutin kuluessa. Voit myös manuaalisesti saldo alueellisen palvelimet klusterin headnode kirjautuminen ja suorittamalla seuraavat komennot komentokehote-ikkunassa:

        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer

- Myrsky

    Voit saumattomasti lisääminen tai poistaminen tietojen solmujen myrsky-klusteriin on käynnissä. Mutta onnistunut käyttöönotto skaalauksen toiminnan, kun tarvitset saattaa jälleen tasapainoon topologian.

    Yhdistelemällä onnistuu kahdella tavalla:

    * Myrsky web-Käyttöliittymä
    * Käyttöliittymä (CLI)-työkalu

    Katso lisätietoja [Apache myrsky ohjeissa](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) .

    Myrsky web-Käyttöliittymä on käytettävissä HDInsight-klusterin:

    ![HDInsight myrsky asteikko rebalance](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.storm.rebalance.png)

    Tässä on esimerkki saattaa jälleen tasapainoon myrsky topologian CLI-komennon avulla:

        ## Reconfigure the topology "mytopology" to use 5 worker processes,
        ## the spout "blue-spout" to use 3 executors, and
        ## the bolt "yellow-bolt" to use 10 executors

        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

Jos haluat muuttaa Hadoop klusteri Azure PowerShell-toiminnolla, suorita seuraava komento asiakastietokoneessa:

    Set-AzureRmHDInsightClusterSize -ClusterName <Cluster Name> -TargetInstanceCount <NewSize>
    

##<a name="grantrevoke-access"></a>Myönnä/revoke käyttö

Klustereiden Hdinsightista on verkkopalvelut seuraavat HTTP (kaikkia näistä palveluista on RESTful päätepisteet):

- ODBC
- JDBC
- Ambari
- Oozie
- Templeton


Oletusarvon mukaan palveluista myönnetään käytön. Voit voit peruuttaa/Myönnä käyttöoikeudet. Jos haluat peruuttaa:

    Revoke-AzureRmHDInsightHttpServicesAccess -ClusterName <Cluster Name>

Jos haluat myöntää:

    $clusterName = "<HDInsight Cluster Name>"

    # Credential option 1
    $hadoopUserName = "admin"
    $hadoopUserPassword = "<Enter the Password>"
    $hadoopUserPW = ConvertTo-SecureString -String $hadoopUserPassword -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential($hadoopUserName,$hadoopUserPW)

    # Credential option 2
    #$credential = Get-Credential -Message "Enter the HTTP username and password:" -UserName "admin"
    
    Grant-AzureRmHDInsightHttpServicesAccess -ClusterName $clusterName -HttpCredential $credential

>[AZURE.NOTE] Mukaan myöntämistä/peruutetaan käyttöoikeudet, voit palauttaa klusterin käyttäjänimi ja salasana.

Tämän voi tehdä myös portaalin kautta. Katso [Hallinta HDInsight käyttämällä Azure portaalin][hdinsight-admin-portal].

##<a name="update-http-user-credentials"></a>Päivitä HTTP käyttäjän tunnistetietoja

[Myönnä/revoke HTTP access](#grant/revoke-access)kuvattuja on. Jos klusterin on myönnetty HTTP-käyttö, se on ensin kumota.  Ja Myönnä sitten uusi HTTP-käyttäjätietoja käytön.


##<a name="find-the-default-storage-account"></a>Etsi tallennustilan oletustilin

Seuraavaa Powershell-komentosarjaa kerrotaan, miten tallennustilan oletusnimi ja klusterin tallennustilan-tilin avain.

    $clusterName = "<HDInsight Cluster Name>"
    
    $cluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $cluster.ResourceGroup
    $defaultStorageAccountName = ($cluster.DefaultStorageAccount).Replace(".blob.core.windows.net", "")
    $defaultBlobContainerName = $cluster.DefaultStorageContainer
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey 

##<a name="find-the-resource-group"></a>Etsi resurssiryhmän

Resurssienhallinta-tilassa kunkin HDInsight-klusterin kuuluu Azure resurssiryhmä.  Voit etsiä resurssiryhmän seuraavasti:

    $clusterName = "<HDInsight Cluster Name>"
    
    $cluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $cluster.ResourceGroup


##<a name="submit-jobs"></a>Lähettää työt

**Voit lähettää MapReduce työt**

On [Windows-pohjaisesta HDInsight-Suorita Hadoop MapReduce esimerkkejä](hdinsight-run-samples.md).

**Voit lähettää rakenteen työt** 

Kohdassa [Suorita rakenne kyselyjen PowerShellin avulla](hdinsight-hadoop-use-hive-powershell.md).

**Voit lähettää Possu työt**

[Suorita Possu työt PowerShellin avulla](hdinsight-hadoop-use-pig-powershell.md)on artikkelissa.

**Voit lähettää Sqoop työt**

Katso [Käytä Sqoop HDInsight kanssa](hdinsight-use-sqoop.md).

**Voit lähettää Oozie työt**

Katso [Käytä Oozie Hadoop määrittäminen ja HDInsight-työnkulun suorittaminen kanssa](hdinsight-use-oozie.md).

##<a name="upload-data-to-azure-blob-storage"></a>Tietojen lataaminen Azure-Blob-säiliö
Katso [Lataa tiedot HDInsight][hdinsight-upload-data].


## <a name="see-also"></a>Katso myös
* [HDInsight cmdlet-oppaat][hdinsight-powershell-reference]
* [Hallita HDInsight Azure-portaalissa][hdinsight-admin-portal]
* [Hallita HDInsight käyttämällä käyttöliittymä][hdinsight-admin-cli]
* [Luo HDInsight klustereiden][hdinsight-provision]
* [Tietojen lataaminen Hdinsightiin][hdinsight-upload-data]
* [Lähettää Hadoop työt ohjelmallisesti][hdinsight-submit-jobs]
* [Azure Hdinsightiin käytön aloittaminen][hdinsight-get-started]


[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-provision-custom-options]: hdinsight-provision-clusters.md#configuration
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-flight]: hdinsight-analyze-flight-delay-data.md

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx

[powershell-install-configure]: powershell-install-configure.md

[image-hdi-ps-provision]: ./media/hdinsight-administer-use-powershell/HDI.PS.Provision.png
