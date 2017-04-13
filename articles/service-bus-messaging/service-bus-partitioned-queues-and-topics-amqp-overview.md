<properties 
    pageTitle="Palvelun Bus AMQP 1.0 tuki osioitu olevien ja aiheet | Microsoft Azure" 
    description="Opi käyttämään Advanced viestin Queuing Protocol (AMQP) 1.0 kanssa palvelun Bus osioitu olevien ja ohjeita." 
    services="service-bus" 
    documentationCenter=".net" 
    authors="hillaryc" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="10/14/2016" 
    ms.author="hillaryc;sethm"/>

# <a name="amqp-10-support-for-service-bus-partitioned-queues-and-topics"></a>Palvelun Bus osioitu olevien ja aiheet AMQP 1.0 tuki 

Azure palvelun Bus tukee nyt Advanced viestin Queuing protokolla (**AMQP**) 1.0 for palvelun Bus **osioitu olevien ja aiheet.**

Avaa-standardin viestin queuing protokolla, jonka avulla voit kehittää Office kaikissa ympäristöissä sovellusten ohjelmoinnin kielten on **AMQP** . Yleisiä tietoja AMQP-palvelun Bus tuesta on kohdassa [AMQP 1.0 palvelun Bus tuki](service-bus-amqp-overview.md).

**Partitioned olevien ja aiheet**, eli *osioitua kohteita*, tarjoavat suurempi käytettävyyden, luotettavuutta ja siirtonopeuden kuin tavalliseen osittamattoman olevien ja ohjeita. Lisätietoja osioitua kohteiden artikkelissa [Osioitua Messaging kohteiden](service-bus-partitioning.md).

AMQP 1.0 lisäämällä kuin protokolla yhteydenpito osioitua olevien ja aiheet avulla voit luoda yhteentoimivien sovelluksia, jotka voivat hyödyntää suurempi käytettävyys luotettavuutta, ja koko tarjoaman palvelun Bus osioitu kohteiden.

Artikkelissa yksityiskohtaiset wire tason AMQP 1.0 protokolla-oppaan, jossa kerrotaan, miten palvelun Bus toteuttaa ja muodostaa OASIS AMQP tekniset tarkentamisesta, [AMQP 1.0 Azure palvelun Bus ja tapahtuman keskittimet protokolla opas](service-bus-amqp-protocol-guide.md).    

## <a name="use-amqp-with-partitioned-queues"></a>Osioitu olevien AMQP käyttäminen

Olevien on hyötyä skenaariot, jotka tarvitsevat ajallinen irtautus, kuormituksen tasaamisen, kuormituksen tasaamisen ja irtoaa kytkentä. Jono julkaisijat lähettää viestejä jonossa ja kuluttajille vastaanottaa viestejä jonossa tilanteissa, joissa viestin vain voi vastaanottaa kerran. Hyvä esimerkki tämä on myyntipisteen päätteitä julkaiseminen tietojen jonon sijaan suoraan varaston hallintajärjestelmän, jossa Varastojärjestelmä. Varaston hallintajärjestelmän käyttämän tiedot sitten voit hallita varaston täydennyksen milloin tahansa. Tässä on monia etuja, mukaan lukien varaston hallintajärjestelmän ei tarvitse olla aina online-tilassa. Saat lisätietoja palvelun Bus olevien [Luo sovelluksia, jotka käyttävät palvelun Bus olevien](service-bus-create-queues.md). 

Osioitu jonon edelleen kasvaa käytettävyyden, luotettavuutta ja siirtonopeuden sovellusten, kun nämä olevien on osioitu eri viestin vakuutuksenvälittäjän ja tekstiviesti stores välillä.     

### <a name="create-partitioned-queues"></a>Luo osioitua olevien

Voit luoda osioitua jonon [Azure perinteinen portal][] ja palvelun Bus SDK-paketissa. Luoda osioitua jonon, asettamalla [EnablePartitioning](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx) -ominaisuuden arvoksi **true** [QueueDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx) esiintymän. Seuraava koodi esitetään, kuinka voit luoda osioitua jonon palvelun Bus SDK: N avulla. 
 
```
// Create partitioned queue
var nm = NamespaceManager.CreateFromConnectionString(myConnectionString);
var queueDescription = new QueueDescription("myQueue");
queueDescription.EnablePartitioning = true;
nm.CreateQueue(queueDescription);
```

### <a name="send-and-receive-messages-using-amqp"></a>Käyttämällä AMQP viestien lähettäminen ja vastaanottaminen

Voit lähettää viestejä ja vastaanottaa viestejä osioitua jonon käyttäen AMQP protokolla, määrittämällä [TransportType](https://msdn.microsoft.com/library/azure/microsoft.servicebus.servicebusconnectionstringbuilder.transporttype.aspx) -ominaisuuden arvoksi [TransportType.Amqp](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.transporttype.aspx) seuraava koodi esitetyllä tavalla.  

```
// Sending and receiving messages to and from a queue
var myConnectionStringBuilder = new ServiceBusConnectionStringBuilder(myConnectionString);
myConnectionStringBuilder.TransportType = TransportType.Amqp;
string amqpConnectionString = myConnectionStringBuilder.ToString();
var queueClient = QueueClient.CreateFromConnectionString(amqpConnectionString, "myQueue");

BrokeredMessage message = new BrokeredMessage("Hello AMQP");
Console.WriteLine("Sending message {0}...", message.MessageId);
queueClient.Send(message);

var receivedMessage = queueClient.Receive();
Console.WriteLine("Received message: {0}", receivedMessage.GetBody<string>());
receivedMessage.Complete();
```

## <a name="use-amqp-with-partitioned-topics"></a>AMQP käyttäminen osioitua aiheita

Aiheet ovat käsitteellisesti olevien, mutta aiheet voit lähettää saman viestin kopio useita *tilauksia*. Aiheen julkaisijat lähettää viestejä aihetta ja kuluttajille vastaanottaa viestejä tilaukset. Varaston järjestelmän myyntipisteen skenaariossa päätteitä julkaista tiedot aihetta. Varaston hallintajärjestelmän vastaanottaa viestit sitten tilaus. Lisäksi valvonta järjestelmän saavat saman viestin eri tilauksen. Lisätietoja palvelun Bus aiheet ja tilaukset-kohdassa [Luo sovelluksia, jotka käyttävät palvelun Bus aiheet ja tilaukset](service-bus-create-topics-subscriptions.md). 

Olevien, jossa edelleen osioitua aiheet kasvaa käytettävyyden, luotettavuutta ja siirtonopeuden sovellusten ohjeaiheet ja niiden tilaukset osioitu eri viestin vakuutuksenvälittäjän ja tekstiviesti stores välillä. 

### <a name="create-partitioned-topics"></a>Luo osioitua aiheita

Voit luoda [Azure perinteinen portal][] ja palvelun Bus SDK osioitua aiheen. Osioitu aiheen luominen asettamalla [EnablePartitioning](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.enablepartitioning.aspx) -ominaisuuden arvoksi **true** [TopicDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.aspx) esiintymän. Seuraava koodi näkyy luominen käyttämällä palvelun Bus SDK osioitua aiheen.
    
```
// Create partitioned topic
var nm = NamespaceManager.CreateFromConnectionString(myConnectionString);
var topicDescription = new TopicDescription("myTopic");
topicDescription.EnablePartitioning = true;
nm.CreateTopic(topicDescription);

var subscriptionDescription = new SubscriptionDescription("myTopic", "mySubscription");
nm.CreateSubscription(subscriptionDescription);
```

### <a name="send-and-receive-messages-using-amqp"></a>Käyttämällä AMQP viestien lähettäminen ja vastaanottaminen

Voit lähettää viestejä ja vastaanottaa viestejä käyttäen AMQP protokolla, määrittämällä [TransportType](https://msdn.microsoft.com/library/azure/microsoft.servicebus.servicebusconnectionstringbuilder.transporttype.aspx) -ominaisuuden arvoksi [TransportType.Amqp](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.transporttype.aspx), kuten seuraava koodi osioitua aiheen tilauksesta.  

```
// Sending and receiving messages to a topic and from a subscription
var myConnectionStringBuilder = new ServiceBusConnectionStringBuilder(myConnectionString);
myConnectionStringBuilder.TransportType = TransportType.Amqp;
string amqpConnectionString = myConnectionStringBuilder.ToString();
    
var topicClient = TopicClient.CreateFromConnectionString(amqpConnectionString, "myTopic");
BrokeredMessage message = new BrokeredMessage("Hello AMQP");
Console.WriteLine("Sending message {0}...", message.MessageId);
topicClient.Send(message);
    
var subcriptionClient = SubscriptionClient.CreateFromConnectionString(amqpConnectionString, "myTopic", "mySubscription");
var receivedMessage = subcriptionClient.Receive();
Console.WriteLine("Received message: {0}", receivedMessage.GetBody<string>());
receivedMessage.Complete();
```

## <a name="next-steps"></a>Seuraavat vaiheet

Katso lisätietoja osioitua tekstiviesti kohteita sekä AMQP seuraavat lisätiedot.

*    [Osioitu tekstiviesti yksiköt](service-bus-partitioning.md)
*    [OASIS Message Queuing Protocol (AMQP) Version 1.0 Lisäasetukset](http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf)
*    [Palvelun Bus AMQP 1.0-tuki](service-bus-amqp-overview.md)
*    [AMQP 1.0 Azure palvelun Bus ja tapahtuman keskittimet protokolla-oppaasta](service-bus-amqp-protocol-guide.md)
*    [Palvelun Bus ja AMQP 1.0 Java viestin Service (JMS) Ohjelmointirajapinnan käyttäminen](service-bus-java-how-to-use-jms-api-amqp.md)
*    [Voit käyttää AMQP 1.0 palvelun Bus .NET-Ohjelmointirajapinta](service-bus-dotnet-advanced-message-queuing.md)

[Azure perinteinen portal]: http://manage.windowsazure.com
