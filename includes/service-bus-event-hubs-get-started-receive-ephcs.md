## <a name="receive-messages-with-eventprocessorhost"></a>Vastaanottamaan viestejä EventProcessorHost

[EventProcessorHost][] on .NET-luokka, joka helpottaa tapahtuman keskittimet mukaan hallinta pysyvä tarkistuspisteet vastaanottamisessa tapahtumia ja näiden tapahtuman keskittimet niistä rinnakkain. Käytä [EventProcessorHost][], voit jakaa tapahtumien yli useita vastaanottajia, myös silloin, kun eri solmujen ylläpidettävä. Tässä esimerkissä näytetään, miten voit käyttää [EventProcessorHost][] yksittäisen vastaanottajan. [Skaalattu ulos tapahtuman käsittely][] -malli näkyy [EventProcessorHost][] käyttäminen useiden vastaanottajia.

Jos haluat käyttää [EventProcessorHost][], on oltava [Azure-tallennustilan tilin][]:

1. Kirjaudu sisään [Azure portal][]ja sitten **Uusi** yläreunassa näytön vasemmassa.

2. **Esimerkkitiedot + tallennustilan**ja sitten **tallennustilan tilin**.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage1.png)

3. Kirjoita **Luo tallennustilan tili** -sivu-tallennustilan tilin. Valitse Azure tilauksen, resurssiryhmä ja sijainti, johon resurssi luomiseen. Valitse **Luo**.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage2.png)

4. Valitse luomasi tallennustilan tilin tallennustilan tilien luettelo.

5. Valitse **pikanäppäinten**tallennustilan tili-sivu. Kopioi **Avain1** käyttäminen jäljempänä tässä opetusohjelmassa arvo.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage3.png)

4. Visual Studiossa Luo uusi Visual C# Desktop App projekti käyttämällä **Konsolisovelluksen** projektimalli. Projektin **vastaanottajan**nimi.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-receiver-csharp1.png)

5. Napsauta ratkaisunhallinnassa-ratkaisun hiiren kakkospainikkeella ja valitse sitten **Ratkaisu NuGet pakettien hallinta**.

6. **Selaa** -välilehti ja valitse Hae `Microsoft Azure Service Bus Event Hub - EventProcessorHost`. Varmista, että projektinimi (**Vastaanottaja**) on määritetty **versiot** -ruutuun. Valitse **Asenna**ja hyväksy käyttöehdot.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-eph-csharp1.png)

    Visual Studio Lataa, asentaa ja lisää viittauksen [Azure palvelun Bus tapahtuman keskittimeen - EventProcessorHost NuGet-paketti](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost), kaikki sen riippuvuussuhteessa.

7. **Vastaanottajan** projektin hiiren kakkospainikkeella, valitse **Lisää**ja valitse sitten **luokan**. Uuden luokan **SimpleEventProcessor**nimi ja valitse sitten **Lisää** uuden luokan.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-receiver-csharp2.png)

8. Lisää seuraavista väittämistä SimpleEventProcessor.cs tiedoston yläosassa:

    ```
    using Microsoft.ServiceBus.Messaging;
    using System.Diagnostics;
    ```

    Korvaamalla luokan leipäteksti seuraava koodi:

    ```
    class SimpleEventProcessor : IEventProcessor
    {
        Stopwatch checkpointStopWatch;

        async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
            if (reason == CloseReason.Shutdown)
            {
                await context.CheckpointAsync();
            }
        }

        Task IEventProcessor.OpenAsync(PartitionContext context)
        {
            Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
            this.checkpointStopWatch = new Stopwatch();
            this.checkpointStopWatch.Start();
            return Task.FromResult<object>(null);
        }

        async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (EventData eventData in messages)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());

                Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                    context.Lease.PartitionId, data));
            }

            //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
            if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
            {
                await context.CheckpointAsync();
                this.checkpointStopWatch.Restart();
            }
        }
    }
    ```

    Tähän luokkaan kutsutaan **EventProcessorHost** prosessin tapahtumien vastaanotetut tapahtuma-toiminnosta. Huomaa, että `SimpleEventProcessor` luokan käyttää ajanottokello kutsua säännöllisesti **EventProcessorHost** kontekstin tarkistuspiste-menetelmää. Näin varmistat, että vastaanottaja on käynnistettävä uudelleen, jos se menetetään enintään viisi minuuttia käsittelyn työmäärä.

9. **Ohjelma** luokassa, Lisää seuraava `using` lauseen tiedoston yläosassa:

    ```
    using Microsoft.ServiceBus.Messaging;
    ```

    Korvaa sitten `Main` menetelmällä `Program` seuraava koodi-toiminnossa tapahtuman nimi ja nimitila tason yhteysmerkkijonon, jotka on tallennettu aiemmin, korvaaminen ja tallennustilaa tili ja avainta, joka on kopioitu edellisten kohtien luokan. 

    ```
    static void Main(string[] args)
    {
      string eventHubConnectionString = "{Event Hub connection string}";
      string eventHubName = "{Event Hub name}";
      string storageAccountName = "{storage account name}";
      string storageAccountKey = "{storage account key}";
      string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);

      string eventProcessorHostName = Guid.NewGuid().ToString();
      EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
      Console.WriteLine("Registering EventProcessor...");
      var options = new EventProcessorOptions();
      options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
      eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();

      Console.WriteLine("Receiving. Press enter key to stop worker.");
      Console.ReadLine();
      eventProcessorHost.UnregisterEventProcessorAsync().Wait();
    }
    ```

> [AZURE.NOTE] Tässä opetusohjelmassa käytetään [EventProcessorHost][]yksittäisen esiintymän. Niin, että siirtonopeuden, on suositeltavaa, että suoritat [EventProcessorHost][]useita kertoja [skaalattu ulos käsittely][] otosten esitetyllä tavalla. Tällöin eri esiintymien automaattisesti toistensa lataamaan saldo vastaanotettu tapahtumien järjestämiseen. Jos haluat useita vastaanottajia kunkin prosessin *kaikki* tapahtumat, sinun on käytettävä **ConsumerGroup** käsitteestä. Kun vastaanottaminen tapahtumat eri tietokoneissa, voi olla hyödyllistä nimet [EventProcessorHost][] esiintymien koneet (tai roolit) on määritettävä johon ne on otettu käyttöön. Lisätietoja näistä aiheista on artikkeleissa [Tapahtuman keskittimet yleiskatsaus][] ja [Tapahtuman keskittimet Programming Guide][] .

<!-- Links -->
[Tapahtuman keskittimet yleiskatsaus]: ../articles/event-hubs/event-hubs-overview.md
[Tapahtuman keskittimet ohjelmointi opas]: ../articles/event-hubs/event-hubs-programming-guide.md
[Skaalattu ulos käsittely]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[Azure-tallennustilan tilin]: ../articles/storage/storage-create-storage-account.md
[EventProcessorHost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx
[Azure portal]: https://portal.azure.com