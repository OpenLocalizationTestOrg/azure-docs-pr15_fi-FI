<properties
 pageTitle="Suorita cloud laitteen viestien malli-sovellus | Microsoft Azure"
 description="Harjoitus 4 malli-sovellus toimii oman pii ja valvoo saapuvien viestien IoT-toiminnosta. Uuden tehtävän gulp lähettää viestejä oman pii IoT-toiminnosta LED vilkkuu."
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

# <a name="41-run-the-sample-application-to-receive-cloud-to-device-messages"></a>4.1 cloud laitteen viestien malli-sovelluksen käyttämiseen

Tässä osassa käyttöönottoa vadelma pii-3-sovelluksen malli. Sovelluksen malli valvoo saapuvien viestien IoT-toiminnosta. Voit myös suorittamalla gulp tehtävän tietokoneessa lähettää viestejä oman pii IoT-toiminnosta. Yhteydessä vastaanota viestit-sovelluksen malli vilkkuu LED. Jos täytät ilmenee ongelmia, Etsi ratkaisuja [sivun vianmääritys](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="411-what-you-will-do"></a>4.1.1 mitä hyötyä

- Yhdistä sovelluksen IoT keskittimeen malli.
- Ottaa käyttöön ja suorita sovelluksen malli.
- Lähettää viestejä IoT-toiminnosta oman pii LED vilkkuu.

## <a name="412-what-you-will-learn"></a>4.1.2 mitä opit

- Voit valvoa saapuvien viestien IoT-toiminnosta.
- Voit lähettää viestejä cloud laitteen IoT-toiminnosta oman pii. 

## <a name="413-what-do-you-need"></a>4.1.3 mitä on hyvä

- Vadelma pii 3, joka on määritetty käytettäväksi. Lisätietoja oman pii määrittämisestä on artikkelissa [Harjoitus 1: vadelma pii 3 laitteen käytön aloittaminen](iot-hub-raspberry-pi-kit-node-get-started.md)
- IoT-keskittimeen, joka on luotu Azure-tilaukseesi. Lisätietoja Azure IoT-toiminnossa luomisesta on artikkelissa [Harjoitus 2: Luo Azure IoT-toiminnossa](iot-hub-raspberry-pi-kit-node-get-started.md)

## <a name="414-connect-the-sample-application-to-your-iot-hub"></a>4.1.4 yhteyden IoT keskittimeen sovelluksen malli

1. Varmista, että repo-kansiossa on `iot-hub-node-raspberrypi-getting-started`. Avaa Visual Studio koodi sovelluksen malli suorittamalla seuraavat komennot:

    ```bash
    cd Lesson4
    code .
    ```

    Ilmoitus `app.js` -tiedoston `app` alikansioon. `app.js` Tiedosto on avaimen lähdetiedosto, joka sisältää koodin seurannassa saapuvien viestien IoT-toiminnosta. `blinkLED` Funktion vilkkuu LED.

    ![Repo rakenne](media/iot-hub-raspberry-pi-lessons/lesson4/repo_structure.png)

2. Alusta kokoonpanotiedosto seuraavia komentoja:

    ```bash
    npm install
    gulp init
    ```

    Jos olet suorittanut tämän tietokoneen Harjoitus 3, kaikki määritykset periytyvät, jotta voit siirtyä vaiheeseen 4.1.5. Harjoitus 3 valmis toiseen tietokoneeseen, jos haluat korvata paikkamerkit `config-raspberrypi.json` tiedosto. `config-raspberrypi.json` Tiedosto on koti-kansion alikansio.

    ![Config](media/iot-hub-raspberry-pi-lessons/lesson4/config_raspberrypi.png)

- Korvaa **[laitteen isäntänimi tai IP-osoite]** oman pii IP-osoite tai isäntänimi, voit avata-komennon suorittaminen`devdisco list --eth`
- Korvaa **[IoT laitteen yhteysmerkkijonon]** laitteen yhteysmerkkijonon, jonka saat suorittamalla komennon `az iot hub show-connection-string --name {my hub name} --resource-group {resource group name}`.
- Korvaa **[IoT keskittimeen yhteysmerkkijonon]** IoT keskittimeen yhteysmerkkijonon, jonka saat suorittamalla komennon `az iot device show-connection-string --hub {my hub name} --device-id {device id} --resource-group {resource group name}`.

## <a name="415-deploy-and-run-the-sample-application"></a>4.1.5 käyttöön ja suorita sovelluksen malli

Käyttöönotto ja käyttää sovelluksen malli-kohdassa pii suorittamalla seuraavat komennot:
  
```
gulp
```

Gulp-komennon suorittaminen Asenna työkalut tehtävän ensimmäisen kerran. Valitse se ottaa käyttöön sovelluksen malli-kohdassa pii. Lopuksi suorituskerran sovellus että pii ja erillinen tehtävä tietokoneeseen 20 vilkunta viestien lähettäminen oman pii IoT-toiminnosta.

Esimerkkisovellus suoritetaan, kun se käynnistyy listening viestien IoT-toiminnosta. Tänä aikana gulp tehtävän lähettää useita "vilkunta" viestien IoT-toiminnosta oman pii. Kunkin vilkunta viesti-sovelluksen malli kutsuu vilkkuu LED blinkLED-funktiota.

Raportissa pitäisi näkyä vilkkuvaa kahden sekunnin välein, kuten gulp tehtävän lähettää 20 viestejä IoT-toiminnosta oman pii LED. Viimeinen on "Lopeta"-sanoma, joka kertoo sovelluksen lopettavat.

![Gulp](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_blink.png)

## <a name="416-summary"></a>4.1.6 yhteenveto

Olet onnistuneesti lähetettyjen viestien IoT-toiminnosta oman pii LED vilkkuu. Seuraavassa osassa on valinnainen osa, joka näytetään, miten voit muuttaa käyttöön tai poistaminen käytöstä LED toiminnan.

## <a name="next-steps"></a>Seuraavat vaiheet

[Valinnainen osa: käyttöön tai käytöstä LED toiminnan muuttaminen](iot-hub-raspberry-pi-kit-node-lesson4-change-led-behavior.md)