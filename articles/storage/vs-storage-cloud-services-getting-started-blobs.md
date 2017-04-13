<properties
    pageTitle="Aloita blob storage ja Visual Studio yhteydessä services (pilvipalveluihin) | Microsoft Azure"
    description="Pikaviestien käyttäminen Azure-Blob-säiliö cloud palvelun projektin Visual Studiossa, kun yhteyden Visual Studiossa tallennustilan tilin yhdistetyt palvelut"
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

# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Azure-Blob-säiliö ja Visual Studio aloittaminen yhdistetyt palvelut (cloud services projektit)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Yleiskatsaus

Tässä artikkelissa käsitellään aloittaminen Azure-Blob-säiliö, kun olet luonut tai Viitattu Azure-tallennustilan tilin Visual Studio **Lisää yhdistetyt palvelut** -valintaikkunan avulla Visual Studio cloud services-projekti. Esitellään että käyttämisestä ja Blob-objektien säilöjen luominen ja niiden suorittaa yleisiä tehtäviä, kuten lataaminen, luettelon ja lataaminen BLOB-objektit. Mallit on kirjoitettu C\# ja käytä [Microsoft Azure tallennustilan asiakkaan kirjasto .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

Azure-Blob-säiliö on paljon erimuotoisia tietoja, joita voi käyttää mitä tahansa HTTP tai HTTPS maailmanlaajuisesti tallentamista varten. Yksittäisen Blob-objektien voi olla minkä kokoinen. BLOB voi olla esimerkiksi kuvia, ääni- ja videotiedostoista, tietoja ja asiakirja.

Samalla tavalla kuin tiedostot tallennetaan kansiot-BLOB storage live säilöjen. Luotuasi tallennustilan voit luoda yhden tai useamman muistiin. Esimerkiksi tallennustilaan, jota kutsutaan "Leikekirjan", voit luoda säilöjen nimeltä "kuvat" kuvia tallennetaan muistiin ja toinen nimeltään "ääni" ääni tallentamista varten. Kun olet luonut säilöt, voit ladata ne Blob-objektien yksittäisiä tiedostoja.

- Lisätietoja käsittelevistä ohjelmallisesti BLOB on artikkelissa [Azure-Blob-säiliö käyttämällä .NET käytön aloittaminen](storage-dotnet-how-to-use-blobs.md).
- Yleisiä tietoja Azuren tallennustilaan [tallennustilan](https://azure.microsoft.com/documentation/services/storage/)ohjeissa.
- Yleisiä tietoja Azure pilvipalveluihin [Pilvipalveluihin](https://azure.microsoft.com/documentation/services/cloud-services/)ohjeissa.
- Saat lisätietoja ohjelmointi ASP.NET-sovelluksia [ASP.NET](http://www.asp.net).

## <a name="access-blob-containers-in-code"></a>Access-Blob-objektien säilöjä koodi

Ohjelmallisesti BLOB cloud palvelun projekteissa, tarvitset lisää seuraavat kohteet, jos ne eivät jo olemassa.

1. Lisää minkä tahansa C# jossa haluat käyttää ohjelmallisesti Azuren tallennustilaan yläpuolelle koodin nimitilan-määrityksiä.

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;

2. Pyydä **CloudStorageAccount** objekti, joka esittää tallennustilan tilin tiedot. Seuraava koodi avulla saat-tallennustilan yhteysmerkkijonon ja Azure palvelun tallennustilan tilin tiedot.

        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("<storage account name>_AzureStorageConnectionString"));

3. Pyydä **CloudBlobClient** -objektin viittaamaan aiemmin säilö, tallennustilan tilissäsi.

        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

4. Pyydä **CloudBlobContainer** -objektin viittaamaan tietyn blob-säilö.

        // Get a reference to a container named “mycontainer.”
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

> [AZURE.NOTE] Käytä koko koodi näkyvät seuraavassa lukukoodin edellä annettujen ohjeiden mukaisesti.

## <a name="create-a-container-in-code"></a>Luoda säilön koodi

> [AZURE.NOTE] Jotkin, jotka suorittavat puhelut ulos Azuren tallennustilaan ASP.NET-ohjelmointirajapinnan on asynkroninen. Lisätietoja on kohdassa [asynkroninen ohjelmointi asynkroninen ja Await](http://msdn.microsoft.com/library/hh191443.aspx) . Seuraavassa esimerkissä koodin oletetaan, kun käytät asynkroninen selaimessa.

Voit luoda säilön tallennustilan tilisi, sinun tarvitsee on **CreateIfNotExistsAsync** puhelun kuin seuraava koodi:

    // If “mycontainer” doesn’t exist, create it.
    await container.CreateIfNotExistsAsync();


Tekemään säilöön tiedostoja kaikille, voit määrittää säilö on julkinen käyttämällä seuraava koodi.

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });


Kuka tahansa Internetissä kohdassa julkinen säilön BLOB, mutta voit muokata tai poistaa ne vain, jos sinulla on riittävät käyttöoikeudet-näppäintä.

## <a name="upload-a-blob-into-a-container"></a>Lataa blob säilöön

Azure-tallennustilan tukee estä BLOB-objektit ja sivun BLOB-objektit. Useimmissa tapauksissa estä blob on käytettävä suositellut tyyppi.

Voit ladata tiedoston lohko-blob-säilö viittauksen hankkimalla ja avulla saat lohko-blob-viite. Kun blob-viite, voit ladata kaikki tiedot stream siihen **UploadFromStream** -menetelmällä. Tämä toiminto luo blob, jos se ei ole aiemmin tai korvaa se, jos sitä ole. Seuraavassa esimerkissä esitetään, kuinka voit ladata blob säilöön ja oletetaan, että säilö on jo luotu.

    // Retrieve a reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## <a name="list-the-blobs-in-a-container"></a>Luettelon BLOB-säilö

Luettelon BLOB-säilö, hanki säilö viittaus. Voit hakea BLOB-objektit ja/tai hakemistoja se sitten säilö **ListBlobs** menetelmän avulla. Yhteyden ominaisuuksien ja menetelmien monipuoliset palautettu **IListBlobItem**pakottaa sen **CloudBlockBlob**, **CloudPageBlob**tai **CloudBlobDirectory** objektiin. Jos tyyppi on tuntematon, voit määrittää, johon haluat liittää sen tyyppi-valintaruutu. Seuraava koodi kerrotaan, miten noutaa ja Näytä **Valokuvat** -säilössä kunkin kohteen URI:

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

Edellisen esitetyllä blob-palvelulla säilöjen sekä hakemistoja käsite. Tämä on niin, että voit järjestää oman BLOB Lisää kansion kaltaisessa rakenteessa. Esimerkkinä estä BLOB-säilö **Valokuvat**seuraavat määrittäminen:

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

Kun soitat **ListBlobs** -säilöä (kuten Edellinen malli), palautetaan kokoelma sisältää **CloudBlobDirectory** ja **CloudBlockBlob** objektit, jotka edustavat kansioiden ja BLOB sisälsi ylimmällä tasolla. Näin tulos:

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


Voit halutessasi määrittää **UseFlatBlobListing** parametrin **ListBlobs** menetelmän **Tosi**. Tämä aiheuttaa jokaisen blob palautettavien kuin **CloudBlockBlob**, riippumatta siitä, hakemisto. Näin **ListBlobs**puhelu:

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

ja seuraavat tulokset:

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg

Lisätietoja on artikkelissa [CloudBlobContainer.ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx).

## <a name="download-blobs"></a>Lataa BLOB-objektit

Voit ladata BLOB-ensin hakea blob-viite ja kutsu **DownloadToStream** -menetelmää. Seuraavassa esimerkissä **DownloadToStream** menetelmä blob sisällön siirtämiseen stream objektia, jonka voit sitten Poistu paikalliseen tiedostoon.

    // Get a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents to a file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

Voit myös ladata Blob-objektien merkkijonomuotoisen sisällön **DownloadToStream** -menetelmää.

    // Get a reference to a blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## <a name="delete-blobs"></a>Poista BLOB-objektit

Voit poistaa blob-haluat ensin blob viitata ja kutsua sitten **Delete** -menetelmällä.

    // Get a reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    blockBlob.Delete();


## <a name="list-blobs-in-pages-asynchronously"></a>Luettelon BLOB-sivujen asynkronisesti

Jos on luettelo on runsaasti BLOB tai haluat palauttaa luettelon-toiminnossa tulosten määrää, voit lisätä BLOB tulosten sivuilta. Tässä esimerkissä esitetään, kuinka palauttamaan tulokset sivujen asynkronisesti, niin, että suorittamisen ei ole estetty odotettaessa palauttaa tuloksia suuri joukko.

Tässä esimerkissä näytetään tasainen Blob-objektien päälle, mutta voit suorittaa hierarkkisen luettelon määrittämällä **ListBlobsSegmentedAsync** -menetelmän **useFlatBlobListing** parametrin arvoksi **false**.

Esimerkki menetelmä soittaa asynkroninen menetelmä, koska se etuliitteenä **asynkroninen** -avainsanalla ja se on palautettava objekti on **tehtävä** . Määritetyn **ListBlobsSegmentedAsync** await avainsanan keskeyttää otoksen menetelmän suorittamista, kunnes luettelon tehtävä on valmis.

    async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
    {
        // List blobs to the console window, with paging.
        Console.WriteLine("List blobs in pages:");

        int i = 0;
        BlobContinuationToken continuationToken = null;
        BlobResultSegment resultSegment = null;

        // Call ListBlobsSegmentedAsync and enumerate the result segment returned, while the continuation token is non-null.
        // When the continuation token is null, the last page has been returned and execution can exit the loop.
        do
        {
            // This overload allows control of the page size. You can return all remaining results by passing null for the maxResults parameter,
            // or by calling a different overload.
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

## <a name="next-steps"></a>Seuraavat vaiheet

[AZURE.INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]
