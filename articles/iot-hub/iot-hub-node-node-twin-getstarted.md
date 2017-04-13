<properties
    pageTitle="Aloita twins | Microsoft Azure"
    description="Tässä opetusohjelmassa näytetään, miten voit käyttää twins"
    services="iot-hub"
    documentationCenter="node"
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="node"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/13/2016"
     ms.author="elioda"/>

# <a name="tutorial-get-started-with-device-twins-preview"></a>Opetusohjelma: Käytön aloittaminen laitteen twins (ennakkoversio)

[AZURE.INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Tässä opetusohjelmassa lopussa on kaksi Node.js console-sovellusta:

* **AddTagsAndQuery.js**, Node.js-sovellus voidaan suorittaa taustatietokantaan, joka lisää tunnisteet ja kyselyt laitteen twins tarkoitus.
* **TwinSimulatedDevice.js**, Node.js-sovellus, joka varmistaminen laitteella, joka muodostaa yhteyden IoT keskittimeen aiemmin luotu laitteen käyttäjätietoja ja yhteyden kunto raportoi.

> [AZURE.NOTE] Artikkelin [IoT keskittimeen SDK: T] [ lnk-hub-sdks] on tietoja eri SDK: T, että voit luoda laitteen ja taustatietokantaan sovelluksia.

Tässä opetusohjelmassa suorittamiseen tarvitset seuraavat:

+ Node.js versio 0.10.x tai uudempi versio.

+ Azure active tili. (Jos sinulla ei ole tiliä, voit luoda [ilmaisen tilin] [ lnk-free-trial] -muutamaan minuuttiin.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-the-service-app"></a>Service-sovelluksen luominen

Tässä osassa Node.js console-sovelluksen, joka lisää sijainti-metatiedot **myDeviceId**liittyvät sekä luominen. Sen jälkeen kyselyjä, jotka on tallennettu valitsemalla laitteet toiminnossa twins, joka sijaitsee Yhdysvalloissa ja valitse sitten niistä, raportointi matkapuhelinverkon kautta.

1. Luo uusi tyhjä kansio nimeltä **addtagsandqueryapp**. **Addtagsandqueryapp** -kansioon Luo uusi package.json tiedosto käyttämällä komentokehotteeseen seuraava komento. Hyväksy kaikki oletusarvot:

    ```
    npm init
    ```

2. Komentoriville oman-varten suorittamalla seuraavan komennon Asenna **azure iothub** paketti **addtagsandqueryapp** -kansiossa:

    ```
    npm install azure-iothub@dtpreview --save
    ```

3. Uusi **AddTagsAndQuery.js** -tiedostoa tekstieditorissa, luo **addtagsandqueryapp** -kansioon.

4. Lisää seuraava koodi **AddTagsAndQuery.js** -tiedostoon ja korvata **{palvelun yhteysmerkkijonon}** -paikkamerkki, jossa on kopioitu Oman keskittimeen luotaessa yhteysmerkkijonon:

        'use strict';
        var iothub = require('azure-iothub');
        var connectionString = '{service hub connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);

        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var patch = {
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                      }
                    }
                };
             
                twin.update(patch, function(err) {
                  if (err) {
                    console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                  } else {
                    console.log(twin.deviceId + ' twin updated successfully');
                    queryTwins();
                  }
                });
            }
        });

    **Rekisterin** objekti sisältää kaikki menetelmät, pakollinen vuorovaikutuksessa laitteen twins-palvelusta. Edellinen koodi alustaa ensin **rekisterin** kohdetta, ja valitse hakee **myDeviceId**varten sekä ja päivittää sen tunnisteet lopuksi alatunnisteessa tiedoilla.

    Kun olet päivittänyt tunnisteet kutsuu **queryTwins** -funktio.

7. Lisää seuraava koodi **AddTagsAndQuery.js** toteuttamisesta **queryTwins** -funktion lopussa:

        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });
            
            query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43 using cellular network: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });
        };

    Edellinen koodi suorittaa kyselyt: ensimmäisen valitsee vain laitteen twins **Redmond43** tehdas niin sijaitsevat laitteiden ja toinen parantaa siirtymästä kysely ja valitse vain matkapuhelinverkon verkon kautta myös liitettyjen laitteiden.

    Huomautus Edellinen koodi, kun se luo **kysely** -kohdetta, määrittää enimmäismäärä palautetut tiedostot. **Kyselyn** objekti sisältää **hasMoreResults** totuusarvo ominaisuutta, joiden avulla voit käynnistää **nextAsTwin** menetelmiä useita kertoja noutaa kaikki tulokset. **Seuraavan** menetelmän on käytettävissä, jotka eivät ole laitteen twins esimerkiksi kooste kyselyjen tulokset tulokset.

8. Suorita sovellus kanssa:

        node AddTagsAndQuery.js

    Raportissa pitäisi näkyä kaikissa laitteissa, jotka sijaitsevat **Redmond43** ja ei mitään, kyselyn, joka rajoittaa tulokset matkapuhelinverkon verkon käyttävät laitteet pyytävä kyselyn tulokset yhteen laitteeseen.

    ![][1]

Seuraavassa osassa voit luoda laite-sovellusta, joka ilmoittaa yhteyden tiedot ja muuttaa edellisessä osassa kyselyn tuloksen.

## <a name="create-the-device-app"></a>Laite-sovelluksen luominen

Tässä osassa voit luoda Node.js, console-sovellusta, joka muodostaa yhteyden oman keskittimeen **myDeviceId**ja päivittää sen sekä raportoitu tietoa, että se on liitetty matkapuhelinverkon verkon ominaisuudet.

> [AZURE.NOTE] Tällä hetkellä voi käyttää vain IoT keskittimeen MQTT-protokollan avulla yhteyden muodostavien laitteiden laitteen twins. Lue lisätietoja [MQTT tukevat] [ lnk-devguide-mqtt] artikkelista ohjeita siitä, miten voit muuntaa olemassa olevan laitteen-sovelluksen käyttämään MQTT.

1. Luo uusi tyhjä kansio nimeltä **reportconnectivity**. **Reportconnectivity** -kansioon Luo uusi package.json tiedosto käyttämällä komentokehotteeseen seuraava komento. Hyväksy kaikki oletusarvot:

    ```
    npm init
    ```

2. Komentoriville oman- **reportconnectivity** -kansiossa Suorita seuraava komento **azure-iot-laite**ja **azure-iot-laite-mqtt** paketin asentaminen:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Uusi **ReportConnectivity.js** -tiedostoa tekstieditorissa, luo **reportconnectivity** -kansioon.

4. Lisää seuraava koodi **ReportConnectivity.js** -tiedostoon ja korvata **{laitteen yhteysmerkkijonon}** -paikkamerkki, jossa on kopioitu **myDeviceId** laitteen käyttäjätietoja luotaessa yhteysmerkkijonon:

        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;

        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);

        client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');

            client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                var patch = {
                    connectivity: {
                        type: 'cellular'
                    }
                };

                twin.properties.reported.update(patch, function(err) {
                    if (err) {
                        console.error('could not update twin');
                    } else {
                        console.log('twin state reported');
                        process.exit();
                    }
                });
            }
            });
        }
        });

    **Asiakas** -objekti sisältää kaikki asetat laitteen twins laitteesta ja käsittele menetelmät. Edellinen koodi sen jälkeen, kun se alustaa **asiakas** -objektin hakee **myDeviceId** varten sekä ja päivittää sen raportoidun ominaisuuden yhteyden tiedot.

5. Suorita laite-sovellus

        node ReportConnectivity.js

    Sanoma tulee näkyviin `twin state reported`.

6. Nyt, kun laite raportoidun yhteyden sen tiedot, se pitäisi näkyä sekä kyselyt. Siirry takaisin **addtagsandqueryapp** -kansioon ja Suorita kyselyt uudelleen:

        node AddTagsAndQuery.js

    Tämän ajan **myDeviceId** pitäisi näkyä sekä kyselyn tuloksiin.

    ![][3]

## <a name="next-steps"></a>Seuraavat vaiheet
Tässä opetusohjelmassa määritetty keskittimeen IoT Azure-portaalissa, ja luo laitteen identity toiminnossa tunnistetietojen rekisterissä. Laitteen metatiedot lisättyjä tunnisteita taustatietokantaan-sovelluksesta ja kirjoittamasi Simuloitu laite-sovelluksen raportin laitteen yhteyden tiedot-laitteen sekä. Opit, myös nämä tiedot käyttämällä IoT-toiminnossa SQL kaltaisessa kyselykielen kyselyjen tekeminen.

Seuraavien resurssien avulla opit miten:

- Lähetä telemetriatietojen käytetyssä laitteessa on [aloittaminen IoT keskittimeen] [ lnk-iothub-getstarted] opetusohjelmassa
- Määritä laitteiden avulla sekä käyttäjän haluamasi ominaisuudet [haluamasi ominaisuuksien määrittäminen laitteet] [ lnk-twin-how-to-configure] opetusohjelmassa
- ohjata laitteita vuorovaikutteisesti (kuten ottaminen käyttöön tuuletin käyttäjän hallita sovelluksen), [Käytä suoraan menetelmiä] [ lnk-methods-tutorial] opetusohjelma.

<!-- images -->
[1]: media/iot-hub-node-node-twin-getstarted/service1.png
[3]: media/iot-hub-node-node-twin-getstarted/service2.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/

[lnk-twin-how-to-configure]: iot-hub-node-node-twin-how-to-configure.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-methods-tutorial]: iot-hub-c2d-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md