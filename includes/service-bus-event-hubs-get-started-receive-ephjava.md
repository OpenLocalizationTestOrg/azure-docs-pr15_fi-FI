## <a name="receive-messages-with-eventprocessorhost-in-java"></a>Vastaanottamaan viestejä EventProcessorHost Java-

EventProcessorHost on Java-luokka, joka helpottaa tapahtuman keskittimet mukaan hallinta pysyvä tarkistuspisteet vastaanottamisessa tapahtumia ja näiden tapahtuman keskittimet niistä rinnakkain. EventProcessorHost avulla voit jakaa tapahtumien yli useita vastaanottajia, myös silloin, kun eri solmujen ylläpidettävä. Tässä esimerkissä näytetään, miten voit käyttää EventProcessorHost yksittäisen vastaanottajan.

###<a name="create-a-storage-account"></a>Tallennustilan tilin luominen

Jotta voit käyttää EventProcessorHost, sinulla on oltava [Azure-tallennustilan tilin][]:

1. [Azure perinteinen portal][]Kirjaudu ja valitse näytön alareunassa **Uusi** .

2. **Data Services**-ja sitten **tallennustilan**ja sitten **Nopea luominen**ja kirjoita sitten tallennustilan tilin nimi. Valitse haluamasi alue ja valitse sitten **Luo tallennustilan tili**.

    ![][11]

3. Valitse juuri luomasi tallennustilan-tili ja valitse sitten **Hallitse pikanäppäimet**:

    ![][12]

    Kopioi ensisijainen pikanäppäin käyttäminen jäljempänä tässä opetusohjelmassa.

###<a name="create-a-java-project-using-the-eventprocessor-host"></a>Käyttämällä EventProcessor Host Java projektin luominen

Tapahtuman keskittimien Java asiakas-kirjasto on käytettävissä maven-testi projekteista [Maven-testi keskitettyyn säilöön][Maven Package], ja niihin voidaan viitata käyttämällä sisällä maven-testi projektitiedoston riippuvuuden seuraava ilmoitus:    

``` XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs-eph</artifactId>
    <version>{VERSION}</version>
</dependency>
```
 
Muodosta ympäristöissä eri tyyppisten erikseen saat uusimmat julkaistut PURKKI-tiedostoja [Maven-testi keskitettyyn säilöön] [ Maven Package] tai [GitHub release jakauman-kohtaa](https://github.com/Azure/azure-event-hubs/releases).  

1. Seuraavassa esimerkissä Luo uuden maven-testi projektin console/shell-sovelluksen ensin tuttuja Java-kehitysympäristö. Luokan kutsutaan ```ErrorNotificationHandler```.     

    ``` Java
    import java.util.function.Consumer;
    import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;

    public class ErrorNotificationHandler implements Consumer<ExceptionReceivedEventArgs>
    {
        @Override
        public void accept(ExceptionReceivedEventArgs t)
        {
            System.out.println("SAMPLE: Host " + t.getHostname() + " received general error notification during " + t.getAction() + ": " + t.getException().toString());
        }
    }
    ```

2. Seuraava koodi avulla voit luoda uuden luokan nimi ```EventProcessor```.

    ```Java
    import com.microsoft.azure.eventhubs.EventData;
    import com.microsoft.azure.eventprocessorhost.CloseReason;
    import com.microsoft.azure.eventprocessorhost.IEventProcessor;
    import com.microsoft.azure.eventprocessorhost.PartitionContext;

    public class EventProcessor implements IEventProcessor
    {
        private int checkpointBatchingCount = 0;

        @Override
        public void onOpen(PartitionContext context) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is opening");
        }

        @Override
        public void onClose(PartitionContext context, CloseReason reason) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is closing for reason " + reason.toString());
        }
        
        @Override
        public void onError(PartitionContext context, Throwable error)
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " onError: " + error.toString());
        }

        @Override
        public void onEvents(PartitionContext context, Iterable<EventData> messages) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " got message batch");
            int messageCount = 0;
            for (EventData data : messages)
            {
                System.out.println("SAMPLE (" + context.getPartitionId() + "," + data.getSystemProperties().getOffset() + "," +
                        data.getSystemProperties().getSequenceNumber() + "): " + new String(data.getBody(), "UTF8"));
                messageCount++;
                
                this.checkpointBatchingCount++;
                if ((checkpointBatchingCount % 5) == 0)
                {
                    System.out.println("SAMPLE: Partition " + context.getPartitionId() + " checkpointing at " +
                        data.getSystemProperties().getOffset() + "," + data.getSystemProperties().getSequenceNumber());
                    context.checkpoint(data);
                }
            }
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " batch size was " + messageCount + " for host " + context.getOwner());
        }
    }
    ```

3. Luoda lopullinen luokan kutsutaan ```EventProcessorSample```, käyttämällä seuraava koodi.

    ```Java
    import com.microsoft.azure.eventprocessorhost.*;
    import com.microsoft.azure.servicebus.ConnectionStringBuilder;
    import com.microsoft.azure.eventhubs.EventData;

    public class EventProcessorSample
    {
        public static void main(String args[])
        {
            final String consumerGroupName = "$Default";
            final String namespaceName = "----ServiceBusNamespaceName-----";
            final String eventHubName = "----EventHubName-----";
            final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
            final String sasKey = "---SharedAccessSignatureKey----";

            final String storageAccountName = "---StorageAccountName----";
            final String storageAccountKey = "---StorageAccountKey----";
            final String storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=" + storageAccountName + ";AccountKey=" + storageAccountKey;
            
            ConnectionStringBuilder eventHubConnectionString = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
            
            EventProcessorHost host = new EventProcessorHost(eventHubName, consumerGroupName, eventHubConnectionString.toString(), storageConnectionString);
            
            System.out.println("Registering host named " + host.getHostName());
            EventProcessorOptions options = new EventProcessorOptions();
            options.setExceptionNotification(new ErrorNotificationHandler());
            try
            {
                host.registerEventProcessor(EventProcessor.class, options).get();
            }
            catch (Exception e)
            {
                System.out.print("Failure while registering: ");
                if (e instanceof ExecutionException)
                {
                    Throwable inner = e.getCause();
                    System.out.println(inner.toString());
                }
                else
                {
                    System.out.println(e.toString());
                }
            }

            System.out.println("Press enter to stop");
            try
            {
                System.in.read();
                host.unregisterEventProcessor();
                
                System.out.println("Calling forceExecutorShutdown");
                EventProcessorHost.forceExecutorShutdown(120);
            }
            catch(Exception e)
            {
                System.out.println(e.toString());
                e.printStackTrace();
            }
            
            System.out.println("End of sample");
        }
    }
    ```

4. Korvaa käytetään, kun olet luonut sen tapahtuman ja tallennustilaa tilin arvot seuraaviin kenttiin.

    ``` Java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";

    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";

    final String storageAccountName = "---StorageAccountName----"
    final String storageAccountKey = "---StorageAccountKey----";
    ```

> [AZURE.NOTE] Tässä opetusohjelmassa käytetään EventProcessorHost yksittäisen esiintymän. Suurentaa siirtonopeuden, on suositeltavaa, että suoritat EventProcessorHost useita kertoja. Tällöin eri esiintymien automaattisesti toistensa kanssa haluat ladata saldo vastaanotettu tapahtumien järjestämiseen. Jos haluat useita vastaanottajia kunkin prosessin *kaikki* tapahtumat, sinun on käytettävä **ConsumerGroup** käsitteestä. Kun vastaanotti tapahtumat eri tietokoneissa, voi olla hyödyllistä nimet EventProcessorHost esiintymien koneet (tai roolit) on määritettävä johon ne on otettu käyttöön.

<!-- Links -->
[Event Hubs overview]: ../articles/event-hubs/event-hubs-overview.md
[Azure-tallennustilan tilin]: ../articles/storage/storage-create-storage-account.md
[Azure perinteinen portal]: http://manage.windowsazure.com
[Maven Package]: https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22

<!-- Images -->
[11]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp2.png
[12]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp3.png

