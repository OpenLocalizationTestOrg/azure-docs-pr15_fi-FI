<properties
    pageTitle="Lataa tiedostot laitteilla käyttämällä IoT keskittimeen | Microsoft Azure"
    description="Katso tämä opetusohjelma esitellään, miten voit ladata tiedostoja käyttämällä Azure IoT keskittimeen ja C# laitteilla."
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
     ms.date="06/21/2016"
     ms.author="elioda"/>

# <a name="tutorial-how-to-upload-files-from-devices-to-the-cloud-with-iot-hub"></a>Opetusohjelma: Voit ladata tiedostoja laitteilla kanssa IoT keskittimeen pilveen

## <a name="introduction"></a>Johdanto

Azure IoT-toiminnossa on täysin hallitun palvelu, joka mahdollistaa luotettava ja suojatun kaksisuuntaisen tietoliikenteen miljoonia IoT laitteet ja sovelluksen takaisin end-näppäinyhdistelmää. Edellisen opetusohjelmat ([IoT keskittimeen käytön aloittaminen] ja [Cloud laitteen IoT keskittimeen sisältävien viestien lähettäminen]) havainnollistaa basic laite--pilvipalveluun ja cloud laitteen IoT keskittimeen ja [prosessin laitteen Cloud viestien] opetusohjelman viestinnän toimintoja kuvataan tapa laitteen cloud viestien tallentaminen luotettavasti Azure-blob-säiliö. Kuitenkin joissakin tilanteissa et voi helposti yhdistät laitteet Lähetä suhteellisen pieni laitteen cloud viesteihin, jotka IoT keskittimeen hyväksyy tiedot. Esimerkkejä tällaisista tiedostoista ovat suuria tiedostoja, jotka sisältävät kuvia, videoita, tärinä tietojen näyte on hyvin usein tai jotka sisältävät joitakin lomakkeen esikäsitelty tiedot. Nämä tiedostot ovat yleensä erä käsitteleminen työkaluilla, kuten [Azure Data Factory] tai [Hadoop] -pino pilveen. Kun laitteesta tiedostojen latauksia ensisijaiset lähettäessään tapahtumia, voit silti IoT keskittimeen suojausta ja luotettavuutta toimintoja.

Tässä opetusohjelmassa muodostaa koodin [Cloud laitteen IoT keskittimeen sisältävien viestien lähettäminen] opetusohjelmassa, kuinka IoT keskittimeen tiedoston lataamisen ominaisuuksia. Sen avulla voit antaa suojatusti laite Azure blob URI-tiedosto ja Opi käyttämään IoT keskittimeen tiedoston latauksen odotussanomat käynnistettävän käsittelyn app takaisin päässä tiedosto ladataan.

Jäljempänä tässä opetusohjelmassa suorittaa kaksi Windows console-sovelluksissa:

* **SimulatedDevice**, luotu [Cloud laitteen IoT keskittimeen sisältävien viestien lähettäminen] opetusohjelman, jossa Lataa tiedosto tallennustilan käyttämällä SAS-URI myöntämä IoT-toiminnossa muokattu sovellusversio.
* **ReadFileUploadNotification**, joka vastaanottaa tiedoston latauksen odotussanomat IoT-toiminnosta.

> [AZURE.NOTE] IoT toiminto tukee useita laitteen alustojen ja eri kielten (mukaan lukien C ja Javascript Java) kautta laitteen Azure IoT SDK: T. Lisätietoja siitä, miten voit liittää laitteesi Tässä opetusohjelmassa esitetyn koodin ja yleensä Azure IoT keskittimeen ohjeiden [Azure IoT Developer Center] .

Jotta voit suorittaa tässä opetusohjelmassa, sinun on seuraavasti:

+ Microsoft Visual Studio 2015,

+ Azure active tili. (Jos sinulla ei ole tiliä, voit luoda [ilmaisen tilin] [ lnk-free-trial] -muutamaan minuuttiin.)

## <a name="associate-an-azure-storage-account-to-iot-hub"></a>Liitä Azure-tallennustilan tilin IoT keskittimeen

Koska Simuloitu laitteen Lataa tiedosto blob, sinulla on liitetty IoT keskittimeen [Azure-tallennustilan] tilin. Kun liität Azure-tallennustilan tilin IoT-keskittimeen, valitsemalla voi luoda SAS URI, laitteen avulla voit ladata tiedoston suojatusti blob-säilö. IoT keskittimeen-palveluun ja laitteen SDK: T yhteen prosessi, joka luo SAS URI ja työnkulkuna laitteeseen, jotta voit ladata tiedoston.

Noudata [Configure tiedostojen latauksia Azure-portaalissa] [ lnk-configure-upload] yhdistää Azure-tallennustilan tilin IoT-toiminnossa.

## <a name="upload-a-file-from-a-simulated-device"></a>Simuloitu laitteesta tiedoston lataaminen

Tässä osassa voit muokata Simuloitu laite-sovelluksen [Cloud laitteen IoT keskittimeen sisältävien viestien lähettäminen] cloud laitteen virhesanomia IoT-toiminnosta on luotu.

1. Visual Studiossa **SimulatedDevice** projektin hiiren kakkospainikkeella, valitse **Lisää**ja valitse sitten **Olemassa olevan kohteen**. Siirry kuvatiedostoa ja Sisällytä se projektin. Tässä opetusohjelmassa oletetaan, että kuva on nimeltään `image.jpg`.

2. Napsauta kuvaa hiiren kakkospainikkeella ja valitse sitten **Ominaisuudet**. Varmista, että **Kopioi tulosteen kansioon** asetuksena **aina kopio**.

    ![][1]

3. Lisää seuraavista väittämistä **Program.cs** -tiedostoon tiedoston yläosassa:

        using System.IO;

4. Lisää **ohjelma** luokan seuraavalla tavalla:
         
        private static async void SendToBlobAsync()
        {
            string fileName = "image.jpg";
            Console.WriteLine("Uploading file: {0}", fileName);
            var watch = System.Diagnostics.Stopwatch.StartNew();

            using (var sourceData = new FileStream(@"image.jpg", FileMode.Open))
            {
                await deviceClient.UploadToBlobAsync(fileName, sourceData);
            }

            watch.Stop();
            Console.WriteLine("Time to upload file: {0}ms\n", watch.ElapsedMilliseconds);
        }

    `UploadToBlobAsync` Menetelmä tiedoston nimi ja stream lähteessä tiedoston ladattava ja tallennustilaa lataamisen käsittelee. Console-sovellus näyttää ensin lataamaan tiedosto kuluvaa aikaa.

5. Lisää seuraava menetelmä **Main** -tapaa oikealle ennen `Console.ReadLine()` riville:

        SendToBlobAsync();

> [AZURE.NOTE] Tässä opetusohjelmassa Toteuta uudelleen kaikki käytännöt yksinkertaisuuden 's sake. Tuotannon koodi olisi Toteuta kuin ehdotettua [Lyhytkestoisia vika käsittely]MSDN-sivuston artikkelissa (kuten eksponentiaalisen backoff), yritä käytännöt.

## <a name="receive-a-file-upload-notification"></a>Tiedoston lataaminen-ilmoituksen saaminen

Tässä osassa voit kirjoittaa Windows console-sovellus, joka vastaanottaa tiedoston lataamisen ilmoitukset IoT-toiminnosta.

1. Nykyisen Visual Studio-ratkaisuun Luo uusi Visual C# Windows projekti käyttämällä **Konsolisovelluksen** projektimalli. Projektin **ReadFileUploadNotification**nimi.

    ![Uuden projektin Visual Studiossa][2]

2. Napsauta ratkaisunhallinnassa **ReadFileUploadNotification** projektin hiiren kakkospainikkeella ja valitse sitten **NuGet pakettien hallinta**.

    Tämä näyttää NuGet pakettien hallinta-ikkunassa.

2. Etsi `Microsoft.Azure.Devices`, valitsemalla **Asenna**ja hyväksy käyttöehdot. 

    Tämä Lataa, asentaa ja lisää viittauksen [Azure IoT - palvelun SDK NuGet paketti] **ReadFileUploadNotification** Projectissa.

3. Lisää seuraavista väittämistä **Program.cs** -tiedostoon tiedoston yläosassa:

        using Microsoft.Azure.Devices;

4. Lisää seuraavat kentät **ohjelma** -luokka. Korvaa paikkamerkin arvon IoT keskittimeen yhteysmerkkijonoa [IoT keskittimeen käytön aloittaminen]:

        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
        
5. Lisää **ohjelma** luokan seuraavalla tavalla:
   
        private async static Task ReceiveFileUploadNotificationAsync()
        {
            var notificationReceiver = serviceClient.GetFileNotificationReceiver();

            Console.WriteLine("\nReceiving file upload notification from service");
            while (true)
            {
                var fileUploadNotification = await notificationReceiver.ReceiveAsync();
                if (fileUploadNotification == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received file upload noticiation: {0}", string.Join(", ", fileUploadNotification.BlobName));
                Console.ResetColor();

                await notificationReceiver.CompleteAsync(fileUploadNotification);
            }
        }

    Huomaa, että vastaanotto-kaava on sama kuin cloud laitteen virhesanomia laite-sovelluksen avulla.

6. Lisää seuraavat rivit- **Main** -menetelmää:

        Console.WriteLine("Receive file upload notifications\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
        ReceiveFileUploadNotificationAsync().Wait();
        Console.ReadLine();

## <a name="run-the-applications"></a>Suorita sovellukset

Nyt olet valmis suorittamaan sovellukset.

1. Visual Studiossa napsauttamalla sitä hiiren kakkospainikkeella ratkaisu ja valitse **Määritä käynnistys projektit**. Valitse **useita käynnistys projekteja**ja valitse sitten **Aloita** itseltäsi **ReadFileUploadNotification** ja **SimulatedDevice**.

2. Painamalla **F5-näppäintä**. Käynnistä molemmat sovellukset. Raportissa pitäisi näkyä valmis yhden console-sovelluksessa ja lataa Ilmoitusviesti on vastaanottanut console-sovelluksen lataaminen. Voit käyttää [Azure portal] tai Visual Studio palvelimen Explorer tarkistaa ladatun tiedoston esiintyminen Azure-tallennustilan tilin.

  ![][50]


## <a name="next-steps"></a>Seuraavat vaiheet

Tässä opetusohjelmassa opit, miten voit hyödyntää tiedoston lataamisen ominaisuuksia IoT keskittimeen helpottaa tiedostojen latauksia laitteilla. Voit jatkaa Tutustu IoT keskittimeen ominaisuudet ja skenaariot, jossa on seuraavissa artikkeleissa:

- [Luo IoT-toiminnossa ohjelmallisesti][lnk-create-hub]
- [C SDK esittely][lnk-c-sdk]
- [IoT keskittimeen SDK: T][lnk-sdks]

Tutustu tarkemmin IoT keskittimeen ominaisuuksia, katso:

- [Simuloimalla laitetta, jonka IoT yhdyskäytävän SDK-paketissa][lnk-gateway]

<!-- Images. -->

[50]: ./media/iot-hub-csharp-csharp-file-upload/run-apps1.png
[1]: ./media/iot-hub-csharp-csharp-file-upload/image-properties.png
[2]: ./media/iot-hub-csharp-csharp-file-upload/create-identity-csharp1.png

<!-- Links -->

[Azure portal]: https://portal.azure.com/

[Azure Data Factory]: https://azure.microsoft.com/documentation/services/data-factory/
[Hadoop]: https://azure.microsoft.com/documentation/services/hdinsight/

[Cloud laitteen IoT keskittimeen sisältävien viestien lähettäminen]: iot-hub-csharp-csharp-c2d.md
[Käsittele laitteen Cloud viestit]: iot-hub-csharp-csharp-process-d2c.md
[IoT keskittimeen käytön aloittaminen]: iot-hub-csharp-csharp-getstarted.md
[Azure IoT Developer Center]: http://www.azure.com/develop/iot

[Lyhytkestoisia virheen käsittely]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure-tallennustilan]: ../storage/storage-create-storage-account.md#create-a-storage-account
[lnk-configure-upload]: iot-hub-configure-file-upload.md
[Azure IoT - palvelun SDK NuGet paketti]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md


