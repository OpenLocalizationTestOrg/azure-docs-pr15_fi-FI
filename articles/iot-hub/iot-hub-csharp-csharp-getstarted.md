<properties
    pageTitle="Azure IoT-sivusto C#-aloittaminen | Microsoft Azure"
    description="Azure IoT-toiminnosta ja C# käytön aloittaminen opetusohjelma. Toteuta asioita Internet-ratkaisun avulla Azure IoT keskittimeen ja C# Microsoft Azure IoT SDK: T."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/12/2016"
     ms.author="dobett"/>

# <a name="get-started-with-azure-iot-hub-for-net"></a>Aloittaminen Azure IoT keskittimeen .NET

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Tässä opetusohjelmassa lopussa on kolme Windows console-sovelluksissa:

* **CreateDeviceIdentity**, joka luo laitteen käyttäjätieto- ja liittyvät suojaustunnuksen yhteyden muodostamiseksi Simuloitu laitteen.
* **ReadDeviceToCloudMessages**, joka näyttää Simuloitu laitteen lähettämä telemetriatietojen.
* **SimulatedDevice**, joka muodostaa yhteyden IoT keskittimeen aiemmin luotu laitteen käyttäjätietoja ja lähettää viestin telemetriatietojen sekunnin välein AMQP-protokollan avulla.

> [AZURE.NOTE] Tietoja eri SDK: T, joiden avulla voit luoda molemmat sovellukset toimimaan laitteet ja ratkaisu takaisin päässä artikkelissa [IoT keskittimeen SDK: T][lnk-hub-sdks].

Tässä opetusohjelmassa suorittamiseen tarvitset seuraavat:

+ Microsoft Visual Studio 2015.

+ Azure active tili. (Jos sinulla ei ole tiliä, voit luoda [ilmaisen tilin] [ lnk-free-trial] -muutamaan minuuttiin.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Olet nyt luonut IoT-toiminnosta ja sinulla on isäntänimi ja yhteysmerkkijonon, sinun on suoritettava jäljempänä tässä opetusohjelmassa.

## <a name="create-a-device-identity"></a>Laitteen käyttäjätietojen luominen

Tässä osassa voit luoda Windows console-sovellus, joka luo laitteen tunnistetietojen IoT-toiminnossa identity-rekisterissä. Laite ei voi muodostaa yhteyttä IoT keskittimeen, paitsi jos siinä on merkintä laitteen tunnistetietojen rekisterissä. Lisätietoja on kohdassa "Laitteen tunnistetietojen rekisterin" [IoT keskittimeen Sovelluskehittäjän opas][lnk-devguide-identity]. Kun suoritat tämä konsolisovellus, se luo laitteen yksilöivä tunnus ja -laitteen avulla voit tunnistaa itse, kun se lähettää laitteen cloud viestien IoT keskittimeen avaimen.

1. Visual Studiossa lisätä Visual C# Windows perinteinen työpöytä projektin nykyistä ratkaisua käyttämällä **Konsolisovelluksen** projektimalli. Varmista, että .NET Framework-versio on 4.5.1 tai uudempi. Projektin **CreateDeviceIdentity**nimi.

    ![Uusi Visual C# Windows perinteinen työpöytä-projekti][10]

2. Napsauta ratkaisunhallinnassa **CreateDeviceIdentity** projektin hiiren kakkospainikkeella ja valitse sitten **Nuget pakettien hallinta**.

3. **Nuget pakettien hallinta** -ikkunassa valitsemalla **Selaa**, Etsi **microsoft.azure.devices**, valitse Asenna **Microsoft.Azure.Devices** paketti **asentaa** ja hyväksy käyttöehdot. Tämä toiminto lataa, asentaa ja lisää viittauksen [Microsoft Azure IoT palvelun SDK] [ lnk-nuget-service-sdk] Nuget paketin ja riippuvaa.

    ![Nuget pakettien hallinta-ikkuna][11]

4. Lisää seuraava `using` lauseet **Program.cs** tiedoston yläosassa:

        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Common.Exceptions;

5. Lisää seuraavat kentät **ohjelma** -luokka. Korvaa paikkamerkinarvo, jonka loit edellisessä osassa IoT-toiminnossa yhteysmerkkijono.

        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";

6. Lisää **ohjelma** luokan seuraavalla tavalla:

        private static async Task AddDeviceAsync()
        {
            string deviceId = "myFirstDevice";
            Device device;
            try
            {
                device = await registryManager.AddDeviceAsync(new Device(deviceId));
            }
            catch (DeviceAlreadyExistsException)
            {
                device = await registryManager.GetDeviceAsync(deviceId);
            }
            Console.WriteLine("Generated device key: {0}", device.Authentication.SymmetricKey.PrimaryKey);
        }

    Tämä menetelmä luo laitteen tunnistetietojen tunnus **myFirstDevice**. (Jos laitteen tunnus on jo rekisterissä, koodi yksinkertaisesti hakee olemassa olevan laitteen tiedot.) Sovellus näyttää käyttäjätietojen perusavain. Muodostat yhteyden IoT keskittimeen avaimeen Simuloitu-laitteessa.

7. Lisää seuraavat rivit- **Main** -menetelmää:

        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddDeviceAsync().Wait();
        Console.ReadLine();

8. Tämän sovelluksen käyttämiseen ja huomioi laite-näppäintä.

    ![Laitteen avainta sovelluksen luoman][12]

> [AZURE.NOTE] IoT keskittimeen tunnistetietojen rekisterin tallentaa vain laitteen käyttäjätietojen käyttöön valitsemalla suojattu käyttö. Tallennetaan laitteen tunnukset ja näppäimet, jotta voit käyttää suojausvaltuudet ja käytössä/poissa käytöstä-merkinnän, voit poistaa käytöstä yksittäisiä laitteen. Jos sovellus täytyy tallentaa laitekohtaiset muita metatietoja, se kannattaa käyttää sovelluksen kielikohtaiset-kaupan. Lisätietoja on artikkelissa [IoT keskittimeen Sovelluskehittäjän opas][lnk-devguide-identity].

## <a name="receive-device-to-cloud-messages"></a>Laitteen cloud virhesanomia

Tässä osassa voit luoda Windows console-sovellus, jossa lukee laitteen cloud viestien IoT-toiminnosta. IoT-toiminnossa paljastaa [Azure tapahtuman keskittimet][lnk-event-hubs-overview]-yhteensopiva päätepisteen, jotta voit laitteen cloud viestien lukeminen. Pidä asiat yksinkertainen Tässä opetusohjelmassa Luo basic lukuohjelmaa, joka ei sovi hyvin siirtonopeuden käyttöönottoa varten. Voit käsitellä laitteen cloud viestit tasolla on artikkelissa [prosessin laitteen cloud viestien] [ lnk-process-d2c-tutorial] opetusohjelma. Lisätietoja siitä, miten prosessi viesteihin-tapahtuman keskittimet, katso [Aloittaminen tapahtuman keskittimet] [ lnk-eventhubs-tutorial] opetusohjelma. (Tässä opetusohjelmassa on käytettävissä IoT keskittimeen tapahtuman keskittimeen yhteensopivan päätepisteet).

> [AZURE.NOTE] Tapahtuman keskittimeen yhteensopivan päätepisteen laitteen cloud viestien lukeminen aina käyttää AMQP-protokollaa.

1. Visual Studiossa lisätä Visual C# Windows perinteinen työpöytä projektin nykyistä ratkaisua käyttämällä **Konsolisovelluksen** projektimalli. Varmista, että .NET Framework-versio on 4.5.1 tai uudempi. Projektin **ReadDeviceToCloudMessages**nimi.

    ![Uusi Visual C# Windows perinteinen työpöytä-projekti][10]

2. Napsauta ratkaisunhallinnassa **ReadDeviceToCloudMessages** projektin hiiren kakkospainikkeella ja valitse sitten **Nuget pakettien hallinta**.

3. **Nuget pakettien hallinta** -ikkunassa Hae **WindowsAzure.ServiceBus**, valitse **Asenna**ja hyväksy käyttöehdot. Tämä toiminto lataa, asentaa ja lisää viittauksen [Azure palvelun Bus][lnk-servicebus-nuget], kaikki sen riippuvuussuhteessa. Tämän paketin ottaa käyttöön sovelluksen muodostaa IoT-toiminnossa tapahtumaa-toiminnossa yhteensopivan päätepiste.

4. Lisää seuraava `using` lauseet **Program.cs** tiedoston yläosassa:

        using Microsoft.ServiceBus.Messaging;
        using System.Threading;

5. Lisää seuraavat kentät **ohjelma** -luokka. Korvaa "Luo IoT-toiminnossa"-osassa luomasi IoT-toiminnossa yhteysmerkkijono paikkamerkki-arvon.

        static string connectionString = "{iothub connection string}";
        static string iotHubD2cEndpoint = "messages/events";
        static EventHubClient eventHubClient;

6. Lisää **ohjelma** luokan seuraavalla tavalla:

        private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct)
        {
            var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow);
            while (true)
            {
                if (ct.IsCancellationRequested) break;
                EventData eventData = await eventHubReceiver.ReceiveAsync();
                if (eventData == null) continue;

                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data);
            }
        }

    Tässä menetelmässä **EventHubReceiver** esiintymä vastaanottaa viestit pilvestä kaikki IoT keskittimeen laite-ja-vastaanottaa osiot. Ilmoitus siitä, miten voit välittää `DateTime.Now` parametri, kun luot **EventHubReceiver** kohdetta, niin, että se saa vain sen jälkeen, kun se käynnistyy lähetetyt viestit. Tämä suodatin on hyötyä testiympäristössä niin, että käytössä olevista viestejä. Tuotantoympäristössä koodin Varmista, että käsittelee kaikki viestit. Lisätietoja on artikkelissa [IoT keskittimeen laitteen cloud viestien käsitteleminen] [ lnk-process-d2c-tutorial] opetusohjelma.

7. Lisää seuraavat rivit- **Main** -menetelmää:

        Console.WriteLine("Receive messages. Ctrl-C to exit.\n");
        eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, iotHubD2cEndpoint);

        var d2cPartitions = eventHubClient.GetRuntimeInformation().PartitionIds;

        CancellationTokenSource cts = new CancellationTokenSource();

        System.Console.CancelKeyPress += (s, e) =>
        {
          e.Cancel = true;
          cts.Cancel();
          Console.WriteLine("Exiting...");
        };

        var tasks = new List<Task>();
        foreach (string partition in d2cPartitions)
        {
           tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token));
        }  
        Task.WaitAll(tasks.ToArray());

## <a name="create-a-simulated-device-app"></a>Simuloitu laitteen sovelluksen luominen

Tässä osassa Windows console sovelluksen laitteella, joka lähettää laitteen cloud IoT keskittimeen tulokset vastaavat luominen.

1. Visual Studiossa lisätä Visual C# Windows perinteinen työpöytä projektin nykyistä ratkaisua käyttämällä **Konsolisovelluksen** projektimalli. Varmista, että .NET Framework-versio on 4.5.1 tai uudempi. Projektin **SimulatedDevice**nimi.

    ![Uusi Visual C# Windows perinteinen työpöytä-projekti][10]

2. Napsauta ratkaisunhallinnassa **SimulatedDevice** projektin hiiren kakkospainikkeella ja valitse sitten **Nuget pakettien hallinta**.

3. **Nuget pakettien hallinta** -ikkunassa valitsemalla **Selaa**, Etsi **Microsoft.Azure.Devices.Client**, valitse Asenna **Microsoft.Azure.Devices.Client** paketti **asentaa** ja hyväksy käyttöehdot. Tämä toiminto lataa, asentaa ja lisää viittauksen [Azure IoT - laitteen SDK Nuget paketti] [ lnk-device-nuget] ja sen riippuvuudet.

4. Lisää seuraava `using` **Program.cs** tiedoston yläosassa lause:

        using Microsoft.Azure.Devices.Client;
        using Newtonsoft.Json;

5. Lisää seuraavat kentät **ohjelma** -luokka. Korvaa "Luominen IoT-toiminnossa"-osassa noudettu IoT keskittimeen isäntänimi paikkamerkin arvot ja laitteen avainta, jotka on haettu "Laitteen käyttäjätietojen luominen"-osassa.

        static DeviceClient deviceClient;
        static string iotHubUri = "{iot hub hostname}";
        static string deviceKey = "{device key}";

6. Lisää **ohjelma** luokan seuraavalla tavalla:

        private static async void SendDeviceToCloudMessagesAsync()
        {
            double avgWindSpeed = 10; // m/s
            Random rand = new Random();

            while (true)
            {
                double currentWindSpeed = avgWindSpeed + rand.NextDouble() * 4 - 2;

                var telemetryDataPoint = new
                {
                    deviceId = "myFirstDevice",
                    windSpeed = currentWindSpeed
                };
                var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
                var message = new Message(Encoding.ASCII.GetBytes(messageString));

                await deviceClient.SendEventAsync(message);
                Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, messageString);

                Task.Delay(1000).Wait();
            }
        }

    Tämä menetelmä lähettää viestiin laitteen cloud sekunnin välein. Viesti sisältää JSON muuntaa sarjaksi objektin, voit simuloida tuulen nopeus tunnistimen laitteen tunnus ja satunnaisluvun kanssa.

7. Lisää seuraavat rivit- **Main** -menetelmää:

        Console.WriteLine("Simulated device\n");
        deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey("myFirstDevice", deviceKey));

        SendDeviceToCloudMessagesAsync();
        Console.ReadLine();

  **Create** -menetelmä luo oletusarvoisesti **DeviceClient** ‑esiintymä, jossa AMQP-protokollaa käytetään vaihtamaan IoT toiminnossa. Jos haluat käyttää HTTP-protokolla, käyttää **Create** -menetelmä, jonka avulla voit määrittää protokolla ohitus. Jos käytät HTTP-protokolla, **Microsoft.AspNet.WebApi.Client** Nuget paketin pitäisi myös lisääminen projektiin sisällytettävien **System.Net.Http.Formatting** nimitilan.

Tässä opetusohjelmassa kerrotaan IoT keskittimeen-laitteen asiakkaan luomisen ohjeita. Voit myös käyttää [Azure IoT keskittimeen yhdistetty palvelun] [ lnk-connected-service] Visual Studio laajennus tarvittavan koodin lisääminen laitteen asiakassovellus.


> [AZURE.NOTE] Tässä opetusohjelmassa avulla voit jatkaa samaa yksinkertainen, Toteuta uudelleen kaikki käytännöt. Tuotannon koodissa olisi Toteuta kuin ehdotettua [Lyhytkestoisia vika käsittely]MSDN-sivuston artikkelissa (kuten Eksponentiaalinen backoff), yritä käytännöt[lnk-transient-faults].

## <a name="run-the-applications"></a>Suorita sovellukset

Olet nyt valmiina sovelluksia.

1.  Visual Studiossa, napsauta ratkaisunhallinnassa-ratkaisu hiiren kakkospainikkeella ja valitse sitten **Määritä käynnistys projektit**. Valitse **useita käynnistys projekteja**ja valitse sitten kuin **ReadDeviceToCloudMessages** ja **SimulatedDevice** projektit-toiminto **Käynnistä-painiketta** .

    ![Projektin käynnistysasetukset][41]

2.  Painamalla **F5** Aloita sekä sovellukset käynnissä. Konsolin tulosteen **SimulatedDevice** sovelluksen näkyvät Simuloitu laitteen lähettää IoT-toiminnossa viestit. **ReadDeviceToCloudMessages** sovelluksen console-tulos näyttää viestit, joka vastaanottaa IoT-toiminnossa.

    ![Konsolin tuloste sovelluksista][42]

3. **Käyttö** -ruudun [Azure portal] [ lnk-portal] näkyy keskittimeen viestien lukumäärä:

    ![Azure-portaalin käytön-ruutu][43]


## <a name="next-steps"></a>Seuraavat vaiheet

Tässä opetusohjelmassa määritetty IoT-toiminnossa Azure-portaalissa, ja luo laitteen identity toiminnossa tunnistetietojen rekisterissä. Tämä laite tunnistetietojen avulla laitteen cloud viestien lähettämiseen keskittimeen Simuloitu laite-sovelluksen käyttöön. Voit luoda myös sovellus, joka näyttää keskittimeen vastaanotettujen viestien. 

Jatka IoT keskittimeen aloittaminen ja tutustu IoT muissa tilanteissa, katso:

- [Laitteen yhteyden muodostamisessa][lnk-connect-device]
- [Mobiililaitteiden hallinnan käytön aloittaminen][lnk-device-management]
- [IoT yhdyskäytävän SDK: N käytön aloittaminen][lnk-gateway-SDK]

Voit laajentaa IoT ratkaisu ja käsittele laitteen cloud viestejä tasolla on artikkelissa [käsitellä laitteen cloud viestejä] [ lnk-process-d2c-tutorial] opetusohjelma.

<!-- Images. -->
[41]: ./media/iot-hub-csharp-csharp-getstarted/run-apps1.png
[42]: ./media/iot-hub-csharp-csharp-getstarted/run-apps2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png
[10]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp1.png
[11]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp2.png
[12]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp3.png

<!-- Links -->
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-servicebus-nuget]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-device-nuget]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-connected-service]: https://visualstudiogallery.msdn.microsoft.com/e254a3a5-d72e-488e-9bd3-8fee8e0cd1d6
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
