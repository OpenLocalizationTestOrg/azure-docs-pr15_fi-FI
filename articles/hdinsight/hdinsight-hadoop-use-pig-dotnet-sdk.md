<properties
   pageTitle="Hadoop Possu käyttäminen .NET HDInsight | Microsoft Azure"
   description="Opettele käyttämään .NET SDK Hadoop voit lähettää Possu työt Hadoop-Hdinsightista."
   services="hdinsight"
   documentationCenter=".net"
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/17/2016"
   ms.author="larryfr"/>

#<a name="run-pig-jobs-using-the-net-sdk-for-hadoop-in-hdinsight"></a>Käyttämällä .NET SDK Hadoop HDInsight Possu töiden suorittaminen

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Tässä tiedostossa on esimerkki .NET SDK Hadoop avulla voit lähettää Hadoop HDInsight-klusterissa Possu työt.

HDInsight .NET SDK sisältää .NET asiakas-kirjastoja, joissa on helppo HDInsight klustereiden .NET-käyttöä varten. Possu avulla voit luoda MapReduce toimien mukaan mallinnus tietojen muunnokset sarjaa. Opit lähetät Possu työn HDInsight-klusterin basic C#-sovelluksen avulla.

## <a name="prerequisites"></a>Edellytykset

Tämän artikkelin seuraavien vaiheiden suorittamiseen tarvitset seuraavasti.

* Azure Hdinsightiin (Hadoop-HDInsight)-klusterin (joko Windows- tai Linux-pohjaiset).
* Visual Studio 2012 tai 2013 tai 2015.

## <a name="create-the-application"></a>Sovelluksen luominen

HDInsight .NET SDK on .NET asiakkaan kirjastot, joka on helppo HDInsight klustereiden .NET-käyttöä varten. 


1. Avaa Visual Studio 2012 tai 2013
2. **Tiedosto** -valikosta **Uusi** ja valitse sitten **Projekti**.
3. Kirjoita uuden projektin tai seuraavat arvot.

    <table>
    <tr>
    <th>Ominaisuus</th>
    <th>Arvo</th>
    </tr>
    <tr>
    <th>Luokka</th>
    <th>Mallit ja Visual C# / Windows</th>
    </tr>
    <tr>
    <th>Malli</th>
    <th>Console-sovellus</th>
    </tr>
    <tr>
    <th>Nimi</th>
    <th>SubmitPigJob</th>
    </tr>
    </table>
4. Valitse **OK** , jos haluat luoda projektin.
5. Valitse **Työkalut** -valikosta **Kirjaston pakettien hallinta** tai **Nuget pakettien hallinta**ja valitse sitten **Paketin hallinta-konsolin**.
6. Suorita seuraava komento asentaa .NET SDK-paketteja konsolissa.

        Install-Package Microsoft.Azure.Management.HDInsight.Job

7. Kaksoisnapsauttamalla ratkaisunhallinnassa, avaa se **Program.cs** . Korvaa aiemmin luotu koodi seuraavasti.

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

                    SubmitPigJob();

                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }

                private static void SubmitPigJob()
                {
                    var parameters = new PigJobSubmissionParameters
                    {
                        Query = @"LOGS = LOAD 'wasbs:///example/data/sample.log';
                                    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
                                    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
                                    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
                                    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
                                    RESULT = order FREQUENCIES by COUNT desc;
                                    DUMP RESULT;"
                    };

                    System.Console.WriteLine("Submitting the Pig job to the cluster...");
                    var response = _hdiJobManagementClient.JobManagement.SubmitPigJob(parameters);
                    System.Console.WriteLine("Validating that the response is as expected...");
                    System.Console.WriteLine("Response status code is " + response.StatusCode);
                    System.Console.WriteLine("Validating the response object...");
                    System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
                }
            }
        }


7. Painamalla **F5-näppäintä** , Käynnistä sovellus.
8. Paina **ENTER** Lopeta sovellus.

## <a name="summary"></a>Yhteenveto

Kuten näet, .NET-SDK Hadoop avulla voit luoda .NET-sovelluksia, jotka lähettävät Possu työt HDInsight-klusterin ja projektin tilaa.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoa HDInsight Possu.

* [Possu käyttäminen Hadoop-Hdinsightiin](hdinsight-use-pig.md)

Lisätietoja muista tavoista voit käsitellä Hadoop-Hdinsightista.

* [Hadoop HDInsight-rakenteen käyttäminen](hdinsight-use-hive.md)

* [Käytä MapReduce Hadoop-HDInsight kanssa](hdinsight-use-mapreduce.md) [Esikatselu-portaalin]: https://portal.azure.com/
