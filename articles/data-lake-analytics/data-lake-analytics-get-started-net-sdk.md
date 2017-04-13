<properties 
   pageTitle="Pääset alkuun käyttämällä .NET SDK Azure tietojen järvi Analytics | Azure" 
   description="Opettele käyttämään .NET SDK järvi tietovaraston tilien luomien, luoda tietojen järvi Analytics töitä ja lähettää työt kirjoitettu U SQL. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/26/2016"
   ms.author="edmaca"/>

# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-net-sdk"></a>Opetusohjelma: Azure tietojen järvi Analytics käyttämällä .NET SDK käytön aloittaminen

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]


Opettele käyttämään voit lähettää työt kirjoittaa tietoja järvi Analytics [U](data-lake-analytics-u-sql-get-started.md) SQL Azure-.NET SDK. Lisätietoja tietojen järvi Analytics artikkelissa [Azure tietojen järvi Analytics yleiskatsaus](data-lake-analytics-overview.md).

Tässä opetusohjelmassa kehittää C# konsolisovelluksen lähettää U-SQL-työ, jossa lukee sarkainmerkin erotetut arvot (TSV)-tiedosto ja muuntaa sen pilkuilla erotetut arvot (CSV)-tiedostoon. Siirry kautta käyttämällä muita tuetuilla työkaluilla saman opetusohjelman napsauttamalla tämän artikkelin ylälaidassa välilehdet.

##<a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat Tässä opetusohjelmassa, sinun on oltava seuraavasti:

- **Visual Studio 2015, Visual Studio 2013 Päivitä 4, tai Visual Studio 2012, jossa on Visual C++ asennettuna**.
- **Microsoft Azure SDK .NET 2.5-version tai yläpuolella**.  Asenna se käyttämällä [WWW-ympäristö asennusohjelma](http://www.microsoft.com/web/downloads/platform.aspx).
- **Azure tietojen järvi Analytics-tili**. Katso [Hallinta tietojen järvi Analytics Azure .NET SDK: N avulla](data-lake-analytics-manage-use-dotnet-sdk.md).

##<a name="create-console-application"></a>Console-sovelluksen luominen

Tässä opetusohjelmassa käsitellä joitakin haun lokitiedot.  Etsi lokissa voidaan tallentaa tiedot järvi kaupan tai Azure-Blob-säiliö. 

Esimerkki haku lokia löytyvät julkisen Azure-Blob-säilö. Sovelluksen Lataa tiedosto työasemaan ja lataa tiedosto järvi tietovaraston oletustilin tietojen järvi Analytics-tilisi.

**Voit luoda U-SQL-komentosarja**

Tietoja järvi Analytics työt kirjoitetaan U SQL-kielen. Lisätietoja U-SQL-kohdassa [U SQL-kielen käytön aloittaminen](data-lake-analytics-u-sql-get-started.md) - ja [U-SQL-kieliohje](http://go.microsoft.com/fwlink/?LinkId=691348).

Luo **SampleUSQLScript.txt** tiedosto, jossa Seuraava U-SQL-komentosarja ja Vie tiedostoon **C:\temp\* * polku.  Polku on englanninkielisissä .NET-sovelluksessa, jossa voit luoda seuraavaan vaiheeseen.  

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();
    
    OUTPUT @searchlog   
        TO "/Output/SearchLog-from-Data-Lake.csv"
    USING Outputters.Csv();

Tämä U-SQL-komentosarja lukee käyttämällä **Extractors.Tsv()**lähdetiedosto tiedot ja luo csv-tiedoston **Outputters.Csv()**. 

C#-ohjelmassa sinun täytyy valmisteleminen **/Samples/Data/SearchLog.tsv** -tiedosto ja **/Output/** -kansio.    

On helpompaa käyttämään suhteelliset polut oletusarvon tietojen järvi tilit tallennettuja tiedostoja. Voit käyttää myös suoria polkuja.  Esimerkki 

    adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
    
Sinun on käytettävä absoluuttiset polut linkitetyt tallennustilan asiakkaat tiedostoja.  Liitetyn Azure-tallennustilan tilin tallennettuja tiedostoja syntaksi on seuraava:

    wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

>[AZURE.NOTE] Azure tietopalvelu järvi tunnettu ongelma tällä hetkellä.  Jos otoksen sovelluksen keskeytyy tai tapahtuu virhe, joudut ehkä poistaa manuaalisesti järvi tietovaraston ja tietojen järvi Analytics-tilejä, komentosarja luo.  Jos et ole tottunut Azure portaalin, [hallita Azure tietojen järvi Analytics käyttämällä Azure portal](data-lake-analytics-manage-use-portal.md) -opas Aloita näillä Taulukkomalleilla.       

**Sovelluksen luominen**

1. Avaa Visual Studio.
2. Luo C#-Konsolisovellus.
3. Avaa NuGet paketin Management console ja suorittamalla seuraavat komennot:

        Install-Package Microsoft.Azure.Management.DataLake.Analytics -Pre
        Install-Package Microsoft.Azure.Management.DataLake.Store -Pre
        Install-Package Microsoft.Azure.Management.DataLake.StoreUploader -Pre
        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
        Install-Package WindowsAzure.Storage


       
5. Program.cs liitä seuraava koodi:

        using System;
        using System.IO;
        using System.Collections.Generic;
        using System.Threading;
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.StoreUploader;
        using Microsoft.Azure.Management.DataLake.Analytics;
        using Microsoft.Azure.Management.DataLake.Analytics.Models;
        using Microsoft.WindowsAzure.Storage.Blob;

        namespace SdkSample
        {
          class Program
          {
            private const string SUBSCRIPTIONID = "<Enter Your Azure Subscription ID>";
            private const string CLIENTID = "1950a258-227b-4e31-a9cf-717495945fc2";
            private const string DOMAINNAME = "common"; // Replace this string with the user's Azure Active Directory tenant ID or domain name, if needed.

            private static string _adlaAccountName = "<Enter an Existing Data Lake Analytics Account Name>";
            private static string _adlsAccountName = "<Enter the default Data Lake Store Account Name>";

            private static DataLakeAnalyticsAccountManagementClient _adlaClient;
            private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;
            private static DataLakeAnalyticsJobManagementClient _adlaJobClient;
        
            private static void Main(string[] args)
            {
                string localFolderPath = @"c:\temp\";

                // Connect to Azure
                var creds = AuthenticateAzure(DOMAINNAME, CLIENTID);

                SetupClients(creds, SUBSCRIPTIONID);

                // Transfer the source file from a public Azure Blob container to Data Lake Store.
                CloudBlockBlob blob = new CloudBlockBlob(new Uri("https://adltutorials.blob.core.windows.net/adls-sample-data/SearchLog.tsv"));
                blob.DownloadToFile(localFolderPath + "SearchLog.tsv", FileMode.Create); // from WASB
                UploadFile(localFolderPath + "SearchLog.tsv", "/Samples/Data/SearchLog.tsv"); // to ADLS
                WaitForNewline("Source data file prepared.", "Submitting a job.");


                // Submit the job
                Guid jobId = SubmitJobByPath(localFolderPath + "SampleUSQLScript.txt", "My First ADLA Job");
                WaitForNewline("Job submitted.", "Waiting for job completion.");

                // Wait for job completion
                WaitForJob(jobId);
                WaitForNewline("Job completed.", "Downloading job output.");

                // Download job output
                DownloadFile(@"/Output/SearchLog-from-Data-Lake.csv", localFolderPath + "SearchLog-from-Data-Lake.csv");
        
                WaitForNewline("Job output downloaded. You can now exit.");
            }
        
            public static ServiceClientCredentials AuthenticateAzure(
                string domainName,
                string nativeClientAppCLIENTID)
            {
                // User login via interactive popup
                SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
                // Use the client ID of an existing AAD "Native Client" application.
                var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(nativeClientAppCLIENTID, new Uri("urn:ietf:wg:oauth:2.0:oob"));
                return UserTokenProvider.LoginWithPromptAsync(domainName, activeDirectoryClientSettings).Result;
            }

            public static void SetupClients(ServiceClientCredentials tokenCreds, string subscriptionId)
            {
                _adlaClient = new DataLakeAnalyticsAccountManagementClient(tokenCreds);
                _adlaClient.SubscriptionId = subscriptionId;

                _adlaJobClient = new DataLakeAnalyticsJobManagementClient(tokenCreds);

                _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(tokenCreds);
            }

            public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
            {
                var parameters = new UploadParameters(srcFilePath, destFilePath, _adlsAccountName, isOverwrite: force);
                var frontend = new DataLakeStoreFrontEndAdapter(_adlsAccountName, _adlsFileSystemClient);
                var uploader = new DataLakeStoreUploader(parameters, frontend);
                uploader.Execute();
            }

            public static void DownloadFile(string srcPath, string destPath)
            {
                var stream = _adlsFileSystemClient.FileSystem.Open(_adlsAccountName, srcPath);
                var fileStream = new FileStream(destPath, FileMode.Create);

                stream.CopyTo(fileStream);
                fileStream.Close();
                stream.Close();
            }

            // Helper function to show status and wait for user input
            public static void WaitForNewline(string reason, string nextAction = "")
            {
                Console.WriteLine(reason + "\r\nPress ENTER to continue...");

                Console.ReadLine();

                if (!String.IsNullOrWhiteSpace(nextAction))
                    Console.WriteLine(nextAction);
            }


            // List all Data Lake Analytics accounts within the subscription
            public static List<DataLakeAnalyticsAccount> ListADLAAccounts()
            {
                var response = _adlaClient.Account.List();
                var accounts = new List<DataLakeAnalyticsAccount>(response);

                while (response.NextPageLink != null)
                {
                    response = _adlaClient.Account.ListNext(response.NextPageLink);
                    accounts.AddRange(response);
                }

                Console.WriteLine("You have %i Data Lake Analytics account(s).", accounts.Count);
                for (int i = 0; i < accounts.Count; i++)
                {
                    Console.WriteLine(accounts[i].Name);
                }

                return accounts;
            }
            public static Guid SubmitJobByPath(string scriptPath, string jobName)
            {
                var script = File.ReadAllText(scriptPath);

                var jobId = Guid.NewGuid();
                var properties = new USqlJobProperties(script);
                var parameters = new JobInformation(jobName, JobType.USql, properties, priority: 1, degreeOfParallelism: 1, jobId: jobId);
                var jobInfo = _adlaJobClient.Job.Create(_adlaAccountName, jobId, parameters);

                return jobId;
            }

            public static JobResult WaitForJob(Guid jobId)
            {
                var jobInfo = _adlaJobClient.Job.Get(_adlaAccountName, jobId);
                while (jobInfo.State != JobState.Ended)
                {
                    jobInfo = _adlaJobClient.Job.Get(_adlaAccountName, jobId);
                }
                return jobInfo.Result.Value;
            }
          }
        }

6. Painamalla **F5** sovelluksen käyttämiseen. Tulos muistuttaa:

    ![Azure tietojen järvi Analytics työn U SQL .NET SDK tulostus](./media/data-lake-analytics-get-started-net-sdk/data-lake-analytics-dotnet-job-output.png)

7. Valitse kohdetiedosto.  Polku ja tiedostonimi oletusnimi on c:\Temp\SearchLog-from-Data-Lake.csv.

## <a name="see-also"></a>Katso myös

- Saat saman opetusohjelman muiden työkaluilla valitsemalla välilehden valitsimet-sivulla.
- Monimutkaisen kyselyn on ohjeartikkelissa [Analysoi sivuston lokit Azure tietojen järvi Analytics avulla](data-lake-analytics-analyze-weblogs.md).
- Aloita U SQL kehityssovellusten ohjeaiheessa [kehittää U-SQL-komentosarjojen käyttäminen tietojen järvi Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- Lisätietoja U SQL on artikkelissa [Azure tietojen järvi Analytics U-SQL-kielen käytön aloittaminen](data-lake-analytics-u-sql-get-started.md)ja [U-SQL-kieliohje](http://go.microsoft.com/fwlink/?LinkId=691348).
- Katso hallintatehtäviä, [hallita Azure tietojen järvi Analytics käyttämällä Azure portal](data-lake-analytics-manage-use-portal.md).
- Saat yleiskatsauksen tietojen järvi Analytics-artikkelissa [Azure tietojen järvi Analytics yleiskatsaus](data-lake-analytics-overview.md).
