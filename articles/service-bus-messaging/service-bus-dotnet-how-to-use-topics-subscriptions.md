<properties
    pageTitle="Palvelun Bus aiheet käyttäminen .NET | Microsoft Azure"
    description="Opi käyttämään palvelua Bus aiheet ja tilaukset .NET Azure-tietokannassa. MALLIKOODEJA kirjoitetaan .NET-sovelluksissa."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="09/16/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Palvelun Bus aiheet ja tilaukset käyttäminen

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Tässä artikkelissa käsitellään palvelun Bus aiheet ja tilaukset. Mallit on kirjoitettu C# ja käyttämällä .NET-ohjelmointirajapinnan. Tilanteita, joissa kattaa Sisällytä luominen aiheet ja tilaukset-tilauksen suodattimien luomisesta, lähettää viestejä aiheen, vastaanottaa viestejä tilauksesta ja poistaminen aiheet ja tilaukset. Lisätietoja aiheet ja tilaukset-kohdassa [seuraavat vaiheet](#next-steps) .

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="configure-the-application-to-use-service-bus"></a>Määritä sovellus käyttää palvelun Bus

Kun luot palvelun Bus käyttävä sovellus, Lisää palvelu Bus kokoonpanon viittaus ja sisältää vastaavan nimitilan. Helpoin tapa on Lataa vastaava [NuGet](https://www.nuget.org) paketti.

## <a name="get-the-service-bus-nuget-package"></a>Hae palvelun Bus NuGet-paketti

[Palvelun Bus NuGet paketti](https://www.nuget.org/packages/WindowsAzure.ServiceBus) on helpoin tapa saat palvelun Bus Ohjelmointirajapinnan ja määrittää sovelluksen tarvittavat palvelun Bus riippuvuudet. Asenna Service Bus NuGet paketti projektin, toimi seuraavasti:

1.  Napsauta ratkaisunhallinnassa **viittaukset**hiiren kakkospainikkeella ja valitse **NuGet pakettien hallinta**.
2.  Etsi "Palvelun Bus" ja **Microsoft Azure palvelun Bus** kohteen valitseminen. Viimeistele asennus valitsemalla **Asenna** ja valitse Sulje seuraavassa valintaikkunassa:

    ![][7]

Olet nyt valmiina kirjoittaa koodin palvelu Bus.

## <a name="create-a-service-bus-connection-string"></a>Palvelun Bus yhteysmerkkijonon luominen

Palvelun Bus käyttää yhteysmerkkijonon päätepisteet ja tunnistetietojen tallentamiseen. Voit lisätä yhteysmerkkijono määritystiedoston sijaan Kova-koodaus sitä:

- Azure-palveluita käytettäessä kannattaa tallentaa yhteysmerkkijono Azure palvelun määritysten järjestelmän (.csdef ja .cscfg tiedostot).
- Azure sivustoja tai Azuren näennäiskoneiden käytettäessä kannattaa tallentaa yhteysmerkkijono .NET-määritysten järjestelmän (esimerkiksi seuraavan koodin korostetut) avulla.

Kummassakin tapauksessa voit hakea yhteys-merkkijono-avulla `CloudConfigurationManager.GetSetting` menetelmä, kuten tämän artikkelin.

### <a name="configure-your-connection-string"></a>Määritä yhteysmerkkijono

Palvelun määritysten järjestelmä avulla voit muuttaa dynaamisesti määritysasetukset [Azure portal][] ilman mallirakenteeseen sovelluksen. Esimerkiksi lisäämällä `Setting` tarra palvelun määritys (**.csdef**)-tiedostoon, seuraavan esimerkin mukaisesti.

```
<ServiceDefinition name="Azure1">
...
    <WebRole name="MyRole" vmsize="Small">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString" />
        </ConfigurationSettings>
    </WebRole>
...
</ServiceDefinition>
```

Palvelun määritystiedoston (.cscfg) Määritä sitten arvot.

```
<ServiceConfiguration serviceName="Azure1">
...
    <Role name="MyRole">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString"
                     value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
        </ConfigurationSettings>
    </Role>
...
</ServiceConfiguration>
```

Jaettu Access allekirjoitus (SAS) avaimen nimi ja noutaa portaalin ohjeiden aiemmin avainarvot avulla.

### <a name="configure-your-connection-string-when-using-azure-websites-or-azure-virtual-machines"></a>Määritä yhteysmerkkijono Azure sivustoja tai Azuren näennäiskoneiden käytettäessä

Sivustot tai näennäiskoneiden käytettäessä on suositeltavaa, että käytät järjestelmän .NET kokoonpano (esimerkiksi Web.config). Voit tallentaa yhteyden merkkijonon käyttäminen `<appSettings>` elementti.

```
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
    </appSettings>
</configuration>
```

Käytä SAS nimi ja avainarvot, joka noutaa [Azure portal][]kuvatulla.

## <a name="create-a-topic"></a>Aiheen luominen

Voit suorittaa hallintatoiminnot palvelun Bus aiheet ja tilauksista [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) -luokka. Tähän luokkaan sisältää tapoja luoda ja luetteloida poistaminen ohjeita.

Seuraavassa esimerkissä rakenteiden `NamespaceManager` objektin avulla Azure `CloudConfigurationManager` koostuva palvelun Bus nimitila ja sopiva Suojaussidokset perusosoitteen yhteysmerkkijonon luokan tunnistetiedot oikeuksilla voit hallita sitä. Tämä yhteysmerkkijono on seuraavassa muodossa:

```
Endpoint=sb://<yourNamespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<yourKey>
```

Käytä seuraavassa esimerkissä annettu edellisessä osassa asetukset.

```
// Create the topic if it does not exist already.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic("TestTopic");
}
```

On Osastollasi [CreateTopic](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.createtopic.aspx) menetelmän, joiden avulla voit määrittää aiheen; ominaisuudet esimerkiksi oletusarvoisen määrittämisestä--elinaika (TTL)-arvo käytetään aiheen lähetettyihin viesteihin. Nämä asetukset otetaan käyttöön käyttämällä [TopicDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.aspx) -luokka. Seuraavassa esimerkissä esitetään luomisesta ohjeaiheessa nimeltä **TestTopic** , joiden koko on enintään 5 gt ja oletusarvon viestin minuutin TTL.

```
// Configure Topic Settings.
TopicDescription td = new TopicDescription("TestTopic");
td.MaxSizeInMegabytes = 5120;
td.DefaultMessageTimeToLive = new TimeSpan(0, 1, 0);

// Create a new Topic with custom settings.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic(td);
}
```

> [AZURE.NOTE] [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) objektien [TopicExists](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.topicexists.aspx) menetelmän avulla voit tarkistaa, onko aiheen, määritetty nimi on jo nimitila.

## <a name="create-a-subscription"></a>Tilauksen luominen

Voit myös luoda aiheen tilauksista [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) -luokka. Tilaukset nimetään, ja se voi olla valinnainen suodatin, joka rajoittaa tietuejoukon tilauksen virtual jonon välitetään viestejä.

> [AZURE.IMPORTANT] Viestien vastaanottaminen tilauksessa tilauksen sinun on luotava, ennen kuin lähetät viestejä aiheen tilauksen. Jos ei ole aihe-tilaukset, aiheen hylkää viestit.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Luo tilauksen oletussuodatin (MatchAll)

Jos suodatinta ei määritetä, kun uusi tilaus on luotu, **MatchAll** suodatin on oletussuodatin, jota käytetään. Kun **MatchAll** -suodatin, kaikki viestit, jotka on julkaistu aiheen sijoitetaan tilauksen virtual jonossa. Seuraava esimerkki luo tilausta nimeltä "AllMessages" ja käyttää **MatchAll** oletussuodatin.

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.SubscriptionExists("TestTopic", "AllMessages"))
{
    namespaceManager.CreateSubscription("TestTopic", "AllMessages");
}
```

### <a name="create-subscriptions-with-filters"></a>Luo tilaukset suodattimet

Voit myös määrittää suodattimia, joiden avulla voit määrittää, mitkä aiheen lähetetyt viestit kannattaa tiettyä aihetta-tilauksen piiriin kuuluvien näkyvän.

Tuettu tilauksissa suodattimen eniten joustavia tyyppi on [SqlFilter][] -luokka, joka toteuttaa SQL92 alijoukkoa. Viestit, jotka on julkaistu aiheen ominaisuudet toimivat SQL suodattimet. Saat lisätietoja lausekkeita, jotka voidaan käyttää SQL-suodattimella [SqlFilter.SqlExpression][] syntaksi.

Seuraavassa esimerkissä luodaan nimeltä **HighMessages** [SqlFilter][] objektia, jonka valitsee vain viestit, joissa on suurempi kuin 3 mukautetun **MessageNumber** ominaisuuden tilauksen.

```
// Create a "HighMessages" filtered subscription.
SqlFilter highMessagesFilter =
   new SqlFilter("MessageNumber > 3");

namespaceManager.CreateSubscription("TestTopic",
   "HighMessages",
   highMessagesFilter);
```

Seuraavassa esimerkissä luodaan vastaavasti tilausta nimeltä **LowMessages** [SqlFilter][] , joka valitsee vain viestit, joiden **MessageNumber** ominaisuus pienempi tai yhtä 3 kanssa.

```
// Create a "LowMessages" filtered subscription.
SqlFilter lowMessagesFilter =
   new SqlFilter("MessageNumber <= 3");

namespaceManager.CreateSubscription("TestTopic",
   "LowMessages",
   lowMessagesFilter);
```

Nyt, kun viesti on lähetetty `TestTopic`, se toimitetaan aina vastaanottajia **AllMessages** aihe-tilaus on tilattu ja valikoivasti toimitettu vastaanottajia tilannut (sen mukaan, viestisisältö) **HighMessages** ja **LowMessages** aihe-tilauksia.

## <a name="send-messages-to-a-topic"></a>Viestien lähettäminen aiheen

Jos haluat lähettää viestin palvelun Bus aiheen, sovelluksen Luo yhteysmerkkijonon [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) objektin.

Seuraava koodi näytetään, miten voit luoda [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) objektin **TestTopic** aiheen aiemmin luotu [`CreateFromConnectionString`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.createfromconnectionstring.aspx) API-kutsu.

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

TopicClient Client =
    TopicClient.CreateFromConnectionString(connectionString, "TestTopic");

Client.Send(new BrokeredMessage());
```

Palvelun Bus aiheet lähetetyt viestit ovat [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) luokan esiintymät. [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) objekteissa on joukko vakio-ominaisuuksia (esimerkiksi [otsikon](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) ja [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)) sanasto, jota käytetään pidä mukautetun sovelluksen kielikohtaiset ominaisuudet ja haluamaansa hakemuksen tiedot. Sovelluksen voit määrittää viestin tekstiosaan siirtämällä voi sarjoittaa objektin konstruktoria [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) objektin ja sopiva **System.Runtime.Serialization** käytetään onnistu objekti. Vaihtoehtoisesti voit toimittaa **System.IO.Stream** .

Seuraavassa esimerkissä kerrotaan, miten **TestTopic** [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) objektiin saatu koodin edellisessä esimerkissä viisi testi viestien lähettämiseen. Huomaa, että kustakin viestistä [MessageNumber](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.properties.aspx) ominaisuuden arvo vaihtelee riippuen tapahtuu iteraatio (paina ALT + w määrittää, mitkä tilaukset vastaanottaa sen).

```
for (int i=0; i<5; i++)
{
  // Create message, passing a string message for the body.
  BrokeredMessage message = new BrokeredMessage("Test message " + i);

  // Set additional custom app-specific property.
  message.Properties["MessageNumber"] = i;

  // Send message to the topic.
  Client.Send(message);
}
```

Palvelun Bus ohjeita tukea enimmäiskoon koosta [Vakio taso](service-bus-premium-messaging.md) 256 Kilotavua ja 1 Megatavu [Premium taso](service-bus-premium-messaging.md). Otsikko, joka sisältää vakio- ja mukautettu-sovelluksen ominaisuudet, voi olla koko on enintään 64 Kilotavua. Ei ole rajoitettu viestit säilytetään aiheen määrän, mutta ole pää kokonaiskoko viestit aiheen mukaan. Tässä aiheessa kokoa on määritetty luominen aikaa, yläraja on 5 gt. Jakaminen on käytössä, jos yläraja on korkeampi. Lisätietoja on artikkelissa [Partitioned messaging kohteita](service-bus-partitioning.md).

## <a name="how-to-receive-messages-from-a-subscription"></a>Voit vastaanottaa viestejä tilauksesta

Vastaanottaa viestejä tilauksesta myös käyttää [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) objektia avulla. [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) objektit työskennellä kahta eri tilat: [ *ReceiveAndDelete* ja *PeekLock*](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx).

Kun **ReceiveAndDelete** -tilassa on yksi tuleva; toiminto Kun palvelun Bus vastaanottaa lukukuittauksen viesti-tilauksen, se merkitsee viestin kulutettu käytössä-tilassa ja palaa sovelluksen. **ReceiveAndDelete** tila on helpoin malli, ja se toimii parhaiten skenaarioissa, jossa sovellus hyväksyt ei käsittele viesti, jos. Tutustu tämän harkitse tilanne, jossa kuluttaja vastaanota pyynnön ja sitten kaatuu, ennen kuin sitä käsitellään. Palvelun Bus on merkinnyt viestin kuin niitä käytetään, kun sovellus käynnistyy ja aloittaa muissa viestejä uudelleen, koska se jääneet viesti, joka on käytetty ennen kaatumisen.

**PeekLock** tilassa (joka on oletusarvo) vastaanota-prosessin on kaksivaiheinen-toimintoa, joka mahdollistaa sellaisten tuki-sovelluksia, jotka ei hyväksy puuttuu viestejä. Palvelun Bus saa pyynnön, kun se havaitsee seuraava viesti voi olla kulutettu, lukitsee voit estää muita kuluttajille vastaanottanut sitä ja palaa sovelluksen. Kun sovellus käsittelemisen viestin (tai tallentaa sen luotettavasti myöhempää käsittelyä varten), se suorittaa vastaanota-prosessin toisen vaiheen kutsumalla [valmiina](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) vastaanotetun viestin. Palvelun Bus näkee Soita **valmiina** , kun se merkitsee viestin kulutettu käytössä-tilassa ja poistaa sen tilauksen.

Seuraavassa esimerkissä näytetään, miten viestit voi vastaanottaa ja käsitellä oletusarvon **PeekLock** -tilassa. Jos haluat määrittää eri [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) arvon, voit käyttää [CreateFromConnectionString](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.createfromconnectionstring.aspx)toiseen liikaa. Tässä esimerkissä käytetään [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.onmessage.aspx) takaisinkutsu prosessin viestien saapuessa **HighMessages** -tilaukseen.

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

SubscriptionClient Client =
    SubscriptionClient.CreateFromConnectionString
            (connectionString, "TestTopic", "HighMessages");

// Configure the callback options.
OnMessageOptions options = new OnMessageOptions();
options.AutoComplete = false;
options.AutoRenewTimeout = TimeSpan.FromMinutes(1);

Client.OnMessage((message) =>
{
    try
    {
        // Process message from subscription.
        Console.WriteLine("\n**High Messages**");
        Console.WriteLine("Body: " + message.GetBody<string>());
        Console.WriteLine("MessageID: " + message.MessageId);
        Console.WriteLine("Message Number: " +
            message.Properties["MessageNumber"]);

        // Remove message from subscription.
        message.Complete();
    }
    catch (Exception)
    {
        // Indicates a problem, unlock message in subscription.
        message.Abandon();
    }
}, options);
```

Tässä esimerkissä määrittää [OnMessageOptions](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.aspx) -objektin [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.onmessage.aspx) takaisinkutsu. [Automaattisen täydennyksen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autocomplete.aspx) on määritetty **Epätosi** soittaminen [valmiina](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) vastaanotetun viestin kannattaa hallita manuaalisesti. [AutoRenewTimeout](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autorenewtimeout.aspx) on määritetty minuutin, joka aiheuttaa asiakkaan odottamaan minuutin ennen lopetetaan automaattisen uusinnan-ominaisuus ja asiakkaan soittaa uusien viestien tarkistusvälin. Tämän ominaisuuden arvon vähentää asiakkaan tekee laskutettavia kutsuja, joka noutaa viestien määrä.

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Sovellus kaatuu ja voi lukea viestien käsittelemisestä

Palvelun Bus on toimintoja, joiden avulla virheet sovelluksessa tai viestin käsittelyn vaikeuksia tilanteen palauttamisessa. Jos vastaanottava sovellus ei voi käsitellä viestiä jostain syystä, se voit kutsua [Hylkää](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.abandon.aspx) -menetelmää vastaanotetun viestin (sijaan [valmiina](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) -menetelmä). Tämä aiheuttaa palvelun Bus tilauksen piiriin kuuluvien viestin lukituksen ja vastaanottaa uudelleen samaan vievää sovelluksen tai jonkin muun vievää sovelluksen käytettäväksi.

On myös lukittu tilauksen piiriin kuuluvien viestin liittyvät aikakatkaisu ja jos sovellus ei käsitellä viestiä ennen Lukitse aikakatkaisun vanhenee (esimerkiksi, jos sovellus kaatuu), palvelun Bus lukitus viestin automaattisesti ja mahdollistaa sen käytön vastaanotettu uudelleen.

Silloin, kun sovellus kaatuu viestin käsittelyn jälkeen mutta ennen [valmiina](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) -pyyntö annetaan, viesti on toimitettu edelleen sovelluksen uudelleenkäynnistyksen yhteydessä. Tätä kutsutaan usein *vähintään kerran käsittely*; jokaisen viestin käsitellään vähintään kerran, mutta tietyissä tilanteissa sama viesti saattaa olla toimitettu edelleen. Jos skenaariota ei hyväksy kaksoiskappaleiden käsittely, sitten sovelluskehittäjät Lisää muita logiikan niiden sovelluksen käsittelee kaksoiskappaleita viestin lähettämisen. Tämä on usein saavuttaa sanoman, joka pysyy vakiona yli toimitusyritysten [MessageId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) -ominaisuuden avulla.

## <a name="delete-topics-and-subscriptions"></a>Poista aiheet ja tilaukset

Seuraavassa esimerkissä kerrotaan, miten voit poistaa aiheen **TestTopic** **HowToSample** palvelun nimitila.

```
// Delete Topic.
namespaceManager.DeleteTopic("TestTopic");
```

Aiheen poistaminen poistaa myös tilaukset, jotka on rekisteröity aihetta. Tilauksia voi poistaa myös erikseen. Seuraava koodi esitellään, miten voit poistaa tilausta nimeltä **HighMessages** **TestTopic** aiheesta.

```
namespaceManager.DeleteSubscription("TestTopic", "HighMessages");
```

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet tutustunut palvelun Bus aiheet ja tilaukset-näistä linkeistä saat lisätietoja.

-   [Olevien, aiheet, ja tilaukset][].
-   [Aiheen suodattimet-Esimerkki][]
-   [SqlFilter][]API-viittaus.
-   Muodosta toimimasta-sovellus, joka lähettää ja vastaanottaa viestejä ja palvelun Bus jonon: [palvelun Bus se tekstiviesti .NET-opetusohjelma][].
-   Palvelun Bus esimerkit: Lataa [Azure näytteiden][] tai artikkelissa [Yleiskatsaus](service-bus-samples.md).

  [Azure portal]: https://portal.azure.com

  [7]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/getting-started-multi-tier-13.png

  [Olevien, aiheet ja tilaukset]: service-bus-queues-topics-subscriptions.md
  [Aiheen suodattimet-Esimerkki]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx
  [SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [Palvelun Bus se tekstiviesti .NET-opetusohjelma]: service-bus-brokered-tutorial-dotnet.md
  [Azure-objektit]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
