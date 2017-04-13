<properties
    pageTitle="Luo malli Azure Resurssienhallinta ja PowerShell IoT-toiminnossa | Microsoft Azure"
    description="Katso tämä opetusohjelma, Aloita Azure Resurssienhallinta mallien avulla voit luoda IoT-toiminnossa PowerShellin avulla."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="multiple"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/07/2016"
     ms.author="dobett"/>

# <a name="create-an-iot-hub-using-powershell"></a>Luo PowerShellin IoT-toiminnossa

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Johdanto

Voit luoda ja hallita Azure IoT keskittimet ohjelmallisesti Azure Resurssienhallinta. Tässä opetusohjelmassa näytetään, miten Azure Resurssienhallinta-mallin avulla voit luoda IoT-toiminnossa PowerShellin avulla.

> [AZURE.NOTE] Azure on kaksi eri käyttöönoton mallien luominen ja käyttäminen resurssit: [Azure Resurssienhallinta ja perinteinen](../resource-manager-deployment-model.md).  Tässä artikkelissa käsitellään käyttämällä Azure resurssien hallinnan käyttöönottomalli.

Tässä opetusohjelmassa suorittamiseen tarvitset seuraavat:

- Azure active tili. <br/>Jos sinulla ei ole tiliä, voit luoda [ilmaisen tilin] [ lnk-free-trial] -vain muutaman minuutin.
- [Microsoft Azure PowerShell 1.0] [ lnk-powershell-install] tai uudempi versio.

> [AZURE.TIP] On artikkelissa [Azure PowerShellin Azure resurssien hallinnan] [ lnk-powershell-arm] on lisätietoja PowerShell-komentosarjojen ja Azure Resurssienhallinta mallien käyttäminen luominen Azure resurssit. 

## <a name="connect-to-your-azure-subscription"></a>Yhteyden muodostaminen Azure tilauksen

Kirjoita PowerShell-komentokehote Azure-tilaukseen kirjautuminen seuraava komento:

```
Login-AzureRmAccount
```

Saat tietää, missä voit ottaa IoT-toiminnosta ja tuetut API-versiot käyttää seuraavia komentoja:

```
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).Locations
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).ApiVersions
```

Luo resurssiryhmä sisältävät oman IoT-toiminnossa seuraavan komennon avulla jossakin IoT keskittimeen tuetut sijainneista. Tässä esimerkissä luodaan kutsutaan **MyIoTRG1**resurssiryhmä:

```
New-AzureRmResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="submit-an-azure-resource-manager-template-to-create-an-iot-hub"></a>Lähetä Azure Resurssienhallinta-mallin luominen IoT-toiminnossa

JSON-mallin avulla voit luoda IoT-toiminnossa resurssi-ryhmässä. Voit käyttää myös Azure Resurssienhallinta-malli voit tehdä muutoksia aiemmin IoT-toiminnossa.

1. Tekstieditorissa avulla voit kutsua **template.json** seuraavat resurssien määritelmää, voit luoda uuden normaalin IoT-toiminnossa Azure Resurssienhallinta-mallin luominen. Tässä esimerkissä Lisää IoT-toiminnossa **Yhdysvaltojen Itä** -alueella, luo kaksi kuluttajaryhmiä (**cg1** ja **cg2**) tapahtumaa-toiminnossa yhteensopivan päätepisteen ja käyttää **2016-02-03** API-versiota. Tämän mallin odottaa voit välittää IoT keskittimeen nimessä parametrina kutsutaan **hubName**. Katso nykyinen luettelo sijainneista, jotka tukevat IoT keskittimeen [Azure tila][lnk-status].

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
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg1')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg2')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
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

2. Tallenna Azure Resurssienhallinta mallitiedoston paikallisessa tietokoneessa. Tässä esimerkissä oletetaan, se tallennetaan **c:\templates**-nimiseen kansioon.

3. Suorita seuraava komento ottamaan yhteyttä uusi IoT-toiminnossa kulkeva IoT-toiminnossa nimi parametrina. Tässä esimerkissä IoT-toiminnossa on **abcmyiothub** (Huomaa, että nimi on oltava yksilöivä niin, että se sisältää oma nimesi tai nimikirjaimet):

    ```
    New-AzureRmResourceGroupDeployment -ResourceGroupName MyIoTRG1 -TemplateFile C:\templates\template.json -hubName abcmyiothub
    ```

4. Tulos näyttää näppäimet, joita olet luonut IoT-toiminnossa.

5. Voit varmistaa, että sovelluksesi lisätty uusi IoT pääsivusto ohjesisältöä [Azure portal] [ lnk-azure-portal] ja resurssien tai **Hae AzureRmResource** PowerShell-cmdlet-komennon avulla luettelon tarkasteleminen.

> [AZURE.NOTE] Tässä esimerkissä-sovelluksen Lisää S1 vakio IoT keskittimeen, jossa on laskutettu. Voit poistaa palvelun [Azure portal] IoT-toiminnossa[ lnk-azure-portal] tai **Poista AzureRmResource** PowerShell-cmdlet-komennon avulla, kun olet valmis.

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt Azure Resurssienhallinta-mallin avulla PowerShellin IoT-toiminnosta on otettu käyttöön, voit tutustua tarkemmin:

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
[lnk-powershell-arm]: ../powershell-azure-resource-manager.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
