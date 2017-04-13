<properties 
    pageTitle="Palvelun Bus tapahtumat | Microsoft Azure" 
    description="Yleisiä tietoja Azure palvelun Bus käytettäviä jakamattomia tapahtumia ja Lähetä kautta" 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na" 
    ms.date="10/04/2016"
    ms.author="clemensv;sethm"/>

# <a name="overview-of-service-bus-transaction-processing"></a>Yleistä palvelun Bus tapahtumien käsittely

Tässä artikkelissa käsitellään Azure palvelun Bus tapahtuman ominaisuudet. Paljon keskustelun on esitelty [Palvelun Bus otoksen kanssa käytettäviä jakamattomia tapahtumia](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AtomicTransactions). Tässä artikkelissa on rajoitettu, tapahtumienkäsittelyn ja palvelun Bus *Lähetä* -toiminnon ollessa käytettäviä jakamattomia tapahtumia otosten laajempi ja monimutkaisia laajuus.

## <a name="transactions-in-service-bus"></a>Palvelun Bus tapahtumat

[Tapahtuman](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AtomicTransactions#what-are-transactions) ryhmien kahden tai useamman toimintoja yhdessä *suorittamisen laajuus*. Laatu kuten tapahtuman on varmistaa, että kaikki toiminnot-toimintoa annetun ryhmään kuuluvat onnistuu tai epäonnistuu yhdessä. Tällöin tapahtumat toimia yhtenä yksikkönä, jossa viitataan usein nimellä *atomisuuden*. 

Palvelun Bus on tapahtumien viestin broker ja varmistaa, että kaikki sisäinen toiminnot vastaan sen viestin stores tapahtumien eheyden. Viestien siirtäminen viestien [Perille toimittamattomat jonossa](service-bus-dead-letter-queues.md) tai viestien [automaattinen välittäminen](service-bus-auto-forwarding.md) toiseen kohteita, kuten palvelun Bus sisältyy kaikki siirrot ovat tapahtumien. Sellaisenaan, jos palvelun Bus hyväksyy viestin, se on jo tallennettu ja, jossa on järjestysnumero. Tämän jälkeen sisällä palvelun Bus viestin kaikki siirrot ovat koordinoidun toimintojen yksiköt ja kumpaakaan johtaa tappio (lähteen onnistuu ja kohde epäonnistuu) tai monistaminen (lähteen epäonnistuu ja kohde on muodostettu) viestin.

Palvelun Bus tukee ryhmittely toimintoja vastaan yhden tekstiviesti kohteen (jonossa, aiheen, tilauksen) alueessa tapahtuman. Voit lähettää useina viesteinä yksi jono tapahtuman alueella, ja viestit on vain vahvistettu jonon lokiin, kun tapahtuman onnistuu.

## <a name="operations-within-a-transaction-scope"></a>Toimintojen tapahtuman-alueella 

Toiminnot, jotka voidaan suorittaa tapahtuman alueella ovat seuraavat:

- ** [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx), [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx), [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx)**: lähettää, SendAsync, SendBatch, SendBatchAsync 

- **[BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx)**: valmiina, CompleteAsync, Abandon, AbandonAsync, perille toimittamattomien sanomien, DeadletterAsync, myöhemmäksi, DeferAsync, RenewLock, RenewLockAsync 

Saat toiminnot eivät sisälly, koska sen oletetaan, että sovellus hankkii viestien [ReceiveMode.PeekLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) -tilassa sisällä joitakin saavat silmukan tai [OnMessage](https://msdn.microsoft.com/library/azure/dn369601.aspx) kanssa takaisinsoitto ja vain sitten Avaa tapahtuma laajuuden viestin käsittelyn.

(Kokonaan, abandon Perille toimittamattomat myöhemmäksi) viestin poistamisen jälkeen toteutuu tapahtuman yleinen tulos-alueessa, ja se on riippuvainen.

## <a name="transfers-and-send-via"></a>Siirto- ja "Lähetä"

Tapahtumien handover tiedot jonon suorittimen ja sen jälkeen toinen jono käyttöön palvelun Bus tukee *siirrot*. Siirrä-toiminnolla lähettäjän lähettää viestin ensin "siirto jonon" ja siirron jonossa siirtää viestin heti saman tehokkaat siirto käyttöönoton, joka on riippuvainen automaattinen eteenpäin-ominaisuuksien käyttäminen oikeaan kohteeseen jonossa. Viestin pyrkii koskaan siirto-jonossa log siten, että se tulee näkyviin siirron jonossa kuluttajille.

Tämän tapahtumien vain power tulee näennäinen, kun siirto jonossa itse on lähettäjän syötteen viestit lähde. Toisin sanoen palvelun Bus voit siirtää viestin kohteeseen "kautta" siirto-jonossa suorittaessasi täydellinen (tai siirtää myöhemmäksi, tai Perille toimittamattomat) näytettävä sanoma kaikki atomisia kerralla-toimintoa. 

### <a name="see-it-in-code"></a>Katso koodissa

Määrittämään siirrot voit luoda viestin lähettäjälle, joka sopii kohde jonossa siirto jonossa kautta. Sinulla on myös vastaanotin, joka hakee, että sama jonon viestejä. Esimerkki:

```
var sender = this.messagingFactory.CreateMessageSender(destinationQueue, myQueueName);
var receiver = this.messagingFactory.CreateMessageReceiver(myQueueName);
```

Valitse Yksinkertainen tapahtuman käyttää näitä elementtejä, kuten seuraavassa esimerkissä:

```
var msg = receiver.Receive();

using (scope = new TransactionScope())
{
    // Do whatever work is required 

    var newmsg = ... // package the result 

    msg.Complete(); // mark the message as done
    sender.Send(newmsg); // forward the result

    scope.Complete(); // declare the transaction done
} 
```

## <a name="next-steps"></a>Seuraavat vaiheet

Katso lisätietoja palvelun Bus olevien on seuraavissa artikkeleissa:

- [Ketjutus palvelun Bus kohteiden automaattinen välittäminen kanssa](service-bus-auto-forwarding.md)
- [Esimerkki automaattisen eteenpäin](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AutoForward)
- [Palvelun Bus otoksen kanssa käytettäviä jakamattomia tapahtumia](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AtomicTransactions)
- [Azure olevien ja palvelun Bus olevien verrattuna](service-bus-azure-and-service-bus-queues-compared-contrasted.md)
- [Opi käyttämään palvelua Bus olevien](service-bus-dotnet-get-started-with-queues.md)