<properties 
    pageTitle="Tiedostojen lataaminen Media-palveluita käyttämällä .NET tilille | Microsoft Azure" 
    description="Opettele mediasisältöä tuoda Media Services luomalla ja resurssien ladataan." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="juliako"/>



# <a name="upload-files-into-a-media-services-account-using-net"></a>Tiedostojen lataaminen käyttämällä .NET Media Services-tilille

 > [AZURE.SELECTOR]
 - [.NET](media-services-dotnet-upload-files.md)
 - [MUILLE KÄYTTÄJILLE](media-services-rest-upload-files.md)
 - [Portal](media-services-portal-upload-files.md)

Media-palveluja voit ladata (tai ingest) digitaalisten tiedostojen sijoituksen kyselyjä. **Kohteiden** kohde voi olla video, ääni, kuvia, pikkukuvan sivustokokoelmat, tekstin raidat tekstitys tiedostot (ja nämä tiedostot metatietoa.)  Kun tiedostot on ladattu palvelimeen, sisällön tallennetaan Pilvipalvelun myöhempää käsittelyä ja streaming suojatusti.

Kohteen tiedostoja kutsutaan **Resurssi-tiedostoiksi**. **AssetFile** esiintymän ja todellinen mediatiedosto on kaksi erillistä objektia. AssetFile esiintymää on metatietoa mediatiedosto, mediatiedosto on todellinen mediasisältöä.

>[AZURE.NOTE]Kun valitset resurssi-tiedostonimi ottaa huomioon seuraavat asiat:
>
>- Media Services käyttää IAssetFile.Name-ominaisuuden arvon luotaessa URL-osoitteet streaming sisällön (esimerkiksi http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Tästä syystä URL-koodausta ei sallita. Ominaisuuden **nimi** ei saa olla seuraavia [prosentti-koodaus-varattu merkit](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Lisäksi voi olla vain yksi ".' tiedostotunnisteen varten.
>
>- Nimen pituus ei pitäisi olla suurempi kuin 260 merkkiä.

Kun luot varat, voit määrittää seuraavat salausasetukset. 

- **Ei mitään** - salausta käytetään. Tämä on oletusarvo. Huomaa, että kun tämä vaihtoehto sisältöä ei ole suojattu siirrossa tai muiden tallennustilaan.
Jos haluat pitää MP4 käyttämällä asteittain lataaminen, käytä tätä vaihtoehtoa. 
- **CommonEncryption** – Käytä tätä vaihtoehtoa, jos lataat sisältöä, joka on jo salattu ja suojattu Yleiset-salausta tai PlayReady DRM (esimerkiksi tasainen Streaming suojattu with PlayReady DRM).
- **EnvelopeEncrypted** – Käytä tätä vaihtoehtoa, jos lataat HLS AES salattu. Huomaa, että tiedostot on koodattu ja salattu Transform hallinta.
- **StorageEncrypted** - salaa käyttämällä paikallisesti 256 bitti AES-salausta ja sen Azure-tallennustilan, johon se on tallennettu salattu lataukset muiden Tyhjennä sisältö. Varat suojannut tallennustilan salaus automaattisesti salaamattomana ja sijoitetaan salattuja tiedostojärjestelmässä ennen koodaus ja halutessasi uudelleen salataan ennen lataamista takaisin kuin uuden tulostus-resurssi. Tallennustilan salauksen ensisijainen käyttötapaus on, kun haluat laadukkaita mediatiedostojen suojaaminen salauksesta osoitteessa muille käyttäjille levyn.

    Media-palvelut on levyn tallennustilan salaus kohteiden, ei over-johdinta, kuten digitaalisten oikeuksien hallinta (DRM).

    Jos yhteyttä resurssi on salattu tallennustilan, sinun on määritettävä resurssi toimituksen käytännön. Katso lisätietoja [määrittäminen resurssi toimituksen käytännön](media-services-dotnet-configure-asset-delivery-policy.md).

Jos määrität, että annetaan salata **CommonEncrypted** -vaihtoehto tai **EnvelopeEncypted** -vaihtoehdon kanssa, sinun on liitettävä **ContentKey**oman resurssi. Lisätietoja on artikkelissa [luomisesta ContentKey](media-services-dotnet-create-contentkey.md). 

Jos määrität, että annetaan salata **StorageEncrypted** -vaihtoehto, .NET Media-palveluiden SDK Luo **StorateEncrypted** **ContentKey** , että annetaan.


Tässä ohjeaiheessa kerrotaan, miten Media Services .NET SDK sekä Media Services .NET SDK tunnisteet tiedostojen lataaminen Media Services-kohteiden tuominen.

 
## <a name="upload-a-single-file-with-media-services-net-sdk"></a>Ladata yhden tiedoston, jonka Media Services .NET SDK 

Seuraava esimerkki koodi käyttämällä .NET SDK suorittaa seuraavia tehtäviä: 

- Luo tyhjä resurssi.
- Luo erillisen AssetFile, jotka haluat liittää kohteen.
- Luo erillisen AccessPolicy, joka määrittää käyttöoikeuksia ja keston access kohteen.
- Luo Locator esiintymän, joka tarjoaa kohteen.
- Lataa yksittäinen mediatiedosto kyselyjä Media-palveluita. 

        
        static public IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string singleFilePath)
        {
            if (!File.Exists(singleFilePath))
            {
                Console.WriteLine("File does not exist.");
                return null;
            }

            var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
            IAsset inputAsset = _context.Assets.Create(assetName, assetCreationOptions); 

            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));

            Console.WriteLine("Created assetFile {0}", assetFile.Name);

            var policy = _context.AccessPolicies.Create(
                                    assetName,
                                    TimeSpan.FromDays(30),
                                    AccessPermissions.Write | AccessPermissions.List);

            var locator = _context.Locators.CreateLocator(LocatorType.Sas, inputAsset, policy);

            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            locator.Delete();
            policy.Delete();

            return inputAsset;
        }

##<a name="upload-multiple-files-with-media-services-net-sdk"></a>Lataa useita tiedostoja, joissa Media Services .NET SDK 

Seuraava koodi näytetään, miten voit luoda sijoituksen ja Lataa useita tiedostoja.

Koodin tekee seuraavat toimet:
    
-   Luo tyhjä resurssi, joka on määritetty edellisessä vaiheessa CreateEmptyAsset-menetelmällä.
    
-   Luo erillisen **AccessPolicy** , joka määrittää käyttöoikeuksia ja keston access kohteen.
    
-   Luo **Locator** esiintymän, joka tarjoaa kohteen.
    
-   Luo **BlobTransferClient** esiintymän. Tällaista edustaa asiakas, joka toimii Azure BLOB-objektit. Tässä esimerkissä Käytämme asiakkaan latauksen etenemistä. 
    
-   Luetteloi tiedostoja ovat määritettyyn kansioon ja luo erillisen **AssetFile** kaikille tuotaville tiedostoille.
    
-   Lataa tiedostot yhdeksi Media Services **UploadAsync** -menetelmällä. 
    
>[AZURE.NOTE] UploadAsync menetelmän avulla voit varmistaa, että kutsut ei estä ja tiedostoja ladataan rinnakkain.
    
    
        static public IAsset CreateAssetAndUploadMultipleFiles(AssetCreationOptions assetCreationOptions, string folderPath)
        {
            var assetName = "UploadMultipleFiles_" + DateTime.UtcNow.ToString();

            IAsset asset = _context.Assets.Create(assetName, assetCreationOptions);

            var accessPolicy = _context.AccessPolicies.Create(assetName, TimeSpan.FromDays(30),
                                                                AccessPermissions.Write | AccessPermissions.List);

            var locator = _context.Locators.CreateLocator(LocatorType.Sas, asset, accessPolicy);

            var blobTransferClient = new BlobTransferClient();
            blobTransferClient.NumberOfConcurrentTransfers = 20;
            blobTransferClient.ParallelTransferThreadCount = 20;

            blobTransferClient.TransferProgressChanged += blobTransferClient_TransferProgressChanged;

            var filePaths = Directory.EnumerateFiles(folderPath);

            Console.WriteLine("There are {0} files in {1}", filePaths.Count(), folderPath);

            if (!filePaths.Any())
            {
                throw new FileNotFoundException(String.Format("No files in directory, check folderPath: {0}", folderPath));
            }

            var uploadTasks = new List<Task>();
            foreach (var filePath in filePaths)
            {
                var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                Console.WriteLine("Created assetFile {0}", assetFile.Name);

                // It is recommended to validate AccestFiles before upload. 
                Console.WriteLine("Start uploading of {0}", assetFile.Name);
                uploadTasks.Add(assetFile.UploadAsync(filePath, blobTransferClient, locator, CancellationToken.None));
            }

            Task.WaitAll(uploadTasks.ToArray());
            Console.WriteLine("Done uploading the files");

            blobTransferClient.TransferProgressChanged -= blobTransferClient_TransferProgressChanged;

            locator.Delete();
            accessPolicy.Delete();

            return asset;
        }
    
    static void  blobTransferClient_TransferProgressChanged(object sender, BlobTransferProgressChangedEventArgs e)
    {
        if (e.ProgressPercentage > 4) // Avoid startup jitter, as the upload tasks are added.
        {
            Console.WriteLine("{0}% upload competed for {1}.", e.ProgressPercentage, e.LocalFile);
        }
    }



Kun lataus on paljon resursseja, toimi seuraavasti.

- Luo uusi **CloudMediaContext** objekti viestiketjun kohden. **CloudMediaContext** luokka ei ole Turvalliset viestiketjun.
 
- Kasvaa NumberOfConcurrentTransfers oletusarvoa 2 kuten 5 suurempi arvo. Tämän ominaisuuden määrittäminen vaikuttaa **CloudMediaContext**kaikki esiintymät. 
 
- Säilyttää ParallelTransferThreadCount 10 oletusarvon.
 
##<a id="ingest_in_bulk"></a>Ingesting varat joukkona Media Services .NET SDK: N avulla 

Suuri resurssi tiedostojen lataamisesta voi olla pullonkaula resurssi luonnin aikana. Ingesting joukkona tai "Joukkona Ingesting" varat liittyy irtautus lataus luomisen resurssi. Käyttämään ingesting lähestymistavan syötteessä Luo luettelo (IngestManifest), joka kuvaa kohteen ja siihen liittyvät tiedostot. Lataa tiedostot luettelo blob-säilö käyttää valittua Lataa-menetelmää. Microsoft Azure Media Services seuraa liittyvä luettelo blob-säilö. Kun tiedosto ladataan blob-säilö, Microsoft Azure Media Services suorittaa resurssi-luettelo (IngestManifestAsset) määrittäminen perustuu kohteiden luominen.


Voit luoda uuden IngestManifest puhelun näyttämiä CloudMediaContext IngestManifests-sivustokokoelman luominen-menetelmää. Tämä menetelmä luo uuden IngestManifest antaisit luettelon nimi.

    IIngestManifest manifest = context.IngestManifests.Create(name);

Luo ne kalusteet, joiden päivitetään usean IngestManifest. Kohteen osalta samalla kertaa ingesting haluamasi salaus-asetusten määrittäminen.

    // Create the assets that will be associated with this bulk ingest manifest
    IAsset destAsset1 = _context.Assets.Create(name + "_asset_1", AssetCreationOptions.None);
    IAsset destAsset2 = _context.Assets.Create(name + "_asset_2", AssetCreationOptions.None);

IngestManifestAsset liittää sijoituksen joukkona IngestManifest joukkona ingesting varten. Se myös liittää AssetFiles, jotka muodostavat kunkin resurssi. Voit luoda IngestManifestAsset käyttämällä Create-menetelmä palvelimen kontekstissa.

Seuraavassa esimerkissä lisääminen kahden uuden IngestManifestAssets, joka yhdistää kaksi varat aiemmin luotu usean ingest luettelo. Kunkin IngestManifestAsset liittää kunkin osalta, jotka voi ladata tiedostoja myös joukkona ingesting aikana.  

    string filename1 = _singleInputMp4Path;
    string filename2 = _primaryFilePath;
    string filename3 = _singleInputFilePath;
    
    IIngestManifestAsset bulkAsset1 =  manifest.IngestManifestAssets.Create(destAsset1, new[] { filename1 });
    IIngestManifestAsset bulkAsset2 =  manifest.IngestManifestAssets.Create(destAsset2, new[] { filename2, filename3 });
    
Voit käyttää minkä tahansa nopea asiakassovellus voi resurssi-tiedostojen lataamisesta blob storage säilöön URI IngestManifest **IIngestManifest.BlobStorageUriForUpload** -ominaisuuden myöntämä. Yksi huomattavat nopea Lataa palvelu on [Aspera pyydettäessä Azure-sovelluksen](https://datamarket.azure.com/application/2cdbc511-cb12-4715-9871-c7e7fbbb82a6). Voit myös kirjoittaa koodin lisäämään kalusto-tiedostoja koodin seuraavan esimerkin mukaisesti.
    
    static void UploadBlobFile(string destBlobURI, string filename)
    {
        Task copytask = new Task(() =>
        {
            var storageaccount = new CloudStorageAccount(new StorageCredentials(_storageAccountName, _storageAccountKey), true);
            CloudBlobClient blobClient = storageaccount.CreateCloudBlobClient();
            CloudBlobContainer blobContainer = blobClient.GetContainerReference(destBlobURI);
    
            string[] splitfilename = filename.Split('\\');
            var blob = blobContainer.GetBlockBlobReference(splitfilename[splitfilename.Length - 1]);
    
            using (var stream = System.IO.File.OpenRead(filename))
                blob.UploadFromStream(stream);
    
            lock (consoleWriteLock)
            {
                Console.WriteLine("Upload for {0} completed.", filename);
            }
        });
    
        copytask.Start();
    }

Tässä ohjeaiheessa käytetyn näytteen resurssi tiedostot ladataan koodi näkyy koodi seuraavan esimerkin mukaisesti.
    
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename1);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename2);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename3);
    

Voit määrittää etenemisen joukkona ingesting, kaikki kalusto **IngestManifest** liittyvät kyselyt **IngestManifest**tilasto-ominaisuuden avulla. Jotta voit päivittää edistymistiedot, sinun on käytettävä uusi **CloudMediaContext** aina, kun tilasto-ominaisuus kyselyn.

Seuraavassa esimerkissä kysely IngestManifest muodon **tunnuksen**.
    
    static void MonitorBulkManifest(string manifestID)
    {
       bool bContinue = true;
       while (bContinue)
       {
          CloudMediaContext context = GetContext();
          IIngestManifest manifest = context.IngestManifests.Where(m => m.Id == manifestID).FirstOrDefault();
    
          if (manifest != null)
          {
             lock(consoleWriteLock)
             {
                Console.WriteLine("\nWaiting on all file uploads.");
                Console.WriteLine("PendingFilesCount  : {0}", manifest.Statistics.PendingFilesCount);
                Console.WriteLine("FinishedFilesCount : {0}", manifest.Statistics.FinishedFilesCount);
                Console.WriteLine("{0}% complete.\n", (float)manifest.Statistics.FinishedFilesCount / (float)(manifest.Statistics.FinishedFilesCount + manifest.Statistics.PendingFilesCount) * 100);
    
                if (manifest.Statistics.PendingFilesCount == 0)
                {
                   Console.WriteLine("Completed\n");
                   bContinue = false;
                }
             }
    
             if (manifest.Statistics.FinishedFilesCount < manifest.Statistics.PendingFilesCount)
                Thread.Sleep(60000);
          }
          else // Manifest is null
             bContinue = false;
       }
    }
    


##<a name="upload-files-using-net-sdk-extensions"></a>Lataa tiedostot .NET SDK-laajennukset 

Alla olevassa esimerkissä lataaminen yksittäisen tiedoston käyttämällä .NET SDK tunnisteet. Tässä tapauksessa **CreateFromFile** -menetelmää käytetään, mutta asynkroninen versio on saatavilla (**CreateFromFileAsync**). **CreateFromFile** -menetelmällä voit määrittää tiedoston nimeä, salaus-asetusta ja takaisinkutsun raportoiminen tiedoston latauksen edistymisestä.


    static public IAsset UploadFile(string fileName, AssetCreationOptions options)
    {
        IAsset inputAsset = _context.Assets.CreateFromFile(
            fileName,
            options,
            (af, p) =>
            {
                Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });
    
        Console.WriteLine("Asset {0} created.", inputAsset.Id);
    
        return inputAsset;
    }

Seuraavassa esimerkissä UploadFile-funktiota ja määrittää tallennustilan salaus kohteiden luominen-asetus.  


    var asset = UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.StorageEncrypted);


##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="next-step"></a>Seuraava vaihe

Nyt kun lataamasi sijoituksen Media Services, siirry [hankkiminen Media-suoritin][] aihetta.

[Media-suoritin hankkiminen]: media-services-get-media-processor.md
 
