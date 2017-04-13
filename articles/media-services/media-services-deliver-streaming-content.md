<properties 
    pageTitle="Käyttämällä .NET Azure Media-palveluiden sisällön julkaiseminen" 
    description="Opettele luomaan URL-osoite, voidaan luoda streaming URL-osoite. MALLIKOODEJA kirjoitetaan C# ja käyttää Media Services SDK .NET." 
    authors="juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/30/2016"
    ms.author="juliako"/>


# <a name="publish-azure-media-services-content-using-net"></a>Käyttämällä .NET Azure Media-palveluiden sisällön julkaiseminen
 
> [AZURE.SELECTOR]
- [MUILLE KÄYTTÄJILLE](media-services-rest-deliver-streaming-content.md)
- [.NET](media-services-deliver-streaming-content.md)
- [Portal](media-services-portal-publish.md)

##<a name="overview"></a>Yleiskatsaus

Voit käyttää mukautuvat bittitaajuus MP4 määrittäminen luomalla streaming OnDemand-locator ja rakentaminen streaming URL-osoite. [Sijoituksen koodaus](media-services-encode-asset.md) -ohjeaiheessa esitellään, miten koodata mukautuvat bittitaajuus, joka määrittää MP4 kyselyjä. 

>[AZURE.NOTE]Jos sisältö on salattu, resurssi toimituksen käytännön määrittäminen ( [Tässä](media-services-dotnet-configure-asset-delivery-policy.md) ohjeaiheessa kuvatulla) ennen kuin luot locator. 

Voit käyttää myös OnDemand streaming locator luonnissa URL-osoitteet, jotka viittaavat MP4-tiedostoja, jotka voivat ladata asteittain.  

Tässä ohjeaiheessa esitellään luomisesta OnDemand streaming locator julkaista oman resurssi ja tasainen, MPEG VIIVA ja HLS streaming URL-osoitteet. Se näyttää myös kuuma luonnissa asteittain Lataa URL-osoitteet. 
     
##<a name="create-an-ondemand-streaming-locator"></a>Luo OnDemand streaming locator

Haluat luoda streaming locator OnDemand ja URL-osoitteet seuraavasti:

   1. Jos sisältö on salattu, Määritä käyttöoikeuskäytäntö.
   2. Luo OnDemand streaming locator.
   3. Jos aiot käyttää, saat kohteen streaming luettelon tiedoston (.ism). 
        
    Jos aiot ladata asteittain, saat kohteen MP4-tiedostojen nimet.  
   4. Luoda URL-osoitteiden luettelon tiedostoon tai MP4-tiedostot. 
   

###<a name="use-media-services-net-sdk"></a>Media-palveluiden .NET SDK-paketissa 

Muodosta Streaming URL-osoitteet 

    private static void BuildStreamingURLs(IAsset asset)
    {
    
        // Create a 30-day readonly access policy. 
        // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);
    
        // Create a locator to the streaming content on an origin. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));
    
        // Display some useful values based on the locator.
        Console.WriteLine("Streaming asset base path on origin: ");
        Console.WriteLine(originLocator.Path);
        Console.WriteLine();
    
        // Get a reference to the streaming manifest file from the  
        // collection of files in the asset. 
        var manifestFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                    EndsWith(".ism")).
                                    FirstOrDefault();
        
        // Create a full URL to the manifest file. Use this for playback
        // in streaming media clients. 
        string urlForClientStreaming = originLocator.Path + manifestFile.Name + "/manifest";
        Console.WriteLine("URL to manifest for client streaming using Smooth Streaming protocol: ");
        Console.WriteLine(urlForClientStreaming);
        Console.WriteLine("URL to manifest for client streaming using HLS protocol: ");
        Console.WriteLine(urlForClientStreaming + "(format=m3u8-aapl)");
        Console.WriteLine("URL to manifest for client streaming using MPEG DASH protocol: ");
        Console.WriteLine(urlForClientStreaming + "(format=mpd-time-csf)"); 
        Console.WriteLine();
    }

Koodin tulostaa:
    
    URL to manifest for client streaming using Smooth Streaming protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest
    URL to manifest for client streaming using HLS protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=m3u8-aapl)
    URL to manifest for client streaming using MPEG DASH protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=mpd-time-csf)
    

>[AZURE.NOTE]Voit myös käyttää sisältöä SSL-yhteyttä. Toiminto, varmista, että streaming URL-osoitteet alkavat HTTPS. 

Muodosta asteittain Lataa URL-osoitteet 

    private static void BuildProgressiveDownloadURLs(IAsset asset)
    {
        // Create a 30-day readonly access policy. 
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);
    
        // Create an OnDemandOrigin locator to the asset. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));
    
        // Display some useful values based on the locator.
        Console.WriteLine("Streaming asset base path on origin: ");
        Console.WriteLine(originLocator.Path);
        Console.WriteLine();
    
        // Get MP4 files.
        IEnumerable<IAssetFile> mp4AssetFiles = asset
            .AssetFiles
            .ToList()
            .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));
                
        // Create a full URL to the MP4 files. Use this to progressively download files.
        foreach (var pd in mp4AssetFiles)
            Console.WriteLine(originLocator.Path + pd.Name);
    }

Koodin tulostaa:
    
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4
    
    . . . 

###<a name="use-media-services-net-sdk-extensions"></a>Käytä Media Services .NET SDK-laajennukset

Seuraava koodi kutsuu .NET SDK tunnisteet menetelmiä, jotka locator luominen ja luo mukautuvat streaming tasainen Streaming, HLS ja MPEG-VIIVA URL-osoitteet.

    // Create a loctor.
    _context.Locators.Create(
        LocatorType.OnDemandOrigin,
        inputAsset,
        AccessPermissions.Read,
        TimeSpan.FromDays(30));
    
    // Get the streaming URLs.
    Uri smoothStreamingUri = inputAsset.GetSmoothStreamingUri();
    Uri hlsUri = inputAsset.GetHlsUri();
    Uri mpegDashUri = inputAsset.GetMpegDashUri();
    
    Console.WriteLine(smoothStreamingUri);
    Console.WriteLine(hlsUri);
    Console.WriteLine(mpegDashUri);


##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Katso myös

[Lataa varat](media-services-deliver-asset-download.md)
[määrittäminen resurssi toimituksen käytäntö](media-services-dotnet-configure-asset-delivery-policy.md)
