<properties
    pageTitle="Aloittaminen jonon tallennustilan ja Visual Studio yhdistetyt palvelut (ASP.NET) | Microsoft Azure"
    description="Pikaviestien käyttäminen Visual Studio ASP.NET-projektiksi jonon Azure-tallennustilan, kun yhteyden Visual Studiossa tallennustilan tilin yhdistetyt palvelut"
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
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="get-started-with-azure-queue-storage-and-visual-studio-connected-services"></a>Aloita Azure jono tallennustilan ja Visual Studio yhdistetyt palvelut

[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Yleiskatsaus

Tässä artikkelissa kuvataan, miten käyttäminen Azure jonon tallennustilan Visual Studiossa, sen jälkeen, kun olet luonut tai Viitattu Azure-tallennustilan tilin ASP.NET-Projectin Visual Studio **Lisää yhdistetyt palvelut** -valintaikkunan.

Annamme ohjeet voit luoda ja käyttää Azure-jonossa tallennustilan tilissäsi. Myös esitellään että tietoja basic jonon toimintoja, kuten lisääminen, muokkaaminen, lukeminen ja sanomien poistaminen. Mallit on kirjoitettu C#-koodin ja käytä [Microsoft Azure tallennustilan asiakkaan kirjasto .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx). Saat lisätietoja ASP.NET [ASP.NET](http://www.asp.net).

Azure jonon tallennustila on palvelu, projektiin paljon viestejä, jotka voi käyttää mitä tahansa maailmanlaajuisesti todennetut puhelujen soittaminen HTTP tai HTTPS kautta. Yksittäisen jonon viesti voi olla enintään 64 Kilotavun kokoinen ja jono voi sisältää miljoonia viestit-tallennustilan tilin kokonaiskapasiteetti rajoissa.

## <a name="access-queues-in-code"></a>Access-olevien koodissa

Voit käyttää olevien projektien ASP.NET-haluat lisätä minkä tahansa C#-lähdetiedostoon seuraavat kohteet, jotka käyttävät jonon Azure-tallennustilan.

1. Varmista, että nimitilan ilmoitukset C#-tiedoston yläosassa lauseiden **käyttäminen** .

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;

2. Pyydä **CloudStorageAccount** objekti, joka esittää tallennustilan tilin tiedot. Seuraava koodi avulla saat-tallennustilan yhteysmerkkijonon ja Azure palvelun tallennustilan tilin tiedot.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. Pyydä **CloudQueueClient** -objektin viittaamaan jono-objektien tallennustilaan tilisi.  

        // Create the CloudQueueClient object for this storage account.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

4. Pyydä **CloudQueue** -objektin viittaamaan tietyn jonon.

        // Get a reference to a queue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");


**Huomautus** Käyttää kaikkia edellä koodin lukukoodin seuraavat esimerkit.

## <a name="create-a-queue-in-code"></a>Luo jono koodi

Azure jonon luominen koodi, Lisää **CreateIfNotExists** puhelun yllä koodiin.

    // Create the messageQueue if it does not exist
    messageQueue.CreateIfNotExists();

## <a name="add-a-message-to-a-queue"></a>Lisää viestiin jonossa

Jos haluat lisätä viestiin aiemmin jonossa, Luo uusi **CloudQueueMessage** -objekti ja valitse **AddMessage** menetelmää.

**CloudQueueMessage** objektin voi luoda merkkijono (UTF-8-muodossa) tai DBCS-matriisi.

Tässä on esimerkki, jossa Lisää viesti "Hei maailma".

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    messageQueue.AddMessage(message);

## <a name="read-a-message-in-a-queue"></a>Jonossa viestin lukeminen

Voit vilkaista jonon edestä suojuksen viestin poistamatta jonossa, PeekMessage()-menetelmällä.

    // Peek at the next message
    CloudQueueMessage peekedMessage = messageQueue.PeekMessage();

## <a name="read-and-remove-a-message-in-a-queue"></a>Lue ja poistaa viestin jonossa

Koodin poistaa (poistaa jonon) viestin jonon seuraavalla tavalla.
1. Soita GetMessage() tulee seuraava sanoma jonossa. Viestin palauttamien GetMessage() lukuoikeudet muut viestit luetaan Tämä jono koodi. Oletusarvoisesti tämä viesti säilyy näkymätön 30 sekuntia.
2.  Viestin poistaminen jonossa lopuksi Soita **DeleteMessage**.

Poistaa viestin tavalla varmistaa, että jos koodi ei käsitellä viestiä laitteiston ja ohjelmiston virheen vuoksi, koodisi esiintymässä voi saman sanoma ja yritä uudelleen. Seuraava koodi kutsuu **DeleteMessage** oikealle, kun viesti on käsitelty.

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = messageQueue.GetMessage();

    // Process the message in less than 30 seconds

    // Then delete the message.
    await messageQueue.DeleteMessage(retrievedMessage);


## <a name="use-additional-options-for-de-queuing-messages"></a>Käyttää lisäasetuksia varaustiedoista queuing viestit

Voit mukauttaa viestin hakeminen jonon kahdella tavalla.
Saat ensin viestien (enintään 32). Se voit määrittää pidentää tai lyhentää invisibility aikakatkaisu, sallia koodissa enemmän tai vähemmän aikaa käsitellä täysin jokaisen viestin. Seuraava koodiesimerkki 20 viestien noutaminen yksi kutsussa **GetMessages** menetelmän avulla. Valitse käsittelee **foreach** silmukan jokaisen viestin. Se myös asettaa invisibility aikakatkaisu kustakin viestistä viisi minuuttia. Huomaa, että kaikki viestit samaan aikaan 5 minuuttia-käynnistyy niin jälkeen 5 minuuttia on kulunut **GetMessages**, viestit, joihin ei ole poistettu puhelu tulevat näkyviin uudelleen.

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    foreach (CloudQueueMessage message in queue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.
        queue.DeleteMessage(message);
    }

## <a name="get-the-queue-length"></a>Hae jonon pituuden

Saat jonossa arvio viestien määrä. **FetchAttributes** menetelmä pyytää queueservice hakemiseen jonon määritteiden, kuten viestin määrä. **ApproximateMethodCount** -ominaisuus palauttaa noutaa **FetchAttributes** -menetelmällä ilman kutsumista queueservice viimeisen arvon.

    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-async-await-pattern-with-common-queueapis"></a>Yleisiä queueAPIs asynkroninen odotettava kuvion käyttäminen

Tässä esimerkissä näytetään asynkroninen odotettava kuvion käyttäminen yleisiä queueAPIs. Otosten soittaa kunkin annetun menetelmistä asynkroninen versionumero, tämä näkevät asynkroninen jälkeinen korjaus kutakin menetelmää. Kun asynkroninen menetelmä käytetään asynkronisten-odotettava kuvion keskeyttää paikallisen suorittamisen, kunnes kutsu on valmis. Tämä ongelma avulla voit työskennellä, joka auttaa välttämään suorituskyvyn pullonkaulojen ja parantaa sovelluksen vasteaikaa yleinen nykyisen säikeen. Lisätietoja käytöstä asynkronisen Await kuvion .NET artikkelissa [asynkroninen ja Await (C# ja Visual Basic)] (https://msdn.microsoft.com/library/hh191443.aspx)

    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue the message
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete the message
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## <a name="delete-a-queue"></a>Poista jono

Poista jono, ja kaikki sen sisältävät viestit, soittaminen **Delete** -menetelmällä jono-objekti.

    // Delete the queue.
    messageQueue.Delete();

## <a name="next-steps"></a>Seuraavat vaiheet

[AZURE.INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]