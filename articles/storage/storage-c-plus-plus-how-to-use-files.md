<properties
    pageTitle="Tiedostojen tallennus-C++ käyttämisestä | Microsoft Azure"
    description="Tallentaa tiedoston tietojen kanssa Azure tiedostojen tallentamisesta pilveen."
    services="storage"
    documentationCenter=".net"
    authors="seguler"
    manager="jahogg"
    editor="tysonn" />

<tags ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="seguler" />

# <a name="how-to-use-file-storage-from-c"></a>Tiedostojen tallennus-C++ käyttäminen

[AZURE.INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]
[AZURE.INCLUDE [storage-file-overview-include](../../includes/storage-file-overview-include.md)]

## <a name="about-this-tutorial"></a>Tässä opetusohjelmassa tietoja

Tässä opetusohjelmassa opit suorittamaan peruslaskutoimituksia Microsoft Azure tiedoston tallennustilan-palvelusta. Kirjoitettu C++ mallien avulla opit luominen osakkeet ja hakemistojen, lataaminen, luettelon ja poista tiedostot. Jos ole aiemmin käyttänyt Microsoft Azure tiedoston tallennuspalvelu, tapoja, joilla käsitteitä loppuun on erittäin hyödyllistä tietoa mallit.

[AZURE.INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>C++-sovelluksen luominen

Voit luoda malleja, sinun on asentaminen Azure tallennustilan asiakkaan kirjaston 2.4.0 C++. Voit myös kannattaa luoda Azure-tallennustilan tilin.

Voit asentaa Azure-tallennustilan asiakkaan 2.4.0 C++-jollakin seuraavista tavoista:

-   **Linux:** Noudata annettuja [Azure tallennustilan asiakkaan kirjasto c++: n Lueminut](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) -sivulla.

-   **Windows:** Valitse Visual Studion **Työkalut &gt; NuGet Package Manager &gt; Package hallinta-konsolin**. Kirjoita seuraava komento [NuGet paketin hallinta-konsolin](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) ja paina **ENTER**-näppäintä.

    Asenna-Package wastorage

## <a name="set-up-your-application-to-use-file-storage"></a>Sovelluksen käyttäminen tiedostojen tallentamisesta määrittäminen

Lisää seuraavat ovat lauseet C++-tiedosto, johon Azure tallennustilan ohjelmointirajapinnan avulla voit käyttää tiedostoja alkuun:

    #include "was/storage_account.h"
    #include "was/file.h"

## <a name="set-up-an-azure-storage-connection-string"></a>Azure-tallennustilan-yhteysmerkkijonon määrittäminen

Tiedostojen tallentamisesta käyttämään haluat muodostaa yhteyden tiliisi Azure-tallennustilan. Voit määrittää yhteyden merkkijono, joka muodostaa yhteyden tiliisi tallennustilan Käytämme olisi ensimmäiseksi. Määritä staattinen muuttuja, voit tehdä seuraavaksi.

    // Define the connection-string with your values.
    const utility::string_t 
    storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

## <a name="connecting-to-an-azure-storage-account"></a>Yhteyden muodostaminen Azure-tallennustilan tilin

Voit käyttää **cloud_storage_account** luokan edustavan tallennustilan tilin tiedot. Tallennustilan yhteysmerkkijonon noutaminen tallennustilan tilin tiedot, voit **jäsentää** -menetelmää.

    // Retrieve storage account from connection string. 
    azure::storage::cloud_storage_account storage_account = 
      azure::storage::cloud_storage_account::parse(storage_connection_string);

## <a name="how-to-create-a-share"></a>Toimintaohje: Luo jaettu kansio

Kaikki tiedostot ja kansiot-tiedostojen tallentamisesta sijaitsevat säilön kutsutaan **Jaa**. Tallennustilan tilin voi olla niin monta osakkeet tilin kapasiteetti-toiminnolla. Voit hankkia jaettuun ja sen sisällön käyttöoikeus, tarvitset tiedostojen tallennustila-asiakasohjelman.

    // Create the file storage client.
    azure::storage::cloud_file_client file_client = 
      storage_account.create_cloud_file_client();

Tiedoston tallennustilan asiakkaan avulla voit hankkia sitten jaettuun viittaus.

    // Get a reference to the file share
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));

Voit luoda Jaa käyttämällä **cloud_file_share** objektin **create_if_not_exists** -menetelmää.

    if (share.create_if_not_exists()) { 
        std::wcout << U("New share created") << std::endl;  
    }

Tässä vaiheessa **jakaminen** pitää viittauksen **Oma otoksen – Jaa**-resurssiin.

## <a name="how-to-upload-a-file"></a>Toimintaohje: tiedoston lataaminen

Vähintään Azure tiedostoresurssin tallennustila on pääkansio, johon tiedostot voivat sijaita. Tässä osassa opit paikallisesta tallennussijainnista sivulle pääkansiossa jaetun tiedoston lataaminen.

Ensimmäinen vaihe tiedoston lataamisessa on hankkiminen hakemiston viittaa, joissa se olisi sijaitsevat. Voit tehdä tämän jaetun objektin **get_root_directory_reference** -menetelmällä.

    //Get a reference to the root directory for the share.
    azure::storage::cloud_file_directory root_dir = share.get_root_directory_reference();

Nyt kun olet luonut jaetun kansion pääkansion viittaa, voit ladata tiedoston sen päälle. Tässä esimerkissä Lataa tiedosto, tekstin ja stream.

    // Upload a file from a stream.
    concurrency::streams::istream input_stream = 
      concurrency::streams::file_stream<uint8_t>::open_istream(_XPLATSTR("DataFile.txt")).get();

    azure::storage::cloud_file file1 = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-1"));
    file1.upload_from_stream(input_stream);

    // Upload some files from text.
    azure::storage::cloud_file file2 = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-2"));
    file2.upload_text(_XPLATSTR("more text"));

    // Upload a file from a file.
    azure::storage::cloud_file file4 = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
    file4.upload_from_file(_XPLATSTR("DataFile.txt"));  

## <a name="how-to-create-a-directory"></a>Toimintaohje: Hakemiston luominen

Voit myös järjestää tallennustilan sijoittamalla tiedostot sen sijaan, että ne kaikki pääkansiossa alihakemistoista sisällä. Voit luoda mahdollisimman paljon kansioita, kuten tilisi sallii Azure tiedoston tallennuspalvelu. Seuraava koodi luo **Oma malli-kansion** pääkansion sekä alikansioon nimeltään **Oma malli-alikansion**hakemiston.
    
    // Retrieve a reference to a directory
    azure::storage::cloud_file_directory directory = share.get_directory_reference(_XPLATSTR("my-sample-directory"));
    
    // Return value is true if the share did not exist and was successfully created.
    directory.create_if_not_exists();
    
    // Create a subdirectory.
    azure::storage::cloud_file_directory subdirectory = 
      directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));
    subdirectory.create_if_not_exists();

## <a name="how-to-list-files-and-directories-in-a-share"></a>Toimintaohje: tiedostojen ja kansioiden jaettuun-luettelo

Tiedostojen ja kansioiden sisällä olevaa jaettua luettelo hankkiminen tehdään helposti kutsumalla **list_files_and_directories** **cloud_file_directory** viittaus. Jos haluat käyttää monenlaisia ominaisuuksia ja menetelmiä palautettu **list_file_and_directory_item**, voit soittaa saat **cloud_file** objektin **list_file_and_directory_item.as_file** -menetelmää tai Hae **cloud_file_directory** objekti **list_file_and_directory_item.as_directory** noudattamalla.

Seuraava koodi esitellään, miten haluat hakea ja siirtää jaetun kansion pääkansion kunkin kohteen URI.

    //Get a reference to the root directory for the share.
    azure::storage::cloud_file_directory root_dir = 
      share.get_root_directory_reference();
    
    // Output URI of each item.
    azure::storage::list_file_and_diretory_result_iterator end_of_results;
    
    for (auto it = directory.list_files_and_directories(); it != end_of_results; ++it)
    {
        if(it->is_directory())
        {
            ucout << "Directory: " << it->as_directory().uri().primary_uri().to_string() << std::endl;
        }
        else if (it->is_file())
        {
            ucout << "File: " << it->as_file().uri().primary_uri().to_string() << std::endl;
        }       
    }
    

## <a name="how-to-download-a-file"></a>Toimintaohje: Lataa tiedosto

Tiedostojen lataaminen ensin hakea tiedostoviittauksen ja kutsua sitten tiedoston sisällön siirtämiseen stream-objekti, joka voimassa paikalliseen tiedostoon **download_to_stream** -menetelmää. Vaihtoehtoisesti voit ladata tiedoston sisällön paikallisen tiedoston **download_to_file** -menetelmää. Voit ladata merkkijonomuotoisen tiedoston sisällön **download_text** menetelmän avulla.

Seuraavassa esimerkissä **download_to_stream** ja **download_text** menetelmiä osoittamaan ladata tiedostoja, jotka on luotu edellisten kohtien.

    // Download as text
    azure::storage::cloud_file text_file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-2"));
    utility::string_t text = text_file.download_text();
    ucout << "File Text: " << text << std::endl;
    
    // Download as a stream.
    azure::storage::cloud_file stream_file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
    
    concurrency::streams::container_buffer<std::vector<uint8_t>> buffer;
    concurrency::streams::ostream output_stream(buffer);
    stream_file.download_to_stream(output_stream);
    std::ofstream outfile("DownloadFile.txt", std::ofstream::binary);
    std::vector<unsigned char>& data = buffer.collection();
    outfile.write((char *)&data[0], buffer.size());
    outfile.close();

## <a name="how-to-delete-a-file"></a>Toimintaohje: tiedoston poistaminen

Toinen yleisiä tiedostojen tallennustila-toiminto on tiedoston poistaminen. Seuraava koodi poistaa tiedoston nimeltä oma-malli-tiedosto-3 tallennettu pääkansioon.

    // Get a reference to the root directory for the share. 
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    azure::storage::cloud_file_directory root_dir = 
      share.get_root_directory_reference();
    
    azure::storage::cloud_file file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
    
    file.delete_file_if_exists();

## <a name="how-to-delete-a-directory"></a>Toimintaohje: kansion poistaminen

Hakemiston poistetaan yksinkertaisen tehtävän, vaikka huomattava ei voi poistaa kansio, joka sisältää silti tiedostot tai muihin kansioihin.
    
    // Get a reference to the share.
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    // Get a reference to the directory.
    azure::storage::cloud_file_directory directory = 
      share.get_directory_reference(_XPLATSTR("my-sample-directory"));
    
    // Get a reference to the subdirectory you want to delete.
    azure::storage::cloud_file_directory sub_directory =
      directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));
    
    // Delete the subdirectory and the sample directory.
    sub_directory.delete_directory_if_exists();
    
    directory.delete_directory_if_exists();

## <a name="how-to-delete-a-share"></a>Toimintaohje: jaettuun poistaminen

Jaettuun poistaminen tehdään **delete_if_exists** kutsumista cloud_file_share aluetta. Tässä on esimerkki koodi, joka toimii.
    
    // Get a reference to the share.
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    // delete the share if exists
    share.delete_share_if_exists();

## <a name="set-the-maximum-size-for-a-file-share"></a>Jaetun tiedoston enimmäiskoko

Voit määrittää jaetun tiedostoresurssin gigatavua kiintiön (tai enimmäiskoko). Voit myös tarkistaa, kuinka paljon tietoja on tallennettu Jaa.

Määrittämällä jaettuun kiintiön voit rajoittaa kokonaiskoko jaa tallennettuja tiedostoja. Jos Jaa tiedostoja kokonaiskoko ylittää kiintiön määrittäminen Jaa, sitten asiakkaat voi aiemmin luotujen tiedostojen kokoa tai luoda uusia tiedostoja, ellei kyseiset tiedostot ovat tyhjiä.

Seuraavassa esimerkissä näytetään, miten voit tarkistaa nykyisen jaetun käytön ja jaa kiintiön määrittäminen.

    // Parse the connection string for the storage account.
    azure::storage::cloud_storage_account storage_account = 
      azure::storage::cloud_storage_account::parse(storage_connection_string);
    
    // Create the file client.
    azure::storage::cloud_file_client file_client = 
      storage_account.create_cloud_file_client();
    
    // Get a reference to the share.
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    if (share.exists())
    {
        std::cout << "Current share usage: " << share.download_share_usage() << "/" << share.properties().quota();
    
        //This line sets the quota to 2560GB
        share.resize(2560);
    
        std::cout << "Quota increased: " << share.download_share_usage() << "/" << share.properties().quota();
    
    }

## <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a>Luo jaettu käyttö allekirjoituksen tiedosto tai jaettuun tiedostoon

Voit luoda jaettu käyttö allekirjoituksen (SAS) jaetun tiedostoresurssin tai yksittäisen tiedoston. Voit myös luoda jaetun käyttöoikeuskäytäntö tiedoston verkkosijainnissa, voit hallita jaettua käyttöä allekirjoitukset. Jaetun access-käytännön luominen suositellaan, kun se sisältää keino Suojaussidokset peruutetaan, jos se on käsiin.

Seuraavassa esimerkissä Luo jaettu käyttöoikeuskäytäntö jaettuun ja antaa rajoitukset SAS Jaa tiedostoa käytännön avulla.

    // Parse the connection string for the storage account.
    azure::storage::cloud_storage_account storage_account = 
      azure::storage::cloud_storage_account::parse(storage_connection_string);
    
    // Create the file client and get a reference to the share
    azure::storage::cloud_file_client file_client = 
      storage_account.create_cloud_file_client();
    
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    if (share.exists())
    {
        // Create and assign a policy
        utility::string_t policy_name = _XPLATSTR("sampleSharePolicy");

        azure::storage::file_shared_access_policy sharedPolicy = 
          azure::storage::file_shared_access_policy();

        //set permissions to expire in 90 minutes
        sharedPolicy.set_expiry(utility::datetime::utc_now() + 
          utility::datetime::from_minutes(90));

        //give read and write permissions
        sharedPolicy.set_permissions(azure::storage::file_shared_access_policy::permissions::write | azure::storage::file_shared_access_policy::permissions::read);

        //set permissions for the share
        azure::storage::file_share_permissions permissions; 

        //retrieve the current list of shared access policies
        azure::storage::shared_access_policies<azure::storage::file_shared_access_policy> policies;
        
        //add the new shared policy
        policies.insert(std::make_pair(policy_name, sharedPolicy));

        //save the updated policy list
        permissions.set_policies(policies);
        share.upload_permissions(permissions);

        //Retrieve the root directory and file references
        azure::storage::cloud_file_directory root_dir = 
          share.get_root_directory_reference();
        azure::storage::cloud_file file = 
          root_dir.get_file_reference(_XPLATSTR("my-sample-file-1"));

        // Generate a SAS for a file in the share 
        //  and associate this access policy with it.       
        utility::string_t sas_token = file.get_shared_access_signature(sharedPolicy);
        
        // Create a new CloudFile object from the SAS, and write some text to the file.     
        azure::storage::cloud_file file_with_sas(azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()));
        utility::string_t text = _XPLATSTR("My sample content");        
        file_with_sas.upload_text(text);        
        
        //Download and print URL with SAS.
        utility::string_t downloaded_text = file_with_sas.download_text();      
        ucout << downloaded_text << std::endl;      
        ucout << azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()).to_string() << std::endl;
    
    }

Lisätietoja jaettujen käytön allekirjoitusten luomisesta ja käyttämisestä on [Käyttämällä jaetun Access allekirjoitukset (SAS)](storage-dotnet-shared-access-signature-part-1.md).

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja Azure-tallennustilan, tutki nämä resurssit:

-   [Tallennustilan asiakkaan kirjasto C++](https://github.com/Azure/azure-storage-cpp)

-   [Azure-tallennustilan Explorer](http://go.microsoft.com/fwlink/?LinkID=822673&clcid=0x409)

-   [Azure-tallennustilan dokumentaatio](https://azure.microsoft.com/documentation/services/storage/)
