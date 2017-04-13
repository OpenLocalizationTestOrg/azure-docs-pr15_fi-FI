<properties
    pageTitle="Valvoa ohjelmallisesti Stream Analytics työt | Microsoft Azure"
    description="Lue, miten voit valvoa ohjelmallisesti Stream Analytics töitä luotu REST API, Azure SDK tai PowerShellin kautta."
    keywords=".NET-näyttö, työn valvonta-sovelluksen seuranta"
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


# <a name="programmatically-create-a-stream-analytics-job-monitor"></a>Luoda ohjelmallisesti Stream Analytics-työ-näyttö
 Tässä artikkelissa esitellään Stream Analytics projektin seurannan ottaminen käyttöön. Stream Analytics tehdä töitä, jotka on luotu REST API, Azure SDK tai PowerShellin kautta ei ole seuranta oletusarvon mukaan käytössä.  Voit manuaalisesti ottaa tämä Azure-portaalissa siirtyminen projektin näytön sivulle ja valitsemalla Ota käyttöön-painiketta tai tämä automatisoida noudattamalla tämän artikkelin. Tiedot näkyy muodossa Analytics-työn Azure-portaalissa "Näyttö"-välilehti.

![työn näytön työt-välilehti](./media/stream-analytics-monitor-jobs/stream-analytics-monitor-jobs-tab.png)

## <a name="prerequisites"></a>Edellytykset
Ennen kuin aloitat tämän artikkelin, sinulla on oltava seuraavasti:

- Visual Studio 2012 tai 2013.
- Lataa ja asenna [Azure.NET SDK](https://azure.microsoft.com/downloads/).
- Aiemmin luotu Stream Analytics työ on käytössä seuranta.

## <a name="setup-a-project"></a>Projektin asetukset

1.  Visual Studio C# .net console-sovelluksen luominen
2.  Paketin hallinta-konsolin suorittamalla seuraavat komennot NuGet pakettien asentamisen. Ensimmäinen on Azure Stream Analytics hallinta .NET SDK. Toinen on Azure näytön SDK joka käytetään käyttöön seuranta. Viimeinen on Azure Active Directory-asiakas, jota käytetään todennusta varten.

    ```
    Install-Package Microsoft.Azure.Management.StreamAnalytics
    Install-Package Microsoft.Azure.Insights -Pre
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

3.  Lisää seuraavat appSettings osa App.config-tiedostoa.

    ```
    <appSettings>
        <!--CSM Prod related values-->
        <add key="ResourceGroupName" value="RESOURCE GROUP NAME" />
        <add key="JobName" value="YOUR JOB NAME" />
        <add key="StorageAccountName" value="YOUR STORAGE ACCOUNT"/>
        <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
        <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
        <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
        <add key="AsaClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
        <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
        <add key="SubscriptionId" value="YOUR AZURE SUBSCRIPTION ID" />
        <add key="ActiveDirectoryTenantId" value="YOUR TENANT ID" />
    </appSettings>
    ```
Arvojen korvaaminen *SubscriptionId* ja *ActiveDirectoryTenantId* Azure-tilaus ja vuokraaja tunnukset. Voit hankkia nämä arvot suorittamalla seuraavan PowerShell cmdlet-komennon:

    ```
    Get-AzureAccount
    ```
4.  Lisää seuraava lauseiden käyttämisestä projektin lähdetiedosto (Program.cs).

    ```
        using System;
        using System.Configuration;
        using System.Threading;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.Insights;
        using Microsoft.Azure.Management.Insights.Models;
        using Microsoft.Azure.Management.StreamAnalytics;
        using Microsoft.Azure.Management.StreamAnalytics.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
    ```
5.  Lisää avustaja todentamismenetelmä.

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

## <a name="create-management-clients"></a>Luo asiakkaiden hallinta
Seuraava koodi määrittää tarvittavat muuttujat ja asiakkaiden hallinta.

    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamAnalyticsJobName = "<YOUR STREAM ANALYTICS JOB NAME>";

    // Get authentication token
    TokenCloudCredentials aadTokenCredentials =
        new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader());

    Uri resourceManagerUri = new
    Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    // Create Stream Analytics and Insights management client
    StreamAnalyticsManagementClient streamAnalyticsClient = new
    StreamAnalyticsManagementClient(aadTokenCredentials, resourceManagerUri);
    InsightsManagementClient insightsClient = new
    InsightsManagementClient(aadTokenCredentials, resourceManagerUri);

## <a name="enable-monitoring-for-an-existing-stream-analytics-job"></a>Aiemmin luodun Stream Analytics-työn seurannan ottaminen käyttöön

Seuraava koodi otetaan käyttöön seuranta **Stream Analytics-projektille** . Koodin alkuosa suorittaa GET-pyynnössä vastaan Stream Analytics-palvelu noutaa tietoja tietyssä muodossa Analytics-työstä. Se käyttää "Tunnus"-ominaisuuden (hakee GET-pyynnössä) parametrina hyllytetty menetelmän toinen puolet koodin, joka lähettää HYLLYTETTY pyytää tietoja-palvelun käyttöön Stream Analytics Työn seuranta.

> [AZURE.WARNING]
> Jos olet aiemmin ottanut projektille eri Stream Analytics-palvelun Azure-portaalissa tai ohjelmallisesti kautta valvontaa koodi, alla **on suositeltavaa, että annat saman tallennustilan tilin nimi aiemmin käytössä seuranta tehneen.**
>
> Tallennustilan-tili on linkitetty luomasi Stream Analytics työn alue, ei ole nimenomaisesti työn itse.
>
> Kaikki Stream Analytics työn (ja muita Azure resursseja) samalla alueella jakaminen tallentamiseen seurantatiedot tallennustilan tähän tiliin. Jos annat eri tallennustilan tilin, se saattaa aiheuttaa tahattomia vaikutukset seurantaan muut Stream Analytics-työt ja/tai muita Azure resursseja.
>
> Tallennustilan tilin nimi, jota käytetään korvaa ```“<YOUR STORAGE ACCOUNT NAME>”``` alla on oltava tallennustilan-tili, joka on saman tilauksen Stream Analytics projektin voit ottaa käyttöön seurantaa varten.

    // Get an existing Stream Analytics job
    JobGetParameters jobGetParameters = new JobGetParameters()
    {
        PropertiesToExpand = "inputs,transformation,outputs"
    };
    JobGetResponse jobGetResponse = streamAnalyticsClient.StreamingJobs.Get(resourceGroupName, streamAnalyticsJobName, jobGetParameters);

    // Enable monitoring
    ServiceDiagnosticSettingsPutParameters insightPutParameters = new ServiceDiagnosticSettingsPutParameters()
    {
            Properties = new ServiceDiagnosticSettings()
            {
                StorageAccountName = "<YOUR STORAGE ACCOUNT NAME>"
            }
    };
    insightsClient.ServiceDiagnosticSettingsOperations.Put(jobGetResponse.Job.Id, insightPutParameters);



## <a name="get-support"></a>Käytä tukipalveluita
Saat lisäohjeita voit kokeilla Microsoftin [Azure Stream Analytics-keskustelupalstalla](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Seuraavat vaiheet

- [Azure Stream Analytics esittely](stream-analytics-introduction.md)
- [Azure Stream Analytics käytön aloittaminen](stream-analytics-get-started.md)
- [Skaalaa Azure Stream Analytics töitä](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics kyselyn kieliohje](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics hallinta REST API-viittaus](https://msdn.microsoft.com/library/azure/dn835031.aspx)
