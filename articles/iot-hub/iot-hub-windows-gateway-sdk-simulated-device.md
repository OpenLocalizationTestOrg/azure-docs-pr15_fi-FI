<properties
    pageTitle="Simuloida laitetta, jonka IoT yhdyskäytävän SDK | Microsoft Azure"
    description="Azure IoT yhdyskäytävän SDK ongelmatilanteita Windowsin esitä lähettämisen telemetriatietojen Simuloitu laitteesta Azure IoT yhdyskäytävän SDK: N avulla."
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/29/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta--send-device-to-cloud-messages-with-a-simulated-device-using-windows"></a>Azure IoT yhdyskäytävän SDK (beta) – lähettämiseen laitteen pilvipalveluun käyttämällä Windows Simuloitu laitteen kanssa

[AZURE.INCLUDE [iot-hub-gateway-sdk-simulated-selector](../../includes/iot-hub-gateway-sdk-simulated-selector.md)]

## <a name="build-and-run-the-sample"></a>Luoda ja suorittaa otosten

Ennen aloittamista, toimi seuraavasti:

- [Kehitysympäristön määritys] [ lnk-setupdevbox] käsittelyyn Windows SDK.
- [Luo IoT-toiminnossa] [ lnk-create-hub] Azure-tilaukseesi tarvitset oman keskittimeen vaiheiden suorittamiseen nimi. Jos sinulla ei ole tiliä, voit luoda [ilmaisen tilin] [ lnk-free-trial] -vain muutaman minuutin.
- Kahta lisääminen IoT-toiminnosta ja niiden tunnukset ja laitteen avaimet muistiin. Voit käyttää [laitteen Explorer tai iothub explorer] [ lnk-explorer-tools] laitteet lisääminen edellisessä vaiheessa luomasi IoT-toiminnosta ja hakea niiden näppäimet-työkalu.

Voit muodostaa otosten seuraavasti:

1. Avaa komentokehote **VS2015 Developer komentorivi-ikkuna** .
2. Siirry **azure-iot-yhdyskäytävä-sdk** -säilön paikallisen tietokannan pääkansio.
3. Suorita **Työkalut\\build.cmd** komentosarjan. Tämä komentosarja luo tiedoston ratkaisun Visual Studiossa, muodostaa ratkaisun ja suorittaa testejä. Löytyvät Visual Studio-ratkaisun **azure-iot-yhdyskäytävä-sdk** -säilön paikallisen tietokannan **luominen** -kansiossa.

Voit suorittaa otosten seuraavasti:

Avaa tekstieditorissa, tiedosto **näytteiden\\simulated_device_cloud_upload\\src\\simulated_device_cloud_upload_win.json** paikallisen tietokannan **azure-iot-yhdyskäytävä-sdk** -säilön. Tämä tiedosto määrittää moduulit otoksen yhdyskäytävän:

- **IoTHub** -moduulin muodostaa yhteyden IoT-toiminnossa. Sinun on määritettävä niiden tietojen lähettäminen IoT-toiminnossa. Tarkemmin sanottuna **IoTHubName** arvo IoT-toiminnossa nimi ja määritä **IoTHubSuffix** arvo **azure devices.net**. Aseta arvoksi yksi **Transport** : "HTTP", "AMQP" tai "MQTT". Huomaa, että tällä hetkellä vain "HTTP" jakaa yhden TCP-yhteyden laitteen viestit. Jos määrität arvon "AMQP" tai "MQTT", yhdyskäytävän ylläpitää erillistä TCP-yhteyden IoT keskittimeen kunkin laitteen.
- **Yhdistäminen** -moduulin yhdistää Simuloitu laitteilla MAC-osoitteet IoT keskittimeen laite-tunnukset. Varmista, että **deviceId** arvot vastaavat tunnukset IoT-toiminnossa on lisätty laitteet ja **deviceKey** arvoja sisältävät näppäimet kaksi laitteilla.
- **BLE1** ja **BLE2** moduulit ovat Simuloitu laitteita. Huomaa, kuinka niiden MAC-osoitteet vastaavat **Yhdistäminen** -moduulissa.
- **Lokin** moduulin kirjaa yhdyskäytävän tehtävän tiedostoon.
- **Moduulin polku** arvot alla oletetaan, että monistaa IoT yhdyskäytävän SDK **c** -asema varmenteiden säilön. Jos latasit toiseen sijaintiin, sinun on muutettava **moduulin polku** arvot vastaavasti.
- JSON-tiedoston alareunassa **linkit** -taulukko yhdistää **BLE1** ja **BLE2** moduulit **Yhdistäminen** -moduulin ja **Yhdistäminen** moduulin **IoTHub** -moduuli. Se myös varmistaa, että kaikki viestit kirjaamat **lokin** moduuli.

```
{
    "modules" :
    [ 
        {
            "module name" : "IoTHub",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\iothub\\Debug\\iothub_hl.dll",
            "args" : 
            {
                "IoTHubName" : "{Your IoT hub name}",
                "IoTHubSuffix" : "azure-devices.net",
                "Transport": "HTTP"
            }
        },
        {
            "module name" : "mapping",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\identitymap\\Debug\\identity_map_hl.dll",
            "args" : 
            [
                {
                    "macAddress" : "01-01-01-01-01-01",
                    "deviceId"   : "{Device ID 1}",
                    "deviceKey"  : "{Device key 1}"
                },
                {
                    "macAddress" : "02-02-02-02-02-02",
                    "deviceId"   : "{Device ID 2}",
                    "deviceKey"  : "{Device key 2}"
                }
            ]
        },
        {
            "module name":"BLE1",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\simulated_device\\Debug\\simulated_device_hl.dll",
            "args":
            {
                "macAddress" : "01-01-01-01-01-01"
            }
        },
        {
            "module name":"BLE2",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\simulated_device\\Debug\\simulated_device_hl.dll",
            "args":
            {
                "macAddress" : "02-02-02-02-02-02"
            }
        },
        {
            "module name":"Logger",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\logger\\Debug\\logger_hl.dll",
            "args":
            {
                "filename":"C:\\azure-iot-gateway-sdk\\deviceCloudUploadGatewaylog.log"
            }
        }
    ],
    "links" : [
        { "source" : "*", "sink" : "Logger" },
        { "source" : "BLE1", "sink" : "mapping" },
        { "source" : "BLE2", "sink" : "mapping" },
        { "source" : "mapping", "sink" : "IoTHub" }
    ]
}
```

Tallenna määritystiedosto tekemäsi muutokset.

Voit suorittaa otosten seuraavasti:

1. Komentokehotteeseen Siirry **azure-iot-yhdyskäytävä-sdk** -tietovarasto paikallisen kopion pääkansioon.
2. Suorita seuraava komento:
  
    ```
    build\samples\simulated_device_cloud_upload\Debug\simulated_device_cloud_upload_sample.exe samples\simulated_device_cloud_upload\src\simulated_device_cloud_upload_win.json
    ```

3. Voit käyttää [laitteen Explorer tai iothub explorer] [ lnk-explorer-tools] työkalun viestit, jotka IoT keskittimeen vastaanottaa yhdyskäytävän seurannassa.


## <a name="next-steps"></a>Seuraavat vaiheet

Halutessasi artikkelista monimutkaisemman ymmärtämisen IoT yhdyskäytävän SDK ja kokeilla koodiesimerkkejä seuraavasta developer opetusohjelmassa ja resurssit:

- [Laitteen cloud viestien lähettäminen reaali laitetta, jonka IoT yhdyskäytävän SDK-paketissa][lnk-physical-device]
- [Azure IoT yhdyskäytävän SDK-paketissa][lnk-gateway-sdk]

Tutustu tarkemmin IoT keskittimeen ominaisuuksia, katso:

- [Sovelluskehittäjän opas][lnk-devguide]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/

[lnk-physical-device]: iot-hub-gateway-sdk-physical-device.md

[lnk-devguide]: ./iot-hub-devguide.md
[lnk-create-hub]: iot-hub-create-through-portal.md 