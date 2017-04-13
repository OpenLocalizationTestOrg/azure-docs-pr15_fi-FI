<properties
 pageTitle="Mobiililaitteiden hallinnan käytön aloittaminen | Microsoft Azure"
 description="Tässä opetusohjelmassa kerrotaan, miten hallinta-Azure IoT keskittimeen käytön aloittaminen"
 services="iot-hub"
 documentationCenter=".net"
 authors="juanjperez"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="juanpere"/>

# <a name="tutorial-get-started-with-device-management-preview"></a>Opetusohjelma: Käytön aloittaminen hallinta (ennakkoversio)

## <a name="introduction"></a>Johdanto
IoT cloud sovellukset voivat käyttää primitiivien Azure IoT-toiminnossa eli laitteen sekä ja suorat menetelmiä, voit käynnistää etäyhteyden ja seurata laitteen hallinnan toiminnot laitteissa.  Tässä artikkelissa on ohjeita ja koodi, miten IoT cloud-sovelluksen ja laitteen toimivat yhdessä voidaan aloittaa ja seurata käyttämällä IoT keskittimeen etälaitteen uudelleen.

Käynnistä ja seurata laitteen hallinnan toiminnot laitteisiisi pilvipohjainen, taustatietokantaan sovelluksesta etäyhteyden käyttämällä Azure IoT keskittimeen perusalkioiden, kuten [laitteen sekä] [ lnk-devtwin] ja [cloud laitteen (C2D) menetelmiä][lnk-c2dmethod]. Tässä opetusohjelmassa näytetään, miten taustatietokantaan-sovellus ja laitteen voit toimivat yhdessä sen sallimiseksi, että voit aloittaa ja seurata etälaitteen uudelleenkäynnistyksen IoT-toiminnosta.

C2D menetelmän avulla voit aloittaa laitteen hallintaan liittyvät toiminnot (kuten uudelleenkäynnistyksen, factory Palauta ja laiteohjelmiston päivityksen) pilveen taustatietokantaan-sovelluksesta. Laite on vastuussa:

- Lähetetty IoT-toiminnosta menetelmä pyyntö käsitellään.
- Aloitetaan vastaavan laitteen tietyn toiminnon laitteeseen.
- Ominaisuudet tarjoavat tilapäivitykset kautta laitteen sekä raportoitu IoT keskittimeen.

Voit taustatietokantaan app pilveen laitteen sekä kyselyt laitteen hallinnan toiminnot edistymisen raportointia.

Tässä opetusohjelmassa näytetään, miten voit:

- Azure portaalin avulla voit luoda IoT-toiminnosta ja laitteen käyttäjätietojen luominen IoT-toiminnossa.
- Luo Simuloitu laitteissa, joissa on suora menetelmä, joka mahdollistaa uudelleenkäynnistyksen, jossa voit kutsua pilveen.
- Luo console-sovellus, joka kutsuu uudelleenkäynnistyksen suora menetelmä Simuloitu laitteeseen IoT-toiminnossa kautta.

Tässä opetusohjelmassa lopussa on kaksi Node.js console-sovellusta:

**dmpatterns_getstarted_device.js**, joka yhdistää IoT-toiminnossa aiemmin luotu laitteen käyttäjätietojen kanssa ja vastaanottaa uudelleenkäynnistyksen suora menetelmä, varmistaminen fyysinen uudelleenkäynnistys ja raporttien viimeisen uudelleenkäynnistyksen ajan.

**dmpatterns_getstarted_service.js**, joka kutsuu suora menetelmä luo Simuloitu laitteeseen, näyttää vastauksen ja näyttää päivitetyn laitteen sekä raportoitu ominaisuudet.

Tässä opetusohjelmassa suorittamiseen tarvitset seuraavat:

Node.js versio 0.12.x tai uudempi <br/>  [Paikallinen ympäristö valmistellaan kehittäminen] [ lnk-dev-setup] Node.js asentaminen Windows-tai Linux Tässä opetusohjelmassa kuvataan.

Azure active tili. (Jos sinulla ei ole tiliä, voit luoda [ilmaisen tilin] [ lnk-free-trial] -muutamaan minuuttiin.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Simuloitu laitteen sovelluksen luominen

Tässä osassa luot Node.js console-sovelluksen, joka vastaa suoraan menetelmän mukaan pilvipalveluun, joka käynnistää Simuloitu laite uudelleen ja käyttävän laitteen sekä raportoitu laitteen sekä kyselyiden tunnistaa laitteet ja kun ne viimeksi käynnistettävä ominaisuudet.

1. Luo uusi tyhjä kansio nimeltä **manageddevice**.  Luoda package.json tiedoston käyttämällä komentokehotteeseen seuraava komento **manageddevice** -kansioon.  Hyväksy kaikki oletusarvot:

    ```
    npm init
    ```
    
2. Komentoriville oman- **manageddevice** -kansiossa, suorita seuraava komento asentaa **azure-iot-device@dtpreview** laitteen SDK-paketti ja **azure-iot-device-mqtt@dtpreview** paketti:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Uusi **dmpatterns_getstarted_device.js** -tiedostoa tekstieditorissa, luo **manageddevice** -kansioon.

4. Lisää seuraavat 'vaadi' lauseet **dmpatterns_getstarted_device.js** tiedoston alkuun:

    ```
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. Lisää **connectionString** muuttujan ja luoda laitteen asiakas.  Korvaa laitteen yhteysmerkkijono yhteysmerkkijono.  

    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Lisää seuraava funktio toteuttaa suora menetelmä laitteeseen

    ```
    var onReboot = function(request, response) {
        
        // Respond the cloud app for the direct method
        response.send(200, 'Reboot started', function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
        
        // Report the reboot before the physical restart
        var date = new Date();
        var patch = {
            iothubDM : {
                reboot : {
                    lastReboot : date.toISOString(),
                }
            }
        };

        // Get device Twin
        client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                console.log('twin acquired');
                twin.properties.reported.update(patch, function(err) {
                    if (err) throw err;
                    console.log('Device reboot twin state reported')
                });  
            }
        });
        
        // Add your device's reboot API for physical restart.
        console.log('Rebooting!');
    };
    ```

7. Avaa yhteyden IoT-toiminnosta ja Aloita suora menetelmä kuuntelua:

    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not open IotHub client');
        }  else {
            console.log('Client opened.  Waiting for reboot method.');
            client.onDeviceMethod('reboot', onReboot);
        }
    });
    ```
    
8. Tallenna ja sulje **dmpatterns_getstarted_device.js** -tiedosto.

 [AZURE.NOTE] Tässä opetusohjelmassa avulla voit jatkaa samaa yksinkertainen, Toteuta uudelleen kaikki käytännöt. Tuotannon koodissa olisi Toteuta kuin ehdotettua [Lyhytkestoisia vika käsittely]MSDN-sivuston artikkelissa (kuten Eksponentiaalinen backoff), yritä käytännöt[lnk-transient-faults].

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Käynnistimen remote uudelleenkäynnistyksen suora menetelmä laitteella 

Tässä osassa voit luoda Node.js console-sovellus, joka käynnistää remote uudelleenkäynnistyksen suora menetelmä laitteella ja laitteen sekä kyselyt Etsi laitteen uudelleen viimeksi.

1. Luo uusi tyhjä kansio nimeltä **triggerrebootondevice**.  Luoda package.json tiedoston käyttämällä komentokehotteeseen seuraava komento **triggerrebootondevice** -kansioon.  Hyväksy kaikki oletusarvot:

    ```
    npm init
    ```
    
2. Komentoriville oman- **triggerrebootondevice** -kansiossa, suorita seuraava komento asentaa **azure-iothub@dtpreview** laitteen SDK-paketti ja **azure-iot-device-mqtt@dtpreview** paketti:

    ```
    npm install azure-iothub@dtpreview --save
    ```
    
3. Uusi **dmpatterns_getstarted_service.js** -tiedostoa tekstieditorissa, luo **triggerrebootondevice** -kansioon.

4. Lisää seuraavat 'vaadi' lauseet **dmpatterns_getstarted_service.js** tiedoston alkuun:

    ```
    'use strict';

    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```

5. Lisää seuraavat muuttujan määrittelyjä ja paikkamerkki-arvojen korvaaminen:

    ```
    var connectionString = '{iothubconnectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToReboot = 'myDeviceId';
    ```
    
6. Lisää seuraavan funktion käynnistää laitteen menetelmä Käynnistä Kohdelaite uudelleen:

    ```
    var startRebootDevice = function(twin) {

        var methodName = "reboot";
        
        var methodParams = {
            methodName: methodName,
            payload: null,
            timeoutInSeconds: 30
        };
        
        client.invokeDeviceMethod(deviceToReboot, methodParams, function(err, result) {
            if (err) { 
                console.error("Direct method error: "+err.message);
            } else {
                console.log("Successfully invoked the device to reboot.");  
            }
        });
    };
    ```

7. Lisää seuraavan funktion kyselyn laitteen ja Käynnistä viimeksi:

    ```
    var queryTwinLastReboot = function() {

        registry.getTwin(deviceToReboot, function(err, twin){

            if (twin.properties.reported.iothubDM != null)
            {
                if (err) {
                    console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
                } else {
                    var lastRebootTime = twin.properties.reported.iothubDM.reboot.lastReboot;
                    console.log('Last reboot time: ' + JSON.stringify(lastRebootTime, null, 2));
                }
            } else 
                console.log('Waiting for device to report last reboot time.');
        });
    };
    ```
    
8. Lisää seuraava koodi Soita Funktiot, jonka haluat käynnistävän uudelleenkäynnistyksen suora menetelmä ja kyselyn uudelleen viimeksi varten:

    ```
    startRebootDevice();
    setInterval(queryTwinLastReboot, 2000);
    ```
    
9. Tallenna ja sulje **dmpatterns_getstarted_service.js** -tiedosto.

## <a name="run-the-applications"></a>Suorita sovellukset

Olet nyt valmiina sovelluksia.

1. Komentokehotteen- **manageddevice** -kansioon, suorita seuraava komento aloittaa Kuuntele uudelleenkäynnistyksen suoraa menetelmää.

    ```
    node dmpatterns_getstarted_device.js
    ```

2. Komentokehotteen- **triggerrebootondevice** -kansiossa Suorita seuraava komento käynnistimeen remote uudelleen ja laitteen sekä viimeisen Etsi kyselyn uudelleen aika.

    ```
    node dmpatterns_getstarted_service.js
    ```

3. Näet suora menetelmä reagoivat tulostamalla viestin ulos

## <a name="customize-and-extend-the-device-management-actions"></a>Mukauttamiseen ja laajentamiseen laitteen hallintaan liittyvät toiminnot

IoT ratkaisujen laajentaa laitteen hallinta kuviot määritettyjä ja Mukautetut kuviot laitteen sekä ja C2D menetelmä perusalkioiden käyttöön. Muita esimerkkejä laitteen hallinnan toiminnot ovat factory Palauta, laiteohjelmiston päivitys, ohjelmistopäivitys, power hallinta, verkko- ja yhteyksien hallinta ja salauksen.

## <a name="device-maintenance-windows"></a>Laitteen ylläpito windows

Voit määrittää yleensä laitteiden määrittämänäsi ajankohtana pienentää keskeytyksiä ja käyttökatkot toimintojen suorittamiseen.  Laitteen ylläpito windows ovat usein käytettyjä kuvion määrittää ajan, kun laite on päivitettävä sen asetukset. Taustatietokantaan ratkaisujen käyttää laitteen sekä haluamasi ominaisuuksien määrittäminen ja aktivoi laitteen, joka mahdollistaa ylläpitovalintaikkunassa käytännön. Kun laite vastaanottaa ylläpito-ikkunassa käytännön, se käytännön tilan raportoiminen laitteen sekä raportoidun-ominaisuuden avulla. Taustatietokannan sovelluksen, voit annettu laitteet ja kunkin käytännön noudattaminen laitteen sekä kyselyjen avulla.

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä opetusohjelmassa suora menetelmä käyttää käynnistettävän remote uudelleen, valitse laite, jota käytetään laitteen sekä raportoidaan ominaisuudet ja raportoi uudelleen viimeksi laitteesta ja laitteen sekä tuttuihin pilvestä laitteen uudelleen viimeksi kysely.

Jatka ja käytön aloittaminen IoT keskittimeen laitteen hallinta kuviot, kuten etäyhteyksien päälle ilma laiteohjelmiston päivitys on:

[Opetusohjelma: Hallinnasta laitteisto-päivitys][lnk-fwupdate]

Lisätietoja laajentaa oman IoT ratkaisu ja joissa menetelmä kutsuu useilla eri laitteilla, näet [aikataulun ja lähetyksen työt] [ lnk-tutorial-jobs] opetusohjelma.

Jatka käytön aloittaminen IoT keskittimeen ohjeaiheessa [käytön aloittaminen IoT yhdyskäytävän SDK][lnk-gateway-SDK].


<!-- images and links -->
[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-fwupdate]: iot-hub-firmware-update.md
[Azure portal]: https://portal.azure.com/
[Using resource groups to manage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx