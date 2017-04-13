<properties
    pageTitle="Azure-Blob-säiliö (objektin tallennus) käyttämällä .NET aloittaminen | Microsoft Azure"
    description="Tallentaa erimuotoisia tietoja pilveen Azure-Blob-säiliö (objektin tallennus) kanssa."
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="get-started-with-azure-blob-storage-using-net"></a>Pääset alkuun käyttämällä .NET Azure-Blob-säiliö

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Yleiskatsaus

Azure-Blob-säiliö on palvelu, joka tallentaa tiedot rakenteeton pilveen objektit-BLOB-objektit. Blob-objektien tallennustilaan voit tallentaa minkä tyyppisessä tekstiä tai binaarinen tietoja, kuten asiakirjan, mediatiedosto tai sovelluksen asennusohjelma. Blob-objektien tallennustilaan kutsutaan myös objektin säilön.

### <a name="about-this-tutorial"></a>Tässä opetusohjelmassa tietoja

Tässä opetusohjelmassa näytetään, miten voit kirjoittaa havainnollistetaan Yleisiä tilanteita, Azure-Blob-säiliö käyttämällä .NET-koodia. Skenaariot kattaa Sisällytä lataaminen, luettelon, lataaminen ja poistaminen BLOB-objektit.

**Arvioitu kesto:** 45 minuuttia

**Prerequisities:**

- [Microsoft Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)
- [.NET Azure tallennustilan asiakkaan kirjasto](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [.NET Azure hallintatoiminto](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
- [Azure-tallennustilan tilin](storage-create-storage-account.md#create-a-storage-account)


[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a>Lisää esimerkkejä

Katso Lisää esimerkkejä Blob-objektien tallennustilaan, [Azure-Blob-säiliö .NET käytön aloittaminen](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/). Voit ladata sovelluksen malli ja suorittaa sen tai Selaa GitHub koodia.


[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-namespace-declarations"></a>Lisää nimitilan ilmoitukset

Lisää seuraava `using` lauseet yläreunaan `program.cs` tiedosto:

    using Microsoft.Azure; // Namespace for CloudConfigurationManager
    using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
    using Microsoft.WindowsAzure.Storage.Blob; // Namespace for Blob storage types

### <a name="parse-the-connection-string"></a>Yhteysmerkkijonon jäsentää

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-blob-service-client"></a>Luo Blob-palvelun asiakasohjelmaa

**CloudBlobClient** -luokan avulla voit hakea säilöjen ja Blob-objektien tallennustilaan tallennettuja BLOB. Näin voit luoda palvelun-asiakasohjelmaa:

    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

Nyt olet valmis, joka lukee tietoja ja kirjoittaa tietoja Blob-objektien tallennustilaan koodin kirjoittaminen.

## <a name="create-a-container"></a>Luoda säilön

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Tässä esimerkissä näytetään, miten voit luoda säilön, jos sitä ei ole jo:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Create the container if it doesn't already exist.
    container.CreateIfNotExists();

Oletusarvon mukaan uusi säilö on yksityinen, mikä tarkoittaa, että sinun on määritettävä tallennustilan access-näppäintä, voit ladata tämän säilön BLOB-objektit. Jos haluat tehdä säilöön tiedostot kaikille, voit määrittää säilö on julkinen käyttämällä seuraava koodi:

    container.SetPermissions(
        new BlobContainerPermissions { PublicAccess = BlobContainerPublicAccessType.Blob });

Kuka tahansa Internetissä näkevät yleisen säilön BLOB, mutta voit muokata tai poistaa ne vain, jos sinulla on haluamasi tili pikanäppäin tai jaettuun käyttöön allekirjoituksen.

## <a name="upload-a-blob-into-a-container"></a>Lataa blob säilöön

Azure-Blob-säiliö tukee estä BLOB-objektit ja sivun BLOB-objektit.  Useimmissa tapauksissa estä blob on käytettävä suositellut tyyppi.

Voit ladata tiedoston lohko-blob-säilö viittauksen hankkimalla ja avulla saat lohko-blob-viite. Kun blob-viite, voit ladata kaikki tiedot stream siihen **UploadFromStream** -menetelmällä. Tämä toiminto luo blob, jos se ei ole aiemmin olemassa tai korvata sen, jos sitä ole.

Seuraavassa esimerkissä esitetään, kuinka voit ladata blob säilöön ja oletetaan, että säilö on jo luotu.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## <a name="list-the-blobs-in-a-container"></a>Luettelon BLOB-säilö

Luettelon BLOB-säilö, hanki säilö viittaus. Voit hakea BLOB-objektit ja/tai hakemistoja se sitten säilö **ListBlobs** menetelmän avulla. Yhteyden ominaisuuksien ja menetelmien monipuoliset palautettu **IListBlobItem**pakottaa sen **CloudBlockBlob**, **CloudPageBlob**tai **CloudBlobDirectory** objektiin.  Jos tyyppi on tuntematon, voit määrittää, johon haluat liittää sen tyyppi-valintaruutu.  Seuraava koodi näytetään, miten haluat hakea ja tulosteen kunkin kohteen URI `photos` säilö:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("photos");

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, false))
    {
        if (item.GetType() == typeof(CloudBlockBlob))
        {
            CloudBlockBlob blob = (CloudBlockBlob)item;

            Console.WriteLine("Block blob of length {0}: {1}", blob.Properties.Length, blob.Uri);

        }
        else if (item.GetType() == typeof(CloudPageBlob))
        {
            CloudPageBlob pageBlob = (CloudPageBlob)item;

            Console.WriteLine("Page blob of length {0}: {1}", pageBlob.Properties.Length, pageBlob.Uri);

        }
        else if (item.GetType() == typeof(CloudBlobDirectory))
        {
            CloudBlobDirectory directory = (CloudBlobDirectory)item;

            Console.WriteLine("Directory: {0}", directory.Uri);
        }
    }

Yllä esitetyllä tavalla voit nimetä BLOB heidän nimensä polku tiedoilla. Tämä luo näennäiskansio rakenteen, jota voit järjestää ja käy läpi tapaan perinteinen tiedostojärjestelmässä. Huomaa, että kansiorakenne on vain virtual - Blob-objektien tallennustilaan vain resurssit ovat säilöjä ja BLOB-objektit. Tallennustilan asiakas-kirjasto on kuitenkin **CloudBlobDirectory** -objektin viittaamaan näennäiskansio ja yksinkertaistaa käyttäminen BLOB-objektit, jotka on järjestetty tällä tavalla.

Esimerkkinä estä BLOB-niminen säilön seuraavat joukko `photos`:

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

Kun soitat **ListBlobs** -'valokuvia' säilöä (kuten esimerkissä), palautetaan hierarkkisen luettelon. Se sisältää **CloudBlobDirectory** ja **CloudBlockBlob** objekteja, jotka edustavat kansioiden ja BLOB-säilö. Tulos näyttää tältä:

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


Voit halutessasi määrittää **UseFlatBlobListing** parametrin **ListBlobs** menetelmän **Tosi**. Tässä tapauksessa **CloudBlockBlob** objektin palauttaa jokaisen blob-säilö. Palauttaa kiinteänä luettelo **ListBlobs** puhelu näyttää seuraavanlaiselta:

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

ja tulos näyttää seuraavalta:

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


## <a name="download-blobs"></a>Lataa BLOB-objektit

Voit ladata BLOB-ensin hakea blob-viite ja kutsu **DownloadToStream** -menetelmää. Seuraavassa esimerkissä **DownloadToStream** menetelmä blob sisällön siirtämiseen stream objektia, jonka voit sitten Poistu paikalliseen tiedostoon.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents to a file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

Voit myös ladata Blob-objektien merkkijonomuotoisen sisällön **DownloadToStream** -menetelmää.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## <a name="delete-blobs"></a>Poista BLOB-objektit

Voit poistaa blob-haluat ensin blob viitata ja kutsua sitten **Delete** -menetelmällä.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    blockBlob.Delete();


## <a name="list-blobs-in-pages-asynchronously"></a>Luettelon BLOB-sivujen asynkronisesti

Jos BLOB suuri määrä on luettelo tai haluat palauttaa luettelon-toiminnossa tulosten määrää, voit lisätä sivujen tulosten BLOB. Tässä esimerkissä esitetään, kuinka palauttamaan tulokset sivujen asynkronisesti, niin, että suorittamisen ei ole estetty odotettaessa palauttaa tulokset suuri joukko.

Tässä esimerkissä näytetään tasainen Blob-objektien päälle, mutta voit suorittaa hierarkkisen luettelon määrittämällä `useFlatBlobListing` **ListBlobsSegmentedAsync** menetelmän parametrin `false`.

Esimerkki menetelmä soittaa asynkroninen menetelmä, koska se on etuliitteenä on `async` avainsanan, ja se on palautettava objekti on **tehtävä** . Määritetyn **ListBlobsSegmentedAsync** await avainsanan keskeyttää otoksen menetelmän suorittamista, kunnes luettelon tehtävä on valmis.

    async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
    {
        //List blobs to the console window, with paging.
        Console.WriteLine("List blobs in pages:");

        int i = 0;
        BlobContinuationToken continuationToken = null;
        BlobResultSegment resultSegment = null;

        //Call ListBlobsSegmentedAsync and enumerate the result segment returned, while the continuation token is non-null.
        //When the continuation token is null, the last page has been returned and execution can exit the loop.
        do
        {
            //This overload allows control of the page size. You can return all remaining results by passing null for the maxResults parameter,
            //or by calling a different overload.
            resultSegment = await container.ListBlobsSegmentedAsync("", true, BlobListingDetails.All, 10, continuationToken, null, null);
            if (resultSegment.Results.Count<IListBlobItem>() > 0) { Console.WriteLine("Page {0}:", ++i); }
            foreach (var blobItem in resultSegment.Results)
            {
                Console.WriteLine("\t{0}", blobItem.StorageUri.PrimaryUri);
            }
            Console.WriteLine();

            //Get the continuation token.
            continuationToken = resultSegment.ContinuationToken;
        }
        while (continuationToken != null);
    }

## <a name="writing-to-an-append-blob"></a>Liitä Blob-objektien kirjoittaminen

Liitä-blob on uusi blob-otettu käyttöön versiosta 5.x .NET Azure tallennustilan asiakas-kirjaston. Liitä-blob on tarkoitettu Liitä toimintoja, kuten kirjaaminen. Estä-blob, kuten Liitä Blob-objektien koostuu lohkot, mutta jos lisäät uuden kohteen liittäminen Blob-objektien, se näkyy aina blob loppuun. Et voi päivittää tai poistaa aiemmin lohko, Liitä-blob. Estä tunnukset, Liitä Blob-objektien eikä niitä julkaista ne ovat estä Blob-objektien.

Liitä Blob-objektien kunkin lohkon voi olla erikokoinen, enintään 4 Mt, ja liitä Blob-objektien voivat sisältää enintään 50 000 lohkot. Liitä Blob-objektien suurimman sallitun koon vuoksi on hieman enemmän kuin 195 Gigatavua (4 Mt X 50 000 estää).

Alla olevassa esimerkissä Luo uusi Liitä Blob-objektien ja liittää tietoja, simuloimalla yksinkertainen kirjaaminen-toimintoa.

    //Parse the connection string for the storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

    //Create service client for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("my-append-blobs");

    //Create the container if it does not already exist.
    container.CreateIfNotExists();

    //Get a reference to an append blob.
    CloudAppendBlob appendBlob = container.GetAppendBlobReference("append-blob.log");

    //Create the append blob. Note that if the blob already exists, the CreateOrReplace() method will overwrite it.
    //You can check whether the blob exists to avoid overwriting it by using CloudAppendBlob.Exists().
    appendBlob.CreateOrReplace();

    int numBlocks = 10;

    //Generate an array of random bytes.
    Random rnd = new Random();
    byte[] bytes = new byte[numBlocks];
    rnd.NextBytes(bytes);

    //Simulate a logging operation by writing text data and byte data to the end of the append blob.
    for (int i = 0; i < numBlocks; i++)
    {
        appendBlob.AppendText(String.Format("Timestamp: {0:u} \tLog Entry: {1}{2}",
            DateTime.UtcNow, bytes[i], Environment.NewLine));
    }

    //Read the append blob to the console window.
    Console.WriteLine(appendBlob.DownloadText());

Katso [tietoja estä BLOB-objektit, sivun BLOB-objektit, ja liitä BLOB](https://msdn.microsoft.com/library/azure/ee691964.aspx) lisätietoja BLOB kolmenlaisia erot.

## <a name="managing-security-for-blobs"></a>BLOB-suojauksen hallinta

Azuren tallennustilaan säilyttää oletusarvoisesti tietojen suojatun rajoittamalla käyttöoikeuksia tilin omistajan, joilla on tili pikanäppäimet. Kun haluat jakaa Blob-objektien tallennustila-tilisi, on tärkeää tehdä ilman suojausriskiä tilin pikanäppäimet. Lisäksi voit salata blob-tietoja, varmista, että se on suojattu ‑toiminto ja Azuren tallennustilaan.

[AZURE.INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

### <a name="controlling-access-to-blob-data"></a>Blob-tietojen käytön valvominen

Oletusarvon mukaan tallennustilan tilisi blob-tiedot ovat käytettävissä vain tallennustilan tilin omistajan. Tilin pikanäppäin todennustapa Blob-objektien tallennustilaan pyynnöt edellyttää oletusarvoisesti. Haluat ehkä kuitenkin käytettäväksi Blob-objektien tiettyjä tietoja muille käyttäjille. Sinulla on kaksi vaihtoehtoa:

- **Anonyymin käytön:** Säilön tai sen BLOB voi julkaista yleiseen käyttöön anonyymin käytön. Lisätietoja on kohdassa [Manage anonyymi lukuoikeudet säilöjä ja BLOB-objektit](storage-manage-access-to-resources.md) .
- **Jaettu access allekirjoitukset:** Voit antaa asiakkaiden jaettua käyttöä allekirjoituksella (SAS), josta pääsee valtuutetun tallennustilan tilisi resurssin käyttöoikeuksilla, jotka voit määrittää ja aikavälin, jotka määrität päälle. Lisätietoja on kohdassa [Käyttämällä jaettu Access allekirjoitukset (SAS)](storage-dotnet-shared-access-signature-part-1.md) .

### <a name="encrypting-blob-data"></a>Salauksen blob-tietoja

Azure-tallennustilan tukee salausta blob-tietojen sekä asiakkaan ja palvelimen:

- **Asiakkaan salaus:** .NET tallennustilan asiakkaan kirjasto tukee salaamaan tiedot ‑asiakassovelluksissa ennen Azuren tallennustilaan lataaminen ja salauksen tietojen ladattaessa asiakkaalle. Kirjaston tukee myös Azure avaimen säilö integrointi tallennustilan tilin hallintaa varten. Lisätietoja on kohdassa [Asiakkaan salaus, Microsoft Azuren tallennustilaan .NET](storage-client-side-encryption.md) . Katso myös [Opetusohjelma: salaaminen ja salauksen BLOB Microsoft Azuren tallennustilaan käyttämällä Azure avaimen säilö](storage-encrypt-decrypt-blobs-key-vault.md).
- **Palvelinpuolen salaus**: Azuren tallennustilaan tukee nyt palvelinpuolen salausta. Katso [Azure tallennustilan palvelun salaus tietojen, loput (ennakkoversio)](storage-service-encryption.md).

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet tutustunut Blob-objektien tallennustilaan perusteet, noudata näitä linkkejä lisätietoja.

### <a name="microsoft-azure-storage-explorer"></a>Microsoft Azure-tallennustilan Explorer
- [Microsoft Azure tallennustilan Explorer (MASE)](../vs-azure-tools-storage-manage-with-storage-explorer.md) on ilmainen, erillinen sovellus, jonka avulla voit visuaalisesti Azuren tallennustilaan tietojen käsitteleminen Windows, OS X ja Linux Microsoftilta.

### <a name="blob-storage-samples"></a>BLOB storage objektit

- [Azure-Blob-säiliö .NET-käytön aloittaminen](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/)

### <a name="blob-storage-reference"></a>BLOB storage viittaus

- [Tallennustilan asiakkaan kirjaston .NET-käyttöä varten](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
- [REST API-viittaus](http://msdn.microsoft.com/library/azure/dd179355)

### <a name="conceptual-guides"></a>Käsitteellinen apuviivat

- [Siirrä tiedot AzCopy komentorivin-apuohjelman avulla](storage-use-azcopy.md)
- [Aloita tallennus .NET varten](storage-dotnet-how-to-use-files.md)
- [Voit käyttää WebJobs SDK Azure-blob-säiliö](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)

  [Blob5]: ./media/storage-dotnet-how-to-use-blobs/blob5.png
  [Blob6]: ./media/storage-dotnet-how-to-use-blobs/blob6.png
  [Blob7]: ./media/storage-dotnet-how-to-use-blobs/blob7.png
  [Blob8]: ./media/storage-dotnet-how-to-use-blobs/blob8.png
  [Blob9]: ./media/storage-dotnet-how-to-use-blobs/blob9.png

  [Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [Configuring Connection Strings]: http://msdn.microsoft.com/library/azure/ee758697.aspx
  [.NET client library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [REST API reference]: http://msdn.microsoft.com/library/azure/dd179355
