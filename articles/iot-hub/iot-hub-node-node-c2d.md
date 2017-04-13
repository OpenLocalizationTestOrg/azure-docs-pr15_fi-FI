<properties
    pageTitle="Cloud laitteen IoT keskittimeen sisältävien viestien lähettäminen | Microsoft Azure"
    description="Katso tämä opetusohjelma lisätietoja Azure IoT keskittimeen käyttäminen Java cloud laitteen viestien lähettämiseen."
    services="iot-hub"
    documentationCenter="nodejs"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="javascript"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/23/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-send-cloud-to-device-messages-with-iot-hub-and-nodejs"></a>Opetusohjelma: Miten cloud laitteen IoT keskittimeen ja Node.js sisältävien viestien lähettäminen

[AZURE.INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Johdanto

Azure IoT-toiminnossa on täysin hallittujen palvelu, joka auttaa käyttöön luotettavan ja turvallisen kaksisuuntaisen tietoliikenteen miljoonia IoT laitteiden välillä ja sovelluksen takaisin lopettaa. [IoT keskittimeen käytön aloittaminen] -opetusohjelma näytetään, miten voit luoda IoT-toiminnosta ja valmistella laitteen tunnistetiedot, ei koodin Simuloitu laitteella, joka lähettää laitteen pilveen.

Tässä opetusohjelmassa muodostaa [IoT toiminnosta on]aloittaminen. Se näyttää, miten voit:

- Lähettää cloud laitteen viestien oman sovelluksen cloud taustatietokantaan, yhteen laitteeseen IoT keskittimeen kautta.
- Cloud laitteen virhesanomia laitteessa.
- Sovelluksen cloud takaisin loppuun, pyytää kuittausta toimituksen (*palaute*) lähetetyt laitteeseen IoT-toiminnosta.

Voit etsiä lisätietoja cloud laitteen viesteille [IoT keskittimeen Sovelluskehittäjän opas][IoT Hub Developer Guide - C2D].

Tässä opetusohjelmassa lopussa suorittaa kaksi Node.js console-sovellusta:

* **SimulatedDevice**, luotu [aloittaminen IoT keskittimeen], joka muodostaa yhteyden IoT-toiminnosta ja vastaanottaa cloud laite muokattu sovellusversio.
* **SendCloudToDeviceMessage**, joka lähettää cloud laitteen viestin IoT keskittimeen Simuloitu laitteesi ja vastaanottaa sen toimitus-kuittausta.

> [AZURE.NOTE] IoT toiminnosta on monta laitteen alustojen ja eri kielten (mukaan lukien C ja Javascript Java) – Azure IoT laitteen SDK: T SDK tuki. Vaiheittaiset ohjeet laitteen yhdistäminen Tässä opetusohjelmassa koodi ja yleensä Azure IoT-toiminnossa on artikkelissa [Azure IoT Developer Center].

Tässä opetusohjelmassa suorittamiseen tarvitset seuraavat:

+ Node.js versio 0.10.x tai uudempi versio.

+ Azure active tili. (Jos sinulla ei ole tiliä, voit luoda [ilmaisen tilin] [ lnk-free-trial] -muutamaan minuuttiin.)

## <a name="receive-messages-on-the-simulated-device"></a>Vastaanottaa viestejä Simuloitu laitteeseen

Tässä osassa voit muokata Simuloitu laite-sovellus on luotu cloud laitteen virhesanomia IoT-toiminnosta [IoT keskittimeen käytön aloittaminen] .

1. SimulatedDevice.js tiedostoa tekstieditorissa, Avaa.

2. Muokkaa **connectCallback** -funktio käsittelee IoT-toiminnosta lähetetyt viestit. Tässä esimerkissä laitteen käynnistää aina IoT toiminto ilmoittaa, että se on käsitellyt viestin **Valmis** -funktiota. Uuden version **connectCallback** -funktio on seuraavanlainen:

    ```
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');
        client.on('message', function (msg) {
          console.log('Id: ' + msg.messageId + ' Body: ' + msg.data);
          client.complete(msg, printResultFor('completed'));
        });
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

    > [AZURE.NOTE] Jos käytät HTTP MQTT tai AMQP sijaan kuljetus, **DeviceClient** esiintymän tarkistaa viestien harvoin IoT toiminnosta (enintään 25 minuutin välein). Lisätietoja MQTT, AMQP ja HTTP-tuki ja IoT keskittimeen rajoittimen erot on [IoT keskittimeen Sovelluskehittäjän opas][IoT Hub Developer Guide - C2D].

## <a name="send-a-cloud-to-device-message"></a>Cloud laitteen viestin lähettäminen

Tässä osassa voit luoda Node.js console-sovellus, joka lähettää cloud laitteen Simuloitu laite-sovellukseen. Tarvitset laite Valitse [IoT keskittimeen käytön aloittaminen] -opetusohjelmassa lisäämääsi laitteen tunnus. Lisäksi tarvitset yhteysmerkkijonon oman IoT-toiminnossa voit etsiä [Azure portal].

1. Luo tyhjä kansio nimeltä **sendcloudtodevicemessage**. Luoda package.json tiedoston käyttämällä komentokehotteeseen seuraava komento **sendcloudtodevicemessage** -kansioon. Hyväksy kaikki oletusarvot:

    ```
    npm init
    ```

2. Komentoriville oman-varten suorittamalla seuraavan komennon Asenna **azure iothub** paketti **sendcloudtodevicemessage** -kansiossa:

    ```
    npm install azure-iothub --save
    ```

3. Uusi **SendCloudToDeviceMessage.js** -tiedostoa tekstieditorissa, luo **sendcloudtodevicemessage** -kansioon.

4. Lisää seuraava `require` lauseet **SendCloudToDeviceMessage.js** tiedoston alkuun:

    ```
    'use strict';
    
    var Client = require('azure-iothub').Client;
    var Message = require('azure-iot-common').Message;
    ```

5. Lisää seuraava koodi **SendCloudToDeviceMessage.js** tiedostoon. Yhteyden merkkijonoarvo paikkamerkin tilalle [IoT keskittimeen käytön aloittaminen] -opetusohjelma luomasi IoT-toiminnossa yhteysmerkkijono. Korvaa kohteen laitteen paikkamerkki laitteen tunnus laitteen lisäämäsi [IoT keskittimeen käytön aloittaminen] -opetusohjelma:

    ```
    var connectionString = '{iot hub connection string}';
    var targetDevice = '{device id}';

    var serviceClient = Client.fromConnectionString(connectionString);
    ```

6. Lisää seuraavan funktion tulostaa konsoliin toiminnon tulokset:

    ```
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```

7. Lisää seuraavan funktion tulostaa toimituksen palautetta viestien konsoliin:

    ```
    function receiveFeedback(err, receiver){
      receiver.on('message', function (msg) {
        console.log('Feedback message:')
        console.log(msg.getData().toString('utf-8'));
      });
    }
    ```

8. Lisää viestin lähettäminen laitteeseen ja käsitellä palautetta viestin, kun laite hyväksyy sen cloud laitteen viestin seuraava koodi:

    ```
    serviceClient.open(function (err) {
      if (err) {
        console.error('Could not connect: ' + err.message);
      } else {
        console.log('Service client connected');
        serviceClient.getFeedbackReceiver(receiveFeedback);
        var message = new Message('Cloud to device message.');
        message.ack = 'full';
        message.messageId = "My Message ID";
        console.log('Sending message: ' + message.getData());
        serviceClient.send(targetDevice, message, printResultFor('send'));
      }
    });
    ```

7. Tallenna ja sulje **SendCloudToDeviceMessage.js** tiedosto.

## <a name="run-the-applications"></a>Suorita sovellukset

Olet nyt valmiina sovelluksia.

1. Komentokehotteen- **simulateddevice** -kansiossa Suorita seuraava komento telemetriatietojen lähettäminen IoT keskittimeen ja cloud laitteen viestien kuunteleminen:

    ```
    node SimulatedDevice.js 
    ```

    ![Suorita Simuloitu laite-sovellus][img-simulated-device]

2. Komentokehotteeseen- **sendcloudtodevicemessage** -kansiossa Suorita cloud laitteen sähköpostin ja odota, kunnes acknowledgment palautetta seuraava komento:

    ```
    node SendCloudToDeviceMessage.js 
    ```

    ![Suorita sovellus lähettää c2d-komento][img-send-command]

    > [AZURE.NOTE] Tässä opetusohjelmassa Toteuta uudelleen kaikki käytännöt yksinkertaisuuden 's sake. Tuotannon koodi olisi Toteuta kuin ehdotettua [Lyhytkestoisia vika käsittely]MSDN-sivuston artikkelissa (kuten eksponentiaalisen backoff), yritä käytännöt.

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä opetusohjelmassa opit, miten cloud laitteen viestien lähettämiseen ja vastaanottamiseen. 

Esimerkkejä valmis lopusta loppuun-ratkaisuja, jotka käyttävät IoT keskittimeen, on artikkelissa [Azure IoT Suite].

Saat lisätietoja kanssa IoT keskittimeen, ratkaisujen kehittämiseen on [IoT keskittimeen Sovelluskehittäjän opas].

<!-- Images -->
[img-simulated-device]: media/iot-hub-node-node-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-node-node-c2d/sendc2d.png

<!-- Links -->

[IoT keskittimeen käytön aloittaminen]: iot-hub-node-node-getstarted.md
[IoT Hub Developer Guide - C2D]: iot-hub-devguide-messaging.md
[IoT keskittimeen Sovelluskehittäjän opas]: iot-hub-devguide.md
[Azure IoT Developer Center]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[Lyhytkestoisia virheen käsittely]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure portal]: https://portal.azure.com
[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/