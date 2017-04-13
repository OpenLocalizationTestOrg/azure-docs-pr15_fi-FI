<properties
    pageTitle="Luo Hadoop, HBase, myrsky tai ohjattu klustereiden HDInsight käyttämällä HDInsight .NET SDK Linux | Microsoft Azure"
    description="Opettele luomaan Hadoop, HBase, myrsky tai ohjattu klustereiden Linux käyttämällä HDInsight .NET SDK HDInsight varten."
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/02/2016"
    ms.author="jgao"/>

#<a name="create-linux-based-clusters-in-hdinsight-using-the-net-sdk"></a>Linux-pohjaiset klustereiden luominen käyttämällä .NET SDK Hdinsightiin

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

HDInsight .NET SDK sisältää .NET asiakkaan kirjastoissa, jotka helpottavat käsitellä HDInsight .NET Framework-sovellus. Tässä asiakirjassa kerrotaan, miten käyttämällä .NET SDK Linux-pohjaiset HDInsight-klusterin luominen.

> [AZURE.IMPORTANT] Ohjeita tämän asiakirjan luominen klusterin yksi työntekijä solmu. Jos aio yli 32 työntekijä solmut-klusterin luominen tai mukaan skaalaus klusterin luonnin jälkeen, on valittava pään solmu kokoa, ja vähintään 8 sydämiä 14 gt RAM-muistia.
>
> Saat lisätietoja solmu kokojen ja niihin liittyvät kustannukset, [HDInsight hinnat](https://azure.microsoft.com/pricing/details/hdinsight/).

##<a name="prerequisites"></a>Edellytykset

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

- **Azure-tilaus**. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- __Visual Studio 2013: n tai 2015__

### <a name="access-control-requirements"></a>Access-ohjausobjektin vaatimukset

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-clusters"></a>Klustereiden luominen

1. Avaa Visual Studio 2013 tai 2015.

2. Visual Studio uuden projektin luominen seuraavilla asetuksilla

  	|Ominaisuus|Arvo|
  	|--------|-----|
  	|Malli|Mallit ja Visual C# / Windows/Console-sovellus|
  	|Nimi|CreateHDICluster|

5. Valitse **Työkalut** -valikosta **Nuget paketin**hallinta ja valitse sitten **Paketin hallinta-konsolin**.

6. Asenna paketit konsolissa varten suorittamalla seuraavan komennon:

        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
        Install-Package Microsoft.Azure.Management.ResourceManager -Pre
        Install-Package Microsoft.Azure.Management.HDInsight

    Nämä komennot lisääminen projektista Visual Studio .NET-kirjastojen ja viittaukset niihin.

6. Kaksoisnapsauttamalla ratkaisunhallinnassa, avaa se ja liitä seuraava koodi Anna arvot muuttujat **Program.cs** :

        using System;
        using System.Threading;
        using System.Threading.Tasks;
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.HDInsight;
        using Microsoft.Azure.Management.HDInsight.Models;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using System.Net.Http;
        using Newtonsoft.Json;
        using System.Collections.Generic;

        namespace CreateHDInsightCluster
        {
            class Program
            {
                private static HDInsightManagementClient _hdiManagementClient;

                // Replace with your AAD tenant ID if necessary
                private const string TenantId = UserTokenProvider.CommonTenantId; 
                private const string SubscriptionId = "<Your Azure Subscription ID>";
                // This is the GUID for the PowerShell client. Used for interactive logins in this example.
                private const string ClientId = "1950a258-227b-4e31-a9cf-717495945fc2";

                private static string SubscriptionId = "<Enter Your Subscription ID>";
                private const string ExistingResourceGroupName = "<Enter Resource Group Name>";
                private const string ExistingStorageName = "<Enter Default Storage Account Name>.blob.core.windows.net";
                private const string ExistingStorageKey = "<Enter Default Storage Account Key>";
                private const string ExistingBlobContainer = "<Enter Default Bob Container Name>";
                private const string NewClusterName = "<Enter HDInsight Cluster Name>";
                private const int NewClusterNumNodes = 2;
                private const string NewClusterLocation = "EAST US 2";     // Must be the same as the default Storage account
                private const OSType NewClusterOSType = OSType.Linux;
                private const string NewClusterType = "Hadoop";
                private const string NewClusterVersion = "3.2";
                private const string NewClusterUsername = "admin";
                private const string NewClusterPassword = "<Enter HTTP User Password>";
                private const string NewClusterSshUserName = "sshuser";
                private const string NewClusterSshPublicKey = @"---- BEGIN SSH2 PUBLIC KEY ----
                    Comment: ""rsa-key-20150731""
                    AAAAB3NzaC1yc2EAAAABJQAAAQEA4QiCRLqT7fnmUA5OhYWZNlZo6lLaY1c+IRsp
                    gmPCsJVGQLu6O1wqcxRqiKk7keYq8bP5s30v6bIljsLZYTnyReNUa5LtFw7eauGr
                    yVt3Pve6ejfWELhbVpi0iq8uJNFA9VvRkz8IP1JmjC5jsdnJhzQZtgkIrdn3w0e6
                    WVfu15kKyY8YAiynVbdV51EB0SZaSLdMZkZQ81xi4DDtCZD7qvdtWEFwLa+EHdkd
                    pzO36Mtev5XvseLQqzXzZ6aVBdlXoppGHXkoGHAMNOtEWRXpAUtEccjpATsaZhQR
                    zZdZlzHduhM10ofS4YOYBADt9JohporbQVHM5w6qUhIgyiPo7w==
                    ---- END SSH2 PUBLIC KEY ----"; //replace the public key with your own

                static void Main(string[] args)
                {
                    System.Console.WriteLine("Creating a cluster.  The process takes 10 to 20 minutes ...");

                    // Authenticate and get a token
                    var authToken = Authenticate(TenantId, ClientId, SubscriptionId);
                    // Flag subscription for HDInsight, if it isn't already.
                    EnableHDInsight(authToken);
                    // Get an HDInsight management client
                    _hdiManagementClient = new HDInsightManagementClient(authToken);

                    // Set parameters for the new cluster
                    var parameters = new ClusterCreateParameters
                    {
                        ClusterSizeInNodes = NewClusterNumNodes,
                        UserName = NewClusterUsername,
                        ClusterType = NewClusterType,
                        OSType = NewClusterOSType,
                        Version = NewClusterVersion,

                        DefaultStorageAccountName = ExistingStorageName,
                        DefaultStorageAccountKey = ExistingStorageKey,
                        DefaultStorageContainer = ExistingBlobContainer,

                        Password = NewClusterPassword,
                        Location = NewClusterLocation,

                        SshUserName = NewClusterSshUserName,
                        SshPublicKey = NewClusterSshPublicKey
                    };
                    // Create the cluster
                    _hdiManagementClient.Clusters.Create(ExistingResourceGroupName, NewClusterName, parameters);

                    System.Console.WriteLine("The cluster has been created. Press ENTER to continue ...");
                    System.Console.ReadLine();
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
            }
        }
    
10. Korvaa luokan jäsenen arvot.

7. Painamalla **F5** sovelluksen käyttämiseen. Konsolin ikkuna avataan ja sovelluksen tilan. Voit pyydetään myös tunnistetietosi Azure-tili. Voi kestää useita minuutteja luominen HDInsight-klusterin, tavallisesti noin 15.

## <a name="use-bootstrap"></a>Käytä Automaattinen

Lisätietoja on artikkelissa [mukauttaminen HDInsight klustereiden käyttämällä automaattinen](hdinsight-hadoop-customize-cluster-bootstrap.md).

Muokkaa näyte [Luo klustereiden](#create-clusters) rakenne-asetus:

    static void Main(string[] args)
    {
        System.Console.WriteLine("Creating a cluster.  The process takes 10 to 20 minutes ...");

        // Authenticate and get a token
        var authToken = Authenticate(TenantId, ClientId, SubscriptionId);
        // Flag subscription for HDInsight, if it isn't already.
        EnableHDInsight(authToken);
        // Get an HDInsight management client
        _hdiManagementClient = new HDInsightManagementClient(authToken);

        // Set parameters for the new cluster
        var extendedParameters = new ClusterCreateParametersExtended
        {
            Location = NewClusterLocation,
            Properties = new ClusterCreateProperties
            {
                ClusterDefinition = new ClusterDefinition
                {
                    ClusterType = NewClusterType.ToString()
                },
                ClusterVersion = NewClusterVersion,
                OperatingSystemType = NewClusterOSType
            }
        };

        var coreConfigs = new Dictionary<string, string>
        {
            {"fs.defaultFS", string.Format("wasbs://{0}@{1}", ExistingBlobContainer, ExistingStorageName)},
            {
                string.Format("fs.azure.account.key.{0}", ExistingStorageName),
                ExistingStorageKey
            }
        };

        // bootstrap
        var hiveConfigs = new Dictionary<string, string>
        {
            { "hive.metastore.client.socket.timeout", "90"}
        };

        var gatewayConfigs = new Dictionary<string, string>
        {
            {"restAuthCredential.isEnabled", "true"},
            {"restAuthCredential.username", NewClusterUsername},
            {"restAuthCredential.password", NewClusterPassword}
        };

        var configurations = new Dictionary<string, Dictionary<string, string>>
        {
            {"core-site", coreConfigs},
            {"gateway", gatewayConfigs},
            {"hive-site", hiveConfigs}
        };

        var serializedConfig = JsonConvert.SerializeObject(configurations);
        extendedParameters.Properties.ClusterDefinition.Configurations = serializedConfig;

        var sshPublicKeys = new List<SshPublicKey>();
        var sshPublicKey = new SshPublicKey
        {
            CertificateData =
                string.Format("ssh-rsa {0}", NewClusterSshPublicKey)
        };
        sshPublicKeys.Add(sshPublicKey);

        var headNode = new Role
        {
            Name = "headnode",
            TargetInstanceCount = 2,
            HardwareProfile = new HardwareProfile
            {
                VmSize = "Large"
            },
            OsProfile = new OsProfile
            {
                LinuxOperatingSystemProfile = new LinuxOperatingSystemProfile
                {
                    UserName = NewClusterSshUserName,
                    Password = NewClusterSshPassword //,
                    // When use a SSH pulbic key, make sure to remove comments, headers and trailers, and concatenate the key into one line 
                    //SshProfile = new SshProfile
                    //{
                    //    SshPublicKeys = sshPublicKeys
                    //}
                }
            }
        };

        var workerNode = new Role
        {
            Name = "workernode",
            TargetInstanceCount = NewClusterNumNodes,
            HardwareProfile = new HardwareProfile
            {
                VmSize = "Large"
            },
            OsProfile = new OsProfile
            {
                LinuxOperatingSystemProfile = new LinuxOperatingSystemProfile
                {
                    UserName = NewClusterSshUserName,
                    Password = NewClusterSshPassword //,
                    //SshProfile = new SshProfile
                    //{
                    //    SshPublicKeys = sshPublicKeys
                    //}
                }
            }
        };

        extendedParameters.Properties.ComputeProfile = new ComputeProfile();
        extendedParameters.Properties.ComputeProfile.Roles.Add(headNode);
        extendedParameters.Properties.ComputeProfile.Roles.Add(workerNode);

        _hdiManagementClient.Clusters.Create(ExistingResourceGroupName, NewClusterName, extendedParameters);

        System.Console.WriteLine("The cluster has been created. Press ENTER to continue ...");
        System.Console.ReadLine();
    }


## <a name="use-script-action"></a>Komentosarja-toiminnolla

Saat lisätietoja-kohdassa [Mukauta Linux-pohjaiset HDInsight klustereiden komentosarja-toiminnon avulla](hdinsight-hadoop-customize-cluster-linux.md).

Muokkaa näyte [Luo klustereiden](#create-clusters) Soita komentosarja-toiminnon R: asentaminen

    static void Main(string[] args)
    {
        System.Console.WriteLine("Creating a cluster.  The process takes 10 to 20 minutes ...");

        // Authenticate and get a token
        var authToken = Authenticate(TenantId, ClientId, SubscriptionId);
        // Flag subscription for HDInsight, if it isn't already.
        EnableHDInsight(authToken);
        // Get an HDInsight management client
        _hdiManagementClient = new HDInsightManagementClient(authToken);

        // Set parameters for the new cluster
        var parameters = new ClusterCreateParameters
        {
            ClusterSizeInNodes = NewClusterNumNodes,
            Location = NewClusterLocation,
            ClusterType = NewClusterType,
            OSType = NewClusterOSType,
            Version = NewClusterVersion,

            DefaultStorageAccountName = ExistingStorageName,
            DefaultStorageAccountKey = ExistingStorageKey,
            DefaultStorageContainer = ExistingBlobContainer,

            UserName = NewClusterUsername,
            Password = NewClusterPassword,
            SshUserName = NewClusterSshUserName,
            SshPublicKey = NewClusterSshPublicKey
        };

        ScriptAction rScriptAction = new ScriptAction("Install R",
            new Uri("https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh"), "");

        parameters.ScriptActions.Add(ClusterNodeType.HeadNode,new System.Collections.Generic.List<ScriptAction> { rScriptAction});
        parameters.ScriptActions.Add(ClusterNodeType.WorkerNode, new System.Collections.Generic.List<ScriptAction> { rScriptAction });
        
        _hdiManagementClient.Clusters.Create(ExistingResourceGroupName, NewClusterName, parameters);

        System.Console.WriteLine("The cluster has been created. Press ENTER to continue ...");
        System.Console.ReadLine();
    }

##<a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet luonut HDInsight-klusterin, katso, miten voit käyttää yhteyttä klusterin seuraavat avulla. 

###<a name="hadoop-clusters"></a>Hadoop klustereiden

* [Rakenteen käyttäminen Hdinsightiin](hdinsight-use-hive.md)
* [Possu käyttäminen Hdinsightiin](hdinsight-use-pig.md)
* [MapReduce käyttäminen Hdinsightiin](hdinsight-use-mapreduce.md)

###<a name="hbase-clusters"></a>HBase klustereiden

* [HBase HDInsight-käytön aloittaminen](hdinsight-hbase-tutorial-get-started-linux.md)
* [Kehittää Java HBase HDInsight-sovelluksia](hdinsight-hbase-build-java-maven-linux.md)

###<a name="storm-clusters"></a>Myrsky klustereiden

* [Kehitä Java topologioissa myrsky HDInsight-](hdinsight-storm-develop-java-topology.md)
* [Myrsky HDInsight-Python osien käyttäminen](hdinsight-storm-develop-python-topology.md)
* [Ottaa käyttöön ja topologioissa myrsky HDInsight-ja seuranta](hdinsight-storm-deploy-monitor-topology-linux.md)

###<a name="spark-clusters"></a>Ohjattu klustereiden

* [Luo erillisen-sovelluksen käyttäminen Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Suorita työt etäyhteyden käyttämällä Livy ohjattu klusterissa](hdinsight-apache-spark-livy-rest-interface.md)
* [Ohjattu BI: vuorovaikutteinen tietojen analysoinnissa ohjattu käyttäminen HDInsight kanssa Liiketoimintatieto-työkaluista](hdinsight-apache-spark-use-bi-tools.md)
* [Ohjattu koneen Learning kanssa: Käytä ohjattu elintarvikkeiden tulokset ennustetaan HDInsight-](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Ohjattu virtautetun median: Käytä ohjattu HDInsight reaaliaikainen streaming sovellusten luomiseen:](hdinsight-apache-spark-eventhub-streaming.md)

### <a name="run-jobs"></a>Töiden suorittaminen

- [Käyttämällä .NET SDK HDInsight rakenteen töiden suorittaminen](hdinsight-hadoop-use-hive-dotnet-sdk.md)
- [Käyttämällä .NET SDK HDInsight Possu töiden suorittaminen](hdinsight-hadoop-use-pig-dotnet-sdk.md)
- [Käyttämällä .NET SDK HDInsight Sqoop töiden suorittaminen](hdinsight-hadoop-use-sqoop-dotnet-sdk.md)
- [Oozie töiden suorittaminen Hdinsightiin](hdinsight-use-oozie.md)
