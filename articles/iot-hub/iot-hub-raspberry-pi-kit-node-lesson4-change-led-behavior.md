<properties
 pageTitle="Valinnaisen osan - muuta käyttöön tai pois LED toimintaa | Microsoft Azure"
 description="Voit mukauttaa viestin LED henkilön käyttöön ja poistaminen käytöstä toiminnan muuttaminen."
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

# <a name="42-optional-section-change-the-on-and-off-behavior-of-the-led"></a>4.2 valinnaisen osan: käyttöön tai käytöstä LED toiminnan muuttaminen

## <a name="421-what-you-will-do"></a>4.2.1 mitä hyötyä

Voit mukauttaa viestin LED henkilön käyttöön ja poistaminen käytöstä toiminnan muuttaminen. Jos täytät ilmenee ongelmia, Etsi ratkaisuja [sivun vianmääritys](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="422-what-you-will-learn"></a>4.2.2 mitä opit

Lisätoiminnot Node.js avulla voit muuttaa LED henkilön käyttöön ja poistaminen käytöstä toiminta.

## <a name="423-what-you-need"></a>4.2.3 mitä tarvitset

Sinun täytyy onnistui [4.1 suorittaa vadelma piillä cloud laitteen viestien vastaanottamiseen sovelluksen malli](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md).

## <a name="424-add-nodejs-functions"></a>4.2.4 Lisää Node.js-Funktiot

1. Avaa Visual Studio koodi sovelluksen malli suorittamalla seuraavat komennot:

    ```bash
    cd Lesson4
    code .
    ```

2. Avaa `app.js` tiedosto ja lisää sitten lopussa seuraavat toiminnot:

    ```javascript
    function turnOnLED() {
      wpi.digitalWrite(CONFIG_PIN, 1);
    }

    function turnOffLED() {
      wpi.digitalWrite(CONFIG_PIN, 0);
    }
    ```

    ![Päivitä app.js](media/iot-hub-raspberry-pi-lessons/lesson4/updated_app_js.png)

3. Lisää seuraavat ehdot ennen oletusarvo jokin Vaihda kirjainkoko estoasetukset `receiveMessageCallback` funktiota:

    ```javascript
    case 'on':
      turnOnLED();
      break;
    case 'off':
      turnOffLED();
      break;
    ```

    Olet nyt määrittänyt sovelluksen malli vastata Lisää ohjeiden avulla. "," Käsky otetaan käyttöön LED ja LED sulkee "käytöstä"-ohjepaketti.

4. Avaa gulpfile.js-tiedosto ja lisää sitten uusi funktio ennen funktion `sendMessage`:

    ```javascript
    var buildCustomMessage = function (messageId) {
      if ((messageId & 1) && (messageId < MAX_MESSAGE_COUNT)) {
        return new Message(JSON.stringify({ command: 'on', messageId: messageId }));
      } else if (messageId < MAX_MESSAGE_COUNT) {
        return new Message(JSON.stringify({ command: 'off', messageId: messageId }));
      } else {
        return new Message(JSON.stringify({ command: 'stop', messageId: messageId }));
      }
    }
    ```

    ![Päivitä gulpfile](media/iot-hub-raspberry-pi-lessons/lesson4/updated_gulpfile.png)

5. - `sendMessage` Funktio, korvaa rivi `var message = buildMessage(sentMessageCount);` näkyvät seuraavat koodikatkelman uuden rivin kanssa:

    ```javascript
    var message = buildCustomMessage(sentMessageCount);
    ```

6. Tallenna muutokset.

### <a name="425-deploy-and-run-the-sample-application"></a>4.2.5 käyttöön ja suorita sovelluksen malli

Ottaa käyttöön ja käyttää sovelluksen malli-että pii suorittamalla seuraavan komennon:

```bash
gulp
```

Raportissa pitäisi näkyä LED kahden sekunnin ajan käynnissä ja sitten poistettu käytöstä toisen kahden sekunnin ajan. Viimeisen "Lopeta" viestin lopettaa käynnistymisen näytteen-sovelluksen.

![käyttöön ja poistaminen käytöstä](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_on_and_off.png)

Onnittelen! Olet onnistuneesti mukautettu pii IoT-toiminnosta lähetetyt viestit.

### <a name="427-summary"></a>4.2.7 yhteenveto

Tämän valinnaisen osan demos mukauttamisesta viestejä niin, että sovelluksen malli voit hallita käyttöön tai pois LED toimintaa eri tavalla.

