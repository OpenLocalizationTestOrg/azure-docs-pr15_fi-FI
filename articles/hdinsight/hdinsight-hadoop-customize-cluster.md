<properties
    pageTitle="Mukauta HDInsight klustereiden komentosarja-toimintojen käyttäminen | Microsoft Azure"
    description="Opettele mukauttamaan HDInsight klustereiden komentosarja-toiminnon avulla."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="nitinme"/>

# <a name="customize-windows-based-hdinsight-clusters-using-script-action"></a>Mukauta Windows-pohjaisesta HDInsight klustereiden komentosarja-toiminnon käyttäminen

**Komentosarja-toiminnon** avulla voidaan käynnistää [komentosarjat](hdinsight-hadoop-script-actions.md) klusterin luontiprosessi lisäohjelmistoa asentamisessa klusterin aikana.

Tämän artikkelin tiedot on Windows-pohjaisesta HDInsight klustereiden. Katso Linux-pohjaiset klustereiden [mukauttaminen Linux-pohjaiset HDInsight klustereiden komentosarja-toiminnon avulla](hdinsight-hadoop-customize-cluster-linux.md). 

HDInsight klustereiden voi mukauttaa erilaisia käyttötapoja sekä, kuten mukaan lukien Azuren tallennustilaan tilejä, muuttaminen Hadoop määritys-tiedostot (core site.xml, rakenteen site.xml jne.) tai kirjastojen (esimerkiksi rakenne, Oozie) lisääminen jaetut yleiset sijainnit klusterin kyselyjä. Nämä mukautukset voidaan toteuttaa PowerShellin Azure, Azure Hdinsightiin .NET SDK ja Azure-portaalissa. Lisätietoja on artikkelissa [Luo Hadoop varausyksiköt HDInsight][hdinsight-provision-cluster].

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell-cli-and-dotnet-sdk.md)]

## <a name="script-action-in-the-cluster-creation-process"></a>Klusterin luontiprosessi komentosarja-toiminto

Komentosarja-toiminto on käytössä vain klustereiden ollessa luomisen yhteydessä. Seuraavassa kaaviossa on kuvattu kun komentosarja-toiminnon suoritetaan luomisen aikana:

![HDInsight-klusterin mukauttaminen ja vaiheet klusterin luonnin aikana][img-hdi-cluster-states]

Kun komentosarja on käynnissä, klusterin Lisää **ClusterCustomization** vaiheen. Tässä vaiheessa komentosarja suoritetaan järjestelmän järjestelmänvalvojan tili-rinnakkain määritetyn solmuissa klusterin-kohdasta ja tarjoaa solmut koko järjestelmänvalvojan oikeuksia.

> [AZURE.NOTE] Koska sinulla järjestelmänvalvojan oikeuksia klusterisolmut **ClusterCustomization** vaiheen aikana, voit suorittaa toimintoja, kuten pysäyttämisestä ja palveluja, kuten Hadoop liittyviä palveluja aloitetaan komentosarja. Siten, tietyn komentosarja sinun on varmistettava, että Ambari palvelut ja muut Hadoop liittyvät palvelut ovat käytön ennen komentosarjan on päättynyt. Näiden palvelujen tarvitaan selvittämiseksi onnistuneesti terveys- ja tilan klusterin, kun se luodaan. Jos muutat kokoonpanoa klusterin, joka vaikuttaa palveluun, sinun on käytettävä hakuja avustaja-funktioita. Saat lisätietoja funktioita avustaja [kehittää komentosarja-toiminnon komentosarjat HDInsight][hdinsight-write-script].

Tulos ja komentosarja virhelokien on tallennettu klusterin määritetylle tallennustilan oletustilin. Taulukko, jossa nimi on tallennettu lokit **u < \cluster-name-fragment >< \time-stamp > setuplog**. Nämä ovat kooste lokit komentosarjan suorittaminen kaikissa solmuissa (pään solmu ja työntekijä solmujen) klusterin.

Kunkin klusterin hyväksyä useita komentosarjatoimintoja, jotka aktivoidaan siinä järjestyksessä, jossa ne on määritetty. Komentosarjan on suorittanut pään solmu, työntekijä solmut tai molemmat.

Hdinsightista on useita komentosarjojen asentaminen HDInsight klustereiden seuraavat osat:

Nimi | Komentosarja
----- | -----
**Ohjattu asennus** | https://hdiconfigactions.BLOB.Core.Windows.NET/sparkconfigactionv03/spark-Installer-v03.ps1. Katso [asentaminen ja käyttäminen tiedostojen HDInsight klustereiden][hdinsight-install-spark].
**Asenna R** | https://hdiconfigactions.BLOB.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1. [Asentaminen]ja käyttäminen R-HDInsight klustereiden[hdinsight-install-r].
**Asenna Solr** | https://hdiconfigactions.BLOB.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1. Katso [asentaminen ja käyttäminen Solr HDInsight-klusterit](hdinsight-hadoop-solr-install.md).
- **Asenna Giraph** | https://hdiconfigactions.BLOB.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1. Katso [asentaminen ja käyttäminen Giraph HDInsight-klusterit](hdinsight-hadoop-giraph-install.md).
| **Lataa valmiiksi rakenne-kirjastot** | https://hdiconfigactions.BLOB.Core.Windows.NET/setupcustomhivelibsv01/Setup-customhivelibs-v01.ps1. Katso [HDInsight klustereiden Lisää rakenne kirjastoihin](hdinsight-hadoop-add-hive-libraries.md) |


## <a name="call-scripts-using-the-azure-portal"></a>Puhelun komentosarjojen Azure-portaalissa

**Azure-portaalista**

1. Aloita luominen klusterin [luominen Hadoop varausyksiköt HDInsight](hdinsight-provision-clusters.md#portal)-palvelussa kuvatulla.
2. Valinnainen kokoonpano- **Komentosarjatoiminnot** -sivu, kohdasta tietojen näyttäminen komentosarja-toiminnon **lisääminen komentosarja-toiminnon** alla kuvatulla tavalla:

    ![Käytä komentosarja-toiminnon klusterin mukauttamiseen] (./media/hdinsight-hadoop-customize-cluster/HDI.CreateCluster.8.png "Käytä komentosarja-toiminnon klusterin mukauttamiseen")

    <table border='1'>
        <tr><th>Ominaisuus</th><th>Arvo</th></tr>
        <tr><td>Nimi</td>
            <td>Määritä komentosarja-toiminnon nimi.</td></tr>
        <tr><td>Komentosarjan URI</td>
            <td>Määritä komentosarja, joka käynnistyy, voit mukauttaa klusterin URI. s</td></tr>
        <tr><td>Pää-/ työntekijä</td>
            <td>Määritä solmut (**otsikko** tai **Työntekijä**) joka mukauttaminen komentosarja suoritetaan. </b>.
        <tr><td>Parametrit</td>
            <td>Määrittää parametreja, jos vaatii komentosarja.</td></tr>
    </table>

    Paina ENTER voit lisätä useita osia asennetaan klusterin useita komentosarja-toiminnon.

3. Valitse **Valitse** komentosarja-toiminnon määritys Tallenna ja jatka klusterin luominen.

## <a name="call-scripts-using-azure-powershell"></a>Puhelun komentosarjojen Azure PowerShellin avulla

Tämä seuraavaa PowerShell-komentosarjaa esitellään ohjattu asentamisesta Windows perustuvat HDInsight-klusterin.  

    # Provide values for these variables
    $subscriptionID = "<Azure Suscription ID>" # After "Login-AzureRmAccount", use "Get-AzureRmSubscription" to list IDs.

    $nameToken = "<Enter A Name Token>"  # The token is use to create Azure service names.
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
    
    $resourceGroupName = $namePrefix + "rg"
    $location = "EAST US 2" # used for creating resource group, storage account, and HDInsight cluster.
    
    $hdinsightClusterName = $namePrefix + "spark"
    $httpUserName = "admin"
    $httpPassword = "<Enter a Password>"
    
    $defaultStorageAccountName = "$namePrefix" + "store"
    $defaultBlobContainerName = $hdinsightClusterName
    
    #############################################################
    # Connect to Azure
    #############################################################
    
    Try{
        Get-AzureRmSubscription
    }
    Catch{
        Login-AzureRmAccount
    }
    Select-AzureRmSubscription -SubscriptionId $subscriptionID
    
    #############################################################
    # Prepare the dependent components
    #############################################################
    
    # Create resource group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    
    # Create storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_GRS
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext
    
    #############################################################
    # Create cluster with ApacheSpark
    #############################################################
    
    # Specify the configuration options
    $config = New-AzureRmHDInsightClusterConfig `
                -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
                -DefaultStorageAccountKey $defaultStorageAccountKey 
                
    
    # Add a script action to the cluster configuration
    $config = Add-AzureRmHDInsightScriptAction `
                -Config $config `
                -Name "Install Spark" `
                -NodeType HeadNode `
                -Uri https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1 `
    
    # Start creating a cluster with Spark installed
    New-AzureRmHDInsightCluster `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $hdinsightClusterName `
            -Location $location `
            -ClusterSizeInNodes 2 `
            -ClusterType Hadoop `
            -OSType Windows `
            -DefaultStorageContainer $defaultBlobContainerName `
            -Config $config


Jos haluat asentaa muiden ohjelmien, tarvitset korvaa komentosarja script-tiedosto:


Kirjoita pyydettäessä klusterin tunnistetiedot. Voi kestää hetken, ennen kuin klusterin luodaan.

## <a name="call-scripts-using-net-sdk"></a>Käyttämällä .NET SDK komentosarjat 

Seuraavassa esimerkissä ohjattu asentamisesta Windows perustuvat HDInsight-klusterin. Jos haluat asentaa muiden ohjelmien, sinun on korvaa koodin komentosarjatiedosto.

**Luo HDInsight-klusterin ohjattu** 

1. C#-konsolisovelluksen luominen Visual Studio.
2. Nuget pakettien hallinta-konsolin varten suorittamalla seuraavan komennon.

        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
        Install-Package Microsoft.Azure.Management.ResourceManager -Pre
        Install-Package Microsoft.Azure.Management.HDInsight

2. Käytä seuraavia lauseiden käyttäminen Program.cs-tiedosto:

        using System;
        using System.Security;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.HDInsight;
        using Microsoft.Azure.Management.HDInsight.Models;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;

3. Sijoittaa koodin luokan kanssa seuraavasti:

        private static HDInsightManagementClient _hdiManagementClient;

        // Replace with your AAD tenant ID if necessary
        private const string TenantId = UserTokenProvider.CommonTenantId; 
        private const string SubscriptionId = "<Your Azure Subscription ID>";
        // This is the GUID for the PowerShell client. Used for interactive logins in this example.
        private const string ClientId = "1950a258-227b-4e31-a9cf-717495945fc2";
        private const string ResourceGroupName = "<ExistingAzureResourceGroupName>";
        private const string NewClusterName = "<NewAzureHDInsightClusterName>";
        private const int NewClusterNumWorkerNodes = 2;
        private const string NewClusterLocation = "East US";
        private const string NewClusterVersion = "3.2";
        private const string ExistingStorageName = "<ExistingAzureStorageAccountName>";
        private const string ExistingStorageKey = "<ExistingAzureStorageAccountKey>";
        private const string ExistingContainer = "<ExistingAzureBlobStorageContainer>";
        private const string NewClusterType = "Hadoop";
        private const OSType NewClusterOSType = OSType.Windows;
        private const string NewClusterUsername = "<HttpUserName>";
        private const string NewClusterPassword = "<HttpUserPassword>";

        static void Main(string[] args)
        {
            System.Console.WriteLine("Running");

            // Authenticate and get a token
            var authToken = Authenticate(TenantId, ClientId, SubscriptionId);
            // Flag subscription for HDInsight, if it isn't already.
            EnableHDInsight(authToken);
            // Get an HDInsight management client
            _hdiManagementClient = new HDInsightManagementClient(authToken);

            CreateCluster();
        }

        private static void CreateCluster()
        {
            var parameters = new ClusterCreateParameters
            {
                ClusterSizeInNodes = NewClusterNumWorkerNodes,
                Location = NewClusterLocation,
                ClusterType = NewClusterType,
                OSType = NewClusterOSType,
                Version = NewClusterVersion,

                DefaultStorageAccountName = ExistingStorageName,
                DefaultStorageAccountKey = ExistingStorageKey,
                DefaultStorageContainer = ExistingContainer,

                UserName = NewClusterUsername,
                Password = NewClusterPassword,
            };

            ScriptAction sparkScriptAction = new ScriptAction("Install Spark",
                new Uri("https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1"), "");

            parameters.ScriptActions.Add(ClusterNodeType.HeadNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });
            parameters.ScriptActions.Add(ClusterNodeType.WorkerNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });

            _hdiManagementClient.Clusters.Create(ResourceGroupName, NewClusterName, parameters);
        }

        /// <summary>
        /// Authenticate to an Azure subscription and retrieve an authentication token
        /// </summary>
        /// <param name="TenantId">The AAD tenant ID</param>
        /// <param name="ClientId">The AAD client ID</param>
        /// <param name="SubscriptionId">The Azure subscription ID</param>
        /// <returns></returns>
        static TokenCloudCredentials Authenticate(string TenantId, string ClientId, string SubscriptionId)
        {
            var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + TenantId);
            var tokenAuthResult = authContext.AcquireToken("https://management.core.windows.net/", 
                ClientId, 
                new Uri("urn:ietf:wg:oauth:2.0:oob"), 
                PromptBehavior.Always, 
                UserIdentifier.AnyUser);
            return new TokenCloudCredentials(SubscriptionId, tokenAuthResult.AccessToken);
        }
        /// <summary>
        /// Marks your subscription as one that can use HDInsight, if it has not already been marked as such.
        /// </summary>
        /// <remarks>This is essentially a one-time action; if you have already done something with HDInsight
        /// on your subscription, then this isn't needed at all and will do nothing.</remarks>
        /// <param name="authToken">An authentication token for your Azure subscription</param>
        static void EnableHDInsight(TokenCloudCredentials authToken)
        {
            // Create a client for the Resource manager and set the subscription ID
            var resourceManagementClient = new ResourceManagementClient(new TokenCredentials(authToken.Token));
            resourceManagementClient.SubscriptionId = SubscriptionId;
            // Register the HDInsight provider
            var rpResult = resourceManagementClient.Providers.Register("Microsoft.HDInsight");
        }


4. Painamalla **F5** sovelluksen käyttämiseen.


## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>Avaa lähde-ohjelmiston HDInsight klustereiden käytettyyn tuki
Microsoft Azure HDInsight-palvelu on joustava ympäristö, jonka avulla voit luoda big datasta sovelluksia pilvipalveluun käyttämällä ekosysteemiin Avaa lähde-tekniikoiden muotoiltu Hadoop ympärille. Microsoft Azure tarjoaa tuen Yleiset taso Avaa lähde tekniikoita, kuten edellä <a href="http://azure.microsoft.com/support/faq/" target="_blank">Azure tukevat usein kysytyt kysymykset sivuston</a> **Tukevat oletusalue** -osassa. HDInsight-palvelu sisältää tuki joitakin osia, Lisää taso seuraavalla tavalla.

Avaa lähde-osat, jotka ovat käytettävissä HDInsight-palvelussa on kahdenlaisia:

- **Valmiit osat** - komponentit on asennettu valmiiksi HDInsight klustereiden ja anna klusterin perustoiminnot. Esimerkiksi kuitenkaan Resurssienhallinta, rakenne-kyselykieltä (HiveQL) ja Mahout kirjaston kuuluvat tähän luokkaan. Klusterin osat täydellinen luettelo on käytettävissä [uudet myöntämä HDInsight Hadoop-klusterin versioissa?](hdinsight-component-versioning.md) </a>.
- **Mukautettu osat** -, klusterin-käyttäjänä voit asentaa tai käyttää havainnollistamiseen osan yhteisön tai luotu itse.

Valmiit osat ovat tuettuja, ja Microsoft Support auttavat eristämään ja ratkaista ongelmat, jotka liittyvät komponentit.

> [AZURE.WARNING] Osien HDInsight-klusterin mukana tuetaan täysin ja Microsoft Support auttavat eristämään ja ratkaista ongelmat, jotka liittyvät komponentit.
>
> Mukautettujen osien saa järkevän tukea helpottavat edelleen ongelman vianmäärityksen. Tämä saattaa aiheuttaa ratkaisemiseksi tai sinulta kysytään, haluatko osallistuminen käytettävissä olevat kanavat Avaa lähde-tekniikoiden laaja osaamisalueet, tekniikkaa löytyi. Esimerkiksi ovat yhteisön sivustoja, joita voidaan käyttää, kuten: [HDInsight MSDN-keskustelupalsta](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Myös Apache projektien on projektisivustojen [http://apache.org](http://apache.org), esimerkiksi: [Hadoop](http://hadoop.apache.org/), [Ohjattu](http://spark.apache.org/).

HDInsight-palvelun tarjoaa useita tapoja käyttää mukautettuja osia. Riippumatta siitä, miten osaa on käytetty tai klusterin asennettuihin koskee tuki samalla tasolla. Alla on luettelo yleisimmistä tavalla, että mukautettuja osia voidaan käyttää HDInsight klustereiden:

1. Lähetys - Hadoop tai muun tyyppisiä suorittaa tai käyttää mukautettuja osia töitä projektin voidaan lähettää klusterin.
2. Klusterin mukauttaminen - klusterin luonnin aikana voit määrittää Lisäasetukset ja mukautetun osat, jotka asennetaan klusterisolmut.
3. Objektit - Suositut mukautettuja osia, Microsoftin ja muiden voi antaa esimerkkejä siitä, miten näitä osia voidaan käyttää HDInsight-klustereiden. Mallit toimitetaan ilman tukea.

## <a name="develop-script-action-scripts"></a>Komentosarja-toiminnon kehittämiseen

Katso [kehittää komentosarja-toiminnon komentosarjat HDInsight][hdinsight-write-script].


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
