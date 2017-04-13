<properties 
    pageTitle="Lataa mediasisältöä" 
    description="Katso tietoja resurssien lataaminen tietokoneeseen. MALLIKOODEJA kirjoitetaan C# ja käyttää Media Services SDK .NET." 
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
    ms.date="09/26/2016"
    ms.author="juliako"/>

#<a name="how-to-deliver-an-asset-by-download"></a>Toimintaohje: pitää ladata resurssi

Tässä ohjeaiheessa kerrotaan välittää mediasisältöä ladataan Media Servicesin asetukset. Voit näyttää Media Services sisällön monia sovelluksen tilanteissa. Voit ladata mediasisältöä tai käyttää niitä locator avulla. Voit lähettää mediasisältöä toiseen sovellukseen tai toiseen sisältötoimittajan. Parannettu suorituskyky ja skaalattavuus voit näyttää sisällön myös käyttämällä sisällön toimituksen verkon (CDN).

Tässä esimerkissä näytetään mediasisältöä lataamisesta Media Services paikalliseen tietokoneeseen. Koodin kyselyt Media Services-tilin sijaan työn tunnuksen liittyvät työt ja käyttää sen **OutputMediaAssets** sivustokokoelman (joka on vähintään yksi tulos mediasisältöä joukko suorittamisen työn tuloksena). Tässä esimerkissä näytetään tulosteen mediasisältöä lataamisesta työn, mutta voit käyttää samoin voit ladata muita varoja.

    
    // Download the output asset of the specified job to a local folder.
    static IAsset DownloadAssetToLocal( string jobId, string outputFolder)
    {
        // This method illustrates how to download a single asset. 
        // However, you can iterate through the OutputAssets
        // collection, and download all assets if there are many. 
    
        // Get a reference to the job. 
        IJob job = GetJob(jobId);
    
        // Get a reference to the first output asset. If there were multiple 
        // output media assets you could iterate and handle each one.
        IAsset outputAsset = job.OutputMediaAssets[0];
    
        // Create a SAS locator to download the asset
        IAccessPolicy accessPolicy = _context.AccessPolicies.Create("File Download Policy", TimeSpan.FromDays(30), AccessPermissions.Read);
        ILocator locator = _context.Locators.CreateLocator(LocatorType.Sas, outputAsset, accessPolicy);
    
        BlobTransferClient blobTransfer = new BlobTransferClient
        {
            NumberOfConcurrentTransfers = 20,
            ParallelTransferThreadCount = 20
        };
    
        var downloadTasks = new List<Task>();
        foreach (IAssetFile outputFile in outputAsset.AssetFiles)
        {
            // Use the following event handler to check download progress.
            outputFile.DownloadProgressChanged += DownloadProgress;
    
            string localDownloadPath = Path.Combine(outputFolder, outputFile.Name);
    
            Console.WriteLine("File download path:  " + localDownloadPath);
    
            downloadTasks.Add(outputFile.DownloadAsync(Path.GetFullPath(localDownloadPath), blobTransfer, locator, CancellationToken.None));
    
            outputFile.DownloadProgressChanged -= DownloadProgress;
        }
    
        Task.WaitAll(downloadTasks.ToArray());
    
        return outputAsset;
    }
    
    static void DownloadProgress(object sender, DownloadProgressChangedEventArgs e)
    {
        Console.WriteLine(string.Format("{0} % download progress. ", e.Progress));
    }



##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

   
##<a name="see-also"></a>Katso myös 

[Toimita streaming sisältö](media-services-deliver-streaming-content.md)

