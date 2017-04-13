<properties
    pageTitle="Käyttämisestä jonon storage (C++) | Microsoft Azure"
    description="Opettele käyttämään jonon tallennuspalvelu Azure-tietokannassa. Esimerkkejä, joiden on kirjoitettu C++."
    services="storage"
    documentationCenter=".net"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>

# <a name="how-to-use-queue-storage-from-c"></a>Opi käyttämään jonon tallennustilaa C++  

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Yleiskatsaus
Tässä oppaassa kerrotaan, kuinka voit suorittaa yleisiä tilanteita, joissa Azure jonon tallennustilan-palvelun avulla. Mallit kirjoitetaan c++ ja [Azure tallennustilan asiakkaan kirjasto C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md)avulla. Tilanteita, joissa kattaa Sisällytä **lisääminen**, **Vilkaiseminen**, **käytön**ja **poistamalla** niiden sanomien sekä **luominen ja poistaminen olevien**.

>[AZURE.NOTE] Tämän oppaan jotakin Azure-tallennustilan asiakkaan kirjaston C++ versio 1.0.0 ja yläpuolella. Suositellut versio on tallennustilan asiakkaan kirjaston 2.2.0, joka ei ole saatavilla [NuGet](http://www.nuget.org/packages/wastorage) tai [GitHub](http://github.com/Azure/azure-storage-cpp/)kautta.

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]
[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>C++-sovelluksen luominen  
Tämän oppaan voit käyttää tallennustilan ominaisuuksia, jotka voidaan suorittaa C++-sovelluksesta.

Tällöin sinun on Azure-tallennustilan asiakkaan kirjaston for C++ ja Azure-tallennustilan tilin luominen Azure-tilaukseesi.

Voit asentaa Azure-tallennustilan asiakkaan kirjaston C++-seuraavista tavoista:

-   **Linux:** Noudata annettuja [Azure tallennustilan asiakkaan kirjasto c++: n Lueminut](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) -sivulla.
-   **Windows:** Valitse Visual Studion **Työkalut > NuGet Package Manager > Package hallinta-konsolin**. Kirjoita seuraava komento [NuGet Package hallinta-konsolin](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) ja paina **ENTER**-näppäintä.

        Install-Package wastorage

## <a name="configure-your-application-to-access-queue-storage"></a>Sovelluksen jonon tallennuskiintiön käyttämään määrittäminen
Lisää seuraavat ovat lauseet C++ tiedoston kohtaa, johon haluat käyttää Azure tallennustilan ohjelmointirajapinnan olevien alkuun:  

    #include "was/storage_account.h"
    #include "was/queue.h"

## <a name="set-up-an-azure-storage-connection-string"></a>Azure-tallennustilan-yhteysmerkkijonon määrittäminen

Azure-tallennustilan-asiakas käyttää tallennustilan yhteysmerkkijonon päätepisteet ja data management Services-palvelun käyttäminen tunnistetietojen tallentamiseen. Kun asiakassovellus, sinun on määritettävä tallennustilan yhteysmerkkijonon muodossa, käyttämällä *AccountName* ja *AccountKey* arvojen [Azure-portaalissa](https://portal.azure.com) näkyvä tallennustilan tilin nimi tallennustilan-tilisi ja tallennustilaa pikanäppäin. Lisätietoja tallennustilan asiakkaat ja pikanäppäinten on artikkelissa [Tietoja Azure tallennustilan tilit](storage-create-storage-account.md). Tässä esimerkissä näytetään, miten kielessä pitoon yhteysmerkkijonon staattisia kentän:  

    // Define the connection-string with your values.
    const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

Voit esikatsella sovelluksen paikalliseen Windows-tietokoneeseen, voit käyttää Microsoft Azure- [tallennustilan emulaattorin](storage-use-emulator.md) , joka on asennettu [Azure SDK](https://azure.microsoft.com/downloads/). Tallennustilan emulaattori on paikallista kehittämistä tietokoneessasi käytettävissä Azure Blob, jonossa ja taulukon services tulokset vastaavat apuohjelma. Seuraavassa esimerkissä esitetään, miten kielessä pitoon yhteysmerkkijonon paikallinen tallennus emulaattorin staattinen kentän:  

    // Define the connection-string with Azure Storage Emulator.
    const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  

Käynnistäminen Azure tallennustilan emulaattori, valitse **Käynnistä** -painiketta tai paina **Windows** -näppäintä. Aloita kirjoittaminen **Azure-tallennustilan emulaattorin**ja valitse **Microsoft Azure-tallennustilan emulaattorin** sovellusten luettelosta.

Seuraavat esimerkit oletetaan, että olet käyttänyt jokin näistä tavoista saat tallennustilan yhteysmerkkijonon.

## <a name="retrieve-your-connection-string"></a>Noutaminen yhteysmerkkijono
Voit käyttää **cloud_storage_account** luokan edustavan tallennustilan tilin tiedot. Tallennustilan yhteysmerkkijonon noutaminen tallennustilan tilin tiedot, voit **jäsentää** -menetelmää.

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

## <a name="how-to-create-a-queue"></a>Toimintaohje: jonon luominen
**Cloud_queue_client** objektin voit hankkia viittaus objektien olevien. Seuraava koodi luo **cloud_queue_client** objektin.

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create a queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

Viittaaminen jonossa, jota haluat käyttää **cloud_queue_client** -objektin avulla. Voit luoda jonossa, jos sitä ei ole.

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Create the queue if it doesn't already exist.
    queue.create_if_not_exists();  

## <a name="how-to-insert-a-message-into-a-queue"></a>Toimintaohje: jonon lisääminen viestiin
Jos haluat lisätä viestiin aiemmin jonossa, Luo uusi **cloud_queue_message**. Seuraavaksi kutsua **add_message** -menetelmää. **Cloud_queue_message** voidaan luoda merkkijono tai **DBCS** -matriisi. Näin koodi, joka luo jonon (jos se ei ole) ja lisää viesti "Hei maailma":

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Create the queue if it doesn't already exist.
    queue.create_if_not_exists();

    // Create a message and add it to the queue.
    azure::storage::cloud_queue_message message1(U("Hello, World"));
    queue.add_message(message1);  

## <a name="how-to-peek-at-the-next-message"></a>Toimintaohje: vilkaista seuraava viesti
Voit vilkaista jonon edestä suojuksen viestin poistamatta jonossa, **peek_message** -menetelmällä.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Peek at the next message.
    azure::storage::cloud_queue_message peeked_message = queue.peek_message();

    // Output the message content.
    std::wcout << U("Peeked message content: ") << peeked_message.content_as_string() << std::endl;

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Toimintaohje: jonossa viestin sisällön muuttaminen
Voit muuttaa viestin käytönaikainen jonossa sisällön. Jos viesti on työtehtävä, työtehtävän tilan päivittäminen voi käyttää tätä toimintoa. Seuraava koodi päivittää jonossa viestin uusi sisältö ja määrittää näkyvyyden aikakatkaisun siirtämisestä toiseen 60 sekuntia. Tämä tallentaa viestin liittyviä töitä tilan ja antaa asiakkaan toiseen minuutin jatkaa viestin. Voit käyttää tätä tapaa seurata monivaiheinen työnkulut sanomien, eikä sinun tarvitse aloittaa alusta alusta, jos käsittelyvaiheen epäonnistuu laitteiston ja ohjelmiston virheen vuoksi. Yleensä säilyttää myös uudelleen laskeminen ja jos viestiä yritetään n useita kertoja, sinun on poistettava se. Tämä suojaa viesti, joka käynnistää sovelluksen virhe aina, kun se käsitellään.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_conection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Get the message from the queue and update the message contents.
    // The visibility timeout "0" means make it visible immediately.
    // The visibility timeout "60" means the client can get another minute to continue
    // working on the message.
    azure::storage::cloud_queue_message changed_message = queue.get_message();

    changed_message.set_content(U("Changed message"));
    queue.update_message(changed_message, std::chrono::seconds(60), true);

    // Output the message content.
    std::wcout << U("Changed message content: ") << changed_message.content_as_string() << std::endl;  

## <a name="how-to-de-queue-the-next-message"></a>Toimintaohje: jono varaustiedoista seuraava viesti
Koodin jonot poistaa viestin jonon seuraavalla tavalla. Kun soitat **get_message**, saat seuraavan viestin jonossa. Viestin palauttamien **get_message** lukuoikeudet muut viestit luetaan Tämä jono koodi. Viestin poistaminen jonossa lopuksi on myös soittaa **delete_message**. Poistaa viestin tavalla varmistaa, että jos koodi ei käsitellä viestiä laitteiston ja ohjelmiston virheen vuoksi, koodisi esiintymässä voi saman sanoma ja yritä uudelleen. Koodisi kutsuu **delete_message** oikealle, kun viesti on käsitelty.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Get the next message.
    azure::storage::cloud_queue_message dequeued_message = queue.get_message();
    std::wcout << U("Dequeued message: ") << dequeued_message.content_as_string() << std::endl;

    // Delete the message.
    queue.delete_message(dequeued_message);

## <a name="how-to-leverage-additional-options-for-de-queuing-messages"></a>Toimintaohje: hyödyntää lisäasetuksia varaustiedoista queuing viestien
Voit mukauttaa viestin hakeminen jonon kahdella tavalla. Saat ensin viestien (enintään 32). Se voit määrittää pidentää tai lyhentää invisibility aikakatkaisu, sallia koodissa enemmän tai vähemmän aikaa käsitellä täysin jokaisen viestin. Seuraava koodiesimerkki 20 viestien noutaminen yksi kutsussa **get_messages** menetelmän avulla. Valitse käsittelee jokaisen viestin avulla **Saat** silmukan. Se myös asettaa invisibility aikakatkaisu kustakin viestistä viisi minuuttia. Huomaa, että kaikki viestit samaan aikaan 5 minuuttia-käynnistyy niin jälkeen 5 minuuttia on kulunut **get_messages**, viestit, joihin ei ole poistettu puhelu tulevat näkyviin uudelleen.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Dequeue some queue messages (maximum 32 at a time) and set their visibility timeout to
    // 5 minutes (300 seconds).
    azure::storage::queue_request_options options;
    azure::storage::operation_context context;

    // Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
    std::vector<azure::storage::cloud_queue_message> messages = queue.get_messages(20, std::chrono::seconds(300), options, context);

    for (auto it = messages.cbegin(); it != messages.cend(); ++it)
    {
        // Display the contents of the message.
        std::wcout << U("Get: ") << it->content_as_string() << std::endl;
    }

## <a name="how-to-get-the-queue-length"></a>Toimintaohje: Hae jonon pituuden
Saat jonossa arvio viestien määrä. **Download_attributes** menetelmä pyytää jonon-palvelun hakemiseen jonon määritteiden, kuten viestin määrä. **Approximate_message_count** -menetelmä saa jonossa lähes viestien määrä.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Fetch the queue attributes.
    queue.download_attributes();

    // Retrieve the cached approximate message count.
    int cachedMessageCount = queue.approximate_message_count();

    // Display number of messages.
    std::wcout << U("Number of messages in queue: ") << cachedMessageCount << std::endl;  

## <a name="how-to-delete-a-queue"></a>Toimintaohje: jonon poistaminen
Poista jono, ja kaikki sen sisältävät viestit, kutsua jonon objektin **delete_queue_if_exists** -menetelmää.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // If the queue exists and delete it.
    queue.delete_queue_if_exists();  

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet tutustunut jonon tallennustilan perusteet, näistä linkeistä saat lisätietoja Azure-tallennustilan.

-   [Opi käyttämään C++-Blob-objektien tallennustilaan](storage-c-plus-plus-how-to-use-blobs.md)
-   [Opi käyttämään C++-taulukkotallennus](storage-c-plus-plus-how-to-use-tables.md)
-   [Azure-tallennustilan resurssit ovat c++](storage-c-plus-plus-enumeration.md)
-   [Tallennustilan asiakkaan kirjasto C++-Ohje](http://azure.github.io/azure-storage-cpp)
-   [Azure-tallennustilan dokumentaatio](https://azure.microsoft.com/documentation/services/storage/)

