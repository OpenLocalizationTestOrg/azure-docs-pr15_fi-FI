<properties 
    pageTitle="Opetusohjelma: Luo putkijohto kopioi aktiviteettiin .NET-Ohjelmointirajapinnan käyttäminen | Microsoft Azure" 
    description="Tässä opetusohjelmassa luot Azure Data Factory-myyntijakso kopioi aktiviteettiin käyttämällä .NET-Ohjelmointirajapinnan." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="10/27/2016" 
    ms.author="spelluru"/>

# <a name="tutorial-create-a-pipeline-with-copy-activity-using-net-api"></a>Opetusohjelma: Luo putkijohto kopioi aktiviteettiin .NET-Ohjelmointirajapinnan käyttäminen
> [AZURE.SELECTOR]
- [Yleiskatsaus ja edellytykset](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Ohjattu kopiointi](data-factory-copy-data-wizard-tutorial.md)
- [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShellin](data-factory-copy-activity-tutorial-using-powershell.md)
- [Azure Resurssienhallinta-malli](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST-OHJELMOINTIRAJAPINNALLA](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET-OHJELMOINTIRAJAPINTA](data-factory-copy-activity-tutorial-using-dotnet-api.md)


Tässä opetusohjelmassa näytetään, miten voit luoda ja seurata tietoja Azure-factory, .NET-Ohjelmointirajapinnan käyttäminen. Tietoja factory putkijohto käyttää kopioi tehtävän tietojen kopioiminen Azure-Blob-säiliö Azure SQL-tietokantaan.

Kopioi tehtävän suorittaa tietojen siirto Azure Data Factory. Tehtävä on tarjoaa yleisesti saatavilla palvelu, jota voit kopioida tiedot eri tavalla turvallinen, luotettava ja skaalattava tietojen stores välillä. Katso [Tietojen siirtämistä toimintoja](data-factory-data-movement-activities.md) artikkelissa lisätietoja kopio-tehtävän.   

> [AZURE.NOTE] 
> Tässä artikkelissa ei käsitellä kaikki tiedot Factory .NET API. Hae [Tiedot Factory .NET API viiteoppaasta](https://msdn.microsoft.com/library/mt415893.aspx) lisätietoja tietojen Factory .NET SDK. 

## <a name="prerequisites"></a>Edellytykset
- Käymällä läpi [opetusohjelma yleiskatsaus ja vaatimat](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) saat yleiskatsauksen opetusohjelman ja **edellytyksenä** vaiheiden avulla. 
- Visual Studio 2012- tai 2013- tai 2015
- Lataa ja asenna [Azure .NET SDK-paketissa](http://azure.microsoft.com/downloads/)
- Azure PowerShell. Ohjeiden [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md) artikkelin PowerShellin Azure asentaminen tietokoneeseen. Azure Active Directory-sovelluksen luominen Azure-PowerShellin avulla.

### <a name="create-an-application-in-azure-active-directory"></a>Azure Active Directory-sovelluksen luominen
Azure Active Directory-sovelluksen luominen, luoda service-lyhennys sovelluksen ja määrittää sen **Tietojen Factory osallistujan** rooli.  

1. Käynnistä **PowerShell**. 
1. Suorita seuraava komento ja kirjoita käyttäjänimi ja salasana, jonka avulla Kirjaudu Azure-portaaliin.
    
        Login-AzureRmAccount   
2. Suorita seuraava komento voit tarkastella kaikkien tilausten tilin.

        Get-AzureRmSubscription 
3. Suorita seuraava komento, jota haluat käyttää tilaus. Korvaa ** &lt;NameOfAzureSubscription** &gt; Azure tilauksen nimi. 

        Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext

    > [AZURE.IMPORTANT] Huomautus Tämä komento tulosteesta **SubscriptionId** ja **TenantId** . 
4. Luo nimetty **ADFTutorialResourceGroup** suorittamalla seuraava komento PowerShell Azure resurssiryhmä.  

        New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

    Jos resurssiryhmän on jo luotu, voit määrittää mielenkiinnon (N) ja päivität sen (Y). 

    Jos käytät eri resurssiryhmä, sinun on käytettävä resurssiryhmän tekstin näyttäminen ADFTutorialResourceGroup nimi Tässä opetusohjelmassa.
5. Azure Active Directory-sovelluksen luominen. 

        $azureAdApplication = New-AzureRmADApplication -DisplayName "ADFCopyTutotiralApp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.adfcopytutorialapp.org/example" -Password "Pass@word1"

    Jos saat seuraavan virheilmoituksen, Määritä eri URL-osoite ja suorita-komento uudelleen. 

        Another object with the same value for property identifierUris already exists.

6. Luo Mainos palvelun lyhennyksen. 

        New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

7. Lisää palvelu lyhennys **Tietojen Factory osallistujan** rooli. 

        New-AzureRmRoleAssignment -RoleDefinitionName "Data Factory Contributor" -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

8. Hanki sovellus ID-tunnuksellasi.

        $azureAdApplication

    Huomautus alaspäin Sovellustunnus (**applicationID** tulosteesta).

Sinulla on oltava neljän arvon seuraamisen seuraavasti: 

- Vuokraajan tunnus
- Tilauksen tunnus
- Tunnus 
- Salasana (määritetty ensimmäisen komennon)   

## <a name="walkthrough"></a>Vaiheittainen kuvaus
1. Käytä Visual Studio 2012/2013/2015, Luo C# .NET console-sovellus.
    1. Käynnistä **Visual Studio** 2012/2013/2015.
    2. Valitse **Tiedosto**, valitse **Uusi**ja sitten **Projekti**.
    3. **Mallit**ja sitten **Visual C#**. Tätä vaiheittaista käytät C#, mutta voit käyttää mitä tahansa .NET-kieltä.
    4. Valitse luettelosta erilaiset projektityypit oikealla **Console-sovellus** .
    5. Kirjoita **DataFactoryAPITestApp** nimi.
    6. Valitse **C:\ADFGetStarted** sijainti.
    7. Valitse **OK** , jos haluat luoda projektin.
2. Valitse **Työkalut**, osoita **Nuget pakettien hallinta**ja valitse **Paketti hallinta-konsolin**.
3.  **Paketin hallinta-konsolin**toimi seuraavasti: 
    1.  Suorita seuraava komento Data Factory-paketin asentaminen:`Install-Package Microsoft.Azure.Management.DataFactories`       
    2.  Suorita seuraava komento asentaa Azure Active Directory-paketti (Voit käyttää Active Directory-Ohjelmointirajapinnan koodiin):`Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`
6. Lisää seuraavat **appSetttings** osa **App.config** -tiedostoa. Näitä asetuksia käytetään avustaja-menetelmällä: **GetAuthorizationHeader**. 

    Korvaa arvot ** &lt;Sovellustunnus&gt;**, ** &lt;salasanan&gt;**, ** &lt;Tilaustunnus&gt;**, ja ** &lt;vuokraaja tunnus&gt; ** oman arvoilla. 

        <?xml version="1.0" encoding="utf-8" ?>
        <configuration>
            <startup> 
                <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
            </startup>
            <appSettings>
                <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
                <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
                <add key="WindowsManagementUri" value="https://management.core.windows.net/" />

                <add key="ApplicationId" value="your application ID" />
                <add key="Password" value="Password you used while creating the AAD application" />
                <add key="SubscriptionId" value= "Subscription ID" />
                <add key="ActiveDirectoryTenantId" value="Tenant ID" />
            </appSettings>
        </configuration>
6. Lisää seuraavat **käyttämällä** lauseet projektin lähdetiedosto (Program.cs).

        using System.Threading;
        using System.Configuration;
        using System.Collections.ObjectModel;
        
        using Microsoft.Azure.Management.DataFactories;
        using Microsoft.Azure.Management.DataFactories.Models;
        using Microsoft.Azure.Management.DataFactories.Common.Models;
        
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure;
6. Lisää seuraava koodi, joka luo erillisen **DataPipelineManagementClient** luokan **Main** -menetelmää. Tätä objektia voit luoda tietojen factory, linkitetyn palvelu tai syöttö- ja tietojoukkoja putkijohto. Tätä objektia käyttää myös seurannassa sektoreita tietojoukko suorituksen aikana.    

            // create data factory management client
            string resourceGroupName = "ADFTutorialResourceGroup";
            string dataFactoryName = "APITutorialFactory";

            TokenCloudCredentials aadTokenCredentials =
                new TokenCloudCredentials(
                    ConfigurationManager.AppSettings["SubscriptionId"],
                    GetAuthorizationHeader());

            Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

            DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);

    > [AZURE.IMPORTANT] 
    > Korvaa **resourceGroupName** arvosta Azure resurssiryhmän nimi. 
    > 
    > Päivitä tiedot factory (**dataFactoryName**) on yksilöllinen nimi. Tietoja factory nimen on oltava yksilöivä. Data Factory palvelutiedot nimeämiskäytäntö säännöt [Tietojen Factory - nimeäminen säännöt](data-factory-naming-rules.md) aiheessa. 

7. Lisää seuraava koodi, joka luo **tietojen factory** **Main** -menetelmää.

            // create a data factory
            Console.WriteLine("Creating a data factory");
            client.DataFactories.CreateOrUpdate(resourceGroupName,
                new DataFactoryCreateOrUpdateParameters()
                {
                    DataFactory = new DataFactory()
                    {
                        Name = dataFactoryName,
                        Location = "westus",
                        Properties = new DataFactoryProperties() { }
                    }
                }
            );

8. Lisää seuraava koodi, joka luo **Azuren tallennustilaan linkitetty palvelun** **Main** -menetelmää. 

    > [AZURE.IMPORTANT] Korvaa **storageaccountname** ja **accountkey** nimen ja avaimen Azure-tallennustilan tilin. 

            // create a linked service for input data store: Azure Storage
            Console.WriteLine("Creating Azure Storage linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureStorageLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            new AzureStorageLinkedService("DefaultEndpointsProtocol=https;AccountName=<storageaccountname>;AccountKey=<accountkey>")
                        )
                    }
                }
            );
9. Lisää seuraava koodi, joka luo **Azure SQL-linkitetty palvelun** **Main** -menetelmää.
 
    > [AZURE.IMPORTANT] Korvaa **palvelimen nimi**, **nimi**, **käyttäjänimi**ja **salasana** nimet Azure SQL-palvelimeen, tietokannan, käyttäjä ja salasana.  

            // create a linked service for output data store: Azure SQL Database
            Console.WriteLine("Creating Azure SQL Database linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureSqlLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            new AzureSqlDatabaseLinkedService("Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30")
                        )
                    }
                }
            );
10. Lisää seuraava koodi, joka luo **pää** -menetelmän **syöttö- ja tietojoukkoja** . 

            // create input and output datasets
            Console.WriteLine("Creating input and output datasets");
            string Dataset_Source = "DatasetBlobSource";
            string Dataset_Destination = "DatasetAzureSqlDestination";

            Console.WriteLine("Creating input dataset of type: Azure Blob");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,


            new DatasetCreateOrUpdateParameters()
            {
                Dataset = new Dataset()
                {
                    Name = Dataset_Source,
                    Properties = new DatasetProperties()
                    {
                        Structure = new List<DataElement>()
                        {
                            new DataElement() { Name = "FirstName", Type = "String" },
                            new DataElement() { Name = "LastName", Type = "String" }
                        },
                        LinkedServiceName = "AzureStorageLinkedService",
                        TypeProperties = new AzureBlobDataset()
                        {
                            FolderPath = "adftutorial/",
                            FileName = "emp.txt"
                        },
                        External = true,
                        Availability = new Availability()
                        {
                            Frequency = SchedulePeriod.Hour,
                            Interval = 1,
                        },

                        Policy = new Policy()
                        {
                            Validation = new ValidationPolicy()
                            {
                                MinimumRows = 1
                            }
                        }
                    }
                }
            });


            Console.WriteLine("Creating output dataset of type: Azure SQL");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new DatasetCreateOrUpdateParameters()
                {
                    Dataset = new Dataset()
                    {
                        Name = Dataset_Destination,
                        Properties = new DatasetProperties()
                        {
                            Structure = new List<DataElement>()
                            {
                                new DataElement() { Name = "FirstName", Type = "String" },
                                new DataElement() { Name = "LastName", Type = "String" }
                            },
                            LinkedServiceName = "AzureSqlLinkedService",
                            TypeProperties = new AzureSqlTableDataset()
                            {
                                TableName = "emp"
                            },

                            Availability = new Availability()
                            {
                                Frequency = SchedulePeriod.Hour,
                                Interval = 1,
                            },
                        }
                    }
                });

11. Lisää seuraava koodi, **Luo ja aktivoi putkijohto** **Main** -menetelmää. Tämä myyntijakso on **CopyActivity** , joka vie **BlobSource** lähteenä ja **BlobSink** käsittelytoiminto.

            // create a pipeline
            Console.WriteLine("Creating a pipeline");
            DateTime PipelineActivePeriodStartTime = new DateTime(2016, 8, 9, 0, 0, 0, 0, DateTimeKind.Utc);
            DateTime PipelineActivePeriodEndTime = PipelineActivePeriodStartTime.AddMinutes(60);
            string PipelineName = "ADFTutorialPipeline";

            client.Pipelines.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new PipelineCreateOrUpdateParameters()
                {
                    Pipeline = new Pipeline()
                    {
                        Name = PipelineName,
                        Properties = new PipelineProperties()
                        {
                            Description = "Demo Pipeline for data transfer between blobs",

                    // Initial value for pipeline's active period. With this, you won't need to set slice status
                    Start = PipelineActivePeriodStartTime,
                            End = PipelineActivePeriodEndTime,

                            Activities = new List<Activity>()
                            {
                                new Activity()
                                {
                                    Name = "BlobToAzureSql",
                                    Inputs = new List<ActivityInput>()
                                    {
                                        new ActivityInput() {
                                            Name = Dataset_Source
                                        }
                                    },
                                    Outputs = new List<ActivityOutput>()
                                    {
                                        new ActivityOutput()
                                        {
                                            Name = Dataset_Destination
                                        }
                                    },
                                    TypeProperties = new CopyActivity()
                                    {
                                        Source = new BlobSource(),
                                        Sink = new BlobSink()
                                        {
                                            WriteBatchSize = 10000,
                                            WriteBatchTimeout = TimeSpan.FromMinutes(10)
                                        }
                                    }
                                }
                            },
                        }
                    }
                }); 

12. Lisää seuraava koodi **Main** -menetelmä, jolla tiedot sektoria tulosteen DataSet tilan. On vain tässä esimerkissä odotettu sektoria.   
 
            // Pulling status within a timeout threshold
            DateTime start = DateTime.Now;
            bool done = false;

            while (DateTime.Now - start < TimeSpan.FromMinutes(5) && !done)
            {
                Console.WriteLine("Pulling the slice status");
                // wait before the next status check
                Thread.Sleep(1000 * 12);

                var datalistResponse = client.DataSlices.List(resourceGroupName, dataFactoryName, Dataset_Destination,
                    new DataSliceListParameters()
                    {
                        DataSliceRangeStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString(),
                        DataSliceRangeEndTime = PipelineActivePeriodEndTime.ConvertToISO8601DateTimeString()
                    });

                foreach (DataSlice slice in datalistResponse.DataSlices)
                {
                    if (slice.State == DataSliceState.Failed || slice.State == DataSliceState.Ready)
                    {
                        Console.WriteLine("Slice execution is done with status: {0}", slice.State);
                        done = true;
                        break;
                    }
                    else
                    {
                        Console.WriteLine("Slice status is: {0}", slice.State);
                    }
                }
            }

14. Lisää seuraava koodi Hae suorittaa tietojen sektoria tiedot **Main** -menetelmää.

            Console.WriteLine("Getting run details of a data slice");

            // give it a few minutes for the output slice to be ready
            Console.WriteLine("\nGive it a few minutes for the output slice to be ready and press any key.");
            Console.ReadKey();

            var datasliceRunListResponse = client.DataSliceRuns.List(
                    resourceGroupName,
                    dataFactoryName,
                    Dataset_Destination,
                    new DataSliceRunListParameters()
                    {
                        DataSliceStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString()
                    }
                );

            foreach (DataSliceRun run in datasliceRunListResponse.DataSliceRuns)
            {
                Console.WriteLine("Status: \t\t{0}", run.Status);
                Console.WriteLine("DataSliceStart: \t{0}", run.DataSliceStart);
                Console.WriteLine("DataSliceEnd: \t\t{0}", run.DataSliceEnd);
                Console.WriteLine("ActivityId: \t\t{0}", run.ActivityName);
                Console.WriteLine("ProcessingStartTime: \t{0}", run.ProcessingStartTime);
                Console.WriteLine("ProcessingEndTime: \t{0}", run.ProcessingEndTime);
                Console.WriteLine("ErrorMessage: \t{0}", run.ErrorMessage);
            }

            Console.WriteLine("\nPress any key to exit.");
            Console.ReadKey();

13. Lisää seuraavat helper tapa, jolla **ohjelma** luokkaan **Main** -menetelmää.  
 
        public static string GetAuthorizationHeader()
        {
            AuthenticationResult result = null;
            var thread = new Thread(() =>
            {
                try
                {
                    var context = new AuthenticationContext(ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] + ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);

                    ClientCredential credential = new ClientCredential(ConfigurationManager.AppSettings["ApplicationId"], ConfigurationManager.AppSettings["Password"]);
                    result = context.AcquireToken(resource: ConfigurationManager.AppSettings["WindowsManagementUri"], clientCredential: credential);
                }
                catch (Exception threadEx)
                {
                    Console.WriteLine(threadEx.Message);
                }
            });

            thread.SetApartmentState(ApartmentState.STA);
            thread.Name = "AcquireTokenThread";
            thread.Start();
            thread.Join();

            if (result != null)
            {
                return result.AccessToken;
            }

            throw new InvalidOperationException("Failed to acquire token");
        }  


15. Napsauta ratkaisunhallinnassa Laajenna projektin (**DataFactoryAPITestApp**), **viittaukset**hiiren kakkospainikkeella ja valitse **Lisää viittaus**. Valitse "**System.Configuration**" kokoonpano-valintaruutu ja valitse **OK**. 
16. Luoda console-sovellus. Valitse-valikosta **Luo** ja valitse **Luo ratkaisu**. 
16. Varmista, että siellä on vähintään yksi tiedosto **adftutorial** säilössä Azuren Blob-objektien tallennustilaan. Jos ei ole, Luo tiedosto, **Emp.txt** Muistiossa seuraavan sisällön ja lataa se adftutorial säilöön.

        John, Doe
        Jane, Doe
     
17. Suorittaminen malli napsauttamalla **Virheenkorjaus** -> **Käynnistä virheenkorjaus** -valikosta. Kun näet **käytön suorittaa tietojen muotoilu tiedot**, odota muutama minuutti ja paina **ENTER**-näppäintä. 
18. Azure portaalin avulla voit varmistaa, että tietojen factory **APITutorialFactory** luodaan sisältää seuraavat tiedot: 
    - Linkitetty service: **LinkedService_AzureStorage** 
    - Tietojoukko: **DatasetBlobSource** ja **DatasetBlobDestination**.
    - Myyntijakso: **PipelineBlobSample** 
18. Tarkista, että kaksi työntekijän tietueisiin luodaan "**emp**" taulukon määritetyn Azure SQL-tietokantaan.

## <a name="next-steps"></a>Seuraavat vaiheet

- Lue [Tietojen siirtämistä toimintoja](data-factory-data-movement-activities.md) artikkeli, joka on kopio tehtävän opetusohjelman käytit yksityiskohtaisia tietoja.
- Hae [Tiedot Factory .NET API viiteoppaasta](https://msdn.microsoft.com/library/mt415893.aspx) lisätietoja tietojen Factory .NET SDK. Tässä artikkelissa ei käsitellä kaikki tiedot Factory .NET API. 

 
