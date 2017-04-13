<properties
    pageTitle="Aloita blob storage ja Visual Studio yhteydessä services (ASP.NET-5) | Microsoft Azure"
    description="Pikaviestien käyttäminen Azure-Blob-säiliö Visual Studio ASP.NET 5 projektin luotuasi tallennustilan tilillä Visual Studio yhdistetyt palvelut"
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

# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet-5"></a>Aloita Azure Blob storage ja Visual Studio yhteydessä services (ASP.NET-5)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

##<a name="overview"></a>Yleiskatsaus

Tässä artikkelissa käsitellään käyttäminen Azure-Blob-säiliö Visual Studiossa, kun olet luonut tai Viitattu Azure-tallennustilan tilin ASP.NET-5-Projectin Visual Studio Lisää yhdistetyt palvelut-valintaikkunan.

Azure-Blob-säiliö on paljon erimuotoisia tietoja, joita voi käyttää mitä tahansa HTTP tai HTTPS maailmanlaajuisesti tallentamista varten. Yksittäisen Blob-objektien voi olla minkä kokoinen. BLOB voi olla esimerkiksi kuvia, ääni- ja videotiedostoista, tietoja ja asiakirja. Tässä artikkelissa käsitellään aloittaa Blob-objektien tallennustilaan jälkeen Azure-tallennustilan tilin luominen käyttämällä ASP.NET-5-projektin Visual Studio **Lisää yhdistetyt palvelut** -valintaikkunassa.

Samalla tavalla kuin tiedostot tallennetaan kansiot-BLOB storage live säilöjen. Luotuasi tallennustilan voit luoda yhden tai useamman muistiin. Esimerkiksi tallennustilaan, jota kutsutaan "Leikekirjan", voit luoda säilöjen nimeltä "kuvat" kuvia tallennetaan muistiin ja toinen nimeltään "ääni" ääni tallentamista varten. Kun olet luonut säilöt, voit ladata ne Blob-objektien yksittäisiä tiedostoja. Saat lisätietoja käsittelevistä ohjelmallisesti BLOB [Azure-Blob-säiliö käyttämällä .NET käytön aloittaminen](storage-dotnet-how-to-use-blobs.md) .

##<a name="access-blob-containers-in-code"></a>Access-Blob-objektien säilöjä koodi

Ohjelmallisesti BLOB ASP.NET-5 projekteissa, tarvitset lisää seuraavat kohteet, jos ne eivät jo olemassa.

1. Lisää minkä tahansa C# jossa, jota haluat käyttää ohjelmallisesti Azure tallennustilan yläpuolelle koodin nimitilan-määrityksiä.

        using Microsoft.Extensions.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Extensions.Logging.LogLevel;

2. Pyydä **CloudStorageAccount** objekti, joka esittää tallennustilan tilin tiedot. Seuraava koodi avulla saat-tallennustilan yhteysmerkkijonon ja Azure palvelun tallennustilan tilin tiedot.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

    **Huomautus:** Käyttää kaikkia edellä koodin lukukoodin seuraavissa osissa.


3. Saat aiempaa säilöä **CloudBlobContainer** viittaa tallennustilan tilisi **CloudBlobClient** objektin avulla.

        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        // Get a reference to a container named “mycontainer.”
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");



##<a name="create-a-container-in-code"></a>Luoda säilön koodi

Voit myös luoda säilön tallennustilan tilisi **CloudBlobClient** . Kaikki sinun on suoritettava on **CreateIfNotExistsAsync** puhelun kuin seuraava koodi:

    // Create a blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get a reference to a container named “my-new-container.”
    CloudBlobContainer container = blobClient.GetContainerReference("my-new-container");

    // If “mycontainer” doesn’t exist, create it.
    await container.CreateIfNotExistsAsync();


**Huomautus:** API, jotka suorittavat Azure tallennustilan puhelut ASP.NET-5 on asynkroninen. Lisätietoja on kohdassa [asynkroninen ohjelmointi asynkroninen ja Await](http://msdn.microsoft.com/library/hh191443.aspx) . Seuraava koodi olettaa asynkroninen selaimessa käytetään.

Tekemään säilöön tiedostoja kaikille, voit määrittää säilö on julkinen käyttämällä seuraava koodi.

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });

##<a name="upload-a-blob-into-a-container"></a>Lataa blob säilöön

Lataa blob tiedosto säilöön, hanki säilö viittauksen ja käyttää sitä saat blob-viite. Kun sinulla on blob-viite, voit ladata kaikki tiedot stream siihen **UploadFromStreamAsync** -menetelmällä. Tämä toiminto luo blob, jos se ei ole vielä lisätty tai korvaa se, jos sitä ole. Seuraavassa esimerkissä esitetään, kuinka voit ladata blob säilöön ja oletetaan, että säilö on jo luotu.

    // Get a reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with the contents of a local file
    // named “myfile”.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        await blockBlob.UploadFromStreamAsync(fileStream);
    }

##<a name="list-the-blobs-in-a-container"></a>Luettelon BLOB-säilö
Luettelon BLOB-säilö, hanki säilö viittaus. Voit kutsua sitten säilö **ListBlobsSegmentedAsync** menetelmä noutamiseen BLOB-objektit ja/tai hakemistoja sitä. Yhteyden ominaisuuksien ja menetelmien monipuoliset palautettu **IListBlobItem**pakottaa sen **CloudBlockBlob**, **CloudPageBlob**tai **CloudBlobDirectory** objektiin. Tiedä blob-tyyppi, voit määrittää, johon haluat liittää sen tyyppi-valintaruutu. Seuraava koodi esitellään, miten haluat hakea ja tulosteen säilön kunkin kohteen URI.

    BlobContinuationToken token = null;
    do
    {
        BlobResultSegment resultSegment = await container.ListBlobsSegmentedAsync(token);
        token = resultSegment.ContinuationToken;

        foreach (IListBlobItem item in resultSegment.Results)
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
    } while (token != null);

On muita tapoja blob-säilö sisällön. Saat lisätietoja [Azure-Blob-säiliö käyttämällä .NET käytön aloittaminen](storage-dotnet-how-to-use-blobs.md#list-the-blobs-in-a-container) .

##<a name="download-a-blob"></a>Lataa blob
Voit ladata blob-haluat ensin blob viitata ja kutsu **DownloadToStreamAsync** -menetelmää. Seuraavassa esimerkissä **DownloadToStreamAsync** menetelmä blob sisällön siirtämiseen stream objektia, jonka voit sitten tallentaa paikallisen tiedoston.

    // Get a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save the blob contents to a file named “myfile”.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        await blockBlob.DownloadToStreamAsync(fileStream);
    }

On muita tapoja tallentaa BLOB-tiedostoiksi. Saat lisätietoja [Azure-Blob-säiliö käyttämällä .NET käytön aloittaminen](storage-dotnet-how-to-use-blobs.md#download-blobs) .

##<a name="delete-a-blob"></a>Poista blob
Poista blob haluat ensin blob viitata ja kutsua sitten **DeleteAsync** -menetelmää.

    // Get a reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    await blockBlob.DeleteAsync();

## <a name="next-steps"></a>Seuraavat vaiheet

[AZURE.INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]
