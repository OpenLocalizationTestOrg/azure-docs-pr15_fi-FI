<properties
    pageTitle="Azure IoT-toiminnossa aloittaminen Node.js | Microsoft Azure"
    description="Azure IoT keskittimeen kanssa Node.js käytön aloittaminen opetusohjelma. Toteuta asioita Internet-ratkaisun avulla Azure IoT keskittimeen ja Node.js Microsoft Azure IoT SDK: T."
    services="iot-hub"
    documentationCenter="nodejs"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="javascript"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/12/2016"
     ms.author="dobett"/>

# <a name="get-started-with-azure-iot-hub-for-nodejs"></a>Aloittaminen Azure IoT toiminnossa Node.js

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Tässä opetusohjelmassa lopussa on kolme Node.js console-sovellusta:

* **CreateDeviceIdentity.js**, joka luo laitteen käyttäjätieto- ja liittyvät suojaustunnuksen yhteyden muodostamiseksi Simuloitu laitteen.
* **ReadDeviceToCloudMessages.js**, joka näyttää Simuloitu laitteen lähettämä telemetriatietojen.
* **SimulatedDevice.js**, joka muodostaa yhteyden IoT keskittimeen aiemmin luotu laitteen käyttäjätietoja ja lähettää telemetriatietojen viestin, joka toinen käyttämällä AMQP-protokollaa.

> [AZURE.NOTE] Artikkelin [IoT keskittimeen SDK: T] [ lnk-hub-sdks] tietoja siitä, eri SDK: T, joiden avulla voit luoda molemmat sovellukset toimimaan laitteet ja ratkaisu takaisin päässä.

Tässä opetusohjelmassa suorittamiseen tarvitset seuraavat:

+ Node.js versio 0.10.x tai uudempi versio.

+ Azure active tili. (Jos sinulla ei ole tiliä, voit luoda [ilmaisen tilin] [ lnk-free-trial] -muutamaan minuuttiin.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Olet nyt luonut IoT-toiminnossa. Sinulla on IoT keskittimeen isäntänimi ja IoT keskittimeen yhteysmerkkijonon, sinun on suoritettava jäljempänä tässä opetusohjelmassa.

## <a name="create-a-device-identity"></a>Laitteen käyttäjätietojen luominen

Tässä osassa voit luoda Node.js console-sovellus, joka luo laitteen tunnistetietojen IoT-toiminnossa identity-rekisterissä. Laite ei voi muodostaa yhteyttä IoT keskittimeen, paitsi jos siinä on merkintä laitteen tunnistetietojen rekisterissä. **Laitteen tunnistetietojen rekisterin** -osassa [IoT keskittimeen Sovelluskehittäjän opas] [ lnk-devguide-identity] lisätietoja. Kun suoritat tämä konsolisovellus, se luo laitteen yksilöivä tunnus ja -laitteen avulla voit tunnistaa itse, kun se lähettää laitteen cloud viestien IoT keskittimeen avaimen.

1. Luo uusi tyhjä kansio nimeltä **createdeviceidentity**. Luoda package.json tiedoston käyttämällä komentokehotteeseen seuraava komento **createdeviceidentity** -kansioon. Hyväksy kaikki oletusarvot:

    ```
    npm init
    ```

2. Oman-komentorivin **createdeviceidentity** -kansiossa Suorita seuraava komento asentaa **azure iothub** palvelun SDK-paketti:

    ```
    npm install azure-iothub --save
    ```

3. **CreateDeviceIdentity.js** tiedostoa tekstieditorissa, luo **createdeviceidentity** -kansioon.

4. Lisää seuraava `require` lauseen **CreateDeviceIdentity.js** tiedoston alkuun:

    ```
    'use strict';
    
    var iothub = require('azure-iothub');
    ```

5. Lisää seuraava koodi **CreateDeviceIdentity.js** -tiedostoon ja korvaa edellisessä osassa luomasi IoT-toiminnossa yhteysmerkkijono paikkamerkinarvo: 

    ```
    var connectionString = '{iothub connection string}';
    
    var registry = iothub.Registry.fromConnectionString(connectionString);
    ```

6. Lisää seuraava koodi luo laitteen määrityksen laitteen tunnistetietojen rekisterin IoT-toiminnossa. Koodi luo laitteen, jos laitteen tunnus ei ole rekisterin, se palauttaa aiemmin laitteen näppäintä:

    ```
    var device = new iothub.Device(null);
    device.deviceId = 'myFirstNodeDevice';
    registry.create(device, function(err, deviceInfo, res) {
      if (err) {
        registry.get(device.deviceId, printDeviceInfo);
      }
      if (deviceInfo) {
        printDeviceInfo(err, deviceInfo, res)
      }
    });

    function printDeviceInfo(err, deviceInfo, res) {
      if (deviceInfo) {
        console.log('Device id: ' + deviceInfo.deviceId);
        console.log('Device key: ' + deviceInfo.authentication.SymmetricKey.primaryKey);
      }
    }
    ```

7. Tallenna ja sulje **CreateDeviceIdentity.js** tiedosto.

8. **Createdeviceidentity** -sovelluksen käyttämiseen, suorita seuraava komento komentokehotteeseen-createdeviceidentity-kansiossa:

    ```
    node CreateDeviceIdentity.js 
    ```

9. Pane merkille **laitteen tunnus** ja **laitteen avainta**. Tarvitset nämä arvot myöhemmin sovellus, joka muodostaa yhteyden IoT keskittimeen laitteen luodessasi.

> [AZURE.NOTE] IoT keskittimeen tunnistetietojen rekisterin tallentaa vain laitteen käyttäjätietojen käyttöön valitsemalla suojattu käyttö. Tallennetaan laitteen tunnukset ja näppäimet, jotta voit käyttää suojausvaltuudet ja käytössä/poissa käytöstä-merkinnän, voit poistaa käytöstä yksittäisiä laitteen. Jos sovellus täytyy tallentaa laitekohtaiset muita metatietoja, se kannattaa käyttää sovelluksen kielikohtaiset-kaupan. [IoT keskittimeen Developer] oppaassa[ lnk-devguide-identity] lisätietoja.

## <a name="receive-device-to-cloud-messages"></a>Laitteen cloud virhesanomia

Tässä osassa voit luoda Node.js console-sovellus, jossa lukee laitteen cloud viestien IoT-toiminnosta. IoT-toiminnossa paljastaa [Tapahtuman keskittimet][lnk-event-hubs-overview]-yhteensopiva päätepisteen, jotta voit laitteen cloud viestien lukeminen. Pidä asiat yksinkertainen Tässä opetusohjelmassa Luo basic lukuohjelmaa, joka ei sovi hyvin siirtonopeuden käyttöönottoa varten. [Prosessin laitteen cloud viestien] [ lnk-process-d2c-tutorial] opetusohjelmassa näytetään, miten voit käsitellä laitteen cloud viestit tasolla. [Tapahtuman keskittimet aloittaminen] [ lnk-eventhubs-tutorial] opetusohjelma tietoja edelleen prosessin viesteihin keskittimet tapahtuma- ja soveltuu IoT keskittimeen tapahtuman keskittimeen yhteensopivan päätepisteet.

> [AZURE.NOTE] Tapahtuman keskittimeen yhteensopivan päätepisteen laitteen cloud viestien lukeminen aina käyttää AMQP-protokollaa.

1. Luo uusi tyhjä kansio nimeltä **readdevicetocloudmessages**. Luoda package.json tiedoston käyttämällä komentokehotteeseen seuraava komento **readdevicetocloudmessages** -kansioon. Hyväksy kaikki oletusarvot:

    ```
    npm init
    ```

2. Komentoriville oman-varten suorittamalla seuraavan komennon Asenna **azure-tapahtuma-keskittimet** paketti **readdevicetocloudmessages** -kansiossa:

    ```
    npm install azure-event-hubs --save
    ```

3. **ReadDeviceToCloudMessages.js** tiedostoa tekstieditorissa, luo **readdevicetocloudmessages** -kansioon.

4. Lisää seuraava `require` lauseet **ReadDeviceToCloudMessages.js** tiedoston alkuun:

    ```
    'use strict';

    var EventHubClient = require('azure-event-hubs').Client;
    ```

5. Lisää seuraavat muuttujan määrittely ja korvaa IoT-toiminnossa yhteysmerkkijono paikkamerkinarvo:

    ```
    var connectionString = '{iothub connection string}';
    ```

6. Lisää seuraavat kaksi Funktiot, jotka konsoliin tulostaminen:

    ```
    var printError = function (err) {
      console.log(err.message);
    };

    var printMessage = function (message) {
      console.log('Message received: ');
      console.log(JSON.stringify(message.body));
      console.log('');
    };
    ```

7. Lisää seuraava koodi luo **EventHubClient**, Avaa IoT-toiminnossa yhteys ja luoda kunkin osion vastaanotin. Tämän sovelluksen käyttää suodattimia, kun se luo vastaanotin niin, että vastaanottaja lukee vain vastaanotin käynnistyttyä käynnissä IoT keskittimeen lähetetyt viestit. Tämä suodatin on hyötyä testiympäristössä niin, että näet vain parhaillaan seuraamasi viestejä. Tuotantoympäristössä-koodin Varmista että käsittelee kaikki viestit - kohdassa [IoT keskittimeen laitteen cloud viestien käsitteleminen] [ lnk-process-d2c-tutorial] opetusohjelman Lisätietoja:

    ```
    var client = EventHubClient.fromConnectionString(connectionString);
    client.open()
        .then(client.getPartitionIds.bind(client))
        .then(function (partitionIds) {
            return partitionIds.map(function (partitionId) {
                return client.createReceiver('$Default', partitionId, { 'startAfterTime' : Date.now()}).then(function(receiver) {
                    console.log('Created partition receiver: ' + partitionId)
                    receiver.on('errorReceived', printError);
                    receiver.on('message', printMessage);
                });
            });
        })
        .catch(printError);
    ```

8. Tallenna ja sulje **ReadDeviceToCloudMessages.js** -tiedosto.

## <a name="create-a-simulated-device-app"></a>Simuloitu laitteen sovelluksen luominen

Tässä osassa Node.js konsolin sovelluksen laitteella, joka lähettää laitteen cloud IoT keskittimeen tulokset vastaavat luominen.

1. Luo uusi tyhjä kansio nimeltä **simulateddevice**. Luoda package.json tiedoston käyttämällä komentokehotteeseen seuraava komento **simulateddevice** -kansioon. Hyväksy kaikki oletusarvot:

    ```
    npm init
    ```

2. Komentoriville oman- **simulateddevice** -kansiossa Suorita seuraava komento **azure-iot-laitteen** laitteen SDK-paketti ja **azure-iot-laite-amqp** paketin asentaminen:

    ```
    npm install azure-iot-device azure-iot-device-amqp --save
    ```

3. Uusi **SimulatedDevice.js** -tiedostoa tekstieditorissa, luo **simulateddevice** -kansioon.

4. Lisää seuraava `require` lauseet **SimulatedDevice.js** tiedoston alkuun:

    ```
    'use strict';

    var clientFromConnectionString = require('azure-iot-device-amqp').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    ```

5. Lisää **connectionString** muuttujan ja luoda laitteen asiakas. Korvaa **{youriothostname}** IoT-toiminnossa nimellä luomasi luominen *IoT-toiminnossa* . Korvaa **{yourdevicekey}** laitteen avainarvon luotu *Luo laitteen käyttäjätiedot* -kohdassa:

    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myFirstNodeDevice;SharedAccessKey={yourdevicekey}';
    
    var client = clientFromConnectionString(connectionString);
    ```

6. Lisää seuraavan funktion näyttämään sovelluksen:

    ```
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```

7. Luo takaisinkutsun ja uuden viestin lähettäminen IoT-toiminnossa sekunnin välein **setInterval** -funktion avulla:

    ```
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');

        // Create a message and send it to the IoT Hub every second
        setInterval(function(){
            var windSpeed = 10 + (Math.random() * 4);
            var data = JSON.stringify({ deviceId: 'myFirstNodeDevice', windSpeed: windSpeed });
            var message = new Message(data);
            console.log("Sending message: " + message.getData());
            client.sendEvent(message, printResultFor('send'));
        }, 1000);
      }
    };
    ```

8. Avaa yhteyden IoT-toiminnosta ja viestien lähettäminen:

    ```
    client.open(connectCallback);
    ```

9. Tallenna ja sulje **SimulatedDevice.js** -tiedosto.

> [AZURE.NOTE] Tässä opetusohjelmassa avulla voit jatkaa samaa yksinkertainen, Toteuta uudelleen kaikki käytännöt. Tuotannon koodissa olisi Toteuta kuin ehdotettua [Lyhytkestoisia vika käsittely]MSDN-sivuston artikkelissa (kuten Eksponentiaalinen backoff), yritä käytännöt[lnk-transient-faults].


## <a name="run-the-applications"></a>Suorita sovellukset

Olet nyt valmiina sovelluksia.

1. Komentokehotteeseen- **readdevicetocloudmessages** -kansioon, suorita seuraava komento aloittaa seuranta IoT-toiminnossa:

    ```
    node ReadDeviceToCloudMessages.js 
    ```

    ![Node.js IoT keskittimeen palvelun asiakassovellus seurannassa laitteen cloud viestit][7]

2. Komentokehotteeseen- **simulateddevice** -kansioon, suorita seuraava komento aloittaa telemetriatietoja lähettäminen IoT-toiminnossa:

    ```
    node SimulatedDevice.js
    ```

    ![Node.js IoT keskittimeen laitteen asiakassovellus laitteen cloud viestien lähettäminen][8]

3. **Käyttö** -ruudun [Azure portal] [ lnk-portal] näkyy keskittimeen viestien lukumäärä:

    ![Azure portaalin käyttö ruutu näkyy viestien lukumäärä IoT keskittimeen][43]

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä opetusohjelmassa määritetty keskittimeen IoT Azure-portaalissa, ja luo laitteen identity toiminnossa tunnistetietojen rekisterissä. Tämä laite tunnistetietojen avulla laitteen cloud viestien lähettämiseen keskittimeen Simuloitu laite-sovelluksen käyttöön. Voit luoda myös sovellus, joka näyttää keskittimeen vastaanotettujen viestien. 

Jatka IoT keskittimeen aloittaminen ja tutustu IoT muissa tilanteissa, katso:

- [Laitteen yhteyden muodostamisessa][lnk-connect-device]
- [Mobiililaitteiden hallinnan käytön aloittaminen][lnk-device-management]
- [IoT yhdyskäytävän SDK: N käytön aloittaminen][lnk-gateway-SDK]

Voit laajentaa IoT ratkaisu ja käsittele laitteen cloud viestejä tasolla on artikkelissa [käsitellä laitteen cloud viestejä] [ lnk-process-d2c-tutorial] opetusohjelma.

<!-- Images. -->
[7]: ./media/iot-hub-node-node-getstarted/runapp1.png
[8]: ./media/iot-hub-node-node-getstarted/runapp2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
