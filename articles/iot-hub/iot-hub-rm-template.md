<properties
    pageTitle="Luo ARM-malli ja C# IoT-toiminnossa | Microsoft Azure"
    description="Katso tämä opetusohjelma, Aloita Azure Resurssienhallinta mallien avulla voit luoda IoT-toiminnossa C#-ohjelmalla."
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

# <a name="create-an-iot-hub-using-a-c-program-with-an-azure-resource-manager-template"></a>Luo C#-ohjelman käyttäminen Azure Resurssienhallinta-mallin IoT-toiminnossa

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Johdanto

Voit luoda ja hallita Azure IoT keskittimet ohjelmallisesti Azure Resurssienhallinta. Tässä opetusohjelmassa näytetään, miten Azure Resurssienhallinta-mallin avulla voit luoda IoT-toiminnossa C#-ohjelmasta.

> [AZURE.NOTE] Azure on kaksi eri käyttöönoton mallien luominen ja käyttäminen resurssit: [Azure Resurssienhallinta ja perinteinen](../resource-manager-deployment-model.md).  Tässä artikkelissa käsitellään käyttämällä Azure resurssien hallinnan käyttöönottomalli.

Tässä opetusohjelmassa suorittamiseen tarvitset seuraavat:

- Microsoft Visual Studio 2015.
- Azure active tili. <br/>Jos sinulla ei ole tiliä, voit luoda [ilmaisen tilin] [ lnk-free-trial] -vain muutaman minuutin.
- [Azure-tallennustilan tilin] [ lnk-storage-account] kohtaa, johon voidaan tallentaa Azure Resurssienhallinta-mallitiedostot.
- [Microsoft Azure PowerShell 1.0] [ lnk-powershell-install] tai uudempi versio.

[AZURE.INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>Visual Studio projektin valmisteleminen

1. Visual Studiossa Luo Visual C# Windows projekti **Konsolisovelluksen** project-mallin avulla. Projektin **CreateIoTHub**nimi.

2. Napsauta ratkaisunhallinnassa projektin hiiren kakkospainikkeella ja valitse sitten **NuGet pakettien hallinta**.

3. Nuget paketin hallinta Tarkista **Sisällytä prerelease**ja etsiä **Microsoft.Azure.Management.ResourceManager**. Valitse **Asenna**, **Tarkastele muutoksia** valitsemalla **OK**ja valitse sitten **hyväksyn** Hyväksy käyttöoikeudet.

4. Nuget paketin hallinta Hae **Microsoft.IdentityModel.Clients.ActiveDirectory**.  Valitse **Asenna**, **Tarkastele muutoksia** valitsemalla **OK**ja valitse sitten **hyväksyn** Hyväksy käyttöoikeussopimus.

5. Program.cs, valitse Korvaa aiemmin luotu **käyttäen** lausekkeita seuraavasti:

    ```
    using System;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```
    
6. Lisää paikkamerkki-arvojen korvaaminen seuraavat staattinen muuttujat Program.cs. Olet tehnyt **ApplicationId**, **SubscriptionId**, **TenantId**ja **salasana** muistiin aiemmin tässä opetusohjelmassa. **Your Azure-tallennustilan tilin nimi** on Azure-tallennustilan tilin nimi Azure Resurssienhallinta mallin tiedostojen tallennuspaikka. **Resurssiryhmän nimi** on nimeä käytetään silloin, kun luot IoT-toiminnossa resurssiryhmä, voi olla olemassa resurssiryhmä tai uusi tunnus. **Käyttöönoton nimi** on käyttöönoton, kuten **Deployment_01**.

    ```
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId}";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";
    static string storageAddress = "https://{Your storage account name}.blob.core.windows.net";
    static string rgName = "{Resource group name}";
    static string deploymentName = "{Deployment name}";
    ```

[AZURE.INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## <a name="submit-an-azure-resource-manager-template-to-create-an-iot-hub"></a>Lähetä Azure Resurssienhallinta-mallin luominen IoT-toiminnossa

JSON mallin ja parametri-tiedoston avulla voit luoda IoT-toiminnossa resurssi-ryhmässä. Voit käyttää myös Azure Resurssienhallinta-malli voit tehdä muutoksia aiemmin IoT-toiminnossa.

1. Napsauta ratkaisunhallinnassa projektin hiiren kakkospainikkeella, valitsemalla **Lisää**ja valitse **Uusi kohde**. Lisää JSON tiedoston nimeltä **template.json** lisääminen projektiin.

2. Korvaa **template.json** sisällön seuraavaa resurssin määritelmää, vakio IoT-toiminnossa lisääminen **Yhdysvaltojen Itä** -alueen (Katso nykyisen luettelon alueista, jotka tukevat IoT keskittimeen [Azure tila][lnk-status]):

    ```
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": {
          "type": "string"
        }
      },
      "resources": [
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs",
        "name": "[parameters('hubName')]",
        "location": "East US",
        "sku": {
          "name": "S1",
          "tier": "Standard",
          "capacity": 1
        },
        "properties": {
          "location": "East US"
        }
      }
      ],
      "outputs": {
        "hubKeys": {
          "value": "[listKeys(resourceId('Microsoft.Devices/IotHubs', parameters('hubName')), '2016-02-03')]",
          "type": "object"
        }
      }
    }
    ```

3. Napsauta ratkaisunhallinnassa projektin hiiren kakkospainikkeella, valitsemalla **Lisää**ja valitse **Uusi kohde**. Lisää JSON tiedoston nimeltä **parameters.json** projektiin.

4. Korvaa **parameters.json** sisällön seuraavat parametritiedot, joka määrittää uusi IoT pääsivusto nimi, kuten **{nimikirjaimesi} mynewiothub**. Huomaa, että IoT keskittimeen nimen on oltava yksilöivä niin, että se sisältää oma nimesi tai nimikirjaimet):

    ```
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": { "value": "mynewiothub" }
      }
    }
    ```

5. **Palvelimen Explorer**muodostaa Azure-tilauksesi ja Azure-tallennustilan tilin luoda säilön eli **malleja**. **Ominaisuudet** -ruudussa **Blob**-arvoksi **mallien** säilö **julkisen** lukuoikeudet.

6. **Palvelimen Explorer**- **Mallit** -säilöä hiiren kakkospainikkeella ja valitse sitten **Näytä Blob-säilö**. **Lataa Blob** -painiketta, valitse kaksi tiedostoa, **parameters.json** ja **templates.json**, ja valitse sitten **Avaa** JSON-tiedostojen lataaminen **mallien** säilö. URL-osoitteet BLOB-objektit, joissa JSON tiedot ovat seuraavat:

    ```
    https://{Your storage account name}.blob.core.windows.net/templates/parameters.json
    https://{Your storage account name}.blob.core.windows.net/templates/template.json
    ```

7. Lisää Program.cs seuraavalla tavalla:
    
    ```
    static void CreateIoTHub(ResourceManagementClient client)
    {
        
    }
    ```

5. Lisää seuraava koodi lähettää mallin ja parametri tiedostot Azure Resurssienhallinta **CreateIoTHub** -menetelmää:

    ```
    var createResponse = client.Deployments.CreateOrUpdate(
        rgName,
        deploymentName,
        new Deployment()
        {
          Properties = new DeploymentProperties
          {
            Mode = DeploymentMode.Incremental,
            TemplateLink = new TemplateLink
            {
              Uri = storageAddress + "/templates/template.json"
            },
            ParametersLink = new ParametersLink
            {
              Uri = storageAddress + "/templates/parameters.json"
            }
          }
        });
    ```

6. Lisää seuraava koodi, joka näyttää tila ja uusi IoT pääsivusto näppäimet **CreateIoTHub** tapaan:

    ```
    string state = createResponse.Properties.ProvisioningState;
    Console.WriteLine("Deployment state: {0}", state);

    if (state != "Succeeded")
    {
      Console.WriteLine("Failed to create iothub");
    }
    Console.WriteLine(createResponse.Properties.Outputs);
    ```

## <a name="complete-and-run-the-application"></a>Täydennä ja suorita sovellus

Sovelluksen suorittamista nyt **CreateIoTHub** kutsumista, ennen kuin luoda ja suorittaa sen.

1. Lisää seuraava koodi **Main** -menetelmä loppuun:

    ```
    CreateIoTHub(client);
    Console.ReadLine();
    ```
    
2. Valitse **Muodosta** ja **luoda ratkaisu**. Korjaa virheitä.

3. Valitse **Korjaa** ja **Käynnistä virheenkorjaus** sovelluksen käyttämiseen. Voi kestää useita minuutteja suorittaa käyttöönottoa varten.

4. Voit varmistaa, että sovelluksesi lisätty uusi IoT pääsivusto ohjesisältöä [Azure portal] [ lnk-azure-portal] ja resurssien tai **Hae AzureRmResource** PowerShell-cmdlet-komennon avulla luettelon tarkasteleminen.

> [AZURE.NOTE] Tässä esimerkissä-sovelluksen Lisää S1 vakio IoT keskittimeen, jossa on laskutettu. Voit poistaa palvelun [Azure portal] IoT-toiminnossa[ lnk-azure-portal] tai **Poista AzureRmResource** PowerShell-cmdlet-komennon avulla, kun olet valmis.

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt Azure Resurssienhallinta-mallin avulla C#-ohjelmalla IoT-toiminnosta on otettu käyttöön, voit tutustua tarkemmin:

- Lisätietoja [IoT keskittimeen resurssin tarjoajaan REST API]ominaisuuksia[lnk-rest-api].
- Lue [Azure Resurssienhallinta Yleiset] [ lnk-azure-rm-overview] lisätietoja ominaisuuksia Azure resurssien hallinta.

Lisätietoja kehittäminen IoT toiminnosta on seuraavissa artikkeleissa:

- [C SDK esittely][lnk-c-sdk]
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
[lnk-storage-account]: ../storage/storage-create-storage-account.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
