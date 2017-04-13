<properties
    pageTitle="Käytä sekä ominaisuuksia | Microsoft Azure"
    description="Tässä opetusohjelmassa näytetään, miten voit käyttää sekä ominaisuudet"
    services="iot-hub"
    documentationCenter=".net"
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

# <a name="tutorial-use-desired-properties-to-configure-devices-preview"></a>Opetusohjelma: Haluamasi ominaisuuksien avulla voit määrittää laitteita (ennakkoversio)

[AZURE.INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Tässä opetusohjelmassa lopussa on kaksi Node.js console-sovellusta:

* **SimulateDeviceConfiguration.js**, Simuloitu laite-sovellusta, joka odottaa uudet määritykset päivitys ja ilmoittaa Simuloitu määritysten Päivitysprosessista.
* **SetDesiredConfigurationAndQuery.js**, Node.js-sovellus voidaan suorittaa taustatietokantaan, joka määrittää uudet määritykset laitteessa ja kyselyt päivityksen määritysprosessia tarkoitus.

> [AZURE.NOTE] Artikkelin [IoT keskittimeen SDK: T] [ lnk-hub-sdks] on tietoja eri SDK: T, että voit luoda laitteen ja taustatietokantaan sovelluksia.

Tässä opetusohjelmassa suorittamiseen tarvitset seuraavat:

+ Node.js versio 0.10.x tai uudempi versio.

+ Azure active tili. (Jos sinulla ei ole tiliä, voit luoda [ilmaisen tilin] [ lnk-free-trial] -muutamaan minuuttiin.)

Jos olet toiminut [aloittaminen laitteen twins] [ lnk-twin-tutorial] opetusohjelmassa on jo laitteen käytössä hallinta-toiminnosta ja laite-tunnistetiedot, kutsutaan **myDeviceId**; Voit ohittaa [Simuloitu laite-sovelluksen luominen] ja[ lnk-how-to-configure-createapp] osa.

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-the-simulated-device-app"></a>Simuloitu laite-sovelluksen luominen

Tässä osassa voit luoda Node.js console-sovellus, joka muodostaa yhteyttä **myDeviceId**keskittimeen, odottaa uudet määritykset päivitys ja sitten raportit päivitykset Simuloitu määritysprosessia päivitys.

1. Luo uusi tyhjä kansio nimeltä **simulatedeviceconfiguration**. **Simulatedeviceconfiguration** -kansioon Luo uusi package.json tiedosto käyttämällä komentokehotteeseen seuraava komento. Hyväksy kaikki oletusarvot:

    ```
    npm init
    ```

2. Komentoriville oman- **simulatedeviceconfiguration** -kansiossa Suorita seuraava komento **azure-iot-laite**ja **azure-iot-laite-mqtt** paketin asentaminen:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Uusi **SimulateDeviceConfiguration.js** -tiedostoa tekstieditorissa, luo **simulatedeviceconfiguration** -kansioon.

4. Lisää seuraava koodi **SimulateDeviceConfiguration.js** -tiedostoon ja korvata **{laitteen yhteysmerkkijonon}** -paikkamerkki, jossa on kopioitu **myDeviceId** laitteen käyttäjätietoja luotaessa yhteysmerkkijonon:

        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;

        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);

        client.open(function(err) {
            if (err) {
                console.error('could not open IotHub client');
            } else {
                client.getTwin(function(err, twin) {
                    if (err) {
                        console.error('could not get twin');
                    } else {
                        console.log('retrieved device twin');
                        twin.properties.reported.telemetryConfig = {
                            configId: "0",
                            sendFrequency: "24h"
                        }
                        twin.on('properties.desired', function(desiredChange) {
                            console.log("received change: "+JSON.stringify(desiredChange));
                            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
                            if (desiredChange.telemetryConfig &&desiredChange.telemetryConfig.configId !== currentTelemetryConfig.configId) {
                                initConfigChange(twin);
                            }
                        });
                    }
                });
            }
        });

    **Asiakas** -objekti sisältää kaikki tarvittavat laitteen twins laitteesta vuorovaikutuksessa menetelmät. Edellinen koodi sen jälkeen, kun se alustaa **asiakas** -objektin hakee sekä **myDeviceId**varten, ja liittää tapahtumakäsittelijän päivityksen haluamasi ominaisuudet. Käsittelijä varmistaa, että on todellinen määritys-muutospyyntö vertaamalla configIds ja sitten käynnistää menetelmän, joka alkaa määritysten muutos.

    Huomaa, että yksinkertaisuuden, Edellinen koodi käyttää oletusarvoinen koodattu alkuperäiset määritykset. Reaali app todennäköisesti ladata, määritys paikallisesta tallennussijainnista.
    
    > [AZURE.IMPORTANT] Halutun ominaisuuden muuttaminen tapahtumat ovat aina lähettämän kerran laitteen verkkoyhteydessä, varmista, että voit tarkistaa, että ennen kuin suoritat toiminnon haluamasi ominaisuuksiin todellinen muutos.

5. Lisää seuraavat toimet ennen `client.open()` käynnistäminen:

        var initConfigChange = function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.pendingConfig = twin.properties.desired.telemetryConfig;
            currentTelemetryConfig.status = "Pending";

            var patch = {
            telemetryConfig: currentTelemetryConfig
            };
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.log('Could not report properties');
                } else {
                    console.log('Reported pending config change: ' + JSON.stringify(patch));
                    setTimeout(function() {completeConfigChange(twin);}, 60000);
                }
            });
        }

        var completeConfigChange =  function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.configId = currentTelemetryConfig.pendingConfig.configId;
            currentTelemetryConfig.sendFrequency = currentTelemetryConfig.pendingConfig.sendFrequency;
            currentTelemetryConfig.status = "Success";
            delete currentTelemetryConfig.pendingConfig;
            
            var patch = {
                telemetryConfig: currentTelemetryConfig
            };
            patch.telemetryConfig.pendingConfig = null;

            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.error('Error reporting properties: ' + err);
                } else {
                    console.log('Reported completed config change: ' + JSON.stringify(patch));
                }
            });
        };

    **InitConfigChange** menetelmä päivittää config pyynnössä raportoidun paikallisen sekä objektin ominaisuudet ja määrittää tilaksi **odottaa**ja valitse päivittää laitteen sekä-palvelusta. Kun olet päivittänyt onnistuneesti sekä, varmistaminen pitkään käynnissä prosessi, joka päättyy **completeConfigChange**suorittamisen. Tämä menetelmä päivittää paikallisen sekä raportoidun ominaisuudet tilan asettaminen **onnistumisen** ja poistamalla **pendingConfig** -objekti. Valitse päivittää sekä-palvelusta.

    Huomaa, joka tallentaa kaistanleveys, ilmoitettu ominaisuudet päivitetään määrittämällä vain muokattava (nimetty **korjaustiedoston** yllä koodissa), koko asiakirjaan korvaamatta ominaisuudet.

    > [AZURE.NOTE] Tässä opetusohjelmassa simuloida ei mitään toiminta samanaikainen määritysten päivitykset. Jotkin määritysten päivityksen prosessit ehkä muutoksia kohde-määritys vastaavaksi, kun päivitys on käynnissä ja muiden käyttäjien on ehkä jonon ne muiden saattaa hylätä ne virhetilassa. Varmista, että haluamasi toiminnan muuttaminen tietyn määritysprosessia huomioitavia ja lisää haluamasi logiikan ennen aloitetaan määritysten muutos.

6. Suorita laite-sovellus:

        node SimulateDeviceConfiguration.js

    Sanoma tulee näkyviin `retrieved device twin`. Pidä käynnissä sovellus.

## <a name="create-the-service-app"></a>Service-sovelluksen luominen

Tässä osiossa luot Node.js console-sovellus, joka päivittää liittyvän **myDeviceId** uuden telemetriatietojen määritysten objektin kanssa sekä *haluamasi ominaisuudet* . Kyselyt tallennettu toiminnossa laitteen twins jälkeen ja näyttää haluamasi ja raportoidun-määrityksiä laitteen erotuksen.

1. Luo uusi tyhjä kansio nimeltä **setdesiredandqueryapp**. **Setdesiredandqueryapp** -kansioon Luo uusi package.json tiedosto käyttämällä komentokehotteeseen seuraava komento. Hyväksy kaikki oletusarvot:

    ```
    npm init
    ```

2. Komentoriville oman-varten suorittamalla seuraavan komennon Asenna **azure iothub** paketti **setdesiredandqueryapp** -kansiossa:

    ```
    npm install azure-iothub@dtpreview node-uuid --save
    ```

3. Uusi **SetDesiredAndQuery.js** -tiedostoa tekstieditorissa, luo **addtagsandqueryapp** -kansioon.

4. Lisää seuraava koodi **SetDesiredAndQuery.js** -tiedostoon ja korvata **{palvelun yhteysmerkkijonon}** -paikkamerkki, jossa on kopioitu Oman keskittimeen luotaessa yhteysmerkkijonon:

        'use strict';
        var iothub = require('azure-iothub');
        var uuid = require('node-uuid');
        var connectionString = '{service connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);
         
        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var newConfigId = uuid.v4();
                var newFrequency = process.argv[2] || "5m";
                var patch = {
                    properties: {
                        desired: {
                            telemetryConfig: {
                                configId: newConfigId,
                                sendFrequency: newFrequency
                            }
                        }
                    }
                }
                twin.update(patch, function(err) {
                    if (err) {
                        console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                    } else {
                        console.log(twin.deviceId + ' twin updated successfully');
                    }
                });
                setInterval(queryTwins, 10000);
            }
        });
            

    **Rekisterin** objekti sisältää kaikki menetelmät, pakollinen vuorovaikutuksessa laitteen twins-palvelusta. Edellinen koodi sen jälkeen, kun se alustaa **rekisterin** kohdetta, hakee **myDeviceId**varten sekä, ja päivittää uuden telemetriatietojen määritysten objektin sen haluamasi ominaisuudet. Tämän jälkeen kutsuu **queryTwins** funktion tapahtuman 10 sekuntia.

    > [AZURE.IMPORTANT] Tämän sovelluksen kyselyn IoT keskittimeen 10 sekunnin välein epäsuotuisista tarkoituksiin. Kyselyitä käyttää useissa laitteissa usean käyttäjän osoittava raporttien luomiseen, mutta ei tunnista muutokset toiseen. Jos ratkaisu edellyttää reaaliaikaiset ilmoitukset laitteen tapahtumien käyttää [laitteen cloud viestien][lnk-d2c].

5. Lisää seuraavat koodin oikealle, ennen kuin `registry.getDeviceTwin()` toteuttamisesta **queryTwins** -toiminnon käynnistäminen:

        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log();
                    results.forEach(function(twin) {
                        var desiredConfig = twin.properties.desired.telemetryConfig;
                        var reportedConfig = twin.properties.reported.telemetryConfig;
                        console.log("Config report for: " + twin.deviceId);
                        console.log("Desired: ");
                        console.log(JSON.stringify(desiredConfig, null, 2));
                        console.log("Reported: ");
                        console.log(JSON.stringify(reportedConfig, null, 2));
                    });
                }
            });
        };

    Edellisen koodin kyselyt twins tallennettu toiminnossa ja tulostaa haluamasi ja raportoidun telemetriatietojen-määrityksiä. [IoT keskittimeen kyselykielen] viitata[ lnk-query] esitellään, miten voit luoda monipuolisia raportteja kaikissa kaikissa laitteissasi.


8. Kun **SimulateDeviceConfiguration.js** on käytössä, suorita sovellukseen:

        node SetDesiredAndQuery.js 5m

    Raportissa pitäisi näkyä ilmoitetaan määritysten muutosta **Success** **odotetaan** **menestyksen** salaisuus uudelleen käyttämällä uuden aktiivisen Lähetä korkojakso viisi minuuttia sen sijaan, että 24 tuntia.

    > [AZURE.IMPORTANT] Ei viiveen ylöspäin minuuteiksi laitteen raportti-toimintoa ja kyselyn tuloksen välillä. Tämä on erittäin suuri asteikko työskentelemään kysely-infrastruktuurin käyttöön. Hakea yksittäisen sekä yhdenmukaisia näkymät-menetelmällä **getDeviceTwin** **rekisterin** luokan.

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä opetusohjelmassa Määritä uudet määritykset *haluamasi* ominaisuuksina taustatietokantaan-sovelluksesta ja kirjoittamasi Simuloitu laite-sovellus tunnistaa, muuttaminen ja simuloida sen tiloja raportoinnin kuin *ilmoitettu ominaisuudet* sekä monivaiheinen Päivitysprosessista.

Seuraavien resurssien avulla opit miten:

- Lähetä telemetriatietojen käytetyssä laitteessa on [aloittaminen IoT keskittimeen] [ lnk-iothub-getstarted] opetusohjelmassa
- ajoittaa tai suorittaa toimintoja suurten tietojoukkojen laitteet-kohdassa [Käytä töiden ajoittaminen ja lähettäminen laitteeseen toimintojen] [ lnk-schedule-jobs] opetusohjelma.
- ohjata laitteita vuorovaikutteisesti (kuten ottaminen käyttöön tuuletin käyttäjän hallita sovelluksen), [Käytä suoraan menetelmiä] [ lnk-methods-tutorial] opetusohjelma.


<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: iot-hub-device-management-overview.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-schedule-jobs.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-c2d-methods.md

[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

[lnk-how-to-configure-createapp]: iot-hub-node-node-twin-how-to-configure.md#create-the-simulated-device-app
