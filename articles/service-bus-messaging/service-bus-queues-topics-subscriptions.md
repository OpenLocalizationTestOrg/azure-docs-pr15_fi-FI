<properties 
    pageTitle="Palvelun Bus olevien, aiheet ja tilaukset | Microsoft Azure"
    description="Palvelun Bus messaging kohteiden yleiskatsaus."
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
    ms.date="10/14/2016"
    ms.author="sethm" />

# <a name="service-bus-queues-topics-and-subscriptions"></a>Palvelun Bus olevien, aiheet ja tilaukset

Microsoft Azure palvelun Bus tukee pilvipohjainen, viesti--välitysohjelmistoksi tekniikkaan luotettava Viestijono ja kestävät Julkaise ja tilaa messaging. Nämä se sähköpostiviestinnän toimintoja voidaan ajatella erilliset messaging ominaisuudet, joita tuki Julkaise-tilaa, ajallinen irtautus ja kuormituksen skenaarioiden messaging kangasta palvelun Bus. Erilliset viestintä on monia etuja; esimerkiksi asiakkaiden ja palvelinten muodostaa tarpeen mukaan ja suorittaa niiden asynkroninen tavalla.

Tekstiviesti kohteet, jotka muodostavat se sähköpostiviestinnän toimintoja core palvelun Bus ovat olevien, aiheet ja tilaukset ja säännöt ja toiminnot.

## <a name="queues"></a>Olevien

Olevien tarjouksen ensimmäisen-First Out (FIFO) viestin lähettämisen vähintään yksi kilpailevien kuluttajille. Viestien yleensä, odotetaan vastaanotettu ja käsittelee siinä järjestyksessä, jossa ne on lisätty jonossa ja viesti on vastaanotettu ja käsitellä vain yhden viestin kuluttaja vastaanottajia. Tärkein etu käyttämällä olevien on saavuttamiseksi "Ajallinen irtautus" sovelluksen osia. Toisin sanoen tuottajat (lähettäjien) ja kuluttajille (vastaanottajia) ei tarvitse lähettää ja vastaanottaa viestejä samanaikaisesti, koska viestit tallennetaan kuuluminen jonossa. Lisäksi tuottaja ei tarvitse odottaa kuluttaja vastauksessa, jotta voit käsitellä ja viestien lähettäminen edelleen.

Aiheeseen liittyvät etu on "kuormituksen tasaus," tuottajat ja käyttäjät voivat lähettää ja vastaanottaa viestejä on erilainen avulla. Monissa sovelluksissa järjestelmän kuormituksen vaihtelee jaettuina; Kukin työmäärän yksikkö käsittelyaika on yleensä vakio. Intermediating viestin tuottajat ja kuluttajille jonon kanssa tarkoittaa, että vievää sovellus vain valmistellaan voivat käsitellä keskimääräinen kuormituksen piikin kuormituksen sijaan. Jonossa syvyys kasvaa ja sopimuksia kuin saapuvan kuormituksen vaihtelee. Toiminto tallentaa suoraan rahaa infrastruktuurin vastaamisessa sovelluksen lataa tarvittavan määrän osalta. Kun lataus kasvaa, Lisää työntekijä prosessit voidaan lisätä lukea jonon. Kukin viesti käsitellään vain yksi työntekijä prosesseista. Lisäksi tässä salaus puretaan perustuva kuormituksen sallii työntekijä tietokoneet optimaalisen käytön vaikka työntekijä tietokoneet eroavat toisistaan osalta käsittely power kuin ne otetaan viestien etsiminen omia enimmäisnopeus. Tämä kaava on usein termillä "pikaluistelukilpailuissa kuluttaja"-kuvion.

Olevien Keskitaso viestin tuottajat ja kuluttajille sisältävät luontaisen irtoaa kytkentä osien välillä. Koska tuottajat ja käyttäjät eivät tiedä toisiinsa, kuluttaja voi päivittää käyttämättä mitään vaikutusta tuottaja.

Jonon luominen on monivaiheinen prosessi. Voit tehdä palvelun Bus messaging kohteiden (olevien ja aiheet) hallintatoiminnot [Microsoft.ServiceBus.NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) -luokka, joka on rakennettava toimittamalla perusosoitteen palvelun Bus nimitilan ja käyttäjän tunnistetietoja kautta. [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) sisältää tapaa luoda, Luetteloi ja poistaa tekstiviesti kohteita. Kun olet luonut [Microsoft.ServiceBus.TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) objektin SAS nimi ja avain ja palvelun nimitilan hallinta-objekti, Luo jonossa [Microsoft.ServiceBus.NamespaceManager.CreateQueue](https://msdn.microsoft.com/library/azure/hh293157.aspx) menetelmän avulla. Esimerkki:

```
// Create management credentials
TokenProvider credentials = TokenProvider. CreateSharedAccessSignatureTokenProvider(sasKeyName,sasKeyValue);
// Create namespace client
NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", ServiceNamespace, string.Empty), credentials);
```

Voit sitten luoda jono-objekti ja tekstiviesti factory palvelun Bus URI argumenttina. Esimerkki:

```
QueueDescription myQueue;
myQueue = namespaceClient.CreateQueue("TestQueue");
MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", ServiceNamespace, string.Empty), credentials); 
QueueClient myQueueClient = factory.CreateQueueClient("TestQueue");
```

Voit lähettää viestejä sitten jonossa. Jos se viestejä, jota kutsutaan luettelo on esimerkiksi `MessageList`-koodi näkyy seuraavanlainen:

```
for (int count = 0; count < 6; count++)
{
    var issue = MessageList[count];
    issue.Label = issue.Properties["IssueTitle"].ToString();
    myQueueClient.Send(issue);
}
```

Voit sitten saapuvista viesteistä jonon seuraavasti:

```
while ((message = myQueueClient.Receive(new TimeSpan(hours: 0, minutes: 0, seconds: 5))) != null)
    {
        Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
        message.Complete();

        Console.WriteLine("Processing message (sleeping...)");
        Thread.Sleep(1000);
    }
```

[ReceiveAndDelete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) -tilassa ja Vastaanottotoiminto on single-vahvistetaan; Kun palvelun Bus saa pyynnön, se merkitsee viestin kulutettu käytössä-tilassa ja palauttaa sen sovelluksen. [ReceiveAndDelete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) tila on helpoin malli, ja se toimii parhaiten tilanteissa, joissa sovellusta hyväksyt ei käsittele viesti, jos. Tutustu tämän harkitse tilanne, jossa kuluttaja vastaanota pyynnön ja sitten kaatuu, ennen kuin sitä käsitellään. Palvelun Bus merkitsee viestin parhaillaan kulutettu, kun sovellus käynnistyy ja aloittaa muissa viestejä uudelleen, koska se jääneet viesti, joka on käytetty ennen kaatumisen.

[PeekLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) -tilassa vastaanota-toimintoa on kaksivaiheinen, joka mahdollistaa sellaisten tuki-sovelluksia, jotka puuttuvat viestejä ei hyväksy. Palvelun Bus saa pyynnön, kun se havaitsee seuraava viesti voi olla kulutettu, lukitsee estää muita kuluttajille vastaanottanut sitä ja palaa sovelluksen. Kun sovellus käsittelemisen viestin (tai tallentaa sen luotettavasti myöhempää käsittelyä varten), se suorittaa vastaanota-prosessin toisen vaiheen kutsumalla [valmiina](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) vastaanotetun viestin. Kun palvelun Bus näkee Soita [valmiina](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) , se merkitsee viestin on käytetty.

Jos sovellusta ei voi käsitellä viestiä jostain syystä, se voit kutsua [Hylkää](https://msdn.microsoft.com/library/azure/hh181837.aspx) -menetelmän (sijaan [valmiina](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx)) viestin. Näin palvelun Bus viestin lukituksen ja voi vastaanottaa uudelleen samaan kuluttaja tai toisen kilpailevien kuluttaja käytettäväksi. Toiseksi on liittyvät lukituksen aikakatkaisu ja jos sovellus ei käsitellä viestiä lukitus ennen aikakatkaisu vanhentuu (esimerkiksi, jos sovellus kaatuu) ja valitse palvelu Bus lukitus viestin ja mahdollistaa sen käytön vastaanotettu uudelleen (pääosin suorittaa [Hylkää](https://msdn.microsoft.com/library/azure/hh181837.aspx) oletusarvoisesti).

Huomaa, että silloin, kun sovellus kaatuu viestin käsittelyn jälkeen mutta ennen [valmiina](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) -pyyntö annetaan, viesti on toimitettu edelleen sovelluksen uudelleenkäynnistyksen yhteydessä. Tätä kutsutaan usein *Vähintään kerran *käsittely; Kukin viesti käsitellään eli vähintään kerran. Kuitenkin tietyissä tilanteissa sama viesti saattaa olla toimitettu edelleen. Jos skenaariota ei hyväksy kaksoiskappaleet käsitellään, ja valitse Lisää logiikan tarvitaan sovellus tunnistaa kaksoiskappaleet, jotka voidaan saavuttaa perustuvan sanoman, joka pysyy vakiona yli toimitusyritysten **MessageId** -ominaisuutta. Tätä kutsutaan nimellä *Täsmälleen kerran* käsitellään.

Lisätietoja ja voit luoda ja lähettää viestejä ja työn Esimerkki peräisin olevien on artikkelissa [palvelun Bus se messaging .NET opetusohjelma](service-bus-brokered-tutorial-dotnet.md).

## <a name="topics-and-subscriptions"></a>Ohjeita ja -tilaukset

Toisin kuin olevien, jossa jokaisen viestin käsitellään yksittäisen kuluttaja, *aiheet* ja *tilaukset* on yksi-moneen-lomakkeen tiedonvälityksen, *Julkaise ja tilaa* kuvio. Hyödyllisiä skaalauksen erittäin paljon vastaanottajia, julkaistun kustakin viestistä käyttöönsä jokaisen tilauksen rekisteröityjä aihetta. Viestit lähetetään aiheeseen ja vähintään yhden liittyvän tilaukset-suodatusehdot voi määrittää tilausta kohti välein riippuen toimitettu. Tilausten rajoita viestejä, jotka he haluavat saada Lisäsuodattimet avulla. Viestit lähetetään aiheeseen samalla tavalla kuin ne lähetetään jonossa, mutta viestejä ei vastaanoteta aiheesta suoraan. Sen sijaan ne on saatu tilaukset. Aiheen tilauksen muistuttaa virtual jono, joka vastaanottaa viesteistä, jotka lähetetään aiheen. Viestejä vastaanotetaan tilauksesta samannimisen tapaan, ne on saatu jonossa.

Mitä vertailu viestin lähettäminen toimintoja jonon yhdistää suoraan aiheen ja viestin vastaanottaminen toiminnoista yhdistää tilaukseen. Tämä tarkoittaa muun muassa tilaukset tukevat samat kuviot edempänä tässä osassa olevien osalta: kilpailevien kuluttajille, ajallinen irtautus, kuormituksen tasaamisen ja kuormituksen tasaamisen.

Aiheen luominen on jonossa, samalla tavalla kuin edellisessä osassa esimerkin mukaisesti. Luo palvelun URI ja luoda nimitilan asiakkaan [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) -luokan avulla. Voit luoda aiheen [CreateTopic](https://msdn.microsoft.com/library/azure/hh293080.aspx) -menetelmällä. Esimerkki:

```
TopicDescription dataCollectionTopic = namespaceClient.CreateTopic("DataCollectionTopic");
```

Lisää seuraavaksi tilaukset haluamallasi tavalla:

```
SubscriptionDescription myAgentSubscription = namespaceClient.CreateSubscription(myTopic.Path, "Inventory");
SubscriptionDescription myAuditSubscription = namespaceClient.CreateSubscription(myTopic.Path, "Dashboard");
```

Voit luoda aiheen asiakas. Esimerkki:

```
MessagingFactory factory = MessagingFactory.Create(serviceUri, tokenProvider);
TopicClient myTopicClient = factory.CreateTopicClient(myTopic.Path)
```

Käytä viestin lähettäjä, voit lähettää ja vastaanottaa viestejä ja ja edellisessä osassa esitetyllä tavalla. Esimerkki:

```
foreach (BrokeredMessage message in messageList)
{
    myTopicClient.Send(message);
    Console.WriteLine(
    string.Format("Message sent: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
}
```

Kaltaisilta olevien viestejä vastaanotetaan käyttämällä [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) objekti sen sijaan, että [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) objektin tilaukseen. Luo tilauksen asiakkaan kulkeva parametreiksi aiheen ja nimi, tilaus ja (valinnainen) vastaanotto-tilassa. Esimerkiksi ja **Varasto** -tilaus:

```
// Create the subscription client
MessagingFactory factory = MessagingFactory.Create(serviceUri, tokenProvider); 

SubscriptionClient agentSubscriptionClient = factory.CreateSubscriptionClient("IssueTrackingTopic", "Inventory", ReceiveMode.PeekLock);
SubscriptionClient auditSubscriptionClient = factory.CreateSubscriptionClient("IssueTrackingTopic", "Dashboard", ReceiveMode.ReceiveAndDelete); 

while ((message = agentSubscriptionClient.Receive(TimeSpan.FromSeconds(5))) != null)
{
    Console.WriteLine("\nReceiving message from Inventory...");
    Console.WriteLine(string.Format("Message received: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
    message.Complete();
}          

// Create a receiver using ReceiveAndDelete mode
while ((message = auditSubscriptionClient.Receive(TimeSpan.FromSeconds(5))) != null)
{
    Console.WriteLine("\nReceiving message from Dashboard...");
    Console.WriteLine(string.Format("Message received: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
}
```

### <a name="rules-and-actions"></a>Säännöt ja toiminnot

Skenaarioita viestejä, joilla on tiettyjä ominaisuuksia, on käsiteltävä eri tavoilla. Voit ottaa käyttöön, voit määrittää tilaukset, jos haluat löytää viestit, jotka on haluamasi ominaisuudet ja suorita sitten tietyt muutokset nämä ominaisuudet. Kun palvelun Bus tilaukset näkyvissä aiheeseen lähetetyt viestit, voit kopioida alijoukkoa viestit vain virtual tilauksen jonossa. Tämä tehdään tilauksen suodattimien avulla. Muutoksia kutsutaan *suodatustoiminnot*. Kun tilaus on luotu, voit antaa suodatin-lausekkeen, joka toimii ominaisuuksien viestin järjestelmän ominaisuuksia (esimerkiksi **otsikko**) ja mukautetun sovelluksen ominaisuuksia (esimerkiksi **StoreName**.) SQL-suodatin-lausekkeen on valinnainen tällöin; ilman SQL suodatin-lausekkeen määritetty tilauksen suodattimen toiminnon suoriteta, tilauksen kaikkiin viesteihin.

Edellisessä esimerkissä, suodattimen viesteihin tulevat vain **Store1**, luot Dashboard-tilauksen seuraavasti:

```
namespaceManager.CreateSubscription("IssueTrackingTopic", "Dashboard", new SqlFilter("StoreName = 'Store1'"));
```

Tämän tilauksen suodattimen paikassa, vain ne viestit, joiden kanssa `StoreName` -ominaisuuden arvoksi `Store1` kopioidaan virtual jonossa `Dashboard` tilaus.

Saat lisätietoja mahdollisista suodatinarvoja [SqlFilter](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx) ja [SqlRuleAction](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlruleaction.aspx) luokkien ohjeissa. Katso myös [se Messaging: lisäsuodattimia](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749) ja [Aiheen suodattimet](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters) malleja.

## <a name="next-steps"></a>Seuraavat vaiheet

Katso ohjeaiheissa on lisätietoja ja esimerkkejä palvelun Bus se tekstiviesti kohteiden Lisäasetukset.

- [Palvelun Bus tekstiviesti yleiskatsaus](service-bus-messaging-overview.md)
- [Palvelun Bus se tekstiviesti .NET-opetusohjelma](service-bus-brokered-tutorial-dotnet.md)
- [Palvelun Bus se tekstiviesti REST-opetusohjelma](service-bus-brokered-tutorial-rest.md)
- [Aiheen suodattimet-Esimerkki](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters)
- [Se viestit: Advanced suodattimet-Esimerkki](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749)

