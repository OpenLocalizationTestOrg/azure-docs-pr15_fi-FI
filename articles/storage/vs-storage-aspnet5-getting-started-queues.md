<properties
    pageTitle="Aloittaminen jonon tallennustilan ja Visual Studio yhdistetyt palvelut (ASP.NET-5) | Microsoft Azure"
    description="Pikaviestien käyttäminen Azure jonon tallennustilan Visual Studio ASP.NET-5-projektiksi"
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="tarcher"/>

# <a name="get-started-with-queue-storage-and-visual-studio-connected-services-aspnet-5"></a>Aloittaminen jonon tallennustilan ja Visual Studio yhdistetyt palvelut (ASP.NET-5)

[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

##<a name="overview"></a>Yleiskatsaus

Tässä artikkelissa käsitellään käyttäminen Azure jonon tallennustilan Visual Studiossa, sen jälkeen, kun olet luonut tai Viitattu Azure-tallennustilan tilin ASP.NET-5-Projectin Visual Studio **Lisää yhdistetyt palvelut** -valintaikkunan. **Lisää yhdistetyt palvelut** -toiminnon asentaa tarvittavat NuGet paketit, voit käyttää projektin Azure tallennustilan ja lisää yhteysmerkkijonon tallennustilan tilin määritys projektitiedostoihin.

Azure jonon tallennustila on palvelu, projektiin paljon viestejä, jotka voi käyttää mitä tahansa maailmanlaajuisesti todennetut puhelujen soittaminen HTTP tai HTTPS kautta. Yksittäisen jonon viesti voi olla enintään 64 kilotavun kokoinen ja jono voi sisältää miljoonia viestit-tallennustilan tilin kokonaiskapasiteetti rajoissa.

Aluksi sinun on Azure jonon luominen tallennustilan-tilillesi. Annamme ohjeet jonon luominen koodi. Myös esitellään että tietoja basic jonon toimintoja, kuten lisääminen, muokkaaminen, lukeminen ja sanomien poistaminen. Mallit on kirjoitettu C\# koodi ja käyttää Azure-tallennustilan asiakkaan kirjaston .NET. Saat lisätietoja ASP.NET [ASP.NET](http://www.asp.net).

**Huomautus:** Jotkin API, jotka suorittavat Azure tallennustilan puhelut ASP.NET-5 ovat asynkroninen. Lisätietoja on kohdassa [asynkroninen ohjelmointi asynkroninen ja Await](http://msdn.microsoft.com/library/hh191443.aspx) . Seuraava koodi olettaa asynkroninen selaimessa käytetään.

- Saat lisätietoja käsittelevistä ohjelmallisesti olevien [Azure jonon tallennustilan käyttämällä .NET käytön aloittaminen](storage-dotnet-how-to-use-queues.md) .
- Yleisiä tietoja Azuren tallennustilaan [tallennustilan](https://azure.microsoft.com/documentation/services/storage/) ohjeissa.
- Yleisiä tietoja Azure pilvipalveluihin [Pilvipalveluihin](https://azure.microsoft.com/documentation/services/cloud-services/) ohjeissa.
- Saat lisätietoja ohjelmointi ASP.NET-sovelluksia [ASP.NET](http://www.asp.net) .





##<a name="access-queues-in-code"></a>Access-olevien koodissa

Voit käyttää olevien projektien ASP.NET-5-haluat lisätä kaikki C# lähdetiedosto, joka käyttää Azure jonon tallennustilan seuraavat kohteet.

1. Varmista, että nimitilan ilmoitukset C#-tiedoston yläosassa lauseiden **käyttäminen** .

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;

2. Pyydä **CloudStorageAccount** objekti, joka esittää tallennustilan tilin tiedot. Seuraava koodi avulla saat-tallennustilan yhteysmerkkijonon ja Azure palvelun tallennustilan tilin tiedot.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. Pyydä **CloudQueueClient** -objektin viittaamaan jono-objektien tallennustilaan tilisi.  

        // Create the CloudQueueClient object for the storage account.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

4. Pyydä **CloudQueue** -objektin viittaamaan tietyn jonon.

        // Get a reference to the CloudQueue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");


**Huomautus:** Käyttää kaikkia edellä koodin lukukoodin seuraavat esimerkit.

###<a name="create-a-queue-in-code"></a>Luo jono koodi

Jos Azure jonossa koodi, lisätä **CreateIfNotExistsAsync**puhelun.

    // Create the CloudQueue if it does not exist.
    await messageQueue.CreateIfNotExistsAsync();

##<a name="add-a-message-to-a-queue"></a>Lisää viestiin jonossa

Jos haluat lisätä viestiin aiemmin jonossa, Luo uusi **CloudQueueMessage** -objekti ja valitse **AddMessageAsync** menetelmää.

**CloudQueueMessage** objektin voi luoda merkkijono (UTF-8-muodossa) tai DBCS-matriisi.

Tässä on esimerkki, jossa Lisää viesti "Hei maailma".

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    await messageQueue.AddMessageAsync(message);

##<a name="read-a-message-in-a-queue"></a>Jonossa viestin lukeminen

Voit vilkaista jonon edestä suojuksen viestin poistamatta jonossa, **PeekMessageAsync** -menetelmällä.

    // Peek the next message in the queue. 
    CloudQueueMessage peekedMessage = await messageQueue.PeekMessageAsync();


##<a name="read-and-remove-a-message-in-a-queue"></a>Lue ja poistaa viestin jonossa

Koodin poistaa (jonosta poistamisen) viestin jonon seuraavalla tavalla.
1. Soita **GetMessageAsync** saat seuraavan viestin jonossa. Viestin palauttamien **GetMessageAsync** lukuoikeudet muut viestit luetaan Tämä jono koodi. Oletusarvoisesti tämä viesti säilyy näkymätön 30 sekuntia.
2.  Viestin poistaminen jonossa lopuksi Soita **DeleteMessageAsync**.

Poistaa viestin tavalla varmistaa, että jos koodi ei käsitellä viestiä laitteiston ja ohjelmiston virheen vuoksi, koodisi esiintymässä voi saman sanoma ja yritä uudelleen. Seuraava koodi kutsuu **DeleteMessageAsync** oikealle, kun viesti on käsitelty.

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();

    // Process the message in less than 30 seconds.

    // Then delete the message.
    await messageQueue.DeleteMessageAsync(retrievedMessage);

## <a name="leverage-additional-options-for-dequeuing-messages"></a>Hyödyntää lisäasetuksia dequeuing viestit

Voit mukauttaa viestin hakeminen jonon kahdella tavalla.
Saat ensin viestien (enintään 32). Se voit määrittää pidentää tai lyhentää invisibility aikakatkaisu, sallia koodissa enemmän tai vähemmän aikaa käsitellä täysin jokaisen viestin. Seuraava koodiesimerkki 20 viestien noutaminen yksi kutsussa **GetMessages** menetelmän avulla. Valitse käsittelee **foreach** silmukan jokaisen viestin. Se määrittää myös invisibility aikakatkaisu kustakin viestistä 5 minuuttia. Huomaa, että 5 minuuttia Käynnistä kaikkien viestien samanaikaisesti, niin kun 5 minuuttia on välitetty jälkeen **GetMessages**, kaikki viestit, joihin ei ole poistettu tulevat näkyviin puhelun.

    // Retrieve 20 messages at a time, keeping those messages invisible for 5 minutes, 
    //   delete each message after processing.

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.
        queue.DeleteMessage(message);
    }

## <a name="get-the-queue-length"></a>Hae jonon pituuden

Saat jonossa arvio viestien määrä. **FetchAttributes** menetelmä pyytää jonon-palvelun hakemiseen jonon määritteiden, kuten viestin määrä. **ApproximateMethodCount** -ominaisuus palauttaa noutaa **FetchAttributes** -menetelmällä ilman kutsumista jonon palvelun viimeisen arvon.

    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display the number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-the-async-await-pattern-with-common-queue-apis"></a>Käytä asynkroninen odotettava kuvio, jossa yleisiä jonon API

Tässä esimerkissä näytetään asynkroninen odotettava kuvion käyttäminen yleisiä jonon API. Otosten puhelujen kunkin annetun menetelmistä asynkroninen versionumero. Tämä näkevät asynkroninen jälkeinen korjaus kutakin menetelmää. Asynkroninen-menetelmää käytettäessä, asynkronisten odotettava kuvion keskeyttää paikallisen suorittamisen, kunnes kutsu on valmis. Tämä ongelma avulla voit työskennellä, joka auttaa välttämään suorituskyvyn pullonkaulojen ja parantaa sovelluksen vasteaikaa yleinen nykyisen säikeen. Saat lisätietoja asynkroninen Await kuvion käyttäminen .NET [asynkroninen ja Await (C# ja Visual Basic)] (https://msdn.microsoft.com/library/hh191443.aspx)

    // Create a message to add to the queue.
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue the message.
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message.
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete the message.
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");
## <a name="delete-a-queue"></a>Poista jono

Poista jono, ja kaikki sen sisältävät viestit, soittaminen **Delete** -menetelmällä jono-objekti.

    // Delete the queue.
    messageQueue.Delete();


##<a name="next-steps"></a>Seuraavat vaiheet

[AZURE.INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]
