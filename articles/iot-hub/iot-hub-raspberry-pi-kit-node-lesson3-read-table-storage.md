<properties
 pageTitle="Luetut viestit samanlainen Azuren tallennustilaan | Microsoft Azure"
 description="Valvoa laitteen cloud-viestejä, kun ne on kirjoitettu Azure-taulukkotallennus."
 services="iot-hub"
 documentationCenter=""
 authors="shizn"
 manager="timlt"
 tags=""
 keywords=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/21/2016"
 ms.author="xshi"/>

# <a name="33-read-messages-persisted-in-azure-storage"></a>3.3 Lue viestit samanlainen Azuren tallennustilaan

## <a name="331-what-will-you-do"></a>3.3.1 mitä tehdä

Seurata laitteen cloud viestit, jotka lähetetään vadelma pii-3 IoT-toiminnossa, kun viestejä kirjoitetaan Azure-taulukkotallennus. Jos täytät ilmenee ongelmia, Etsi ratkaisuja [sivun vianmääritys](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="332-what-will-you-learn"></a>3.3.2 Mitä on tietoja

- Opit käyttämään gulp Lue viesti tehtävän luettujen viestien samanlainen Azure-taulukkotallennus.

## <a name="333-what-do-you-need"></a>3.3.3 mitä on hyvä

- On onnistui edellisessä osassa [vadelma pii-3 Azure vilkunta malli-sovelluksen käyttämiseen](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md).

## <a name="334-read-new-messages-from-your-storage-account"></a>3.3.4 uusien viestien lukeminen tallennustilan tililtä

Edellisessä osassa suoritit oman pii-sovelluksen malli. Esimerkki sovelluksen lähetetyt viestit Azure IoT keskittimeen. IoT-toiminnossa lähetetyt viestit tallennetaan Azuren taulukkojen varastoon Azure funktio-sovelluksella. Tarvitset Azuren tallennustilaan yhteysmerkkijonoa Azure-taulukkotallennus viestien lukeminen.

Jos haluat lukea Azuren taulukkojen tallennustilaan tallennettuja, toimi seuraavasti:

1. Hae yhteysmerkkijonon suorittamalla seuraavat komennot:

    ```bash
    az storage account list -g iot-sample --query [].name
    az storage account show-connection-string -g iot-sample -n {storage name}
    ```

    Ensimmäisen komennon noutaa `storage name` , käytetään tietoyhteyden yhteysmerkkijonon toisen-komentoa. `iot-sample`Oletusarvo on `{resource group name}` Jos ole muuttanut Harjoitus 2 arvon.

2. Avaa määritystiedoston `config-raspberrypi.json` Visual Studio Code tiedoston suorittamalla seuraavan komennon:

    ```bash
    # For Windows command prompt
    code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json

    # For macOS or Ubuntu
    code ~/.iot-hub-getting-started/config-raspberrypi.json
    ```

3. Korvaa `[Azure Storage connection string]` yhteysmerkkijonolla olet saanut vaiheessa 1.
4. Tallenna `config-raspberrypi.json` tiedosto.
5. Lähetä uudelleen ja lukea niitä Azure-taulukkotallennus suorittamalla seuraavan komennon:

    ```bash
    gulp run --read-storage
    ```

    Logiikan Azure-taulukkotallennus luettaessa ei `azure-table.js` tiedosto.

    ![Suorita--gulp luku-tallennustilan](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_read_message.png)

## <a name="335-summary"></a>3.3.5 yhteenveto

Olet yhteydessä oman pii IoT-toiminnossa pilveen ja käyttää sovelluksen vilkunta malli laitteen cloud viestien lähettämiseen. Azure funktion sovellus käyttää myös tallentamiseen IoT-toiminnossa saapuvien viestien Azure-taulukkotallennus. Voit siirtää seuraava Harjoitus lähettämisestä cloud laitteen viestien IoT-toiminnosta oman pii.

## <a name="next-steps"></a>Seuraavat vaiheet

[Harjoitus 4: Cloud laitteen viestien lähettäminen](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md)
