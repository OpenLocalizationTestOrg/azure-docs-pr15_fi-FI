<properties 
    pageTitle="Sijoituksen koodata käytettäessä Media Encoder Standard käyttämällä .NET | Microsoft Azure" 
    description="Tässä ohjeaiheessa esitellään, miten koodata resurssi Media Encoder Strandard käyttämällä .NET avulla." 
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
    ms.author="juliako;anilmur"/>


# <a name="encode-an-asset-with-media-encoder-standard-using-net"></a>Sijoituksen koodata Media Encoder vakio käyttämällä .NET kanssa

Koodauksen työt ovat yksi yleisimmistä käsittely-toimintoa Media Services-palveluissa. Voit luoda koodauksen töitä muunnettava mediatiedostojen koodaukset toiseen. Kun koodata, voit käyttää Media Services valmiin Media Encoder. Voit myös käyttää encoder myöntämä Media Services kumppanin; kolmannen osapuolen koodauslaitteista ovat saatavilla Azure Marketplacesta. 

Tässä ohjeaiheessa esitellään opit käyttämään .NET koodata oman varat kanssa Media Encoder Vakio (MES). Media Encoder vakio määritetään Käytä jotakin seuraavista encoder Esiasetukset kuvattu [seuraavassa](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

On suositeltavaa koodata aina mezzanine tiedostojen siirtäminen mukautuvat bittitaajuus MP4 määrittäminen ja muunna Määritä haluamasi [Dynaaminen pakkaus](media-services-dynamic-packaging-overview.md)-muodossa. Dynaaminen pakkaus hyödyntää, sinun täytyy saada vähintään yhden tarvittaessa streaming yksikön streaming päätepisteen, josta aiot toimituksen sisältöä. Katso lisätietoja, [miten asteikko Media Services](media-services-portal-manage-streaming-endpoints.md).

Jos tulos resurssi on salattu tallennustilan, sinun on määritettävä resurssi toimituksen käytännön. Katso lisätietoja [määrittäminen resurssi toimituksen käytännön](media-services-dotnet-configure-asset-delivery-policy.md).

>[AZURE.NOTE]Markkinatalousaseman tuottaa tulostus-tiedosto, jonka nimessä on 32 ensimmäistä merkkiä syötteen nimi. Mitä on määritetty ennalta määritetty tiedoston perustuu nimi. Esimerkiksi "tiedostonimi": "{Basename} {Index} {tunniste} _". {Basename} korvataan syötteen nimi 32 ensimmäistä merkkiä.

###<a name="mes-formats"></a>Markkinatalousaseman muodot

[Muotoilut ja pakkauksenhallinnat](media-services-media-encoder-standard-formats.md)

###<a name="mes-presets"></a>Markkinatalousaseman Esiasetukset

Media Encoder vakio määritetään Käytä jotakin seuraavista encoder Esiasetukset kuvattu [seuraavassa](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

###<a name="input-and-output-metadata"></a>Syöttö- ja metatiedot

Kun olet koodata syötteen resurssi (tai kalusto) käyttämällä MES, saat tulosteen kohteiden osoitteessa onnistuneesti, joka valmiiksi koodata tehtävän. Tulostus-resurssi on video, ääni, pikkukuvat, luettelo, käytät koodauksen esiasetus perusteella jne.

Tulostus-resurssi on myös tiedoston, jonka syötteen kohteiden metatietoa. Metatietojen XML-tiedoston nimi on seuraavanlainen: < asset_id > _metadata.xml (esimerkiksi 41114ad3-eb5e - 4c 57 8d 92-5354e2b7d4a4_metadata.xml), missä < asset_id > syötteen kohteen aihetunnus-arvo. Tämän syötteen XML-metatiedot rakenne on kuvattu [seuraavassa](http://msdn.microsoft.com/library/azure/dn783120.aspx).

Tulostus-resurssi on myös tiedoston, jonka tulos kohteen metatietoa. Metatietojen XML-tiedoston nimi on seuraavanlainen: < source_file_name > _manifest.xml (esimerkiksi BigBuckBunny_manifest.xml). XML-funktio on kuvattu tulosteen metatiedot rakenteen [tähän](http://msdn.microsoft.com/library/azure/dn783217.aspx).

Jos haluat tarkistaa jommankumman kahdesta metatietojen tiedostot, voit luoda SAS locator ja lataa tiedosto paikalliseen tietokoneeseen. Löydät esimerkki siitä, miten voit luoda SAS locator ja ladata tiedoston Media Services .NET SDK-laajennusten.

##<a name="download-sample"></a>Lataa malli

Hae ja suorita otoksen [täältä](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).

##<a name="example"></a>Esimerkki

Seuraava koodiesimerkki käyttää Media Services .NET SDK voit suorittaa seuraavia tehtäviä:

- Luo koodauksen työ.
- Viittaaminen Media Encoder vakio encoder.
- Määritä käyttämään "H264 useita bittitaajuus 720p" esiasetettuja. Näet kaikki Esiasetukset [tähän](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409). Voit myös tarkastella rakenne, johon nämä esiasetukset on täytettävä [Tässä](https://msdn.microsoft.com/library/mt269962.aspx) aiheessa.
- Lisää yhden koodauksen tehtävän työn. 
- Määritä koodattava syötteen resurssi.
- Luo tulostus-resurssi, joka sisältää koodattu resurssi.
- Lisää tapahtumakäsittelijän tarkistamaan työn edistymistä.
- Lähetä työn.
        
        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("Media Encoder Standard Job");
            // Get a media processor reference, and pass to it the name of the 
            // processor to use for the specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
        

            // Create a task with the encoding details, using a string preset.
            // In this case "H264 Multiple Bitrate 720p" preset is used.
            ITask task = job.Tasks.AddNew("My encoding task",
                processor,
                "H264 Multiple Bitrate 720p",
                TaskOptions.None);
        
            // Specify the input asset to be encoded.
            task.InputAssets.Add(asset);
            // Add an output asset to contain the results of the job. 
            // This output is specified as AssetCreationOptions.None, which 
            // means the output asset is not encrypted. 
            task.OutputAssets.AddNew("Output asset",
                AssetCreationOptions.None);
        
            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
            job.Submit();
            job.GetExecutionProgressTask(CancellationToken.None).Wait();
        
            return job.OutputMediaAssets[0];
        }
        
        private static void JobStateChanged(object sender, JobStateChangedEventArgs e)
        {
            Console.WriteLine("Job state changed event:");
            Console.WriteLine("  Previous state: " + e.PreviousState);
            Console.WriteLine("  Current state: " + e.CurrentState);
            switch (e.CurrentState)
            {
                case JobState.Finished:
                    Console.WriteLine();
                    Console.WriteLine("Job is finished. Please wait while local tasks or downloads complete...");
                    break;
                case JobState.Canceling:
                case JobState.Queued:
                case JobState.Scheduled:
                case JobState.Processing:
                    Console.WriteLine("Please wait...\n");
                    break;
                case JobState.Canceled:
                case JobState.Error:
        
                    // Cast sender as a job.
                    IJob job = (IJob)sender;
        
                    // Display or log error details as needed.
                    break;
                default:
                    break;
            }
        }
        
        
        private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
        {
            var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
            ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
            if (processor == null)
                throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
            return processor;
        }


##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Katso myös 

[Miten Luo pikkukuva .NET Media Encoder vakio käyttäminen](media-services-dotnet-generate-thumbnail-with-mes.md)
[Media koodaus yleiskatsaus](media-services-encode-asset.md)
