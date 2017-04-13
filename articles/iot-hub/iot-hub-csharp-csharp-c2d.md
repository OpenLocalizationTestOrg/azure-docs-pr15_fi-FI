<properties
    pageTitle="Cloud laitteen IoT keskittimeen sisältävien viestien lähettäminen | Microsoft Azure"
    description="Katso tämä opetusohjelma lisätietoja cloud laitteen lähettämiseen käyttäminen Azure IoT keskittimeen C#."
    services="iot-hub"
    documentationCenter=".net"
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="06/23/2016"
     ms.author="elioda"/>

# <a name="tutorial-how-to-send-cloud-to-device-messages-with-iot-hub-and-net"></a>Opetusohjelma: Miten cloud laitteen IoT keskittimeen ja .net sisältävien viestien lähettäminen

[AZURE.INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Johdanto

Azure IoT-toiminnossa on täysin hallittujen palvelu, joka auttaa käyttöön luotettavan ja turvallisen kaksisuuntaisen tietoliikenteen miljoonia IoT laitteiden välillä ja sovelluksen takaisin lopettaa. [IoT keskittimeen käytön aloittaminen] -opetusohjelma näytetään, miten voit luoda IoT-toiminnosta ja valmistella laitteen tunnistetiedot, ei koodin Simuloitu laitteella, joka lähettää laitteen pilveen.

Tässä opetusohjelmassa muodostaa [IoT toiminnosta on]aloittaminen. Se näyttää, miten voit:

- Lähettää cloud laitteen viestien oman sovelluksen cloud taustatietokantaan, yhteen laitteeseen IoT keskittimeen kautta.
- Cloud laitteen virhesanomia laitteessa.
- Sovelluksen cloud takaisin loppuun, pyytää kuittausta toimituksen (*palaute*) lähetetyt laitteeseen IoT-toiminnosta.

Voit etsiä lisätietoja cloud laitteen viesteille [IoT keskittimeen Sovelluskehittäjän opas][IoT Hub Developer Guide - C2D].

Tässä opetusohjelmassa lopussa suoritetaan kaksi Windows console-sovelluksissa:

* **SimulatedDevice**, luotu [aloittaminen IoT keskittimeen], joka muodostaa yhteyden IoT-toiminnosta ja vastaanottaa cloud laite muokattu sovellusversio.
* **SendCloudToDevice**, joka lähettää cloud laitteen viestin IoT keskittimeen Simuloitu laitteesi ja vastaanottaa sen toimitus-kuittausta.

> [AZURE.NOTE] IoT toiminnosta on monta laitteen alustojen ja eri kielten (mukaan lukien C ja Javascript Java) – Azure IoT laitteen SDK: T SDK tuki. Vaiheittaiset ohjeet laitteen yhdistäminen Tässä opetusohjelmassa koodi ja yleensä Azure IoT-toiminnossa on artikkelissa [Azure IoT Developer Center].

Jotta voit suorittaa tässä opetusohjelmassa, sinun on seuraavasti:

+ Microsoft Visual Studio 2015

+ Azure active tili. (Jos sinulla ei ole tiliä, voit luoda [ilmaisen tilin] [ lnk-free-trial] -muutamaan minuuttiin.)

## <a name="receive-messages-on-the-simulated-device"></a>Vastaanottaa viestejä Simuloitu laitteeseen

Tässä osassa Muokkaa Simuloitu laite-sovellus on luotu cloud laitteen virhesanomia IoT-toiminnosta [IoT keskittimeen käytön aloittaminen] .

1. Lisätä seuraavalla tavalla Visual Studion **SimulatedDevice** Projectissa, **ohjelma** -luokka.

        private static async void ReceiveC2dAsync()
        {
            Console.WriteLine("\nReceiving cloud to device messages from service");
            while (true)
            {
                Message receivedMessage = await deviceClient.ReceiveAsync();
                if (receivedMessage == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received message: {0}", Encoding.ASCII.GetString(receivedMessage.GetBytes()));
                Console.ResetColor();

                await deviceClient.CompleteAsync(receivedMessage);
            }
        }

    `ReceiveAsync` Menetelmä palauttaa vastaanotetun viestin asynkronisesti suunnitellut aloittamispäivämäärät, jotka on vastaanottanut laite. Palauttaa *null* , kun specifiable aikakatkaisuajan (Tässä tapauksessa minuutin oletusarvo on käytössä). Tällöin koodin Jatka odottaa uusissa viesteissä. Tämä on syy `if (receivedMessage == null) continue` rivi.

    Puhelun `CompleteAsync()` IoT keskittimeen ilmoittaa, että viesti on käsitelty onnistuneesti. Viesti voidaan turvallisesti poistaa laitteen jonossa. Jos järjestelmässä on tapahtunut, joka esti laitteen sovelluksen suorittamasta viestin käsittely-toiminnossa IoT toimittaa sen uudelleen. On sitten tärkeää, että viestin käsittely logiikan laite-sovelluksessa on *idempotent*niin, että vastaanottavaan saman useita kertoja tuottaa saman tuloksen. Sovelluksen myös tilapäisesti voit hylätä viestin, joka johtaa säilyttämiseen näyttöön tulevien kulutus jonossa IoT-toiminnossa. Tai sovelluksen voit hylätä viestin, joka poistaa pysyvästi viestin jonossa. Lisätietoja cloud laitteen viestin elinkaari on [IoT keskittimeen Sovelluskehittäjän opas][IoT Hub Developer Guide - C2D].

    > [AZURE.NOTE] Kun http:n sijaan MQTT tai AMQP kuin transport, `ReceiveAsync` menetelmä palauttaa heti. Tuetut kuvio cloud laitteen viestit, joiden HTTP on ajoittain liitettyjen laitteiden, Tarkista viestit harvoin (enintään 25 minuutin välein). Lisää HTTP varmenteiden saa ilmoituksen tulokset IoT toiminnossa rajoittimen pyynnöt. Lisätietoja MQTT, AMQP ja HTTP-tuki ja IoT keskittimeen rajoittimen erot on [IoT keskittimeen Sovelluskehittäjän opas][IoT Hub Developer Guide - C2D].

2. Lisää seuraava menetelmä **Main** -tapaa oikealle ennen `Console.ReadLine()` riville:

        ReceiveC2dAsync();

> [AZURE.NOTE] Tässä opetusohjelmassa Toteuta uudelleen kaikki käytännöt yksinkertaisuuden 's sake. Tuotannon koodi olisi Toteuta kuin ehdotettua [Lyhytkestoisia vika käsittely]MSDN-sivuston artikkelissa (kuten eksponentiaalisen backoff), yritä käytännöt.

## <a name="send-a-cloud-to-device-message"></a>Cloud laitteen viestin lähettäminen

Tässä osassa kirjoitat Windows console-sovellus, joka lähettää cloud laitteen Simuloitu laite-sovellukseen.

1. Nykyisen Visual Studio-ratkaisuun Luo uusi Visual C# Desktop App projekti **Konsolisovelluksen** project-mallin avulla. Projektin **SendCloudToDevice**nimi.

    ![Uuden projektin Visual Studiossa][20]

2. Napsauta ratkaisunhallinnassa-ratkaisun hiiren kakkospainikkeella ja valitse sitten **... Ratkaisu NuGet pakettien hallinta**. 

    Tämä avaa **NuGet pakettien hallinta** -ikkunan.

3. Etsi `Microsoft Azure Devices`, valitsemalla **Asenna**ja hyväksy käyttöehdot. 

    Tämä on latautunut, asentaa ja lisää [Azure IoT - palvelun SDK NuGet paketin]viittaus.

4. Lisää seuraava `using` **Program.cs** tiedoston yläosassa lause:

        using Microsoft.Azure.Devices;

5. Lisää seuraavat kentät **ohjelma** -luokka. Korvaa paikkamerkin arvon IoT keskittimeen yhteysmerkkijonoa [IoT keskittimeen käytön aloittaminen]:

        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";

6. Lisää **ohjelma** luokan seuraavalla tavalla:

        private async static Task SendCloudToDeviceMessageAsync()
        {
            var commandMessage = new Message(Encoding.ASCII.GetBytes("Cloud to device message."));
            await serviceClient.SendAsync("myFirstDevice", commandMessage);
        }

    Tämä menetelmä lähettää viestiin cloud laitteen laitetta, jonka tunnus, `myFirstDevice`. Muuta parametriä näin ollen siltä varalta, että muokkaamasi väristä käytetään [IoT keskittimeen käytön aloittaminen].

7. Lisää seuraavat rivit- **Main** -menetelmää:

        Console.WriteLine("Send Cloud-to-Device message\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);

        Console.WriteLine("Press any key to send a C2D message.");
        Console.ReadLine();
        SendCloudToDeviceMessageAsync().Wait();
        Console.ReadLine();

8. Visual Studion ratkaisu hiiren kakkospainikkeella ja valitse **Määritä käynnistys projekteja...**. Valitse **useita käynnistys projekteja**ja **käynnistää** toiminnon **ProcessDeviceToCloudMessages**, **SimulatedDevice**ja **SendCloudToDevice**.

9.  Painamalla **F5-näppäintä**. Kaikki kolme sovellusta käynnistyy. Valitse **SendCloudToDevice** windows ja paina **Enter**-näppäintä. Pitäisi näkyä Simuloitu sovellus on vastaanottanut viestin.

    ![Sovelluksen vastaanottamisessa viesti][21]

## <a name="receive-delivery-feedback"></a>Saat toimituksen palaute
On mahdollista pyynnön toimituksen (tai vanhentumispäivä) kuittaussanomien IoT-toiminnosta cloud laitteen kuhunkin viestiin. Näin cloud takaisin päässä antavat yritä tai korvaus logiikan helposti. Lisätietoja cloud laitteen palautetta on [IoT keskittimeen Sovelluskehittäjän opas][IoT Hub Developer Guide - C2D].

Tässä osassa muokataan **SendCloudToDevice** sovelluksen Palautteen pyytäminen ja tulee IoT-toiminnosta.

1. Lisätä seuraavalla tavalla Visual Studion **SendCloudToDevice** Projectissa, **ohjelma** -luokka.
   
        private async static void ReceiveFeedbackAsync()
        {
            var feedbackReceiver = serviceClient.GetFeedbackReceiver();

            Console.WriteLine("\nReceiving c2d feedback from service");
            while (true)
            {
                var feedbackBatch = await feedbackReceiver.ReceiveAsync();
                if (feedbackBatch == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received feedback: {0}", string.Join(", ", feedbackBatch.Records.Select(f => f.StatusCode)));
                Console.ResetColor();

                await feedbackReceiver.CompleteAsync(feedbackBatch);
            }
        }

    Huomaa, että vastaanotto-kaava on sama kuin cloud laitteen virhesanomia laite-sovelluksen avulla.

2. Lisää seuraava menetelmä **Main** -tapaa heti sen jälkeen `serviceClient = ServiceClient.CreateFromConnectionString(connectionString)` riville:

        ReceiveFeedbackAsync();

3. Pyytää palautetta cloud laitteen viestin toimittamista varten on määritettävä ominaisuus **SendCloudToDeviceMessageAsync** -menetelmää. Lisää seuraava rivi, heti sen jälkeen `var commandMessage = new Message(...);` riville:

        commandMessage.Ack = DeliveryAcknowledgement.Full;

4.  Suorita sovellukset painamalla **F5-näppäintä**. Raportissa pitäisi näkyä kaikki kolme sovellusta Käynnistä-painiketta. Valitse **SendCloudToDevice** windows ja paina **Enter**-näppäintä. Pitäisi näkyä viesti on Simuloitu sovelluksessa ja muutaman sekunnin kuluttua on vastaanottanut **SendCloudToDevice** sovelluksen palautetta viestin.

    ![Sovelluksen vastaanottamisessa viesti][22]

> [AZURE.NOTE] Tässä opetusohjelmassa Toteuta uudelleen kaikki käytännöt yksinkertaisuuden 's sake. Tuotannon koodi olisi Toteuta kuin ehdotettua [Lyhytkestoisia vika käsittely]MSDN-sivuston artikkelissa (kuten eksponentiaalisen backoff), yritä käytännöt.

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä opetusohjelmassa opit, miten cloud laitteen viestien lähettämiseen ja vastaanottamiseen. 

Esimerkkejä valmis lopusta loppuun-ratkaisuja, jotka käyttävät IoT keskittimeen, on artikkelissa [Azure IoT Suite].

Saat lisätietoja kanssa IoT keskittimeen, ratkaisujen kehittämiseen on [IoT keskittimeen Sovelluskehittäjän opas].

<!-- Images -->
[20]: ./media/iot-hub-csharp-csharp-c2d/create-identity-csharp1.png
[21]: ./media/iot-hub-csharp-csharp-c2d/sendc2d1.png
[22]: ./media/iot-hub-csharp-csharp-c2d/sendc2d2.png

<!-- Links -->

[Azure IoT - palvelun SDK NuGet paketti]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[Lyhytkestoisia virheen käsittely]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[IoT Hub Developer Guide - C2D]: iot-hub-devguide-messaging.md

[IoT keskittimeen Sovelluskehittäjän opas]: iot-hub-devguide.md
[IoT keskittimeen käytön aloittaminen]: iot-hub-csharp-csharp-getstarted.md
[Azure IoT Developer Center]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/