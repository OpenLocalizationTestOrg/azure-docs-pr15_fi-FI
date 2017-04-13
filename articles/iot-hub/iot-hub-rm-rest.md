<properties
    pageTitle="Luo IoT-keskittimeen REST-Ohjelmointirajapinnalla | Microsoft Azure"
    description="Katso tämä opetusohjelma, Aloita REST-Ohjelmointirajapinnalla avulla voit luoda IoT-toiminnossa."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/16/2016"
     ms.author="dobett"/>

# <a name="tutorial-create-an-iot-hub-using-a-c-program-and-the-rest-api"></a>Opetusohjelma: Luo C#-ohjelman ja REST-Ohjelmointirajapinnalla IoT-toiminnossa

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Johdanto

Voit käyttää [IoT keskittimeen resurssin tarjoajaan REST API] [ lnk-rest-api] voit luoda ja hallita Azure IoT keskittimet ohjelmallisesti. Tässä opetusohjelmassa kerrotaan, miten IoT keskittimeen resurssin tarjoajaan REST API avulla voit luoda IoT-toiminnossa C#-ohjelmasta.

> [AZURE.NOTE] Azure on kaksi eri käyttöönoton mallien luominen ja käyttäminen resurssit: [Azure Resurssienhallinta ja perinteinen](../resource-manager-deployment-model.md).  Tässä artikkelissa käsitellään käyttämällä Azure resurssien hallinnan käyttöönottomalli.

Tässä opetusohjelmassa suorittamiseen tarvitset seuraavat:

- Microsoft Visual Studio 2015.
- Azure active tili. <br/>Jos sinulla ei ole tiliä, voit luoda [ilmaisen tilin] [ lnk-free-trial] -vain muutaman minuutin.
- [Microsoft Azure PowerShell 1.0] [ lnk-powershell-install] tai uudempi versio.

[AZURE.INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>Visual Studio projektin valmisteleminen

1. Visual Studiossa Luo Visual C# Windows projekti **Konsolisovelluksen** project-mallin avulla. Projektin **CreateIoTHubREST**nimi.

2. Napsauta ratkaisunhallinnassa projektin hiiren kakkospainikkeella ja valitse sitten **NuGet pakettien hallinta**.

3. Nuget paketin hallinta Tarkista **Sisällytä prerelease**ja etsiä **Microsoft.Azure.Management.ResourceManager**. Valitse **Asenna**, **Tarkastele muutoksia** valitsemalla **OK**ja valitse sitten **hyväksyn** Hyväksy käyttöoikeudet.

4. Nuget paketin hallinta Hae **Microsoft.IdentityModel.Clients.ActiveDirectory**.  Valitse **Asenna**, **Tarkastele muutoksia** valitsemalla **OK**ja valitse sitten **hyväksyn** Hyväksy käyttöoikeussopimus.

6. Program.cs, valitse Korvaa aiemmin luotu **käyttäen** lausekkeita seuraavasti:

    ```
    using System;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Text;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Newtonsoft.Json;
    using Microsoft.Rest;
    using System.Linq;
    using System.Threading;
    ```
    
7. Lisää paikkamerkki-arvojen korvaaminen seuraavat staattinen muuttujat Program.cs. Paina **ApplicationId**, **SubscriptionId**, **TenantId**ja **salasana** on tehty aiemmassa Tässä opetusohjelmassa. **Resurssiryhmän nimi** on nimeä käytetään silloin, kun luot IoT-toiminnossa resurssiryhmä, voi olla olemassa resurssiryhmä tai uusi tunnus. **IoT keskittimeen nimi** on nimi, luot, kuten **MyIoTHub** (nimi on oltava yksilöivä, niin, että se sisältää oma nimesi tai nimikirjaimet) IoT-toiminnossa. **Käyttöönoton nimi** on käyttöönoton, kuten **Deployment_01**.

    ```
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId}";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";
    
    static string rgName = "{Resource group name}";
    static string iotHubName = "{IoT Hub name including your initials}";
    ```

[AZURE.INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## <a name="use-the-rest-api-to-create-an-iot-hub"></a>REST-Ohjelmointirajapinnalla avulla voit luoda IoT-toiminnossa

Käytä [IoT keskittimeen REST API] [ lnk-rest-api] IoT-toiminnossa luominen resurssiryhmä. Voit käyttää myös REST-Ohjelmointirajapinta, voit tehdä muutoksia aiemmin IoT-toiminnossa.

1. Lisää Program.cs seuraavalla tavalla:
    
    ```
    static void CreateIoTHub(string token)
    {
        
    }
    ```

2. Lisää seuraava koodi ja Luo objekti **HttpClient** ja otsikot todennus-tunnuksen **CreateIoTHub** -menetelmää:

    ```
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
    ```

3. Lisää seuraava koodi kuvaamaan IoT-toiminnossa luonnissa JSON edustuksen sekä **CreateIoTHub** -menetelmän (Katso nykyinen luettelo sijainneista, jotka tukevat IoT keskittimeen [Azure tila][lnk-status]):

    ```
    var description = new
    {
      name = iotHubName,
      location = "East US",
      sku = new
      {
        name = "S1",
        tier = "Standard",
        capacity = 1
      }
    };
    
    var json = JsonConvert.SerializeObject(description, Formatting.Indented);
    ```

4. Lisää seuraava koodi esittää Azure REST pyynnön, tarkista vastauksen ja hakea URL-Osoitteen avulla voit valvoa käyttöönoton tehtävän tilan **CreateIoTHub** menetelmä:

    ```
    var content = new StringContent(JsonConvert.SerializeObject(description), Encoding.UTF8, "application/json");
    var requestUri = string.Format("https://management.azure.com/subscriptions/{0}/resourcegroups/{1}/providers/Microsoft.devices/IotHubs/{2}?api-version=2016-02-03", subscriptionId, rgName, iotHubName);
    var result = client.PutAsync(requestUri, content).Result;
      
    if (!result.IsSuccessStatusCode)
    {
      Console.WriteLine("Failed {0}", result.Content.ReadAsStringAsync().Result);
      return;
    }
    
    var asyncStatusUri = result.Headers.GetValues("Azure-AsyncOperation").First();
    ```

5. Lisää seuraava koodi **CreateIoTHub** menetelmän edellisessä vaiheessa noutaa **asyncStatusUri** osoitetta käytetään odottamaan suorittamiseen käyttöönottoa varten:

    ```
    string body;
    do
    {
      Thread.Sleep(10000);
      HttpResponseMessage deploymentstatus = client.GetAsync(asyncStatusUri).Result;
      body = deploymentstatus.Content.ReadAsStringAsync().Result;
    } while (body == "{\"status\":\"Running\"}");
    ```

6. Lisää seuraava koodi **CreateIoTHub** -menetelmä noutamiseen IoT-keskittimeen, luoda ja tulostaa ne konsoliin näppäimet loppuun:

    ```
    var listKeysUri = string.Format("https://management.azure.com/subscriptions/{0}/resourceGroups/{1}/providers/Microsoft.Devices/IotHubs/{2}/IoTHubKeys/listkeys?api-version=2015-08-15-preview", subscriptionId, rgName, iotHubName);
    var keysresults = client.PostAsync(listKeysUri, null).Result;
    
    Console.WriteLine("Keys: {0}", keysresults.Content.ReadAsStringAsync().Result);
    ```
    
## <a name="complete-and-run-the-application"></a>Täydennä ja suorita sovellus

Sovelluksen suorittamista nyt **CreateIoTHub** kutsumista, ennen kuin luoda ja suorittaa sen.

1. Lisää seuraava koodi **Main** -menetelmä loppuun:

    ```
    CreateIoTHub(token.AccessToken);
    Console.ReadLine();
    ```
    
2. Valitse **Muodosta** ja **luoda ratkaisu**. Korjaa virheitä.

3. Valitse **Korjaa** ja **Käynnistä virheenkorjaus** sovelluksen käyttämiseen. Voi kestää useita minuutteja suorittaa käyttöönottoa varten.

4. Voit varmistaa, että sovelluksesi lisätty uusi IoT pääsivusto ohjesisältöä [Azure portal] [ lnk-azure-portal] ja resurssien tai **Hae AzureRmResource** PowerShell-cmdlet-komennon avulla luettelon tarkasteleminen.

> [AZURE.NOTE] Tässä esimerkissä-sovelluksen Lisää S1 vakio IoT keskittimeen, jossa on laskutettu. Kun olet valmis, voit poistaa palvelun [Azure portal] IoT-toiminnossa[ lnk-azure-portal] tai **Poista AzureRmResource** PowerShell-cmdlet-komennon avulla, kun olet valmis.

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt REST-Ohjelmointirajapinnan käyttäminen IoT-toiminnosta on otettu käyttöön, voit tutustua tarkemmin:

- Lisätietoja [IoT keskittimeen resurssin tarjoajaan REST API]ominaisuuksia[lnk-rest-api].
- Lue [Azure Resurssienhallinta Yleiset] [ lnk-azure-rm-overview] lisätietoja ominaisuuksia Azure resurssien hallinta.

Lisätietoja kehittäminen IoT toiminnosta on seuraavissa artikkeleissa:

- [Johdanto C SDK-paketissa][lnk-c-sdk]
- [IoT keskittimeen SDK: T][lnk-sdks]

Tutustu tarkemmin IoT keskittimeen ominaisuuksia, katso:

- [Simuloimalla laitetta, jonka IoT yhdyskäytävän SDK-paketissa][lnk-gateway]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-powershell-install]: ../powershell-install-configure.md
[lnk-rest-api]: https://msdn.microsoft.com/library/mt589014.aspx
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
