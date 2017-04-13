<properties
    pageTitle="Java jonon tallennustilaa käyttämisestä | Microsoft Azure"
    description="Opettele Azure jonon-palvelun avulla voit luoda ja poistaa olevien, Lisää, Hae ja poistaa viestit. Esimerkkejä, joiden on kirjoitettu Java."
    services="storage"
    documentationCenter="java"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-queue-storage-from-java"></a>Opi käyttämään Java jonon tallennustilaa

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Yleiskatsaus

Tässä oppaassa kerrotaan, kuinka voit suorittaa yleisiä tilanteita, joissa Azure jonon tallennustilan-palvelun avulla. Mallit on kirjoitettu Java ja käytä [Java Azure tallennustilan SDK-paketissa][]. Tilanteita, joissa kattaa Sisällytä **lisääminen**, **Vilkaiseminen**, **käytön**ja **poistamalla** sanomien sekä olevien **luominen** ja **poistaminen** . Lisätietoja olevien on kohdassa [vaiheisiin](#Next-Steps) .

Huomautus: SDK: ta ei ole käytettävissä sovelluskehittäjille, jotka käyttävät Azuren tallennustilaan Android-laitteisiin. Lisätietoja [Android Azure tallennustilan SDK-paketissa][].

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Java-sovelluksen luominen

Tämän oppaan voit käyttää tallennustilan ominaisuuksia, jotka voidaan suorittaa Java-sovelluksen paikallisesti tai web-roolin tai työntekijän rooli Azure koodin.

Tällöin sinun on asennettava Java Development Kit (JDK) ja Azure-tallennustilan tilin luominen Azure-tilaukseesi. Kun olet tehnyt, sinun on varmistaa, että kehittäminen järjestelmän täyttää vähimmäisvaatimukset ja riippuvuuksia, jotka on lueteltu GitHub [Azure-tallennustilan SDK Java][] -säilössä. Jos järjestelmä täyttää näitä vaatimuksia, voit noudattaa lataaminen ja asentaminen järjestelmään, säilöstä Java Azure-tallennustilan kirjastoja koskevia ohjeita. Kun olet suorittanut sellaiset tehtävät, voi luoda Java-sovellusta, jolla tämän artikkelin esimerkeissä käytetään.

## <a name="configure-your-application-to-access-queue-storage"></a>Sovelluksen jonon tallennuskiintiön käyttämään määrittäminen

Lisää seuraavat Tuontilauseet Java-tiedoston kohtaa, johon haluat käyttää Azure-tallennustilan ohjelmointirajapinnan olevien alkuun:

    // Include the following imports to use queue APIs.
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.queue.*;

## <a name="setup-an-azure-storage-connection-string"></a>Azure-tallennustilan-yhteysmerkkijonon asetukset

Azure-tallennustilan-asiakas käyttää tallennustilan yhteysmerkkijonon päätepisteet ja data management Services-palvelun käyttäminen tunnistetietojen tallentamiseen. Kun asiakassovellus, sinun on määritettävä tallennustilan yhteysmerkkijonon muodossa, käyttämällä *AccountName* ja *AccountKey* arvojen [Azure-portaalissa](https://portal.azure.com) näkyvä tallennustilan tilin nimi ja tallennustilaa tilisi ensisijainen pikanäppäin. Tässä esimerkissä näytetään, miten kielessä pitoon yhteysmerkkijonon staattinen kentän:

    // Define the connection-string with your values.
    public static final String storageConnectionString =
        "DefaultEndpointsProtocol=http;" +
        "AccountName=your_storage_account;" +
        "AccountKey=your_storage_account_key";

Microsoft Azure-roolin sisällä suoritettavien sovelluksen merkkijono voidaan tallentaa service-kokoonpanotiedosto *ServiceConfiguration.cscfg*, ja niitä voi käyttää puhelun **RoleEnvironment.getConfigurationSettings** tapaan. Tässä on esimerkki yhteysmerkkijonon käytön **asetus** osan nimeltä *StorageConnectionString* palvelun määritystiedostossa:

    // Retrieve storage account from connection-string.
    String storageConnectionString =
        RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");

Seuraavat esimerkit oletetaan, että olet käyttänyt jokin näistä tavoista saat tallennustilan yhteysmerkkijonon.

## <a name="how-to-create-a-queue"></a>Toimintaohje: jonon luominen

**CloudQueueClient** objektin voit hankkia viittaus objektien olevien. Seuraava koodi luo **CloudQueueClient** objektin. (Huomautus: muilla tavoilla voit luoda **CloudStorageAccount** objekteja, Lisätietoja on artikkelissa **CloudStorageAccount** [Azure tallennustilan Client SDK-viittaus].)

Viittaaminen jonossa, jota haluat käyttää **CloudQueueClient** -objektin avulla. Voit luoda jonossa, jos sitä ei ole.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

       // Create the queue client.
       CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

       // Retrieve a reference to a queue.
       CloudQueue queue = queueClient.getQueueReference("myqueue");

       // Create the queue if it doesn't already exist.
       queue.createIfNotExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-add-a-message-to-a-queue"></a>Toimintaohje: jonon lisääminen viestiin

Jos haluat lisätä viestiin aiemmin jonossa, Luo uusi **CloudQueueMessage**. Seuraavaksi kutsua **addMessage** -menetelmää. **CloudQueueMessage** voi luoda merkkijono (UTF-8-muodossa) tai DBCS-matriisi. Näin koodi, joka luo jonon (jos se ei ole) ja lisää viesti "Hei maailma".

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Create the queue if it doesn't already exist.
        queue.createIfNotExists();

        // Create a message and add it to the queue.
        CloudQueueMessage message = new CloudQueueMessage("Hello, World");
        queue.addMessage(message);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-peek-at-the-next-message"></a>Toimintaohje: vilkaista seuraava viesti

Voit vilkaista jonon edestä suojuksen viestin poistamatta jonossa, soittamalla **peekMessage**.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Peek at the next message.
        CloudQueueMessage peekedMessage = queue.peekMessage();

        // Output the message value.
        if (peekedMessage != null)
        {
          System.out.println(peekedMessage.getMessageContentAsString());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Toimintaohje: jonossa viestin sisällön muuttaminen

Voit muuttaa viestin käytönaikainen jonossa sisällön. Jos viesti on työtehtävä, työtehtävän tilan päivittäminen voi käyttää tätä toimintoa. Seuraava koodi päivittää jonossa viestin uusi sisältö ja määrittää näkyvyyden aikakatkaisun siirtämisestä toiseen 60 sekuntia. Tämä tallentaa viestin liittyviä töitä tilan ja antaa asiakkaan toiseen minuutin jatkaa viestin. Voit käyttää tätä tapaa seurata monivaiheinen työnkulut sanomien, eikä sinun tarvitse aloittaa alusta alusta, jos käsittelyvaiheen epäonnistuu laitteiston ja ohjelmiston virheen vuoksi. Yleensä säilyttää myös uudelleen laskeminen ja jos viestiä yritetään yli *n* kertaa, sinun on poistettava se. Tämä suojaa viesti, joka käynnistää sovelluksen virhe aina, kun se käsitellään.

Seuraava koodi otoksen etsii kautta jonossa olevien viestien määrittää ensimmäisen viestin, joka vastaa "Hei maailma" sen sisältöä ja valitse Muokkaa viestin sisältö ja lopettaa.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // The maximum number of messages that can be retrieved is 32.
        final int MAX_NUMBER_OF_MESSAGES_TO_PEEK = 32;

        // Loop through the messages in the queue.
        for (CloudQueueMessage message : queue.retrieveMessages(MAX_NUMBER_OF_MESSAGES_TO_PEEK,1,null,null))
        {
            // Check for a specific string.
            if (message.getMessageContentAsString().equals("Hello, World"))
            {
                // Modify the content of the first matching message.
                message.setMessageContent("Updated contents.");
                // Set it to be visible in 30 seconds.
                EnumSet<MessageUpdateFields> updateFields =
                    EnumSet.of(MessageUpdateFields.CONTENT,
                    MessageUpdateFields.VISIBILITY);
                // Update the message.
                queue.updateMessage(message, 30, updateFields, null, null);
                break;
            }
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

Seuraava koodi-malli voit myös päivittää vain jonossa näkyvissä ensimmäisen viestin.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve the first visible message in the queue.
        CloudQueueMessage message = queue.retrieveMessage();

        if (message != null)
        {
            // Modify the message content.
            message.setMessageContent("Updated contents.");
            // Set it to be visible in 60 seconds.
            EnumSet<MessageUpdateFields> updateFields =
                EnumSet.of(MessageUpdateFields.CONTENT,
                MessageUpdateFields.VISIBILITY);
            // Update the message.
            queue.updateMessage(message, 60, updateFields, null, null);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-get-the-queue-length"></a>Toimintaohje: Hae jonon pituuden

Saat jonossa arvio viestien määrä. **DownloadAttributes** menetelmä pyytää useita nykyiset arvot, mukaan lukien määrä, kuinka monta viestit ovat jonossa jonon palvelu. Määrä on vain lähes, koska viestien voi lisätä tai poistaa jälkeen jonon palvelu vastaa puhelinnumeroon. **GetApproximateMessageCount** -menetelmä palauttaa noutamien **downloadAttributes**puhelu ilman kutsumista jonon palvelun viimeisen arvon.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

       // Download the approximate message count from the server.
        queue.downloadAttributes();

        // Retrieve the newly cached approximate message count.
        long cachedMessageCount = queue.getApproximateMessageCount();

        // Display the queue length.
        System.out.println(String.format("Queue length: %d", cachedMessageCount));
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-dequeue-the-next-message"></a>Toimintaohje: jonosta poistamisen seuraava viesti

Koodin dequeues osoitteesta jonon seuraavalla tavalla. Kun soitat **retrieveMessage**, saat seuraavan viestin jonossa. Viestin palauttamien **retrieveMessage** lukuoikeudet muut viestit luetaan Tämä jono koodi. Oletusarvoisesti tämä viesti säilyy näkymätön 30 sekuntia. Viestin poistaminen jonossa lopuksi on myös soittaa **deleteMessage**. Poistaa viestin tavalla varmistaa, että jos koodi ei käsitellä viestiä laitteiston ja ohjelmiston virheen vuoksi, koodisi esiintymässä voi saman sanoma ja yritä uudelleen. Koodisi kutsuu **deleteMessage** oikealle, kun viesti on käsitelty.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve the first visible message in the queue.
        CloudQueueMessage retrievedMessage = queue.retrieveMessage();

        if (retrievedMessage != null)
        {
            // Process the message in less than 30 seconds, and then delete the message.
            queue.deleteMessage(retrievedMessage);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }


## <a name="additional-options-for-dequeuing-messages"></a>Lisäasetusten määrittäminen dequeuing viestit

Voit mukauttaa viestin hakeminen jonon kahdella tavalla. Saat ensin viestien (enintään 32). Se voit määrittää pidentää tai lyhentää invisibility aikakatkaisu, sallia koodissa enemmän tai vähemmän aikaa käsitellä täysin jokaisen viestin.

Seuraava koodiesimerkki 20 viestien noutaminen yksi kutsussa **retrieveMessages** menetelmän avulla. Valitse käsittelee jokaisen viestin avulla **Saat** silmukan. Se myös asettaa invisibility aikakatkaisu viisi minuuttia (300 sekuntia) kuhunkin viestiin. Huomaa, että viisi minuuttia käynnistyy kaikkien viestien samanaikaisesti, niin kun viisi minuuttia on kulunut **retrieveMessages**puhelu, viestit, joihin ei ole poistettu tulevat näkyviin uudelleen.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
        for (CloudQueueMessage message : queue.retrieveMessages(20, 300, null, null)) {
            // Do processing for all messages in less than 5 minutes,
            // deleting each message after processing.
            queue.deleteMessage(message);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-list-the-queues"></a>Toimintaohje: luettelo olevien

Voit hankkia nykyisen olevien luettelo-kutsu **CloudQueueClient.listQueues()** menetelmää, jotka palauttavat joukko **CloudQueue** objekteja.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient =
            storageAccount.createCloudQueueClient();

        // Loop through the collection of queues.
        for (CloudQueue queue : queueClient.listQueues())
        {
            // Output each queue name.
            System.out.println(queue.getName());
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-delete-a-queue"></a>Toimintaohje: jonon poistaminen

Poista jono, ja kaikki sen sisältävät viestit, kutsua **CloudQueue** objektin **deleteIfExists** -menetelmää.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Delete the queue if it exists.
        queue.deleteIfExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet tutustunut jonon tallennustilan perusteet, näistä linkeistä saat lisätietoja tallennustilan monimutkaisia tehtäviä.

- [Azure-tallennustilan SDK Java][]
- [Azure-tallennustilan asiakkaan SDK-ohje][]
- [Azure liittyviä palveluja REST-Ohjelmointirajapinnalla][]
- [Azure-tallennustilan tiimin blogia][]

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure-tallennustilan SDK Java]: https://github.com/azure/azure-storage-java
[Azure-tallennustilan SDK androidille]: https://github.com/azure/azure-storage-android
[Azure-tallennustilan asiakkaan SDK-ohje]: http://dl.windowsazure.com/storage/javadoc/
[Azure liittyviä palveluja REST-Ohjelmointirajapinnalla]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure-tallennustilan tiimin blogia]: http://blogs.msdn.com/b/windowsazurestorage/
