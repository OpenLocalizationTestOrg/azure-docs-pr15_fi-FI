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
* **SetDesiredConfigurationAndQuery**, tarkoitus voidaan suorittaa taustatietokantaan, joka määrittää uudet määritykset laitteen ja kyselyjen päivityksen määritysprosessia .NET console-sovellus.

> [AZURE.NOTE] Artikkelin [IoT keskittimeen SDK: T] [ lnk-hub-sdks] on tietoja eri SDK: T, että voit luoda laitteen ja taustatietokantaan sovelluksia.

Tässä opetusohjelmassa suorittamiseen tarvitset seuraavat:

+ Microsoft Visual Studio 2015.

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

    **Asiakas** -objekti sisältää kaikki tarvittavat laitteen twins laitteesta vuorovaikutuksessa menetelmät. Edellinen koodi sen jälkeen, kun se alustaa **asiakas** -objektin hakee sekä **myDeviceId**varten, ja liittää käsittelytoiminto päivityksen haluamasi ominaisuudet. Käsittelijä varmistaa, että on todellinen määritys-muutospyyntö vertaamalla configIds ja sitten käynnistää menetelmän, joka alkaa määritysten muutos.

    Huomaa, että yksinkertaisuuden, Edellinen koodi käyttää oletusarvoinen koodattu alkuperäiset määritykset. Todellinen app todennäköisesti ladata, määritys paikallisesta tallennussijainnista.
    
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

    Huomaa, joka tallentaa kaistanleveys, ilmoitettu ominaisuudet päivitetään määrittämällä vain muokataan (nimetty **korjaustiedoston** yllä koodissa), koko asiakirjaan korvaamatta ominaisuudet.

    > [AZURE.NOTE] Tässä opetusohjelmassa simuloida ei mitään toiminta samanaikainen määritysten päivitykset. Jotkin määritys päivityksen prosessit ehkä sopimaan muutosten kohde-määritys, kun päivitys on käynnissä ja muiden käyttäjien on ehkä jonon ne muiden saattaa hylätä ne virhetilassa. Varmista, että haluamasi toiminnan muuttaminen tietyn määritysprosessia huomioitavia ja lisää haluamasi logiikan ennen aloitetaan määritysten muutos.

6. Suorita laite-sovellus:

        node SimulateDeviceConfiguration.js

    Sanoma tulee näkyviin `retrieved device twin`. Pidä käynnissä sovellus.

## <a name="create-the-service-app"></a>Service-sovelluksen luominen

Tässä osiossa luot .NET console-sovellus, joka päivittää liittyvän **myDeviceId** uuden telemetriatietojen määritysten objektin kanssa sekä *haluamasi ominaisuudet* . Kyselyt tallennettu toiminnossa laitteen twins jälkeen ja näyttää haluamasi ja raportoidun-määrityksiä laitteen erotuksen.

1. Visual Studiossa lisätä Visual C# Windows perinteinen työpöytä projektin nykyistä ratkaisua käyttämällä **Konsolisovelluksen** projektimalli. Projektin **SetDesiredConfigurationAndQuery**nimi.

    ![Uusi Visual C# Windows perinteinen työpöytä-projekti][img-createapp]

2. Napsauta ratkaisunhallinnassa **SetDesiredConfigurationAndQuery** projektin hiiren kakkospainikkeella ja valitse sitten **Nuget pakettien hallinta**.

3. **Nuget pakettien hallinta** -ikkunassa, varmista, että **Sisällytä prerelease** on valittuna, Etsi **microsoft.azure.devices**, valitse Asenna **asentaa** *, **Microsoft.Azure.Devices** ennakkoversion* pakkaaminen ja hyväksy käyttöehdot. Tämä toiminto lataa, asentaa ja lisää viittauksen [Microsoft Azure IoT palvelun SDK] [ lnk-nuget-service-sdk] Nuget paketin ja riippuvaa.

    ![Nuget pakettien hallinta-ikkuna][img-servicenuget]

4. Lisää seuraava `using` lauseet **Program.cs** tiedoston yläosassa:

        using Microsoft.Azure.Devices;
        using System.Threading;
        using Newtonsoft.Json;

5. Lisää seuraavat kentät **ohjelma** -luokka. Korvaa paikkamerkinarvo, jonka loit edellisessä osassa IoT-toiminnossa yhteysmerkkijono.

        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";

6. Lisää **ohjelma** luokan seuraavalla tavalla:

        static private async Task SetDesiredConfigurationAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch = new {
                    properties = new {
                        desired = new {
                            telemetryConfig = new {
                                configId = Guid.NewGuid().ToString(),
                                sendFrequency = "5m"
                            }
                        }
                    }
                };
            
            await registryManager.UpdateTwinAsync(twin.DeviceId, JsonConvert.SerializeObject(patch), twin.ETag);
            Console.WriteLine("Updated desired state");

            while (true)
            {
                var query = registryManager.CreateQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'");
                var results = await query.GetNextAsTwinAsync();
                foreach (var result in results)
                {
                    Console.WriteLine("Config report for: {0}", result.DeviceId);
                    Console.WriteLine("Desired telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Desired["telemetryConfig"], Formatting.Indented));
                    Console.WriteLine("Reported telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Reported["telemetryConfig"], Formatting.Indented));
                    Console.WriteLine();
                }
                Thread.Sleep(10000);
            }
        }

    **Rekisterin** objekti sisältää kaikki menetelmät, pakollinen vuorovaikutuksessa laitteen twins-palvelusta. Edellinen koodi sen jälkeen, kun se alustaa **rekisterin** kohdetta, hakee **myDeviceId**varten sekä, ja päivittää uuden telemetriatietojen määritysten objektin sen haluamasi ominaisuudet.
    Tämän jälkeen 10 sekunnin välein sen tallennettu toiminnossa twins kyselyt ja tulostaa haluamasi ja raportoidun telemetriatietojen-määrityksiä. [IoT keskittimeen kyselykielen] viitata[ lnk-query] esitellään, miten voit luoda monipuolisia raportteja kaikissa kaikissa laitteissasi.

    > [AZURE.IMPORTANT] Tämän sovelluksen kyselyn IoT keskittimeen 10 sekunnin välein epäsuotuisista tarkoituksiin. Kyselyitä käyttää useissa laitteissa usean käyttäjän osoittava raporttien luomiseen, mutta ei tunnista muutokset toiseen. Jos ratkaisu edellyttää reaaliaikaiset ilmoitukset laitteen tapahtumien käyttää [laitteen cloud viestien][lnk-d2c].

7. Lisää seuraavat rivit- **Main** -menetelmää:

        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        SetDesiredConfigurationAndQuery();
        Console.WriteLine("Press any key to quit.");
        Console.ReadLine();

8. Kun **SimulateDeviceConfiguration.js** on käytössä, suorita käyttämällä **F5-näppäintä** , ja Visual Studio .NET-sovelluksen pitäisi näkyä muuttaa **onnistui** - **odotetaan** **menestyksen** salaisuus uudelleen ja uuden aktiivisen raportoidun määritysten Lähetä korkojakso viisi minuuttia sen sijaan, että 24 tuntia.

    > [AZURE.IMPORTANT] Ei viiveen ylöspäin minuuteiksi laitteen raportti-toimintoa ja kyselyn tuloksen välillä. Tämä on erittäin suuri asteikko työskentelemään kysely-infrastruktuurin käyttöön. Hakea yksittäisen sekä yhdenmukaisia näkymät-menetelmällä **getDeviceTwin** **rekisterin** luokan.

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä opetusohjelmassa uudet määritykset määrittäminen uudelleen nimellä *haluamasi ominaisuudet* ja kirjoittamasi laite-sovellus tunnistaa muutoksen ja simuloida sen tiloja raportoinnin *raportoidun* ominaisuuksina sekä monivaiheinen Päivitysprosessista.

Seuraavien resurssien avulla opit miten:

- Lähetä telemetriatietojen käytetyssä laitteessa on [aloittaminen IoT keskittimeen] [ lnk-iothub-getstarted] opetusohjelmassa
- ajoittaa tai suorittaa toimintoja suurten tietojoukkojen laitteet-kohdassa [Käytä töiden ajoittaminen ja lähettäminen laitteeseen toimintojen] [ lnk-schedule-jobs] opetusohjelma.
- ohjata laitteita vuorovaikutteisesti (kuten ottaminen käyttöön tuuletin käyttäjän hallita sovelluksen), [Käytä suoraan menetelmiä] [ lnk-methods-tutorial] opetusohjelma.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-twin-getstarted/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-twin-getstarted/createnetapp.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/1.1.0-preview-004

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
