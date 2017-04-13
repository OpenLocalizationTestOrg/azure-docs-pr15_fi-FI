<properties 
    pageTitle="Yleistä Azure tapahtuman keskittimet ohjelmointirajapinnan | Microsoft Azure"
    description="Yhteenveto joistakin avaimen tapahtuman keskittimet .NET-asiakasohjelman API."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/16/2016"
    ms.author="sethm" />

# <a name="event-hubs-api-overview"></a>Tapahtuman keskittimet API yleiskatsaus

Tässä artikkelissa on yhteenveto joistakin tapahtuman keskittimet .NET asiakkaan ohjelmointirajapinnan avain. Jakautuvat kahteen luokkaan: hallinta ja näyttää ajonaikaisen ohjelmointirajapinnan. Suorituksenaikainen ohjelmointirajapinnan koostuvat kaikki toiminnot, voit lähettää ja vastaanottaa viestin tarvitaan. Hallintatoiminnot avulla voit hallita tapahtuman keskittimet toimija-tilan luominen, päivittäminen ja poistaminen kohteita.

Valvonnan tilanteissa välisenä ajankohtana, hallinta ja suorituksenaikainen. Saat ohjeet .NET-API- [Palvelun Bus .NET](https://msdn.microsoft.com/library/azure/mt419900.aspx) ja [EventProcessorHost API](https://msdn.microsoft.com/library/azure/mt445521.aspx) -viittaukset.

## <a name="management-apis"></a>Hallinnan API

Voit suorittaa seuraavat hallintatoiminnot, on oltava tapahtuman keskittimet nimitilan käyttöoikeuksien **hallinta** :

### <a name="create"></a>Luo

```
// Create the Event Hub
EventHubDescription ehd = new EventHubDescription(eventHubName);
ehd.PartitionCount = SampleManager.numPartitions;
namespaceManager.CreateEventHubAsync(ehd).Wait();
```

### <a name="update"></a>Päivitys

```
EventHubDescription ehd = await namespaceManager.GetEventHubAsync(eventHubName);

// Create a customer SAS rule with Manage permissions
ehd.UserMetadata = "Some updated info";
string ruleName = "myeventhubmanagerule";
string ruleKey = SharedAccessAuthorizationRule.GenerateRandomKey();
ehd.Authorization.Add(new SharedAccessAuthorizationRule(ruleName, ruleKey, new AccessRights[] {AccessRights.Manage, AccessRights.Listen, AccessRights.Send} )); 
namespaceManager.UpdateEventHubAsync(ehd).Wait();
```

### <a name="delete"></a>Poista

```
namespaceManager.DeleteEventHubAsync("Event Hub name").Wait();
```

## <a name="run-time-apis"></a>Suorituksenaikainen API

### <a name="create-publisher"></a>Luo julkaisija

```
// EventHubClient model (uses implicit factory instance, so all links on same connection)
EventHubClient eventHubClient = EventHubClient.Create("Event Hub name");
```

### <a name="publish-message"></a>Viestin julkaiseminen

```
// Create the device/temperature metric
MetricEvent info = new MetricEvent() { DeviceId = random.Next(SampleManager.NumDevices), Temperature = random.Next(100) };
EventData data = new EventData(new byte[10]); // Byte array
EventData data = new EventData(Stream); // Stream 
EventData data = new EventData(info, serializer) //Object and serializer 
    {
       PartitionKey = info.DeviceId.ToString()
    };

// Set user properties if needed
data.Properties.Add("Type", "Telemetry_" + DateTime.Now.ToLongTimeString());

// Send single message async
await client.SendAsync(data);
```

### <a name="create-consumer"></a>Luo kuluttaja

```
// Create the Event Hubs client
EventHubClient eventHubClient = EventHubClient.Create(EventHubName);

// Get the default consumer group
EventHubConsumerGroup defaultConsumerGroup = eventHubClient.GetDefaultConsumerGroup();

// All messages
EventHubReceiver consumer = await defaultConsumerGroup.CreateReceiverAsync(shardId: index);

// From one day ago
EventHubReceiver consumer = await defaultConsumerGroup.CreateReceiverAsync(shardId: index, startingDateTimeUtc:DateTime.Now.AddDays(-1));
                        
// From specific offset, -1 means oldest
EventHubReceiver consumer = await defaultConsumerGroup.CreateReceiverAsync(shardId: index,startingOffset:-1); 
```

### <a name="consume-message"></a>Kuluttavat viesti

```
var message = await consumer.ReceiveAsync();

// Provide a serializer
var info = message.GetBody<Type>(Serializer)
                                    
// Get a byte[]
var info = message.GetBytes(); 
msg = UnicodeEncoding.UTF8.GetString(info);
```

## <a name="event-processor-host-apis"></a>Tapahtuman suoritin host API

Nämä ohjelmointirajapinnan on vikasietoisuudelle työntekijä prosesseja, jotka voivat poistuvat käytöstä, jakamalla shards käytettävissä työntekijöiden yli.

```
// Checkpointing is done within the SimpleEventProcessor and on a per-consumerGroup per-partition basis, workers resume from where they last left off.
// Use the EventData.Offset value for checkpointing yourself, this value is unique per partition.

string eventHubConnectionString = System.Configuration.ConfigurationManager.AppSettings["Microsoft.ServiceBus.ConnectionString"];
string blobConnectionString = System.Configuration.ConfigurationManager.AppSettings["AzureStorageConnectionString"]; // Required for checkpoint/state

EventHubDescription eventHubDescription = new EventHubDescription(EventHubName);
EventProcessorHost host = new EventProcessorHost(WorkerName, EventHubName, defaultConsumerGroup.GroupName, eventHubConnectionString, blobConnectionString);
            host.RegisterEventProcessorAsync<SimpleEventProcessor>();

// To close
host.UnregisterEventProcessorAsync().Wait();   
```

[IEventProcessor](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ieventprocessor.aspx) -liittymän määritellään seuraavasti:

```
public class SimpleEventProcessor : IEventProcessor
{
    IDictionary<string, string> map;
    PartitionContext partitionContext;

    public SimpleEventProcessor()
    {
        this.map = new Dictionary<string, string>();
    }

    public Task OpenAsync(PartitionContext context)
    {
        this.partitionContext = context;

        return Task.FromResult<object>(null);
    }

    public async Task ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
    {
        foreach (EventData message in messages)
        {
            Process messages here
        }
        
        // Checkpoint when appropriate
        await context.CheckpointAsync();

    }


    public async Task CloseAsync(PartitionContext context, CloseReason reason)
    {
        if (reason == CloseReason.Shutdown)
        {
            await context.CheckpointAsync();
        }
    }
}
```

## <a name="next-steps"></a>Seuraavat vaiheet

Saat lisätietoja tapahtuman keskittimet skenaariot seuraavasta seuraavissa linkeissä:

- [Mikä on Azure tapahtuman keskittimet?](event-hubs-what-is-event-hubs.md)
- [Tapahtuman keskittimet yleiskatsaus](event-hubs-overview.md)
- [Tapahtuman keskittimet ohjelmointi opas](event-hubs-programming-guide.md)
- [Tapahtuman keskittimet MALLIKOODEJA](http://code.msdn.microsoft.com/site/search?query=event hub&f[0].Value=event hubs&f[0].Type=SearchText&ac=5)

.NET-Ohjelmointirajapinnan viittaukset eivät tähän:

- [Palvelun Bus ja tapahtuman keskittimet .NET API.](https://msdn.microsoft.com/library/azure/mt419900.aspx)
- [Tapahtuman suoritin host API-viittaus](https://msdn.microsoft.com/library/azure/mt445521.aspx)
