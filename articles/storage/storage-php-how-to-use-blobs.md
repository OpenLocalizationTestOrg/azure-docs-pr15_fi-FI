<properties
    pageTitle="Opi käyttämään blob-säiliö (objektin tallennus) PHP | Microsoft Azure"
    description="Tallentaa erimuotoisia tietoja pilveen Azure-Blob-säiliö (objektin tallennus) kanssa."
    documentationCenter="php"
    services="storage"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="how-to-use-blob-storage-from-php"></a>Opi käyttämään PHP-Blob-objektien tallennustilaan

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Yleiskatsaus

Azure-Blob-säiliö on palvelu, joka tallentaa tiedot rakenteeton pilveen objektit-BLOB-objektit. Blob-objektien tallennustilaan voit tallentaa minkä tyyppisessä tekstiä tai binaarinen tietoja, kuten asiakirjan, mediatiedosto tai sovelluksen asennusohjelma. Blob-objektien tallennustilaan kutsutaan myös objektin säilön.

Tämän oppaan avulla voit suorittaa yleisiä tilanteita, joissa Azure-blob-palvelun avulla. Mallit on kirjoitettu PHP ja käyttää [Azure SDK PHP] [download]. Tilanteita, joissa kattaa Sisällytä **lataaminen**, **luettelon**, **lataaminen**ja **poistaminen** BLOB-objektit. Lisätietoja BLOB-kohdassa [seuraavat vaiheet](#next-steps) .

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>PHP-sovelluksen luominen

Vain vaatimus luodaan PHP-sovellusta, joka käyttää Azure-blob-palvelu on oman koodiin PHP luokkien Azure SDK viittaava. Mikä tahansa Kehitystyökalut voit luoda sovellus, esimerkiksi Muistiossa.

Tämän oppaan avulla voit käyttää palvelun ominaisuuksia, jotka voidaan kutsua PHP-sovelluksen paikallisesti tai koodin suorittamisen Azure web rooli, työntekijän rooli tai sivuston.

## <a name="get-the-azure-client-libraries"></a>Hae Azure asiakas-kirjastot

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-the-blob-service"></a>Sovelluksen käyttämään blob-palvelun määrittäminen

Azure-blob-service API käyttämään on:

1. Viittaus [require_once] -lauseella autoloader tiedoston ja
2. Viittaus käytöstä kaikki luokat.

Seuraavassa esimerkissä esitetään autoloader-tiedoston ja viittaa **ServicesBuilder** -luokkaan.

> [AZURE.NOTE] Tässä esimerkissä (ja muita Esimerkkejä tämän artikkelin) oletetaan, olet asentanut Azure kautta tunnus PHP asiakas-kirjastoissa. Jos olet asentanut kirjastot manuaalisesti, sinun on viittaus `WindowsAzure.php` autoloader tiedosto.

    require_once 'vendor/autoload.php';
    use WindowsAzure\Common\ServicesBuilder;


Esimerkeissä `require_once` lauseen näytetään aina, mutta vain luokat, jotka ovat esimerkiksi suorittamiseen tarvittavat viitataan.

## <a name="set-up-an-azure-storage-connection"></a>Azure-tallennustilan-yhteyden määrittäminen

Azure-blob-palvelun asiakasohjelmaa vahvistamiseen on kelvollinen yhteysmerkkijono. Blob-palvelun yhteysmerkkijonon muoto on:

Live palvelun käyttöön:

    DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]

Käyttää tallennustilan emulaattori:

    UseDevelopmentStorage=true


Voit luoda minkä tahansa Azure service-asiakas, haluat käyttää **ServicesBuilder** -luokka. Voit:

* Välittää yhteysmerkkijonon suoraan tai
* **CloudConfigurationManager (magnesiitin)** avulla voit tarkistaa useista ulkoisista lähteistä yhteysmerkkijonon varten:
    * Oletusarvon mukaan se sisältyy tuki ulkoisen tietolähteen - ympäristön muuttujat.
    * Voit lisätä uusia lähteitä pidentämällä **ConnectionStringSource** -luokka.

Tässä kuvatut esimerkkejä yhteysmerkkijonon siirtyvät suoraan.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;

    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);

## <a name="create-a-container"></a>Luoda säilön

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

**BlobRestProxy** objektin voit luoda blob-säilö **createContainer** -menetelmällä. Säilön luotaessa määritettävät asetukset säilö, mutta näin sitä ei tarvita. (Alla olevassa esimerkissä määrittämisestä säilön käyttöoikeusluettelo (Käyttöoikeusluettelon) ja säilö metatiedot.)

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Blob\Models\CreateContainerOptions;
    use MicrosoftAzure\Storage\Blob\Models\PublicAccessType;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    // OPTIONAL: Set public access policy and metadata.
    // Create container options object.
    $createContainerOptions = new CreateContainerOptions();

    // Set public access policy. Possible values are
    // PublicAccessType::CONTAINER_AND_BLOBS and PublicAccessType::BLOBS_ONLY.
    // CONTAINER_AND_BLOBS:
    // Specifies full public read access for container and blob data.
    // proxys can enumerate blobs within the container via anonymous
    // request, but cannot enumerate containers within the storage account.
    //
    // BLOBS_ONLY:
    // Specifies public read access for blobs. Blob data within this
    // container can be read via anonymous request, but container data is not
    // available. proxys cannot enumerate blobs within the container via
    // anonymous request.
    // If this value is not specified in the request, container data is
    // private to the account owner.
    $createContainerOptions->setPublicAccess(PublicAccessType::CONTAINER_AND_BLOBS);

    // Set container metadata.
    $createContainerOptions->addMetaData("key1", "value1");
    $createContainerOptions->addMetaData("key2", "value2");

    try {
        // Create container.
        $blobRestProxy->createContainer("mycontainer", $createContainerOptions);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Soittavan **setPublicAccess (PublicAccessType::CONTAINER\_ja\_BLOB)** on suunniteltu kautta anonyymit pyynnöt säilö ja blob-tietoja. Soittavan **setPublicAccess(PublicAccessType::BLOBS_ONLY)** on suunniteltu blob-tietojen anonyymit pyynnöt kautta. Saat lisätietoja säilön käyttöoikeusluettelot [määrittäminen säilön Käyttöoikeusluettelo (REST API)][container-acl].

Saat lisätietoja Blob-palvelun virhekoodit [Blob-palvelun virhekoodit][error-codes].

## <a name="upload-a-blob-into-a-container"></a>Lataa blob säilöön

Voit ladata tiedoston blob, käytä **BlobRestProxy -> createBlockBlob** -menetelmää. Tämä toiminto luo blob, jos se ei ole tai korvaa se, jos se ei. Alla olevassa esimerkissä koodin oletetaan, että säilö on jo luotu ja käyttää [fopen] [ fopen] stream tiedoston avaamiseen.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    $content = fopen("c:\myfile.txt", "r");
    $blob_name = "myblob";

    try {
        //Upload blob
        $blobRestProxy->createBlockBlob("mycontainer", $blob_name, $content);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Huomautus edellisen näytteen latauksia Blob-objektien stream nimellä. Kuitenkin blob myös voi ladata käyttäviksi merkkijono, esimerkiksi [tiedoston\_Hae\_sisältö] [ file_get_contents] funktio. Voit tehdä tämän käyttämällä edellisen näytteen muuttaminen `$content = fopen("c:\myfile.txt", "r");` , `$content = file_get_contents("c:\myfile.txt");`.

## <a name="list-the-blobs-in-a-container"></a>Luettelon BLOB-säilö

Luettelon BLOB-säilö, käyttää **foreach** silmukka-keskeytyksettä läpi tulos **BlobRestProxy -> listBlobs** -menetelmää. Seuraava koodi näyttää kunkin Blob-objektien nimi säilön tulostus ja näyttää sen URI selaimeen.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // List blobs.
        $blob_list = $blobRestProxy->listBlobs("mycontainer");
        $blobs = $blob_list->getBlobs();

        foreach($blobs as $blob)
        {
            echo $blob->getName().": ".$blob->getUrl()."<br />";
        }
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }


## <a name="download-a-blob"></a>Lataa blob

Voit ladata blob- **BlobRestProxy -> getBlob** menetelmää ja valitse kutsu tuloksena **GetBlobResult** objektin **getContentStream** -menetelmää.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Get blob.
        $blob = $blobRestProxy->getBlob("mycontainer", "myblob");
        fpassthru($blob->getContentStream());
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Huomaa, että edellä olevassa esimerkissä saa blob resurssiksi stream (oletusasetus). Voit kuitenkin käyttää [stream\_Hae\_sisältö] [ stream-get-contents] -funktion avulla palautettujen stream muuntaminen merkkijonon.

## <a name="delete-a-blob"></a>Poista blob

Voit poistaa blob- **BlobRestProxy -> deleteBlob**säilön nimi ja Blob-objektien nimi välittää.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Delete blob.
        $blobRestProxy->deleteBlob("mycontainer", "myblob");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="delete-a-blob-container"></a>Poista blob-säilö

Lopuksi poistettava blob-säilö välittää- **BlobRestProxy > deleteContainer**säilön nimi.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Delete container.
        $blobRestProxy->deleteContainer("mycontainer");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet oppinut perustiedot Azure-blob-palvelun, näistä linkeistä saat lisätietoja tallennustilan monimutkaisia tehtäviä.

- [Azuren tallennustilaan tiimin blogia](http://blogs.msdn.com/b/windowsazurestorage/)
- Katso [PHP estä blob Esimerkki](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/BlockBlobExample.php).
- Katso [PHP sivun blob Esimerkki](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/PageBlobExample.php).
- [Siirrä tiedot ja AzCopy komentorivivalitsimet-apuohjelma](storage-use-azcopy.md)
 
Lisätietoja on Katso myös [PHP Developer Center](/develop/php/).


[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[container-acl]: http://msdn.microsoft.com/library/azure/dd179391.aspx
[error-codes]: http://msdn.microsoft.com/library/azure/dd179439.aspx
[file_get_contents]: http://php.net/file_get_contents
[require_once]: http://php.net/require_once
[fopen]: http://www.php.net/fopen
[stream-get-contents]: http://www.php.net/stream_get_contents
