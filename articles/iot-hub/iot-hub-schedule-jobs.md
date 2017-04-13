<properties
 pageTitle="Töiden ajoittaminen | Microsoft Azure"
 description="Tässä opetusohjelmassa kerrotaan, miten haluat ajoittaa töitä"
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

# <a name="tutorial-schedule-and-broadcast-jobs-preview"></a>Opetusohjelma: Ajoittaminen ja lähettäminen töitä (ennakkoversio)

## <a name="introduction"></a>Johdanto
Azure IoT-toiminnossa on täysin hallitun palvelu, jonka avulla voit luoda ja seurata työt, jotka ajoittaminen ja päivittää laitteiden miljoonia sovelluksen takaisin loppu.  Työt voidaan käyttää seuraavia toimintoja:

- Päivitä laitteen sekä haluamasi ominaisuudet
- Päivitä laitteen sekä tunnisteet
- Käynnistää cloud laitteen menetelmät

Käsitteellisesti työn rivittyy jokin näistä toiminnoista ja seuraa suorittamisen vastaan määrittäminen laitteisiin, joka on määritetty sekä kyselyn etenemisen.  Esimerkiksi käyttämällä projektin sovelluksen takaisin loppu-voi käynnistää uudelleen menetelmän 10 000 laitteissa sekä kyselyn määrittämää ja ajoittaa myöhemmin.  Sovelluksen sitten voit seurata edistymistä, jokainen laite vastaanottaa ja Suorita Käynnistä-menetelmää.

Lue lisää eri näissä artikkeleissa seuraavia ominaisuuksia:

- Laitteen sekä ja ominaisuudet: [Aloita sekä] [ lnk-get-started-twin] ja [Opetusohjelma: sekä ominaisuuksien käyttäminen][lnk-twin-props]
- Cloud laitteen menetelmät: [Developer guide - suora menetelmät] [ lnk-dev-methods] ja [Opetusohjelma: C2D menetelmät][lnk-c2d-methods]

Tässä opetusohjelmassa näytetään, miten voit:

- Luo Simuloitu laitteissa, joissa on suora menetelmä, joka mahdollistaa lockDoor, joita voidaan kutsua sovelluksen takaisin loppu.
- Luo console-sovellus, joka soittaa lockDoor suora menetelmä luo Simuloitu laitteella työn ja päivittää laitteen avulla sekä haluamasi ominaisuudet.

Tässä opetusohjelmassa lopussa on kaksi Node.js console-sovellusta:

**simDevice.js**, joka muodostaa yhteyden IoT-toiminnosta ja laitteen käyttäjätietoja ja vastaanottaa lockDoor suoraa menetelmää.

**scheduleJobService.js**, johon soittaa suora menetelmä Simuloitu laitteeseen ja Päivitä käyttämällä työ sekä disired ominaisuudet.

Tässä opetusohjelmassa suorittamiseen tarvitset seuraavat:

Node.js versio 0.12.x tai uudempi <br/>  [Paikallinen ympäristö valmistellaan kehittäminen] [ lnk-dev-setup] Node.js asentaminen Windows-tai Linux Tässä opetusohjelmassa kuvataan.

Azure active tili. (Jos sinulla ei ole tiliä, voit luoda [ilmaisen tilin] [ lnk-free-trial] -muutamaan minuuttiin.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Simuloitu laitteen sovelluksen luominen

Tässä osassa luot Node.js console-sovelluksen, joka vastaa suoraan menetelmän mukaan pilvipalveluun, joka käynnistää Simuloitu laite uudelleen ja käyttävän laitteen sekä raportoitu laitteen sekä kyselyiden tunnistaa laitteet ja kun ne viimeksi käynnistettävä ominaisuudet.

1. Luo uusi tyhjä kansio nimeltä **simDevice**.  Luoda package.json tiedoston käyttämällä komentokehotteeseen seuraava komento **simDevice** -kansioon.  Hyväksy kaikki oletusarvot:

    ```
    npm init
    ```
    
2. Komentoriville oman- **simDevice** -kansiossa Suorita seuraava komento **azure-iot-laitteen** laitteen SDK-paketti ja **azure-iot-laite-mqtt** paketin asentaminen:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Uusi **simDevice.js** -tiedostoa tekstieditorissa, luo **simDevice** -kansioon.

4. Lisää seuraavat 'vaadi' lauseet **simDevice.js** tiedoston alkuun:

    ```
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. Lisää **connectionString** muuttujan ja luoda laitteen asiakas.  

    ```
    var connectionString = 'HostName={youriothostname};DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Lisää seuraava funktio käsittelee lockDoor-menetelmää.

    ```
    var onLockDoor = function(request, response) {
        
        // Respond the cloud app for the direct method
        response.send(200, function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
        
        console.log('Locking Door!');
    };
    ```

7. Lisää seuraava koodi lockDoor menetelmän tapahtumakäsittelijän rekisteröityä.

    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not connect to IotHub client.');
        }  else {
            console.log('Client connected to IoT Hub.  Waiting for reboot direct method.');
            client.onDeviceMethod('lockDoor', onLockDoor);
        }
    });
    ```
    
8. Tallenna ja sulje **simDevice.js** -tiedosto.

> [AZURE.NOTE] Tässä opetusohjelmassa avulla voit jatkaa samaa yksinkertainen, Toteuta uudelleen kaikki käytännöt. Tuotannon koodissa olisi Toteuta kuin ehdotettua [Lyhytkestoisia vika käsittely]MSDN-sivuston artikkelissa (kuten Eksponentiaalinen backoff), yritä käytännöt[lnk-transient-faults].

## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-twins-properties"></a>Ajoita työt suoraan kutsumista ja päivityksessä sekä ominaisuudet

Tässä osassa, joka käynnistää remote lockDoor suora menetelmä laitteella Node.js console-sovelluksen luominen ja päivittäminen sekä ominaisuudet.

1. Luo uusi tyhjä kansio nimeltä **scheduleJobService**.  Luoda package.json tiedoston käyttämällä komentokehotteeseen seuraava komento **scheduleJobService** -kansioon.  Hyväksy kaikki oletusarvot:

    ```
    npm init
    ```
    
2. Komentoriville oman- **scheduleJobService** -kansiossa Suorita seuraava komento **azure iothub** laitteen SDK-paketti ja **azure-iot-laite-mqtt** paketin asentaminen:

    ```
    npm install azure-iothub@dtpreview uuid --save
    ```
    
3. Uusi **scheduleJobService.js** -tiedostoa tekstieditorissa, luo **scheduleJobService** -kansioon.

4. Lisää seuraavat 'vaadi' lauseet **dmpatterns_gscheduleJobServiceetstarted_service.js** tiedoston alkuun:

    ```
    'use strict';

    var uuid = require('uuid');
    var JobClient = require('azure-iothub').JobClient;
    ```

5. Lisää seuraavat muuttujan määrittelyjä ja paikkamerkki-arvojen korvaaminen:

    ```
    var connectionString = '{iothubconnectionstring}';
    var deviceArray = ['myDeviceId'];
    var startTime = new Date();
    var maxExecutionTimeInSeconds =  3600;
    var jobClient = JobClient.fromConnectionString(connectionString);
    ```
    
6. Lisää seuraavan funktion, jota käytetään suorittamisen työn seurannassa:

    ```
    function monitorJob (jobId, callback) {
        var jobMonitorInterval = setInterval(function() {
            jobClient.getJob(jobId, function(err, result) {
            if (err) {
                console.error('Could not get job status: ' + err.message);
            } else {
                console.log('Job: ' + jobId + ' - status: ' + result.status);
                if (result.status === 'completed' || result.status === 'failed' || result.status === 'cancelled') {
                clearInterval(jobMonitorInterval);
                callback(null, result);
                }
            }
            });
        }, 5000);
    }
    ```

7. Lisää seuraava koodi, joka kutsuu laitteen menetelmää Työn ajoittaminen:

    ```
    var methodParams = {
        methodName: 'lockDoor',
        payload: null,
        timeoutInSeconds: 45
    };

    var methodJobId = uuid.v4();
    console.log('scheduling Device Method job with id: ' + methodJobId);
    jobClient.scheduleDeviceMethod(methodJobId,
                                deviceArray,
                                methodParams,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule device method job: ' + err.message);
        } else {
            monitorJob(methodJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor device method job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```
        
8. Lisää seuraava koodi päivitettävä sekä työn ajoittaminen:

    ```
    var twinPatch = {
        etag: '*',
        desired: {
            building: '43',
            floor: 3
        }
    };

    var twinJobId = uuid.v4();

    console.log('scheduling Twin Update job with id: ' + twinJobId);
    jobClient.scheduleTwinUpdate(twinJobId,
                                deviceArray,
                                twinPatch,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule twin update job: ' + err.message);
        } else {
            monitorJob(twinJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor twin update job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```
    
9. Tallenna ja sulje **scheduleJobService.js** -tiedosto.

## <a name="run-the-applications"></a>Suorita sovellukset

Olet nyt valmiina sovelluksia.

1. Komentokehotteen- **simDevice** -kansioon, suorita seuraava komento aloittaa Kuuntele uudelleenkäynnistyksen suoraa menetelmää.

    ```
    node simDevice.js
    ```

2. Komentokehotteen- **scheduleJobService** -kansiossa Suorita seuraava komento käynnistimeen remote uudelleen ja laitteen sekä viimeisen Etsi kyselyn uudelleen aika.

    ```
    node scheduleJobService.js
    ```

3. Näet tulosteen laitteesta-ja Edellinen sovelluksia.


## <a name="next-steps"></a>Seuraavat vaiheet

Tässä opetusohjelmassa käytetään Työn ajoittaminen suora menetelmä, jolla laite ja laitteen sekä ominaisuudet päivitys.

Jatka ja käytön aloittaminen IoT keskittimeen laitteen hallinta kuviot, kuten etäyhteyksien päälle ilma laiteohjelmiston päivitys on:

[Opetusohjelma: Hallinnasta laitteisto-päivitys][lnk-fwupdate]

Jatka käytön aloittaminen IoT keskittimeen ohjeaiheessa [käytön aloittaminen IoT yhdyskäytävän SDK][lnk-gateway-SDK].

[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-props]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: iot-hub-firmware-update.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx