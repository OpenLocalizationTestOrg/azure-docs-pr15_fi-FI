<properties
    pageTitle="Pääset alkuun käyttämällä .NET Azure jonon tallennustilan | Microsoft Azure"
    description="Azure olevien on luotettava, asynkroninen messaging application osien välillä. Cloud tekstiviesti ottaa käyttöön sovelluksen komponentit skaalata erikseen."
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/12/2016"
    ms.author="robinsh"/>

# <a name="get-started-with-azure-queue-storage-using-net"></a>Käyttämällä .NET Azure jonon tallennustilan käytön aloittaminen

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Yleiskatsaus

Azure jonon tallennustila on cloud messaging application osien välillä. -Sovellusten akselille suunnitteleminen sovelluksen osat ovat usein erillisen, niin, että ne voi skaalata erikseen. Jonon tallennustilan toimittaa asynkronisen viestinvälityksen välisen sovelluksen osia, onko ne ovat käytössä pilvipalvelussa, työpöydällä, paikallisen palvelimessa tai mobiililaitteessa. Jonon tallennustilan tukee myös prosessin työnkulussa kehittämistä sekä asynkroninen tehtävien hallinnasta.

### <a name="about-this-tutorial"></a>Tässä opetusohjelmassa tietoja

Tässä opetusohjelmassa näytetään, miten voit kirjoittaa havainnollistetaan Yleisiä tilanteita, Azure jonon tallennustilan käyttämällä .NET-koodia. Koskee skenaariot ovat luominen ja poistaminen olevien ja lisäämällä, lukeminen ja sanomien poistaminen.

**Arvioitu kesto:** 45 minuuttia

**Prerequisities:**

- [Microsoft Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)
- [.NET Azure tallennustilan asiakkaan kirjasto](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [.NET Azure hallintatoiminto](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
- [Azure-tallennustilan tilin](storage-create-storage-account.md#create-a-storage-account)


[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-namespace-declarations"></a>Lisää nimitilan ilmoitukset

Lisää seuraava `using` lauseet yläreunaan `program.cs` tiedosto:

    using Microsoft.Azure; // Namespace for CloudConfigurationManager
    using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
    using Microsoft.WindowsAzure.Storage.Queue; // Namespace for Queue storage types

### <a name="parse-the-connection-string"></a>Yhteysmerkkijonon jäsentää

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-queue-service-client"></a>Luo jonon service-asiakas

**CloudQueueClient** -luokan avulla voit hakea olevien jonon tallennustilaan tallennettuja. Näin voit luoda palvelun-asiakasohjelmaa:

    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

Nyt olet valmis, joka lukee tietoja ja kirjoittaa tietoja jonossa tallennustilan koodin kirjoittaminen.

## <a name="create-a-queue"></a>Luo jono

Tässä esimerkissä näytetään luomisesta jonossa, jos sitä ei ole jo:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a container.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Create the queue if it doesn't already exist
    queue.CreateIfNotExists();

## <a name="insert-a-message-into-a-queue"></a>Viestin lisääminen jonossa

Jos haluat lisätä viestiin aiemmin jonossa, Luo uusi **CloudQueueMessage**. Seuraavaksi kutsua **AddMessage** -menetelmää. **CloudQueueMessage** voi luoda merkkijono (UTF-8-muodossa) tai **DBCS** -matriisi. Näin koodi, joka luo jonon (jos se ei ole) ja lisää viesti "Hei maailma":

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Create the queue if it doesn't already exist.
    queue.CreateIfNotExists();

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    queue.AddMessage(message);

## <a name="peek-at-the-next-message"></a>Vilkaista seuraava viesti

Voit vilkaista jonon edestä suojuksen viestin poistamatta jonossa, **PeekMessage** -menetelmällä.

    // Retrieve storage account from connection string
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Peek at the next message
    CloudQueueMessage peekedMessage = queue.PeekMessage();

    // Display message.
    Console.WriteLine(peekedMessage.AsString);

## <a name="change-the-contents-of-a-queued-message"></a>Jonossa viestin sisällön muuttaminen

Voit muuttaa viestin käytönaikainen jonossa sisällön. Jos viesti on työtehtävä, työtehtävän tilan päivittäminen voi käyttää tätä toimintoa. Seuraava koodi päivittää jonossa viestin uusi sisältö ja määrittää näkyvyyden aikakatkaisun siirtämisestä toiseen 60 sekuntia. Tämä tallentaa viestin liittyviä töitä tilan ja antaa asiakkaan toiseen minuutin jatkaa viestin. Voit käyttää tätä tapaa seurata monivaiheinen työnkulut sanomien, eikä sinun tarvitse aloittaa alusta alusta, jos käsittelyvaiheen epäonnistuu laitteiston ja ohjelmiston virheen vuoksi. Yleensä säilyttää myös uudelleen laskeminen ja jos viestiä yritetään yli *n* kertaa, sinun on poistettava se. Tämä suojaa viesti, joka käynnistää sovelluksen virhe aina, kun se käsitellään.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Get the message from the queue and update the message contents.
    CloudQueueMessage message = queue.GetMessage();
    message.SetMessageContent("Updated contents.");
    queue.UpdateMessage(message,
        TimeSpan.FromSeconds(60.0),  // Make it visible for another 60 seconds.
        MessageUpdateFields.Content | MessageUpdateFields.Visibility);

## <a name="de-queue-the-next-message"></a>Poista jono seuraava viesti

Koodin jonot poistaa viestin jonon seuraavalla tavalla. Kun soitat **GetMessage**, saat seuraavan viestin jonossa. Viestin palauttamien **GetMessage** tulee näkymätön muut viestit luetaan Tämä jono koodiin. Oletusarvoisesti tämä viesti säilyy näkymätön 30 sekuntia. Viestin poistaminen jonossa lopuksi on myös soittaa **DeleteMessage**. Poistaa viestin tavalla varmistaa, että jos koodi ei käsitellä viestiä laitteiston ja ohjelmiston virheen vuoksi, koodisi esiintymässä voi saman sanoma ja yritä uudelleen. Koodisi kutsuu **DeleteMessage** oikealle, kun viesti on käsitelty.

    // Retrieve storage account from connection string
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Get the next message
    CloudQueueMessage retrievedMessage = queue.GetMessage();

    //Process the message in less than 30 seconds, and then delete the message
    queue.DeleteMessage(retrievedMessage);

## <a name="use-async-await-pattern-with-common-queue-storage-apis"></a>Käytä asynkroninen odotettava kuvion yleisiä jonon tallennustilan ohjelmointirajapinnan kanssa

Tässä esimerkissä näytetään asynkroninen odotettava kuvion käyttäminen yleisiä jonon tallennustilan API. Otosten puhelujen kunkin annetun tavoista asynkroninen versionumero kummassakin menetelmässä *asynkroninen* liitteen merkitty. Asynkroninen menetelmää käytettäessä, asynkroninen-odotettava kuvion keskeyttää paikallisen suorittamisen, kunnes kutsu on valmis. Tämä ongelma avulla voit työskennellä, joka auttaa välttämään suorituskyvyn pullonkaulojen ja parantaa sovelluksen vasteaikaa yleinen nykyisen säikeen. Lisätietoja asynkroninen Await kuvion käyttäminen .NET artikkelissa [asynkroninen ja Await (C# ja Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)

    // Create the queue if it doesn't already exist
    if(await queue.CreateIfNotExistsAsync())
    {
        Console.WriteLine("Queue '{0}' Created", queue.Name);
    }
    else
    {
        Console.WriteLine("Queue '{0}' Exists", queue.Name);
    }

    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue the message
    await queue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await queue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete the message
    await queue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## <a name="leverage-additional-options-for-de-queuing-messages"></a>Hyödyntää lisäasetuksia varaustiedoista queuing viestien

Voit mukauttaa viestin hakeminen jonon kahdella tavalla.
Saat ensin viestien (enintään 32). Se voit määrittää pidentää tai lyhentää invisibility aikakatkaisu, sallia koodissa enemmän tai vähemmän aikaa käsitellä täysin jokaisen viestin. Seuraava koodiesimerkki 20 viestien noutaminen yksi kutsussa **GetMessages** menetelmän avulla. Valitse käsittelee **foreach** silmukan jokaisen viestin. Se myös asettaa invisibility aikakatkaisu kustakin viestistä viisi minuuttia. Huomaa, että kaikki viestit samaan aikaan 5 minuuttia-käynnistyy niin jälkeen 5 minuuttia on kulunut **GetMessages**, viestit, joihin ei ole poistettu puhelu tulevat näkyviin uudelleen.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

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

Saat jonossa arvio viestien määrä. **FetchAttributes** menetelmä pyytää jonon-palvelun hakemiseen jonon määritteiden, kuten viestin määrä. **ApproximateMessageCount** -ominaisuus palauttaa noutaa **FetchAttributes** -menetelmällä ilman kutsumista jonon palvelun viimeisen arvon.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Fetch the queue attributes.
    queue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = queue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="delete-a-queue"></a>Poista jono

Poista jono, ja kaikki sen sisältävät viestit, soittaminen **Delete** -menetelmällä jono-objekti.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Delete the queue.
    queue.Delete();

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet tutustunut jonon tallennustilan perusteet, näistä linkeistä saat lisätietoja tallennustilan monimutkaisia tehtäviä.

- Näytä kaikki tiedot jonon service-oppaat käytettävissä ohjelmointirajapinnan tietoja:
    - [Tallennustilan asiakkaan kirjaston .NET-käyttöä varten](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
    - [REST API-viittaus](http://msdn.microsoft.com/library/azure/dd179355)
- Opettele yksinkertaistaa koodi, voit kirjoittaa Azuren tallennustilaan käyttämällä [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md)-käyttöä varten.
- Näytä Lisää ominaisuus apuviivoja Saat lisätietoja lisäasetusten määrittäminen tiedot tallennetaan Azure.
    - [Pääset alkuun käyttämällä .NET Azure-taulukkotallennus](storage-dotnet-how-to-use-tables.md) jäsenneltyjen tietojen tallentamiseen.
    - [Pääset alkuun käyttämällä .NET Azure-Blob-säiliö](storage-dotnet-how-to-use-blobs.md) rakenteeton tietojen tallentamiseen.
    - [Yhteyden muodostaminen SQL-tietokanta käyttämällä .NET (C#)](../sql-database/sql-database-develop-dotnet-simple.md) relaatiotietoja tallentamiseen.

  [Download and install the Azure SDK for .NET]: /develop/net/
  [.NET client library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [Creating a Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx
  [Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [OData]: http://nuget.org/packages/Microsoft.Data.OData/5.0.2
  [Edm]: http://nuget.org/packages/Microsoft.Data.Edm/5.0.2
  [Spatial]: http://nuget.org/packages/System.Spatial/5.0.2
