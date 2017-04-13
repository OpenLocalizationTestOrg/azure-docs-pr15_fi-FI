## <a name="send-messages-to-event-hubs"></a>Tapahtuman porttiin lähettämiseen

Tässä osassa kirjoitat Windows console-sovellus, joka lähettää tapahtumien tapahtumaa-toiminnossa.

1. Visual Studiossa Luo uusi Visual C# Desktop App projekti käyttämällä **Konsolisovelluksen** projektimalli. Projektin **lähettäjän**nimi.

    ![](./media/service-bus-event-hubs-getstarted-send-csharp/create-sender-csharp1.png)

2. Napsauta ratkaisunhallinnassa-ratkaisun hiiren kakkospainikkeella ja valitse sitten **Ratkaisu NuGet pakettien hallinta**. 

3. **Selaa** -välilehti ja valitse Hae `Microsoft Azure Service Bus`. Varmista, että projektinimi (**Lähettäjä**) on määritetty **versiot** -ruutuun. Valitse **Asenna**ja hyväksy käyttöehdot. 

    ![](./media/service-bus-event-hubs-getstarted-send-csharp/create-sender-csharp2.png)

    Visual Studio Lataa, asentaa ja lisää viittauksen [Azure palvelun Bus kirjaston NuGet paketti](https://www.nuget.org/packages/WindowsAzure.ServiceBus).

4. Lisää seuraava `using` lauseet **Program.cs** tiedoston yläosassa:

    ```
    using System.Threading;
    using Microsoft.ServiceBus.Messaging;
    ```

5. Lisää seuraavat kentät **ohjelma** luokan korvaaminen edellisessä osassa luomasi tapahtuman-toiminnosta ja tallennettu aiemmin nimitilan tason yhteysmerkkijonon nimen paikkamerkki-arvoja.

    ```
    static string eventHubName = "{Event Hub name}";
    static string connectionString = "{send connection string}";
    ```

6. Lisää **ohjelma** luokan seuraavalla tavalla:

    ```
    static void SendingRandomMessages()
    {
        var eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, eventHubName);
        while (true)
        {
            try
            {
                var message = Guid.NewGuid().ToString();
                Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, message);
                eventHubClient.Send(new EventData(Encoding.UTF8.GetBytes(message)));
            }
            catch (Exception exception)
            {
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("{0} > Exception: {1}", DateTime.Now, exception.Message);
                Console.ResetColor();
            }

            Thread.Sleep(200);
        }
    }
    ```

    Tämä menetelmä lähettää tapahtumien jatkuvasti tapahtumaa-toiminnossa 200 ms viiveellä.

7. Lisää seuraavat rivit- **Main** -menetelmää:

    ```
    Console.WriteLine("Press Ctrl-C to stop the sender process");
    Console.WriteLine("Press Enter to start now");
    Console.ReadLine();
    SendingRandomMessages();
    ```
