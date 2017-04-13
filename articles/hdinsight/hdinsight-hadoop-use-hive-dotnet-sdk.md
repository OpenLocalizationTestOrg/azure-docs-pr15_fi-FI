<properties
    pageTitle="Suorita rakenteen kyselyjen HDInsight .NET SDK | Microsoft Azure"
    description="Lue, miten voit lähettää Hadoop työt Azure Hdinsightiin Hadoop HDInsight .NET SDK: N avulla."
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
   ms.date="09/14/2016"
    ms.author="jgao"/>

# <a name="run-hive-queries-using-hdinsight-net-sdk"></a>Suorita HDInsight .NET SDK kyselyjen rakenne

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]


Tietoja Lähetä HDInsight .NET SDK kyselyjen rakenne.

> [AZURE.NOTE] Tämän artikkelin vaiheet on suoritettava Windows-asiakasohjelmassa. Saat lisätietoja Linux, OS X tai Unix-asiakasohjelmaa käyttävien rakenne-käyttöä varten käytöstä on artikkelissa ylälaidassa näkyvän sarkainvalitsinta.

##<a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat tämän artikkelin, sinulla on oltava seuraavasti:

- **A Hadoop klusterin Hdinsightista**. Katso [käyttäminen Linux-pohjaiset Hadoop-Hdinsightista](hdinsight-use-sqoop.md#create-cluster-and-sql-database).
- **Visual Studio 2012/2013/2015**.

##<a name="submit-hive-queries-using-hdinsight-net-sdk"></a>Lähetä HDInsight .NET SDK kyselyjen rakenne

HDInsight .NET SDK on .NET asiakkaan kirjastot, joka on helppo HDInsight klustereiden .NET-käyttöä varten. 

**Voit lähettää työt**

1. C#-konsolisovelluksen luominen Visual Studio.
2. Nuget pakettien hallinta-konsolin varten suorittamalla seuraavan komennon.

        Install-Package Microsoft.Azure.Management.HDInsight.Job

2. Käytä seuraava koodi:

        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading;
        using Microsoft.Azure.Management.HDInsight.Job;
        using Microsoft.Azure.Management.HDInsight.Job.Models;
        using Hyak.Common;

        namespace SubmitHDInsightJobDotNet
        {
            class Program
            {
                private static HDInsightJobManagementClient _hdiJobManagementClient;

                private const string ExistingClusterName = "<Your HDInsight Cluster Name>";
                private const string ExistingClusterUri = ExistingClusterName + ".azurehdinsight.net";
                private const string ExistingClusterUsername = "<Cluster Username>";
                private const string ExistingClusterPassword = "<Cluster User Password>";

                private const string DefaultStorageAccountName = "<Default Storage Account Name>";
                private const string DefaultStorageAccountKey = "<Default Storage Account Key>";
                private const string DefaultStorageContainerName = "<Default Blob Container Name>";

                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");

                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);

                    SubmitHiveJob();

                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }

                private static void SubmitHiveJob()
                {
                    Dictionary<string, string> defines = new Dictionary<string, string> { { "hive.execution.engine", "ravi" }, { "hive.exec.reducers.max", "1" } };
                    List<string> args = new List<string> { { "argA" }, { "argB" } };
                    var parameters = new HiveJobSubmissionParameters
                    {
                        Query = "SHOW TABLES",
                        Defines = defines,
                        Arguments = args
                    };

                    System.Console.WriteLine("Submitting the Hive job to the cluster...");
                    var jobResponse = _hdiJobManagementClient.JobManagement.SubmitHiveJob(parameters);
                    var jobId = jobResponse.JobSubmissionJsonResponse.Id;
                    System.Console.WriteLine("Response status code is " + jobResponse.StatusCode);
                    System.Console.WriteLine("JobId is " + jobId);

                    System.Console.WriteLine("Waiting for the job completion ...");

                    // Wait for job completion
                    var jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    while (!jobDetail.Status.JobComplete)
                    {
                        Thread.Sleep(1000);
                        jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    }

                    // Get job output
                    var storageAccess = new AzureStorageAccess(DefaultStorageAccountName, DefaultStorageAccountKey,
                        DefaultStorageContainerName);
                    var output = (jobDetail.ExitValue == 0)
                        ? _hdiJobManagementClient.JobManagement.GetJobOutput(jobId, storageAccess) // fetch stdout output in case of success
                        : _hdiJobManagementClient.JobManagement.GetJobErrorLogs(jobId, storageAccess); // fetch stderr output in case of failure

                    System.Console.WriteLine("Job output is: ");

                    using (var reader = new StreamReader(output, Encoding.UTF8))
                    {
                        string value = reader.ReadToEnd();
                        System.Console.WriteLine(value);
                    }
                }
            }
        }

5. Painamalla **F5** sovelluksen käyttämiseen.


## <a name="next-steps"></a>Seuraavat vaiheet

Tässä artikkelissa on oppimiasi HDInsight-klusterin luominen eri tavoin. Lisätietoja on seuraavissa artikkeleissa:

* [Azure Hdinsightiin käytön aloittaminen][hdinsight-get-started]
* [Luo Hadoop klustereiden Hdinsightiin][hdinsight-provision]
* [Azure-portaalissa voit hallita Hadoop varausyksiköt Hdinsightiin](hdinsight-administer-use-management-portal.md)
* [HDInsight .NET SDK-ohje](https://msdn.microsoft.com/library/mt271028.aspx)
* [Possu käyttäminen Hdinsightiin](hdinsight-use-pig.md)
* [Sqoop käyttäminen Hdinsightiin](hdinsight-use-sqoop-mac-linux.md)
* [Ei ole vuorovaikutteinen todennus .NET HDInsight-sovellusten luominen](hdinsight-create-non-interactive-authentication-dotnet-applications.md)


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md


