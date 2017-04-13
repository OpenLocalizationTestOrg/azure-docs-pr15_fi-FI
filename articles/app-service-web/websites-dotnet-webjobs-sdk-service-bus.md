<properties 
    pageTitle="Voit käyttää Azure palvelun Bus WebJobs SDK-paketissa" 
    description="Opettele käyttämään Azure palvelun Bus olevien ja aiheet WebJobs SDK: N kanssa." 
    services="app-service\web, service-bus" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="06/01/2016" 
    ms.author="tdykstra"/>

# <a name="how-to-use-azure-service-bus-with-the-webjobs-sdk"></a>Voit käyttää Azure palvelun Bus WebJobs SDK-paketissa

## <a name="overview"></a>Yleiskatsaus

Tässä oppaassa on C# MALLIKOODEJA, jotka näyttävät käynnistämisestä prosessin Azure palvelun Bus viestin saapuessa. MALLIKOODEJA Käytä [WebJobs SDK](websites-dotnet-webjobs-sdk.md) -versiota 1.x.

Oppaan oletetaan, että tiedät, [kuinka voit luoda yhteyden merkkijonoja, jotka osoittavat tallennustilan tilin Visual Studiossa WebJob projektin](websites-dotnet-webjobs-sdk-get-started.md).

Koodikatkelmat Näytä vain Funktiot, ei ole, luo koodi `JobHost` objektin, kuten tässä esimerkissä:

```
public class Program
{
   public static void Main()
   {
      JobHostConfiguration config = new JobHostConfiguration();
      config.UseServiceBus();
      JobHost host = new JobHost(config);
      host.RunAndBlock();
   }
}
```

[Valmis palvelun Bus Esimerkki](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Program.cs) on GitHub.com azure-webjobs-sdk-mallit-säilössä.

## <a id="prerequisites"></a>Edellytykset

Palvelun Bus-käyttöä varten sinun on lisäksi WebJobs SDK-pakettien [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/) NuGet-paketin asentaminen. 

Sinun on myös asettaa AzureWebJobsServiceBus yhteysmerkkijonon lisäksi tallennustilan yhteyden merkkijonoja.  Voit tehdä tämän `connectionStrings` osa App.config-tiedostoa, kuten seuraavassa esimerkissä:

        <connectionStrings>
            <add name="AzureWebJobsDashboard" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsServiceBus" connectionString="Endpoint=sb://[yourServiceNamespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[yourKey]"/>
        </connectionStrings>

Esimerkki projektin, joka sisältää palvelun Bus yhteyden merkkijono-asetus App.config-tiedostoa Katso [palvelun Bus esimerkkiä](https://github.com/Azure/azure-webjobs-sdk-samples/tree/master/BasicSamples/ServiceBus). 

Yhteyden merkkijonot voidaan määrittää myös Azure runtime-ympäristössä, joka ohittaa sitten App.config asetukset, kun WebJob Azure; Lisätietoja on artikkelissa [WebJobs SDK: N käytön aloittaminen](websites-dotnet-webjobs-sdk-get-started.md#configure-the-web-app-to-use-your-azure-sql-database-and-storage-account).

## <a id="trigger"></a>Funktion käynnistämisestä palvelun Bus jonon viestin saapuessa

Jos haluat kirjoittaa funktion, joka WebJobs SDK kutsuu jonon viestin saapuessa, käytä `ServiceBusTrigger` määrite. Määritteen konstruktoria parametri, joka määrittää jonon lisääminen nimi.

### <a name="how-servicebustrigger-works"></a>ServiceBusTrigger toiminta

SDK vastaanottaa viestin `PeekLock` tila ja puhelut `Complete` tulee näyttöön, jos funktio on suoritettu onnistuneesti tai puhelut `Abandon` Jos funktio epäonnistuu. Jos funktio suorittaa yli `PeekLock` aikakatkaisu, Lukitse uusitaan automaattisesti.

Palvelun Bus ei omassa torjuttujen jonon käsittely, joka ei voi hallita tai määrittämän WebJobs SDK-paketissa. 

### <a name="string-queue-message"></a>Merkkijono jonon viesti

Seuraava koodi malli lukee jonon sanoman, joka sisältää merkkijonon kirjoittaa merkkijonon WebJobs SDK Raporttinäkymät-ikkunan.

        public static void ProcessQueueMessage([ServiceBusTrigger("inputqueue")] string message, 
            TextWriter logger)
        {
            logger.WriteLine(message);
        }

**Huomautus:** Jos olet luomassa sanomien sovellus, joka ei käytä WebJobs SDK-paketissa, varmista, että määrität [BrokeredMessage.ContentType](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx) "text/plain".

### <a name="poco-queue-message"></a>POCO jonossa viesti

SDK automaattisesti poistaa jonon viestiin, joka sisältää JSON POCO [(Normaali vanha CLR-objekti](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) tyyppi. Seuraava koodi malli lukee, joka sisältää jonon viestin `BlobInformation` objekti, joka on `BlobName` ominaisuus:

        public static void WriteLogPOCO([ServiceBusTrigger("inputqueue")] BlobInformation blobInfo,
            TextWriter logger)
        {
            logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
        }

Katso MALLIKOODEJA esittää, kuinka POCO ominaisuuksien käyttäminen BLOB-objektit ja taulukoita samaan toimintoon, [tallennustilan olevien tämän artikkelin versio](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#pocoblobs).

Jos koodi, joka luo jonon viestiä ei WebJobs SDK-paketissa, käytä koodi samalla tavalla kuin seuraavassa esimerkissä:

        var client = QueueClient.CreateFromConnectionString(ConfigurationManager.ConnectionStrings["AzureWebJobsServiceBus"].ConnectionString, "blobadded");
        BlobInformation blobInformation = new BlobInformation () ;
        var message = new BrokeredMessage(blobInformation);
        client.Send(message);

### <a name="types-servicebustrigger-works-with"></a>Tyypit ServiceBusTrigger toimii

Paitsi useimpien yritysten `string` ja POCO tyypit, voit käyttää `ServiceBusTrigger` DBCS-taulukko-määritteen tai `BrokeredMessage` objekti.

## <a id="create"></a>Palvelun Bus jonon viestien luomisesta

Voit kirjoittaa funktion, joka luo uuden jonon viestin käyttötavan `ServiceBus` määrite ja välittää Määritekonstruktorilla jonossa nimessä. 


### <a name="create-a-single-queue-message-in-a-non-async-function"></a>Luo yhden jonon viesti asynkroninen funktio

Seuraava koodi malli käyttää parametrin uuden viestin luominen nimeltä "outputqueue", jolla on sama sisältö kuin nimeltä "inputqueue" jonossa vastaanotettu viesti jonossa.

        public static void CreateQueueMessage(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] out string outputQueueMessage)
        {
            outputQueueMessage = queueMessage;
        }

Yksittäisen jonon viestin luomista tulostusparametri voi olla mikä tahansa seuraavista:

* `string`
* `byte[]`
* `BrokeredMessage`
* Sarjoitettavissa oleva POCO-tyyppi, joka määritetään. Automaattisesti muuntaa sarjaksi JSON nimellä.

POCO tyyppi parametrien jonon viestin aina luodaan, kun funktio päättyy. Jos parametri on tyhjä, SDK Luo jonon viestin, jotka palauttavat null, kun viesti on vastaanotettu ja poistaa. Muuntyyppisten, jos parametrin arvo on null jonon sanomaa luodaan.

### <a name="create-multiple-queue-messages-or-in-async-functions"></a>Luo useita sanomien tai asynkroninen Funktiot

Voit luoda useita viestejä `ServiceBus` määritteiden kanssa `ICollector<T>` tai `IAsyncCollector<T>`, seuraava koodi näyte esitetyllä tavalla:

        public static void CreateQueueMessages(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

Jonon jokaisen viestin luodaan heti kun `Add` menetelmää kutsutaan.

## <a id="topics"></a>Palvelun Bus aiheet käsittelemisestä

Jos haluat kirjoittaa funktion, joka on SDK soittaa viestin saapuessa palvelun Bus aiheeseen, käytä `ServiceBusTrigger` määritteen konstruktoria, joka on ohjeaiheessa nimen ja tilauksen seuraava koodi näyte esitetyllä tavalla:

        public static void WriteLog([ServiceBusTrigger("outputtopic","subscription1")] string message,
            TextWriter logger)
        {
            logger.WriteLine("Topic message: " + message);
        }

Voit luoda viestin aiheeseen `ServiceBus` määrite aiheen nimellä samalla tavalla kuin voit käyttää sitä jonon nimi.

## <a name="features-added-in-release-11"></a>Version 1.1-ominaisuuksia

Version 1.1 on lisätty seuraavat ominaisuudet:

* Salli laaja mukauttaminen viestin käsittelyn kautta `ServiceBusConfiguration.MessagingProvider`.
* `MessagingProvider`tukee palvelun Bus mukauttaminen `MessagingFactory` ja `NamespaceManager`.
* A `MessageProcessor` strategia mallin avulla voit määrittää suoritin jonon/aiheen kohden.
* Viestin käsittely samanaikainen tuetaan oletusarvoisesti. 
* Helppo `OnMessageOptions` kautta `ServiceBusConfiguration.MessageOptions`.
* Salli [AccessRights](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Functions.cs#L71) avulla voidaan määrittää `ServiceBusTriggerAttribute` / `ServiceBusAttribute` (, jos sinulla ei ole skenaariot käyttöoikeuksien hallinta). 

## <a id="queues"></a>Aiheeseen liittyvät tallennustilan olevien toimintaohjeet artikkelin aiheita

Katso tietoja WebJobs SDK tilanteita, joissa ei koske palvelun Bus [käyttämisestä Azure jonon tallennustilan WebJobs SDK: N kanssa](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

Artikkelin aiheita ovat seuraavat:

* Asynkroninen Funktiot
* Useita kertoja
* Kaksivaiheista sulkeminen
* Käytä funktiota tekstissä WebJobs SDK määritteet
* Määritä SDK yhteyden merkkijonot koodi
* Määritä arvot WebJobs SDK konstruktori parametrit-koodi
* Funktion käynnistäminen manuaalisesti
* Kirjoita lokit

## <a id="nextsteps"></a>Seuraavat vaiheet

Tässä oppaassa on tarjonnut MALLIKOODEJA, jotka esittävät yleisiä tilanteita, joissa käyttäminen Azure palvelun Bus käsittelemisestä. Saat lisätietoja Azure WebJobs ja WebJobs SDK käyttäminen [Azure WebJobs suositellaan resurssit](http://go.microsoft.com/fwlink/?linkid=390226).
 
