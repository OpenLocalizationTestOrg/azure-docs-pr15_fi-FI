<properties
 pageTitle="Luoda ja ottaa käyttöön blink-sovellus | Microsoft Azure"
 description="Kloonaa otoksen Node.js sovelluksen Github ja ottaa käyttöön sovelluksen vadelma pii 3 tauluun gulp. Tämän esimerkkisovelluksen vilkkuu LED yhteydessä tauluun kahden sekunnin välein."
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

# <a name="13-create-and-deploy-the-blink-application"></a>1.3 luominen ja vilkunta sovelluksen käyttöönotto

## <a name="131-what-you-will-do"></a>1.3.1 mitä hyötyä

Esimerkki Node.js sovelluksen Github Kloonaa ja ottaa käyttöön sovelluksen malli vadelma pii-3 gulp-työkalun avulla. Sovelluksen malli vilkkuu LED yhteydessä tauluun kahden sekunnin välein. Jos täytät ilmenee ongelmia, Etsi ratkaisuja [sivun vianmääritys](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="132-what-you-will-learn"></a>1.3.2 mitä opit

- Opi käyttämään `device-discover-cli` verkko tietoja oman pii-työkalu.
- Miten käyttöönotto ja käyttää sovelluksen malli-kohdassa pii.
- Käyttöönottamisesta ja virheenkorjaus sovellusten etäyhteyden oman pii.

## <a name="133-what-you-need"></a>1.3.3 mitä tarvitset

On onnistui seuraa osiot Harjoitus 1:

- [Laitteen määrittäminen](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md)
- [Hanki työkalut](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)

## <a name="134-obtain-the-ip-address-and-host-name-of-your-pi"></a>1.3.4 hankkia IP-osoite ja isännän nimen, että pii

Avaa komentorivi-Windows- tai Mac OS-tai Ubuntu pääte-ikkuna ja suorittamalla seuraava komento:

```bash
devdisco list --eth
```

Pitäisi näkyä tulos, joka on seuraavanlainen:

![laitteen etsiminen](media/iot-hub-raspberry-pi-lessons/lesson1/device_discovery.png)

Huomaa `IP address` ja `hostname` oman piin. Tarvitset näitä tietoja jäljempänä tässä osassa.

> [AZURE.NOTE] Varmista, että samassa verkossa tietokoneesi on yhteydessä oman pii. Esimerkiksi jos tietokone on yhteydessä langattomaan verkkoon, että pii ollessa yhdistettynä kiinteää verkkoa, et välttämättä devdisco tulosteen IP-osoite.

## <a name="135-clone-the-sample-application"></a>1.3.5 Kloonaa sovelluksen malli

Avaa mallikoodi, toimi seuraavasti:

1. Esimerkki säilö-Github Kloonaa suorittamalla seuraavan komennon:

    ```bash
    git clone https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started.git
    ```

2. Avaa Visual Studio koodi sovelluksen malli suorittamalla seuraavat komennot:

    ```bash
    cd iot-hub-node-raspberrypi-getting-started
    cd Lesson1
    code .
    ```

![Repo rakenne](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-blink-mac.png)

`app.js` -Tiedoston `app` alikansio on avaimen lähdetiedosto, joka sisältää ohjausobjektiin LED koodia.

### <a name="136-install-application-dependencies"></a>1.3.6 asentaa sovelluksen riippuvuudet

Asenna kirjastot ja muut moduulit tarvitset sovelluksen malli varten suorittamalla seuraavan komennon:

```bash
npm install
```

## <a name="137-configure-the-device-connection"></a>1.3.7 laite-yhteyden määrittäminen

Laitteen-yhteyden määrittäminen seuraavasti:

1. Luo laitteen kokoonpanotiedosto suorittamalla seuraavan komennon:

    ```bash
    gulp init
    ```

    Määritystiedoston `config-raspberrypi.json` on käyttäjän tunnistetiedot, joita käytät kirjautuessasi oman pii. Käyttäjän tunnistetietoja muistivuodon välttämiseksi määritystiedosto luodaan alikansion `.iot-hub-getting-started` kotiverkon kansion tietokoneessa.

2. Avaa laitteen kokoonpanotiedosto Visual Studio Code suorittamalla seuraavan komennon:

    ```bash
    # For Windows command prompt
    code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json

    # For macOS or Ubuntu
    code ~/.iot-hub-getting-started/config-raspberrypi.json
    ```

3. Korvaa paikkamerkki `[device hostname or IP address]` IP-osoite tai isäntänimi, jonka saat 1.3.4-osassa.

    ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-config-mac.png)

Onnittelen! Olet luonut oman pii sovelluksen ensimmäisen malli.

## <a name="138-deploy-and-run-the-sample-application"></a>1.3.8 käyttöönotto ja suorita sovelluksen malli

### <a name="1381-install-nodejs-and-npm-on-your-pi"></a>1.3.8.1 oman pii asentaa Node.js ja NPM

Asenna Node.js ja NPM oman pii suorittamalla seuraavan komennon:

```bash
gulp install-tools
```

Kymmenen minuuttia ensimmäistä kertaa, voit suorittaa tämän tehtävän voi kestää.

### <a name="1382-deploy-and-run-the-sample-app"></a>1.3.8.2 käyttöön ja suorita malli-sovellus

Ottaa käyttöön ja käyttää sovelluksen malli-suorittamalla seuraavan komennon:

```bash
gulp deploy && gulp run
```

### <a name="1383-verify-the-app-works"></a>1.3.8.3 Tarkista sovelluksen toiminta

Pitäisi tulla näkyviin LED oman piillä vilkkuvaa kahden sekunnin välein.  Jos et näe vilkkuvaa LED, on artikkelissa [vianmääritysoppaan](iot-hub-raspberry-pi-kit-node-troubleshooting.md) ratkaisu yleisimpiin ongelmiin.
![LED vilkkuvaa](media/iot-hub-raspberry-pi-lessons/lesson1/led_blinking.jpg)

> [AZURE.NOTE] Käytä `Ctrl + C` Lopeta sovellus.

## <a name="139-summary"></a>1.3.9 yhteenveto

Olet asentanut oman pii-käyttöä varten tarvittavat työkalut ja tiedot-kohdassa pii otoksen sovelluksen vilkkuu LED käytössä. Voit nyt siirtää seuraava Harjoitus luominen, ottaminen käyttöön ja suorita toisen sovelluksen malli, joka muodostaa yhteyttä pii Azure IoT-toiminnossa voit lähettää ja vastaanottaa viestejä.

## <a name="next-steps"></a>Seuraavat vaiheet

Olet nyt valmiina [saada Azure työkaluja](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md) Harjoitus 2, joka alkaa aloittaminen
