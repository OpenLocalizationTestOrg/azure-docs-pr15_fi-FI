<properties
    pageTitle="Käytä Hadoop Sqoop HDInsight | Microsoft Azure"
    description="Opettele käyttämään HDInsight .NET SDK Sqoop Tuo ja vie Hadoop-klusterin ja Azure SQL-tietokantaan."
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

#<a name="run-sqoop-jobs-using-net-sdk-for-hadoop-in-hdinsight"></a>Käyttämällä .NET SDK Hadoop HDInsight Sqoop töiden suorittaminen

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Opettele käyttämään HDInsight .NET SDK toimimaan Sqoop työt HDInsight tuomiseen ja viemiseen HDInsight-klusterin ja Azure SQL-tietokantaan tai SQL Server-tietokannan välillä.

> [AZURE.NOTE] Tässä artikkelissa kuvattuja voidaan käyttää joko Windows- tai Linux-pohjaiset HDInsight klusterin; kuitenkin seuraavasti toimii vain Windows-asiakasohjelmassa. Sarkainvalitsinta ylälaidassa tämän artikkelin avulla voit valita muita menetelmiä.

###<a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat Tässä opetusohjelmassa, sinun on oltava seuraavasti:

- **A Hadoop klusterin Hdinsightista**. Katso [Luo klusterin ja SQL-tietokantaan](hdinsight-use-sqoop.md#create-cluster-and-sql-database).

## <a name="run-sqoop-using-net-sdk"></a>Suorita Sqoop .NET SDK: N avulla

HDInsight .NET SDK on .NET asiakkaan kirjastot, joka on helppo HDInsight klustereiden .NET-käyttöä varten. Tässä osassa luot C# konsolisovelluksen hivesampletable vieminen SQL-tietokantaan taulukon, jonka loit aiemmin tässä opetusohjelmat.

**Voit lähettää Sqoop työn**

1. C#-konsolisovelluksen luominen Visual Studio.
2. Visual Studio pakettien hallinta-konsolin käyttämällä seuraavaa komentoa Nuget Tuo paketin.

        Install-Package Microsoft.Azure.Management.HDInsight.Job
        
3. Käytä Program.cs tiedosto seuraava koodi:

        using System.Collections.Generic;
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
        
                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");
        
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);
        
                    SubmitSqoopJob();
        
                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }
        
                private static void SubmitSqoopJob()
                {
                    var sqlDatabaseServerName = "<SQLDatabaseServerName>";
                    var sqlDatabaseLogin = "<SQLDatabaseLogin>";
                    var sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>";
                    var sqlDatabaseDatabaseName = "<DatabaseName>";
        
                    var tableName = "<TableName>";
                    var exportDir = "/tutorials/usesqoop/data";
        
                    // Connection string for using Azure SQL Database.
                    // Comment if using SQL Server
                    var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ".database.windows.net;user=" + sqlDatabaseLogin + "@" + sqlDatabaseServerName + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
                    // Connection string for using SQL Server.
                    // Uncomment if using SQL Server
                    //var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ";user=" + sqlDatabaseLogin + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
        
                    var parameters = new SqoopJobSubmissionParameters
                    {
                        Files = new List<string> { "/user/oozie/share/lib/sqoop/sqljdbc41.jar" }, // This line is required for Linux-based cluster.
                        Command = "export --connect " + connectionString + " --table " + tableName + "_mobile --export-dir " + exportDir + "_mobile --fields-terminated-by \\t -m 1"
                    };
        
                    System.Console.WriteLine("Submitting the Sqoop job to the cluster...");
                    var response = _hdiJobManagementClient.JobManagement.SubmitSqoopJob(parameters);
                    System.Console.WriteLine("Validating that the response is as expected...");
                    System.Console.WriteLine("Response status code is " + response.StatusCode);
                    System.Console.WriteLine("Validating the response object...");
                    System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
                }
            }
        }
        
4. Painamalla **F5** ohjelman. 

##<a name="limitations"></a>Rajoitukset

* Vie - ja Linux-pohjaiset HDInsight, Microsoft SQL Server tai Azure SQL-tietokannan tietojen vieminen Sqoop liittimen joukkona ei tue joukkona Lisää tällä hetkellä.

* Jonottaminen - Linux-pohjaiset HDInsight kanssa käytettäessä `-batch` vaihtaminen Lisää suoritettaessa, Sqoop suorittaa useita Lisää sijaan jonottaminen Lisää toimintoja.

##<a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet oppinut käyttämään Sqoop. Lisätietoja on artikkeleissa:

- [Käytä Oozie kanssa HDInsight](hdinsight-use-oozie.md): Oozie-työnkulun käyttäminen Sqoop-toiminto.
- [Analysoi viive lentotietoihin käyttämällä HDInsight](hdinsight-analyze-flight-delay-data.md): Käytä rakenne analysointiin lento viivyttää tiedot ja vie Azure SQL-tietokantaan Sqoop avulla.
- [Lataa tiedot HDInsight](hdinsight-upload-data.md): Etsi tietojen lataaminen HDInsight/Azure-Blob-säiliö muilla tavoilla.


