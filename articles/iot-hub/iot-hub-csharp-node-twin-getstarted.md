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

Tässä opetusohjelmassa lopussa on .NET ja Node.js console-sovelluksen:

* **AddTagsAndQuery.sln**, .NET-sovellus voidaan suorittaa taustatietokantaan, joka lisää tunnisteet ja kyselyt laitteen twins tarkoitus.
* **TwinSimulatedDevice.js**, Node.js-sovellus, joka varmistaminen laitteella, joka muodostaa yhteyden IoT keskittimeen aiemmin luotu laitteen käyttäjätietoja ja yhteyden kunto raportoi.

> [AZURE.NOTE] Artikkelin [IoT keskittimeen SDK: T] [ lnk-hub-sdks] on tietoja eri SDK: T, että voit luoda laitteen ja taustatietokantaan sovelluksia.

Tässä opetusohjelmassa suorittamiseen tarvitset seuraavat:

+ Microsoft Visual Studio 2015.

+ Node.js versio 0.10.x tai uudempi versio.

+ Azure active tili. (Jos sinulla ei ole tiliä, voit luoda [ilmaisen tilin] [ lnk-free-trial] -muutamaan minuuttiin.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-the-service-app"></a>Service-sovelluksen luominen

Tässä osassa Node.js console-sovelluksen, joka lisää sijainti-metatiedot **myDeviceId**liittyvät sekä luominen. Sen jälkeen kyselyjä, jotka on tallennettu valitsemalla laitteet toiminnossa twins, joka sijaitsee Yhdysvalloissa ja valitse sitten niistä, raportointi matkapuhelinverkon kautta.

1. Visual Studiossa lisätä Visual C# Windows perinteinen työpöytä projektin nykyistä ratkaisua käyttämällä **Konsolisovelluksen** projektimalli. Projektin **AddTagsAndQuery**nimi.

    ![Uusi Visual C# Windows perinteinen työpöytä-projekti][img-createapp]

2. Napsauta ratkaisunhallinnassa **AddTagsAndQuery** projektin hiiren kakkospainikkeella ja valitse sitten **Nuget pakettien hallinta**.

3. **Nuget pakettien hallinta** -ikkunassa, varmista, että **Sisällytä prerelease** on valittuna, Etsi **microsoft.azure.devices**, valitse Asenna **asentaa** *, **Microsoft.Azure.Devices** ennakkoversion* pakkaaminen ja hyväksy käyttöehdot. Tämä toiminto lataa, asentaa ja lisää viittauksen [Microsoft Azure IoT palvelun SDK] [ lnk-nuget-service-sdk] Nuget paketin ja riippuvaa.

    ![Nuget pakettien hallinta-ikkuna][img-servicenuget]

4. Lisää seuraava `using` lauseet **Program.cs** tiedoston yläosassa:

        using Microsoft.Azure.Devices;

5. Lisää seuraavat kentät **ohjelma** -luokka. Korvaa paikkamerkinarvo, jonka loit edellisessä osassa IoT-toiminnossa yhteysmerkkijono.

        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";

6. Lisää **ohjelma** luokan seuraavalla tavalla:

        public static async Task AddTagsAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch =
                @"{
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                        }
                    }
                }";
            await registryManager.UpdateTwinAsync(twin.DeviceId, patch, twin.ETag);

            var query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            var twinsInRedmond43 = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43: {0}", string.Join(", ", twinsInRedmond43.Select(t => t.DeviceId)));

            query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            var twinsInRedmond43UsingCellular = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43 using cellular network: {0}", string.Join(", ", twinsInRedmond43UsingCellular.Select(t => t.DeviceId)));
        }

    **RegistryManager** luokan sisältää kaikki menetelmät, pakollinen vuorovaikutuksessa laitteen twins-palvelusta. Edellinen koodi alustaa ensin **registryManager** kohdetta, ja valitse hakee **myDeviceId**varten sekä ja päivittää sen tunnisteet lopuksi alatunnisteessa tiedoilla.

    Kun olet päivittänyt, se suorittaa kyselyt: ensimmäisen valitsee vain laitteen twins **Redmond43** tehdas niin sijaitsevat laitteiden ja toinen parantaa siirtymästä kysely ja valitse vain matkapuhelinverkon verkon kautta myös liitettyjen laitteiden.

    Huomautus Edellinen koodi, kun se luo **kysely** -kohdetta, määrittää enimmäismäärä palautetut tiedostot. **Kyselyn** objekti sisältää **HasMoreResults** totuusarvo ominaisuutta, joiden avulla voit käynnistää **GetNextAsTwinAsync** menetelmiä useita kertoja noutaa kaikki tulokset. Menetelmän **GetNextAsJson** on käytettävissä, jotka eivät ole laitteen twins esimerkiksi kooste kyselyjen tulokset tulokset.

7. Lisää seuraavat rivit- **Main** -menetelmää:

        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddTagsAndQuery().Wait();
        Console.WriteLine("Press Enter to exit.");
        Console.ReadLine();

8. Tämän sovelluksen käyttämiseen, jolloin näkyviin tulee pyytävä kaikissa laitteissa, jotka sijaitsevat **Redmond43** ja ei mitään, joka rajoittaa tulokset matkapuhelinverkon verkon käyttävät laitteet kyselyn kyselyn tulokset yhteen laitteeseen.

    ![Kyselytulokset-ikkunassa][img-addtagapp]

Seuraavassa osassa voit luoda laite-sovellusta, joka ilmoittaa yhteyden tiedot ja edellisessä osassa kyselyn tulos.

## <a name="create-the-device-app"></a>Laite-sovelluksen luominen

Tässä osassa voit luoda Node.js, console-sovellusta, joka muodostaa yhteyden oman keskittimeen **myDeviceId**ja päivittää sen sekä raportoitu tietoa, että se on liitetty matkapuhelinverkon verkon ominaisuudet.

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

    **Asiakas** -objekti sisältää kaikki menetelmät, tarvitset Käsittele laitteesta twins laitteen kanssa. Edellinen koodi sen jälkeen, kun se alustaa **asiakas** -objektin hakee **myDeviceId** varten sekä ja päivittää sen raportoidun ominaisuuden yhteyden tiedot.

5. Suorita laite-sovellus

        node ReportConnectivity.js

    Sanoma tulee näkyviin `twin state reported`.

6. Nyt, kun laite raportoidun yhteyden sen tiedot, se pitäisi näkyä sekä kyselyt. Suorita.NET **AddTagsAndQuery** sovellus suorittaa muutoskyselyn uudelleen. Tämän ajan **myDeviceId** pitäisi näkyä sekä kyselyn tuloksiin.

    ![][img-addtagapp2]

## <a name="next-steps"></a>Seuraavat vaiheet
Tässä opetusohjelmassa määritetty keskittimeen IoT Azure-portaalissa, ja luo laitteen identity toiminnossa tunnistetietojen rekisterissä. Laitteen metatiedot lisättyjä tunnisteita taustatietokantaan-sovelluksesta ja kirjoittamasi Simuloitu laite-sovelluksen raportin laitteen yhteyden tiedot-laitteen sekä. Opit, myös nämä tiedot käyttämällä IoT-toiminnossa SQL kaltaisessa kyselykielen kyselyjen tekeminen.

Seuraavien resurssien avulla opit miten:

- Lähetä telemetriatietojen käytetyssä laitteessa on [aloittaminen IoT keskittimeen] [ lnk-iothub-getstarted] opetusohjelmassa
- Määritä laitteiden avulla sekä käyttäjän haluamasi ominaisuudet [haluamasi ominaisuuksien määrittäminen laitteet] [ lnk-twin-how-to-configure] opetusohjelmassa
- laitteiden vuorovaikutteisesti (kuten ottaminen tuuletin käyttäjän hallita sovelluksen) hallinta [käyttää suoraan menetelmiä] [ lnk-methods-tutorial] opetusohjelma.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-twin-getstarted/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-twin-getstarted/createnetapp.png
[img-addtagapp]: media/iot-hub-csharp-node-twin-getstarted/addtagapp.png
[img-addtagapp2]: media/iot-hub-csharp-node-twin-getstarted/addtagapp2.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/1.1.0-preview-004

[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-c2d-methods.md
[lnk-twin-how-to-configure]: iot-hub-csharp-node-twin-how-to-configure.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

