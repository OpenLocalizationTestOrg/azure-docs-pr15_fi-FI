<properties 
    pageTitle="Luo sovelluksia, jotka käyttävät palvelun Bus aiheet ja tilaukset | Microsoft Azure"
    description="Johdanto Julkaise-tilaa palvelun Bus aiheet ja tilaukset tarjoamia ominaisuuksia."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/04/2016"
    ms.author="sethm" />

# <a name="create-applications-that-use-service-bus-topics-and-subscriptions"></a>Luo sovelluksia, jotka käyttävät palvelun Bus aiheet ja tilaukset

Azure palvelun Bus tukee luotettava Viestijono tekniikkaan pilvipohjainen sekä viesti-middleware ja kestävät Julkaise ja tilaa messaging. Tässä artikkelissa muodostaa antamien [Luo](service-bus-create-queues.md) sovelluksissa, jotka käyttävät palvelun Bus olevien tietojen ja tarjoaa esittely palvelun Bus aiheet tarjoamia Julkaise ja tilaa ominaisuuksia.

## <a name="evolving-retail-scenario"></a>Kehittyville jälleenmyynti-skenaario

Tässä artikkelissa on edelleen käyttää [Luo sovelluksia, jotka käyttävät palvelun Bus olevien](service-bus-create-queues.md)jälleenmyynti-skenaario. Et voi peruuttaa, myyntitiedot-yksittäisen kohdan myynti (Myyntipiste) päätteitä on reititetty varasto-hallintajärjestelmään joka käyttää tietoja milloin stock on täydennystä. Kunkin kassapääte ilmoittaa sen myyntitiedot lähettämällä viestejä **DataCollectionQueue** -jonossa, missä ne kunnes ne ovat vastaanottanut varaston hallintajärjestelmän, kuten kuvassa:

![Palvelun Bus 1](./media/service-bus-create-topics-subscriptions/IC657161.gif)

Muodostaen Tämä skenaario, järjestelmän on lisätty uusi pyyntö: myymälän omistaja haluaa voi seurata, miten säilön toimii reaaliajassa.

Tämä vaatimus verkko-osoitteissa, järjestelmä on "napauttamalla" myyntitiedot virta käytöstä. Haluamme kunkin viestiä Kassapäätteet varaston hallintajärjestelmän ennen lähetetään edelleen, mutta haluamme kustakin viestistä, joka esittää Raporttinäkymät-ikkunan Näytä myymälän omistaja Käytämme kopio.

Tilanteen, esimerkiksi, jossa tarvitset jokaisen viestin kulutettu useita osapuolten voit palvelun Bus *aiheet*. Julkaise ja tilaa kuviota, jossa julkaistun viesti otetaan käyttöön yhden tai useamman tilauksia aiheen rekisteröityjä ohjeaiheissa. Sen sijaan kanssa olevien viesti vastaanotetaan yksittäisen kuluttaja.

Viestit lähetetään aiheen samalla tavalla kuin lähetetään jonossa. Kuitenkin viestejä ei vastaanoteta aiheesta suoraan; ne on saatu tilaukset. Voit ajatella aiheen tilannut kuin virtual jono, joka vastaanottaa viesteistä, jotka lähetetään ohjeaiheen. Viestejä vastaanotetaan tilauksesta samalla tavalla kuin ne on saatu jonossa.

Palaaminen jälleenmyynti-skenaario jonossa korvataan aiheen ja tilauksen on lisätty, jonka varaston hallinta järjestelmäosan voit. Järjestelmä näkyy nyt seuraavasti:

![Palvelun Bus 2](./media/service-bus-create-topics-subscriptions/IC657165.gif)

Määritysten tähän suorittaa samannimisen edellisen jonon perustuva rakennetta. Aiheen lähetettyihin viesteihin reititetään, **Varasto** -tilaus, josta **Varaston hallintajärjestelmän** käyttää niitä.

Tukemiseksi hallinnan Raporttinäkymää luodaan toinen tilauksen aiheeseen, tässä esitetyllä tavalla:

![Palvelun Bus 3](./media/service-bus-create-topics-subscriptions/IC657166.gif)

Tässä määrityksessä kanssa Kassapäätteet jokaisen viestin otetaan käyttöön **raporttinäkymät-ikkunan** ja **Varasto** -tilauksia.

## <a name="show-me-the-code"></a>Näytä koodi

[Luo sovelluksia, jotka käyttävät palvelun Bus olevien](service-bus-create-queues.md) on artikkelissa käsitellään Azure-tilin ja luo palvelun nimitila. Sovelluksen viitattava käyttämään palvelua Bus nimitilan palvelun Bus kokoonpanossa, erityisesti Microsoft.ServiceBus.dll. Helpoin tapa viitata palvelun Bus riippuvuudet on Asenna Service Bus [Nuget paketti](https://www.nuget.org/packages/WindowsAzure.ServiceBus/). Voit etsiä myös kokoonpanon Azure SDK osana. Lataaminen on saatavilla kohdassa [Azure SDK lataussivulle](https://azure.microsoft.com/downloads/).

### <a name="create-the-topic-and-subscriptions"></a>Luo aihe ja tilaukset

Palvelun Bus messaging kohteiden (olevien ja Julkaise ja tilaa aiheet) hallintatoiminnot suoritetaan [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) -luokan kautta. Tarvittavat tunnistetiedot tarvitaan, jotta he voivat luoda [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) -esiintymän tietyn nimitilan. Palvelun Bus käyttää [Jaettu Access allekirjoitus (SAS)](service-bus-sas-overview.md) perusteella suojausmalli. [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) luokan edustaa suojaustunnuksen toimittaja joidenkin tunnetun suojaustunnuksen tarjoajien palvelua, joka palauttaa valmiin factory menetelmillä. Esitellään pidä SAS tunnistetietojen [CreateSharedAccessSignatureTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.createsharedaccesssignaturetokenprovider.aspx) menetelmän avulla. [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) esiintymän muodostetaan sitten perusosoitteen palvelun Bus nimitilan ja suojaustunnuksen tarjoajan kanssa.

[NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) luokan sisältää tapaa luoda, Luetteloi ja poistaa tekstiviesti kohteiden. Tunnus, jolla on kuvassa näkyy, kuinka [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) esiintymä luodaan ja käytetään **DataCollectionTopic** aiheen luominen.

```
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", "test-blog", string.Empty);
string name = "RootManageSharedAccessKey";
string key = "abcdefghijklmopqrstuvwxyz";
     
TokenProvider tokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(name, key);
NamespaceManager namespaceManager = new NamespaceManager(uri, tokenProvider);
 
namespaceManager.CreateTopic("DataCollectionTopic");
```

Huomaa, että Osastollasi [CreateTopic](https://msdn.microsoft.com/library/azure/hh293080.aspx) menetelmän, joiden avulla voit määrittää ominaisuuksia aiheen on. Voit esimerkiksi määrittää--elinaika (TTL) oletusarvo aiheen lähetettyihin viesteihin. Lisää seuraavaksi **varaston** ja **Dashboard** -tilauksia.

```
namespaceManager.CreateSubscription("DataCollectionTopic", "Inventory");
namespaceManager.CreateSubscription("DataCollectionTopic", "Dashboard");
```

### <a name="send-messages-to-the-topic"></a>Viestien lähettäminen aihe

Suorituksenaikainen toiminnoille palvelun Bus kohteiden; esimerkiksi lähettää ja vastaanottaa viestejä, sovellus on luotava [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) objekti. Kaltaisilta [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) luokan [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) esiintymä luodaan perusosoitteen palvelun nimitila ja suojaustunnuksen toimittaja.

```
MessagingFactory factory = MessagingFactory.Create(uri, tokenProvider);
```

Viestit ja vastaanotti palvelun Bus aiheet ovat [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) luokan esiintymät. Tähän luokkaan koostuu joukosta vakio-ominaisuuksia (esimerkiksi [otsikon](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) ja [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)) sanasto, jota käytetään sovelluksen ominaisuuksien ja haluamaansa sovelluksen tietoja tekstiosaan. Sovellus määrittää tekstiin mukaan kulkee (Seuraavassa esimerkissä välittää **SalesData** -objekti, joka vastaa myyntitiedot kassapääte) voi sarjoittaa objektissa, joka käyttää [System.Runtime.Serialization](https://msdn.microsoft.com/library/azure/system.runtime.serialization.datacontractserializer.aspx) onnistu objekti. Vaihtoehtoisesti voit toimittaa [Stream](https://msdn.microsoft.com/library/azure/system.io.stream.aspx) -objektin.

```
BrokeredMessage bm = new BrokeredMessage(salesData);
bm.Label = "SalesReport";
bm.Properties["StoreName"] = "Redmond";
bm.Properties["MachineID"] = "POS_1";
```

Helpoin tapa lähettää viestejä aihe on [CreateMessageSender](https://msdn.microsoft.com/library/azure/hh322659.aspx) avulla voit luoda [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx) objekti suoraan [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) esiintymä.

```
MessageSender sender = factory.CreateMessageSender("DataCollectionTopic");
sender.Send(bm);
```

### <a name="receive-messages-from-a-subscription"></a>Vastaanottaa viestejä tilaukseen

Samalla tavalla kuin käytettäessä olevien, vastaanottaa viestejä, voit käyttää [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) objekti, joka luodaan suoraan käyttämällä [CreateMessageReceiver](https://msdn.microsoft.com/library/azure/hh322642.aspx) [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) tilauksesta. Voit käyttää yhtä kahdesta eri vastaanottaa tilat (**ReceiveAndDelete** ja **PeekLock**), kuten edellä [, jotka käyttävät palvelun Bus olevien luominen](service-bus-create-queues.md)-sovelluksissa.

Huomaa, että kun luot [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) tilaukset, *entityPath* -parametri on lomakkeen `topicPath/subscriptions/subscriptionName`. Tämän vuoksi luomaan [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) **varaston** tilauksen **DataCollectionTopic** aiheen *entityPath* on määritettävä `DataCollectionTopic/subscriptions/Inventory`. Koodi näkyy seuraavasti:

```
MessageReceiver receiver = factory.CreateMessageReceiver("DataCollectionTopic/subscriptions/Inventory");
BrokeredMessage receivedMessage = receiver.Receive();
try
{
    ProcessMessage(receivedMessage);
    receivedMessage.Complete();
}
catch (Exception e)
{
    receivedMessage.Abandon();
}
```

## <a name="subscription-filters"></a>Tilauksen suodattimet

Tähän mennessä tässä skenaariossa aiheeseen lähetetyt viestit ovat saataville kaikki rekisteröity tilaukset. Avaimen lause "otetaan käyttöön." Kun palvelun Bus tilaukset näkyvissä aiheeseen lähetetyt viestit, voit kopioida vain ne viestit alijoukon virtual tilauksen jonossa. Suorittaa tilauksen *suodattimien*avulla. Kun luot tilauksen, voit antaa suodatin-lausekkeen SQL92 tyyli-lause, joka toimii viestin järjestelmän ominaisuuksia (esimerkiksi [otsikko](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx)) ja sovelluksen ominaisuudet, kuten edellisessä esimerkissä **StoreName** ominaisuudet-lomakkeessa.

Kehittyville kuvataan Tämä skenaario toisen säilö on Microsoftin jälleenmyynti-skenaario lisättäväksi. Kaikkien Kassapäätteet sekä stores-myyntitiedot yhä reititetään keskitetystä varaston hallintajärjestelmään, mutta dashboard-työkalulla kaupan esimies on vain kyseisen myymälän suorituskyky käyttämällä. Voit käyttää tätä suodattaminen tilauksen. Huomaa, että Kassapäätteet julkaista viestejä, kun ne ominaisuuden **StoreName** sovelluksen viestin. Valita esimerkiksi **Redmond** ja **Seattle**stores Kassapäätteet, Redmond tallentaa **StoreName** yhtä suuri kuin **Redmond**, taas Seattle kaupan Kassapäätteet käyttävät **StoreName** myyntitiedot niiden viestien yhtä suuri kuin **Seattle**aikaleiman. Vain Redmond säilön kaupan esimies haluaa tarkastella sen Kassapäätteet tiedot. Järjestelmä näkyy seuraavasti:

![Palvelun Bus4](./media/service-bus-create-topics-subscriptions/IC657167.gif)

Määrittämään reitityksen voit luoda **raporttinäkymät-ikkunan** tilauksen seuraavasti:

```
SqlFilter dashboardFilter = new SqlFilter("StoreName = 'Redmond'");
namespaceManager.CreateSubscription("DataCollectionTopic", "Dashboard", dashboardFilter);
```

**Raporttinäkymät-ikkunan** tilauksen virtual jonossa kopioidaan vain ne viestit, joiden **StoreName** -ominaisuuden arvoksi **Redmond** tilauksen tämän suodattimen kanssa. On paljon tilauksen suodattaminen, mutta. Sovellusten voi olla useita tilauskohtaisten lisäksi mahdollisuus viestin ominaisuuksien muokkaaminen, kun se kulkee lisääminen tilaukseen virtual jonon suodattimen sääntöjä.

## <a name="summary"></a>Yhteenveto

Kaikki syytä käyttää queuing kuvattu [luoda sovelluksia, jotka käyttävät palvelun Bus olevien](service-bus-create-queues.md) koskevat myös aiheista, erityisesti:

- Ajallinen irtautus – viestin tuottajat ja kuluttajille ei tarvitse olla online-tilassa samanaikaisesti.

- Kuormituksen tasaus – Lataa päät ovat tasoitetun ottaminen käyttöön vievää sovellusten valmisteltu keskimääräinen kuormituksen sijaan piikin kuormituksen aiheen mukaan.

- Kuormituksen tasaaminen – kaltaisilta jono voi olla useita kilpailevien kuluttajille Kuuntele yhden tilauksen jokaisen viestin yhden kuluttajille, jolloin kuormituksen tasaamisen jaettu.

- Irtoaa kytkentä – voit kehittävät tekstiviesti verkon vaikuttamatta aiemmin päätepisteet; esimerkiksi Tilaukset lisääminen tai muuttaminen suodattimia, jotta uusi kuluttajille aiheen.

## <a name="next-steps"></a>Seuraavat vaiheet

Saat lisätietoja olevien käyttämisestä POS jälleenmyynti skenaariota [, jotka käyttävät palvelun Bus olevien sovellusten luominen](service-bus-create-queues.md) .