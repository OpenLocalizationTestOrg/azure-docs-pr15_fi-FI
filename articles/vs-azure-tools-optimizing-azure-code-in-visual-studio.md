<properties
   pageTitle="Optimointi Azure koodisi Visual Studiossa | Microsoft Azure"
   description="Lue tietoja siitä, miten Azure-koodin optimointi Työkalut Visual Studiossa parantaa koodisi entistä tehokkaat ja better-menestyneet."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="optimizing-your-azure-code"></a>Azure koodin optimointi

Kun olet ohjelmoinnin sovellukset, jotka käyttävät Microsoft Azure on joitakin coding käytännöt noudattamalla voit ehkäistä ongelmat app skaalattavuus, toiminta ja suorituskyvyn cloud-ympäristössä. Microsoft toimittaa Azure koodin Analysis-työkalua, joka tunnistaa ja määrittää useita yleisimmin havaitsi ongelmat ja avulla voit ratkaista ne. Voit ladata työkalun Visual Studiossa NuGet kautta.

## <a name="azure-code-analysis-rules"></a>Azure koodin analyysi säännöt

Azure koodi-analyysityökalu Merkitse Azure koodin automaattisesti, kun se havaitsee suorituskykyyn vaikuttavat tunnettuja seuraavien sääntöjen avulla. Havaittu ongelmat näkyvät varoituksia tai kääntäjän virheet. Koodin korjauksia tai ehdotuksia varoituksia tai virheen ratkaisemiseksi toimitetaan usein hehkulampun-kuvakkeen kautta.

## <a name="avoid-using-default-in-process-session-state-mode"></a>Vältä oletusarvo (prosessin) istunnon tila

### <a name="id"></a>TUNNUS

AP0000

### <a name="description"></a>Kuvaus

Jos käytät cloud sovellusten oletusarvo (prosessin) istunnon-tila, enää istunnon tila.

Jaa ideoita ja palautetta osoitteessa [Azure koodin analyysi palautetta](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Syy

Oletusarvon mukaan määritetty seuraavan koodin korostetut istunnon-tila on prosessin. Myös, jos tekstiä ei ole määritetty määritystiedostossa, istunnon tila oletusarvoisesti prosessin. Prosessin tila tallentaa istunnon tila muistiin WWW-palvelimessa. Kun erillisen käynnistetään tai uuden esiintymän käytettäviä kuormituksen tai automaattisesti tukeen, WWW-palvelimessa muistissa istunnon tila ei tallenneta. Tämä tilanne estää sovellusta parhaillaan skaalattava pilveen.

ASP.NET-istunnon tila tukee useita eri tallennusasetukset istunnon tilatiedot: InProc, StateServer, SQL Server-mukautettu ja poistaminen käytöstä. On suositeltavaa, että käytät mukautettu tila host tietoihin ulkoisen istunnon tila-säilössä, kuten [Redis.txt Azure istunnon tila-palvelu](http://go.microsoft.com/fwlink/?LinkId=401521).

### <a name="solution"></a>Ratkaisu

Yksi suositeltu ratkaisu on istunnon tila tallentamiseksi hallitun välimuisti-palvelun. Opettele käyttämään [Redis.txt Azure istunnon tila-palvelu](http://go.microsoft.com/fwlink/?LinkId=401521) tallentamista istunnon tila. Voit myös tallentaa istunnon tila eri kohdissa, jotta sovelluksesi on skaalattava pilveen. Lisätietoja vaihtoehtoisen ratkaisuja Lue [Istunnon tilan tilat](https://msdn.microsoft.com/library/ms178586).

## <a name="run-method-should-not-be-async"></a>Suorita menetelmä ei saa olla asynkroninen

### <a name="id"></a>TUNNUS

AP1000

### <a name="description"></a>Kuvaus

Luo asynkroninen menetelmiä (kuten [odotettava](https://msdn.microsoft.com/library/hh156528.aspx)) ulkopuolella [Run ()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) -menetelmä ja kutsua sitten asynkroninen menetelmiä [Run ()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx). Uudelleenkäynnistys silmukan antamaan Työntekijä-roolin määritteleminen [[Run ()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) -menetelmää kuin asynkroninen valitaan.

Jaa ideoita ja palautetta osoitteessa [Azure koodin analyysi palautetta](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Syy

Cloud palvelun Runtimen Roskakorin Työntekijä-roolin aiheuttaa kutsumista asynkroninen menetelmiä sisällä [Run ()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) -menetelmää. Työntekijän rooli käynnistyessä kaikki ohjelman suorittaminen käydään [Run ()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) -menetelmää. Käynnistämään Työntekijä-roolin aiheuttaa sulkeminen [Run ()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) -menetelmää. Työntekijän roolin runtime käynnit asynkroninen menetelmä, kun se lähettää asynkroninen tavan jälkeen kaikki toiminnot ja palauttaa. Tämä aiheuttaa työntekijän rooli [[[[Run ()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) -menetelmästä suljetaan ja käynnistetään uudelleen. Suorittamisen seuraava iteraation Työntekijä-roolin käynnit asynkroninen menetelmä uudelleen ja käynnistää ilmenneet Roskakorin uudelleen myös työntekijän rooli.

### <a name="solution"></a>Ratkaisu

Vie kaikki asynkroninen toiminnot ulkopuolella [Run ()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) -menetelmää. Valitse kutsu refactored asynkroninen-menetelmää [[Run ()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) -menetelmää, kuten RunAsync () .wait sisällä. Azure koodi-analyysityökalu avulla voit korjata tämän ongelman.

Seuraavat koodikatkelman esittelee koodi korjata tämän ongelman:

```
public override void Run()
{
    RunAsync().Wait();
}

public async Task RunAsync()
{
    //Asynchronous operations code logic
    // This is a sample worker implementation. Replace with your logic.

    Trace.TraceInformation("WorkerRole1 entry point called");

    HttpClient client = new HttpClient();

    Task<string> urlString = client.GetStringAsync("http://msdn.microsoft.com");

    while (true)
    {
        Thread.Sleep(10000);
        Trace.TraceInformation("Working");

        string stream = await urlString;
    }

}
```

## <a name="use-service-bus-shared-access-signature-authentication"></a>Käytä palvelun Bus jaettu Access allekirjoitus-todennus

### <a name="id"></a>TUNNUS

AP2000

### <a name="description"></a>Kuvaus

Käyttää jaettu Access allekirjoitus (SAS) todennusta varten. Accessin ohjausobjektin Service (ACS) ollaan poistamassa bus todentamiseen.

Jaa ideoita ja palautetta osoitteessa [Azure koodin analyysi palautetta](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Syy

Azure Active Directory korvaa tietoturvasyistä SAS todennusta ACS todennusta. Lisätietoja siirtymä-palvelupaketti on [Azure Active Directory on ACS tulevaisuudessa](http://blogs.technet.com/b/ad/archive/2013/06/22/azure-active-directory-is-the-future-of-acs.aspx) .

### <a name="solution"></a>Ratkaisu

Käytä SAS todennus sovelluksia. Seuraavassa esimerkissä, voit käyttää aiemmin SAS-tunnuksen palvelun bus nimitila tai kohteen.

```
MessagingFactory listenMF = MessagingFactory.Create(endpoints, new StaticSASTokenProvider(subscriptionToken));
SubscriptionClient sc = listenMF.CreateSubscriptionClient(topicPath, subscriptionName);
BrokeredMessage receivedMessage = sc.Receive();
```

Katso lisätietoja on seuraavissa artikkeleissa.

- Yleisiä tietoja tarkastella [Jaettujen palvelun Bus Access allekirjoituksen todentaminen](https://msdn.microsoft.com/library/dn170477.aspx)

- [Voit käyttää palvelun Bus jaettu Access allekirjoitus-todennus](https://msdn.microsoft.com/library/dn205161.aspx)

- Esimerkki projektin Katso [palvelun Bus tilauksia käyttämällä jaettu Access allekirjoitus (SAS) todentaminen](http://code.msdn.microsoft.com/windowsazure/Using-Shared-Access-e605b37c)

## <a name="consider-using-onmessage-method-to-avoid-receive-loop"></a>Harkitse OnMessage menetelmä Vältä "vastaanottaa silmukan"

### <a name="id"></a>TUNNUS

AP2002

### <a name="description"></a>Kuvaus

Voit välttää siirrytään "vastaanottaa silmukan" **OnMessage** kutsumista on parempi kuin **Vastaanota** kutsumista viestien vastaanottamiseen. Jos sinun on käytettävä **Vastaanotto** -menetelmällä ja määrität muun kuin oletusarvoisen palvelimen odotusaika, varmista, että palvelimen odotusaika on useampi kuin yksi minuutti.

Jaa ideoita ja palautetta osoitteessa [Azure koodin analyysi palautetta](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Syy

**OnMessage**soitettaessa asiakkaan käynnistyy sisäinen viesti-pumpun, joka tekee kyselyn jatkuvasti jonossa tai tilaus. Tämä viesti pumpun on äärettömän silmukka, ongelmia puhelun vastaanottaa viestejä. Jos puhelun aikakatkaistaan ongelmat uuden puhelun. Aikavälin määräytyy [MessagingFactory](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactory.aspx), jota käytetään [OperationTimeout](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx) -ominaisuuden arvon.

Käyttämällä **OnMessage** verrattuna **Vastaanota** etuna on se, että käyttäjillä ei manuaalisesti äänestyksen järjestäminen viestien, käsittele poikkeukset, käsitellä useita viestejä rinnakkain ja viimeistele viestit.

Jos olet soittanut **Vastaanota** käyttämättä sen oletusarvo, muista *ServerWaitTime* arvo on useampi kuin yksi minuutti. *ServerWaitTime* asettaminen yli minuutin estää palvelimen aikakatkaistu, ennen kuin viesti vastaanotetaan kokonaan.

### <a name="solution"></a>Ratkaisu

Katso suositellut käytöt seuraavasti koodi. Lisätietoja on artikkelissa [QueueClient.OnMessage menetelmä (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.onmessage.aspx)ja [QueueClient.Receive-menetelmän (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.receive.aspx).

Azure tekstiviesti-infrastruktuuria suorituskyvyn parantamiseksi artikkelissa rakenne kuvion [Asynkroninen Messaging askeleet](https://msdn.microsoft.com/library/dn589781.aspx).

Seuraavassa on esimerkki **OnMessage** avulla voit vastaanottaa viestejä.

```
void ReceiveMessages()
{
    // Initialize message pump options.
    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = true; // Indicates if the message-pump should call complete on messages after the callback has completed processing.
    options.MaxConcurrentCalls = 1; // Indicates the maximum number of concurrent calls to the callback the pump should initiate.
    options.ExceptionReceived += LogErrors; // Enables you to get notified of any errors encountered by the message pump.

    // Start receiving messages.
    QueueClient client = QueueClient.Create("myQueue");
    client.OnMessage((receivedMessage) => // Initiates the message pump and callback is invoked for each message that is recieved, calling close on the client will stop the pump.
    {
        // Process the message.
    }, options);
    Console.WriteLine("Press any key to exit.");
    Console.ReadKey();
```

Seuraavassa on esimerkki **Vastaanota** käyttäminen oletusarvon palvelimen odotusaika.

```
string connectionString =  
CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

QueueClient Client =  
    QueueClient.CreateFromConnectionString(connectionString, "TestQueue");

while (true)  
{   
   BrokeredMessage message = Client.Receive();
   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
```

Seuraavassa on esimerkki **Vastaanota** käyttäminen muun kuin oletusarvoisen palvelimen odotusaika.

```
while (true)  
{   
   BrokeredMessage message = Client.Receive(new TimeSpan(0,1,0));

   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
}
```
## <a name="consider-using-asynchronous-service-bus-methods"></a>Harkitse asynkroninen palvelun Bus menetelmät

### <a name="id"></a>TUNNUS

AP2003

### <a name="description"></a>Kuvaus

Asynkroninen palvelun Bus menetelmien avulla voit parantaa suorituskykyä ja se viestiliikennettä.

Jaa ideoita ja palautetta osoitteessa [Azure koodin analyysi palautetta](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Syy

Asynkronisten menetelmien avulla sovelluksen ohjelma samanaikainen, koska suoritetaan jokainen kutsu ei estä tärkeimmät viestiketjun. Palvelun Bus messaging menetelmiä, suorittaa käytettäessä (lähetetään, saavat, poistaa, jne.) kestää jonkin aikaa. Tällä hetkellä sisältää toiminnon käsittelyn palvelun Bus-palvelun viive pyynnön ja vastauksen lisäksi. Kun haluat lisätä toimintoja, ajan, toiminnot on suoritettava samanaikaisesti. Lisätietoja tutustumaan [Suorituskyvyn parannuksia käyttämällä palvelun Bus se Messaging parhaat käytännöt](https://msdn.microsoft.com/library/azure/hh528527.aspx).

### <a name="solution"></a>Ratkaisu

Saat lisätietoja käyttämisestä asynkroninen suositeltava [QueueClient luokan (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.aspx) .

Azure tekstiviesti-infrastruktuuria suorituskyvyn parantamiseksi artikkelissa rakenne kuvion [Asynkroninen Messaging askeleet](https://msdn.microsoft.com/library/dn589781.aspx).

## <a name="consider-partitioning-service-bus-queues-and-topics"></a>Harkitse osioinnin palvelun Bus olevien ja aiheet

### <a name="id"></a>TUNNUS

AP2004

### <a name="description"></a>Kuvaus

Osion palvelun Bus olevien ja aiheet ja palvelun Bus viestiliikennettä suorituskyvyn parantamiseksi.

Jaa ideoita ja palautetta osoitteessa [Azure koodin analyysi palautetta](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Syy

Suorituskyvyn nopeus- ja palveluluettelon käytettävyys jakaminen palvelun Bus olevien ja aiheet suurenee, koska yleinen siirtonopeuden osioitua jonossa tai aiheen rajoittavat enää yhden viestin broker tai tekstiviesti kaupan suorituskykyä. Lisäksi väliaikaisen käyttökatkosta tekstiviesti-säilön ei tee osioitua jonossa tai aiheen ei ole käytettävissä. Lisätietoja on artikkelissa [Jakaminen Messaging kohteita](https://msdn.microsoft.com/library/azure/dn520246.aspx).

### <a name="solution"></a>Ratkaisu

Seuraavat koodikatkelman näyttää, miten osion tekstiviesti kohteita.

```
// Create partitioned topic.
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

Lisätietoja on artikkelissa [osioitu palvelun Bus olevien ja aiheet | Microsoft Azure-blogi](https://azure.microsoft.com/blog/2013/10/29/partitioned-service-bus-queues-and-topics/) ja tutustu [Microsoft Azure palvelun Bus osioitu jonon](https://code.msdn.microsoft.com/windowsazure/Service-Bus-Partitioned-7dfd3f1f) otosten.

## <a name="do-not-set-sharedaccessstarttime"></a>Ei ole määritetty SharedAccessStartTime

### <a name="id"></a>TUNNUS

AP3001

### <a name="description"></a>Kuvaus

Vältä aloittaa heti jaettu käyttöoikeuskäytäntö nykyinen aika SharedAccessStartTimeset avulla. Tarvitset vain tämän ominaisuuden määrittäminen, jos haluat aloittaa jaettu käyttöoikeuskäytäntö myöhemmin.

Jaa ideoita ja palautetta osoitteessa [Azure koodin analyysi palautetta](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Syy

Kellon synkronoinnin aiheuttaa vähäisiä ajan ero palvelinkeskusten kesken. Esimerkiksi loogisesti Ajattele säilön SAS käytännön alkamisajan määrittäminen nykyisen kellonajan käyttämällä DateTime.Now tai samalla menetelmällä aiheuttaa SAS käytäntö tulee voimaan heti. Kuitenkin palvelinkeskusten vähäisiä aika erot saattavat aiheuttaa ongelmia tämän, koska palvelinkeskuksen toisinaan saattaa olla hieman alkamisaika, samalla, kun muut ennen sen viimeistään. Tuloksena SAS käytännön päättyy nopeasti (tai jopa heti) Jos käytännön elinaika on määritetty liian lyhyt.

Katso lisäohjeita jaettu Access allekirjoituksen käyttäminen Azure tallennustilan [Esittely taulukon Suojaussidosten (jaettu Access allekirjoitus), jonossa Suojaussidosten ja Blob-objektien SAS - Microsoft Azure-tallennustilan Ryhmäblogi - sivuston kotisivu - MSDN-blogit-päivityksen](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).

### <a name="solution"></a>Ratkaisu

Poista lause, joka määrittää jaetun käyttöoikeuskäytäntö alkamisaika. Azure koodi-analyysityökalu on korjaa ongelman. Saat lisätietoja suojauksen hallinta rakenne kuvion [Valet avain kuvio](https://msdn.microsoft.com/library/dn568102.aspx).

Seuraavat koodikatkelman esitellään koodi korjata tämän ongelman.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

## <a name="shared-access-policy-expiry-time-must-be-more-than-five-minutes"></a>Jaettu voimassaolon ajan on oltava enintään viisi minuuttia käyttöoikeuskäytäntö

### <a name="id"></a>TUNNUS

AP3002

### <a name="description"></a>Kuvaus

Voi olla mahdollisimman paljon viisi minuuttia erotus kellonaikoja kesken palvelinkeskusten eri paikoissa vuoksi ehdon nimeltään "kellon jakauman.vinous." Voit estää Suojaussidokset käytännön tunnus-päättyvän määrittää aiemmin määräajan aika on yli viisi minuuttia.

Jaa ideoita ja palautetta osoitteessa [Azure koodin analyysi palautetta](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Syy

Palvelinkeskusten eri paikoissa eri puolilla maailmaa synkronoida kellon signaalin. Koska kuluvaa aikaa kellon signaali siirtyvät eri sijainneissa, voi olla eri Maantieteellisten sijaintien palvelinkeskusten aika varianssi vaikka kaikki supposedly synkronoidaan. Ajan ero voivat vaikuttaa jaettu Access käytännön Käynnistä aika- ja vanhentuminen aikaväli. Tämän vuoksi, jotta jaettu Access-käytäntö tulee voimaan heti ei määritetä alkamisajan. Varmista lisäksi päättymisaika on enemmän kuin 5 minuuttia aikainen aikakatkaisu estämiseksi.

Saat lisätietoja jaettujen Access allekirjoituksen käyttäminen Azure tallennustilan [Esittely taulukon Suojaussidosten (jaettu Access allekirjoitus), jonossa Suojaussidosten ja Blob-objektien SAS - Microsoft Azure-tallennustilan Ryhmäblogi - sivuston kotisivu - MSDN-blogit-päivityksen](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).

### <a name="solution"></a>Ratkaisu

Lisätietoja suojauksen hallinta-kohdassa rakenne kuvion [Valet avain kuvio](https://msdn.microsoft.com/library/dn568102.aspx).

Seuraavassa on esimerkki määrittämättä jaettu Access käytännön alkamisaika.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

Seuraavassa on esimerkki jaettu Access käytännön alkamisaika, joka määrittää käytännön vanhentuminen kesto suurempi kuin viisi minuuttia.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
  SharedAccessStartTime = new DateTime(2014,1,20),   
 SharedAccessExpiryTime = new DateTime(2014, 1, 21),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

Lisätietoja on artikkelissa [jaettujen Access-allekirjoituksen luominen ja käyttäminen](https://msdn.microsoft.com/library/azure/jj721951.aspx).

## <a name="use-cloudconfigurationmanager"></a>Käytä CloudConfigurationManager

### <a name="id"></a>TUNNUS

AP4000

### <a name="description"></a>Kuvaus

[ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager(v=vs.110).aspx) -luokan avulla projekteille, kuten Azure verkkosivusto- ja Azure mobile-palvelujen ei esitellä runtime ongelmat. Paras käytäntö on kuitenkin on hyvä käyttää Cloud[ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager(v=vs.110).aspx) yhdistetty tapana käyttömahdollisuudet kaikkien Azure Cloud sovellusten hallinta.

Jaa ideoita ja palautetta osoitteessa [Azure koodin analyysi palautetta](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Syy

CloudConfigurationManager lukee haluamasi sovellus-ympäristöön kokoonpanotiedosto.

[CloudConfigurationManager](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx)

### <a name="solution"></a>Ratkaisu

Refactor koodin käyttämään [CloudConfigurationManager-luokka](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx). Azure koodi-analyysityökalu tarjoaa koodin korjaa ongelman.

Seuraavat koodikatkelman esitellään koodi korjata tämän ongelman. Korvaa

`var settings = ConfigurationManager.AppSettings["mySettings"];`

kanssa

`var settings = CloudConfigurationManager.GetSetting("mySettings");`

Tässä on esimerkki siitä, miten voit tallentaa hakumäärityksen asetus App.config tai Web.config-tiedoston. Lisää asetuksia määritystiedoston appSettings-osaan. Seuraavassa on seuraavan koodin korostetut koodin edellisessä esimerkissä varten.

```
<appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="mySettings" value="[put_your_setting_here]"/>
  </appSettings>  
```

## <a name="avoid-using-hard-coded-connection-strings"></a>Vältä koodattu yhteysmerkkijonon

### <a name="id"></a>TUNNUS

AP4001

### <a name="description"></a>Kuvaus

Jos käytät koodattu yhteysmerkkijonon eikä sinun tarvitse päivittää niitä myöhemmin, sinun on muutosten tekeminen lähdekoodia ja Käännä sovellus uudelleen. Jos tallennat yhteyden merkkijonot määritystiedostossa, voit muuttaa niitä myöhemmin päivittämällä yksinkertaisesti määritystiedosto.

Jaa ideoita ja palautetta osoitteessa [Azure koodin analyysi palautetta](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Syy

Koodaus yhteysmerkkijonon on virheellinen käytäntöjä, koska se esittelee ongelmia yhteyden merkkijonot on muutettava nopeasti. Lisäksi projektin on kuitattava hallintaan, jos koodattu yhteysmerkkijonon heikentää suojausta jälkeen merkkijonot voi tarkastella lähdekoodin.

### <a name="solution"></a>Ratkaisu

Tallenna yhteysmerkkijonon tiedostojen tai Azure ympäristössä.

- Erillinen-sovellusten avulla voit tallentaa merkkijonon yhteysasetukset app.config.

- IIS: N isännöimä web-sovellusten avulla voit tallentaa yhteysmerkkijonon web.config.

- ASP.NET vNext sovellusten avulla voit tallentaa yhteysmerkkijonon configuration.json.

Lisätietoja käyttömahdollisuudet tiedostot, kuten web.config tai app.config käyttämisestä on artikkelissa [ASP.NET Web määritysten ohjeita](https://msdn.microsoft.com/library/vstudio/ff400235(v=vs.100).aspx). Lisätietoja siitä, miten Azure ympäristön muuttujat työ on artikkelissa [Azure-sivustoista: kuinka sovelluksen merkkijonot ja yhteys merkkijonot toimi](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). Artikkelissa tietojen tallentaminen yhteysmerkkijonon tietolähteen ohjausobjektin, [Vältä luottamuksellisia tietoja, kuten yhteyden merkkijonoja tiedostoista, jotka on tallennettu lähde koodin säilöön](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control).

## <a name="use-diagnostics-configuration-file"></a>Käytä diagnostiikka kokoonpanotiedosto

### <a name="id"></a>TUNNUS

AP5000

### <a name="description"></a>Kuvaus

Sen sijaan, että määrittäminen diagnostiikan asetusten omassa koodissasi, kuten ohjelmointi API Microsoft.WindowsAzure.Diagnostics avulla, kannattaa määrittää diagnostiikan asetusten diagnostics.wadcfg-tiedostossa. (Tai jos käytössäsi on Azure SDK 2,5 diagnostics.wadcfgx). Näin voit muuttaa diagnostiikan asetusten eikä sinun tarvitse kääntää koodi.

Jaa ideoita ja palautetta osoitteessa [Azure koodin analyysi palautetta](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Syy

Ennen kuin Azure SDK 2,5 (joka käyttää Azure diagnostiikka 1.3), Azure diagnostiikka (WAD) voi määrittää käyttämällä useilla eri tavoilla: lisääminen määritysten Blob-objektien tallennustilaan, käyttämällä välttämättömien koodi, määritettäviä määritysten tai oletusarvo-määritys. Manuaalisten määrittäminen Diagnostiikka on kuitenkin käyttää XML-määritystiedosto (diagnostics.wadcfg tai diagnositcs.wadcfgx SDK 2,5 tai uudempaa versiota) sovelluksen Projectissa. Valitse tämä vaihtoehto diagnostics.wadcfg tiedostoa kokonaan määrittää määritykset ja voit päivitetään ja uudelleen käyttöön milloin. Sekoitus diagnostics.wadcfg määritystiedosto käytön asetus käyttömahdollisuudet ohjelmallisesti menetelmiä [DiagnosticMonitor](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.diagnosticmonitor.aspx)tai [RoleInstanceDiagnosticManager](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.management.roleinstancediagnosticmanager.aspx)luokkien avulla voi aiheuttaa sekaannusta. Lisätietoja on kohdassa [alusta tai muuta Azure diagnostiikan määrityksissä](https://msdn.microsoft.com/library/azure/hh411537.aspx) .

Alkavat WAD 1.3 (sisältyy Azure SDK 2,5), sitä ei enää voi määrittää diagnostiikka koodin avulla. Tuloksena voidaan lisätä vain määritykset otetaan käyttöön tai päivitettäessä diagnostiikka-tunniste.

### <a name="solution"></a>Ratkaisu

Designerilla diagnostiikka määritysten diagnostiikan asetusten Siirry diagnostiikka-kokoonpanotiedosto (diagnositcs.wadcfg tai diagnositcs.wadcfgx SDK 2,5 tai uudempaa versiota). On myös suositeltavaa, että asennat [Azure SDK 2,5](http://go.microsoft.com/fwlink/?LinkId=513188) ja uusimmat diagnostiikka-toiminnolla.

1. Oikeuksia, jotka haluat määrittää hiiren kakkospainikkeella ja valitse Ominaisuudet ja valitse määritys-välilehti.

1. Varmista **Diagnostiikka** -osassa **Ota diagnostiikka** -valintaruutu on valittuna.

1. Valitse **Määritä** -painiketta.

  ![Ota käyttöön diagnostiikka-vaihtoehdon käyttäminen](./media/vs-azure-tools-optimizing-azure-code-in-visual-studio/IC796660.png)

  Lisätietoja on kohdassa [Määrittäminen diagnostiikka Azure pilvipalveluihin ja näennäiskoneiden](vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) .


## <a name="avoid-declaring-dbcontext-objects-as-static"></a>Vältä määritteleminen DbContext objektien staattiseksi

### <a name="id"></a>TUNNUS

AP6000

### <a name="description"></a>Kuvaus

Voit säästää muistia välttää määritteleminen DBContext objektien staattiseksi.

Jaa ideoita ja palautetta osoitteessa [Azure koodin analyysi palautetta](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Syy

DBContext objektit pidä kunkin puhelun kyselyn tuloksista. Staattinen DBContext objekteja ei ole poistettu, kunnes sovelluksen toimialue on purettu. Tämän vuoksi staattista DBContext objektia voit tarjoaman suuria määriä muistia.

### <a name="solution"></a>Ratkaisu

Määritellä DBContext paikallista muuttujaa tai muu kuin staattinen esiintymän kentän, käytä tehtävän ja anna järjestelmän luovuttaa käytön jälkeen.

Seuraavassa esimerkissä MVC ohjauskoneen luokan esitetään käyttämisestä DBContext objekti.

```
public class BlogsController : Controller
    {
        //BloggingContext is a subclass to DbContext        
        private BloggingContext db = new BloggingContext();
        // GET: Blogs
        public ActionResult Index()
        {
            //business logics…
            return View();
        }
        protected override void Dispose(bool disposing)
        {
            if (disposing)
            {
                db.Dispose();
            }
            base.Dispose(disposing);
        }
    }
```

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja optimzing ja vianmääritys Azure sovellukset, katso [vianmääritys verkkosovellukseen Azure sovelluksen-palvelun avulla Visual Studio](./app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).
