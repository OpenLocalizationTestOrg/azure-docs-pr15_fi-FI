<properties
    pageTitle="Indeksoinnin mediatiedostojen Azure Media indeksointitoiminto 2 esikatselun avulla | Microsoft Azure"
    description="Azure Media indeksointitoiminto avulla voit tehdä mediatiedostojen sisällöstä haettavissa olevia ja luo teksti-tallenne suljettu tekstitys ja avainsanat. Tässä ohjeaiheessa kerrotaan, miten Media indeksointitoiminto 2 esikatselussa."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/26/2016" 
    ms.author="adsolank;juliako;"/>


# <a name="indexing-media-files-with-azure-media-indexer-2-preview"></a>Indeksoinnin mediatiedostojen ja Azure Media indeksointitoiminto 2 esikatselu

##<a name="overview"></a>Yleiskatsaus

**Azure Media indeksointitoiminto 2 Preview** media-suoritin (MP) avulla voit tehdä mediatiedostojen ja sisällön etsittävän sekä luoda suljettu tekstitys raidat. Verrattuna edellisen version [Azure Media indeksointitoiminto](media-services-index-content.md), **Azure Media indeksointitoiminto 2 Preview** suorittaa nopeammin indeksoinnin ja laajempi kieli tukee. Tuetut kieliä ovat englanti, espanja, ranska, saksa, italia, kiina, portugali ja arabia.

**Azure Media indeksointitoiminto 2 Preview** MP on tällä hetkellä esikatselu.

Tässä ohjeaiheessa esitellään, miten voit luoda Indeksointitöiden **Azure Media indeksointitoiminto 2 esikatselu**.

>[AZURE.NOTE]Ottaa huomioon seuraavat asiat:
>
>Azure kiina ja Azure Government ei tueta indeksointitoiminto 2.
>
>Esikatselu on rajoitettu käsittely noin 10 minuutin, mutta se on ilmainen kaikille asiakkaille.
>
>Kun sisällön indeksointi, varmista, että Käytä mediatiedostoja, jotka on hyvin selkeät puheeksi (ilman taustamusiikkia, ääni, tehosteiden ja mikrofonin hiss). Esimerkkejä sisällön: kokouksiin, luentoja tai esityksiä. Seuraavia sisältö ei ehkä sovellu indeksoinnin: elokuvia, TV-ohjelmat, mikä tahansa erilaiset ääni- ja äänitehosteiden, tallennettuja huonosti taustaäänet (hiss) sisällön.


Tässä ohjeaiheessa tarkempia tietoja **Azure Media indeksointitoiminto 2 Preview** ja kerrotaan, miten voit käyttää sitä Media Services SDK .NET varten

##<a name="input-and-output-files"></a>Syöttö- ja tiedostot

###<a name="input-files"></a>Syöte

Ääni- ja videotiedostojen tallentamista varten

###<a name="output-files"></a>Tulostus-tiedostot

Indeksoinnin työn luoda tekstityksen tiedostoja seuraavissa muodoissa:  

- **SAAME**
- **TTML**
- **WebVTT**

Suljettu otsikko (kopio) Nämä tiedostomuodot voidaan tehdä ääni- ja videotiedostoista helppokäyttöisempiä kuuleminen disability kärsivät ihmiset.

##<a name="task-configuration-preset"></a>Tehtävän määrittäminen (esiasetus)

Kun luot indeksoinnin tehtävän **Azure Media indeksointitoiminto 2 Preview**, määritys esiasetus on määritettävä.

Seuraavat JSON asettaa käytettävissä parametrit.

    {
      "version":"1.0",
      "Features":
        [
           {
           "Options": {
                "Formats":["WebVtt","ttml"],
                "Language":"enUs",
                "Type":"RecoOptions"
           },
           "Type":"SpReco"
        }]
    }

##<a name="supported-languages"></a>Tukemat kielet  

Azure Media indeksointitoiminto 2 Preview tukee teksti puheeksi-seuraavilla kielillä (kun määrität kielen nimi tehtävän määritykset, käytä 4 merkin koodi hakasulkeissa, alla kuvatulla tavalla):

- Englannin [EnUs]
- Espanja [EsEs]
- Kiina [ZhCn]
- Ranskan [FrFr]
- Saksa [DeDe]
- Italia [ItIt]
- Portugalin [PtBr]
- Arabia (Egyptian) [ArEg]


## <a name="sample-code"></a>Esimerkki koodi

Seuraavat ohjelma näyttää Toimintaohje:

1. Luo sijoituksen ja Lataa mediatiedosto kohteen kyselyjä.
1. Luo projektin indeksoinnin tehtävästä, joka sisältää seuraavat json esiasetus kokoonpanotiedosto perusteella.
            
        {
          "version":"1.0",
          "Features":
            [
               {
               "Options": {
                    "Formats":["WebVtt","ttml"],
                    "Language":"enUs",
                    "Type":"RecoOptions"
               },
               "Type":"SpReco"
            }]
        }

1. Lataa tiedostot tulos. 
    
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace IndexContent
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
        
                    // Run indexing job.
                    var asset = RunIndexingJob(@"C:\supportFiles\Indexer\BigBuckBunny.mp4",
                                                @"C:\supportFiles\Indexer\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\Indexer\Output");
                }
        
                static IAsset RunIndexingJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Indexing Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Indexing Job");
        
                    // Get a reference to Azure Media Indexer 2 Preview.
                    string MediaProcessorName = "Azure Media Indexer 2 Preview";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Indexing Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset to be indexed.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Indexing Output Asset", AssetCreationOptions.None);
        
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


##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


## <a name="related-links"></a>Aiheeseen liittyvät linkit

[Azure Media Services-palveluiden Analytics yleiskatsaus](media-services-analytics-overview.md)

[Azure Media Analytics esittelyt](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)