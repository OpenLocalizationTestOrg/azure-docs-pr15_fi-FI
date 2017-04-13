<properties
    pageTitle="Käsittele IoT keskittimeen laitteen cloud viestejä (.Net) | Microsoft Azure"
    description="Katso tämä opetusohjelma, saat hyödyllisiä kuvioiden avulla ja käsittele IoT keskittimeen laitteen cloud viestejä."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="csharp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="10/05/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-process-iot-hub-device-to-cloud-messages-using-net"></a>Opetusohjelma: Voit käsitellä IoT keskittimeen laitteen cloud viestiä, joissa .net

[AZURE.INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

## <a name="introduction"></a>Johdanto

Azure IoT-toiminnossa on täysin hallitun palvelu, joka mahdollistaa luotettava ja suojatun kaksisuuntaisen tietoliikenteen miljoonia IoT laitteet ja sovelluksen takaisin end-näppäinyhdistelmää. Muut opetusohjelmat ([IoT keskittimeen käytön aloittaminen] ja [cloud laitteen IoT keskittimeen sisältävien viestien lähettäminen][lnk-c2d]), kuinka voit käyttää pilvipalveluun laitteen ja cloud laitteen tekstiviesti perustoimintoja IoT toiminnossa.

Tässä opetusohjelmassa muodostaa [IoT keskittimeen käytön aloittaminen] -opetusohjelmassa esitetyn koodin, ja se näkyy kaksi skaalattava kuvioita, joiden avulla voit käsitellä laitteen cloud viestit:

- Laitteen cloud viestien [Azure-blob-säiliö]luotettava tallennustilaan. Käytetty vaihtoehto on *kylmän polku* analytics, johon tallennat telemetriatietojen tietojen BLOB käytettävä analytics prosessit-syötteestä. Nämä prosessit voivat perustuva varten työkaluilla, kuten [Azure Data Factory] tai [HDInsight (Hadoop)] -pino.

- *Vuorovaikutteinen* laitteen cloud viestien luotettava käsittely. Laitteen cloud viestit ovat vuorovaikutteisia, kun ne ovat heti käynnistimet sovelluksen takaisin loppuun toimintojoukon. Esimerkiksi laite saattaa Lähetä hälytys, joka käynnistää lippu lisäämisestä CRM-järjestelmään. Sen sijaan *arvopisteen* viestien syötteen yksinkertaisesti analytics-ohjelma. Esimerkiksi lämpötilan telemetriatietojen laitteesta, joka on tallennettu myöhempää käyttöä varten on tietoja kohta sanoma.

Koska IoT keskittimeen paljastaa [Tapahtumaa-toiminnossa][lnk-event-hubs]-yhteensopiva päätepiste vastaanottaa laitteen cloud viestejä, tämän opetusohjelman käyttää [EventProcessorHost] esiintymä. Tämä esiintymä:

* Tallentaa luotettavasti Azure-blob-säiliö *arvopisteen* viestejä.
* Välittää *Vuorovaikutteinen* laitteen cloud viestien Azure [palvelun Bus jono] heti käsittelyä varten.

Palvelun Bus avulla voit varmistaa luotettava käsittelyn vuorovaikutteinen viestejä, kuten se sisältää viestin kohti tarkistuspisteet ja aikaa ikkunan perustuva jään monistaminen.

> [AZURE.NOTE] Erillisen **EventProcessorHost** on vain yksi tapa käsitellä vuorovaikutteinen viestit. Muut vaihtoehdot ovat [Azure palvelun kangasta] [ lnk-service-fabric] ja [Azure Stream analysoinnin][lnk-stream-analytics].

Tässä opetusohjelmassa lopussa suoritat kolme Windows console-sovellusta:

* **SimulatedDevice**, muokattu sovellusversio luotu [aloittaminen IoT keskittimeen] opetusohjelmassa lähettää tietoja pisteen laitteen cloud viestien jokaisen toisen ja vuorovaikutteinen laitteen cloud viestien 10 sekunnin välein. Sovellus käyttää AMQP-protokollaa pitää yhteyttä IoT toiminnossa.
* **ProcessDeviceToCloudMessages** käyttää [EventProcessorHost] luokan viestien noutaminen tapahtumaa-toiminnossa yhteensopivan päätepiste. Se sitten luotettavasti tallentaa tietojen pisteen viestejä Azure-blob-säiliö ja välittää palvelun Bus jonon vuorovaikutteinen viestejä.
* **ProcessD2CInteractiveMessages** jonot varaustiedoista palvelun Bus jonossa vuorovaikutteinen viestejä.

> [AZURE.NOTE] IoT toiminnosta on monta laitteen alustojen ja eri kielellä, esimerkiksi C Java ja JavaScript SDK tuki. Lisätietoja poistamisesta korvaa Tässä opetusohjelmassa Simuloitu laitteen fyysinen laite ja Yhdistä laitteet IoT-keskittimeen on artikkelissa [Azure IoT Developer Center].

Tässä opetusohjelmassa sovelletaan suoraan muita tapoja käyttää tapahtumaa-toiminnossa yhteensopivan viestejä, kuten [HDInsight (Hadoop)] -projektit. Lisätietoja [Azure IoT keskittimeen developer guide - laitteen pilveen].

Tässä opetusohjelmassa suorittamiseen tarvitset seuraavat:

+ Microsoft Visual Studio 2015.

+ Azure active tili. <br/>Jos sinulla ei ole tiliä, voit luoda [ilmaisen tilin](https://azure.microsoft.com/free/) muutamaan minuuttiin.

Jotkin basic kokemusta [Azuren tallennustilaan] ja [Azure palvelun Bus]pitäisi olla.


## <a name="send-interactive-messages-from-a-simulated-device"></a>Vuorovaikutteinen lähettämiseen Simuloitu laitteesta

Tässä osassa voit muokata Simuloitu laitteen sovelluksen luomasi vuorovaikutteinen laitteen cloud viestien lähettämiseen IoT keskittimeen [IoT keskittimeen käytön aloittaminen] -opetusohjelmassa.

1. Lisätä seuraavalla tavalla Visual Studion **SimulatedDevice** Projectissa, **ohjelma** -luokka.

    ```
    private static async void SendDeviceToCloudInteractiveMessagesAsync()
    {
      while (true)
      {
        var interactiveMessageString = "Alert message!";
        var interactiveMessage = new Message(Encoding.ASCII.GetBytes(interactiveMessageString));
        interactiveMessage.Properties["messageType"] = "interactive";
        interactiveMessage.MessageId = Guid.NewGuid().ToString();

        await deviceClient.SendEventAsync(interactiveMessage);
        Console.WriteLine("{0} > Sending interactive message: {1}", DateTime.Now, interactiveMessageString);

        Task.Delay(10000).Wait();
      }
    }
    ```

    Tämä menetelmä muistuttaa **SimulatedDevice** projektin **SendDeviceToCloudMessagesAsync** -menetelmää. Vain erot on, että olet määrittänyt nyt **MessageId** järjestelmä-ominaisuutta ja käyttäjäominaisuus nimeltä **messageType**.
    Koodi määrittää GUID-tunnus (GUID) **MessageId** -ominaisuutta. Palvelun Bus avulla tunnus Poista monista se vastaanottaa viestejä. Otosten erottaa vuorovaikutteinen tietojen pisteen viesteistä **messageType** -ominaisuuden avulla. Sovelluksen määritysvaiheisiin viestin ominaisuudet, tämä tieto sen sijaan, että viestin tekstiosaan niin, että tapahtuma-suoritin ei tarvitse poistaa viestin suorittaa viestin reititys.

    > [AZURE.NOTE] On tärkeää luoda **MessageId** kaksoiskappaleet varaustiedoista vuorovaikutteinen viestien laite-koodin avulla. Katkonainen tietoliikenne tai muita epäonnistuu saattaa johtaa useita uudelleenlähetysten saman viestin kyseisen laitteesta. Voit käyttää myös semanttinen viestin tunnuksen, kuten hash haluamasi viesti-tietokenttiä GUID-tunnus-tekstin näyttäminen.

2. Lisää seuraava menetelmä **Main** -tapaa oikealle ennen `Console.ReadLine()` riville:

    ````
    SendDeviceToCloudInteractiveMessagesAsync();
    ````

    > [AZURE.NOTE] Tässä opetusohjelmassa Toteuta yksinkertaisuuden, yritä kaikki käytännöt. Tuotannon koodi olisi Toteuta uudelleen käytäntö, kuten eksponentiaalisen backoff kuin ehdotettua [Lyhytkestoisia vika käsittely]MSDN-artikkelista.

## <a name="process-device-to-cloud-messages"></a>Käsittele laitteen cloud viestit

Tässä osassa voit luoda Windows console-sovellus, joka käsittelee laitteen cloud viestien IoT-toiminnosta. IOT keskittimeen paljastaa [tapahtumaa-toiminnossa]-yhteensopiva päätepisteen ottaa käyttöön sovelluksen laitteen cloud viestien lukeminen. Tässä opetusohjelmassa käytetään [EventProcessorHost] luokan Käsittele kyseisiä viestejä console-sovelluksessa. Lisätietoja siitä, miten prosessi viesteihin-tapahtuman keskittimet on artikkelissa [Tapahtuman keskittimet käytön aloittaminen] -opetusohjelma.

Todennus, kun otat luotettava tallennustilan arvopiste viestejä tai vuorovaikutteinen viestien edelleenlähetys kyseisen tapahtuman käsittely on riippuvainen viestin kuluttaja tarkistuspisteet antamaan etenemisen. Lisäksi saavuttamiseksi suuren siirtonopeuden, kun luetaan siis tapahtuman keskittimet pitäisi sisältää tarkistuspisteet suuri erissä. Tämä menetelmä luo vahinkojen kaksoiskappaleet käsitellään saat paljon viestejä Jos on ilmennyt virhe ja voit palata edelliseen tarkistuspiste. Tässä opetusohjelmassa näet, miten Azuren tallennustilaan kirjoituksia ja palvelun Bus jään monistaminen windows synkronointiin **EventProcessorHost** tarkistuspisteet.

Kirjoittaa viestejä Azuren tallennustilaan luotettavasti, otosten käyttää yksittäisiä estä Vahvista-ominaisuus, [Estä BLOB][Azure Block Blobs]. Tapahtuman käsittelijä kasvaa viestien muistiin, kunnes se on jatkuva tarkistuspisteen. Viestien kertynyt puskurin saavuttaa 4 Mt suurimman lohkon koon, tai jälkeen-palvelun Bus jään monistaminen aikaikkunan esimerkiksi kuluu. Valitse ennen tarkistuspiste-koodin tallentaa uuden kohteen blob.

Tapahtuman käsittelijä käyttää tapahtuman keskittimet viestin siirtymät palkkina tunnukset. Tämä järjestelmä mahdollistaa tapahtuma-suoritin suorittaa jään monistaminen tarkistuksen, ennen kuin se tallentaa uuden kohteen tallennustilaa, on mahdollista kaatumisen välillä vahvistetaan lohko ja tarkistuspiste varoen.

> [AZURE.NOTE] Tässä opetusohjelmassa käytetään yksittäisen Azure-tallennustilan tilin kirjoittaa kaikki viestit, jotka on haettu IoT-toiminnosta. Päätä, jos haluat käyttää useita Azuren tallennustilaan tilejä ratkaisu, on artikkelissa [Azure-tallennustilan skaalattavuus ohjeita].

Sovellus käyttää palvelun Bus jään monistaminen ominaisuus välttämiseksi kaksoiskappaleet, kun se käsittelee vuorovaikutteinen viestejä. Simuloitu laitteen aikaleimaa vuorovaikutteinen viesti yksilöllinen **MessageId**kanssa. Näiden tunnusten käyttöön palvelun Bus varmistaa, että määritetyn jään monistaminen aika-ikkunassa ei ole kaksi viestit, joissa on sama **MessageId** toimitetaan vastaanottajat. Jään kopiot, ja viestin kohti valmistumista semantiikkaan-liittyvien palvelun Bus olevien myöntämä toteuttamisesta vuorovaikutteinen viestien luotettava käsittely on helppoa.

Koodin synkronointi **EventProcessorHost** tarkistuspiste järjestelmä voit varmistaa, että ei viesti on lähetetty uudelleen jään monistaminen ikkunan ulkopuolella, palvelun Bus jono jään monistaminen-ikkuna. Tämä synkronointi tapahtuu pakottamalla tarkistuspisteen vähintään kerran, joka kerta, kun jään monistaminen ikkunan kuluu (Tässä opetusohjelmassa ikkuna on tunnin).

> [AZURE.NOTE] Tässä opetusohjelmassa käytetään yksittäisen osioitua palvelun Bus-jonossa prosessin vuorovaikutteinen viestit, jotka on haettu IoT-toiminnosta. Lisätietoja opit käyttämään skaalattavuus ehtojen mukaan, että palvelun Bus olevien [Azure palvelun Bus] ohjeissa.

### <a name="provision-an-azure-storage-account-and-a-service-bus-queue"></a>Azure-tallennustilan tilin ja palvelun Bus jonon valmistelu
Käyttämään [EventProcessorHost] -luokka on oltava Azure-tallennustilan tilin käyttöön **EventProcessorHost** tarkistuspisteen tietoja. Voit käyttää aiemmin luodun Azure-tallennustilan tilin tai noudata [Azuren tallennustilaan] voit luoda uuden. Pane merkille Azure-tallennustilan tilin yhteysmerkkijonon.

> [AZURE.NOTE] Kun kopioit ja liität Azure-tallennustilan tilin yhteysmerkkijonon, varmista, että osoitteessa ei ole levyä välilyöntejä.

Sinun on myös palvelun Bus jono käyttöön luotettava käsittelyn vuorovaikutteinen viestejä. Voit luoda jonon ohjelmallisesti tunnin jään monistaminen-ikkunassa [palvelun Bus olevien käyttämisestä][palvelun Bus jonon]artikkelissa kuvatulla tavalla. Vaihtoehtoisesti voit käyttää [Azure perinteinen portal][lnk-classic-portal], toimimalla seuraavasti:

1. Valitse **Uusi** vasemmassa alakulmassa. Valitse **Sovelluksen palvelut** > **Palvelun Bus** > **jonossa** > **Mukautettu luominen**. Kirjoita nimi- **d2ctutorial**, alueen, valitse ja käytä aiemmin nimitilan tai luoda uuden. Valitse seuraavalla sivulla **käyttöön kaksoiskappaleiden**ja asettaminen tunnin **kaksoiskappaleiden tunnistus historia aika-ikkuna** . Valitse oikeassa alakulmassa Tallenna jonon kokoonpanosi valintamerkki.

    ![Luo jono Azure-portaalissa][30]

2. Palvelun Bus olevien ‑luettelosta **d2ctutorial**ja valitse sitten **Määritä**. Luo kaksi jaettuun käyttöön käytännöt, yksi kutsuttu **Lähetä** **Lähetä** oikeuksilla ja yksi kutsuttu **kuunnella** **kuunnella** käyttöoikeuksilla. Kun olet valmis, valitse **Tallenna** alaosassa.

    ![Määritä jonossa Azure-portaalissa][31]

3. Valitse ylä- ja sitten **Yhteystiedot** **raporttinäkymät-ikkunan** alareunassa. Pane merkille kaksi yhteyden merkkijonoja.

    ![Jonon Raporttinäkymät-ikkunan Azure-portaalissa][32]

### <a name="create-the-event-processor"></a>Luo tapahtuma-suoritin

1. Valitse nykyinen Visual Studio-ratkaisuun, voit luoda projektin Visual C# Windows **Console-sovelluksen** project-malli, **Tiedosto** > **Lisää** > **Uusi projekti**. Varmista, että .NET Framework-versio on 4.5.1 tai uudempi. Projektin **ProcessDeviceToCloudMessages**nimeä ja valitse **OK**.

    ![Uuden projektin Visual Studiossa][10]

2. Napsauta ratkaisunhallinnassa **ProcessDeviceToCloudMessages** projektin hiiren kakkospainikkeella ja valitse sitten **NuGet pakettien hallinta**. **NuGet pakettien hallinta** -valintaikkuna.

3. Etsi **WindowsAzure.ServiceBus**, valitsemalla **Asenna**ja hyväksy käyttöehdot. Tämä toiminto lataa, asentaa ja lisää viittauksen [Azure palvelun Bus NuGet-paketti](https://www.nuget.org/packages/WindowsAzure.ServiceBus), kaikki sen riippuvuussuhteessa.

4. Etsi **Microsoft.Azure.ServiceBus.EventProcessorHost**, valitsemalla **Asenna**ja hyväksy käyttöehdot. Tämä toiminto lataa, asentaa ja lisää viittauksen [Azure palvelun Bus tapahtuman keskittimeen - EventProcessorHost NuGet-paketti](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost), kaikki sen riippuvuussuhteessa.

5. **ProcessDeviceToCloudMessages** projektin hiiren kakkospainikkeella, valitse **Lisää**ja valitse sitten **luokka**. Uuden luokan **StoreEventProcessor**nimeä ja valitse **OK** , jos haluat luoda luokan.

6. Lisää seuraavista väittämistä StoreEventProcessor.cs tiedoston yläosassa:

    ```
    using System.IO;
    using System.Diagnostics;
    using System.Security.Cryptography;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

7. Korvaa luokan leipäteksti seuraava koodi:

    ```
    class StoreEventProcessor : IEventProcessor
    {
      private const int MAX_BLOCK_SIZE = 4 * 1024 * 1024;
      public static string StorageConnectionString;
      public static string ServiceBusConnectionString;

      private CloudBlobClient blobClient;
      private CloudBlobContainer blobContainer;
      private QueueClient queueClient;

      private long currentBlockInitOffset;
      private MemoryStream toAppend = new MemoryStream(MAX_BLOCK_SIZE);

      private Stopwatch stopwatch;
      private TimeSpan MAX_CHECKPOINT_TIME = TimeSpan.FromHours(1);

      public StoreEventProcessor()
      {
        var storageAccount = CloudStorageAccount.Parse(StorageConnectionString);
        blobClient = storageAccount.CreateCloudBlobClient();
        blobContainer = blobClient.GetContainerReference("d2ctutorial");
        blobContainer.CreateIfNotExists();
        queueClient = QueueClient.CreateFromConnectionString(ServiceBusConnectionString);
      }

      Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
      {
        Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
        return Task.FromResult<object>(null);
      }

      Task IEventProcessor.OpenAsync(PartitionContext context)
      {
        Console.WriteLine("StoreEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);

        if (!long.TryParse(context.Lease.Offset, out currentBlockInitOffset))
        {
          currentBlockInitOffset = 0;
        }
        stopwatch = new Stopwatch();
        stopwatch.Start();

        return Task.FromResult<object>(null);
      }

      async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
      {
        foreach (EventData eventData in messages)
        {
          byte[] data = eventData.GetBytes();

          if (eventData.Properties.ContainsKey("messageType") && (string) eventData.Properties["messageType"] == "interactive")
          {
            var messageId = (string) eventData.SystemProperties["message-id"];

            var queueMessage = new BrokeredMessage(new MemoryStream(data));
            queueMessage.MessageId = messageId;
            queueMessage.Properties["messageType"] = "interactive";
            await queueClient.SendAsync(queueMessage);

            WriteHighlightedMessage(string.Format("Received interactive message: {0}", messageId));
            continue;
          }

          if (toAppend.Length + data.Length > MAX_BLOCK_SIZE || stopwatch.Elapsed > MAX_CHECKPOINT_TIME)
          {
            await AppendAndCheckpoint(context);
          }
          await toAppend.WriteAsync(data, 0, data.Length);

          Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
            context.Lease.PartitionId, Encoding.UTF8.GetString(data)));
        }
      }

      private async Task AppendAndCheckpoint(PartitionContext context)
      {
        var blockIdString = String.Format("startSeq:{0}", currentBlockInitOffset.ToString("0000000000000000000000000"));
        var blockId = Convert.ToBase64String(ASCIIEncoding.ASCII.GetBytes(blockIdString));
        toAppend.Seek(0, SeekOrigin.Begin);
        byte[] md5 = MD5.Create().ComputeHash(toAppend);
        toAppend.Seek(0, SeekOrigin.Begin);

        var blobName = String.Format("iothubd2c_{0}", context.Lease.PartitionId);
        var currentBlob = blobContainer.GetBlockBlobReference(blobName);

        if (await currentBlob.ExistsAsync())
        {
          await currentBlob.PutBlockAsync(blockId, toAppend, Convert.ToBase64String(md5));
          var blockList = await currentBlob.DownloadBlockListAsync();
          var newBlockList = new List<string>(blockList.Select(b => b.Name));

          if (newBlockList.Count() > 0 && newBlockList.Last() != blockId)
          {
            newBlockList.Add(blockId);
            WriteHighlightedMessage(String.Format("Appending block id: {0} to blob: {1}", blockIdString, currentBlob.Name));
          }
          else
          {
            WriteHighlightedMessage(String.Format("Overwriting block id: {0}", blockIdString));
          }
          await currentBlob.PutBlockListAsync(newBlockList);
        }
        else
        {
          await currentBlob.PutBlockAsync(blockId, toAppend, Convert.ToBase64String(md5));
          var newBlockList = new List<string>();
          newBlockList.Add(blockId);
          await currentBlob.PutBlockListAsync(newBlockList);

          WriteHighlightedMessage(String.Format("Created new blob", currentBlob.Name));
        }

        toAppend.Dispose();
        toAppend = new MemoryStream(MAX_BLOCK_SIZE);

        // checkpoint.
        await context.CheckpointAsync();
        WriteHighlightedMessage(String.Format("Checkpointed partition: {0}", context.Lease.PartitionId));

        currentBlockInitOffset = long.Parse(context.Lease.Offset);
        stopwatch.Restart();
      }

      private void WriteHighlightedMessage(string message)
      {
        Console.ForegroundColor = ConsoleColor.Yellow;
        Console.WriteLine(message);
        Console.ResetColor();
      }
    }
    ```

    **EventProcessorHost** luokan soittaa tähän luokkaan käsittelemään laitteen cloud-viestejä, jotka on vastaanotettu IoT-toiminnosta. Tähän luokkaan koodin toteuttaa logiikan viestien tallentaminen luotettavasti blob-säilö ja palvelun Bus jonossa vuorovaikutteinen viestien edelleenlähetys.

    **OpenAsync** menetelmä alustaa **currentBlockInitOffset** -muuttuja, joka seuraa kuluvan siirtymä lukea tapahtuman suorittimen ensimmäisen viestin. Muista, että suoritin vastaa osio.

    **ProcessEventsAsync** -menetelmä vastaanottaa viestejä erän IoT-toiminnosta ja käsittelee niitä seuraavasti: se lähettää vuorovaikutteinen viestien palvelun Bus jonossa, ja liittää tietojen pisteen viestejä kutsutaan **toAppend**muistin puskuri. Jos muistin puskuri saavuttaa 4 megatavua tai jään monistaminen aina Windowsin kuluu (tunnin kuluttua tarkistuspisteen Tässä opetusohjelmassa)-sovelluksen käynnistää tarkistuspisteen.

    **AppendAndCheckpoint** menetelmä luo ensin blockId liittäminen lohkon. Azure-tallennustilan edellyttää, että kaikki estää tunnuksista on yhtä pitkiksi, joten menetelmä pads siirtymä nollia - `currentBlockInitOffset.ToString("0000000000000000000000000")`. Valitse Jos Estä tämän tunnuksen on jo blob-menetelmä korvaa se puskuri nykyisen sisällön.

    > [AZURE.NOTE] Tässä opetusohjelmassa käyttää helpottaa koodin yksittäisen blob osion kohden viestejä. Todellinen ratkaisu Toteuta tiedoston juoksevan luomalla uusia tiedostoja jälkeen tietyn ajan tai kun ne saavuttaa tietyn koon summan. Muista, että estä Azure-blob sisältää enintään 195 gt tietoja.

8. Lisää **ohjelma** -luokan **avulla** -lauseen yläreunassa:

    ```
    using Microsoft.ServiceBus.Messaging;
    ```

9. Muokkaa **Main** -menetelmä **ohjelma** -luokassa seuraavasti. Korvaa **{iot keskittimeen yhteysmerkkijonon}** [IoT keskittimeen käytön aloittaminen] -opetusohjelma **iothubowner** yhteysmerkkijonoa. Korvaa tallennustilan yhteysmerkkijono yhteysmerkkijono ole mainittu tässä osassa alkuun. Korvaa palvelun Bus yhteysmerkkijonon nimeltä **d2ctutorial** ole mainittu tässä osassa alkuun jonossa **Lähetä** käyttöoikeudet:

    ```
    static void Main(string[] args)
    {
      string iotHubConnectionString = "{iot hub connection string}";
      string iotHubD2cEndpoint = "messages/events";
      StoreEventProcessor.StorageConnectionString = "{storage connection string}";
      StoreEventProcessor.ServiceBusConnectionString = "{service bus send connection string}";

      string eventProcessorHostName = Guid.NewGuid().ToString();
      EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, iotHubD2cEndpoint, EventHubConsumerGroup.DefaultGroupName, iotHubConnectionString, StoreEventProcessor.StorageConnectionString, "messages-events");
      Console.WriteLine("Registering EventProcessor...");
      eventProcessorHost.RegisterEventProcessorAsync<StoreEventProcessor>().Wait();

      Console.WriteLine("Receiving. Press enter key to stop worker.");
      Console.ReadLine();
      eventProcessorHost.UnregisterEventProcessorAsync().Wait();
    }
    ```

    > [AZURE.NOTE] Tässä opetusohjelmassa käyttää yksinkertaisuuden, yksittäisen esiintymän [EventProcessorHost] -luokka. Lisätietoja on artikkelissa [Tapahtuman keskittimet Programming Guide].

## <a name="receive-interactive-messages"></a>Vuorovaikutteinen virhesanomia
Tässä osassa voit kirjoittaa Windows console-sovellus, joka vastaanottaa vuorovaikutteinen viestit palvelun Bus jonossa. Saat lisätietoja arkkitehti käyttäen palvelua Bus ratkaista [palvelun Bus monitasoisten sovellusten luominen][].

1. Nykyisen Visual Studio-ratkaisun luominen Visual C# Windows projektin **Konsolisovelluksen** project-mallin. Projektin **ProcessD2CInteractiveMessages**nimi.

2. Napsauta ratkaisunhallinnassa **ProcessD2CInteractiveMessages** projektin hiiren kakkospainikkeella ja valitse sitten **NuGet pakettien hallinta**. Tämä toiminto näyttää **NuGet Package hallinta** -ikkunassa.

3. Etsi **WindowsAzure.ServiceBus**, valitsemalla **Asenna**ja hyväksy käyttöehdot. Tämä toiminto lataa, asentaa ja lisää viittauksen [Azure palvelun Bus](https://www.nuget.org/packages/WindowsAzure.ServiceBus), kaikki sen riippuvuussuhteessa.

4. Lisää seuraavat **käyttämällä** lauseet **Program.cs** tiedoston yläosassa:

    ```
    using System.IO;
    using Microsoft.ServiceBus.Messaging;
    ```

5. Lisää seuraavat rivit- **Main** -menetelmää. Korvaa yhteysmerkkijonon nimeltä **d2ctutorial**jonossa **kuunnella** käyttöoikeuksilla:

    ```
    Console.WriteLine("Process D2C Interactive Messages app\n");

    string connectionString = "{service bus listen connection string}";
    QueueClient Client = QueueClient.CreateFromConnectionString(connectionString);

    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = false;
    options.AutoRenewTimeout = TimeSpan.FromMinutes(1);

    Client.OnMessage((message) =>
    {
      try
      {
        var bodyStream = message.GetBody<Stream>();
        bodyStream.Position = 0;
        var bodyAsString = new StreamReader(bodyStream, Encoding.ASCII).ReadToEnd();

        Console.WriteLine("Received message: {0} messageId: {1}", bodyAsString, message.MessageId);

        message.Complete();
      }
      catch (Exception)
      {
        message.Abandon();
      }
    }, options);

    Console.WriteLine("Receiving interactive messages from SB queue...");
    Console.WriteLine("Press any key to exit.");
    Console.ReadLine();
    ```

## <a name="run-the-applications"></a>Suorita sovellukset

Nyt olet valmis suorittamaan sovellukset.

1.  Visual Studiossa, napsauta ratkaisunhallinnassa ratkaisu hiiren kakkospainikkeella ja valitse **Määritä käynnistys projektit**. Valitse **useita käynnistys projekteja**ja valitse sitten **Aloita** kuin **ProcessDeviceToCloudMessages**, **SimulatedDevice**ja **ProcessD2CInteractiveMessages** projektit-toiminto.

2.  Painamalla **F5-näppäintä** aloittaaksesi kolme console-sovellukset. **ProcessD2CInteractiveMessages** -sovellus tulee käsitellä jokaisen vuorovaikutteinen viesti **SimulatedDevice** -sovelluksesta.

  ![Kolme console-sovellusta][50]

> [AZURE.NOTE] Näet oman Blob-objektien päivitykset, joudut ehkä vähentää **MAX_BLOCK_SIZE** vakion **StoreEventProcessor** luokan pienempi arvoa, kuten **1 024**. Tämä muutos on hyötyä, koska kestää jonkin aikaa saavuttamiseksi estä kokorajoitus Simuloitu laitteen lähettämien tiedoilla. Estä pienemmäksi, jossa ei tarvitse odottaa niin kauan Nähdäksesi Blob-objektien on luotu ja päivitetään. Kuitenkin suuremmaksi lohko helpottavat sovelluksen paremmin skaalattavissa.

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä opetusohjelmassa opit käsitteleminen luotettavasti käyttämällä [EventProcessorHost] luokan arvopistettä ja vuorovaikutteinen laitteen cloud viestit.

[Cloud laitteen IoT keskittimeen sisältävien viestien lähettäminen] [ lnk-c2d] avulla voit lähettää viestejä laitteet takaisin päästä.

Esimerkkejä valmis lopusta loppuun-ratkaisuja, jotka käyttävät IoT keskittimeen, on artikkelissa [Azure IoT Suite][lnk-suite].

Saat lisätietoja kanssa IoT keskittimeen, ratkaisujen kehittämiseen on [IoT keskittimeen Sovelluskehittäjän opas].

<!-- Images. -->
[50]: ./media/iot-hub-csharp-csharp-process-d2c/run1.png
[10]: ./media/iot-hub-csharp-csharp-process-d2c/create-identity-csharp1.png

[30]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue2.png
[31]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue3.png
[32]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue4.png

<!-- Links -->

[Azure-blob-säiliö]: ../storage/storage-dotnet-how-to-use-blobs.md
[Azure Data Factory]: https://azure.microsoft.com/documentation/services/data-factory/
[HDInsight (Hadoop)]: https://azure.microsoft.com/documentation/services/hdinsight/
[Palvelun Bus jonossa]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md

[Azure IoT keskittimeen developer guide - laitteen pilveen]: iot-hub-devguide-messaging.md

[Azure-tallennustilan]: https://azure.microsoft.com/documentation/services/storage/
[Azure palvelun Bus]: https://azure.microsoft.com/documentation/services/service-bus/

[IoT keskittimeen Sovelluskehittäjän opas]: iot-hub-devguide.md
[IoT keskittimeen käytön aloittaminen]: iot-hub-csharp-csharp-getstarted.md
[Azure IoT Developer Center]: https://azure.microsoft.com/develop/iot
[lnk-service-fabric]: https://azure.microsoft.com/documentation/services/service-fabric/
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-hubs]: https://azure.microsoft.com/documentation/services/event-hubs/
[Lyhytkestoisia virheen käsittely]: https://msdn.microsoft.com/library/hh675232.aspx

<!-- Links -->
[Azure-tallennustilan tietoja]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Tapahtuman keskittimet käytön aloittaminen]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[Azure tallennustilan skaalattavuus ohjeet]: ../storage/storage-scalability-targets.md
[Azure Block Blobs]: https://msdn.microsoft.com/library/azure/ee691964.aspx
[Event Hubs]: ../event-hubs/event-hubs-overview.md
[EventProcessorHost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx
[Tapahtuman keskittimet ohjelmointi opas]: ../event-hubs/event-hubs-programming-guide.md
[Lyhytkestoisia virheen käsittely]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Palvelun Bus monitasoisten sovellusten luominen]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md

[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-c2d]: iot-hub-csharp-csharp-process-d2c.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
