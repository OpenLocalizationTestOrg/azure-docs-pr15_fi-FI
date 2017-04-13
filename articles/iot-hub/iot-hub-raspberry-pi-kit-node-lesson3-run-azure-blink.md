<properties
 pageTitle="Suorita laitteen cloud viestien lähettämiseen sovelluksen malli | Microsoft Azure"
 description="Ottaa käyttöön ja suorita oman vadelma pii-3, joka lähettää viestejä IoT keskittimeen ja vilkkuu LED sovelluksen malli."
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

# <a name="32-run-sample-application-to-send-device-to-cloud-messages"></a>3.2 Suorita laitteen cloud viestien lähettämiseen sovelluksen malli

## <a name="321-what-you-will-do"></a>3.2.1 mitä hyötyä

Käyttöönotto ja käyttää sovelluksen malli-että vadelma pii-3, joka lähettää viestejä IoT-toiminnossa. Jos täytät ilmenee ongelmia, Etsi ratkaisuja [sivun vianmääritys](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="322-what-you-will-learn"></a>3.2.2 mitä opit

- Miten käyttöönotto ja käyttää Node.js-sovelluksen malli-kohdassa pii gulp-työkalun avulla.

## <a name="323-what-you-need"></a>3.2.3 mitä tarvitset

- On onnistui edellisessä osassa tämän oppitunnissa: [Luo sovelluksen Azure-funktio ja Azure-tallennustilan tilin käsittelemään ja IoT keskittimeen viestien tallentaminen](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md).

## <a name="324-get-your-iot-hub-and-device-connection-strings"></a>3.2.4 Hae IoT-toiminnosta ja laitteen yhteysmerkkijonot

Laitteen yhteysmerkkijonon avulla luodaan yhteys pii IoT keskittimeen. IoT-toiminnossa yhteysmerkkijono avulla luodaan yhteys IoT-toiminnossa laitteen käyttäjätietoja, joka vastaa omaa pii IoT-toiminnossa.

- Hae IoT keskittimeen yhteysmerkkijonon suorittamalla seuraava Azure CLI-komento:

```bash
az iot hub show-connection-string --name {my hub name} --resource-group iot-sample
```

`{my hub name}`on nimi, jonka olet määrittänyt Harjoitus 2. Käytä `iot-sample` arvona `{resource group name}` Jos ole muuttanut Harjoitus 2 arvon.

- Pyydä laitteen yhteysmerkkijonon suorittamalla seuraavan komennon:

```bash
az iot device show-connection-string --hub {my hub name} --device-id myraspberrypi --resource-group iot-sample
```

`{my hub name}`Avaa saman arvon kuin tiliä, jota käytit edellä-komennolla. Käytä `iot-sample` arvona `{resource group name}` ja käyttää `myraspberrypi` arvona `{device id}` Jos ole muuttanut Harjoitus 2 arvon.

## <a name="325-configure-the-device-connection"></a>3.2.5 laite-yhteyden määrittäminen

1. Alusta määritystiedosto suorittamalla seuraavat komennot:

    ```bash
    npm install
    gulp init
    ```

2. Avaa laitteen määritystiedoston `config-raspberrypi.json` Visual Studio koodissa suorittamalla seuraavan komennon:

    ```bash
    # For Windows command prompt
    code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
  
    # For macOS or Ubuntu
    code ~/.iot-hub-getting-started/config-raspberrypi.json
    ```

    ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson3/config.png)

3. Tee seuraavat korvauksia- `config-raspberrypi.json` tiedosto:

  - Laitteen IP-osoite tai isäntänimi saatu **[laitteen isäntänimi tai IP-osoite]** korvaaminen `device-discovery-cli` tai perittyjä Harjoitus 1 määritetty arvo.
  - Korvaa **[IoT laitteen yhteysmerkkijonon]** `device connection string` olet saanut.
  - Korvaa **[IoT keskittimeen yhteysmerkkijonon]** `iot hub connection string` olet saanut.

Voit päivittää `config-raspberrypi.json` tiedoston niin, että voit ottaa käyttöön sovelluksen malli tietokoneesta.

## <a name="326-deploy-and-run-the-sample-application"></a>3.2.6 käyttöön ja suorita sovelluksen malli

Ottaa käyttöön ja käyttää sovelluksen malli-että pii suorittamalla seuraavan komennon:

```bash
gulp
```

> [AZURE.NOTE] Oletusarvon gulp suoritetaan `install-tools`, `deploy`, ja `run` tehtävät järjestyksessä. [Harjoitus 1](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)suoritit näiden tehtävien peräkkäin erikseen.

## <a name="327-verify-the-sample-application-works"></a>3.2.7 Tarkista malli-sovelluksen toiminta

Raportissa pitäisi näkyä LED, joka on liitetty oman pii vilkkuvaa kahden sekunnin välein. Aina, kun LED vilkkuu-sovelluksen malli lähettää viestin IoT-toiminnosta ja varmistaa, että viesti on lähetetty onnistuneesti IoT-toiminnossa. Lisäksi kunkin IoT-toiminnossa vastaanottanut viestin, viesti on tulostettu konsoli-ikkunassa. Sovelluksen malli päättyy automaattisesti 20 viestit lähettämisen jälkeen.

![](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_run.png)

## <a name="328-summary"></a>3.2.8 yhteenveto

Olet käyttöön ja suorita uusi blink-sovelluksen malli oman piillä IoT keskittimeen laitteen cloud viestien lähettämiseen. Voit siirtää seuraavaan osaan seurannassa viestit Azure-tallennustilan tilin sellaisenaan.

## <a name="next-steps"></a>Seuraavat vaiheet

[3.3 Lue viestit samanlainen Azuren tallennustilaan](iot-hub-raspberry-pi-kit-node-lesson3-read-table-storage.md)