<properties
    pageTitle="Virta Analytics hallinta .NET SDK | Microsoft Azure"
    description="Aloittaminen Stream Analytics hallinta .NET SDK. Lue, miten voit määrittää ja analytics töiden: Luo projektin, syötteiden, tulostaa ja muunnoksia."
    keywords=".NET SDK-paketissa, analytics Ohjelmointirajapinta"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>


# <a name="management-net-sdk-set-up-and-run-analytics-jobs-using-the-azure-stream-analytics-api-for-net"></a>Hallinta .NET SDK: Määrittäminen ja Azure Stream Analytics-Ohjelmointirajapinnan käyttäminen .NET analytics töiden suorittaminen

Opi määrittämään Stream Analytics-Ohjelmointirajapinnan käyttäminen hallinta .NET SDK käyttämällä .NET Suorita analytics-työt. Projektin määrittäminen, Luo syöttö- ja lähteiden, muunnoksia ja Käynnistä ja Lopeta työt. Analytics-töiden voit käyttää tietoja Blob-objektien tallennustilaan tai tapahtuma-toiminnosta.

Katso [.NET Stream Analytics-Ohjelmointirajapinta hallinnan ohjeet](https://msdn.microsoft.com/library/azure/dn889315.aspx).

Azure Stream Analytics on täysin hallitun palvelu tarjoaa pieni viive erittäin käytettävissä skaalattava, monimutkaisia käsittely päälle streaming tietojen pilveen. Virta Analytics avulla asiakkaat voivat määrittäminen streaming töitä voit analysoida tietoja virtaa ja mahdollistaa lähes reaaliajassa analytics asema.  


## <a name="prerequisites"></a>Edellytykset
Ennen kuin aloitat tämän artikkelin, sinulla on oltava seuraavasti:

- Asenna Visual Studio 2012 tai 2013.
- Lataa ja asenna [Azure.NET SDK](https://azure.microsoft.com/downloads/).
- Luo Azure-resurssiryhmä-tilaukseesi. Seuraavassa on esimerkki Azure PowerShell-komentosarjaa. PowerShellin Azure Lisätietoja on artikkelissa [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md);  


        # Log in to your Azure account
        Add-AzureAccount

        # Select the Azure subscription you want to use to create the resource group
        Select-AzureSubscription -SubscriptionName <subscription name>

            # If Stream Analytics has not been registered to the subscription, remove the remark symbol (#) to run the Register-AzureRMProvider cmdlet to register the provider namespace
            #Register-AzureRMProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>


-   Määritä lähde- ja tulostuksen kohteen käyttämään. Ohjeita kohdassa futher [Lisää syötteiden](stream-analytics-add-inputs.md) määrittämään malli-syöte ja [Lisää tulostaa](stream-analytics-add-outputs.md) otoksen output: n määrittäminen.


## <a name="set-up-a-project"></a>Projektin määrittäminen

Voit luoda analytics-työ käyttämällä .NET, Määritä projektin Stream Analytics-Ohjelmointirajapinnan.

1. Visual Studio C# .NET console-sovelluksen luominen
2. Paketin hallinta-konsolin suorittamalla seuraavat komennot NuGet pakettien asentamisen. Ensimmäinen on Azure Stream Analytics hallinta .NET SDK. Toinen on Azure Active Directory-asiakas, jota käytetään todennusta varten.

        Install-Package Microsoft.Azure.Management.StreamAnalytics
        Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory

4. Lisää App.config-tiedostoa **appSettings** seuraavassa osassa:

        <appSettings>
          <!--CSM Prod related values-->
          <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
          <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
          <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
          <add key="AsaClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
          <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
          <add key="SubscriptionId" value="YOUR AZURE SUBSCRIPTION" />
          <add key="ActiveDirectoryTenantId" value="YOU TENANT ID" />
        </appSettings>


    Arvojen korvaaminen **SubscriptionId** ja **ActiveDirectoryTenantId** Azure-tilaus ja vuokraaja tunnukset. Voit hankkia nämä arvot suorittamalla seuraavan Azure PowerShell cmdlet-komennon:

        Get-AzureAccount

5. Lisää seuraavat **käyttämällä** lauseet projektin lähdetiedosto (Program.cs):

        using System;
        using System.Configuration;
        using System.Threading;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.StreamAnalytics;
        using Microsoft.Azure.Management.StreamAnalytics.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

6.  Lisää avustaja todentamismenetelmä:

        public static string GetAuthorizationHeader()
        {
            AuthenticationResult result = null;
            var thread = new Thread(() =>
            {
                try
                {
                    var context = new AuthenticationContext(
                        ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] +
                        ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);

                    result = context.AcquireToken(
                        resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
                        clientId: ConfigurationManager.AppSettings["AsaClientId"],
                        redirectUri: new Uri(ConfigurationManager.AppSettings["RedirectUri"]),
                        promptBehavior: PromptBehavior.Always);
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


## <a name="create-a-stream-analytics-management-client"></a>Luo Stream Analytics management-asiakas

**StreamAnalyticsManagementClient** objektin avulla voit hallita työn ja projektin osia, kuten syöttö, tulostus ja muunnos.

Lisää seuraava koodi **Main** -menetelmä alkuun:

    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamAnalyticsJobName = "<YOUR STREAM ANALYTICS JOB NAME>";
    string streamAnalyticsInputName = "<YOUR JOB INPUT NAME>";
    string streamAnalyticsOutputName = "<YOUR JOB OUTPUT NAME>";
    string streamAnalyticsTransformationName = "<YOUR JOB TRANSFORMATION NAME>";

    // Get authentication token
    TokenCloudCredentials aadTokenCredentials =
        new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader());

    // Create Stream Analytics management client
    StreamAnalyticsManagementClient client = new StreamAnalyticsManagementClient(aadTokenCredentials);

**ResourceGroupName** muuttuja-arvon pitäisi olla sama kuin resurssiryhmä luoda tai poimia valmistelevat toimenpiteet nimi.

Voit automatisoida tunnistetiedon esityksen näkökohdat työpaikkojen viitata [todennustapa palvelun lyhennys Azure resurssien hallinta](../resource-group-authenticate-service-principal.md).

Tässä artikkelissa jäljellä olevien osien oletetaan, että koodi on alkuun, **Main** -menetelmää.

## <a name="create-a-stream-analytics-job"></a>Luo Stream Analytics-työ

Seuraava koodi luo Stream Analytics-työ, jotka olet määrittänyt resurssin-ryhmästä. Lisää syöte, tulostus ja muunnos työn myöhemmin.

    // Create a Stream Analytics job
    JobCreateOrUpdateParameters jobCreateParameters = new JobCreateOrUpdateParameters()
    {
        Job = new Job()
        {
            Name = streamAnalyticsJobName,
            Location = "<LOCATION>",
            Properties = new JobProperties()
            {
                EventsOutOfOrderPolicy = EventsOutOfOrderPolicy.Adjust,
                Sku = new Sku()
                {
                    Name = "Standard"
                }
            }
        }
    };

    JobCreateOrUpdateResponse jobCreateResponse = client.StreamingJobs.CreateOrUpdate(resourceGroupName, jobCreateParameters);


## <a name="create-a-stream-analytics-input-source"></a>Virta-analyysin syöttö lähteen luominen

Seuraava koodi luo blob lähdetyyppi ja CSV Sarjatoiminto Stream Analytics lähde. Voit luoda tapahtumaa-toiminnossa lähde käyttämällä **EventHubStreamInputDataSource** **BlobStreamInputDataSource**sijaan. Voit mukauttaa vastaavasti syötteen lähteen Sarjatoiminto tyyppi.

    // Create a Stream Analytics input source
    InputCreateOrUpdateParameters jobInputCreateParameters = new InputCreateOrUpdateParameters()
    {
        Input = new Input()
        {
            Name = streamAnalyticsInputName,
            Properties = new StreamInputProperties()
            {
                Serialization = new CsvSerialization
                {
                    Properties = new CsvSerializationProperties
                    {
                        Encoding = "UTF8",
                        FieldDelimiter = ","
                    }
                },
                DataSource = new BlobStreamInputDataSource
                {
                    Properties = new BlobStreamInputDataSourceProperties
                    {
                        StorageAccounts = new StorageAccount[]
                        {
                            new StorageAccount()
                            {
                                AccountName = "<YOUR STORAGE ACCOUNT NAME>",
                                AccountKey = "<YOUR STORAGE ACCOUNT KEY>"
                            }
                        },
                        Container = "samples",
                        PathPattern = ""
                    }
                }
            }
        }
    };

    InputCreateOrUpdateResponse inputCreateResponse =
        client.Inputs.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, jobInputCreateParameters);

Lähteitä-Blob-objektien tallennustilaan tai tapahtumaa-toiminnossa on liitetty tiettyyn projektiin. Jos haluat käyttää samaa lähde eri töiden, kutsua menetelmää uudelleen ja eri projektin nimi.


## <a name="test-a-stream-analytics-input-source"></a>Testaa Stream Analytics lähde

**TestConnection** menetelmä Testaa, onko Stream Analytics-työ on pystyttävä muodostamaan yhteys lähde sekä muita ominaisuuksia, jotka ovat tietyn syötteen lähteen tyyppi. Esimerkiksi blob syötteen lähteessä aiemmassa vaiheessa luomasi-menetelmä tarkistaa, että tallennustilan tilin nimi ja avaimen pari avulla voidaan muodostaa tallennustilan tilin sekä Tarkista määritetyssä säilössä on olemassa.

    // Test input source connection
    DataSourceTestConnectionResponse inputTestResponse =
        client.Inputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsInputName);

## <a name="create-a-stream-analytics-output-target"></a>Luo Stream Analytics tulosteen kohde

Tulostuksen kohteen luominen muistuttaa luominen Stream Analytics lähde. Lähteitä, kuten tulosteen kohteet on liitetty tiettyyn projektiin. Jos haluat käyttää samaa tulostuksen kohteen eri töiden, kutsua menetelmää uudelleen ja eri projektin nimi.

Seuraava koodi luo tulostuksen-kohteen (Azure SQL-tietokanta). Voit mukauttaa tulostuksen kohteen tietotyyppi ja/tai Sarjatoiminto tyyppi.

    // Create a Stream Analytics output target
    OutputCreateOrUpdateParameters jobOutputCreateParameters = new OutputCreateOrUpdateParameters()
    {
        Output = new Output()
        {
            Name = streamAnalyticsOutputName,
            Properties = new OutputProperties()
            {
                DataSource = new SqlAzureOutputDataSource()
                {
                    Properties = new SqlAzureOutputDataSourceProperties()
                    {
                        Server = "<YOUR DATABASE SERVER NAME>",
                        Database = "<YOUR DATABASE NAME>",
                        User = "<YOUR DATABASE LOGIN>",
                        Password = "<YOUR DATABASE LOGIN PASSWORD>",
                        Table = "<YOUR DATABASE TABLE NAME>"
                    }
                }
            }
        }
    };

    OutputCreateOrUpdateResponse outputCreateResponse =
        client.Outputs.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, jobOutputCreateParameters);

## <a name="test-a-stream-analytics-output-target"></a>Testaa Stream Analytics tulosteen kohde

Virta Analytics tulosteen kohde on myös testikäyttöön yhteydet **TestConnection** -menetelmää.

    // Test output target connection
    DataSourceTestConnectionResponse outputTestResponse =
        client.Outputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsOutputName);

## <a name="create-a-stream-analytics-transformation"></a>Luo Stream Analytics-muunnos

Seuraava koodi luo Stream Analytics-muunnos kyselyn kanssa "Valitse * syötetyistä" ja määrittää varata yhden streaming yksikön Stream Analytics projektille. Lisätietoja säätäminen streaming yksiköt-kohdassa [asteikko Azure Stream Analytics työt](stream-analytics-scale-jobs.md).


    // Create a Stream Analytics transformation
    TransformationCreateOrUpdateParameters transformationCreateParameters = new TransformationCreateOrUpdateParameters()
    {
        Transformation = new Transformation()
        {
            Name = streamAnalyticsTransformationName,
            Properties = new TransformationProperties()
            {
                StreamingUnits = 1,
                Query = "select * from Input"
            }
        }
    };

    var transformationCreateResp =
        client.Transformations.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, transformationCreateParameters);

Syötteen ja tulosteen, kuten muunnoksia liittyy myös tiettyä projektia Stream Analytics-kohdassa linkitettyä.

## <a name="start-a-stream-analytics-job"></a>Aloita Stream Analytics-työ
Kun olet luonut Stream Analytics-työ ja sen tulo tai tulot, lähtö tai lähdöt ja muunnos, voit aloittaa työn **Käynnistä** -menetelmällä.

Seuraava esimerkki koodin alkaa Stream Analytics työn mukautettu tulostus alkamisaika ja määritä 12. joulukuuta 2012 12:12:12 UTC-ajan:

    // Start a Stream Analytics job
    JobStartParameters jobStartParameters = new JobStartParameters
    {
        OutputStartMode = OutputStartMode.CustomTime,
        OutputStartTime = new DateTime(2012, 12, 12, 0, 0, 0, DateTimeKind.Utc)
    };

    LongRunningOperationResponse jobStartResponse = client.StreamingJobs.Start(resourceGroupName, streamAnalyticsJobName, jobStartParameters);



## <a name="stop-a-stream-analytics-job"></a>Lopeta Stream Analytics-työ
Voit pysäyttää käynnissä Stream Analytics-työ **Lopeta** -menetelmällä.

    // Stop a Stream Analytics job
    LongRunningOperationResponse jobStopResponse = client.StreamingJobs.Stop(resourceGroupName, streamAnalyticsJobName);

## <a name="delete-a-stream-analytics-job"></a>Poista Stream Analytics-työ
**Poista** -menetelmä poistaa työn sekä pohjana aliraportti resurssit, kuten tulo tai tulot ja lähtö tai lähdöt muunnos työn.

    // Delete a Stream Analytics job
    LongRunningOperationResponse jobDeleteResponse = client.StreamingJobs.Delete(resourceGroupName, streamAnalyticsJobName);


## <a name="get-support"></a>Käytä tukipalveluita
Saat lisäohjeita voit kokeilla Microsoftin [Azure Stream Analytics-keskustelupalstalla](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Seuraavat vaiheet

Olet Koulujen luominen ja suorittaminen analytics työt .NET SDK käytön perusteet. Lisätietoja on seuraavissa artikkeleissa:

- [Azure Stream Analytics esittely](stream-analytics-introduction.md)
- [Azure Stream Analytics käytön aloittaminen](stream-analytics-get-started.md)
- [Skaalaa Azure Stream Analytics töitä](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics hallinta .NET SDK](https://msdn.microsoft.com/library/azure/dn889315.aspx).
- [Azure Stream Analytics kyselyn kieliohje](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics hallinta REST API-viittaus](https://msdn.microsoft.com/library/azure/dn835031.aspx)


<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[azure.blob.storage]: http://azure.microsoft.com/documentation/services/storage/
[azure.blob.storage.use]: http://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/

[azure.event.hubs]: http://azure.microsoft.com/services/event-hubs/
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.forum]: http://go.microsoft.com/fwlink/?LinkId=512151

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.developer.guide]: stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
