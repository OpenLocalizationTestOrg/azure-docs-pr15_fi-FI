<properties
 pageTitle="Suora menetelmillä | Microsoft Azure"
 description="Tässä opetusohjelmassa näytetään, miten voit käyttää suoraan menetelmiä"
 services="iot-hub"
 documentationCenter=""
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/05/2016"
 ms.author="nberdy"/>

# <a name="tutorial-use-direct-methods"></a>Opetusohjelma: Suora menetelmillä

## <a name="introduction"></a>Johdanto

Azure IoT-toiminnossa on täysin hallitun palvelu, joka mahdollistaa luotettava ja suojatun kaksisuuntaisen tietoliikenteen miljoonia IoT laitteet ja sovelluksen takaisin end-näppäinyhdistelmää. Edellisen opetusohjelmat ([IoT keskittimeen käytön aloittaminen] ja [Cloud laitteen IoT keskittimeen sisältävien viestien lähettäminen]) osoittavat laitteen cloud ja cloud laitteen tekstiviesti perustoimintoja IoT toiminnossa. IoT keskittimeen myös ansiosta voit käynnistää kulutustavarat menetelmiä pilvestä laitteissa. Menetelmien edustavat samalla tavalla kuin HTTP-kutsun laitteen pyynnön vastaus käsittelemisen siinä, että ne onnistuu tai epäonnistuu heti (jälkeen käyttäjän määrittämä aikakatkaisu) antaa käyttäjän puhelun tilasta. [Suora menetelmän laitteessa] [ lnk-devguide-methods] kuvataan tapoja yksityiskohtaisemmin ja tarjoaa ohjeita siitä, milloin ja cloud laitteen viestit eri tavalla.

Tässä opetusohjelmassa näytetään, miten voit:

- Azure portaalin avulla voit luoda IoT-toiminnosta ja laitteen käyttäjätietojen luominen IoT-toiminnossa.
- Luo Simuloitu laitteissa, joissa on suora menetelmä, jota voit kutsua pilveen.
- Luo console-sovellus, jossa suora menetelmä kutsuu Simuloitu laitteeseen IoT-toiminnossa kautta.

> [AZURE.NOTE] Tällä hetkellä voi käyttää vain IoT keskittimeen MQTT-protokollan avulla yhteyden muodostavien laitteiden suoraan tavoista. Lue lisätietoja [MQTT tukevat] [ lnk-devguide-mqtt] artikkelista ohjeita siitä, miten voit muuntaa olemassa olevan laitteen-sovelluksen käyttämään MQTT.

Tässä opetusohjelmassa lopussa on kaksi Node.js console-sovellusta:

* **CallMethodOnDevice.js**, johon soittaa menetelmän Simuloitu laitteessa ja näyttää vastauksen.
* **SimulatedDevice.js**, joka muodostaa yhteyden IoT keskittimeen aiemmin luotu laitteen käyttäjätietoja ja vastaa käytettävän menetelmän pilveen.

> [AZURE.NOTE] Artikkelin [IoT keskittimeen SDK: T] [ lnk-hub-sdks] tietoja siitä, eri SDK: T, joiden avulla voit luoda molemmat sovellukset toimimaan laitteet ja ratkaisu takaisin päässä.

Tässä opetusohjelmassa suorittamiseen tarvitset seuraavat:

+ Node.js versio 0.10.x tai uudempi versio.

+ Azure active tili. (Jos sinulla ei ole tiliä, voit luoda [ilmaisen tilin] [ lnk-free-trial] -muutamaan minuuttiin.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Simuloitu laitteen sovelluksen luominen

Tässä osassa voit luoda Node.js console-sovellus, joka vastaa menetelmän mukaan pilveen.

1. Luo uusi tyhjä kansio nimeltä **simulateddevice**. Luoda package.json tiedoston käyttämällä komentokehotteeseen seuraava komento **simulateddevice** -kansioon. Hyväksy kaikki oletusarvot:

    ```
    npm init
    ```

2. Komentoriville oman- **simulateddevice** -kansiossa Suorita seuraava komento **azure-iot-laitteen** laitteen SDK-paketti ja **azure-iot-laite-mqtt** paketin asentaminen:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Uusi **SimulatedDevice.js** -tiedostoa tekstieditorissa, luo **simulateddevice** -kansioon.

4. Lisää seuraava `require` lauseet **SimulatedDevice.js** tiedoston alkuun:

    ```
    'use strict';

    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```

5. Lisää **connectionString** muuttujan ja luoda laitteen asiakas. Korvaa **{laitteen yhteysmerkkijonon}** yhteysmerkkijono- *laitteen käyttäjätietojen* luominen luotu:

    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```

6. Lisää seuraava funktio toteuttaa menetelmä laitteeseen:

    ```
    function onWriteLine(request, response) {
        console.log(request.payload);

        response.send(200, 'Input was written to log.', function(err) {
            if(err) {
                console.error('An error ocurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```

7. Avaa yhteyden IoT-toiminnosta ja aloita alusta menetelmä kuuntelua:

    ```
    client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
            client.onDeviceMethod('writeLine', onWriteLine);
        }
    });
    ```

8. Tallenna ja sulje **SimulatedDevice.js** -tiedosto.

> [AZURE.NOTE] Tässä opetusohjelmassa avulla voit jatkaa samaa yksinkertainen, Toteuta uudelleen kaikki käytännöt. Tuotannon koodissa olisi Toteuta uudelleen käytännöt (kuten yhteyden uudelleen), kuin ehdotettua [Lyhytkestoisia vika käsittely]MSDN-artikkelista[lnk-transient-faults].

## <a name="call-a-method-on-a-device"></a>Soita menetelmän laitteessa

Tässä osassa Node.js console-sovelluksen, soittaa menetelmän Simuloitu laitteessa ja näyttää vastauksen luominen.

1. Luo uusi tyhjä kansio nimeltä **callmethodondevice**. Luoda package.json tiedoston käyttämällä komentokehotteeseen seuraava komento **callmethodondevice** -kansioon. Hyväksy kaikki oletusarvot:

    ```
    npm init
    ```

2. Komentoriville oman-varten suorittamalla seuraavan komennon Asenna **azure iothub** paketti **callmethodondevice** -kansiossa:

    ```
    npm install azure-iothub@dtpreview --save
    ```

3. **CallMethodOnDevice.js** tiedostoa tekstieditorissa, luo **callmethodondevice** -kansioon.

4. Lisää seuraava `require` lauseet **CallMethodOnDevice.js** tiedoston alkuun:

    ```
    'use strict';

    var Client = require('azure-iothub').Client;
    ```

5. Lisää seuraavat muuttujan määrittely ja korvaa IoT-toiminnossa yhteysmerkkijono paikkamerkinarvo:

    ```
    var connectionString = '{iothub connection string}';
    var methodName = 'writeLine';
    var deviceId = 'myDeviceId';
    ```

6. Luo asiakkaan Avaa IoT-toiminnossa yhteys.

    ```
    var client = Client.fromConnectionString(connectionString);
    ```
    
7. Lisää seuraavan funktion laitteen menetelmän ja tulostaa konsoliin laitteen vastaus:

    ```
    var methodParams = {
        methodName: methodName,
        payload: 'a line to be written',
        timeoutInSeconds: 30
    };

    client.invokeDeviceMethod(deviceId, methodParams, function (err, result) {
        if (err) {
            console.error('Failed to invoke method \'' + methodName + '\': ' + err.message);
        } else {
            console.log(methodName + ' on ' + deviceId + ':');
            console.log(JSON.stringify(result, null, 2));
        }
    });
    ```

7. Tallenna ja sulje **CallMethodOnDevice.js** -tiedosto.

## <a name="run-the-applications"></a>Suorita sovellukset

Olet nyt valmiina sovelluksia.

1. Komentokehotteeseen- **simulateddevice** -kansiossa Suorita seuraava komento Aloita Kuuntele menetelmäkutsujen IoT-toiminnosta:

    ```
    node SimulatedDevice.js
    ```

    ![][7]
    
2. Komentokehotteeseen- **callmethodondevice** -kansioon, suorita seuraava komento aloittaa seuranta IoT-toiminnossa:

    ```
    node CallMethodOnDevice.js 
    ```

    ![][8]
    
3. Näet laitteen reagoi menetelmän tulostamalla viestin ja sovellus, jota kutsutaan tapa näyttää vastauksen laitteesta:

    ![][9]
    
## <a name="next-steps"></a>Seuraavat vaiheet

Tässä opetusohjelmassa määritetty keskittimeen IoT Azure-portaalissa, ja luo laitteen identity toiminnossa tunnistetietojen rekisterissä. Tämä laite jäsenyys avulla voit vastata menetelmiä käynnistää pilveen Simuloitu laite-sovelluksen käyttöön. Voit luoda myös sovellus, joka käynnistää laitteelta menetelmistä ja näyttää vastaus laitteelta. 

Jatka IoT keskittimeen aloittaminen ja tutustu IoT muissa tilanteissa, katso:

- [IoT keskittimeen käytön aloittaminen]
- [Ajoita työt useilla eri laitteilla][lnk-devguide-jobs]

Lisätietoja laajentaa oman IoT ratkaisu ja joissa menetelmä kutsuu useilla eri laitteilla, näet [aikataulun ja lähetyksen työt] [ lnk-tutorial-jobs] opetusohjelma.

<!-- Images. -->
[7]: ./media/iot-hub-c2d-methods/run-simulated-device.png
[8]: ./media/iot-hub-c2d-methods/run-callmethodondevice.png
[9]: ./media/iot-hub-c2d-methods/methods-output.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md
[lnk-devguide-methods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[Cloud laitteen IoT keskittimeen sisältävien viestien lähettäminen]: iot-hub-csharp-csharp-c2d.md
[Process Device-to-Cloud messages]: iot-hub-csharp-csharp-process-d2c.md
[IoT keskittimeen käytön aloittaminen]: iot-hub-node-node-getstarted.md
