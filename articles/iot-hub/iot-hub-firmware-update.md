<properties
 pageTitle="Laitteisto-ohjelmiston päivityksen hallinnasta | Microsoft Azure"
 description="Tässä opetusohjelmassa kerrotaan, miten voit tehdä laitteisto-päivitys"
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

# <a name="tutorial-how-to-do-a-firmware-update-preview"></a>Opetusohjelma: Hallinnasta laitteisto-ohjelmiston päivitys (ennakkoversio)

## <a name="introduction"></a>Johdanto
[Mobiililaitteiden hallinnan käytön aloittaminen] [ lnk-dm-getstarted] opetusohjelman näyttöön tuli käyttämisestä [laitteen sekä] [ lnk-devtwin] ja [cloud laitteen (C2D) menetelmiä] [ lnk-c2dmethod] perusalkioiden käynnistämään laitteen etäyhteyden välityksellä. Tässä opetusohjelmassa käyttää samaa IoT keskittimeen perusalkioiden ja ohjeita ja kerrotaan, miten voit tehdä lopusta loppuun Simuloitu laitteisto-päivitys.  Tätä mallia käytetään laitteisto-päivityksen toteutuksen Intel Edison laitteen malli.

Tässä opetusohjelmassa näytetään, miten voit:

- Luo console-sovellus, joka kutsuu firmwareUpdate suora menetelmä Simuloitu laitteeseen IoT-toiminnossa kautta.
- Luo Simuloitu laitteella, joka toteuttaa firmwareUpdate suora menetelmä joka käy läpi monivaiheinen prosessi, joka odottaa, voit ladata laitteisto-kuva, lataa laitteisto-kuva ja lopuksi koskee th laitteisto kuva.  Koko suoritetaan jokaisen vaiheen sekä raportoitu ominaisuudet päivitetään laitteen käyttää edistyminen.

Tässä opetusohjelmassa lopussa on kaksi Node.js console-sovellusta:

**dmpatterns_fwupdate_service.js**, johon soittaa suora menetelmä luo Simuloitu laitteeseen, näyttää vastauksen ja säännöllisesti (joka 500ms) näyttää päivitetyn laitteen sekä raportoitu ominaisuudet.

**dmpatterns_fwupdate_device.js**, joka yhdistää IoT-toiminnossa aiemmin luotu laitteen käyttäjätietojen kanssa ja vastaanottaa firmwareUpdate suora menetelmä, voit simuloida laitteisto-päivitys-myös usean tilan prosessin suoritetaan: näköistiedoston lataus odottaa, uuden kuvan lataamisesta ja käyttämisestä lopuksi kuva.


Tässä opetusohjelmassa suorittamiseen tarvitset seuraavat:

Node.js versio 0.12.x tai uudempi <br/>  [Paikallinen ympäristö valmistellaan kehittäminen] [ lnk-dev-setup] Node.js asentaminen Windows-tai Linux Tässä opetusohjelmassa kuvataan.

Azure active tili. (Jos sinulla ei ole tiliä, voit luoda [ilmaisen tilin] [ lnk-free-trial] -muutamaan minuuttiin.)

Noudata luominen IoT-toiminnosta ja tietoyhteyden yhteysmerkkijonon [Mobiililaitteiden hallinnan käytön aloittaminen](iot-hub-device-management-get-started.md) -artikkelissa.

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Simuloitu laitteen sovelluksen luominen

Tässä osiossa luot Node.js console-sovelluksen, joka vastaa suoraan menetelmän mukaan pilvipalveluun, joka käynnistää Simuloitu laitteen laitteisto-päivitys ja käyttävän laitteen sekä raportoitu ominaisuudet laitteen sekä kyselyiden tunnistaa laitteet ja kun ne viimeksi käynnistettävä.

1. Luo uusi tyhjä kansio nimeltä **manageddevice**.  Luoda package.json tiedoston käyttämällä komentokehotteeseen seuraava komento **manageddevice** -kansioon.  Hyväksy kaikki oletusarvot:

    ```
    npm init
    ```
    
2. Komentoriville oman- **manageddevice** -kansiossa, suorita seuraava komento asentaa **azure-iot-device@dtpreview** laitteen SDK-paketti ja **azure-iot-device-mqtt@dtpreview** paketti:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Uusi **dmpatterns_fwupdate_device.js** -tiedostoa tekstieditorissa, luo **manageddevice** -kansioon.

4. Lisää seuraavat 'vaadi' lauseet **dmpatterns_fwupdate_device.js** tiedoston alkuun:

    ```
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. Lisää **connectionString** muuttujan ja luoda laitteen asiakas.  

    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Lisää seuraava teksti-funktiota käytetään Päivitä sekä, joka ilmoittaa ominaisuudet

    ```
    var reportFWUpdateThroughTwin = function(twin, firmwareUpdateValue) {
      var patch = {
          iothubDM : {
            firmwareUpdate : firmwareUpdateValue
          }
      };

      twin.properties.reported.update(patch, function(err) {
        if (err) throw err;
        console.log('twin state reported')
      });
    };
    ```
    
7. Lisää seuraavat tehtävät, joiden simuloida lataaminen ja käyttäminen laitteisto-kuva.

    ```
    var simulateDownloadImage = function(imageUrl, callback) {
      var error = null;
      var image = "[fake image data]";
      
      console.log("Downloading image from " + imageUrl);
      
      callback(error, image);
    }

    var simulateApplyImage = function(imageData, callback) {
      var error = null;
      
      if (!imageData) {
        error = {message: 'Apply image failed because of missing image data.'};
      }
      
      callback(error);
    }
    ```

8. Lisää seuraava teksti-funktiota, joka päivittyy laiteohjelmiston päivityksen tilan kautta sekä raportoitu ominaisuudet odottaa Lataa.  Yleensä käytettävissä päivitys tiedotetaan laitteet ja järjestelmänvalvojan määrittämät käytännön syitä laitteen Aloita lataamalla ja päivittämistä.  Tämä on, jos kyseinen käytäntö on otettu käyttöön logiikan suoritetaan.  Yksinkertaisuuden syy on 4 sekuntia viivästyy ja aloittamalla Lataa laitteisto-kuva. 

    ```
    var waitToDownload = function(twin, fwPackageUriVal, callback) {
      var now = new Date();
      
      reportFWUpdateThroughTwin(twin, {
        fwPackageUri: fwPackageUriVal,
        status: 'waiting',
        error : null,
        startedWaitingTime : now.toISOString()
      });
      setTimeout(callback, 4000);
    };
    ```
    
9. Lisää seuraava teksti-funktiota, joka päivittyy laiteohjelmiston päivityksen tilan kautta sekä raportoitu ominaisuudet ladataan laitteisto-kuvaa.  Se seuraa simuloimalla laitteisto-ohjelmiston lataaminen ja päivittää lopuksi laiteohjelmiston päivityksen tila ilmoittaa, joko lataamisen onnistumisesta tai epäonnistumisesta.

    ```
    var downloadImage = function(twin, fwPackageUriVal, callback) {
      var now = new Date();   
      
      reportFWUpdateThroughTwin(twin, {
        status: 'downloading',
      });
      
      setTimeout(function() {
        // Simulate download
        simulateDownloadImage(fwPackageUriVal, function(err, image) {
          
          if (err)
          {
            reportFWUpdateThroughTwin(twin, {
              status: 'downloadfailed',
              error: {
                code: error_code,
                message: error_message,
              }
            });
          }
          else {        
            reportFWUpdateThroughTwin(twin, {
              status: 'downloadComplete',
              downloadCompleteTime: now.toISOString(),
            });
            
            setTimeout(function() { callback(image); }, 4000);   
          }
        });
        
      }, 4000);
    }
    ```
    
10. Lisää seuraava teksti-funktiota, joka päivittyy laiteohjelmiston päivityksen tilan kautta sekä raportoitu ominaisuudet käyttämällä laitteisto-kuva.  Se seuraa simuloimalla käyttämällä laitteisto kuvan ja päivittää lopuksi laiteohjelmiston päivityksen tila ilmoittaa, joko Käytä onnistumisesta tai epäonnistumisesta.

    ```
    var applyImage = function(twin, imageData, callback) {
      var now = new Date();   
      
      reportFWUpdateThroughTwin(twin, {
        status: 'applying',
        startedApplyingImage : now.toISOString()
      });
      
      setTimeout(function() {
        
        // Simulate apply firmware image
        simulateApplyImage(imageData, function(err) {
          if (err) {
            reportFWUpdateThroughTwin(twin, {
              status: 'applyFailed',
              error: {
                code: err.error_code,
                message: err.error_message,
              }
            });
          } else { 
            reportFWUpdateThroughTwin(twin, {
              status: 'applyComplete',
              lastFirmwareUpdate: now.toISOString()
            });    
            
          }
        });
        
        setTimeout(callback, 4000);
        
      }, 4000);
    }
    ```

11. Lisää seuraavat functoin, joka käsittelee firmwareUpdate-menetelmä ja aloittaa monivaiheinen laitteisto Päivitysprosessista.

    ```
    var onFirmwareUpdate = function(request, response) {
            
      // Respond the cloud app for the direct method
      response.send(200, 'FirmwareUpdate started', function(err) {
        if (!err) {
          console.error('An error occured when sending a method response:\n' + err.toString());
        } else {
          console.log('Response to method \'' + request.methodName + '\' sent successfully.');
        }
      });

      // Get the parameter from the body of the method request
      var fwPackageUri = JSON.parse(request.payload).fwPackageUri;

      // Obtain the device twin
      client.getTwin(function(err, twin) {
        if (err) {
          console.error('Could not get device twin.');
        } else {
          console.log('Device twin acquired.');
          
          // Start the multi-stage firmware update
          waitToDownload(twin, fwPackageUri, function() {
            downloadImage(twin, fwPackageUri, function(imageData) {
              applyImage(twin, imageData, function() {});    
            });  
          });

        }
      });
    }
    ```
    
12. Lisää lopuksi seuraava koodi muodostaa yhteyden IoT keskittimeen laite, 

    ```
    client.open(function(err) {
      if (err) {
        console.error('Could not connect to IotHub client');
      }  else {
        console.log('Client connected to IoT Hub.  Waiting for firmwareUpdate direct method.');
      }
      
      client.onDeviceMethod('firmwareUpdate', onFirmwareUpdate(request, response));
    });
    ```

> [AZURE.NOTE] Tässä opetusohjelmassa avulla voit jatkaa samaa yksinkertainen, Toteuta uudelleen kaikki käytännöt. Tuotannon koodissa olisi Toteuta kuin ehdotettua [Lyhytkestoisia vika käsittely]MSDN-sivuston artikkelissa (kuten Eksponentiaalinen backoff), yritä käytännöt[lnk-transient-faults].

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a>Suora menetelmä laitteella remote laitteisto-ohjelmiston päivityksen käynnistäminen 

Tässä osassa voit luoda Node.js console-sovellus, joka käynnistää remote laitteisto-päivityksen suora menetelmä laitteella ja laitteen sekä kyselyt saat säännöllisesti kyseiseen laitteeseen aktiivinen laiteohjelmiston päivityksen tila.


1. Luo uusi tyhjä kansio nimeltä **triggerfwupdateondevice**.  Luoda package.json tiedoston käyttämällä komentokehotteeseen seuraava komento **triggerfwupdateondevice** -kansioon.  Hyväksy kaikki oletusarvot:

    ```
    npm init
    ```
    
2. Oman-komentorivin **triggerfwupdateondevice** -kansiossa Suorita seuraava komento **azure iothub** laitteen SDK-paketti ja **azure-iot-laite-mqtt** paketin asentaminen:

    ```
    npm install azure-iot-hub@dtpreview --save
    ```
    
3. Uusi **dmpatterns_getstarted_service.js** -tiedostoa tekstieditorissa, luo **triggerfwupdateondevice** -kansioon.

4. Lisää seuraavat 'vaadi' lauseet **dmpatterns_getstarted_service.js** tiedoston alkuun:

    ```
    'use strict';

    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```

5. Lisää seuraavat muuttujan määrittelyjä ja paikkamerkki-arvojen korvaaminen:

    ```
    var connectionString = '{device_connectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToUpdate = 'myDeviceId';
    ```
    
6. Lisää seuraavan funktion etsimiseen ja näyttö firmwareUpdate arvo ilmoitetaan ominaisuus.

    ```
    var queryTwinFWUpdateReported = function() {
        registry.getTwin(deviceToUpdate, function(err, twin){
            if (err) {
              console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
            } else {
              console.log((JSON.stringify(twin.properties.reported.iothubDM.firmwareUpdate)) + "\n");
            }
        });
    };
    ```

7. Lisää seuraavan funktion käynnistää firmwareUpdate menetelmä Käynnistä Kohdelaite uudelleen:

    ```
    var startFirmwareUpdateDevice = function() {
      var params = {
          fwPackageUri: 'https://secureurl'
      };
      
      var methodName = "firmwareUpdate";
      var payloadData =  JSON.stringify(params);
      
      var methodParams = {
        methodName: methodName,
        payload: payloadData,
        timeoutInSeconds: 30
      };
      
      client.invokeDeviceMethod(deviceToUpdate, methodParams, function(err, result) {
        if (err) {
          console.error('Could not start the firmware update on the device: ' + err.message)
        } 
      });
    };
    ```

8. Lopuksi Lisää seuraavan funktion Aloita laitteisto-koodiin Päivitä sarjan ja tulevat säännöllisesti sekä raportoitu ominaisuudet:

    ```
    startFirmwareUpdateDevice();
    setInterval(queryTwinFWUpdateReported, 500);
    ```
    
9. Tallenna ja sulje **dmpatterns_fwupdate_service.js** -tiedosto.

## <a name="run-the-applications"></a>Suorita sovellukset

Olet nyt valmiina sovelluksia.

1. Komentokehotteen- **manageddevice** -kansioon, suorita seuraava komento aloittaa Kuuntele uudelleenkäynnistyksen suoraa menetelmää.

    ```
    node dmpatterns_fwupdate_device.js
    ```

2. Komentokehotteen- **triggerfwupdateondevice** -kansiossa Suorita seuraava komento käynnistimeen remote uudelleen ja laitteen sekä viimeisen Etsi kyselyn uudelleen aika.

    ```
    node dmpatterns_fwupdate_service.js
    ```

3. Näet suora menetelmä reagoivat tulostamalla viestin ulos

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä opetusohjelmassa suora menetelmä käyttää käynnistettävän remote laitteisto-päivityksen laitteessa ja käyttää säännöllisesti sekä ilmoitetaan ominaisuudet ymmärtää laitteisto edistymisen päivittäminen prosessi.  

Lisätietoja laajentaa oman IoT ratkaisu ja joissa menetelmä kutsuu useilla eri laitteilla, näet [aikataulun ja lähetyksen työt] [ lnk-tutorial-jobs] opetusohjelma.

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
