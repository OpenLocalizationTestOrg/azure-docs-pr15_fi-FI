<properties
    pageTitle="Aloittaminen jonon tallennustilan ja Visual Studio yhdistetyt palvelut (pilvipalveluihin) | Microsoft Azure"
    description="Pikaviestien käyttäminen cloud palvelun projektin Visual Studiossa jonon Azure-tallennustilan, kun yhteyden Visual Studiossa tallennustilan tilin yhdistetyt palvelut"
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

# <a name="getting-started-with-azure-queue-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Azure jonon tallennustilan ja Visual Studio käytön aloittaminen yhdistetyt palvelut (cloud services projektit)

[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Yleiskatsaus

Tässä artikkelissa käsitellään käyttäminen Azure jonon tallennustilan Visual Studiossa, sen jälkeen, kun olet luonut tai Visual Studio **Lisää yhdistetyt palvelut** -valintaikkunan viittaa Azure-tallennustilan tilin cloud services-projektissa.

Annamme ohjeet jonon luominen koodi. Myös esitellään että tietoja basic jonon toimintoja, kuten lisääminen, muokkaaminen, lukeminen ja sanomien poistaminen. Mallit on kirjoitettu C#-koodin ja käytä [Microsoft Azure tallennustilan asiakkaan kirjasto .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

**Lisää yhdistetyt palvelut** -toiminnon asentaa tarvittavat NuGet paketit, voit käyttää projektin Azure tallennustilan ja lisää yhteysmerkkijonon tallennustilan tilin määritys projektitiedostoihin.

 - Saat lisätietoja käsiteltäessä olevien koodissa [Azure jonon tallennustilan käyttämällä .NET käytön aloittaminen](storage-dotnet-how-to-use-queues.md) .
 - Yleisiä tietoja Azuren tallennustilaan [tallennustilan](https://azure.microsoft.com/documentation/services/storage/) ohjeissa.
 - Yleisiä tietoja Azure pilvipalveluihin [Pilvipalveluihin](https://azure.microsoft.com/documentation/services/cloud-services/) ohjeissa.
 - Saat lisätietoja ohjelmointi ASP.NET-sovelluksia [ASP.NET](http://www.asp.net) .


Azure jonon tallennustila on palvelu, projektiin paljon viestejä, jotka voi käyttää mitä tahansa maailmanlaajuisesti todennetut puhelujen soittaminen HTTP tai HTTPS kautta. Yksittäisen jonon viesti voi olla enintään 64 Kilotavun kokoinen ja jono voi sisältää miljoonia viestit-tallennustilan tilin kokonaiskapasiteetti rajoissa.


## <a name="access-queues-in-code"></a>Access-olevien koodissa

Voit käyttää olevien Visual Studio pilvipalveluihin projekteissa, haluat lisätä minkä tahansa C#-lähdetiedostoon seuraavat kohteet, jotka käyttävät jonon Azure-tallennustilan.

1. Varmista, että nimitilan ilmoitukset C#-tiedoston yläosassa lauseiden **käyttäminen** .

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;

2. Pyydä **CloudStorageAccount** objekti, joka esittää tallennustilan tilin tiedot. Seuraava koodi avulla saat-tallennustilan yhteysmerkkijonon ja Azure palvelun tallennustilan tilin tiedot.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. Pyydä **CloudQueueClient** -objektin viittaamaan jono-objektien tallennustilaan tilisi.  

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

4. Pyydä **CloudQueue** -objektin viittaamaan tietyn jonon.

        // Get a reference to a queue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");


**Huomautus:** Käyttää kaikkia edellä koodin lukukoodin seuraavat esimerkit.

## <a name="create-a-queue-in-code"></a>Luo jono koodi

Jos jonossa koodi, lisätä **CreateIfNotExists**puhelun.

    // Create the CloudQueue if it does not exist
    messageQueue.CreateIfNotExists();

## <a name="add-a-message-to-a-queue"></a>Lisää viestiin jonossa

Jos haluat lisätä viestiin aiemmin jonossa, Luo uusi **CloudQueueMessage** -objekti ja valitse **AddMessage** menetelmää.

**CloudQueueMessage** objektin voi luoda merkkijono (UTF-8-muodossa) tai DBCS-matriisi.

Tässä on esimerkki, jossa Lisää viesti "Hei maailma".

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    messageQueue.AddMessage(message);

## <a name="read-a-message-in-a-queue"></a>Jonossa viestin lukeminen

Voit vilkaista jonon edestä suojuksen viestin poistamatta jonossa, **PeekMessage** -menetelmällä.

    // Peek at the next message
    CloudQueueMessage peekedMessage = messageQueue.PeekMessage();

## <a name="read-and-remove-a-message-in-a-queue"></a>Lue ja poistaa viestin jonossa

Koodin poistaa (poistaa jonon) viestin jonon seuraavalla tavalla.

1. Soita **GetMessage** saat seuraavan viestin jonossa. Viestin palauttamien **GetMessage** lukuoikeudet muut viestit luetaan Tämä jono koodi. Oletusarvoisesti tämä viesti säilyy näkymätön 30 sekuntia.
2.  Viestin poistaminen jonossa lopuksi Soita **DeleteMessage**.

Poistaa viestin tavalla varmistaa, että jos koodi ei käsitellä viestiä laitteiston ja ohjelmiston virheen vuoksi, koodisi esiintymässä voi saman sanoma ja yritä uudelleen. Seuraava koodi kutsuu **DeleteMessage** oikealle, kun viesti on käsitelty.

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = messageQueue.GetMessage();

    // Process the message in less than 30 seconds

    // Then delete the message.
    await messageQueue.DeleteMessage(retrievedMessage);


## <a name="use-additional-options-to-process-and-remove-queue-messages"></a>Käsittele ja poistaa sanomien lisäasetusten avulla

Voit mukauttaa viestin hakeminen jonon kahdella tavalla.

 - Saat viestien (enintään 32).
 - Voit määrittää pidentää tai lyhentää invisibility aikakatkaisu, sallia koodissa enemmän tai vähemmän aikaa käsitellä täysin jokaisen viestin. Seuraava koodiesimerkki 20 viestien noutaminen yksi kutsussa **GetMessages** menetelmän avulla. Valitse käsittelee **foreach** silmukan jokaisen viestin. Se myös asettaa invisibility aikakatkaisu kustakin viestistä viisi minuuttia. Huomaa, että kaikki viestit samaan aikaan 5 minuuttia-käynnistyy niin jälkeen 5 minuuttia on kulunut **GetMessages**, viestit, joihin ei ole poistettu puhelu tulevat näkyviin uudelleen.

Tässä on esimerkki:

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.

        // Then delete the message after processing
        messageQueue.DeleteMessage(message);

    }

## <a name="get-the-queue-length"></a>Hae jonon pituuden

Saat jonossa arvio viestien määrä. **FetchAttributes** menetelmä pyytää jonon-palvelun hakemiseen jonon määritteiden, kuten viestin määrä. **ApproximateMethodCount** -ominaisuus palauttaa noutaa **FetchAttributes** -menetelmällä ilman kutsumista jonon palvelun viimeisen arvon.

    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-the-async-await-pattern-with-common-azure-queue-apis"></a>Käytä asynkroninen odotettava kuvio, jossa yleisiä Azure jonon API

Tässä esimerkissä näytetään asynkroninen odotettava kuvion käyttäminen yleisiä Azure jonon API. Otosten soittaa kunkin annetun menetelmistä asynkroninen versionumero, tämä näkevät **asynkroninen** jälkeinen korjaus kutakin menetelmää. Kun asynkroninen menetelmä käytetään asynkronisten-odotettava kuvion keskeyttää paikallisen suorittamisen, kunnes kutsu on valmis. Tämä ongelma avulla voit työskennellä, joka auttaa välttämään suorituskyvyn pullonkaulojen ja parantaa sovelluksen vasteaikaa yleinen nykyisen säikeen. Lisätietoja käytöstä asynkronisen Await kuvion .NET artikkelissa [asynkroninen ja Await (C# ja Visual Basic)] (https://msdn.microsoft.com/library/hh191443.aspx)

    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Add the message asynchronously
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Delete the message asynchronously
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## <a name="delete-a-queue"></a>Poista jono

Poista jono, ja kaikki sen sisältävät viestit, soittaminen **Delete** -menetelmällä jono-objekti.

    // Delete the queue.
    messageQueue.Delete();

## <a name="next-steps"></a>Seuraavat vaiheet

[AZURE.INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]
