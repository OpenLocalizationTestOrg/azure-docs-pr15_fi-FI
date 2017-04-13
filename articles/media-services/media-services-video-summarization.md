<properties
    pageTitle="Videon yhteenvedon luominen Azure Media Video pienoiskuvat avulla | Microsoft Azure"
    description="Videon yhteenveto avulla voit luoda yhteenvetojen pitkä videoita valitsemalla kiinnostavat katkelmat automaattisesti lähde-video. Tästä on hyötyä, kun haluat lisätä pikaisesti toiminta pitkä video."
    services="media-services"
    documentationCenter=""
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/26/2016"   
    ms.author="milanga;juliako;"/>

#<a name="use-azure-media-video-thumbnails-to-create-a-video-summarization"></a>Videon yhteenvedon luominen Azure Media Video pikkukuvien avulla
##<a name="overview"></a>Yleiskatsaus

**Azure Media Video pikkukuvat** media-suoritin (MP) avulla voit luoda videon, joka on hyötyä asiakkaille, joilla haluat esikatsella pitkä videon yhteenveto yhteenvedon. Esimerkiksi asiakkaiden saatat haluta nähdä lyhyt "yhteenveto videon" kun ne osoittimen pikkukuvan. Mukaan tweaking parametrit **Azure Media Video** pikkukuvien määritysten esiasetus kautta, Luo algorithmically kuvaava subclip MP tehokas näyttökuva tunnistaminen ja yhdistäminen teknologia avulla.  

**Azure Media Video pikkukuva** MP on tällä hetkellä esikatselu.

Tässä ohjeaiheessa tarkempia tietoja **Azure Media videon pikkukuva** ja kerrotaan, miten voit käyttää sitä Media Services SDK .NET varten

##<a name="video-summary-example"></a>Videon yhteenveto Esimerkki 

Seuraavassa on joitakin esimerkkejä Azure Media Video pikkukuvat media-suoritin käyttömahdollisuuksista:

###<a name="original-video"></a>Alkuperäinen video

[Alkuperäinen video](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Fnimbuscdn-nimbuspm.streaming.mediaservices.windows.net%2Faed33834-ec2d-4788-88b5-a4505b3d032c%2FMicrosoft%27s%20HoloLens%20Live%20Demonstration.ism%2Fmanifest)

###<a name="video-thumbnail-result"></a>Videon pikkukuvan tulos

[Videon pikkukuvan tulos](http://ampdemo.azureedge.net/azuremediaplayer.html?url=http%3A%2F%2Fnimbuscdn-nimbuspm.streaming.mediaservices.windows.net%2Ff5c91052-4232-41d4-b531-062e07b6a9ae%2FHololens%2520Demo_VideoThumbnails_MotionThumbnail.mp4)

##<a name="task-configuration-preset"></a>Tehtävän määrittäminen (esiasetus)

Kun videon pikkukuvan tehtävän luominen **Azure Media Video pikkukuvien**kanssa, sinun on määritettävä määritysten esiasetus. JSON seuraavat peruskokoonpano on luotu pikkukuvan esimerkissä:

    {"version":"1.0"}

Voit muuttaa tällä hetkellä seuraavia parametreja:

Parametri|Kuvaus
---|---
outputAudio|Määrittää, onko voimassa oleva video sisältää äänitiedostojen tekstittäminen. <br/>Sallittu seuraavia arvoja: TOSI tai EPÄTOSI. Oletusarvo on TOSI.
fadeInFadeOut|Määrittää, onko Häivytys siirtymät käytetään eri liikerata-pienoiskuvat välillä.  <br/>Sallittu seuraavia arvoja: TOSI tai EPÄTOSI.  Oletusarvo on TOSI.
maxMotionThumbnailDurationInSecs|Kokonaisluku, joka määrittää, kuinka kauan koko voimassa oleva video on oltava.  Oletusarvo riippuu alkuperäisen videon kesto.


Seuraavassa taulukossa on kuvattu kestoajan oletusarvo, kun **maxMotionThumbnailInSecs** ei käytetä.

||||
---|---|---|---|---
Videon kesto|d < 3 min|3 min < d < 15 min|15 min < d < 30 minuuttia| 30 minuuttia < d
Pikkukuvan kesto|15 sek (2-3-näkymien)| 30 sekuntia (3 – 5 näkymien)|60 sekunti (5-10-näkymien)|90 sek (10-15 näkymien)


Seuraavat JSON asettaa käytettävissä parametrit.
    
    {
        "version": "1.0",
        "options": {
            "outputAudio": "true",
            "maxMotionThumbnailDurationInSecs": "10",
            "fadeInFadeOut": "true"
        }
    }

## <a name="sample-code"></a>Esimerkki koodi

Seuraavat ohjelma näyttää Toimintaohje:

1. Luo sijoituksen ja Lataa mediatiedosto kohteen kyselyjä.
1. Luo projektin videon pikkukuvan tehtävän kokoonpanotiedosto, joka sisältää seuraavat json esiasetus perusteella. 
        
        {               
            "version": "1.0",
            "options": {
                "outputAudio": "true",
                "maxMotionThumbnailDurationInSecs": "30",
                "fadeInFadeOut": "false"
            }
        }

1. Lataa tulostus-tiedostot. 

###<a name="net-code"></a>.NET-koodi
     
    using System;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;
    using System.Threading.Tasks;
    
    namespace VideoSummarization
    {
        class Program
        {
            // Read values from the App.config file.
            private static readonly string _mediaServicesAccountName =
                ConfigurationManager.AppSettings["MediaServicesAccountName"];
            private static readonly string _mediaServicesAccountKey =
                ConfigurationManager.AppSettings["MediaServicesAccountKey"];
    
            // Field for service context.
            private static CloudMediaContext _context = null;
            private static MediaServicesCredentials _cachedCredentials = null;
    
            static void Main(string[] args)
            {
    
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Used the cached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);
    

                // Run the thumbnail job.
                var asset = RunVideoThumbnailJob(@"C:\supportFiles\VideoThumbnail\BigBuckBunny.mp4",
                                            @"C:\supportFiles\VideoThumbnail\config.json");
    
                // Download the job output asset.
                DownloadAsset(asset, @"C:\supportFiles\VideoThumbnail\Output");
            }
    
            static IAsset RunVideoThumbnailJob(string inputMediaFilePath, string configurationFile)
            {
                // Create an asset and upload the input media file to storage.
                IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                    "My Video Thumbnail Input Asset",
                    AssetCreationOptions.None);
    
                // Declare a new job.
                IJob job = _context.Jobs.Create("My Video Thumbnail Job");
    
                // Get a reference to Azure Media Video Thumbnails.
                string MediaProcessorName = "Azure Media Video Thumbnails";
    
                var processor = GetLatestMediaProcessorByName(MediaProcessorName);
    
                // Read configuration from the specified file.
                string configuration = File.ReadAllText(configurationFile);
    
                // Create a task with the encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("My Video Thumbnail Task",
                    processor,
                    configuration,
                    TaskOptions.None);
    
                // Specify the input asset.
                task.InputAssets.Add(asset);
    
                // Add an output asset to contain the results of the job.
                task.OutputAssets.AddNew("My Video Thumbnail Output Asset", AssetCreationOptions.None);
    
                // Use the following event handler to check job progress.  
                job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);
    
                // Launch the job.
                job.Submit();
    
                // Check job execution and wait for job to finish.
                Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
    
                progressJobTask.Wait();
    
                // If job state is Error, the event handling
                // method for job progress should log errors.  Here we check
                // for error state and exit if needed.
                if (job.State == JobState.Error)
                {
                    ErrorDetail error = job.Tasks.First().ErrorDetails.First();
                    Console.WriteLine(string.Format("Error: {0}. {1}",
                                                    error.Code,
                                                    error.Message));
                    return null;
                }
    
                return job.OutputMediaAssets[0];
            }
    
            static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
            {
                IAsset asset = _context.Assets.Create(assetName, options);
    
                var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                assetFile.Upload(filePath);
    
                return asset;
            }
    
            static void DownloadAsset(IAsset asset, string outputDirectory)
            {
                foreach (IAssetFile file in asset.AssetFiles)
                {
                    file.Download(Path.Combine(outputDirectory, file.Name));
                }
            }
    
            static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
            {
                var processor = _context.MediaProcessors
                    .Where(p => p.Name == mediaProcessorName)
                    .ToList()
                    .OrderBy(p => new Version(p.Version))
                    .LastOrDefault();
    
                if (processor == null)
                    throw new ArgumentException(string.Format("Unknown media processor",
                                                               mediaProcessorName));
    
                return processor;
            }
    
            static private void StateChanged(object sender, JobStateChangedEventArgs e)
            {
                Console.WriteLine("Job state changed event:");
                Console.WriteLine("  Previous state: " + e.PreviousState);
                Console.WriteLine("  Current state: " + e.CurrentState);
    
                switch (e.CurrentState)
                {
                    case JobState.Finished:
                        Console.WriteLine();
                        Console.WriteLine("Job is finished.");
                        Console.WriteLine();
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
                        // LogJobStop(job.Id);
                        break;
                    default:
                        break;
                }
            }
    
        }
    }

###<a name="video-thumbnail-output"></a>Videon pikkukuvan tulostus

[Videon pikkukuvan tulostus](http://ampdemo.azureedge.net/azuremediaplayer.html?url=http%3A%2F%2Fnimbuscdn-nimbuspm.streaming.mediaservices.windows.net%2Fd06f24dc-bc81-488e-a8d0-348b7dc41b56%2FHololens%2520Demo_VideoThumbnails_MotionThumbnail.mp4)

##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-links"></a>Aiheeseen liittyvät linkit

[Azure Media Services-palveluiden Analytics yleiskatsaus](media-services-analytics-overview.md)

[Azure Media Analytics esittelyt](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)