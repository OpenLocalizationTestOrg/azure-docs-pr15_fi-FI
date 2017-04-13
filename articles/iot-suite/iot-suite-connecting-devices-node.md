<properties
   pageTitle="Yhdistä käyttävän Node.js | Microsoft Azure"
   description="Tässä artikkelissa käsitellään laitteen liittäminen Azure IoT Suite valmiiksi remote seurantaa ratkaisun kirjoitettu Node.js-sovelluksesta."
   services=""
   suite="iot-suite"
   documentationCenter="na"
   authors="dominicbetts"
   manager="timlt"
   editor=""/>

<tags
   ms.service="iot-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/05/2016"
   ms.author="dobett"/>


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-nodejs"></a>Laitteen yhdistäminen remote seurantaa esimääritettyjä ratkaisun (Node.js)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-the-nodejs-sample-solution"></a>Luoda ja suorittaa node.js otoksen ratkaisu

1. Kloonaa *Microsoft Azure IoT SDK: T* GitHub säilö ja asenna *Microsoft Azure IoT laitteen SDK Node.js* työpöytääsi, noudata [kehittäminen ympäristön valmisteleminen] [ lnk-github-prepare] ohjeita.

2. FROM [azure-iot-SDK: t] paikallisen kopion[ lnk-github-repo] säilöön, kopioi seuraavat kaksi tiedostot-solmu ja laitteen/Mallit-kansion kansioon laitteen:

  - Packages.JSON
  - remote_monitoring.js

3. Avaa remote_monitoring.js-tiedosto ja Etsi seuraavan muuttujan:

    ```
    var connectionString = "[IoT Hub device connection string]";
    ```

4. Korvaa laitteen yhteysmerkkijonon **[IoT keskittimeen laitteen yhteysmerkkijonon]** . Voit etsiä arvoja IoT-toiminnossa hostname laitteen tunnus ja laitteen avainta remote seurantaa ratkaisu koontinäytön. Laitteen yhteysmerkkijonon on seuraavanlainen:

    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```

    Jos laitetunnus on **mydevice**IoT keskittimeen isäntänimi on **contoso** , yhteysmerkkijono näyttää seuraavanlaiselta:

    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```

5. Tallenna tiedosto. Suorita seuraavat komennot komentokehotteeseen, joka sisältää Asenna tarvittavat paketit ja suorita sitten malli-sovelluksen tiedostot-kansiossa:

    ```
    npm install --save
    node remote_monitoring.js
    ```

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[lnk-github-repo]: https://github.com/azure/azure-iot-sdks
[lnk-github-prepare]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md