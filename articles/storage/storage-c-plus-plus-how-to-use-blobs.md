<properties
    pageTitle="Opi käyttämään blob-säiliö (objektin tallennus) C++ | Microsoft Azure"
    description="Tallentaa erimuotoisia tietoja pilveen Azure-Blob-säiliö (objektin tallennus) kanssa."
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

# <a name="how-to-use-blob-storage-from-c"></a>Opi käyttämään C++-Blob-objektien tallennustilaan  

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Yleiskatsaus

Azure-Blob-säiliö on palvelu, joka tallentaa tiedot rakenteeton pilvipalvelussa objektit-BLOB-objektit. Blob-objektien tallennustilaan voit tallentaa minkä tyyppisessä tekstiä tai binaarinen tietoja, kuten asiakirjan, mediatiedosto tai sovelluksen asennusohjelma. Blob-objektien tallennustilaan kutsutaan myös objektin säilön.

Tämän oppaan näytetään, miten voit suorittaa yleisiä tilanteita, joissa Azure Blob storage-palvelun avulla. Mallit kirjoitetaan c++ ja [Azure tallennustilan asiakkaan kirjasto C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md)avulla. Koskee skenaariot ovat **lataaminen**, **luettelon**, **lataaminen**ja **poistaminen** BLOB-objektit.  

>[AZURE.NOTE] Tämän oppaan jotakin Azure-tallennustilan asiakkaan kirjaston C++ versio 1.0.0 ja yläpuolella. Suositellut versio on tallennustilan asiakkaan kirjaston 2.2.0, joka ei ole saatavilla [NuGet](http://www.nuget.org/packages/wastorage) tai [GitHub](https://github.com/Azure/azure-storage-cpp)kautta.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]
[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>C++-sovelluksen luominen
Tämän oppaan voit käyttää tallennustilan ominaisuuksia, jotka voidaan suorittaa C++-sovelluksesta.  

Tällöin sinun on Azure-tallennustilan asiakkaan kirjaston for C++ ja Azure-tallennustilan tilin luominen Azure-tilaukseesi.   

Voit asentaa Azure-tallennustilan asiakkaan kirjaston C++-seuraavista tavoista:

-   **Linux:** Noudata annettuja [Azure tallennustilan asiakkaan kirjasto c++: n Lueminut](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) -sivulla.  
-   **Windows:** Valitse Visual Studion **Työkalut > NuGet Package Manager > Package hallinta-konsolin**. Kirjoita seuraava komento [NuGet Package hallinta-konsolin](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) ja paina **ENTER**-näppäintä.  

        Install-Package wastorage

## <a name="configure-your-application-to-access-blob-storage"></a>Määrittää sovelluksesi käyttämään Blob-objektien tallennustilaan  
Lisää seuraavat ovat lauseet haluamaasi käyttämään Azure tallennustilan ohjelmointirajapinnan BLOB C++ tiedoston alkuun:  

    #include "was/storage_account.h"
    #include "was/blob.h"

## <a name="setup-an-azure-storage-connection-string"></a>Azure-tallennustilan-yhteysmerkkijonon asetukset
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

Seuraavaksi haluat viitata **cloud_blob_client** luokan, kun sen avulla voit hakea objekteja, jotka edustavat säilöjen ja Blob-Tallennuspalvelu tallennettuna BLOB. Seuraava koodi luo **cloud_blob_client** objektin tallennuspaikka on haettu yläpuolella tilin avulla:  

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();  

## <a name="how-to-create-a-container"></a>Toimintaohje: luoda säilön

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Tässä esimerkissä näytetään, miten voit luoda säilön, jos sitä ei ole jo:  

    try
    {
        // Retrieve storage account from connection string.
        azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

        // Create the blob client.
        azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

        // Retrieve a reference to a container.
        azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

        // Create the container if it doesn't already exist.
        container.create_if_not_exists();
    }
    catch (const std::exception& e)
    {
        std::wcout << U("Error: ") << e.what() << std::endl;
    }  

Oletusarvon mukaan uusi säilö on yksityinen ja sinun on määritettävä tallennustilan access-näppäintä, voit ladata tämän säilön BLOB-objektit. Jos haluat määrittää tiedostot (BLOB) säilöön käytettäviksi kaikille, voit määrittää säilö on julkinen käyttämällä seuraava koodi:  

    // Make the blob container publicly accessible.
    azure::storage::blob_container_permissions permissions;
    permissions.set_public_access(azure::storage::blob_container_public_access_type::blob);
    container.upload_permissions(permissions);  

Kuka tahansa Internetissä kohdassa julkinen säilön BLOB, mutta voit muokata tai poistaa ne vain, jos sinulla on riittävät käyttöoikeudet-näppäintä.  

## <a name="how-to-upload-a-blob-into-a-container"></a>Toimintaohje: lataa blob säilöön
Azure-Blob-säiliö tukee estä BLOB-objektit ja sivun BLOB-objektit. Useimmissa tapauksissa estä blob on käytettävä suositellut tyyppi.  

Voit ladata tiedoston lohko-blob-säilö viittauksen hankkimalla ja avulla saat lohko-blob-viite. Kun blob-viite, voit ladata kaikki tiedot stream siihen **upload_from_stream** -menetelmällä. Tämä toiminto luo blob, jos se ei ole aiemmin olemassa tai korvata sen, jos sitä ole. Seuraavassa esimerkissä esitetään, kuinka voit ladata blob säilöön ja oletetaan, että säilö on jo luotu.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-1".
    azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

    // Create or overwrite the "my-blob-1" blob with contents from a local file.
    concurrency::streams::istream input_stream = concurrency::streams::file_stream<uint8_t>::open_istream(U("DataFile.txt")).get();
    blockBlob.upload_from_stream(input_stream);
    input_stream.close().wait();

    // Create or overwrite the "my-blob-2" and "my-blob-3" blobs with contents from text.
    // Retrieve a reference to a blob named "my-blob-2".
    azure::storage::cloud_block_blob blob2 = container.get_block_blob_reference(U("my-blob-2"));
    blob2.upload_text(U("more text"));

    // Retrieve a reference to a blob named "my-blob-3".
    azure::storage::cloud_block_blob blob3 = container.get_block_blob_reference(U("my-directory/my-sub-directory/my-blob-3"));
    blob3.upload_text(U("other text"));  

Voit vaihtoehtoisesti tiedoston lataaminen estä Blob-objektien **upload_from_file** -menetelmää.

## <a name="how-to-list-the-blobs-in-a-container"></a>Toimintaohje: luettelon BLOB-säilö
Luettelon BLOB-säilö, hanki säilö viittaus. Voit hakea BLOB-objektit ja/tai hakemistoja se sitten säilö **list_blobs** menetelmän avulla. Jos haluat käyttää monenlaisia ominaisuuksia ja menetelmiä palautettu **list_blob_item**, voit soittaa saat **cloud_blob** objektin **list_blob_item.as_blob** -menetelmää tai Hae cloud_blob_directory objekti **list_blob.as_directory** noudattamalla. Seuraava koodi kerrotaan, miten noutaa ja tulosteen kunkin kohteen **Oma malli-säilö** säilössä URI:

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Output URI of each item.
    azure::storage::list_blob_item_iterator end_of_results;
    for (auto it = container.list_blobs(); it != end_of_results; ++it)
    {
        if (it->is_blob())
        {
            std::wcout << U("Blob: ") << it->as_blob().uri().primary_uri().to_string() << std::endl;
        }
        else
        {
            std::wcout << U("Directory: ") << it->as_directory().uri().primary_uri().to_string() << std::endl;
        }
    }

Lisätietoja toimintojen luettelo on artikkelissa [Azure tallennustilan resurssit c++](storage-c-plus-plus-enumeration.md).

## <a name="how-to-download-blobs"></a>Toimintaohje: lataa BLOB-objektit
Voit ladata BLOB-ensin hakea blob-viite ja kutsu **download_to_stream** -menetelmää. Seuraavassa esimerkissä **download_to_stream** menetelmä blob sisällön siirtämiseen stream objektia, jonka voit sitten Poistu paikalliseen tiedostoon.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-1".
    azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

    // Save blob contents to a file.
    concurrency::streams::container_buffer<std::vector<uint8_t>> buffer;
    concurrency::streams::ostream output_stream(buffer);
    blockBlob.download_to_stream(output_stream);

    std::ofstream outfile("DownloadBlobFile.txt", std::ofstream::binary);
    std::vector<unsigned char>& data = buffer.collection();

    outfile.write((char *)&data[0], buffer.size());
    outfile.close();  

Voit vaihtoehtoisesti lataaminen blob sisällön tiedostoon **download_to_file** -menetelmää.
Lisäksi voit käyttää myös **download_text** menetelmä Lataa Blob-objektien merkkijonomuotoisen sisällön.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-2".
    azure::storage::cloud_block_blob text_blob = container.get_block_blob_reference(U("my-blob-2"));

    // Download the contents of a blog as a text string.
    utility::string_t text = text_blob.download_text();

## <a name="how-to-delete-blobs"></a>Toimintaohje: Poista BLOB-objektit
Poista blob haluat ensin blob viitata ja kutsua sitten **delete_blob** -menetelmää.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-1".
    azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

    // Delete the blob.
    blockBlob.delete_blob();

## <a name="next-steps"></a>Seuraavat vaiheet
Nyt kun olet tutustunut Blob-objektien tallennustilaan perusteet, näistä linkeistä saat lisätietoja Azure-tallennustilan.  

-   [Opi käyttämään jonon tallennustilaa C++](storage-c-plus-plus-how-to-use-queues.md)
-   [Opi käyttämään C++-taulukkotallennus](storage-c-plus-plus-how-to-use-tables.md)
-   [Azure-tallennustilan resurssit ovat c++](storage-c-plus-plus-enumeration.md)
-   [Tallennustilan asiakkaan kirjasto C++-Ohje](http://azure.github.io/azure-storage-cpp)
-   [Azure-tallennustilan dokumentaatio](https://azure.microsoft.com/documentation/services/storage/)
- [Siirrä tiedot AzCopy komentorivin-apuohjelman avulla](storage-use-azcopy.md)
