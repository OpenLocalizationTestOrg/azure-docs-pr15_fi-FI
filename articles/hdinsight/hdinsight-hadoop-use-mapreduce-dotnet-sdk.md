<properties
    pageTitle="Lähettää MapReduce työt HDInsight .NET SDK: N avulla | Microsoft Azure"
    description="Lue, miten voit lähettää MapReduce työt Azure Hdinsightiin Hadoop HDInsight .NET SDK: N avulla."
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
   ms.date="10/28/2016"
    ms.author="jgao"/>

# <a name="run-mapreduce-jobs-using-hdinsight-net-sdk"></a>Suorita MapReduce työt HDInsight .NET SDK: N avulla

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Lue, miten voit lähettää MapReduce työt HDInsight .NET SDK: N avulla. HDInsight klustereiden sisältyvät purkki tiedosto, jonka MapReduce muutamia esimerkkejä. Jar-tiedosto on */example/jars/hadoop-mapreduce-examples.jar*.  Mallit on *wordcount*. Voit kehittää C# konsolisovelluksen lähetät wordcount työn.  Työn lukee */example/data/gutenberg/davinci.txt* -tiedosto ja tulostaa */example/data/davinciwordcount*tulokset.  Jos haluat suorittaa sovelluksen Tulostekansio on Puhdista.

> [AZURE.NOTE] Tämän artikkelin vaiheet on suoritettava Windows-asiakasohjelmassa. Saat lisätietoja Linux, OS X tai Unix-asiakasohjelmaa käyttävien rakenne-käyttöä varten käytöstä on artikkelissa ylälaidassa näkyvän sarkainvalitsinta.

##<a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat tämän artikkelin, sinulla on oltava seuraavasti:

- **A Hadoop klusterin Hdinsightista**. Katso [käyttäminen Linux-pohjaiset Hadoop-Hdinsightista](hdinsight-use-sqoop.md#create-cluster-and-sql-database).
- **Visual Studio 2012/2013/2015**.

##<a name="submit-mapreduce-jobs-using-hdinsight-net-sdk"></a>Lähettää MapReduce työt HDInsight .NET SDK: N avulla

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

                    SubmitMRJob();

                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }

                private static void SubmitHiveJob()
                {
                    List<string> args = new List<string> { { "/example/data/gutenberg/davinci.txt" }, { "/example/data/davinciwordcount" } };

                    var paras = new MapReduceJobSubmissionParameters
                    {
                        //Content = "wordcount  ",
                        JarFile = @"/example/jars/hadoop-mapreduce-examples.jar",
                        JarClass = "wordcount",
                        Arguments = args
                    };

                    System.Console.WriteLine("Submitting the MR job to the cluster...");
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

- Klusterin luominen ja lähettäminen rakenne-työ on artikkelissa [Azure Hdinsightiin käytön aloittaminen](hdinsight-hadoop-linux-tutorial-get-started.md).
- Luomisen HDInsight klustereiden kohdassa [Luo Linux-pohjaiset Hadoop varausyksiköt Hdinsightista](hdinsight-hadoop-provision-linux-clusters.md).
- Lisätietoja HDInsight klustereiden hallinnasta on artikkelissa [Hallitse Hadoop varausyksiköt Hdinsightista](hdinsight-administer-use-management-portal.md).
- Käyttöön liittyviä HDInsight .NET SDK-kohdassa [HDInsight .NET SDK-ohje](https://msdn.microsoft.com/library/mt271028.aspx).
- Saat ei ole vuorovaikutteinen todentaa Azure, katso lisätietoja artikkelista [ei ole vuorovaikutteinen käyttöoikeuksien Hdinsightiin .NET sovellusten luominen](hdinsight-create-non-interactive-authentication-dotnet-applications.md).




